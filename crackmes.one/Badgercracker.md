# Badgercracker

- Author: Badgercracker
- Language: C/C++
- Platform: Windows
- Architecture: x86-64

Link: [Badgercracker](https://crackmes.one/crackme/678c9f854d850ac5f7dc5487)

ZIP Password: `crackmes.one`

## Basic Interaction

```
.\crackingupload.exe

Enter your license key: aaaaaa
Invalid license key. Exiting...
```

## Solution 1 [Patch Method]

Steps to reproduce:
1. Open the program in x64dbg
2. Search for string references in all modules, and search for `Invalid license` , and then open the disassembly.
3. See the attached disassembly here and locate it.
```
00007FF70CDB228A  | 84D | test bl,bl                                 |
00007FF70CDB228C  | 74  | je crackingupload.7FF70CDB229E             |
00007FF70CDB228E  | 48: | lea rdx,qword ptr ds:[7FF70CDB54C8]        | 00007FF70CDB54C8:"License key is valid. Welcome!\n"
00007FF70CDB2295  | E8  | call crackingupload.7FF70CDB2D10           |
00007FF70CDB229A  | 33D | xor ebx,ebx                                |
00007FF70CDB229C  | EB  | jmp crackingupload.7FF70CDB22AF            |
00007FF70CDB229E  | 48: | lea rdx,qword ptr ds:[7FF70CDB54E8]        | 00007FF70CDB54E8:"Invalid license key. Exiting...\n"
00007FF70CDB22A5  | E8  | call crackingupload.7FF70CDB2D10           |
```
Here, the `je` standing for `Jump if equals` of `00007FF70CDB228C` address can be updated to `jne` standing for `Jump if not equals` essentially reversing the logic.

4. Update the `je` instruction to `jne` and patch the file.
5. Run the patched application and enter whatever input you like.
```
.\patch1.exe

Enter your license key: aaaa
License key is valid. Welcome!
```

## Solution 2 [Static Analysis Method]

Steps to reproduce:
1. Open the file in any decompiler of your choice, in order to view to high level pseudocode. In my case, I have used IDA Free.
2. Decompile the `main` function, and you will be able to figure out the valid license key.
3. Run the program again and put the correct key in it, and you will be able to see the success message.
