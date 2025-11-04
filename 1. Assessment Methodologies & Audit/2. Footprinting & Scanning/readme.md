# Footprinting & Scanning

Mục đích của việc lập bản đồ mạng và quét cổng liên quan, dịch vụ đang chạy, version

Thực hiện khám phá máy chủ mạng và quét cổng

tool: NMAP

## Mapping a Network

### Mục đích

Xác định scope được pentest

### Quy trình 

#### Physical Access

- OSINT (Open Source Intelligence) - DNS records, websites, public IP addresses

- Social Engineering

- sniffing - (sau khi kết nối) đánh hơi lưu lượng mạng thông qua trinh sát thụ động và bắt gói tin
    thu thập địa chỉ IP và địa chỉ MAC để liệt kê thêm

- ARP (Address Resolution Protocol) - tận dụng bảng ARP và truyền thông phát sóng

- ICMP (Internet Control Message Protocol) - traceroute, ping

## Tools

### Wireshark

### arp-scan

`ip` - routing, network devices, interfaces and tunnels

```bash
ip -br -c a
# -br = brief
# -c  = color
```

`arp-scan` - gửi yêu cầu ARP đến các máy chủ mục tiêu và hiển thị phản hồi

```bash
sudo arp-scan -I eth1 192.168.31.0/24
```

### ping

`ping` - gửi ICMP ECHO_REQUEST đến các máy chủ mạng

```bash
ping 192.168.31.2
```

### fping

`fping` - gửi các gói ICMP ECHO_REQUEST đến nhiều máy chủ mạng

```bash
fping -I eth1 -g 192.168.31.0/24 -a
```

Khởi chạy `fping` mà không gặp lỗi "Host Unreachable"

```bash
fping -I eth1 -g 192.168.31.0/24 -a 2>/dev/null
```

### nmap

### zenmap

`zenmap` - GUI `nmap` chính thức


## Host Discovery

Ping Sweeps (ICMP Echo Request) : phương pháp này bị block bởi windows firewall

ARP Scanning: phương pháp này chỉ chạy được trong mạng cục bộ 

TCP SYN ping: 


### Lab

`ping -c 5 demo.ine.local` -> ko nhận được thông tin gì 

`nmap demo.ine.local` -> ko nhận được thông tin gì 

`nmap -Pn demo.ine.local` - `-Pn` (coi host mở, bỏ qua host discovery) và vẫn quét port -> nhận được các port mở (ko có port 443)

`nmap -Pn -p 443 demo.ine.local` -> quét vào port 443 và nhận được thông tin là bị filter nên ko quét tự động được nhưng vẫn mở 

`nmap -Pn -sV -p 80 demo.ine.local` - `-sV` để lấy thông tin version của service



## Port Scanning

Mục đích của scan port là xác định port mở, các dịch vụ và hệ điều hành, để hiểu loại thiết bị nào được phát hiện (máy chủ, máy tính để bàn, thiết bị mạng, v.v.).

### Operating System

Hệ điều hành được nhận biết thông qua chữ ký hoặc dịch vụ của nó.

### Services

Tìm services bằng cách kết nối tới các port và phân tích phản hồi.

Open Port

- SYN sent ➡️ SYN+ACK received ➡️ ACK sent

- Port is identified/open

- Close the connection with ➡️ RST+ACK sent

Closed Port

- SYN sent ➡️ RST+ACK received

- Port is closed

"Stealthy" Scan

- SYN sent ➡️ SYN+ACK received ➡️ RST sent

- Drops the connection after the received SYN+ACK

Service Version Scan

- SYN sent ➡️ SYN+ACK received ➡️ ACK sent ➡️ BANNER received ➡️ RST+ACK sent

- If BANNER received, the application will send back some information.

- "noisy" scan!


## NSE (Nmap script engine)

`ls -al /usr/share/nmap/scripts/`

`ls -al /usr/share/nmap/scripts/ | grep -e "http"` -> tìm kiếm trong đó

`nmap -T4 -SS -sV -sC -p- IP` -> `-sC` để chạy NSE

chạy script cụ hể 

`nmap -T4 -SS --script=tên -sC -p- IP`

`nmap -T4 -sS -A -p- IP` -> để tự động tất cả những thứ trên = -sV, -sC, --traceroute

### Lab 1

`nmap -sn IP/mask` -> tìm ip mở trong dãy IP nội bộ

`nmap -T4 -sS -p- IP` -> quét tất cả cổng TCP

`nmap -T4 -SS -sV -p- IP` -> tìm thêm thông tin service và version hoặc `nmap -T4 -SS -sV --version-intensity 8 -p- IP` để đoán 

`nmap -T4 -SS -sV -O -p- IP` -> tìm thêm signature của OS

`nmap -T4 -SS -sV -O --osscan-guess -p- IP` -> tìm thêm signature của OS


### Lab 2

`nmap demo.ine.local -p 1-250 -sU` quét cổng UDP

`nmap demo.ine.local -p 134,177,234 -sUV` quét version cho 3 port tìm được

`nmap demo.ine.local -p 134 -sUV --script=discovery` discovery từng port

`tftp demo.ine.local 134 ` kết nối ftp 

### lab 3 

`nmap demo.ine.local -T4 -p-` quét tất cả cổng TCP

-> ko có port nào mở

`nmap demo.ine.local -T4 -sU` quét cổng UDP

ta thấy có 1 cổng 161 mở, điều tra thêm về port này 

`nmap demo.ine.local -T4 -sU -p 161 -A`



## ctf

flag 1: `nmap -T4 -sS -A -p- IP` , đọc phản hồi có flag 1

flag 2: có cổng 80 vào web `/robots.txt`

có `/secret-info/` vào thì thấy có `flag.txt`

flag 3: ftp `ftp anonymous@192.82.246.3` đến password thì enter coi như ko có pass

Sau khi vaof ftp: 

`get flag.txt`

`get creds.txt`

flag 4: ta có username/pass của mysql trong `creds.txt`

`mysql -h IP -u username -p password`

Sau khi vào mysql:

`show DATABASES;`

-> có flag là tên database



## Firewall Detection & IDS Evasion

cách dùng Nmap phát hiện tường lựa trên máy chủ 

`-sA` - để biết port có filter hay ko

`-f` để phân mảnh gói tin, tránh IDS

`-D IP` IP nhập vào là IP khác với IP của mình, giả IP khi gửi nhằm đánh lạc hướng bên nhận được gói tin khi điều tra 


## Output Nmap

`-oN <name>.txt`

`-oX <name>.xml`









