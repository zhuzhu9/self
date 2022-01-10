[TOC]

### #

#是在宏定义中将参数进行字符串化的预处理特征(就是参数不代表值,就是字符本身)

**示例:**

```c
#include <iostream>
 
using namespace std;
#define P(EXP) cout<<#EXP<<":"<<EXP<<endl
int main()
{
    int a=123;
    float f=123.456;
    P(a);
    P(f);
    P(23);
    return 0;
}
```

**输出:**

```c
a:123
f:123.456
23:23
```

### ##

##是连接符

**示例:**

```c
#include <iostream>
using namespace std;
#define V(x)  var##x
int main()
{
    int var1=123,var2=222,var3=321;
    printf("%d\n",V(1));
    printf("%d\n",V(2));
    printf("%d\n",V(3));
    return 0;
}
```

**输出:**

```c
123
222
321
```

### `__`attribute`__`的section子项

**注意:**这里只是简单讲解一下使用方法,深入研究可以自己去查看.

`__`attribute`__`是GNU C的一大特色机制,有兴趣可以自己去深入研究,这里介绍下它的section子项.

使用section可以使我们如在初始化函数时,不用在主函数中去添加一个新的初始化程序,只需要在自己的函数模块内注册就好了.或者在实现某些命令时,添加或删除该命令的支持,会方便很多.

"section"关键字会将被修饰的变量或函数编译到特定的一块位置,不是物理存储器上的特定位置,而是在可执行文件的特定段内.

**可以使用命令查看**

```bash
~ readelf -S a.out
There are 31 section headers, starting at offset 0x1c68:

节头：
  [号] 名称              类型             地址              偏移量
       大小              全体大小          旗标   链接   信息   对齐
  [ 0]                   NULL             0000000000000000  00000000
       0000000000000000  0000000000000000           0     0     0
  [ 1] .interp           PROGBITS         0000000000400238  00000238
       000000000000001c  0000000000000000   A       0     0     1
  [ 2] .note.ABI-tag     NOTE             0000000000400254  00000254
       0000000000000020  0000000000000000   A       0     0     4
  [ 3] .note.gnu.build-i NOTE             0000000000400274  00000274
       0000000000000024  0000000000000000   A       0     0     4
  [ 4] .gnu.hash         GNU_HASH         0000000000400298  00000298
       000000000000001c  0000000000000000   A       5     0     8
  [ 5] .dynsym           DYNSYM           00000000004002b8  000002b8
       0000000000000108  0000000000000018   A       6     1     8
  [ 6] .dynstr           STRTAB           00000000004003c0  000003c0
       00000000000000c4  0000000000000000   A       0     0     1
  [ 7] .gnu.version      VERSYM           0000000000400484  00000484
       0000000000000016  0000000000000002   A       5     0     2
  [ 8] .gnu.version_r    VERNEED          00000000004004a0  000004a0
       0000000000000040  0000000000000000   A       6     1     8
  [ 9] .rela.dyn         RELA             00000000004004e0  000004e0
       0000000000000150  0000000000000018   A       5     0     8
  [10] .rela.plt         RELA             0000000000400630  00000630
       0000000000000078  0000000000000018  AI       5    23     8
  [11] .init             PROGBITS         00000000004006a8  000006a8
       0000000000000017  0000000000000000  AX       0     0     4
  [12] .plt              PROGBITS         00000000004006c0  000006c0
       0000000000000060  0000000000000010  AX       0     0     16
  [13] .plt.got          PROGBITS         0000000000400720  00000720
       0000000000000008  0000000000000008  AX       0     0     8
  [14] .text             PROGBITS         0000000000400730  00000730
       0000000000000302  0000000000000000  AX       0     0     16
  [15] .fini             PROGBITS         0000000000400a34  00000a34
       0000000000000009  0000000000000000  AX       0     0     4
  [16] .rodata           PROGBITS         0000000000400a40  00000a40
       00000000000000b1  0000000000000000   A       0     0     8
  [17] .eh_frame_hdr     PROGBITS         0000000000400af4  00000af4
       0000000000000054  0000000000000000   A       0     0     4
  [18] .eh_frame         PROGBITS         0000000000400b48  00000b48
       0000000000000168  0000000000000000   A       0     0     8
  [19] .init_array       INIT_ARRAY       0000000000600dd8  00000dd8
       0000000000000008  0000000000000008  WA       0     0     8
  [20] .fini_array       FINI_ARRAY       0000000000600de0  00000de0
       0000000000000008  0000000000000008  WA       0     0     8
  [21] .dynamic          DYNAMIC          0000000000600de8  00000de8
       00000000000001f0  0000000000000010  WA       6     0     8
  [22] .got              PROGBITS         0000000000600fd8  00000fd8
       0000000000000028  0000000000000008  WA       0     0     8
  [23] .got.plt          PROGBITS         0000000000601000  00001000
       0000000000000040  0000000000000008  WA       0     0     8
  [24] .data             PROGBITS         0000000000601040  00001040
       0000000000000010  0000000000000000  WA       0     0     8
  [25] .bss              NOBITS           0000000000601080  00001080
       0000000000000008  0000000000000000  WA       0     0     1
  [26] .comment          PROGBITS         0000000000000000  00001080
       0000000000000029  0000000000000001  MS       0     0     1
  [27] handler           PROGBITS         0000000000601050  00001050
       0000000000000030  0000000000000000  WA       0     0     16
  [28] .symtab           SYMTAB           0000000000000000  000010b0
       0000000000000798  0000000000000018          29    55     8
  [29] .strtab           STRTAB           0000000000000000  00001848
       000000000000030f  0000000000000000           0     0     1
  [30] .shstrtab         STRTAB           0000000000000000  00001b57
       000000000000010f  0000000000000000           0     0     1
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings), I (info),
  L (link order), O (extra OS processing required), G (group), T (TLS),
  C (compressed), x (unknown), o (OS specific), E (exclude),
  l (large), p (processor specific)

```

**如上，可以看到程序被分成了很多的段，其中“handler”为稍后步骤中自定义的一个段。**

#### 使用方法

##### 创建lds文件

```bash
~  ld --verbose > main.lds
~ cat main.lds
GNU ld (GNU Binutils for Ubuntu) 2.30
  支持的仿真：
   elf_x86_64
   elf32_x86_64
   elf_i386
   elf_iamcu
   i386linux
   elf_l1om
   elf_k1om
   i386pep
   i386pe
使用内部链接脚本：
==================================================
/* Script for -z combreloc: combine and sort reloc sections */
/* Copyright (C) 2014-2018 Free Software Foundation, Inc.
   Copying and distribution of this script, with or without modification,
   are permitted in any medium without royalty provided the copyright
   notice and this notice are preserved.  */
OUTPUT_FORMAT("elf64-x86-64", "elf64-x86-64",
	      "elf64-x86-64")
OUTPUT_ARCH(i386:x86-64)
ENTRY(_start)
SEARCH_DIR("=/usr/local/lib/x86_64-linux-gnu"); SEARCH_DIR("=/lib/x86_64-linux-gnu"); SEARCH_DIR("=/usr/lib/x86_64-linux-gnu"); SEARCH_DIR("=/usr/lib/x86_64-linux-gnu64"); SEARCH_DIR("=/usr/local/lib64"); SEARCH_DIR("=/lib64"); SEARCH_DIR("=/usr/lib64"); SEARCH_DIR("=/usr/local/lib"); SEARCH_DIR("=/lib"); SEARCH_DIR("=/usr/lib"); SEARCH_DIR("=/usr/x86_64-linux-gnu/lib64"); SEARCH_DIR("=/usr/x86_64-linux-gnu/lib");
SECTIONS
{
  /* Read-only sections, merged into text segment: */
  PROVIDE (__executable_start = SEGMENT_START("text-segment", 0x400000)); . = SEGMENT_START("text-segment", 0x400000) + SIZEOF_HEADERS;
  .interp         : { *(.interp) }
  .note.gnu.build-id : { *(.note.gnu.build-id) }
  .hash           : { *(.hash) }
  .gnu.hash       : { *(.gnu.hash) }
  .dynsym         : { *(.dynsym) }
  .dynstr         : { *(.dynstr) }
  .gnu.version    : { *(.gnu.version) }
  .gnu.version_d  : { *(.gnu.version_d) }
  .gnu.version_r  : { *(.gnu.version_r) }
  .rela.dyn       :
    {
      *(.rela.init)
      *(.rela.text .rela.text.* .rela.gnu.linkonce.t.*)
      *(.rela.fini)
      *(.rela.rodata .rela.rodata.* .rela.gnu.linkonce.r.*)
      *(.rela.data .rela.data.* .rela.gnu.linkonce.d.*)
      *(.rela.tdata .rela.tdata.* .rela.gnu.linkonce.td.*)
      *(.rela.tbss .rela.tbss.* .rela.gnu.linkonce.tb.*)
      *(.rela.ctors)
      *(.rela.dtors)
      *(.rela.got)
      *(.rela.bss .rela.bss.* .rela.gnu.linkonce.b.*)
      *(.rela.ldata .rela.ldata.* .rela.gnu.linkonce.l.*)
      *(.rela.lbss .rela.lbss.* .rela.gnu.linkonce.lb.*)
      *(.rela.lrodata .rela.lrodata.* .rela.gnu.linkonce.lr.*)
      *(.rela.ifunc)
    }
  .rela.plt       :
    {
      *(.rela.plt)
      PROVIDE_HIDDEN (__rela_iplt_start = .);
      *(.rela.iplt)
      PROVIDE_HIDDEN (__rela_iplt_end = .);
    }
  .init           :
  {
    KEEP (*(SORT_NONE(.init)))
  }
  .plt            : { *(.plt) *(.iplt) }
.plt.got        : { *(.plt.got) }
.plt.sec        : { *(.plt.sec) }
  .text           :
  {
    *(.text.unlikely .text.*_unlikely .text.unlikely.*)
    *(.text.exit .text.exit.*)
    *(.text.startup .text.startup.*)
    *(.text.hot .text.hot.*)
    *(.text .stub .text.* .gnu.linkonce.t.*)
    /* .gnu.warning sections are handled specially by elf32.em.  */
    *(.gnu.warning)
  }
  .fini           :
  {
    KEEP (*(SORT_NONE(.fini)))
  }
  PROVIDE (__etext = .);
  PROVIDE (_etext = .);
  PROVIDE (etext = .);
  .rodata         : { *(.rodata .rodata.* .gnu.linkonce.r.*) }
  .rodata1        : { *(.rodata1) }
  .eh_frame_hdr : { *(.eh_frame_hdr) *(.eh_frame_entry .eh_frame_entry.*) }
  .eh_frame       : ONLY_IF_RO { KEEP (*(.eh_frame)) *(.eh_frame.*) }
  .gcc_except_table   : ONLY_IF_RO { *(.gcc_except_table
  .gcc_except_table.*) }
  .gnu_extab   : ONLY_IF_RO { *(.gnu_extab*) }
  /* These sections are generated by the Sun/Oracle C++ compiler.  */
  .exception_ranges   : ONLY_IF_RO { *(.exception_ranges
  .exception_ranges*) }
  /* Adjust the address for the data segment.  We want to adjust up to
     the same address within the page on the next page up.  */
  . = DATA_SEGMENT_ALIGN (CONSTANT (MAXPAGESIZE), CONSTANT (COMMONPAGESIZE));
  /* Exception handling  */
  .eh_frame       : ONLY_IF_RW { KEEP (*(.eh_frame)) *(.eh_frame.*) }
  .gnu_extab      : ONLY_IF_RW { *(.gnu_extab) }
  .gcc_except_table   : ONLY_IF_RW { *(.gcc_except_table .gcc_except_table.*) }
  .exception_ranges   : ONLY_IF_RW { *(.exception_ranges .exception_ranges*) }
  /* Thread Local Storage sections  */
  .tdata	  : { *(.tdata .tdata.* .gnu.linkonce.td.*) }
  .tbss		  : { *(.tbss .tbss.* .gnu.linkonce.tb.*) *(.tcommon) }
  .preinit_array     :
  {
    PROVIDE_HIDDEN (__preinit_array_start = .);
    KEEP (*(.preinit_array))
    PROVIDE_HIDDEN (__preinit_array_end = .);
  }
  .init_array     :
  {
    PROVIDE_HIDDEN (__init_array_start = .);
    KEEP (*(SORT_BY_INIT_PRIORITY(.init_array.*) SORT_BY_INIT_PRIORITY(.ctors.*)))
    KEEP (*(.init_array EXCLUDE_FILE (*crtbegin.o *crtbegin?.o *crtend.o *crtend?.o ) .ctors))
    PROVIDE_HIDDEN (__init_array_end = .);
  }
  .fini_array     :
  {
    PROVIDE_HIDDEN (__fini_array_start = .);
    KEEP (*(SORT_BY_INIT_PRIORITY(.fini_array.*) SORT_BY_INIT_PRIORITY(.dtors.*)))
    KEEP (*(.fini_array EXCLUDE_FILE (*crtbegin.o *crtbegin?.o *crtend.o *crtend?.o ) .dtors))
    PROVIDE_HIDDEN (__fini_array_end = .);
  }
  .ctors          :
  {
    /* gcc uses crtbegin.o to find the start of
       the constructors, so we make sure it is
       first.  Because this is a wildcard, it
       doesn't matter if the user does not
       actually link against crtbegin.o; the
       linker won't look for a file to match a
       wildcard.  The wildcard also means that it
       doesn't matter which directory crtbegin.o
       is in.  */
    KEEP (*crtbegin.o(.ctors))
    KEEP (*crtbegin?.o(.ctors))
    /* We don't want to include the .ctor section from
       the crtend.o file until after the sorted ctors.
       The .ctor section from the crtend file contains the
       end of ctors marker and it must be last */
    KEEP (*(EXCLUDE_FILE (*crtend.o *crtend?.o ) .ctors))
    KEEP (*(SORT(.ctors.*)))
    KEEP (*(.ctors))
  }
  .dtors          :
  {
    KEEP (*crtbegin.o(.dtors))
    KEEP (*crtbegin?.o(.dtors))
    KEEP (*(EXCLUDE_FILE (*crtend.o *crtend?.o ) .dtors))
    KEEP (*(SORT(.dtors.*)))
    KEEP (*(.dtors))
  }
  .jcr            : { KEEP (*(.jcr)) }
  .data.rel.ro : { *(.data.rel.ro.local* .gnu.linkonce.d.rel.ro.local.*) *(.data.rel.ro .data.rel.ro.* .gnu.linkonce.d.rel.ro.*) }
  .dynamic        : { *(.dynamic) }
  .got            : { *(.got) *(.igot) }
  . = DATA_SEGMENT_RELRO_END (SIZEOF (.got.plt) >= 24 ? 24 : 0, .);
  .got.plt        : { *(.got.plt)  *(.igot.plt) }
  .data           :
  {
    *(.data .data.* .gnu.linkonce.d.*)
    SORT(CONSTRUCTORS)
  }
  .data1          : { *(.data1) }
  _edata = .; PROVIDE (edata = .);
  . = .;
  __bss_start = .;
  .bss            :
  {
   *(.dynbss)
   *(.bss .bss.* .gnu.linkonce.b.*)
   *(COMMON)
   /* Align here to ensure that the .bss section occupies space up to
      _end.  Align after .bss to ensure correct alignment even if the
      .bss section disappears because there are no input sections.
      FIXME: Why do we need it? When there is no .bss section, we don't
      pad the .data section.  */
   . = ALIGN(. != 0 ? 64 / 8 : 1);
  }
  .lbss   :
  {
    *(.dynlbss)
    *(.lbss .lbss.* .gnu.linkonce.lb.*)
    *(LARGE_COMMON)
  }
  . = ALIGN(64 / 8);
  . = SEGMENT_START("ldata-segment", .);
  .lrodata   ALIGN(CONSTANT (MAXPAGESIZE)) + (. & (CONSTANT (MAXPAGESIZE) - 1)) :
  {
    *(.lrodata .lrodata.* .gnu.linkonce.lr.*)
  }
  .ldata   ALIGN(CONSTANT (MAXPAGESIZE)) + (. & (CONSTANT (MAXPAGESIZE) - 1)) :
  {
    *(.ldata .ldata.* .gnu.linkonce.l.*)
    . = ALIGN(. != 0 ? 64 / 8 : 1);
  }
  . = ALIGN(64 / 8);
  _end = .; PROVIDE (end = .);
  . = DATA_SEGMENT_END (.);
  /* Stabs debugging sections.  */
  .stab          0 : { *(.stab) }
  .stabstr       0 : { *(.stabstr) }
  .stab.excl     0 : { *(.stab.excl) }
  .stab.exclstr  0 : { *(.stab.exclstr) }
  .stab.index    0 : { *(.stab.index) }
  .stab.indexstr 0 : { *(.stab.indexstr) }
  .comment       0 : { *(.comment) }
  /* DWARF debug sections.
     Symbols in the DWARF debugging sections are relative to the beginning
     of the section so we begin them at 0.  */
  /* DWARF 1 */
  .debug          0 : { *(.debug) }
  .line           0 : { *(.line) }
  /* GNU DWARF 1 extensions */
  .debug_srcinfo  0 : { *(.debug_srcinfo) }
  .debug_sfnames  0 : { *(.debug_sfnames) }
  /* DWARF 1.1 and DWARF 2 */
  .debug_aranges  0 : { *(.debug_aranges) }
  .debug_pubnames 0 : { *(.debug_pubnames) }
  /* DWARF 2 */
  .debug_info     0 : { *(.debug_info .gnu.linkonce.wi.*) }
  .debug_abbrev   0 : { *(.debug_abbrev) }
  .debug_line     0 : { *(.debug_line .debug_line.* .debug_line_end ) }
  .debug_frame    0 : { *(.debug_frame) }
  .debug_str      0 : { *(.debug_str) }
  .debug_loc      0 : { *(.debug_loc) }
  .debug_macinfo  0 : { *(.debug_macinfo) }
  /* SGI/MIPS DWARF 2 extensions */
  .debug_weaknames 0 : { *(.debug_weaknames) }
  .debug_funcnames 0 : { *(.debug_funcnames) }
  .debug_typenames 0 : { *(.debug_typenames) }
  .debug_varnames  0 : { *(.debug_varnames) }
  /* DWARF 3 */
  .debug_pubtypes 0 : { *(.debug_pubtypes) }
  .debug_ranges   0 : { *(.debug_ranges) }
  /* DWARF Extension.  */
  .debug_macro    0 : { *(.debug_macro) }
  .debug_addr     0 : { *(.debug_addr) }
  .gnu.attributes 0 : { KEEP (*(.gnu.attributes)) }
  /DISCARD/ : { *(.note.GNU-stack) *(.gnu_debuglink) *(.gnu.lto_*) }
}


==================================================

```

##### 修改文件

首先我们需要将默认文件的首尾“==================================================”包含这一行要删除，不然会报格式错误

```bash
/usr/bin/ld:test.lds:1: syntax error

collect2: error: ld returned 1 exit status
```

然后选择在“__bss_start”前添加我们自己的段

```
  .data1          : { *(.data1) }
  _edata = .; PROVIDE (edata = .);
  . = .;

   ha_start = .;/* 获取当前的地址赋值给__init_start,在源码中有使用到，指向“.application_init”段的起始地址 */
  handler  : { *(hander) }/* 将“.application_init”的所有内容放在这一段 */
  ha_end = .;/* 获取当前的地址赋值给__init_end,表示“.application_init”段的结束地址 */

  __bss_start = .;
  .bss            :
  {
```

##### 测试源码

**section.h**

```c
#include <stdio.h>

#include <string.h>

struct handler_info
{
    const char *name;
    int (*handler)(void);
};

#define HANDLER(name_)                                           \
    static int handler_##name_(void);                            \
    //放入handler
    static const struct handler_info                             \
        __attribute__((used, section("handler")))                \
            handler_info_##name_ = {.name = #name_,              \
                                    .handler = handler_##name_}; \
    static int handler_##name_()

struct handler_info ha_start; //段"hander"的起始地址，在*.lds文件中定义

struct handler_info ha_end; //段"hander"的末尾地址，在*.lds文件中定义
```

**section.c**

```c
#include "section.h"
HANDLER(file)
{

    printf("execute funtion : %s\n", __FUNCTION__);
    printf("Do you want to open a file\n");

    return 0;
}

HANDLER(test)
{

    printf("execute funtion : %s\n", __FUNCTION__);
    printf("What do you want to test\n");

    return 0;
}

HANDLER(lable)
{

    printf("execute funtion : %s\n", __FUNCTION__);
    printf("There are no labels here\n");

    return 0;
}

```

**main.c**

```c
#include "section.h"
#include <string.h>
int main(int argc, char **argv)
{
    struct handler_info *pf = &ha_start;
    int j, i;
    j = i = &ha_end - &ha_start;

    //添加多少个hander就测试多少次
    do
    {
        int g = 0;
        //字符不要输入太多,用于测试,不要超过12个字符
        char str[12] = {0};
        scanf("%s", str);

        //遍历查找
        for (g; g < j; g++)
        {
            if (0 == strcmp((pf + g)->name, str))
            {
                (pf + g)->handler();
                break;
            }
        }
        i--;
    } while (i);
    return 0;
}
```

**编译**运行

```bash
~ gcc section.c section.h main.c -Tmain.lds
~ ./a.out
```

##### 运行现象

```bash
#终端输入file
file
execute funtion : handler_file
Do you want to open a file
#终端输入test
test
execute funtion : handler_test
What do you want to test
#终端输入lable
lable
execute funtion : handler_lable
There are no labels here

```

可以看到当输入指定的字符时会调用到指定的函数

