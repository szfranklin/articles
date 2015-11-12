#在数据分析流程中整合 Python 和 R（二）
之前一篇文章中，我们探讨了为什么要在同一个流程中整合 Python 和 R ，以及如何使用无相对格式的文件作为中转。上文还涉及了如何使用命令行运行 Python 或 R 脚本，如何获取附加的参数并解析。本文将继续完善这个主题，示范如何通过相互调用使 Python 脚本和 R 脚本相连。

 

##命令行执行和子过程的执行

这里有必要复习一下用命令行执行 Python 或 R 过程的原理，以便读者更好理解子过程如何被执行。当运行下面这条命令时，会启动一个新的 Python 过程来执行脚本：

python path/to/myscript.py arg1 arg2 arg3

执行期间，所有打印到标准输出/标准错误流（standard output and standard error streams）的输出，都会被显示在操纵台（console）。获取（这些输出）最常见的方式是使用各语言内置的功能（Python的print() ，R的cat() 或print() )。这些功能会写一个字符串到标准输出流。脚本执行结束后， Python 过程会被关闭。

用这种方式运行命令行脚本，尽管有用，但在脚本较多时，可能变得枯燥乏味、容易出错。不过，使用相似的方式，可以让 Python 或 R脚本直接相互调用。比如，让一个 Python 父过程指派一个 R 子进程运行特定脚本进行分析。R 脚本运行完毕后，可以直接把输出传到Python 父进程，而不是打印到操纵台。使用这种方法就不必再在命令行中手动执行各个步骤。

 

##实例

这里举两个例子来示范相互调用：一个是 Python 调用 R ，另一个是 R 调用 Python 。案例中刻意选择了一些无关紧要的分析任务，以便能专注于展示相互调用背后的机制及其达成方式。

1. 1 R 脚本示例

这里用来举例的 R 脚本，用途是从命令行接收一组数字，并返回其中最大的。

max.R

获取来自命令行的参数

myArgs <- commandArgs(trailingOnly = TRUE)

转换为数字格式
nums = as.numeric(myArgs)

用cat把结果写至标准输出流

cat(max(nums))

1.2 通过 Python 执行 R 脚本

执行这个脚本需要使用 subprocess 模块。这个模块是 Python 标准库的一部分。使用此模块的 check_output  功能调用 R 脚本。

要在 Python 中执行 R 脚本 max.R ，首先需建立所要执行的命令。这里使用与第一篇文章的命令行语句相似的格式。此语句格式在 Python 中被表示为一列字符串，所包含的元素如下：['<要执行的命令>', '<脚本路径>', '参数arg1' , '参数arg2', '参数arg3', '参数arg4']

代码示例：

run_max.py
import subprocess

定义command命令和参数

command ='Rscript'
path2script ='path/to your script/max.R'

一组有数字组成的参数args

args = ['11','3','9','42']

建立subprocess命令

cmd = [command, path2script] + args

用check_output执行命令，储存结果

x = subprocess.check_output(cmd, universal_newlines=True)

print('The maximum of the numbers is:', x)

这里的参数 universal_newlines=True 是让 Python 把输出视为文本字符串，并处理 Windows 和 Linux 的换行符。如果此参数被省略，那么输出会以比特字符串返回，则在进一步运算之前必须用x.decode()解码为文本。

2.1  Python 脚本实例

这里使用一个简单的 Python 脚本进行举例。此脚本将一个字符串（第一个参数）根据给定的某个子字符串模式（第二个参数）进行拆分。之后逐行将各个子字符串打印到操纵台。

splitstr.py
import sys

获取参数
string = sys.argv[1]
pattern = sys.argv[2]

字符串拆分
ans = string.split(pattern)

把拆分后的元素（子字符串）通过一个换行符合并

打印处理过的字符串

print('\n'.join(ans))

2.2 通过 R 执行 Python 脚本

当需要用 R 执行子过程时，推荐使用 system2 功能来执行和捕获输出。R 内置的 system 功能不好用，而且不能跨平台兼容。

建立要执行的命令与上面 Python 的例子相似，不过 system2  要求命令和参数被分开解析。此外，第一个参数必须是脚本路径。

路径名中包含空格时可能会有麻烦。解决的办法是将路径名用双引号把路径名括上，然后再整体加单引号，这样 R 就能在参数中保留双引号。

代码示例：

run_splitstr.R

command ="python“

（如果有空格）注意单引号+双引号
path2script='"path/to your script/splitstr.py"'

将各参数args表示为一个向量

string ="3523462---12413415---4577678---7967956---5456439"
pattern ="---"
args = c(string, pattern)

添加脚本路径，作为第一个参数

allArgs = c(path2script, args)

output = system2(command, args=allArgs, stdout=TRUE)

print(paste("The Substrings are:\n", output))

为了将标准输出捕获为一个字符向量（每个元素一行），必须在 system2 中标明 stdout=TRUE ，否则只有退出状态 (exit status) 会被返回。当标明 stdout=TRUE 时，退出状态被储存在一个叫“status”的属性中。

 

##总结

使用子过程调用的方式，可以在同一个应用中整合 Python 和 R 。这个方法允许父过程调用子过程，捕获打印在标准输出流的输出。本文通过实例示范了使用这种途径让 Python 和 R 相互调用的方式。

本系列下一篇文章会利用本文和前文的材料，展示一个在应用中同时使用 Python 和 R 的真实案例。

 

 

原作者：Chris Musselle

翻译：王鹏宇

原文链接：http://www.mango-solutions.com/wp/2015/10/integrating-python-and-r-part-ii-executing-r-from-python-and-vice-versa/

 

