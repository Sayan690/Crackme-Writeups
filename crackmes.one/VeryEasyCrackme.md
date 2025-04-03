# VeryEasyCrackme


Host OS: Windows x86-64

Author: `_int2eth`

Link: `https://crackmes.one/crackme/671f212f9b533b4c22bd1edd`

ZIP Password: `crackmes.one`

## Basic Interaction

```cmd
.\VeryEasyCrackme.exe

Digite a senha: somePassword1234@!
Sua senha esta errada
```

## Solution

```cmd
floss .\VeryEasyCrackme.exe --no static
```

Output:

```cmd
 ───────────────────────────
  FLOSS DECODED STRINGS (2)
 ───────────────────────────

xZkyy
YourPass
```
Use `YourPass` as password, and the program outputs it is a correct password.

```cmd
.\VeryEasyCrackme.exe

Digite a senha: YourPass
Sua senha esta correta
```
