# Question
```
1.Assuming that the following JOS kernel code is correct, what type should variable x have, uintptr_t or physaddr_t?
    mystery_t x;
    char* value = return_a_pointer();
    *value = 10;
    x = (mystery_t) value;
```
������mystery_t��uintptr_t

```
2.What entries (rows) in the page directory have been filled in at this point? 
What addresses do they map and where do they point? 
In other words, fill out this table as much as possible:
   Entry    Base Virtual Address        Points to (logically):
   1023     0xffc0000                   page table for top 4MB of phys memory
   1022     0xff80000                   page table for 248MB--(252MB-1) phys mem
   ��        ��                           page table for ... phys mem
   960      0xf000000(KERNBASE)         page table for kernel code & static data 0--(4MB-1) phys mem
   959      0xefc0000(VPT)              page directory self(kernel RW)
   958      0xef80000(ULIM)             page table for kernel stack
   957      0xef40000(UVPT)             same as 959 (user kernel R)
   956      0xef00000(UPAGES)           page table for struct Pages[]
   ...      ��                           NULL
   1        0x00400000                  NULL
   0        0x00000000                  same as 960 (then turn to NULL)
```

```
3.We have placed the kernel and user environment in the same address space. 
Why will user programs not be able to read or write the kernel's memory? 
What specific mechanisms protect the kernel memory?
```
����ҳĿ¼��ҳ���Ȩ��λPTE_U���û�̬�����޷���д�ں�̬�ڴ�
��MMU���������ַ���ں˵�ַ��ʱ������Ȩ��λPTE_U���Ӷ������ں�̬�ڴ�

```
4.What is the maximum amount of physical memory that this operating system can support? Why?
```
pages����ֻ��ռ�����4MB�Ŀռ䣬��ÿ��PageInfoռ��8Byte��Ҳ����˵���ֻ����512kҳ��ÿҳ����4kB���ܹ����2GB

```
5.How much space overhead is there for managing memory, if we actually had the maximum amount of physical memory? How is this overhead broken down?
```
���ﵽ��������ڴ�ʱ��1��ҳĿ¼��1024��ҳ���ڹ��������һ��(1024 + 1) * 4kB = 4100kB����Ҫ����pages������ռ�õ�4MB��һ��8196kB
��PTE_PS��λ��ʹ��ҳ���С��4K��Ϊ4M���ɼ��ٿ�֧

```
6.Revisit the page table setup in kern/entry.S and kern/entrypgdir.c. 
Immediately after we turn on paging, EIP is still a low number (a little over 1MB). 
At what point do we transition to running at an EIP above KERNBASE? 
What makes it possible for us to continue executing at a low EIP between when we enable paging and when we begin running at an EIP above KERNBASE? 
Why is this transition necessary?
```
��jmp֮��EIP��ֵ����KERNBASE���ϵģ���Ϊrelocated = 0xf010002f������֮ǰEIP = 0x10002d������֮��EIP = 0xf010002f
������kern/entrypgdir.c�н�0~4MB��KERNBASE~KERNBASE+4MB�������ַ��ӳ�䵽��0~4MB�������ַ�ϣ�������� EIP �ڸ�λ�͵�λ����ִ��
������ô������Ϊֻӳ���λ��ַ�Ļ�����ô�ڿ�����ҳ���Ƶ���һ�����ͻ�crash



