---
title: using as
date: 2021-06-29 00:05:24
tags:
---
> Translated by zivyou. All rights reserved.
> 由zivyou翻译，保留所有权利。


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
  - [.incbin "file"[, skip[, count]]](#incbin-file-skip-count)
  - [.include "file"](#include-file)
  - [.int expression](#int-expression)
  - [.internal names](#internal-names)
  - [.irp symbolX, values...](#irp-symbolx-values)
  - [.irpc symbolX, values...](#irpc-symbolx-values)
  - [.lcomm symbolX, length](#lcomm-symbolx-length)
  - [.lflags](#lflags)
  - [.line line-number](#line-line-number)
  - [.linkonce [type]](#linkonce-type)
  - [.ln line-number](#ln-line-number)
  - [.mri val](#mri-val)
  - [.list](#list)
  - [.long expression.](#long-expression)
  - [.macro](#macro)
  - [.nolist](#nolist)
  - [.octa bignums](#octa-bignums)
  - [.org new-lc, fill](#org-new-lc-fill)
  - [.p2align[wl] abs-expr, abs-expr, abs-expr](#p2alignwl-abs-expr-abs-expr-abs-expr)
  - [.previous](#previous)
  - [.popsection](#popsection)
  - [.print string](#print-string)
  - [.protected names](#protected-names)
  - [.psize lines, columns](#psize-lines-columns)
  - [.purgem name](#purgem-name)
  - [.pushsection name, subsection](#pushsection-name-subsection)
  - [.quad bignums](#quad-bignums)
  - [.rept count](#rept-count)
  - [.sbttl "subheading"](#sbttl-subheading)
  - [.scl class](#scl-class)
  - [.section name](#section-name)
  - [.set symbol, expression](#set-symbol-expression)
  - [.short expressions](#short-expressions)
  - [.single flonums](#single-flonums)
  - [.size](#size)
  - [.sleb128 expressions](#sleb128-expressions)
  - [.skip size, fill](#skip-size-fill)
  - [.space size, fill](#space-size-fill)
  - [.stabd, .stabn, stabs](#stabd-stabn-stabs)
  - [.string "str"](#string-str)
  - [.struct expression](#struct-expression)
  - [.subsection name](#subsection-name)
  - [.symver](#symver)
  - [.tag structname](#tag-structname)
  - [.text subsection](#text-subsection)
  - [.title "heading"](#title-heading)
  - [.type](#type)
  - [.uleb128 expressions](#uleb128-expressions)
  - [.val addr](#val-addr)
  - [.version "string"](#version-string)
  - [.vtable_entry table, offset](#vtable_entry-table-offset)
  - [.vtable_inherit child, parent](#vtable_inherit-child-parent)
  - [.weak names](#weak-names)
  - [.word expressions](#word-expressions)
- [80386架构相关的特性](#80386架构相关的特性)
  - [命令行选项](#命令行选项)
  - [AT&T汇编语法 vs Intel汇编语法](#att汇编语法-vs-intel汇编语法)
  - [指令命名](#指令命名)
  - [寄存器命名](#寄存器命名)
  - [指令前缀](#指令前缀)
  - [内存寻址](#内存寻址)
  - [跳转类指令的处理](#跳转类指令的处理)
  - [浮点数](#浮点数)
  - [Intel的MMX指令集，以及AMD的 3DNow! SIMD操作](#intel的mmx指令集以及amd的-3dnow-simd操作)
  - [用as写16位的汇编代码](#用as写16位的汇编代码)
  - [AT&T的语法bug](#att的语法bug)
  - [声明CPU架构](#声明cpu架构)
  - [备注](#备注)
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
.if命令以一个开关的形式确定某个section中的一段代码是否最终会汇编到目标程序中，.if命令标识了这段代码的开始，而且只有.if命令的参数（必须是一个常量表达式）的值是非0值的时候，才会将这段代码汇编到目标程序中。这段代码的结束必须通过命令.endif来标识。另外作为可选项，你可以使用.else命令来标识出另外一个选项。如果你有多个条件要检查，你可以使用.elseif命令来避免.if/.else的嵌套。

下面几种.if命令的衍生命令也是支持的：

.ifdef symbolX
  如果给定的参数symbolX被定义过了，那么就汇编这个命令敲定的代码片段。注意，如果一个符号没有被显式地定义过而你直接拿去用了，那这个符号还是会被看作未定义的。

.ifc string1, string2
  如果给定的两个参数，即string1和string2相同，那么就汇编选定的代码片段。两个字符串需要用单引号引起来，如果没有用引号引起来的话，那么逗号前的一个串会被认为是第一个字符串，逗号后的串一直到行的末尾会被认为是第二个字符串。包含空白字符的字符串必须要引起来。字符串的比较是区分大小写的。

.ifeq absolute expression
  如果给定的常量表达式的值是0的话，就汇编选定的代码片段。

.ifeqs string1, string2
  .ifc命令的另一种形式，两个字符串必须通过双引号引起来

.ifge absolute expression
  如果给定的常量表达式的值大于等于0，就汇编选定的代码片段。

.ifgt absolute expression
  如果给定的常量表达式的值大于0，就汇编选定的代码片段。

.ifle absolute expression
  如果给定的常量表达式的值小于等于0，就汇编选定的代码片段。

.iflt absolute expression
  如果给定的常量表达式的值小于0，就汇编选定的代码片段。

.ifnc string1, string2
  和.ifc差不多，不过语义上是反过来的：即如果给定的两个字符串不相同，就汇编选定的代码片段。

.ifndef symbolX
.ifnotdef symbolX
  如果给定的符号没有定义的话，就汇编选定的代码片段。这个命令的两种拼写形式都支持。注意，如果一个符号没有被显式地定义过而你直接拿去用了，那这个符号还是会被看作未定义的。

.ifne abosolute expression
  如果给定的常量表达式的值不等于0，就汇编选定的代码片段（换句话说，这个命令和.if命令是一个意思）。

.ifnes string1, string2
  和.ifeqs差不多，但是语义上反过来了： 即如果给定的两个字符串不同，就汇编选定的代码片段。

## .incbin "file"[, skip[, count]]
  .incbin命令会将指定的文件一字不拉的引入到当前位置来。你可以在as命令的-I参数中指定文件的搜索目录。file参数旁边的双引号是必须的。

  skip参数可以指明在include的时候省略file文件开始的多少字节。count参数指明了最大可以读入多少字节。需要注意的是，读入的数据不会以任何方式对齐，因此使用该命令的人需要自己考虑在include之前和之后的空间对齐问题。
  
## .include "file"
这个命令提供了一种在你的源代码中指定的地方引入一个文件的方式。参数file文件中的代码会当作.include这个点新插入的代码一样被汇编； 当include的文件到达末尾的时候，汇编程序会在调用.include命令的地方继续它的汇编过程。你可以通过as命令的-I选项来控制引入的文件的搜索路径。参数file附近的双引号是必须的。

## .int expression
.int命令可以在任何section中带0个或者多个表达式作为参数，参数以逗号作为分隔。每个表达式的值在运行时都得是一个数字。这个数字的字节序以及大小依赖于汇编的是哪种目标程序。

## .internal names
.internal是一个用来定义ELF文件中变量可见性的命令。其他两个定义可见性的命令是.hidden和.protected命令

这个命令会覆盖掉变量默认的可见性设置（默认的可见性设置是这个变量是local，global还是weak来确定的。这个命令会强制将变量设置位internal这种可见性，即这个变量会被视为hidden状态（不能被其他组件看到），这个符号的相关的一些外部的、处理器相关的处理过程也会一并被执行。

## .irp symbolX, values...
产生一个语句序列，用来给symbolX赋不同的值。这个语句序列以.irp命令开始，以.endr命令结束。对于每个参数中给定的值，symbolX会被赋予这个值，产生的语句序列是可以被汇编的。如果参数中没有声明values，那么这个语句序列只会被汇编一次，并且symbolX会被赋值位空字符串。如果想要在产生的语句序列中引用symbolX，可以使用符号‘\symbolX’。

举个例子，如果我们使用语句：
.irp param, 1,2,3
move d\param,sp@-
那么上面的语句在汇编的时候等价于：
move d1,sp@-
move d2,sp@-
move d3,sp@-

## .irpc symbolX, values...
产生一个语句序列，用来给symbolX赋不同的值。这个语句序列以.irpc命令开始，以.endr命令结束。对于每个参数中给定的字符，symbolX会被赋值为这个字符，产生的语句序列是可以被汇编的。如果参数中没有声明values，那么这个语句序列只会被汇编一次，并且symbolX会被赋值位空字符串。如果想要在产生的语句序列中引用symbolX，可以使用符号‘\symbolX’。

举个例子，如果我们使用语句：
.irp param, 123
move d\param,sp@-
那么上面的语句在汇编的时候等价于：
move d1,sp@-
move d2,sp@-
move d3,sp@-


## .lcomm symbolX, length
给symbolX指定的一个本地变量预留length个字节。符号symbolX的section和值与这个预留的变量相同。变量的地址是分配在bss section中，因此在运行时的时候，这个预留的字节内容全是0.symbolX不是全局定义的，因此通常ld链接器并不能看到这个符号。

一些计算机平台的.lcomm命令允许有第三个参数，这个参数声明了符号symbolX在bss section中对齐方式。

## .lflags
as汇编器会识别这个命令，但仅仅是为了和其他汇编器兼容，as会直接无视这个命令。

## .line line-number
这个命令可以改变逻辑的行号。参数line-number必须是一个常量表达式。这个命令的下一行会使用这个逻辑的行号。因此，任何其他在当前行的语句在逻辑上的行号是line-number - 1。as会在将来废除这个命令： 因为这个命令仅仅是为了和其他的汇编器兼容。

尽管这个命令的功能与obj文件是a.out格式还是b.out格式相关，as汇编器仍然会在汇编COFF文件的时候接受这个命令。在汇编COFF文件的时候，.line命令和COFF文件中的.ln命令类似。

当在.def/.endif命令之间使用.line命令时，这个命令会被编译器用来生成辅助性的符号调试信息。

## .linkonce [type]
将当前section标记一下，让链接器命令这个sectin只能被链接一次。这个命令在将同一个section的内容include到多个不同的obj文件中的时候特别有用。.linkonce这个命令必须在这个section的每个实例中被调用。判断section重复的方法时看多个section的名字是不是相同的，因此使用.linkonce的时候需要确保section的名字不和其他section相同。

这个命令只支持在一小部分的obj文件格式中使用。到这个手册撰写的时间为止，目前只有Windows NT系统的PE格式支持这个命令。

type参数时可选的。如果声明了type参数的下，这个参数的值必须是以下几种字符串：
discard: 丢弃重复的section，并且不发出告警。这个时默认的选项。
one_only: 在有重复的sectoin的时候，只保留一份，同时抛出告警
same_size: 如果重复的section的大小不一样的时候，发出告警。
same_contents: 如果重复的section的内容不是严格一致的话，发出告警。

## .ln line-number
.ln命令和.line命令相同。

## .mri val
如果参数val是一个非0值，这个命令的含义时告诉as汇编器进入MRI模式。如果val的值是0，这个命令的含义时告诉as退出MRI模式。这种操作会影响后续代码的汇编形式，这种影响一直持续到下一个.mri命令或者文件末尾。

## .list
（和.nolist命令一起使用）用来控制汇编的时候是不是要在控制台列出当前的汇编内容。这两个命令会维护一个内部的计数器，计数器的初始值时0..list命令会对计数器++，.nolist命令会对计数器--。汇编器会在这个计数器大于0的时候向控制台列出当前在汇编的内容。

默认情况下，列出功能时关闭的。当你使用as命令的-a选项启用这个功能的时候，这个计数器的值会被设置成1.

## .long expression.
和.int命令相同。

## .macro
.macro命令和.endm命令允许你自定义个一个汇编宏。举个例子，下面的代码定义了一个sum宏，它能将一个数字序列写入到内存中：
```
.macro sum from=0, to=5
.long \from
.if \to-\from
sum "(\from+1)",\to
.endif
.endm
```
在上面的宏定义中，“SUM 0,5”这句话等价于：
.long 0
.long 1
.long 2
.long 3
.long 4
.long 5

.macro macname
.macro macname macargs ...
  这两句话是用来定义一个名位macname的宏。如果你定义的宏接受参数，你可以在macname后面接着声明参数名，参数名之间通过逗号或者空格分隔。你可以用"=deflt"来为每个参数定义一个默认值。举例来说，下面的宏定义都是合法的：
  - .macro comm: 定义一个名为comm的宏，不带参数；
  - .macro plus1 p, p1
  - .macro plus1 p p1: 这两个语句都是定义了一个名为plus1的宏，这个宏带两个参数，在宏的定义范围内，可以使用“\p”和“\p1”来代表这两个参数
  - .macro reserve_str p1=0 p2: 定义一个名为reserve_str的宏，这个宏带两个参数。第一个参数有默认值，第二个没有。宏定义完成之后，你可以“reserve_str a,b”这样的方式定用宏，也可以“reserve_str ,b”这样的方式调用宏。
  
  在调用宏的时候，你既可以通过参数位置来声明一个参数的值，也可以通过给定参数名来声明参数的值。比如，"sum 9,17"等价于"sum to=17,from=9"

.endm 标识一个宏定义的结束。
.exitm 提前从当前的宏定义中退出
\@  as汇编器维护了一个计数器来记录当前运行了多少个宏了，\@就是这个计数器变量。你可以使用“\@”将这个计数器的值拷贝到你的输出中，但是这个符号只能在宏定义的上下文中使用。

## .nolist
见.list

## .octa bignums
这个命令可以接0个或者多个大整数，各个数以逗号隔开。这些大整数最多是16个字节的整数。

为啥这里使用.octa这个单词呢？ 因为在计算机中，通常定义一个word是两个字节，那么octa个word就是16个字节了。

## .org new-lc, fill
将当前section的位置计数器（location counter）推进到new-lc参数指定的位置。new-lc可以是一个常量表达式，或者相同section内的当前subsection的表达式。也就是说，你不能用.org命令来跨section。如果new-lc指定到了一个非法的secton，.org命令会被汇编器给忽略掉。为了和以前的编译器兼容，如果new-lc指定的sectoin是absolute，as汇编器会抛一个告警，然后并提示说new-lc指示的section和当前的subsection相同。

.org一般只能向前增加位置计数器，或者不做改动。你不能时候.org命令来将位置计数器向后移。

由于as汇编器会尽力只执行一遍就完成汇编，new-lc参数不能是undefined。

注意，移动的起点是section的起始点，而不是subsection的起始点。这个是为了和其他的汇编器保持兼容。

当当前subsection的位置计数器开始向前移动时，越过的字节空间会被填充位参数fill提供的字节内容。所以fill参数必须是一个常量表达式。如果逗号和fill参数省略的话，默认使用0进行填充。

## .p2align[wl] abs-expr, abs-expr, abs-expr
将一个位置计数器（在当前subsection的位置计数器）限制在一个特殊的存储范围内。第一个参数必须是一个常量表达式，它声明了在对齐后位置计数器的低多少位必须是0.举个例子： '.p2align 3'说明位置计数器需要需要向前移动，直到位置计数器的值是8的倍数。如果位置计数器已经是8的倍数，那不需要做什么变化。

第二个参数也是一个常量表达式，它声明了在对齐的时候，使用什么字节来填充。第二个参数可以被省略，如果省略的话，默认会使用0来填充。不过呢，在一些系统中，如果这个section中含有代码，并且省略了使用什么值来填充，那么会使用no-op指令来填充。

第三个参数也是一个常量表达式，并且也是可选的。如果使用了这个参数，那么它声明了在对齐命令执行的时候，允许跨过的最大字节数。如果跨过的字节数大于这个参数的给定值，那么这次对齐不会执行。你可以使用两个逗号（‘,,’）来省略第二个参数同时使用第三个参数。这种写法在你想使用no-op命令来填充的时候很有用。

.p2alignw和.p2alignl命令是.p2align命令的衍生。.p2alignw命令按照2个字节的模式来填充。.p2alignl命令按照4字节的模式来填充。举个例子，‘.p2alignw 2,0x368d’会按照4的倍数进行字节对齐，如果对齐过程中跨过的了2个字节，那么跨过的这两个字节会使用‘0x368d’这个值来填充。如果跨过的字节数是1或者3，那么使用什么值来填充是不确定的。

## .previous
这个是ELF文件的section栈操作命令。其他的栈操作命令包括.section, .subsectioin, .pushsection 和 .popsection。

这个命令会交换当前的section和上一个访问的section。同一行中的多个.previous命令会翻转两个section。

从section stack的角度来看，这个命令会交换当前section和section stack的栈顶sectoin。

## .popsection
这是一个ELF文件的secton stack操作命令。

这个命令会将当前section替换成section stack的栈顶section。

## .print string
as汇编器会在汇编的时候将string的内容输出到标准输出。string必须使用双引号引起来。

## .protected names
这个是一个ELF的可见性命令。其他的两个可见性控制命令是.hidden和.internal。

这个命令会覆盖有名符号的默认可见性（由它们的绑定关系确定：local, global或者weak）。这个命令会将可见性设置成protected，这意味着在当前模块中任何对于个这符号的引用都被限制在定义这个符号的模块中，即便在另外一个模块中再定义一下这个符号也不行。

## .psize lines, columns
可以使用这个命令来定义每页可以有多少行，多少列（多少列是一个可选项）。

如果你不使用.psize命令，那在汇编过程展示的时候默认使用60行每页。你可以省略columns这个参数，省略的话默认值是200.

as汇编器会在超出这个限定的行数之后自动添加换行符。如果你想自己搞一个换行符，你可以使用.eject命令。

如果你声明的Lines参数是0，那就意味着as汇编器不会自己加换行符了。

## .purgem name
undefine一个宏名，这样的话后面就不能使用这个宏了。

## .pushsection name, subsection
这是一个ELF文件的secton stack操作命令。

这个命令和.section命令是一个语义。它会将当前section push到section栈的栈顶，然后将当前栈的名字和subsection替换成‘name’参数的值和subsection参数的值。

## .quad bignums
.quad接受0个或者多个bignum参数，参数间通过逗号隔开。每个bignum都占用8个字节，如果这个bignum超过了8个字节，那会抛出一个告警，然后截断低位的8个字节。

单词‘quad’是从'word'这个词演化过来的，'word'是占2个字节，所以'quad-word'是占8个字节。（quad在英文中有4倍的意思）

## .rept count
将指令.rept和指令.endr之前的行数按顺序重复‘count’次。

举个例子，汇编代码：
.rept 3
.long 0
.endr

等价于：
.long 0
.long 0
.long 0

## .sbttl "subheading"
在生成listing的时候将参数“subheading”设置位标题。

这个命令会影响后面的页数，如果当前行在一页的开它，那也会影响当前页。

## .scl class
为一个符号设置存储类型（storage-class）。这个命令只能在.def/.endef命令对中间使用。存储类型指的是将一个符号标记成是静态的或者外部的，或者记录一些将来调试用的符号信息。

.scl命令最开始是COFF输出文件专用的。如果在生成b.out文件的时候设置这个，as汇编器会忽略。

## .section name
使用.section命令来指示汇编器将下面的代码汇编到‘name’参数指定的section中。

这个命令只能在那些支持自定义section名称的目标文件格式中使用。比如在a.out格式中，这个命令就不支持，即使你声明的是一个标准a.out支持的section名也不行。

COFF格式版本
对于COFF目标格式，.section命令有下面几种使用方式：
.section name[, "flags]
.section name[, subsegment]
如果可选的参数是通过双引号引起来的，那么参数会被认为是描述这个section的一些flag标记。每个标记都是一个字符。总共支持的字符包括：
b: 代表bss section
n: 说明section没有被加载
w: 可写section
d: 代表是一个data section
r: 代表是一个只读section
x: 代表是一个可执行section
s: 代表是一个共享section（对于PE格式的输出比较重要）
a: 无意义，仅仅是为了兼容ELF格式

如果没有声明任何flag，那么默认的flag值由section名字决定。如果声明的参数‘name’不是一个标准的section，as汇编器不认识，那么默认的flag会将section设置成可加载、可写。注意，'n'和'w' flag的原理是从指定的section中移除一些属性，而不是往这个section中添加一些属性，因此如果你仅仅使用这两个flag，相当于你没有对这个section的声明任何flag。

如果提供的参数没有加引号，那这个参数会被当作一个subsegment的编号。

ELF格式版本
这个命令是一个ELF section栈操作命令。其他的几个section栈操作命令包括：  .subsection, .pushsection, .previous等等。

对于ELF格式，.section命令的用法是：
.section name [, "flags" [, @type[, @entsize]]]
可选的参数flags需要用引号引起来，它一般是下面几个字符组成的字符串：
a: section可以被分配空间
w: section是可写的
x： section可执行
M：section可以被合并
S：section包含以0结尾的字符串
可选参数‘type’可以使用下面几种可选值：
@progbits: 说明section包含数据
@nobits：说明section不包含数据（也就是说，这个section仅仅是用来占空间的）
注意，如果在一些‘@’字符代表注释的体系架构下（比如ARM架构），那么就得用其他符号来代替’@‘字符了。比如ARM架构就是使用’%‘来代替’@‘的。

如果‘flags’参数中包含‘M’，那么‘type’参数必须带上‘entsize’参数。带有'M'标记但是不带‘S’标记的section必须包含固定大小的常量，每个常量必须是‘entsize’这么大。既带有‘M’标记又带有‘S’标记的section必须包含以0结尾的字符串，并且字符串中的每个字符都必须是‘entsize’这么大。链接器可能会去除一些相同名字、并且相同flags、并且相同entsize的section。

如果没有声明flags，那默认的flags取决于声明的是什么section名字。如果声明的不是一个汇编器认识的名字，那么默认的flags就代表这个section不包含上面介绍的任何一个flag： 也就是说，这个section不能在内存中分配空间，并且不可写，并且不可执行，可以包含数据。

对于ELF格式来说，汇编器支持另外一种形式的.section命令： .section "name"[, flags...]，这个是为了和Solaris汇编器兼容而设计的。

这个命令会将替换当前section和当前subsection的值。被替换的section和subsection会被push到section栈中。可以在gas汇编器源码的测试用例目录gas/testsuite/gas/elf中找到一些section栈操作命令使用的例子。

## .set symbol, expression
将符号‘symbol’的值设置成表达式‘expression’代表的值。这个命令会改变符号'symbol'的值和类型。如果符号'symbol'的flag是external，那么这个符号的flag还是会保留。

你可以使用.set命令重复设置一个符号多次。

如果你使用.set命令设置一个全局符号，那么最终存入到obj文件中的符号的值将会是最后一次设置的值。

## .short expressions
.short命令和.word命令差不多。不过在有一些系统的配置中，.short命令和.word命令生成的长度是不一样的。

## .single flonums
这个命令会编排0个或者多个浮点数，每个浮点数通过逗号隔开。这个命令的效果和.float命令的效果一样。具体这个浮点数是如何标识的依赖于as汇编器的配置。

## .size
这个命令用来设置一个符号的大小

COFF格式
对于COFF格式的目标文件来说，这个.size命令只能在.def/.endef命令对中使用，使用的方法是： .size expression。.size命令只有在生成COFF格式的输出文件时有意义，如果as汇编器输出的b.out格式的文件，那么这个命令会被忽略。

ELF格式
对于ELF格式的目标文件来说，这个.size命令的用法是： .szie name, expression 。这个命令设置了一个名为'name'的符号的大小。符号大小由expression表达式的值给出。这个命令多用来定函数符号的大小。

## .sleb128 expressions
sleb128代表‘single little endian base 128’

## .skip size, fill
这个命令会向前预留'size'个字节，每个字节使用‘fill’值填充。size参数和fill参数都是常量表达式。如果逗号和fill参数省略了的话，fill参数的值默认会是0。这个命令和.space命令是一样的。

## .space size, fill
这个命令会向前预留'size'个字节，每个字节使用‘fill’值填充。size参数和fill参数都是常量表达式。如果逗号和fill参数省略了的话，fill参数的值默认会是0。这个命令和.skip命令是一样的。

## .stabd, .stabn, stabs
这三个命令都以.stab打头。这些个命令都会声明一些供符号调试器使用的符号。这些符号都不会进入到as汇编器的hash表中： 因此它们不能在源代码的任何地方被引用。这些符号有5个字段：

string： 这个是声明这个符号的名字。可以包含任何字符除了字符‘\000’，也就是说这里的符号名可以比普通的符号名包含更多类型的字符。一些调试器往往会在这个字段设置一些更加复杂的符号名。

type: 一个常量表达式。符号的类型会被设置成这个表达式的低8位。任意位的组合都是可以的，但是ld链接器和调试器会被一些奇奇怪怪的位组合给卡住。

desc： 一个常量表达式。符号的descriptor会被设置成这个表达式的低16位。

value: 一个常量表达式，符号的值会被设置成这个表达式的值。

如果在读取一个.stabd, .stabn 或者 .stabs语句的时候产生了告警，那这个符号大概率已经被生成了；不过你会在你的obj文件中得到一个不完整的符号。这种行为也是为了和以前的汇编器保持兼容。

.stabd type, other, desc: 参数‘name‘代表生成的符号名，基本上不可能是一个空字符串。为了保持兼容性，在是一个空字符串的时候，它会是一个空指针。一些老的汇编器会使用空指针来代表空字符串，这样不会浪费空间。定义的符号的值会设置在当前位置计数器的地方。当你的程序被链接时，这个符号的地址会被链接成当前位置计数器的地址。

.stabn type, other, desc, value: 符号的名字会被设置成空字符串。

.stabs string, type, other, desc, value： 5个字段都声明了。

## .string "str"
将当前字符串“str”拷贝到目标文件中。你可以同时声明多个字符串，每个字符串都以逗号隔开。正常情况下，汇编器会将每个字符串的结尾添加一个0字节。

## .struct expression
切换到absolute section，并且将section的偏移设置成参数'expression'的值，这个参数必须时一个常量表达式。你可以这样使用：
.struct 0
field1:
.struct field1+4
field2:
.struct field2+4
field3:
这种写法会将符号field1的值设置成0，符号field2的值设置成4，符号field3的值设置成8.这个汇编过程会被限制在absolute section内，在进一步汇编之前，你需要使用.section命令来切换到其他的section内。

## .subsection name
这个是EFL section栈操作命令之一，其他的几个栈操作命令包括.section， .pushsection, .popsection, 和 .previous。

这个命令会将当前的subsection替换成参数'name'声明的section。当前的sectioni不会发生改变。被替换的subsection会被push到section栈的顶部。

## .symver
你可以使用.symver命令在一个源代码文件中将一个符号绑定到一个指定的版本。这个命令只支持在ELF格式文件中使用，而且在将文件汇编和简介到一个共享库中时十分有用。下面有一些例子展示了如何在共享库中使用.symver命令来将符号限定在某些版本中可用。

对于ELF文件来说，.symver可以这样用：
.symver name, name2@nodename
如果在汇编源代码文件中定义一个符号名‘name’,.symver命令会同时为符号‘name’创建一个别名‘name2@nodename’, 事实上我们不直接创建一个别名符号的原因是'@'符号是不能在符号名中使用的。‘name2’这一部分是这个符号的实际名字，可以被外部引用。参数‘name’仅仅是为了说明这个符号有可能在多个版本中有定义，这样编译器可以明确的知道具体指的是那个符号。'nodename'这一部分是一个在version script(提供给链接器使用)中声明的节点的名字。如果你想在共享库中覆盖一个带版本的符号，那么‘nodename’参数就需要对应上你想覆盖的符号对应的nodename。

当参数‘name’对应的符号没有在汇编的时候定义的时候，所有引用符号‘name’的地方都会被汇编成‘name2@nodename’。如果符号‘name’没有被引用过，那么符号'name2@nodename'会从符号表中被移除。

另一个使用.symver命令的方法： .symver name, name2@@nodename。在这种用法中，符号‘name’必须存在并且在当前被汇编的文件中存在。第二个参数和'name2@nodename'有点像，但是‘name2@@nodename’也会被链接器当作是‘name2’的引用。

第三种使用.symver命令的方法是： .symver name, name2@@@nodename。在汇编的过程中如果发现‘name’没有被定义，那么这个符号会被当作‘name2@nodename’。当符号‘name’定义了的时候，符号’name‘会转成'name2@@nodename'.

## .tag structname
这个命令是由编译器自动生成的，用来在符号表中生成一些辅助的调试信息。这个命令只能在.def/.endef命令对中出现。Tags可以用来连接符号表中这些structure的定义以及这些structure的实例。

‘.tag’只能在生成COFF文件的时候使用。当生成b.out文件的时候，as汇编器会直接忽略这个命令。

## .text subsection
告诉as汇编器将下面的语句汇编到.text的第'subsection'个subsection的末尾，参数‘subsection’必须是一个常量表达式。如果省略了‘subsection’参数，那么默认是0.

## .title "heading"
在生成Listing的时候将title设置成“heading”。

这个命令会影响后续list时候的页数，如果在当前页的前10行出现的时候，也会影响当前页的页数。

## .type
这个命令用来设置一个符号的类型。

COFF版本：
对于COFF格式的文件，这个命令必须出现在.def/.endef命令对的中间。用起来时候像这样： .type int。类型的属性会以一个整数的形式存放在符号表中。
.type命令只对COFF文件格式有效，当生成b.out格式的时候，这个命令会直接被忽略。

ELF版本：
对于ELF格式的文件，这个命令是这样使用的： .type name, type description。这个用法会将符号‘name’的类型设置成一个函数符号或者一个对象符号。参数”type description“有5种不同类型的语法(为了和其他的汇编器兼容):
.type <name>, #function
.type <name>, #object
.type <name>,@function
.type <name>,@object
.type <name>,%function
.type <name>,%object
.type <name>,"function"
.type <name>,"object"
.type <name>,STT_FUNCTION
.type <name>,STT_OBJECT

## .uleb128 expressions
uleb128的意思是： ‘unsigned little endian base 128’。

## .val addr
这个命令只能出现在.def/.endef命令对之间，参数‘addr’记录了符号表中符号的value属性所存放的地址。

.val命令只能在COFF文件格式中使用 ，当as汇编器输出b.out格式文件的时候，这个命令会被忽略。

## .version "string"
这个命令会创建一个.note section，并且会将一个ELF格式的note放入这个section。这个note的类型是NT_VERSION，名字为参数"string"。

## .vtable_entry table, offset
这个命令会查找或者创建一个名为“table”的符号，并且为它创建一个VTABLE_ENTRY重定位。

## .vtable_inherit child, parent
这个命令会查找一个名为'child'的符号，然后查找或创建一个名为‘parent’的符号，然后为符号parent做一次VTABLE_INHERIT重定位，重定位的偏移值是符号child的值。一种特殊的情况是，parent的名字是0，那么parent会被当作ABS section。

## .weak names
这个命令会为参数'names'声明的符号序列（符号序列以逗号相互隔开）设置一个weak属性。如果给定的符号已经不存在，那么会创建新的同名的符号。

## .word expressions
这个命令接受0个或者多个表达式作为参数，每个参数以逗号隔开。

给定的参数中的数字大小范围，以及字节序，取决于汇编器运行的计算机架构。

注意： 为了支持编译器做的一些特殊处理：
32位地址空间但是寻址不足32位的机器，这个命令会做以下特殊处理：如果这个机器使用了32位寻址，你可以忽略这个问题。为了汇编一些编译器生成的中间代码，as汇编器有些时候会在.word命令中做一些奇怪的事情。比如命令格式‘.word sym1-sym2’会经常被编译器用来在表之间做跳转。因此，当as汇编器汇编一个格式为’.word sym1-sym2‘的命令，并且sym1和sym2之前的差异不是16位的时候，as汇编器会在下一个标号前立即创建一个secondary jump table。这个“secondary jump table”会做一个short-jump跳转到这个secondary table后面的第一个字节。这次跳转可以避免控制流意外的进入新的table中。


# 80386架构相关的特性
as汇编器的i386版既支持原有的Intel 386架构(包括16位和32位模式)，也支持AMD在Intel 64位架构下扩展的的x86-64架构。

## 命令行选项
as汇编器的i386版本有一些机器相关的命令行选项：
--32 | --64
设定字长为32位或者64位。选择32位的话意味着是Intel i386架构，选择64位的话意味着是AMD x86-64架构

这个选项只能在生成ELF格式文件的时候有用，并且需要确保必要的BFD支持已经被包含了（在32位平台你需要加上-enable-64-bit-bfd来配置）。

## AT&T汇编语法 vs Intel汇编语法
as汇编器现在也支持使用Intel汇编器的语法，你可以使用.intel_syntax来选择Intel模式，使用.att_syntax来切回默认的AT&T模式。GCC默认是生成AT&T模式的汇编代码。上面这两个命令可以带可选的参数： prefix/noprefix声明寄存器符号是否需要一个"%"作为前缀。AT&T的汇编语法和Intel的语法有很大不同，我们这里特意说明下这些不同，因为几乎所有的80386文档都是使用的Intel语法。这两种语法建最大的两个区别是： 
1. AT&T的立即数以‘$’打头；Intel的立即数没有什么特殊前缀（比如Intel语法的'push 4'在AT&T语法中为'pushl $4'）。AT&T的寄存器操作数以'%'打头，Intel语法中的寄存器操作数没有什么特殊前缀。AT&T中的常量跳转/调用操作（与之相反的是PC相关的跳转/调用操作）以'*'打头，在Intel语法中没有什么特殊前缀。
2. AT&T和Intel的语法在源操作数和目的操作数的顺序上正好相反。Intel的语句“add eax, 4”在AT&T中为“addl $4, %eax”. “source, dest”这样的使用习惯是为了和以前的Unix汇编器保持兼容。需要注意的是，那些多于一个源操作数的指令（比如'enter'指令）没有采用和Intel相反的顺序。
3. AT&T的语法中，内存操作数的大小由指令助记符的最后一个字符决定。这些助记符的后缀包括： 'b', w', 'l', 'q' 分别对应 byte(8位),word(16位), long(32位),和quadruple word(64位)内存寻址。在Intel的语法中，做到相同功能的写法是给内存操作数(不是指令助记符)加前缀，对应的，内存操作数的前缀包括'byte ptr', 'word ptr', 'dword ptr', 'qword ptr'。因此，Intel中的'mov al, byte ptr foo'对应与AT&T中的'movb foo, %al'。
4. AT&T语法中的长跳转/长调用逻辑是：'lcall/ljump $sectoin, $offset'; 对应的Intel语法是： 'call/jmp far section:offset'。对应的，AT&T中的长返回指令为：'lret $stack-adjust'; Intel的长返回语法为：'ret far stack-adjust'.
5. AT&T的汇编器不支持多section的程序，Unix风格的系统希望所有的程序都在一个section中。

## 指令命名
AT&T的指令助记符会以一个特殊字符结尾，这个字符标识了操作数的大小。'b','w','l','q'分别对应于byte, word, long, quadruple word。如果没有声明任何的后缀的话，as汇编器会尝试依据目标寄存器来自动补上这个后缀字符。因此，'mov %ax, %bx'等价于‘movw %ax, %bx’; 同样的，'mov $1, %bx'等价于‘movw $1, bx’。

几乎所有的AT&T指令都会在Intel格式中有同名的指令。不过也有一些例外。符号扩展和0扩展指令在使用时需要两种大小的操作数。一种大小用来指示此次符号/0扩展的from的大小，另一种大小用来指示0扩展中的to的大小。这个需求在AT&T语法中是通过两个指令助记符后缀来完成的。AT&T语法中基础的符号扩展和0扩展为： 'movs ...'/'movz...'（对应Intel语法中的'movsx'和'movzx'）。指令助记符的后缀紧跟着指令的基础名字，from的后缀，然后是to的后缀。因此，AT&T中的语句‘movsbl %al, %eds’意思是：“符号扩展移动，从%al到%edx”。这里的后缀是'bl',意味这是“从byte到long”。其他的还有'bw', 从byte到word；'wl',从word到long；'wl',从word到long; 'bq',从byte 到quadruple word; 'wq', 从word到quadruple word; 'lq', 从long到quadruple word。

在Intel语法中,下面这些指令：
 - 'cbw': 符号扩展，将“%al”扩展为"%ax"
 - 'cwde': 符号扩展，将一个"%ax"中的word扩展成一个存储在"%eax"中的long
 - 'cwd': 符号扩展，将"%ax"中的word扩展成一个存储在"%dx:%ax"中的long
 - 'cdq': 符号扩展，将“%eax”中的一个dword扩展成一个存储在“%edx:%eax”中的quad
 - 'cdqe': 符号扩展，将“%eax”中的一个dword扩展成一个存储在“%rax”中的quad
 - 'cdo': 符号扩展，将"%rax"中的一个quad扩展成一个存储在"%rdx:%rax"中的octuple
在AT&T中对应的写作： "cbtw", 'cwtl', 'cwtd', 'cltd', 'cltq', 'cqto'。

AT&T中的长调用/跳转指令为“lcall”/"ljmp"，Intel中对应的指令为“call far”/"jump far"。

## 寄存器命名
寄存器操作数通常会以"%"为前缀。80386寄存器包括：
 - 8个32位的寄存器%eax(累加器)， %ebx, %ecx, %edx, %edi, %esi, %ebp(栈帧指针), %esp(栈顶指针)
 - 8个16位寄存器： %ax, %bx, %cx, %dx, %di, %si, %bp, %sp
 - 8个8位寄存器： %ah, %al, %bh, %bl, %ch, %cl, %dh, %dl
 - 6个section寄存器： %cs (code section), %ds(data section), %ss(stack section), %es, %fs, %gs
 - 3个处理器控制寄存器： %cr0, %cr2, %cr3
 - 6个调试寄存器： %db0, %db1, %db2, %db3, %db6, %db7
 - 2个调试寄存器： %tr6和%tr7
 - 8个浮点栈寄存器： %st，准确的写法是： %st(0), %st(1), %st(2),%st(3),%st(4),%st(5),%st(6),%st(7)。这些寄存器也就是对应的8个MMX寄存器： %mm0, %mm1, %mm2, %mm3, %mm4, %mm5, %mm6, %mm7。
 - 8个SSE寄存器： %xmm0,%xmm1,%xmm2,%xmm3,%xmm4,%xmm5,%xmm6,%xmm7。

AMD的x86-64扩展的寄存器包括：
 - 将8个32位寄存器扩展到64位： %rax, %rbx, %rcx, %rdx, %rdi, %rsi, %rbp, %rsp
 - 8个扩展寄存器: %r8 - %r15
 - 8个32位的小端扩展寄存器： %r8d - %r15d
 - 8个16位的小端扩展寄存器： %r8w - %r15w
 - 8个8位的小端扩展寄存器： %r8b - %r15b
 - 4个8位的寄存器： %sil, %dil, %bpl, %spl
 - 8个调试寄存器： %db8 - %db15
 - 8个SSE寄存器： %xmm8 - %xmm15

## 指令前缀
指令前缀是用来修改接下来的指令的。它们的功能包括： 重复一些字符串指令，提供section覆盖，执行锁总线操作，改变操作数和地址大小。指令前缀最好和它们要修饰的指令写在同一行。举个例子，“scas”(scan string)指令可以这样被修饰： repne scans %es:(%edi), %al

下面是一些常见的指令前缀：
 - section覆盖类前缀： 'cs', 'ds', 'ss', 'es', 'fs', 'gs'。这些前缀会被汇编器自动添加到section:memory-operand内存寻址表达式上，形成一种类似于“section:memory-operand”形式的地址表达式。
 - 操作符/地址大小前缀（比如data16/addr16）可以将一个32位的操作数/地址转成16位的操作数/地址；data32/addr32可以将16位的操作数/地址转成32位的操作数/地址。这些指令前缀必须和它们修饰的指令出现在同一行。举个例子，在一个16位的.code16 section内， 你可以这么写： addr32 jmpl *(%ebx)。
 - 锁总线前缀可以让当前指令在执行的时候禁用中断。这种用法只对某一些指令有效，详情得参考80386的手册。
 - 等待协处理器的指令前缀："wait"，它可以让当前指令在执行的时候等待协处理器操作完成。这个在80386/80387系列中用不上了。
 - "rep", "repe", "repne"前缀，用于修饰字符串指令，这样可以让这些字符串指令重复“%ecx”次。
 - x86-64指令集使用"rex"系列前缀来扩展i386指令集。"rex"前缀有4位，用来将一个操作数从32位扩展到64位，

## 内存寻址
在Intel汇编中，间接寻找的格式是： section: [base+index*scale+disp]， 对应AT&T格式为： section:disp(base, index, scale)。
这里base和index是可选的32位基址和索引寄存器，disp是可选的偏移量，scale 可以取值1,2,4,8, 代表这个操作数的地址是多少倍的index。如果没有给出scale参数的话，scale默认是1。section参数声明了内存操作数的section寄存器，这个参数可能覆盖默认的section寄存器。不过要注意的是AT&T语法中，寄存器覆盖必须用符号“%”显式说明。如果你声明的section寄存器和默认的section寄存器一样的，as汇编器不会给当前指令加什么前缀修饰。因此，section这个参数一般用来强调声明下当前的内存寻址使用的是哪个section寄存器。

下面是一些具体的例子来比较Intel和AT&T的内存寻址风格：
 - AT&T： '-4(%ebp)', Intel: '[ebp -4]'。base参数位%ebp, disp参数为-4，section参数没有给出，默认为%ss，因为使用%ebp默认对应的就是%ss。index, scale参数都没有给出。
 - AT&T: 'foo(,%eax, 4)', Intel: '[foo+eax*4]': index参数为%eax, scale参数为4，disp参数为'foo'。其他所有的参数都省略了。section寄存器默认使用的是%ds
 - AT&T: 'foo(,1)', Intel: '[foo]': 这个指令是将foo的地址值地址当作了内存操作数。base和index参数省略了，其他的只有一个逗号，因此这个语句是有语法问题的。
 - AT&T： '%gs:foo', Intel: 'gs:foo': 这个是选中了变量foo中存储的内容，section寄存器使用的是gs

绝对调用和跳转指令必须使用'*'作为前缀。如果没有给一个'*'的话，as汇编器通常会选择一个PC计数器相对的地址进行跳转/调用。

任何有内存操作数，但是没有寄存器操作数指令，都必须通过指令助记符的后缀声明数据操作的大小。

x86-64架构添加了RIP(instruction pointer relative)地址。这种地址模式必须使用"rip"寄存器当作基址寄存器，并且只允许使用常量的offset。举个例子：
- AT&T: '1234(%rip)', Intel: '[rip+1234]' 这个地址指令当前指令的向后1234字节处。
- AT&T: 'symbol(%rip)', Intel: '[rip+symbol]'。这个地址以相对RIP指针的方式指向符号symbol。这种写法比使用默认的绝对寻址方式简短一些。

其他32位的寻址模式在x86-64位架构下都保持不变，除了将32位寄存器换成对应的64位寄存器。

## 跳转类指令的处理
跳转类指令通常都会被优化下，这样可以保证跳转的时候只跳转一个最小的偏移量。如果跳转的目标地址足够近的话，跳转指令的偏移量参数只有8位。如果跳转的目标地址太远的话，会使用一个长一点的偏移量参数来做这个事情。在32位模式中，我们不支持16位的跳转偏移量参数。这个不是汇编器的锅，这个是因为80386CPU架构只会使用%eip寄存器的16位，而不是32位。

需要注意的是，'jcxz', 'jecxz', 'loop', 'loopz', 'loope', 'loopnz' 和 'loopne'指令都只能使用8位的跳转偏移量，因此如果你要使用这些指令（反正GCC是不会用的）可能会报错。AT&T的80386汇编器为了应对这种问题，会尝试将'jcxz foo'命令解释成：
```
jcxz cx_zero
jmp cx_nonzero

cx_zero: jmp foo
cx_nonzero:
```

## 浮点数
80387的所有浮点类型（除了packed BCD）都支持。这些数据类型可以是16位、32位、64位的整数，以及32位的单精度浮点数和64位的双精度浮点数，还有80位的扩展京都浮点数。每种支持的类型都有一个指令助记符后缀以及一个与之相关的构造命令。指令助记符的后缀声明了这个操作数的数据类型。构造命令将这些数据类型存入到内存中。

 - 浮点类型的构造方法包括： .float, .single, .double 和 .tfloat 对应与 32位， 32位，64位，和80位的格式。80387只能通过fldt和fstpt指令来支持这些格式。
 - 整数构造命令包括： .word, .long, .int 和 .quad， 对应 16位， 32位， 64位 整数格式。对应的指令助记符后缀是's'(single), l(long), q(quad)。对于80位的实数格式，64位的'q'格式只会在 fildq和fistpq指令中出现。
 - 寄存器到寄存器的操作不应该使用指令助记符后缀。'fstl %st, %st(1)'这样的代码会抛出一个警告，然后当作指令'fst %st, %st(1)'命令去汇编。这是因为所有的寄存器到寄存器操作都是使用80位的浮点操作。

## Intel的MMX指令集，以及AMD的 3DNow! SIMD操作
as汇编器支持Intel的MMX指令集（Intel的整数数据SIMD指令集合）。as汇编器也支持AMD的3DNow!指令集（一种32位的浮点数数据的SIMD指令集）。

到目前位值，as汇编器还不支持Intel的浮点SIMD指令集：Katmai(KNI)。

8个64位MMX操作，也在3DNow!指令集中，它们是：  %mm0, %mm1, ..., %mm7。它们包含8个8位的整数，4个16位的整数，2个32位的整数，1个64位的整数，或者2个32位的浮点值。MMX寄存器不能同时当作浮点栈使用。

可以参考下Intel和AMD的文档，需要注意的是，AT&T语法和Intel语法中操作数的顺序是相反的。

## 用as写16位的汇编代码
除了使用默认的配置，用as来写纯32位的i386代码或者64位的x86-64位代码外，as汇编器也支持跑在实模式的代码和跑在保护模式的16位代码。如果想这么做的话，可以在要以16位模式运行的代码前面写上.code16或者.code16gcc命令。你可以通过.code32命令将as切会32位模式。

.code16gcc提供了一些尚在实验阶段的特性，它们可以支持gcc来生成16位的代码。因此，这个命令修饰的代码和.code16命令修饰的代码有一些不一样，主要是.code16中， call, ret, enter, leave, push, pop, pusha, popa, pushf, popf这几个指令默认是32位大小的。这么做是为了让栈指针可以在函数调用的时候保持同样的操作行为，以此来确保32位模式下函数的参数顺序是没有问题的。.code16gcc命令也会gcc生成32位地址的时候自动添加地址的大小前缀。

as汇编器生成的16位代码不一定必须要运行在16位的80386处理器上。如果你确实是需要跑在这样的处理器上，你必须自己保证绝不在代码中使用任何32位的指令。

如果要写16位的代码指令，得显式地声明指令助记符的前缀和后缀。在32位的代码段中，下面的代码： pushw $4 会生成机器码'66 6a 04'，意思是将4push到栈上，然后将%esp减2.

同样的代码在16位的代码段中会生成机器码'6a 04'（也就是说，忽略了操作符前缀）。这个也是可以正确执行的，因为默认的操作数大小是16位，因为这个代码是在一个16位的代码段中。

## AT&T的语法bug
类Unix汇编器，以及其他从ix86衍生出来的AT&T汇编器，都会在几种特定的情况下，通过预留源寄存器和预留目的寄存器的方式来生成浮点指令。不幸的是，gcc以及其他的程序可能会使用这些预留的寄存器，然后我们就会卡在这了。

举个例子：  fsub %st, %st(3)

这个代码会将 %st(3)中的内容更新成 %st - %st(3) 而不是期望的 %st(3) - %st。这个情况会出现在两个操作数中源操作数是在%st寄存器中，目的操作数在%st(i)寄存器中的这种不可交换的四则浮点运算代码中。

## 声明CPU架构
as汇编器和通过.arch cpu_type命令声明一个特定的CPU架构。声明之后，gas如果检测到一个不属于这个架构的指令之后，会抛出一个告警。cpu_type参数的可选项包括：'i8086', 'i186', 'i286', 'i386', 'i486', 'i586', 'i686', 'pentium', 'pentiumpro', 'pentium4', 'k6', 'athlon', 'sledgehammer'.

除去可以产生告警之外，这个指令还会对as汇编器的行为产生下面两个影响： 1. 如果你声明了一种CPU架构（除了i486架构），那么左移一位的指令，像'sarl $1, %eax'会自动使用一个2字节的操作码序列（i486架构会产生3字节码序列,你可以通过代码'sar1 %eax'显式地请求）。2. 如果你声明了'i8086', 'i186'或者'i286'CPU架构，同时又使用了.code16/.code16gcc命令，那么在必要的时候，一个以字节位单位的条件跳转语句会被转成两条指令，一条指令是相反的条件的跳转指令，另一条是一个无条件跳转指令，这个无条件跳转指令会跳转到原始的指令想要跳转的地方。对于这种情况的CPU架构，你可以使用'jumps'或者'nojumps'指令来控制这种特殊情况的条件跳转指令的转化。默认会使用‘jumps’指令来进行跳转转化。所有的外部跳转都是使用long类型变量，而文件内部的跳转是必须进行跳转转化的。'nojumps'指令将外部跳转指令看作是基于字节偏移的跳转，并且会在as汇编器进行文件内部跳转的转化的时候告警。非条件跳转指令和jumps指令类似。

举个例子，你可以写这样的代码：  .arch i8086, nojumps

## 备注
值的说明的是，mul和imul指令有一些trickery。16位，32位，64位，128位扩展的乘法（乘法基本的操作码是0xf6,扩展位4对应与mul指令，5对应于imul指令）只能以一种操作数格式输出。因此，imul %ebx, %eax 指令不会选择扩展的乘法。扩展乘法会搞乱%edx寄存器，这会对gcc的输出造成影响。可以使用imul %ebx来获取64位的输出值（这个值是存储在%edx:%eax中）。

我们添加了imul指令的2操作数形式，第一个操作数是一个立即数表达式，第二个操作数是一个寄存器。这只是一种简写，因此，如果你需要将 %eax乘以69，可以这么写： imul $69, %eax。而不是这么写：  imul $69, %eax, %eax。