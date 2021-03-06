---
title: Java补码原码反码关联与计算关系
date: 2017-09-28 15:11:24
tags:
categories: Java
---
<img src="http://owiq5fnuk.bkt.clouddn.com/3.jpg"/>
由于想弄懂Java中的hashcode的生成算法，无奈源码中的算法几乎都用到了位运算，然而位运算的相关知识都忘光了，于是来重新温习一下，同时参网上的一些博客，得出了自己的一些领悟。<!--more-->

要熟悉位运算，首先要弄清楚原码、补码、反码这三个概念。二进制在内存的存在形式是补码，正数的补码、原码、反码是相同的，负数的反码是对其原码逐位取反，且最高位（也叫符号位，0正1负）不变，负数的补码是在其反码的末位加1。

那么根据上述规则进行位运算：

1.& （与运算）：两个相应的二进制位都为1，该位的结果值才为1，否则为0    （两真为真）

    如：2 & 10 =   00000000 00000000 00000000 00000010 & 00000000 00000000 00000000 00001010

               =   00000000 00000000 00000000 00000010

               =   2

2.| （或运算）：两个相应的二进制位只要有一个为1，该位的结果值为1，否则为0    （一真即真）

     如：2 | 10 =   00000000 00000000 00000000 00000010 & 00000000 00000000 00000000 00001010

                =   00000000 00000000 00000000 00001010

                =  10
3.^ （异或运算）：两个相应的二进制位相同，该位的结果值为0，否则为1    （相异为真）

     如：2 ^ 10 =  00000000 00000000 00000000 00000010 ^ 00000000 00000000 00000000 00001010

                =  00000000 00000000 00000000 00001000

                =  8

4.~（取反运算）重头戏来了（取反运算小窍门 ：~x = -(x+1)）：

正数取反：取反过程为 ：补码取反减1取反

      如 37：得到原码过程                              37   1

                                                       18   0

                                                        9    1

                                                        4    0

                                                        2     0

                                                        1      1

                        即二进制值为 100101  ，在java中int类型占4个字节，每个字节占8位，因此将其补齐为32位原码：

                                      00000000 00000000 00000000 00100101

              接着得到反码，与原码相同，即： 00000000 000000000 00000000 00100101

              接着得到补码，与原码相同，即：  00000000 000000000 00000000 00100101

    然后取反（得到的是新的负数的补码）：  11111111 11111111 11111111 11011010

    接着补码转反码（公式为 负数反码=补码的末位-1）：11111111 11111111 11111111 11011001

    接着反码转原码（除符号位其余位皆取反）：               10000000 00000000 00000000 00100110

     最后得出十进制值 = -(2+4+32)

                      = -38

负数取反：取反过程为 ：原码取反加1取反

     如  -38：得到原码过程                             -38    0

                                                       -19    1

                                                        -9     1

                                                        -4     0

                                                         -2     0

                                                         -1     1

                        得到二进制值为  100110，将其补齐为32位原码： 

                                         10000000 00000000 00000000 00100110

                         对原码取反：11111111 11111111 11111111 11011001

                         加1：              11111111 11111111 11111111 11011010

                         最后取反：00000000 00000000 00000000 00100101

                        转为十进制 = 1+4 +32

                                   = 37

                        


                        




