# Footprinting & Scanning

Mục đích của việc lập bản đồ mạng và quét cổng liên quan

Thực hiện khám phá máy chủ mạng và quét cổng

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



## Port Scanning

Mục đích của scan port là xác định các dịch vụ và hệ điều hành, để hiểu loại thiết bị nào được phát hiện (máy chủ, máy tính để bàn, thiết bị mạng, v.v.).

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



