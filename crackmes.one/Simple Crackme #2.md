# Simple Crackme 2


- Author: EzDiaoL
- Language: C/C++
- Platform: Windows
- Architecture: x86-64

Link: [https://crackmes.one/crackme/671a4c3e9b533b4c22bd1bdd]()

ZIP Password: `crackmes.one`

## Basic Interaction

```cmd
& '.\simple crackme #2.exe'

Enter the password: aaaaaa
Wrong
```

## Solution

Search for string references in all modules in `x64dbg`

```
Address=00007FF62E341A15
Disassembly=lea rcx,qword ptr ds:[7FF62E34ACA8]
String Address=00007FF62E34ACA8
String="Enter the password: "

Address=00007FF62E341A88
Disassembly=lea rcx,qword ptr ds:[7FF62E34ACC8]
String Address=00007FF62E34ACC8
String="Correct"

Address=00007FF62E341A97
Disassembly=lea rcx,qword ptr ds:[7FF62E34ACD4]
String Address=00007FF62E34ACD4
String="Wrong"
```

After coming to the reference, we get this code.


```
00007FF62E341A15  | 48: | lea rcx,qword ptr ds:[7FF62E34ACA8]     | rcx:ZwQueryInformationThread+14, 00007FF62E34ACA8:"Enter the password: "
00007FF62E341A1C  | E8  | call simple crackme #2.7FF62E34119F     |
00007FF62E341A21  | 90  | nop                                     |
00007FF62E341A22  | 33C | xor ecx,ecx                             |
00007FF62E341A24  | FF1 | call qword ptr ds:[<__acrt_iob_func>]   |
00007FF62E341A2A  | 4C: | mov r8,rax                              |
00007FF62E341A2D  | BA  | mov edx,C                               | 0C:'\f'
00007FF62E341A32  | 48: | lea rcx,qword ptr ss:[rbp+8]            | rcx:ZwQueryInformationThread+14
00007FF62E341A36  | FF1 | call qword ptr ds:[<fgets>]             |
00007FF62E341A3C  | 90  | nop                                     |
00007FF62E341A3D  | 48: | lea rdx,qword ptr ds:[7FF62E34B2B8]     |
00007FF62E341A44  | 48: | lea rcx,qword ptr ss:[rbp+8]            | rcx:ZwQueryInformationThread+14
00007FF62E341A48  | FF1 | call qword ptr ds:[<strcspn>]           |
00007FF62E341A4E  | 48: | mov qword ptr ss:[rbp+138],rax          |
00007FF62E341A55  | 48: | cmp qword ptr ss:[rbp+138],C            | 0C:'\f'
00007FF62E341A5D  | 73  | jae simple crackme #2.7FF62E341A61      |
00007FF62E341A5F  | EB  | jmp simple crackme #2.7FF62E341A67      |
00007FF62E341A61  | E8  | call simple crackme #2.7FF62E3412B2     |
00007FF62E341A66  | 90  | nop                                     |
00007FF62E341A67  | 48: | mov rax,qword ptr ss:[rbp+138]          |
00007FF62E341A6E  | C64 | mov byte ptr ss:[rbp+rax+8],0           |
00007FF62E341A73  | 48: | lea rcx,qword ptr ss:[rbp+8]            | rcx:ZwQueryInformationThread+14
00007FF62E341A77  | E8  | call simple crackme #2.7FF62E3413ED     |
00007FF62E341A7C  | 894 | mov dword ptr ss:[rbp+54],eax           |
00007FF62E341A7F  | 817 | cmp dword ptr ss:[rbp+54],D1E7F089      |
00007FF62E341A86  | 75  | jne simple crackme #2.7FF62E341A97      |
00007FF62E341A88  | 48: | lea rcx,qword ptr ds:[7FF62E34ACC8]     | rcx:ZwQueryInformationThread+14, 00007FF62E34ACC8:"Correct"
00007FF62E341A8F  | E8  | call simple crackme #2.7FF62E34119F     |
00007FF62E341A94  | 90  | nop                                     |
00007FF62E341A95  | EB  | jmp simple crackme #2.7FF62E341AA4      |
00007FF62E341A97  | 48: | lea rcx,qword ptr ds:[7FF62E34ACD4]     | rcx:ZwQueryInformationThread+14, 00007FF62E34ACD4:"Wrong"
```

The `jne` standing for `Jump if not equals` on the address `00007FF62E341A86` is pointing to the address `00007FF62E341A97` which as x64dbg suggests, prints `Wrong` , and so if we want to print `Correct` , what to do?

We simply edit the code of the `jne` to `je` standing for `Jump if equals` , which simply reverses the above compare statement, and allows the code to go on our intended direction. Then we simply patch the file and save it as what we want.

### Patched File Output

```
.\cracked.exe

Enter the password:
Correct
```
