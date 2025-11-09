# SSH

port 22

module: `auxiliary/scanner/ssh/ssh_version`

=> NÊN DÙNG ENUMUSERS trước để đỡ brute-force

module: `auxiliary/scanner/ssh/ssh_login` - `set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt` , `set PASS_FILE /usr/share/metasploit-framework/data/wordlists/common_passwords.txt`

sau khi lấy được username/pass

vào `sessions` để vào session đã đăng nhập vào ssh

module: `auxiliary/scanner/ssh/ssh_enumusers`

`find / -name "flag"`


















