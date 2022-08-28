# asm学习

![20220828_112458_20](image/20220828_112458_20.png)


```
Something I hope you know before go into the coding~
First, please watch or star this repo, I'll be more happy if you follow me.
Bug report, questions and discussion are welcome, you can post an issue or pull a request.
```



## 目录

* [Intel汇编](docs/Intel汇编.md)
* [AT&T汇编](docs/AT&T汇编.md)
* [体系架构](docs/体系架构.md)
    * [x86](docs/体系架构/x86.md)
    * [x64](docs/体系架构/x64.md)
    * [arm32](docs/体系架构/arm32.md)
    * [arm64](docs/体系架构/arm64.md)



## 划重点

**寄存器前缀**

* 在 AT&T 汇编格式中，**寄存器名要加上 '%' 作为前缀**；而在 Intel 汇编格式中，寄存器名不需要加前缀。

| AT&T 格式  | Intel 格式 |
| ---------- | ---------- |
| pushl %eax | push eax   |

**立即数**

* 在 AT&T 汇编格式中，用 '$' 前缀表示一个立即操作数；而在 Intel 汇编格式中，立即数的表示不用带任何前缀。

| AT&T 格式 | Intel 格式 |
| --------- | ---------- |
| push $100 | push 100   |


**操作数顺序**

* AT&T 和 Intel 格式中的源操作数和目标操作数的位置正好相反。
* 在 Intel 汇编格式中，目标操作数在源操作数的左边； 记: "a <- b, a = b"，这里intel汇编更符合常规编程赋值的习惯
* 而在 AT&T 汇编格式中，目标操作数在源操作数的右边  记: "a -> b"

| AT&T 格式       | Intel 格式   |
| --------------- | ------------ |
| push $100, %eax | push eax,100 |

**字长**

如何标记所需操作的目标操作数字长，一个是操作码中锚定，一个是操作数中锚定

* 在 AT&T 汇编格式中，操作数的字长由**操作符**（```操作符 操作数```）的最后一个字母决定；后缀'b'、'w'、'l'分别表示操作数为字节（byte，8 比特）、字（word，16 比特）和长字（long，32比特）；
* 而在 Intel 汇编格式中，操作数的字长是用 "byte ptr" 和 "word ptr" 等前缀来表示的。例如：

| AT&T 格式     | Intel 格式           |
| ------------- | -------------------- |
| movb val, %al | mov al, byte ptr val |

**绝对转移jump call调用指令前缀**


在AT&T汇编格式中，绝对转移和调用指令（jump/call）的操作数前要加上 * 作为前缀，而在Intel格式中则不需要。

```
$ is a prefix is for immediates (constants)
% prefix is for registers (they are required)
* indicates an absolute jump, in contrast with the absense of the asterisk meaning a relative jump.
```

| AT&T 格式  | Intel 格式 |
| ---------- | ---------- |
| jmpq *$r11 | jmpq $r11  |



**远程转移指令、远程子调用指令、调用返回指令**


| AT&T 格式               | Intel 格式              |
| ----------------------- | ----------------------- |
| ljump $section, $offset | jmp far section:offset  |
| lcall $section, $offset | call far section:offset |
| lret $stack_adjust      | ret far stack_adjust    |



**内存操作数的寻址方式**

| AT&T 格式                        | Intel 格式                          |
| -------------------------------- | ----------------------------------- |
| section:disp(base, index, scale) | section:[base + index*scale + disp] |



| AT&T 格式                      | Intel 格式                    |
| ------------------------------ | ----------------------------- |
| movl -4(%ebp), %eax            | mov eax, [ebp - 4]            |
| movl array(, %eax, 4), %eax    | mov eax, [eax*4 + array]      |
| movw array(%ebx, %eax, 4), %cx | mov cx, [ebx + 4*eax + array] |
| movb $4, %fs:(%eax)            | mov fs:eax, 4                 |



| Intel Code               | AT&T Code                      |
| ------------------------ | ------------------------------ |
| mov eax,1                | movl $1,%eax                   |
| mov ebx,0ffh             | movl $0xff,%ebx                |
| int 80h                  | int $0x80                      |
| mov ebx, eax             | movl %eax, %ebx                |
| mov eax,[ecx]            | movl (%ecx),%eax               |
| mov eax,[ebx+3]          | movl 3(%ebx),%eax              |
| mov eax,[ebx+20h]        | movl 0x20(%ebx),%eax           |
| add eax,[ebx+ecx*2h]     | addl (%ebx,%ecx,0x2),%eax      |
| lea eax,[ebx+ecx]        | leal (%ebx,%ecx),%eax          |
| sub eax,[ebx+ecx*4h-20h] | subl -0x20(%ebx,%ecx,0x4),%eax |








---
