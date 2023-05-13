# 三种方法，自动生成好记又好用的密码 ｜ 少数派会员 π+Prime
[三种方法，自动生成好记又好用的密码 ｜ 少数派会员 π+Prime](https://sspai.com/prime/story/password-autogen) 

 尽管「取消密码」的风越刮越猛，但距离我们真正摆脱密码大概还有些时日，而现有密码生成工具并不那么好用。本文将介绍三种自动化生成易识记密码的方法： 快捷指令、终端脚本，当然还有 GPT。

引言
--

尽管「取消密码」的风越刮越猛，但距离我们真正摆脱密码大概还有些时日。换句话说，我们还有大把机会「享受」盯着键盘反复编造密码的体验。

对此，不少密码管理工具都体谅用户，提供了自动生成密码的功能。1Password 这样的专业选手自不必说，iOS 和 macOS 内置的密码管理功能也可以实现。

但如果你尝试过这些工具，可能会注意到它们建议的密码往往不那么好使：如果完全是乱序组合，虽然足够难猜，但记忆和输入就过于麻烦；即使像 1Password 那样支持生成「易记」（memorable）形式的密码（即由若干单词组合、修改而得的字符串），其选用的单词又经常过于晦涩生僻，对于国内用户并不能起到易记的效果。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-5-13%2015-32-48/37275726-e92e-459f-9ae0-837acfcd6312.png?raw=true)

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-5-13%2015-32-48/0063c854-d6ed-486d-8a79-74d9de33f767.png?raw=true)

看来，合适的密码还是得按自己的规则来。考虑到如今的密码一般需要有一定长度，并且规定同时包含大小写字母、数字和特殊字符，我们可以按照这样一种格式编写密码，以便在易记的同时符合大多网站的要求：

*   由三个常见小写英文单词构成，单词长度在 4—6 个字母之间，之间用连字符 `-` 连接。这样，所得密码的长度在 14 到 20 字符之间，符合常见强密码要求。
*   将第一个单词中的部分字母替换为形态类似的数字（例如 `a` 换为 `4`，`e` 换为 `3`，`o` 换为 `0` 等，即早年 BBS 文化中称为 [leetspeak](https://sspai.com/link?target=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FLeet) 的做法），其他字母替换为大写。这就满足了含有数字和大小写的长度要求。
*   将第二个单词中的部分字母替换为形态类似的特殊符号（例如 `a` 换为 `@`，`l` 换为 `!`，`s` 换为 `$` 等）。这就满足了含有特殊字符的长度要求。

例如：

*   `G00D-b@d-ugly`（由 `good, bad, ugly` 修改而来，13 字符长）；
*   `S1LV3R-bu!!3t-joker`（由 `silver, bullet, joker` 修改而来，18 字符长）；
*   `B1G-mou+h-candy`（由 `big, mouth, candy` 修改而来，15 字符长）。

下面，我们看看如何用自动化方法批量获得这样的密码。为了兼顾易用和泛用，本文将介绍快捷指令和终端脚本两种方法。前者适合在 iOS 上运行，后者适合在电脑上整合到各种第三方效率工具中，也可以跨平台使用。文末还会跑题介绍一种不那么酷、但如今无法回避的思路——当然，我是在说 GPT。

使用方法
----

首先，分别下载我做好的成品：[快捷指令](https://sspai.com/link?target=https%3A%2F%2Fwww.icloud.com%2Fshortcuts%2Fb932cefd04024bfd9a1569fe7e73e890) | [终端脚本](https://sspai.com/link?target=https%3A%2F%2Fgist.github.com%2Ffirexcy%2F75ffe0cc7707d6f5d506206b06b9dca1)

对于快捷指令版本，直接运行即可，所得密码会复制到剪贴板以便输入，密码和对应的易记版本还会以通知形式显示。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-5-13%2015-32-48/0f5299da-0cde-43a6-9b62-bbb175ed18ee.png?raw=true)

对于终端脚本版本，直接运行同样会输出生成的密码和对应易记版本。如果你需要将其整合到第三方效率工具，可以自行微调第 9—10 行负责输出的命令。

```null
./pwgen.sh
# example output 1
W1N3-(orn-code
wine corn code
# example output 2
0FF3ND-bro@d-beat
offend broad beat
# example output 3
57R337-$+o(k-easily
street stock easily
```

制作步骤和原理解释
---------

### 准备密码词表

如开头所说，现有易记密码生成方案的普遍问题是单词过于晦涩，记忆和输入都不够方便。因此，本文需求的前置步骤就是挑选一个合适的词表，作为生成密码的词汇来源。

什么词表比较合适呢？我见过英文社区有些用户选择了 [Scrabble](https://sspai.com/link?target=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FScrabble)、[Spelling Bee](https://sspai.com/link?target=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FSpelling_bee) 等单词游戏的词表（例如 Dr Drang [使用 Python 制作的脚本](https://sspai.com/link?target=https%3A%2F%2Fleancrew.com%2Fall-this%2F2020%2F10%2Fpasswords-widgets-and-pyto%2F)），但这些游戏是为了比拼词汇量而生的，其词表里也有不少生僻词，对母语使用者都有些门槛，非母语使用者就更不用说了。

我想到了「[牛津 3000](https://sspai.com/link?target=https%3A%2F%2Fwww.oxfordlearnersdictionaries.com%2Fabout%2Fwordlists%2Foxford3000-5000)」（Oxford 3000）。买过牛津词典学习型系列的读者可能会想起在附录中见过这个名字，它是牛津出版社认为「每个英语学习者都需要知道的核心单词」的列表。实际上，《牛津高阶英语学习词典》的释义都是采用牛津 3000 列表中的单词编写的。对于国内用户来说，牛津 3000 中的单词基本就是初中水平，几乎都已经识记，拿来做密码词表是没有语言门槛的。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-5-13%2015-32-48/df6b03dd-51c8-4123-aec0-48943531ac32.png?raw=true)

确定了目标，下一步就是怎么把这个词表弄下来。牛津 3000 词表在牛津词典网站上是[免费提供](https://sspai.com/link?target=https%3A%2F%2Fwww.oxfordlearnersdictionaries.com%2Fwordlists%2Foxford3000-5000)的（本文使用美语拼法、字母序排列版本演示，下载所得文件为 `American_Oxford_3000.pdf`），但只有 PDF 格式，需要先行转换为程序方便处理的纯文本列表。

为此，我们先用 `pdftotext` 工具将 PDF 中的文本都提取出来。`pdftotext` 是命令行 PDF 工具集 [Poppler](https://sspai.com/link?target=https%3A%2F%2Fpoppler.freedesktop.org%2F) 中附带的，需要先通过 `brew installer poppler` 安装。当然，你也可以选择直接复制粘贴，但取决于所用 PDF 阅读器，复制结果可能没那么干净。

在终端运行：

```null
pdftotext -nopgbrk American_Oxford_3000.pdf -
```

这里，`-nopgbrk` 选项忽略在遇到换页时生成的特殊字符，结尾的 `-` 将结果输出到终端而不是保存成文件。

结果如下（节选）：

```null
...
abandon v. B2
ability n. A2
...
wind1 n. A2
wind2 v. B2
...
```

可以看到，每个词条主要由三项信息构成：单词、词性、[CEFR](https://sspai.com/link?target=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FCommon_European_Framework_of_Reference_for_Languages) 难度等级，各项信息之间用空格分开。此外，个别多义词会后接数字区分不同词性的条目。

这种用固定字符分开的格式，是典型适合用 `awk` 命令处理的类型。经过一番尝试，我们可以使用下面这串命令来做清洁工作：

```null
pdftotext -nopgbrk American_Oxford_3000.pdf - |\
    awk '{print $1}' |\
    awk 'length >= 4 && length <= 6' |\
    awk 'NF>0' |\
    tr -cd '[:alpha:]\n' |\
    sort |\
    uniq > oxford3000.txt
```

这里：

*   `awk '{print $1}'`：是指对于输入中的每一行，只保留第一列（`$1`）。`awk` 默认以空格作为列的分隔符，因此对于「牛津 3000」词表的格式，保留第一列就是只保留单词部分（少数不遵守该格式的行除外）。
*   `awk 'length >= 4 && length <= 6'`：是指检查每一行的长度（`length`），只保留 4 到 6 个字符长度的行。
*   `awk 'NF>0'`：是指检查每一行的列数（`NF`），只保留列数大于零的行。这本质上就是在删除空行。
*   `tr -cd '[:alpha:]\n'`：是指删除（`-d`）每行中后接表达式不能匹配（`-c`）的部分。使用的表达式是 `[:alpha:]\n`，即任何字母加上换行；除此范围之外的内容都会被删除。（`tr` 使用的是类似 `grep` 的「基础」正则表达式 BRE，看着会比较啰嗦。）这样做的目的是删除词表中多余的句号、数字、空白符等。
*   最后，`sort | uniq` 的组合用来删除重复行（考虑到多义词词条），并获得按字母排序的输出结果。

打开输出的 `oxford3000.txt`，可以看到处理效果比较完美，得到了大约 1500 个长度适中、简洁易记的单词作为密码素材。唯一的漏网之鱼是 `adj.`，这是因为原文件个别条目拆写为两行，第二行开头就是词性。不过，对于这种一次性的任务，我们也没有必要过度优化，手动删除即可。

### 制作快捷指令

动手之前，先做一下宏观规划。根据文首确定的需求，我们的快捷指令大致依次要完成这些工作：

1.  从词表中随机提取三个单词；
2.  对单词做字母替换、大小写替换等改动；
3.  将处理结果组合成密码并输出。

下面分别说明其中的要点或难点。

#### 从词表中随机提取单词

为了从牛津 3000 词表中随机提取单词，首先需要将它存储为快捷指令可以读取的格式。为此，建立一个「文本」动作，粘入之前整理好的文本内容，后接一个按行「拆分文本」动作，就获得了一个可以被其他动作引用的「列表」类型变量。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-5-13%2015-32-48/d1b5c8c0-9390-41e2-9582-7561380474c6.png?raw=true)

例如，要随机获得一个单词，只要使用「从列表获取项目动作」，将获取内容选为「随机项目」即可。反复使用同一方法，即可获得作为密码加工原料的三个单词。

#### 修改单词中的字母

如上所述，为了满足密码含有数字和特殊字符的要求，我们需要将单词修改为所谓的 leetspeak 版本，也就是把字母换成形态类似的数字或符号，常见的对应关系如下表所示：

| 原始字母 | 形近数字 | 形近字符 |
| --- | --- | --- |
| a | 4 | @ |
| c |   | ( |
| e | 3 | & |
| i | 1 | ! |
| o | 0 |   |
| s | 5 | $ |
| t | 7 | + |

对此，一种最简单粗暴的思路就是反复查找替换，但这显然不够优雅。想到快捷指令中支持「词典」类型，可以像查词典一样查询输入内容对应的值；因此，只要准备一个以 26 个字母为键（key）的词典，将上表涉及的字母对应的值改为形近数字（或字符），其他字母的值仍为自身，就可以通过一一查询单词中字母的方法获得 leetspeak 版本。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-5-13%2015-32-48/e558c4ea-27df-4e43-aef0-72f01971cf1c.png?raw=true)

关于词典的制作也有讲究。快捷指令默认的词典制作方式是填表格，但对于本文需要的这种多达几十行的词典，一个个点击填写很浪费时间。实际上，只要将对应关系用纯文本的 JSON 格式表达出来（这里可以用到文本编辑器的很多批量化功能），后接一个「获取词典」步骤，就能节省大量操作。

这样，我们就快速做出了两个词典，分别将字母翻译为形近的数字和字符。

接下来，只要根据文首确定的格式，将随机抽取的单词拆分为字符，然后用循环动作获取每个字符在词典中对应的值，将所得结果再次合并即可。至于更改大小写，快捷指令有同名内置动作，无需额外编写。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-5-13%2015-32-48/5d296417-1c12-43c0-bd44-0a49915465f6.png?raw=true)

#### 整合并输出

这一步本身很简单，要考虑的无非是一些（自己的）用户体验问题。

例如，考虑到使用场景，应该将所得密码复制到剪贴板中，以便随用随粘；又如，考虑到促进识记，以及在特殊情况下进一步编辑的需要（例如有些网站并不支持特殊字符，或者密码长度上限较低），还应该在通知中给出未经变形的原始单词。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-5-13%2015-32-48/2ff6314c-69b9-4d25-9900-04ede58f1d9b.png?raw=true)

### 制作终端脚本

相比于快捷指令，这其实是我个人更喜欢的版本。

每个自动化工具都有自己最适合的领域。快捷指令更擅长的是调用设备传感器、以及调用系统原生的功能和服务；但对于本文这种以处理文本为主的需求，实现起来就很折腾。如果不是为了写这篇文章，我根本没有任何动力打开快捷指令做这种事情。

相比之下，不太复杂的文本自动化正是终端脚本的「舒适区」。从前面给出的[成品](https://sspai.com/link?target=https%3A%2F%2Fgist.github.com%2Ffirexcy%2F75ffe0cc7707d6f5d506206b06b9dca1)内容就能看出，本文需求用终端脚本做起来是很快的，这里也只简单解释几处要点：

*   `pick` 函数负责从词表中随机提取单词。这里，为了避免将词表单独存储在外部文件中（因为那样不方便整合进第三方工具），我们用 [heredoc](https://sspai.com/link?target=https%3A%2F%2Fwww.gnu.org%2Fsoftware%2Fbash%2Fmanual%2Fbash.html%23Here-Documents) 的形式将整个词表输入给 `cat`，通过管道原样交给 `shuf` 随机挑选一行（`-n 1`）作为输出。

```
pick() {
    cat <<EOF | shuf -n 1
able
... 
# Rest of the list
}
```

*   `main` 函数是程序主体。其实本来这几个简单的步骤没有必要放在函数里，这里唯一的目的是方便和美观；否则，其中的命令得写到占据一千多行的词表之后去，为了我们的手指健康考虑还是避免这么做。这样，只要在最后一行调用一下 `main` 就行了，主要的编写工作都可以在脚本开头完成。

```
main() {
    part1=$(pick)
    part2=$(pick)
    part3=$(pick)
    part1a=$(echo "$part1" | tr 'aeiost' '431057' | tr '[:lower:]' '[:upper:]')
    part2a=$(echo "$part2" | tr 'aceist' '@(&!$+')
    echo "$part1a-$part2a-$part3"
    echo "$part1 $part2 $part3"
}
```

*   单词的修改通过 `tr` 命令完成。这里充分体现了终端脚本的优势：快捷指令中用上 JSON 词典的奇技淫巧还要折腾好几步的功能，在这里一行就可以完事。具体而言，当 `tr` 后接两个字数相当的字符串（或小写字母 `[:lower:]`、大写字母 `[:upper:]` 等代号）时，其功能是逐一检查输入中的每个字符，如果在第一个字符串中找得到，就将其转为第二个字符串对应位置的字符。

加场
--

这个方法写在最后，因为：

1.  要花钱；
2.  原理决定了输出结果可能是别人用过的；
3.  缺少了一些庸人的乐趣。

但还是得写，因为现在是 2023 年。

请打开你的 ChatGPT、OpenAI Playground、Bing Chat，或者你能摸得到的任何 LLM 聊天工具，把下面的内容喂给它（为了表述简洁，跟文首需求稍有差别，但不影响效果）：

```null
Generate three random passwords, each of which shall:

1. comprise of three distinct and common (within the Oxford 3000 list) english words, combined with hyphens (`-`) in between, with certain letters replaced with their "leetspeak" alternatives; and
2. have 12 to 20 characters in total, with both upper- and lower-case letters, 2 or more numbers, and 2 or more non-alphanumeric ASCII characters.

1. G00D-b@d-ug1y (from "good-bad-ugly", 13 characters)
2. 
```

注意：

*   最后的列表空了一截是为了让模型照葫芦画瓢，输出会自然续写下去；
*   如果有不同格式要求，可以相应调整 prompt 中的表述；
*   我无法确认它知不知道 Oxford 3000 是什么东西，但还是先写上了，反正没有坏处；
*   如果你有得选，实测最合适的模型是 text-davinci-003 或 gpt-3.5-turbo，更简单的如 curie、babbage 领会不了，更复杂的 gpt-4 有点大炮打蚊子。

结果如下，绿色部分是它的回复：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-5-13%2015-32-48/1c8f6660-9f88-4fe7-b4ec-236f26c5b7e2.png?raw=true)

行……给你一朵小红花。