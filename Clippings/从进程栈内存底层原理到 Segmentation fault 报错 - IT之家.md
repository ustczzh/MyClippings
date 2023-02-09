# 从进程栈内存底层原理到 Segmentation fault 报错 - IT之家
[从进程栈内存底层原理到 Segmentation fault 报错 - IT之家](https://www.ithome.com/0/672/338.htm) 

 本文来自微信公众号：[开发内功修炼 （ID：kfngxl）](https://mp.weixin.qq.com/s/pTP7ELOm4-JsAl1o_nyS-Q)，作者：张彦飞 allen

大家好，我是飞哥！

栈是编程中使用内存最简单的方式。例如，下面的简单代码中的局部变量 n 就是在堆栈中分配内存的。

```
#include<stdio.h>  void main()  { int n =0; printf("0x%x\\n",&v); }
```

那么我有几个问题想问问大家，看看大家对于堆栈内存是否真的了解。

*   堆栈的物理内存是什么时候分配的？
    
*   堆栈的大小限制是多大？这个限制可以调整吗？
    
*   当堆栈发生溢出后应用程序会发生什么？
    

如果你对以上问题还理解不是特别深刻，飞哥今天来带你好好修炼进程堆栈内存这块的内功！

一、进程堆栈的初始化
----------

前面我们在《你写的代码是如何跑起来的？》这篇文章中介绍了进程的启动过程。进程启动调用 exec 加载可执行文件过程的时候，会给进程栈申请一个 4 KB 的初始内存。我们今天来专门抽取并看一下这段逻辑。

加载系统调用 execve 依次调用 do\_execve、do\_execve_common 来完成实际的可执行程序加载。

```
//file:fs/exec.c  staticint do\_execve\_common(constchar*filename,)  { bprm\_mm\_init(bprm);  }
```

在 bprm\_mm\_init 中会申请一个全新的地址空间 mm_struct 对象，准备留着给新进程使用。

```
//file:fs/exec.c  staticint bprm\_mm\_init(struct linux_binprm *bprm)  { //申请个全新的地址空间 mm_struct 对象 bprm-mm = mm = mm_alloc(); \_\_bprm\_mm_init(bprm);  }
```

还会给新进程的栈申请一页大小的虚拟内存空间，作为给新进程准备的栈内存。申请完后把栈的指针保存到 bprm->p 中记录起来。

```
//file:fs/exec.c  staticint \_\_bprm\_mm_init(struct linux_binprm *bprm)  { bprm-vma = vma = kmem\_cache\_zalloc(vm\_area\_cachep, GFP_KERNEL); vma-vm_end = STACK\_TOP\_MAX; vma-vm_start = vma-vm_end - PAGE_SIZE; bprm-p = vma-vm_end -sizeof(void*);  }
```

我们平时所说的进程虚拟地址空间在 Linux 是通过一个个的 vm\_area\_struct 对象来表示的。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-9%2020-21-40/bafd7315-e5f7-48c4-b5d8-3b8f8596bc8c.png?raw=true)

每一个 vm\_area\_struct（就是上面 \_\_bprm\_mm\_init 函数中的 vma）对象表示进程虚拟地址空间里的一段范围，其 vm\_start 和 vm_end 表示启用的虚拟地址范围的开始和结束。

```
//file:include/linux/mm_types.h  struct vm\_area\_struct { unsignedlong vm_start; unsignedlong vm_end;  }
```

**要注意的是这只是地址范围，而不是真正的物理内存分配**。

在上面 \_\_bprm\_mm\_init 函数中通过 kmem\_cache\_zalloc 申请了一个 vma 内核对象。vm\_end 指向了 STACK\_TOP\_MAX（地址空间的顶部附近的位置），vm\_start 和 vm\_end 之间留了一个 Page 大小。**也就是说默认给栈准备了 4KB 的大小**。最后把栈的指针记录到 bprm->p 中。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-9%2020-21-40/bcba6a83-4f2a-493c-b238-a7fb2baceb2d.png?raw=true)

接下来进程加载过程会使用 load\_elf\_binary 真正开始加载可执行二进制程序。在加载时，会把前面准备的进程栈的地址空间指针设置到了新进程 mm 对象上。

```
//file:fs/binfmt_elf.c  staticint load\_elf\_binary(struct linux_binprm *bprm)  {//ELF 文件头解析 //Program Header 读取 //清空父进程继承来的资源 retval= flush\_old\_exec(bprm); current-mm-start_stack = bprm-p;  }
```

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-9%2020-21-40/7f8923ac-bdbe-403b-80d0-0c2d715186aa.png?raw=true)

这样新进程将来就可以使用栈进行函数调用，以及局部变量的申请了。

前面我们说了，这里只是给栈申请了地址空间对象，并没有真正申请物理内存。我们接着再来看一下，物理内存页究竟是什么时候分配的。

二、物理页的申请
--------

当进程在运行的过程中在栈上开始分配和访问变量的时候，如果物理页还没有分配，会触发缺页中断。在缺页中断种来真正地分配物理内存。

为了避免篇幅过长，触发缺页中断的过程就先不展开了。我们直接看一下缺页中断的核心处理入口 \_\_do\_page_fault，它位于 arch / x86 / mm / fault.c 文件下。

```
//file:arch/x86/mm/fault.c  staticvoid __kprobes
\_\_do\_page_fault(struct pt_regs *regs,unsignedlong error_code)  { //根据新的 address 查找对应的 vma vma= find_vma(mm, address; //如果找到的 vma 的开始地址比 address 小 //那么就不调用expand_stack了，直接调用 if likelyvma-vm_start = address) goto good_area; if unlikelyexpand_stack(vma, address)){ bad_area(regs, error_code, address; return; } good_area: //调用handle\_mm\_fault来完成真正的内存申请 fault= handle\_mm\_fault(mm, vma, address, flags);  }
```

当访问栈上变量的内存的时候，首先会调用 find\_vma 根据变量地址 address 找到其所在的 vma 对象。接下来调用的 if (vma->vm\_start <= address) 是在判断地址空间还够不够用。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-9%2020-21-40/dbfcb908-8bdc-4d88-a1e9-d406a077443c.png?raw=true)

如果栈内存 vma 的 start 比要访问的 address 小，则证明地址空间够用，只需要分配物理内存页就行了。如果栈内存 vma 的 start 比要访问的 address 大，则需要调用 expand_stack 先扩展一下栈的虚拟地址空间 vma。扩展虚拟地址空间的具体细节我们在第三节再讲。

这里先假设要访问的变量地址 address 处于栈内存 vma 对象的 vm\_start 和 vm\_end 之间。那么缺页中断处理就会跳转到 good\_area 处运行。在这里调用 handle\_mm_fault 来**完成真正物理内存的申请**。

```
//file:mm/memory.c  int handle\_mm\_fault(struct mm_struct *mm,struct vm\_area\_struct *vma, unsignedlong address,unsignedint flags)  { //依次查看每一级页表项 pgd= pgd_offset(mm, address); pud= pud_alloc(mm, pgd, address); pmd= pmd_alloc(mm, pud, address); pte= pte\_offset\_map(pmd, address); return handle\_pte\_fault(mm, vma, address, pte, pmd, flags);  }
```

Linux 是用四级页表来管理虚拟地址空间到物理内存之间的映射管理的。所以在实际申请物理页面之前，需要先 check 一遍需要的每一级页表项是否存在，不存在的话需要申请。

为了好区分，Linux 还给每一级页表都起了一个名字。

*   一级页表：Page Global Dir，简称 pgd
    
*   二级页表：Page Upper Dir，简称 pud
    
*   三级页表：Page Mid Dir，简称 pmd
    
*   四级页表：Page Table，简称 pte
    

看一下下面这个图就比较好理解了

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-9%2020-21-40/c06c37f4-b3aa-4b99-b780-ec7ee99f5f9d.png?raw=true)

```
//file:mm/memory.c  int handle\_pte\_fault(struct mm_struct *mm, struct vm\_area\_struct *vma,unsignedlong address, pte_t*pte,pmd_t*pmd,unsignedint flags)  { //匿名映射页处理 return do\_anonymous\_page(mm, vma, address, pte, pmd, flags);  }
```

在 handle\_pte\_fault 会处理很多种的内存缺页处理，比如文件映射缺页处理、swap 缺页处理、写时复制缺页处理、匿名映射页处理等等几种情况。我们今天讨论的主题是栈内存，这个对应的是匿名映射页处理，会进入到 do\_anonymous\_page 函数中。

```
//file:mm/memory.c  staticint do\_anonymous\_page(struct mm_struct *mm,struct vm\_area\_struct *vma, unsignedlong address,pte_t*page_table,pmd_t*pmd, unsignedint flags)  { // 分配可移动的匿名页面，底层通过 alloc_page page= alloc\_zeroed\_user\_highpage\_movable(vma, address);  }
```

在 do\_anonymous\_page 调用 alloc\_zeroed\_user\_highpage\_movable 分配一个可移动的匿名物理页出来。在底层会调用到伙伴系统的 alloc_pages 进行实际物理页面的分配。

内核是用伙伴系统来管理所有的物理内存页的。其它模块需要物理页的时候都会调用伙伴系统对外提供的函数来申请物理内存。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-9%2020-21-40/c914e5eb-0ac3-426f-9807-e981382df60e.png?raw=true)

关于伙伴系统我们之前在内核内存管理 这篇文章中详细介绍过，感兴趣的同学可以移步到该文中详细了解。

到了这里，开篇的问题一就有答案了，堆栈的物理内存是什么时候分配的？进程在加载的时候只是会给新进程的栈内存分配一段地址空间范围。而真正的物理内存是等到访问的时候触发缺页中断，再从伙伴系统中申请的。

三、栈的自动增长
--------

前面我们看到了，进程在被加载启动的时候，栈内存默认只分配了 4 KB 的空间。那么随着程序的运行，当栈中保存的调用链，局部变量越来越多的时候，必然会超过 4 KB。

我回头看下缺页处理函数 \_\_do\_page\_fault。如果栈内存 vma 的 start 比要访问的 address 大，则需要调用 expand\_stack 先扩展一下栈的虚拟地址空间 vma。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-9%2020-21-40/eaef665d-6ddd-405d-97be-4664eb6d7afc.png?raw=true)

回顾 \_\_do\_page\_fault 源码，看到扩充栈空间的是由 expand\_stack 函数来完成的。

```
//file:arch/x86/mm/fault.c  staticvoid __kprobes
\_\_do\_page_fault(struct pt_regs *regs,unsignedlong error_code)  { if likelyvma-vm_start = address) goto good_area; //如果栈 vma 的开始地址比 address 大，需要扩大栈 if unlikelyexpand_stack(vma, address)){ bad_area(regs, error_code, address; return; } good_area:  }
```

我们来看下 expand_stack 的内部细节。

> 其实在 Linux 栈地址空间增长是分两种方向的，一种是从高地址往低地址增长，一种是反过来。大部分情况都是由高往低增长的。本文只以向下增长为例。

```
//file:mm/mmap.c  int expand_stack(struct vm\_area\_struct *vma,unsignedlong address)  { return expand_downwards(vma, address);  }  int expand_downwards(struct vm\_area\_struct *vma,unsignedlong address)  { //计算栈扩大后的最后大小 size= vma-vm_end - address; //计算需要扩充几个页面 grow=(vma-vm_start - address)> PAGE_SHIFT; //判断是否允许扩充 acct\_stack\_growth(vma, size, grow); //如果允许则开始扩充 vma-vm_start = address; return }
```

在 expand_downwards 中先进行了几个计算。

*   计算出新的堆栈大小。计算公式是 size = vma->vm_end - address;
    
*   计算需要增长的页数。计算公式是 grow = (vma->vm\_start - address) >> PAGE\_SHIFT;
    

然后会判断此次栈空间是否被允许扩充，判断是在 acct\_stack\_growth 中完成的。如果允许扩展，则简单修改一下 vma->vm_start 就可以了！

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-9%2020-21-40/231120f6-5189-4251-ab33-b6afa3393c48.png?raw=true)

我们再来看 acct\_stack\_growth 都进行了哪些限制判断。

```
//file:mm/mmap.c  staticint acct\_stack\_growth(struct vm\_area\_struct *vma,unsignedlong size,unsignedlong grow)  { //检查地址空间是否超出限制 if(!may\_expand\_vm(mm, grow)) return-ENOMEM; //检查是否超出栈的大小限制 if(size  ACCESS_ONCE(rlim\[RLIMIT_STACK\].rlim_cur)) return-ENOMEM; return0;  }
```

在 acct\_stack\_growth 中只是进行一系列的判断。may\_expand\_vm 判断的是增长完这几个页后是否超出整体虚拟地址空间大小的限制。rlim \[RLIMIT\_STACK\].rlim\_cur 中记录的是栈空间大小的限制。这些限制都可以通过 ulimit 命令查看到。

```
# ulimit -a max memory size(kbytes,-m) unlimited
stack size              (kbytes,-s)8192  virtual memory          (kbytes,-v) unlimited
```

上面的这个输出表示虚拟地址空间大小没有限制，栈空间的限制是 8 MB。如果进程栈大小超过了这个限制，会返回 -ENOMEM。如果觉得系统默认的大小不合适可以通过 ulimit 命令修改。

```
# ulimit -s 10240  # ulimit -a stack size(kbytes,-s)10240
```

到这里开篇的第二个问题也有答案了，堆栈的大小限制是多大？这个限制可以调整吗？

进程堆栈大小的限制在每个机器上都是不一样的，可以通过 ulimit 命令来查看，也同样可以使用该命令修改。

至于开篇的问题 3，当堆栈发生溢出后应用程序会发生什么？写个简单的无限递归调用就知道了，估计你也遇到过。报错结果就是

```
'Segmentation fault (core dumped)
```

本文总结
----

来总结下本文的内容，本文讨论了进程栈内存的工作原理。

第一，进程在加载的时候给进程栈申请了一块虚拟地址空间 vma 内核对象。vm\_start 和 vm\_end 之间留了一个 Page ，也就是说默认给栈准备了 4KB 的空间。

第二，当进程在运行的过程中在栈上开始分配和访问变量的时候，如果物理页还没有分配，会触发缺页中断。在缺页中断中调用内核的伙伴系统真正地分配物理内存。

第三，当栈中的存储超过 4KB 的时候会自动进行扩大。不过大小要受到限制，其大小限制可以通过 ulimit -s 来查看和设置。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-9%2020-21-40/73d0de4d-be0b-4239-af58-bc455a28106a.png?raw=true)

注意，今天我们讨论的都是进程栈。线程栈和进程栈有些不一样。等后面有空我们再单独看线程栈。

在回顾和总结下开篇我们抛出的三个问题：

问题一：堆栈的物理内存是什么时候分配的？进程在加载的时候只是会给新进程的栈内存分配一段地址空间范围。而真正的物理内存是等到访问的时候触发缺页中断，再从伙伴系统中申请的。

问题二：堆栈的大小限制是多大？这个限制可以调整吗？

进程堆栈大小的限制在每个机器上都是不一样的，可以通过 ulimit 命令来查看，也同样可以使用该命令修改。

问题 三：当堆栈发生溢出后应用程序会发生什么？当堆栈溢出的时候，我们会收到报错 “Segmentation fault (core dumped)”

最后，抛个问题大家一起思考吧。你觉得内核为什么要对进程栈的地址空间进行限制呢？