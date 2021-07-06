---
title: using as
date: 2021-06-29 00:05:24
tags:
---

# Using as(The GNU Assembly)

By Dean Elsner, Jay Fenlason & friends
Edited by Cygnus Support


# 概要
这是手册是一个GNU汇编器as的使用手册。
下面是一个如何使用as的简要说明。具体的使用细节，可以在第二章[Command Line Options]中看到。


as [-a[cdhlns]] [=file] [-D] [-defsym sym=val] [-f] [-gstabs] [-gdwarf2] [-help] [-I dir] [-J] [-K] [-L] [-listing-lhs-width=NUM] [-listing-lhs-width2=NUM] [-listing-rhs-width=NUM] [-listing-cont-lines=NUM] 
[-keep-locals] [-o objfile] [-R] [-statistic] [-v] [-version] [-W] [-warn] [-fatal-warnings] [-w] [-x] [-Z] [-target-help] [target-options] [-|files ...]

Alpha平台选项：
 [-mcpu]
 [-mdebug | -no-mdebug]
 [-relax] [-g] [-Gsize]
 [-F] [-32addr]


ARC平台选项:
[-marc[5|6|7|8]]
[-EB|-EL]

ARM平台选项:
[-mcpu=processor [+extension ...]]
[-march=architecture [+extension ...]]
[-mfpu=floating-point-fromat ]
[-mthumb]
[-EB|-EL]
[-mapcs-32|-mapcs-26|-mapcs-float|
-mapcs-reentrant]
[-mthumb-interwork] [-moabi] [-k]

CRIS 平台选项:
[–underscore | –no-underscore]
[–pic] [-N]
[–emulation=criself | –emulation=crisaout]

D10V 平台选项:
[-O]

D30V 平台选项:
[-O|-n|-N]2

i386 平台选项:
[–32|–64]

i960 平台选项:
[-ACA|-ACA A|-ACB|-ACC|-AKA|-AKB|
-AKC|-AMC]
[-b] [-no-relax]

Target IP2K 平台选项:
[-mip2022|-mip2022ext]

Target M32R 平台选项:
[–m32rx|–[no-]warn-explicit-parallel-conflicts|
–W[n]p]

Target M680X0 平台选项::
[-l] [-m68000|-m68010|-m68020|...]

Target M68HC11 平台选项::
[-m68hc11|-m68hc12|-m68hcs12]
[-mshort|-mlong]
[-mshort-double|-mlong-double]
[–force-long-branchs] [–short-branchs]
[–strict-direct-mode] [–print-insn-syntax]
[–print-opcodes] [–generate-example]

Target MCORE 平台选项::
[-jsri2bsr] [-sifilter] [-relax]
[-mcpu=[210|340]]

Target MIPS 平台选项::
[-nocpp] [-EL] [-EB] [-n] [-O[optimization level ]]
[-g[debug level ]] [-G num ] [-KPIC] [-call shared]
[-non shared] [-xgot] [–membedded-pic]
[-mabi=ABI ] [-32] [-n32] [-64] [-mfp32] [-mgp32]
[-march=CPU ] [-mtune=CPU ] [-mips1] [-mips2]
[-mips3] [-mips4] [-mips5] [-mips32] [-mips32r2]
[-mips64]
[-construct-floats] [-no-construct-floats]
[-trap] [-no-break] [-break] [-no-trap]
[-mfix7000] [-mno-fix7000]
[-mips16] [-no-mips16]
[-mips3d] [-no-mips3d]
[-mdmx] [-no-mdmx]
[-mdebug] [-no-mdebug]

Target MMIX 平台选项:
[–fixed-special-register-names] [–globalize-symbols]
[–gnu-syntax] [–relax] [–no-predefined-symbols]
[–no-expand] [–no-merge-gregs] [-x]
[–linker-allocated-gregs]

Target PDP11 平台选项::
[-mpic|-mno-pic] [-mall] [-mno-extensions]
[-mextension |-mno-extension ]
[-mcpu ] [-mmachine ]

Target picoJava 平台选项::
[-mb|-me]

Target PowerPC 平台选项::
[-mpwrx|-mpwr2|-mpwr|-m601|-mppc|-mppc32|-m603|-m604|
-m403|-m405|-mppc64|-m620|-mppc64bridge|-mbooke|
-mbooke32|-mbooke64]
[-mcom|-many|-maltivec] [-memb]
[-mregnames|-mno-regnames]
[-mrelocatable|-mrelocatable-lib]
[-mlittle|-mlittle-endian|-mbig|-mbig-endian]
[-msolaris|-mno-solaris]

Target SPARC 平台选项::
[-Av6|-Av7|-Av8|-Asparclet|-Asparclite
-Av8plus|-Av8plusa|-Av9|-Av9a]
[-xarch=v8plus|-xarch=v8plusa] [-bump]
[-32|-64]

Target TIC54X 平台选项::
[-mcpu=54[123589]|-mcpu=54[56]lp] [-mfar-mode|-mf]
[-merrors-to-file <filename>|-me <filename>]

Target Xtensa 平台选项::
[–[no-]density] [–[no-]relax] [–[no-]generics]
[–[no-]text-section-literals]
[–[no-]target-align] [–[no-]longcalls]


下面依次解释各个参数的含义：
-a[cdhlmns]
   可以以多种方式展开这个参数列表：
   -ac 忽略值为false的条件分支
   -ad 忽略debug指令
   -ah 包含高级语言的源代码
   -al 包含汇编代码
   -am 包含宏展开
   -an 包含格式处理
   -as 包含符号
   =file 设置文件列表名
  你可以组合使用这些选项。比如说，可以使用‘-aln’可以不启用格式处理来汇编一系列文件。如果使用了‘=file’这个选项，就必须保证这个选项是参数的最后一个。另外，‘-a’选项默认等于‘-alhs’选项。

-D 这个选项会直接被忽略。这个选项只是为了在脚本中使用汇编器的时候可以兼容其他汇编器的语法。

--defsym sym=value
  在汇编器读入源文件之前，将符号sym的值设置成value。其中，value的值必须是一个整数常量。在C语言中，通常通过0x打头来表示一个十六进制数值，以O打头来表示一个八进制数值。

-f
  就是“fast”的意思。跳过空白字符和注释的预处理过程（通常编译器输出的汇编文件会用这个选项）。

--gstabs
  为汇编代码的每一行生成stabs的debug信息。在debugger支持的情况下，这些信息对于debug可能有点帮助。

--gdwarf2
  为汇编代码的没一行生成DWARF2调试信息。在debugger支持的情况下，这些信息对于debug可能有点帮助。不过要注意，这些信息并不是所有平台都支持，只支持一部分。

--help
  打印一个简略的命令行选项帮助信息。

--target-help
  打印一些声明的目标平台的选项的帮助信息。

-I dir
  将目录dir加入到.include指令的搜索列表中

-J
  忽略带符号数计算时的溢出错误

-K
  Issue warnings when difference tables altered for long displacements.

-L
  --keep-locals
    在符号表中保留本地符号。在传统的输出a.out的系统上，这些符号都以-L打头，当然不同的平台会有不同的local变量前缀方法。
  --listing-lhs-width=number
    设置输出数据的列最多可以有number个字长
  --listing-lhs-width2=number
    设置输出数据的列最多可以有number个字长(允许通过\换行)
  
-o objfile
  指定输出的目标文件的文件名

-R
  将data段打包进text段

--statistics
  统计下汇编器最大使用了多少空间（字节为单位）、最长使用了多长时间（秒为单位）
  
--strip-local-absolute
  从outgoing符号表中去除纯local变量的符号

-V/-version/--version
  打印as的版本

-W
  --no-warn
    隐瞒warn消息
  --fatal-warnings
    将warning信息当作错误信息
  --warn
    不要隐瞒warning信息或者将这些信息当作错误信息

-w
  忽略

-x
  忽略

-Z
  即便是有error信息也生成一个object文件

-- | files ...
  使用标准输入，或者说文件当作汇编器的输入。


GNU as实际上一个汇编器家族。如果你在某一个计算机架构下使用（或者已经用过）GNU汇编器，那你在其他计算机架构下应该对这套环境很熟悉。每个架构下的版本的大体功能是一样的，包括obj文件格式、大多数的汇编器命令（通常也会被称作伪指令）以及汇编语法。

as最早是用来给GCC编译器的输出文件作汇编，然后将结果提供给链接器ld使用的。但是不管怎样，我们都致力于使as能够正确的汇编其他汇编器能够编译的所有东西。任何意外情况都会在文档中显式说明。这并不是说as通常会使用和其他汇编器一样的语法（在同一架构下）； 比如说，我们现在都指导680x0系列的语法存在好几个不兼容的版本。

跟其他更老一些的汇编器不同，as的设计目标是用来在源代码文件的编译过程中的一个步骤来应用的，这对它的.org命令有一些细微的影响。

## obj文件格式
GNU汇编器可以自己配置来产生几种不同类型的obj文件格式。大多数情况下，这个并不会影响你怎么写你的汇编源代码； 但是在不同的obj文件格式下一些命令和debug符号会有不同。

# 语法

这章介绍下和机器架构无关的汇编语法。as的语法和其他的一些汇编器的语法差不太多。这个汇编器是受BSD 4.2的汇编器启发开发的，和BSD 4.2的汇编器差不太多，除了as不会编译Vax这个位域。

## 预处理
as的内置预处理器：
- 调整和移除多与的空白字符。它会在每行的每个关键字前留下一个空格或tab，然后把这行其他的连起来的空格变成一个空格
- 移除所有的注释，将这些注释替换成一个空格，或者是换成空白的新行
- 将字符常量回程对应的数字值

预处理器并不会像C语言的预处理器那样做宏展开、或者include文件。你可以使用.include命令来作文件引入。你可以使用GCC编译器来获取“CPP”风格的预处理器功能---只需要将文件名以.S结尾就行。

不能在代码的正文中使用太多的空格、tab、注释或者字符常量。

如果源码文件的第一行是 #NO_APP 或者如果在命令行中使用-f选项，那么空白和注释不会从源码文件中移除。你也可以在源码文件想移除空格和注释的地方手动写上#APP来手动移除。

## 符号
符号由字符、数字和‘_.$’三个特殊字符组成。大多数的架构下，你可以在符号名中使用$；数字符号可以以数字开头。区分大小写。符号没有长度限制：所有的字符都有意义。符号以单词作为分隔。

## 表达式
表达式以新行或者行分隔符结尾。行分隔符通常是分号，除非与注释符号相冲突的情况。新行和行分隔符是表达式的一部分。字符常量中的新行或者分隔符会被视作一场。

表达式不会以文件末尾作为结尾： 任何源代码的末尾必须是一个新行。

以0或者其他的标识符开始的表达式，可以自我声明自己是一个什么表达式。这些标识符会决定剩余表达式的语法。如果标识符以小数点.开始，则说明这个表达式是一个汇编命令：通常这些命令在其他计算机上也是适用的。如果这个标识符是以一个字母开始的，那通常是一个汇编语言的指令：是会汇编成机器语言的指令。不同计算机的不同版本的as汇编器可以识别出不同的指令集。事实上，有可能同一个表达式在不同的机器上会代表不同的指令。

标签通常是一个符号紧接着一个冒号。标签前面的空格和冒号后面的空格是允许的，但是标签和冒号间的空格不行。

## 常量
常量是一个数字，直接写出来这样它的值不会随上下文变化。比如下面这些都是常量： 
.byte 74, 0112, 092, 0x4A, 0X4a, 'J, '\J
.ascii "Ring the bell\7"
.octa 0x123456789abcdef
.float 0f-31415926394058E-4


## 字符常量
字符常量有两种：一种就是代表一个字符的数字值； 另一种是字符串字面量。

#### 字符串


# 段(section)和重定位（relocation）
*英文中的很多词汇在中文中很难找到一个非常准确的词来与之对应。可能因为中文中词语表意丰富的原因，一个词对应英文中多个词的情况比比皆是，在专业词汇中，这种情况尤为常见。“段”在这里也面临这种情况。通常“段”这个词汇可以有两个英文词汇与之对应：section和segment。这两个英文词汇如此常用，为了避免混淆，下面在翻译的时候直接略过对这两个词的翻译，直接使用英文来描述*

简单粗暴的来说，一个section就是描述的一个连续的地址范围； 在这个地址范围内的所有数据都是同样一种用途的。比如说，我们常说的“read only” section。

链接器ld会读入很多obj文件（这些文件都是一些程序片段），然后将他们的内容组织成一个可以执行的程序。当汇编器as产生一个obj文件，这个‘程序片段’（也就是obj文件）都是以地址0开始的。ld链接器会为这些程序片段分配最终的地址，也就是这样，这些一个个的程序片段才不会出现地址交错的情况。当然以上一个相当简化的过程描述，但是可以很粗暴的解释as是如何使用section的。

ld链接器将你的程序的字节块移动到它们的运行时的地址上去。这些字节块是以一个严格的整体拷贝到他们的运行时地址上去的；这些块的长度不会改变，然后块内的字节的顺序也不会改变。这样一个严格的整体称为section。给section分配运行时地址的过程称为重定位（relocation）。这个过程包括将obj文件的地址调整到他们对应的运行时地址上去。

as汇编器产生的obj文件至少包含3个section，这3个section的任何一个都有可能为空。这3个section是.text，.data和.bss section

当生成COFF或者ELF格式的输出文件时，as也可以生成各种各样其他名字的section，你可以通过.section命令来自己命令这些section。如果你不使用任何命令来指明.text和.data section，这些section依然会存在，但是会是空的。

在obj文件内部，.text section以地址0开始，紧接着是.data setion，然后是.bss section。

为了让ld链接器知道section重定位的时候要改哪些东西以及怎么改这些东西，as汇编器得在obj文件中写一些重定位所需要的细节信息。为了让ld能完成重定向，obj文件中的每一处地址引用（reference）都得提到以下信息：
 - 这个reference指向的是obj文件从开头开始的偏移多少？
 - 这个reference指向多长的空间（以字节为单位）？
 - 这个reference指向的是哪个section？这个地址的数值值是多少？ 
 - 这个reference是不是指向一个“程序计数器相关”的地址？


事实上，as中使用的每一个地址都可以用这个公式表述：
  （section）+ (offset into section)
展开来说的话，每一个as处理的表达式都有和它所在的secton相关的特性。

在下文中，我们使用{secname N}这种描述方法来表述『section secname中N偏移量』这样一个概念

除了我们提到的.text, .data和.bss 这几个section，还有一个你得了解的sectoin是absolute section。当ld链接器在处理多个程序片段的时候，absolution section的地址是要求保留不动的。举个例子，地址{absolute 0}会被链接器重定位到程序运行时地址的0地址处。尽管链接器不会在链接多个程序片段的时候让两个section的地址区域重叠，但是你在两个程序片段中定义了重叠的absolute sectioin的时候它们就必须要重叠了。一个程序片段中的地址{absolute 239}通常也是链接完成后的程序中的{absolute 239}位置，对于其他参与链接过程的程序片段来说他们看到的{absolute 239}也是这个。

既然有section就会有对应的undefined section。任何在汇编时无法确定的地址都会被汇编成{undefined U} --- 这里的U后面会填上的。由于数字常量肯定都是确定值的，因此唯一能够生成undefined地址的方法是声明一个undefined符号。一个指令已经确定的公共代码块的地址引用通常就是一个undefinded符号： 在编译期间它的值是无法确定的，因此这个地址引用会被放在undefined section中。

同样地，类比一下，在链接后的程序中，一组section我们也用section这个词来描述。ld链接器会将所有参与链接的程序片段中的所有.text section组成一个连续的section，这个连续的section一样的被称为这个程序的.text section，它包含了所有程序片段的.text section的所有地址。.data和.bss是一个道理。

有一些section是ld链接器掌控和管理的；还有一些section纯粹是as汇编器在编译时候用的，没什么其他作用。



## 链接器section


