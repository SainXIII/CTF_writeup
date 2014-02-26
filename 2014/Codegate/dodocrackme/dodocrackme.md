---
title: Codegate CTF Quals 2014 dodocrackmewriteup
author: adrian 
layout: post
permalink: /2014-02-25-codegate-ctf-quals-2014-dodocrackme--writeup/
comments: true
categories:
    - CTF 
    - writeup
---

This challenge is 200 points reverse.

Check this file.

    cuiqindeMacBook-Pro:dodocrackme adrian$ file crackme_d079a0af0b01789c01d5755c885da4f6 
    crackme_d079a0af0b01789c01d5755c885da4f6: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, stripped

Stripped x86_64 binary

run the program.

	root@kali:/tmp# ./crackme 
	root@localhost's password: AdrianC
	Permission denied (password).
	
I must find the password.

After load it in IDA Pro. I find someting like this.

    .text:00000000004001C0                 lea     rbp, [rbp-8]
    .text:00000000004001C4                 inc     byte ptr [rbp+0]
    .text:00000000004001C7                 lea     rbp, [rbp+8]
    .text:00000000004001CB                 lea     rbp, [rbp+8]
    .text:00000000004001CF                 inc     byte ptr [rbp+0]
    .text:00000000004001D2                 inc     byte ptr [rbp+0]
    .text:00000000004001D5                 inc     byte ptr [rbp+0]
    .text:00000000004001D8                 inc     byte ptr [rbp+0]
    .text:00000000004001DB                 inc     byte ptr [rbp+0]
    .text:00000000004001DE                 inc     byte ptr [rbp+0]
    .text:00000000004001E1                 inc     byte ptr [rbp+0]
    .text:00000000004001E4                 inc     byte ptr [rbp+0]
    .text:00000000004001E7                 inc     byte ptr [rbp+0]
    .text:00000000004001EA                 inc     byte ptr [rbp+0]
    .text:00000000004001ED                 inc     byte ptr [rbp+0]
    .text:00000000004001F0                 inc     byte ptr [rbp+0]
    .text:00000000004001F3                 inc     byte ptr [rbp+0]
    .text:00000000004001F6                 mov     al, [rbp+0]
    .text:00000000004001F9                 test    al, al
    .text:00000000004001FB                 jz      short loc_400276
    .text:00000000004001FD                 jmp     short $+2 

There are many look like this code.

and other team very quickly solved the problem.

I think the key without complex calculations. May be stored in memory.

the key is stored at the begining of the program of the last of program.

and the program is often modify a memory. and like this:

	.text:000000000040021B                 inc     byte ptr [rbp+0]
	.text:000000000040021E                 lea     rbp, [rbp+8]
	.text:0000000000400222                 inc     byte ptr [rbp+0]
	.text:0000000000400225                 inc     byte ptr [rbp+0]
	.text:0000000000400228                 inc     byte ptr [rbp+0]
	.text:000000000040022B                 inc     byte ptr [rbp+0]
	.text:000000000040022E                 inc     byte ptr [rbp+0]
	.text:0000000000400231                 lea     rbp, [rbp+8]
	.text:0000000000400235                 inc     byte ptr [rbp+0]
	.text:0000000000400238                 inc     byte ptr [rbp+0]
	.text:000000000040023B                 inc     byte ptr [rbp+0]
	.text:000000000040023E                 inc     byte ptr [rbp+0]
	.text:0000000000400241                 inc     byte ptr [rbp+0]
	.text:0000000000400244                 inc     byte ptr [rbp+0]
	.text:0000000000400247                 inc     byte ptr [rbp+0]
	.text:000000000040024A                 inc     byte ptr [rbp+0]
	.text:000000000040024D                 lea     rbp, [rbp+8]
	.text:0000000000400251                 inc     byte ptr [rbp+0]
	.text:0000000000400254                 inc     byte ptr [rbp+0]
	.text:0000000000400257                 inc     byte ptr [rbp+0] 

so I attach the process and check memory.

	adrian   5742  0.0  0.0    224     4 pts/4    S+   15:34   0:00 ./crackme
	root      5747  0.0  0.0   7768   860 pts/6    S+   15:34   0:00 grep crackme
	root@kali:/tmp# gdb -p 5742 -q
	Attaching to process 5742
	Reading symbols from /home/fanrong/computer/ctf2014/Codegate/crackme...(no 	debugging symbols found)...done.
	0x00000000004065c2 in ?? ()
	(gdb) set disassembly-flavor intel
	(gdb) display /i $rip
	1: x/i $rip
	=> 0x4065c2:	lea    rbp,[rbp-0x8]
	(gdb) x/100s %rbp
	....
	0x7f68ec595b58:	 "H"
	...
	0x7f68ec595b68:	 "4"
	...
	0x7f68ec595b78:	 "P"
	...
	0x7f68ec595b88:	 "P"
	...
	0x7f68ec595b98:	 "Y"
	...
	0x7f68ec595ba8:	 "_"
	...
	0x7f68ec595bb8:	 "C"
	...
	0x7f68ec595bc8:	 "0"
	...
	0x7f68ec595bd8:	 "D"
	...
	0x7f68ec595be8:	 "E"
	...
	0x7f68ec595bf8:	 "G"
	...
	0x7f68ec595c08:	 "a"
	...
	0x7f68ec595c18:	 "T"
	...
	0x7f68ec595c28:	 "E"
	...
	0x7f68ec595c38:	 "_"
	...
	0x7f68ec595c48:	 "2"
	...
	0x7f68ec595c58:	 "0"
	...
	0x7f68ec595c68:	 "1"
	...
	0x7f68ec595c78:	 "4"
	...
	0x7f68ec595c88:	 "_"
	...
	0x7f68ec595c98:	 "C"
	...
	0x7f68ec595ca8:	 "U"
	...
	0x7f68ec595cb8:	 "_"
	...
	0x7f68ec595cc8:	 "1"
	...
	0x7f68ec595cd8:	 "N"
	...
	0x7f68ec595ce8:	 "_"
	...
	0x7f68ec595cf8:	 "K"
	...
	0x7f68ec595d08:	 "0"
	...
	0x7f68ec595d18:	 "R"
	...
	0x7f68ec595d28:	 "E"
	...
	0x7f68ec595d38:	 "4"
	...
	
The key is *H4PPY_C0DEGaTE_2014_CU_1N_K0RE4*

	root@kali:/tmp# ./crackme 
	root@localhost's password: H4PPY_C0DEGaTE_2014_CU_1N_K0RE4
	SUCCESS
	
Done!!!