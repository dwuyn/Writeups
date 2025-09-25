## Easy crack - 100 pts
```
❯ file Easy_CrackMe.exe 
Easy_CrackMe.exe: PE32 executable for MS Windows 4.00 (GUI), Intel i386, 4 sections
```

Running the file, we can see it is checking for a password.
<img width="485" height="159" alt="image" src="https://github.com/user-attachments/assets/215e2b4a-3f18-4ed0-823a-80f434c041ee" />

Open it with IDA, in the function `DialogFunc` we can see the `sub_401080` function.

```c
int __cdecl sub_401080(HWND hDlg)
{
  CHAR String[97]; // [esp+4h] [ebp-64h] BYREF
  __int16 v3; // [esp+65h] [ebp-3h]
  char v4; // [esp+67h] [ebp-1h]

  memset(String, 0, sizeof(String));
  v3 = 0;
  v4 = 0;
  GetDlgItemTextA(hDlg, 1000, String, 100);
  if ( String[1] != 97 || strncmp(&String[2], Str2, 2u) || strcmp(&String[4], aR3versing) || String[0] != 69 )
    return MessageBoxA(hDlg, aIncorrectPassw, Caption, 0x10u);
  MessageBoxA(hDlg, Text, Caption, 0x40u);
  return EndDialog(hDlg, 0);
}
```

We can see it is checking a string, using the char order we can get the password.
The pass is `Ea5yR3versing`.
