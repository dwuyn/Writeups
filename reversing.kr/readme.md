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

## Easy Keygen - 100 pts
```
❯ file Easy\ Keygen.exe 
Easy Keygen.exe: PE32 executable for MS Windows 4.00 (console), Intel i386, 3 sections
```

Checking Readme.txt:
```
ReversingKr KeygenMe


Find the Name when the Serial is 5B134977135E7D13
```

Checking main function:
```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  signed int v3; // ebp
  int i; // esi
  char v6; // [esp+Ch] [ebp-130h]
  _BYTE v7[2]; // [esp+Dh] [ebp-12Fh] BYREF
  char v8[100]; // [esp+10h] [ebp-12Ch] BYREF
  char Buffer[197]; // [esp+74h] [ebp-C8h] BYREF
  __int16 v10; // [esp+139h] [ebp-3h]
  char v11; // [esp+13Bh] [ebp-1h]

  memset(v8, 0, sizeof(v8));
  memset(Buffer, 0, sizeof(Buffer));
  v10 = 0;
  v11 = 0;
  v6 = 16;
  qmemcpy(v7, " 0", sizeof(v7));
  sub_4011B9(aInputName);
  scanf("%s", v8);
  v3 = 0;
  for ( i = 0; v3 < (int)strlen(v8); ++i )
  {
    if ( i >= 3 )
      i = 0;
    sprintf(Buffer, "%s%02X", Buffer, v8[v3++] ^ v7[i - 1]);
  }
  memset(v8, 0, sizeof(v8));
  sub_4011B9(aInputSerial);
  scanf("%s", v8);
  if ( !strcmp(v8, Buffer) )
    sub_4011B9(aCorrect);
  else
    sub_4011B9(aWrong);
  return 0;
}
```

This program inputs `name` and xor it with `v7` char by char.
```c
sprintf(Buffer, "%s%02X", Buffer, v8[v3++] ^ v7[i - 1]);
```

`v7` được khởi tạo với các giá trị:
<img width="453" height="130" alt="image" src="https://github.com/user-attachments/assets/9022a70a-3c01-490d-8b4b-db5abf0e2782" />

Script:
```py
serial = "5B134977135E7D13"
b = bytes.fromhex(serial)
v7 = [0x10, 0x20, 0x30]
for i in range(len(serial)//2):
    print(chr(int(b[i]) ^ v7[i%3]), end="")
```

```
K3yg3nm3
```

## Easy Unpack - 100 pts
```
❯ file Easy_UnpackMe.exe 
Easy_UnpackMe.exe: PE32 executable for MS Windows 4.00 (GUI), Intel i386, 5 sections
```

Checking Readme.txt:
```
ReversingKr UnpackMe


Find the OEP

ex) 00401000
```

The PE file is packed and cannot be read normally.
<img width="1267" height="671" alt="image" src="https://github.com/user-attachments/assets/c871bef0-4e57-4031-8d32-b2a505342cef" />

In a packed PE file, there is a Packed Data segment containing original source code. When the program runs, the Entry Point (EP) of the program will redirect into that source code segment. That is the Original Entry Point (OEP). It is the key to unpack the PE.

So before jumping to OEP, the unpacking routine is finished and in the end, all you need to do is find the part of the program that branches into OEP.

<img width="1003" height="548" alt="image" src="https://github.com/user-attachments/assets/82cda143-7c7e-454d-9bb0-63978ab7930e" />
<img width="947" height="673" alt="image" src="https://github.com/user-attachments/assets/f94fe1ed-b3d7-4fa1-b14f-cc904914d63c" />

We can see it jump direct to 0x401150, so I think this is the OEP.

OEP: `00401150`


