# C#多线程之同步基础篇 - xiaolipro - 博客园
一、基本概念
------

**线程安全（thread safe）**：指的是被任意多的线程同时执行，都可以保证正确性。

> 除基本类型外，很少有类型是线程安全的，线程安全的责任基本落在开发者身上，System.Collections.Concurrent命名空间下的类型的除外。

*   线程安全最常见的手段一般是使用【**排它锁**】，将大段代码甚至是访问的整个对象封装在一个排它锁内，从而保证在高层上能进行顺序访问。  
    这种解决方案适用于对象的方法都能够快速执行的场景（否则会导致大量的阻塞）。
*   还有一种手段很高明，即通过【**最小化共享数据**】来减少【线程交互】，web服务器就是最好的案例，由于多个客户端请求可以同时到达，服务端方法必须保证线程安全。类似的案例还有【无状态】设计，在本质上限制了  
    数据交互的可能，具有良好的伸缩性（scalability）。
*   还有一种手段，【**自动锁机制（automatic locking）**】如果继承 ContextBoundObject 类并使用 Synchronization 特性，.NET Framework 就可以实现这种机制，framework全系支持，但是netcore没有，类似java的synchronized。

> 尽管这样降低了开发者实现线程安全的负担，但范围过大的锁定作用域将制造出巨大的麻烦：死锁、非有意的重入以及降低并发度。这使得手动锁定在任何场景都显得更为合适，而并不仅仅只在简单的场景下（直到有更好用的自动锁机制出现）。

*   其它手段，【信号构造】，【内存屏障】，【自旋构造】。。。

**同步（synchronization）**：指对在一个系统中所发生的事件（event）之间进行协调，在时间上出现一致性与统一化的现象 -- 为期望的结果协调多个线程的行为。

当多个线程访问同一个数据时，同步尤为重要，但这是一件非常容易G的事情。

**同步对象（synchronized object）**：对所有参与同步的线程可见的任何对象都可以被当作同步对象使用，但有一个硬性规定：同步对象必须为引用类型。同步对象一般是私有的（因为这有助于封装锁逻辑）。

同步对象也可以就是其要保护的对象。

```null
class ThreadSafe
{
  List <string> _list = new List <string>();

  void Test()
  {
    lock (_list)
    {
      _list.Add ("Item 1");
      

```

一个只被用来加锁的字段可以精确控制锁的作用域与粒度。

```null
private static readonly object _locker = new object();

```

对象自己（this），甚至是类型，lambda 表达式或匿名方法所捕获的局部变量 都可以被当作同步对象来使用：

```null
lock (this) { ... }

lock (typeof (Widget)) { ... }    

```

但这种方式的缺点在于并没有对锁逻辑进行封装，从而很难避免【死锁】或过多的【阻塞】。 同时类型上的锁也可能会跨越应用程序域（application domain）边界（在同一进程内）。

**阻塞（block）**：当线程的执行由于某些原因被暂停，比如【信号构造】或【锁构造】时，比如调用Thread.Sleep，Task.Wait，或者通过Join方法等待其它线程结束时，则认为此线程被阻塞（blocked）。

阻塞会在以下 4 种情况下解除（电源按钮可不能算╮(╯▽╰)╭）：

*   阻塞条件被满足
*   操作超时（如果指定了超时时间）
*   通过Thread.Interrupt中断
*   通过Thread.Abort中止

编译器将async Task转换为状态机，到达 await 时暂停执行等待后台作业完成时继续执行。从理论上讲，这是[异步的承诺模型](https://en.wikipedia.org/wiki/Futures_and_promises)的实现。）

```null
static async Task<Toast> MakeToastWithButterAndJamAsync(int number)
{
    
    var toast = await ToastBreadAsync(number);
    
    return toast;
}

```

**锁构造（lock）**：锁能够限制同一时刻可以执行某些指令或是某段代码的线程数量。排他锁是最常见的，它只允许同一时刻至多有一个线程执行，从而可以使得参与竞争的线程在访问公共数据时不会彼此干扰。  
一般的排他锁有lock（Monitor.Enter/Monitor.Exit）、Mutex、SpinLock，非排他锁有Semaphore、SemaphoreSlim以及reader/writer lock。

**信号构造（signal）**：信号构造可以使一个线程【挂起】，直到接收到另一个线程的通知，避免了低效的轮询 。有两种经常使用的信号设施：事件等待句柄（event wait handle ）和Monitor类的Wait / Pulse方法。  
Framework 4.0 加入了CountdownEvent与Barrier类。

**自旋（spinning）**：有时线程必须阻塞/暂停，直至条件被满足，【信号构造】或【锁构造】可以实现，但在等待条件能够在微秒级的时间被满足时，  
自旋往往更加高效，因为它避免了上下文切换带来的昂贵开销。

```null
while (!condition);

```

自旋往往与阻塞组合使用，防止cpu浪费

```null
while (!condition) Thread.Sleep (10);

```

**线程状态（thread state）**：Unstarted、Running、WaitSleepJoin、Stopped。。

**指令原子性（instruction atomically）**：如果一组【指令】可以在 CPU 上不可分割地执行，那么它就是原子的

**原子性（atomically）**：如果一组变量总是在相同的锁内进行读写，就可以称为原子性读写

```null
lock (locker) { if (x != 0) y /= x; }

```

可以说x和y是被原子的访问的，因为上面的代码块无法被其它的线程分割或抢占（cpu悲观锁/总线锁）。如果被其它线程分割或抢占，x和y就可能被别的线程修改导致计算结果无效（cpu乐观锁/缓存锁）。而现在 x和y总是在相同的排它锁中进行访问，因此不会出现除数为零的错误。

如果lock代码块内发生异常，原子性将被打破

```null
decimal _savingsBalance, _checkBalance;

void Transfer (decimal amount)
{
  lock (_locker)
  {
    _savingsBalance += amount;
    _checkBalance -= amount + GetBankFee();
  }
}

```

如果GetBankFee()方法内抛出异常，银行可能就要亏钱了。在这个例子中，我们可以通过更早的调用GetBankFee()来避免这个问题。对于更复杂情况，解决方案是在catch或finally中实现“回滚（rollback）”逻辑。

二、锁构造
-----

#### Monitor

C# 的lock语句是一个语法糖，它其实就是使用了try / finally来调用Monitor.Enter与Monitor.Exit方法：

```null
bool taken = false;
try
{
    
    Monitor.Enter(_locker,ref taken);
    num++;
}
finally
{
    
    if (taken) Monitor.Exit(_locker);
}

```

Monitor是【可重入的（Reentrant）】,只有当最外层的lock语句退出或是执行了匹配数目的Monitor.Exit语句时，对象才会被解锁。

```null
static void Main()
{
    lock (locker)  
    {
        AnotherMethod();
        
    }
}

static void AnotherMethod()
{
  lock (_locker) { Console.WriteLine ("Another method"); }
}

```

Monitor的性能：在一个 2010 时代的计算机上，没有竞争的情况下获取并释放锁一般只需 20 纳秒。如果存在竞争，产生的上下文切换会把开销增加到微秒的级别，并且线程被重新调度前可能还会等待更久的时间。如果需要锁定的时间很短，那么可以使用【自旋锁（SpinLock）】来避免上下文切换的开销。

Monitor还提供了一个TryEnter方法，允许以毫秒或是TimeSpan方式指定超时时间。如果获得了锁，该方法会返回true，而如果由于超时没有获得锁，则会返回false。TryEnter也可以不传递超时时间进行调用，这是对锁进行“测试”，如果不能立即获得锁就会立即返回false。

> 如果获取锁后保持的时间太长而不释放，就会降低并发度，同时也会加大【死锁】的风险。

#### Mutex

Mutex互斥体类似于Monitor，不同在于它是可以跨越进程工作。换句话说，Mutex可以是机器范围（computer-wide）的，也可以是程序范围（application-wide）的。

> 没有竞争的情况下，获取并释放Mutex需要几微秒的时间，大约比lock慢 50 倍。

使用Mutex类时，可以调用WaitOne方法来加锁，调用ReleaseMutex方法来解锁。关闭或销毁Mutex会自动释放锁。与lock语句一样，Mutex只能被获得该锁的线程释放。

跨进程Mutex的一种常见的应用就是确保只运行一个程序实例。

```null


using (var mutex = new Mutex (false, "Global\oreilly.com OneAtATimeDemo"))
{
  
  if (!mutex.WaitOne (TimeSpan.FromSeconds (3), false))
  {
    Console.WriteLine ("Another app instance is running. Bye!");
    return;
  }
  RunProgram();
}

```

> 如果在终端服务（Terminal Services）下运行，机器范围的Mutex默认仅对于运行在相同终端会话的应用程序可见。要使其对所有终端会话可见，需要在其名字前加上Global\\。

#### 死锁

当两个线程等待的资源都被对方占用时（A等B，B等A），它们都无法执行，这就产生了死锁。更复杂的死锁链可能由三个或更多的线程创建。

```null
object locker1 = new object();
object locker2 = new object();

new Thread(() =>
{
    lock (locker1)
    {
        Thread.Sleep(1000);
        lock (locker2)  
        {
            
        }
    }
}).Start();

lock (locker2)
{
    Thread.Sleep(1000);
    lock (locker1)  
    {
        
    }
}

```

CLR 不会像SQL Server一样自动检测和解决死锁。除非你指定了锁定的超时时间，否则死锁会造成参与的线程无限阻塞。（在SQL CLR 集成宿主环境中，死锁能够被自动检测，并在其中一个线程上抛出可捕获的异常。）

死锁是多线程中最难解决的问题之一，尤其是在有很多关联对象的时候。这个困难在根本上在于无法确定调用方（caller）已经拥有了哪些锁。

你可能会锁定类x中的私有字段a，而并不知道调用方（或者调用方的调用方）已经锁住了类y中的字段b。同时，另一个线程正在执行顺序相反的操作，这样就创建了死锁。讽刺的是，这个问题会由于（良好的）面向对象的设计模式而加剧，因为这类模式建立的调用链直到运行时才能确定。

流行的建议：“以一致的顺序对对象加锁以避免死锁”，尽管它对于我们最初的例子有帮助，但是很难应用到刚才所描述的场景。更好的策略是：如果发现在锁区域中的对其它类的方法调用最终会引用回当前对象，就应该小心，同时考虑是否真的需要对其它类的方法调用加锁（往往是需要的，但是有时也会有其它选择）。更多的依靠声明方式（declarative）与数据并行（data parallelism）、不可变类型（immutable types）与非阻塞同步构造（ nonblocking synchronization constructs），可以减少对锁的需要。

> 有另一种思路：当你在拥有锁的情况下访问其它类的代码，对于锁的封装就存在潜在的泄露。这不是 CLR 或 .NET Framework 的问题，而是因为锁本身的局限性。某些人认为造成这样问题的根因是可重入？

三、信号构造
------

#### Semaphore

信号量类似于一个通道：它具有一定的容量（capacity）房间，并且有保安把守。一旦满员，就不允许其他人进入，这些人将在外面排队。当有一个人离开时，排在最前头的人便可以进入。

容量为1的的信号量就是一把互斥锁，类似mutex，不同的是信号量没有【所有者】，它是线程无关（thread-agnostic）的。任何线程都可以在调用Semaphore上的Release方法，而对于mutex，只有获得锁的线程才可以释放该锁。

> SemaphoreSlim是 standard1.0 就支持的轻量级的信号量，功能与Semaphore相似，不同之处是它对于并行编程的低延迟需求做了优化；支持在等待时指定取消标记 （cancellation token）。但它不能跨进程使用，Semaphore可以。
> 
> 在Semaphore上调用WaitOne或Release会产生大概 1 微秒的开销（无竞争情况下），而SemaphoreSlim产生的开销约是其四分之一。

#### ManualResetEvent

ManualResetEvent 调用WaitOne进入阻塞，任意可访问的线程都能调用Set方法来放行。

```null
var waitHandle = new ManualResetEvent(false);

Task.Run(() =>
{
    _testOutputHelper.WriteLine(Thread.CurrentThread.ManagedThreadId + " 尝试进门...");
    waitHandle.WaitOne();
    _testOutputHelper.WriteLine(Thread.CurrentThread.ManagedThreadId + " 进去了");
    业务逻辑();
    _testOutputHelper.WriteLine("当前门的状态是开启的吗？"+waitHandle.WaitOne(0)); 
});
Thread.Sleep(1000);
_testOutputHelper.WriteLine(Thread.CurrentThread.ManagedThreadId + " say：我来开门");
waitHandle.Set();

```

#### AutoResetEvent

AutoResetEvent 如其命名，收到通知后他能自动复位（reset），而ManualResetEvent不能。

```null
EventWaitHandle waitHandle = new AutoResetEvent(false);
 
 Task.Factory.StartNew(() =>
 {
     _testOutputHelper.WriteLine(Thread.CurrentThread.ManagedThreadId + " 尝试进门...");
     waitHandle.WaitOne();
     _testOutputHelper.WriteLine(Thread.CurrentThread.ManagedThreadId + " 进去了");
     业务逻辑();
     _testOutputHelper.WriteLine("当前门的状态是开启的吗？"+waitHandle.WaitOne(0)); 
     waitHandle.Set();
     Task.Run(() =>
     {
         waitHandle.WaitOne();  
         _testOutputHelper.WriteLine(Thread.CurrentThread.ManagedThreadId + " 进去了");
         业务逻辑();
     });
 });
 Thread.Sleep(1000);
 _testOutputHelper.WriteLine(Thread.CurrentThread.ManagedThreadId + " say：我来开门");
 waitHandle.Set();
 
 Thread.Sleep(1000); 

```

> 从 Framework 4.0 开始，提供了另一个版本的ManualResetEvent，名为ManualResetEventSlim。 后者为短等待时间做了优化，它提供了进行一定次数迭代自旋的能力，也实现了一种更有效的管理机制，允许通过CancellationToken取消Wait等待。但它不能用于跨进程的信号同步。 ManualResetEventSlim不是WaitHandle的子类，但它提供一个WaitHandle的属性，会返回一个基于WaitHandle的对象（使用它的性能和一般的等待句柄相同）。

EventWaitHandle的构造方法允许以命名的方式进行创建，这样它就可以跨进程使用。名称就是一个字符串，可以随意起名，但是注意不要和别人的命名冲突！如果名字在计算机上已存在，你就会获取一个它对应的EventWaitHandle的引用，否则操作系统会创建一个新的。

```null
EventWaitHandle wh = new EventWaitHandle (false,EventResetMode.AutoReset,"MyCompany.MyApp.SomeName");

```

使用AutoResetEvent实现简易的生产消费队列

```null
public class PCQueue:IDisposable
{
    private EventWaitHandle _waitHandle = new AutoResetEvent(false);
    private Thread _worker;
    private readonly object _locker = new object();
    private Queue<string> _tasks = new Queue<string>();

    private readonly ITestOutputHelper _testOutputHelper;

    public PCQueue(ITestOutputHelper testOutputHelper)
    {
        _testOutputHelper = testOutputHelper;

        _worker = new Thread(Work);
        _worker.Start();
    }

    public void AddTask(string task)
    {
        lock (_locker)
        {
            _tasks.Enqueue(task);
        }

        _waitHandle.Set();
    }

    private void Work()
    {
        while (true)
        {
            string task = null;
            lock (_locker)
            {
                if (_tasks.Count > 0)
                {
                    task = _tasks.Dequeue();
                    if (task == null) return;  
                }
            }

            if (task == null)
            {
                _waitHandle.WaitOne();  
            }
            else
            {
                _testOutputHelper.WriteLine("执行任务：" +task);
                Thread.Sleep(1000);  
            }
        }
    }

    public void Dispose()
    {
        AddTask(null);  
        _worker.Join();  
        _waitHandle.Close();  
        _tasks.Clear();
    }
}

```

#### CountdownEvent

CountdownEvent可以用于多线程等待，这个类型是 Framework 4.0 加入的，并且是一个高效的纯托管实现。

```null
CountdownEvent _countdown = new CountdownEvent (3);

void 测试CountdownEvent等待()
{
    Task.Run(工作);
    Task.Run(工作);
    Task.Run(工作);

    _countdown.Wait();
    _testOutputHelper.WriteLine("大家都干完了");
}

void 工作()
{
    _testOutputHelper.WriteLine("干活");
    Thread.Sleep(1000);
    _countdown.Signal();
}

```

四、等待句柄
------

#### 等待句柄和线程池

如果你的应用有很多线程，这些线程大部分时间都在阻塞，那么可以通过调用`ThreadPool.RegisterWaitForSingleObject`来减少资源消耗。当向等待句柄发信号时（或者已超时），委托（这里的Work方法）会在一个线程池线程运行。

```null
[Fact]
void Show()
{
    var _waitHandle = new ManualResetEvent(false);
    var reg = ThreadPool.RegisterWaitForSingleObject(_waitHandle, Work, "hahah", -1, true);
    Thread.Sleep(3000);
    _testOutputHelper.WriteLine("发送复位信号");
    _waitHandle.Set();
    reg.Unregister(_waitHandle);
}
private void Work (object data, bool timedOut)
{
    _testOutputHelper.WriteLine ("Say - " + data);
    
}

```

#### WaitHandle

除了`Set`、`WaitOne`和`Reset`方法外，在`WaitHandle`类上还有一些静态方法用来解决更复杂的同步问题。

`WaitAny`、`WaitAll`和`SignalAndWait`方法可以向多个等待句柄发信号和进行等待操作。等待句柄可以是不同的类型（包括 `Mutex`、`Semaphore`、`CountdownEvent`等，因为它们都派生自抽象类`WaitHandle`）。

*   `waitAny` 等待一组等待句柄中任意一个
    
*   `WaitAll` 等待给定的所有等待句柄。这个等待是原子的。
    
*   `SignalAndWait`会调用一个等待句柄的`Set`方法，然后调用另一个等待句柄的`WaitOne`方法。在向第一个句柄发信号后，会（让当前线程）跳到第二个句柄的等待队列的最前位置（插队）。
    
    ```null
    WaitHandle.SignalAndWait (wh1, wh2);
    
    ```
