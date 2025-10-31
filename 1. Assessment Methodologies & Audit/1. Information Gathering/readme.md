# Information Gathering

Mục tiêu bài học:
- Phân biệt Active và Passive information gathering
- Thực hiện Active và Passive information gathering với nhiều tool

 Information gathering (Reconnaissance) là giai đoạn thu thập thông tin (giai đoạn đầu tiên) 

## Passive information gathering

Là việc thu thập thông tin mà không tương tác trực tiếp mục tiêu 

Ví dụ: thông qua internet để biết các thông tin như IP, host, website,...

**Các thông tin thu thập:**

- Địa chỉ IP, DNS, domain names and người sở hữu domain 
- Email, mạng xã hội 
- Web technologies, subdomains

Thu thập các thông tin cơ bản 
- ip https://search.dnslytics.com/
- web với `robots.txt`, `sitemap.xml` 
- wordpress có file backup: `wp-config.bak`
- Công nghệ sử dụng với extention Wappalyzer

### Whois Enumeration

Whois được sử dụng để xác định thông tin liên quan đến một tên miền cụ thể.

```bash
whois domain
whois ip
```

Hoặc sử dụng web [who.is](https://who.is/whois/hackersploit.org) hoặc [domaintools.com](https://whois.domaintools.com/)

### Website Footprinting with Netcraft

[Netcraft](https://sitereport.netcraft.com/?url=https://hackersploit.org) tổng hợp các thông tin trước đó, dễ đọc hơn


### DNS Reconnaissance

DNS Recon được sử dụng để xác định các bản ghi DNS liên kết với một tên miền, như bản ghi A, địa chỉ IP, IP máy chủ mail

```bash
dnsrecon -d domain
```

hoặc có thể dùng site [dnsdumpster](https://dnsdumpster.com/)

### WAF

Dùng để detect waf 

https://github.com/EnableSecurity/wafw00f

```bash
wafw00f domain

wafw00f domain -a (tìm tất cả waf match với signature)
```

### Subdomain Enumeration `Sublist3r`

Tìm subdomain

nó liệt kê các tên miền phụ bằng cách sử dụng các công cụ tìm kiếm (Google, Yaoo, Bing ...) và các công cụ khác (Netcraft, Virustotal, DNSdumpster, ReverseDNS, ThreatCrowd). -> nên nó là passive

```bash
sublist3r -d domain
sublist3r -d domain -e google,yahoo
sublist3r -d domain -o hs_sub_enum.txt
```

### Google Dorks

Các từ khóa để tìm kiếm chi tiết trên Google

`site:`

`inurl:`

`site:*.site.com` # tìm subdomain 

`intitle:`

`filetype:`

[Wayback Machine ](https://archive.org/web/)

### Thu thập email (Email Harvesting)

[theHarvester](https://github.com/laramies/theHarvester) 

- được sử dụng để liệt kê các email (tên, IP, URL, tên miền phụ) thuộc về một mục tiêu tên miền, bằng cách sử dụng các tài nguyên và cơ sở dữ liệu có sẵn công khai.

```bash
# Pre-installed on Kali Linux.
theHarvester -d <domain>
theHarvester -d <domain> -b dnsdumpster,duckduckgo,crtsh
theHarvester -d <domain> -b all
```

### Leaked Password Databases

https://haveibeenpwned.com/

- an toàn, đáng tin cậy, không cần đăng ký

- chèn email mục tiêu đã tìm thấy vào trang web để kiểm tra vi phạm dữ liệu

- đối với các email cũ hơn, nguy cơ phát hiện vi phạm dữ liệu cao hơn!


## Active Information Gathering


### DNS Zone Transfers

Các loại DNS phổ biến nhất:

| Loại bản ghi (Record Type) | Mô tả (Description) |
| -------------------------- | ---------------------------------- |
| **A**                      | Lưu trữ / phân giải địa chỉ **IPv4** của một tên miền hoặc hostname                                   |
| **AAAA**                   | Lưu trữ / phân giải địa chỉ **IPv6** của một tên miền hoặc hostname                                   |
| **CNAME**                  | Dùng cho **bí danh tên miền (alias)**, chuyển hướng một tên miền hoặc tên miền con sang tên miền khác |
| **MX**                     | Phân giải tên miền tới **máy chủ thư (mail server)**                                                  |
| **TXT**                    | Dùng cho **ghi chú quản trị**, thường được sử dụng cho **bảo mật email**                              |
| **NS**                     | Tham chiếu tới **máy chủ tên miền (Name Server)** của domain                                          |
| **SOA**                    | Lưu trữ **thông tin quản trị** của một tên miền (như **domain authority**)                            |
| **HINFO**                  | Thông tin về **máy chủ (Host Information)**                                                           |
| **SRV**                    | Bản ghi chỉ định các **dịch vụ cụ thể (Specific Services Records)**                                   |
| **PTR**                    | Phân giải **địa chỉ IP thành hostname** – dùng cho **truy vấn ngược (reverse lookup)**                |


tool `dnsenum` (có trên kali)

- liệt kê các bản ghi DNS công khai 
- thực hiện chuyển vùng DNS tự động 
- thực hiện tấn công DNS brute force trên các tên miền phụ

```bash
dnsenum zonetransfer.me
```

tool `dig` truy vấn máy chủ DNS


### Host Discovery with Nmap

nmap

[nmap cheat sheet](https://www.stationx.net/nmap-cheat-sheet/)

lab nmap



# Lab ctf 

1. robots.txt
2. `whatweb domain` hoặc `nmap -sCV -A -O target.ine.local`
3. `dirb domain`  (nếu muốn tìm đệ quy `dirb domain -w`)
4. file bak: `wp-config.bak`
5. `httrack domain` - tìm trong code tải về hoặc `grep -i "FLAG5" -R target.ine.local/`



















