# SMTP

Simple mail transfer protocol 

port 25, 465, 587

Không cần user/pass:

kết nối netcat:

`nc IP 25`

`VRFY admin@openmailbox.xyz` - xem 1 email có tồn tại ko 

kết nối telnet:

`telnet demo.ine.local 25`

`HELO attacker.xyz`

`EHLO attacker.xyz`


ENUM USER:

`smtp-user-enum -U /usr/share/commix/src/txt/usernames.txt -t demo.ine.local`


MSF:

module: `auxiliary/scanner/smtp/smtp_version`

module: `auxiliary/scanner/smtp/smtp_enum`

Send mail giả:

`sendemail -f admin@attacker.xyz -t root@openmailbox.xyz -s demo.ine.local -u Fakemail -m "Hi root, a fake from admin" -o tls=no`





















