---
title: using as
date: 2021-06-29 00:05:24
tags:
---
[TOC]
- [Using as(The GNU Assembly)](#using-asthe-gnu-assembly)
- [概要](#概要)
  - [obj文件格式](#obj文件格式)
- [语法](#语法)
  - [预处理](#预处理)
  - [符号](#符号)
  - [表达式](#表达式)
  - [常量](#常量)
  - [字符常量](#字符常量)
      - [字符串](#字符串)
- [段(section)和重定位（relocation）](#段section和重定位relocation)
  - [链接器专属section](#链接器专属section)
  - [汇编器专属section](#汇编器专属section)
  - [sub-sections](#sub-sections)
  - [bss section](#bss-section)
- [符号（Symbols）](#符号symbols)
  - [标号（labels）](#标号labels)
  - [给符号赋其他值](#给符号赋其他值)
  - [符号名](#符号名)
  - [特殊符号Dot（the special dot symbol）](#特殊符号dotthe-special-dot-symbol)
  - [符号属性](#符号属性)
    - [value属性](#value属性)
    - [type属性](#type属性)
    - [a.out的符号属性](#aout的符号属性)
      - [descriptor](#descriptor)
      - [其他](#其他)
    - [COFF的符号属性](#coff的符号属性)
      - [primary属性](#primary属性)
      - [辅助性属性](#辅助性属性)
- [表达式](#表达式-1)
  - [空表达式](#空表达式)
  - [整数表达式](#整数表达式)
    - [参数(argument)](#参数argument)
    - [操作符](#操作符)
    - [前缀操作符](#前缀操作符)
    - [中缀操作符](#中缀操作符)
- [汇编器命令](#汇编器命令)
  - [.abort](#abort)
  - [.ABORT](#abort-1)
  - [.align abs-expr, abs-expr, abs-expr](#align-abs-expr-abs-expr-abs-expr)
  - [.ascii "string"...](#ascii-string)
  - [.asciz "string"...](#asciz-string)
  - [.balign[wl] abs-expr abs-expr, abs-expr](#balignwl-abs-expr-abs-expr-abs-expr)
  - [.byte expressions](#byte-expressions)
  - [.comm symbolX, lengthX](#comm-symbolx-lengthx)
  - [.data subsectionX](#data-subsectionx)
  - [.def nameX](#def-namex)
  - [.desc symbolX, abs-expression](#desc-symbolx-abs-expression)
  - [.dim](#dim)
  - [.double flonums](#double-flonums)
  - [.eject](#eject)
  - [.else](#else)
  - [.elseif](#elseif)
  - [.end](#end)
  - [.endef](#endef)
  - [.endfunc](#endfunc)
  - [.endif](#endif)
  - [.equ symbolX, expression](#equ-symbolx-expression)
  - [.equiv symbolX, expression](#equiv-symbolx-expression)
  - [.err](#err)
  - [.exitm](#exitm)
  - [.extern](#extern)
  - [.fail expression](#fail-expression)
  - [.file stringX](#file-stringx)
  - [.fill repeat, size, value](#fill-repeat-size-value)
  - [.float flonums](#float-flonums)
  - [.func name[, label]](#func-name-label)
  - [.global symbolX / .globl symbolX](#global-symbolx--globl-symbolx)
  - [.hidden names](#hidden-names)
  - [.hword expressions](#hword-expressions)
  - [.ident](#ident)
  - [.if absolute expression](#if-absolute-expression)
# Using as(The GNU Assembly)

By Dean Elsner, Jay Fenlason & friends- [Using as(The GNU Assembly)](#using-asthe-gnu-assembly)
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



## 链接器专属section
ld只会处理4中类型的专属section，他们大致介绍如下：
 - text section/data section/named section:  这3个section和起来保存了你的程序。as和ld会区分这几个section，但是它们的性质几乎是一样的。你用来描述这3个中任何一个section的概念都适用于另外两个。不过呢，通常在程序运行的时候，text section的内容是不能更改的。一般进程之间会共享同样一份text section内容：这个section里面包含了程序对应的指令、常量等等。data section通常在程序运行的时候内容是可以更改的。比如说，C语言的变量就会放在data seection中。

 - bss section：这个section包含的内容是程序启动的时候用0填充的字节内容。一般bss section是用来存储未初始化的变量或者全局共享的存储。每个程序片段的bss section的长度是一个重要的指标，但是呢，因为这个section的内容全是0填充的字节，因此一般在obj文件中并不会显式地去存储等量的0字节。bss section被发明出来就是为了在obj文件中去除这些显式的0字节。
 - absolute section： 这个section内的地址0通常在重定位之后也会是程序的地址0.这个性质在你想被ld链接之后地址也不发生改变的时候很有用。也就是说，absolute section内的地址是‘不可重定位’的：这个section里面的地址值在重定位之后也不会发生改变。
 - undefined section： 这个section是一个兜底方案，用来记录那些没法在上述几个section填入的地址引用。

一个链接过程的简单示意：

////     程序片段#1                                     \
////     text             data             bss         \
////     |-----------------------------------------    \
////     |      ttttt    |     dddd       |   00  |    \
////     ------------------------------------------    \
////     程序片段#2                                     \
////     text             data            bss          \
////     |---------------------------|                 \
////     |  TTT  |    DDDD  |  000   |                 \
////     -----------------------------                 \
////     链接后的程序                                    \
////          text                        data                bss           \
////     |----|------|--------------|----|----------|--------|---------|    \
////     |    | TTT  |   ttttt      |    |  dddd    | DDDD   | 00000   |    \
////     ---------------------------------------------------------------    \


## 汇编器专属section
这些section都是汇编器as内部使用的，因此在运行时毫无意义。通常你不需要了解这些sectioin到底是做什么用的； 不过这些section有可能会在as汇编过程的warning提示中出现，所以如果你知道这些section对as汇编器有什么作用的话还是挺有帮助的。这些section让你可以将汇编代码程序中的每个表达式的值对应到一个‘section-relative’地址。

ASSEMBLER-INTERNAL-LOGIC-ERROR!   ：  这代表一个一直的汇编器内部的逻辑错误，也就是说，这个as汇编器目前确实有这个bug。

 - expr sectoin： 汇编器会将一些复杂的表达式以一组符合的组合存储在这里。当某个符号确实需要对应一个表达式的时候，它会被放在这个section中。

## sub-sections
汇编后生成的文件中的内容一般会以字节为单位，大部分的内容在.text和.data section。你可能会有这样的需求：将一组单独的section在obj文件末尾一个接一个排开，并且它们在汇编源码文件中可能不是连这些的。as汇编器提供了subsection来满足这个需求。在每个section内，可以使用0-8192来标识subsection。同一个subsection内的内容会被汇编到同一个obj文件中。举个例子，编译器可能会将常量放入text section，但是可能会不希望它们和被汇编的程序分散开。在这种情况下，编译器会声明一个‘.text 0’来标识每个源码中的代码程序，然后用‘.text 1’来标识每个源码中的常量部分。

subsection是一个可选的功能。如果你不使用subsection，那默认所有的东西都是放在subsection 0中。

subsection按数字从小到大顺序出现在你的obj文件中。obj文件中不包含subsection的任何表征信息，ld以及其他任何操作obj文件的程序也看不到subsection的痕迹。它们只将你所有的text subsection当作一个text section，将你所有的data subsection当作一个data section。

为了区分你接下来的语句要被汇编到哪个subsection，你可以使用一个数字参数来显式地声明，语法是‘.text expression’或者'.data expression'（expression是subsection的编号，或者一个常量表达式）。在生成COFF或者ELF文件的时候，你也可以用语法‘.section name, expression’来声明一个带名字的subsection。如果你只写一个‘.text’，那么和'.text 0'是一个意思。同样的道理，‘.data’和‘.data 0’是一个意思。汇编后的程序都是以.text 0开始。举个例子：

.text 0 # The default subsection is text 0 anyway.
.ascii "This lives in the first text subsection. *"
.text 1
.ascii "But this lives in the second text subsection."
.data 0
.ascii "This lives in the data section,"
.ascii "in the first data subsection."
.text 0
.ascii "This lives in the first text section,"
.ascii "immediately following the asterisk (*)."

每个section都有一个位置计数器(location counter)，它会以字节为单位一个个往上累加，这个section汇编进取的内容每多一个就加一。由于subsection的概念几乎是as汇编器使用范围内的优化，因此并没有一个‘subsectin位置计数器’的概念。也就是说没有方法直接去操作一个位置计数器。但是呢，有一个汇编命令.align可以做这个事情，并且汇编语言中的任何一个自定义的标签都可以捕获它当前的位置计数器。这种正在被汇编的section的位置计数器被称作active位置计数器。

## bss section
bss section用来作本地变量的存储。你可以在bss section中分配地址空间，但是你一般不能在程序跑起来之前往这个section中塞入数据。当你的程序跑起来之后，bss section的内容会被用0字节填充。

.lcomm汇编命令可以在bss section定义符合。（后面会讲）

.comm汇编命令可以用来定义另一种形式的为初始化符号。（后面会讲）

在汇编像ELF和COFF这样的支持多个section的目标文件时，你也可以正常的切换到bss section去定义符号。但是一般你只能在这个section中填充0字节值。典型的使用方法是这个section中只包含符号定义或者使用.skip汇编命令。


# 符号（Symbols）
符号是是一个非常核心的概念：程序员会使用符号来给东西命名，链接器使用符号来链接，调试器使用符号来调试。

注意！！： as汇编器并不一定会按照符号定义的顺序将这些符号放入到obj文件中。因为这样做可能会使一些调试器没法正常工作。

## 标号（labels）
标号的写法是在一个符号后面直接加一个冒号（：）。这种符号就可以表示active位置计数器的当前值（比如说一个对应的指令操作符）。如果你在两个不同的地方使用了同一个标号，汇编器会告警：第一个定义的标号被后面的其他定义给覆盖了。

## 给符号赋其他值
一个符号可以通过在后面添加一个等号来赋任意值。这个和使用.set汇编命令是等价的。

## 符号名
符号名一般以字母或者‘._’两个字符中的一个打头。在大多数的机器上，你也可以在符号名中使用‘$’字符。这些打头的字符后面可以跟任何数字、字母、$、_等组成的字符串。

字母是区分大小写的：foo和Foo是两种不同的符号名。

每个符号都有一个与与之对应的独一无二的名字。汇编程序中的每个名字都指向一个与之对应的唯一的符号。你可以在程序中无次数限制地使用任何一个符号名。

本地符号名（Local Symbol names）

本地符号可以让汇编器和编程者临时地使用符号名。汇编器和编程者在一定源代码范围内创建一些唯一的符号。定义本地符号的方法是“N：”（这里N代表一个正整数）。可以使用“Nb”来指代最近的上一个定义的本地符号，这里的N是你想指向的本地符号。同样的，你可以使用“Nf”来指代这一行之后的下一个本地符号N。这里面，“b”代表“backwards”，即向后查找； “f”代表“forwards”，即向前查找。

至于你如何使用这些标号，实际上并没有什么严格的限制。也就是说，有可能会重复地定同一个本地符号（即两个地方使用同一个“N”）。尽管如此，你使用“Nb”或者“Nf”的时候也只可能指向一个唯一的向后查找或者向前查找的本地符号。此外，值的注意的是，前十个本地标号（0:, 1:, ...., 9:）相对于其他的本地标号是以一种略微优化了的方式实现的。

举个例子：
1: branch 1f

2: branch 1b

1: branch 2f

2: branch 1b

上面的语句等价于：
label_1: branch label_3

label_2: branch label_1

label_3: branch label_4

label_4: branch label_3

本地符号名仅仅是一个标记的工具。它们在被汇编器使用之前会直接转化成更方便使用的符合名。符号名存储在符号表中，在汇编错误时的错误信息里会出现，也有可能会记在obj文件中。这些转化后的符号名会由下面的部分组成：

L：所有的本地符号名都以‘L’打头。正常情况下as汇编器和ld链接器都会跳过对以”L“打头的符号的处理。这些标号都是用来标识那些你永远都不想看到的符号。如果你在汇编的时候使用”-L“选项，那边汇编器会在obj文件中保留这些符号。如果你以同样的方式要求ld链接器也保留这些符号，那么你在debug的时候还可以看到和使用它们。

number： 这里的number是说你在定义本的符号的时候使用的数字。因此，如果你的标号写的是”55：“，那么这里的number指的就是55

C-B：这个特殊的字符也会被包含在转化后的符号名中，这样可以保证你不会无意间在代码中写出一个转化后的符号名。（这个C-B其实指的是“control-B”,即control键+B输入的字符，它的asscii码是\002）

ordinal number: 这个是一个数字序列，用来确保转化后的符号名是唯一的。第一个定义的符号名，转化后这个数字是1，第15个定义的符号名转化后这个数字是15，依次往下编。

举个例子，我们自定义的本地符号名“1：”，在转化后会得到符号名“L1C-B1”。我们定义的第44个符号名“3：”在转化后会得到符号“L3C-B44”

$本地标号(dollar local labels)

as也支持一种比本地标号更加‘本地’的标号形式，称为dollar labels。一旦定义了一个非本地标号，这些$本地标号就会在作用范围内立马实效（比如它们变成undefined）。因此它们只会在源码的一小段区域内保持可用。与此相反，正常的本地标号，会在源码文件中的整个文件内保持可用，或者是有其他地方定义了一个同样名字的正常本地标号，这个正常的本地标号才会失效。

dollar标号的定义和正常的本地标号是一样的方式定义，不过dollar标号的末尾不是一个冒号“：”，而是一个dollar符。比如这样的：“55$”。

还有一种区分普通的本地标号和dollar本地标号的方法是：dollar本地标号的转化后的标号中使用的是ascii字符‘\001’(也就是contorl-A)。因此第5个dollar本地标号‘6$’会被转化成'L6C-A5'。

## 特殊符号Dot（the special dot symbol）
特殊符号‘.’指代as在作汇编时当前行的地址。因此，表达式‘melvin: .long .’定义了一个符号‘melvin’来指代这个符号自己的地址。给‘.’符号赋值等价于‘.org’汇编命令。因此，表达式'.=.+4'等价于‘.space 4’

## 符号属性
每个符号除了名字外，还有‘value’和‘type’两个属性。符号还可以有一些额外的辅助属性，这取决于输出的格式。

如果直接使用一个为定义的符合，as汇编器会默认这个符号的所有属性都是0，而且这种情况下是有可能并不会通知你的。这种情况下as汇编器会把这个符号当作是一个外部定义的符号。

### value属性
一个符号的值通常都是32位的。对于一个在text、data、bss或者absolute section内标识位置的符号来说，它的值等于这个符号从section开始的相对地址。自然而然的，text、、data、bss sectoin内的符号的值会在ld链接器拦截的时候被改变，因为链接器会改这些section的基址地址。absolute section内的符号的值不会在链接的时候发生改变： 这也是它们被称为absolute的原因。

一个未定义的符号的value会以一种特殊的方式处理。如果value是0，这就说明在汇编的源代码中这个符号没有被定义，ld链接器在将这个符号链接至目标程序的时候尝试决定这个符号的value。这种用法很简单，你只需要简单的在不定义一个符号的情况下使用这个符号就行，ld链接器会自己去找。如果一个符号的value属性不为0，说明这个符号是使用.comm的方式声明过，这里的value值说明了这个符号需要预留多少字节的存储空间。而符号则指代这片存储区域的开始地址。

### type属性
一个符号的type属性包含了这个符号的重定位/section信息、以及一系列说明这个符号是否是外部声明的，以及其他一些链接器和调试器需要使用的信息。这些信息的确切格式与输出的obj文件格式相关。

### a.out的符号属性

#### descriptor
这个属性是一个任意的16bit的值。你可以自己使用.desc汇编器命令来声明一个符号的descriptor。不过呢，as汇编器对于descriptor的值并不关系，as不会使用这个值。

#### 其他
这是一个任意的8bit的值。同样的，as汇编器并不关系这个东西。

### COFF的符号属性
COFF格式支持大批的辅助性符号属性； 像primary符号属性等。这些属性是通过.def/.defend汇编命令设置的。

#### primary属性
使用.def设置符号名； 对应的，符号的value和type通过.val和.type命令定义

#### 辅助性属性
as汇编器命令.dim, .line, .scl, .size和.tag可以生成COFF文件的辅助性符号表信息。

# 表达式
表达式指代了一个地址或者一个数值。表达式的前后都可以跟空白字符。

表达式的运算结果必须是一个确定的数值、或者是一个section中的地址偏移量。如果一个表达式不是绝对确定的值，并且当as汇编器看到这个表达式的时候没有足够的信息来确定它的section，那么就需要第二轮重新扫一遍程序来解释这个表达式了。不过呢，这个第二轮的扫描as汇编器目前并没有实现，因此as汇编器会直接抛一个错误信息，然后结束这次汇编。

## 空表达式
一个空的表达式没有值：仅仅是空白符号或者null。任何需要一个确定表达式的定方，你都可以用空表达式，并且as汇编器会将空表达式解析成数值0.这一点和其他的汇编器是兼容的。

## 整数表达式
一个整数表达式是由操作符界定的一个或者多个参数。

### 参数(argument)
参数可以是符号(1)、数字(2)或者子表达式(3)。在一些其他的上下文中‘参数’有时会被称为‘arithmetic operands(算术操作数)’。在这个手册中，为了避免和机器语言的‘指令操作数’混淆，我们使用了‘argument’这个词来专门代表一部分表达式，保留了‘operand’这个词来代表及其指令的操作数。

符号(1)在计算和估值之后会产生{section NNN}这样的表达式，这里的section可以是text, data, bss, absolute和undefined中的一个，NNN是一个有符号的，补码形式的32位整数

数字(2)通常都是整数。

一个数值(2)可以是浮点型或者bignum大整数。在这种场景中，汇编器会提示你这个数值内容只有低32位会被使用，而且as汇编器会将这低32位当作一个整数。如果要和外部的常量运算，或者保持和其他汇编器的兼容性，你可能得自己写一个整数操作的指令。

子表达式（3）一般是一个左括号'('接上一个整数表达式，然后接上一个右括号')'； 或者是一个前缀操作符加上另一个参数（argument）

### 操作符
操作符是一种算术运算函数，像 + 或者 % 等等。前缀操作符后面都会跟一个参数。中缀操作符两边都有参数。操作符左右两边都可以有空白字符。

### 前缀操作符
as汇编器有下面几种前缀操作符。它们都只带一个参数，而且参数必须是常量。
 - ‘-’： 负号
 - ‘～’： 非

### 中缀操作符
中缀操作符有两个参数，一边一个。操作符有优先级区分，但是相同优先级的操作是从左至右计算的。除了 + 和 - ， 每个参数都必须是常量，并且结果也会是一个常量
1. 最高优先级
 - '*': 数乘
 - '/': 数除
 - '%'： 取余
 - '<<'， '<': 左移
 - '>>', '>': 右移
2. 中优先级
 - '|': 按位或
 - '&': 按位与
 - '^': 按位异或
 - '!': 按位或非
3. 低优先级
  - '+': 数字加。如果有一个参数(A)是常量，另一个(B)不是，那结果的section会是B的section。如果两个参数属于不同的section，你不能对它们使用加法。
  - ‘-’： 数字减。如果右参数是常量，那结果的section会是左参数的section。如果两个参数来自同一个section，那结果会是一个常量。如果两个参数属于不同的section，你不能对它们使用减法。
  - ‘==’：判断两个参数是否相等。
  - '<>': 判断两个参数是否不相等。
  - ‘<': 小于
  - '>'
  - '>='
  - '<='
  上面这些比较类的操作符可以当作中缀操作符使用。如果比较的结果是真，那么会返回值-1； 如果比较的结果是假，那么返回值是0。这些比较操作都是带符号的比较。

4. 最低优先级
  - '&&': 逻辑与
  - '||': 逻辑或
  这两个逻辑操作符都可以用来组合自表达式的结果。需要注意的是，与比较类操作符不同，这里如果结果是真，那么返回1；如果结果是假，那么返回0。另外需要说明的是逻辑或的优先级比逻辑与要低。

  总结一下，只有对地址的offset作加法和减法有意义；你只能在一个参数中确定section，或者两个参数来自同一个section。

# 汇编器命令
所有的汇编器命令都是以点.开头。剩下的名字部分是一个单词，通常都是小写的。

这一章我们会讨论GNU汇编器内置的与目标机器架构无关的汇编器命令。有一些机器会提供其他额外的汇编器命令，这个我们在下一章讲。

## .abort
这个命令会立即停掉汇编程序。这个命令和其他汇编器是兼容的。最原始的设计想法是，汇编的源码是通过类似与管道的方式流入到汇编器中，然后如果发送方（也就是源码）想停下来，就用这个命令来告诉as汇编器一起停下来。现在在逐步考虑废弃.abort命令了。

## .ABORT
如果是在汇编COFF文件，as汇编器会将它当作.abort命令
如果是在汇编b.out文件，as汇编器会直接忽略这个命令。

## .align abs-expr, abs-expr, abs-expr
这个命令将位置计数器（location counter）限定在一个特定的存储空间范围内。参数中的第一个个表达式必须是一个常量，它声明了对齐要求，下面还会介绍。

参数的第二个表达式也得是一个常量，它给出了在限定的存储范围内的填充值。这个参数可以被省略。如果省略的花，那填充的字节一般是0.不过呢，在一些系统里面，如果这个section内有代码内容在的话，那你给出的填充值是会被忽略的，它们会使用no-op指令来填充这些个空间。


参数的第三个表达式也得是一个常量，并且也是可选的。如果写了第三个参数的话，那它声明了这条内存对齐命令可以跳过的最大字节数。如果在作对齐的时候发现需要跳过的字节数大于这个参数声明的值，那这次对齐压根就不会触发。你可以用两个逗号',,'来省略第二个参数而不省略第三个参数。

内存对齐的方式依着各种各样的系统有着各种各样的方式。ℹ386系统使用的是ELF格式的，这种格式的第一个表达式声明了按多少字节对齐。举个例子，命令'.align 8'会让location counter往前走，一直到当前值是一个8的倍数。如果location counter当前值已经是8的倍数，那么什么都不会发生。

在其他的一些系统里，比如i386的a.out格式，以及arm架构等，第一个参数声明了location counter的低多少位需要是0。举个例子，‘.align 3’声明了location counter的低3位必须是0，也就是说，location counter会一直往前走，直到location counter是8的倍数（这样能保证低3位是0）。如果当前location counter的低3位已经是0了，那么什么都不会发生。

这种不一致性是因为这些系统下面自带的汇编器本身的行为就不一样，as汇编器为了模拟它们的行为只能这么做。as汇编器同时还提供了.balign和.p2align命令，这两个命令在不同的架构下面行为是一致的，这个我们后面还会有介绍。

## .ascii "string"...
.ascii命令后面可以接0或者多个字符串字面量，以逗号分隔。汇编器会将这些字符串汇编到连续的地址内，需要注意的是，汇编器不会自动地给这些字符串末尾加0.

## .asciz "string"...
.asciz和.ascii差不多，但是汇编器会给每个字符串后面加0。.asciz里面的z就是'zero'0的意思。

## .balign[wl] abs-expr abs-expr, abs-expr
将位置计数器(location counter)限定在一定的存储范围内。第一个表达式必须是常量，它声明了需要按多少字节对齐。举个例子，‘.balign 8’会让location counter一直往前走，直到它的值是8的倍数。如果它的值已经是8的倍数了，那么什么都不会发生。

第二个参数也必须是一个常量，它给出了内存对齐时需要用什么内容来填充字节。这个参数可以忽略，如果忽略的话，填充的字节通常是0。但是在一些系统中，如果当前section内有代码，那么这个参数给出的填充内容会直接被忽略，然后使用no-op指令来填充。

第三个参数也得是一个常量，它也是可选的。如果给了这个参数的话，那它描述了这个对齐命令允许跳过的最大字节数。如果对齐过程中跳过的字节数大于了这个给出的参数值，那此次对齐压根就不会做。你可以使用两个逗号‘,,’来忽略第二个参数而使用第三个参数，这种用法在你想要使用no-op指令来填充内存对齐产生的空档的时候很有用。

.balignw 和 .balignl命令是.balign命令的两个衍生。.balignw会按照两字节的模式对齐。.balignl命令按照4字节的模式对齐。举个例子， '.balignw 4, 0x368d'会按照4的倍数进行对齐。如果对齐过程中跳过了2个字节，那这两个字节会使用0x368d进行填充。如果跳过了1个或者3个字节，那就不会用0x368d来填充。

## .byte expressions
.byte命令可以带0个或者多个表达式作为参数，参数之间通过逗号作分隔。每个表达式都会被汇编到下一个字节中去。

## .comm symbolX, lengthX
.comm定义了一个通用的符号，符号名为symbolX。在链接的时候，一个obj文件内的通用符号可能会和在另一个obj文件中定义的通用符号合并。如果ld链接器没有找到这个符号的定义，那么链接器会给这个符号分配lengthX个字节的未初始化内存。lenghtX必须是一个常量表达式。如果ld链接器看到多个同名的符号，并且这些符号没有统一的大小，那链接器会按照声明的最大大小来分配空间。

在ELF文件中，.comm命令可以有第三个参数，它是一个可选参数。这个参数声明了这个符号的按多少字节对齐。这个对齐参数必须是一个常量表达式，并且必须是2的直属。如果ld链接器为这个符号分配了未初始化内存，那ld链接器会考虑在放置这个符号的时候按声明的方式进行对齐。如果没有声明对齐方式，as汇编器会将对齐方式设置成小于等于这个符号的大小的最大的2的指数值，这个值最大是16.

## .data subsectionX
.data命令告诉as汇编器将下面的语句汇编到 .data subsectionX这个subsection的空间后面。如果subsectionX被忽略了，那这个值默认是0.

## .def nameX
从这里开始为nameX这个符合定义调试信息。这个定义一直有效知道.endef命令出现。

这个命令只有在输出COFF文件的时候有用。当输出b.out文件的时候，‘.def’命令会被识别，但是会直接被忽略。

## .desc symbolX, abs-expression
这个命令设置了一个符号的descriptor属性，第二个参数常量表达式的值会被设置到descriptor的低16位。

.desc这个命令在as输出COFF格式文件的时候是无效的。只有在输出a.out和b.out obj文件的时候有用。

## .dim
这个命令只能是编译器生成出来的，它是用来在符号表中设置一些辅助性的调试信息的。这个命令只能在.def/.endef命令包含的区域内使用。

.dim只有在生成COFF格式文件的时候有意义。在as生成b.out文件的时候，as可以识别这个命令，但是会直接忽略。

## .double flonums
.double命令可以接受0个或者多个浮点数作为参数，各个参数通过逗号隔开。它会将浮点数一个接一个地汇编到目标文件中。

## .eject
强制在当前点产生一个‘分页符’(page break)。

## .else
as汇编器支持的条件汇编命令的一部分。

## .elseif
as汇编器支持的条件汇编命令的一部分。

## .end
.end命令标识了这个汇编文件的末尾。as汇编器不会处理.end命令后面的任何内容。

## .endef
这个命令标识了以.def命令开始的符号定义范围的结束。

这个命令只有在生成COFF格式文件的时候有意义。在as生成b.out文件的时候，as可以识别这个命令，但是会直接忽略。

## .endfunc
.endfunc命令标识了.fuc命令提示的函数定义范围的结束。

## .endif
as汇编器支持的条件汇编命令的一部分。

## .equ symbolX, expression
这个命令将symbolX的值设置成表达式代表的值。这个命令和.set命令是同语义的。

## .equiv symbolX, expression
.equiv命令有点像.equ命令和.set命令。不过呢，汇编器如果发现symbolX已经被定义了的话会抛错。需要提醒的是，已经引用过但是没有显示定义的符合同样会被当作未定义看待。

除去抛错这部分的功能，这个命令的语义有点类似于下面的命令组合：
.ifdef SYM
.err
.endif
.equ SYM,VAL

## .err
as汇编器在汇编.err命令是，它会打印出一个错误消息。使用了.err命令会阻止汇编器生成obj文件，除非在汇编的时候声明了‘-Z’选项。这个功能可以用来作代码的条件编译。

## .exitm
从当前的宏定义中提前退出

## .extern
为了兼容其他的汇编器，源码中的.extern命令会被as汇编器识别，但是会被as直接忽略。as汇编器会将所有未定义的符号当作外部符号。

## .fail expression
.fail命令用来产生一个error或者warning。如果参数expression表达式的值大于500，as汇编器会打印一条告警信息。如果expression表达式的值小于500，as汇编器会抛出一条error错误信息。信息中包含有expression表达式的值。这个东西在复杂的嵌套宏或者条件编译中十分有用。

## .file stringX
.file命令告诉as汇编器我们马上要开始一个新的逻辑文件。stringX是这个新的文件的文件名。一般来说，文件名会通过双引号引起来。如果你想声明一个空文件名，你可以直接使用“”这样的方式。这个语法以后可能会被移除掉，所以现在不推荐使用。

## .fill repeat, size, value
这里的三个参数repeat, size和value都必须是常数表达式。这个命令用来重复拷贝repeat次size大小的字节。repeat参数大于等于0.szie参数大于等于0，但是如果size>8，那么会强制size=8，这个也是为了和别的汇编器保持兼容。每块重复的字节块的内容都是一个8字节大小的数字，高位的4个字节是0，低位的4个字节是参数value的值。每个size这么大的字节块是一个重复单元。这种奇怪的行为是也为了和其他的汇编器保持兼容。

size参数和value参数都是可选的。默认情况下，size的值是1，value的值是0.

## .float flonums
这个命令可以汇编0个或者多个浮点数到目标文件中，这些数以逗号隔开。这个命令和.single命令的效果是一样的。

## .func name[, label]
.func命令会触发一段调试信息来指示函数名，因此，这个命令只有在只有在汇编器开启了调试时才有用，否则会被直接忽略。到目前为止，只有as汇编器的--gstabs开启调试选项还支持这个功能。label参数是这个函数的入口。name参数可以省略，或者是一个以下划线开始的单词。目前所有函数返回值类型都是void。函数定义必须以.endfunc来结束。

## .global symbolX / .globl symbolX
.global命令将一个符号暴露给ld链接器。通常情况下，如果你在你的程序片段中定义了一个符号，那么这个符号的值在链接的时候是可以被其他程序片段看到的。不然的话，一个定义的符号就需要从其他程序片段的同名符号中获取属性值了。

两种拼写方式(.global 和 .globl)都是可以的，这个也是为了兼容其他的汇编器。

## .hidden names
这个是ELF格式可见的命令（另外两个EFL格式可见的命令是.internal和.protected，后面会介绍）。

这个命令重新设置了这个names符号的可见性（通常符号的可见性可以被设置成： local, global和weak）。这个命令将符号的可见性设置成hidden，这意味着这个符号不能被其他的模块看到。这种符号通常也会被称为protected符号。

## .hword expressions
这个命令接受0个或多个表达式作为参数，并且每个表达式代表一个16位的数字。

这个命令和'.short'命令是同一个语义。

## .ident
这个命令会被一些汇编器用来给obj文件中放置tag。as汇编器仅仅是为了和其他汇编器保持兼容，实际上as并不会作任何事情。

## .if absolute expression
.if