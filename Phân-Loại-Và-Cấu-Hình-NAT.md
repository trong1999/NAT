# NAT được chia ra làm 3 loại: Static NAT, dynamic NAT, NAT Overload
- Static NAT (one-to-one): 
  + <img src="https://www.totolink.vn/public/uploads/img_article/3loainatnetworkaddresstranslationbancanbietcauhinhstaticnat.png">
  + Static NAT là cách chuyển từ địa chỉ ip này ip kia một cách cố định, nghĩa là địa chỉ ánh xạ là địa chỉ cố định,
thường sẽ là địa chỉ của các server.
- Cấu hình static NAT
  + Thiết lập mối quan hệ chuyển đổi giữa server và mạng outside
  + **Router (config) # ip nat inside source static [local ip] [global ip]**
    + **Router (config) # ip nat inside source static 192.168.1.100 202.1.1.10**
  + Xác định cổng gateway trên router 
    + **Router (config) # interface fa0/0**
    + **Router (config-if) # ip nat inside**
  + Xác định cổng ra internet trên firewall
    + **Router (config) # interface se0/0/0**
    + **Router (config-if) # ip nat outside**
    
- Dynamic NAT (one-to-one): 
  + <img src="https://www.totolink.vn/public/uploads/img_article/3loainatnetworkaddresstranslationbancanbietdynamicnat.png">
  + Dynamic NAT là cách chuyển địa chỉ này sang địa chỉ khác, được thực hiện một cách tự động, các địa chỉ mạng cục bộ đều được
  ánh xạ đến bất kỳ địa chỉ nào đã được xác định ở outside.
 
- Cấu hình dynamic NAT
  + Cấu hình dải địa chỉ public: các địa chỉ NAT
    + **Router (config) # ip nat pool [name start ip] [name end ip] netmask [netmask]/prefix-lenght [prefix-lenght]**
    + **Router (config) # ip nat pool name 202.1.1.177 202.1.1.185 netmask 255.255.255.0**
  + Thiết lập access list các địa chỉ ở mạng cục bộ được chuyển đổi: địa chỉ được NAT
    + **Router (config) # access-list [access-list-number-permit] source [source-wildcard]**
    + **Router (config) # access-list 1 permit 192.168.1.0  0.0.0.255**
  + Xác định mối quan hệ giữa địa chỉ nguồn đã được cấu hình trong access list và dải địa chỉ outside 
    + **Router (config) # ip nat inside source list <acl-number> pool <name>**
    + **Router (config) # ip nat inside source list 1 pool name**
  + Xác định cổng gateway trên router 
    + **Router (config) # interface fa0/0**
    + **Router (config-if) # ip nat inside**
  + Xác định cổng ra internet trên firewall
    + **Router (config) # interface se0/0/0**
    + **Router (config-if) # ip nat outside**
    
 - NAT Overload (PAT: Port Address Translation and many-to-one):
  + <img src="https://www.totolink.vn/public/uploads/img_article/3loainatnetworkaddresstranslationbancanbietoverloadnat.png">
  + Nó là một dạng của Dynamic NAT, thay vì nó sử dụng nhiều địa chỉ public thì ở đây nó chỉ cần một địa chỉ, nhưng nó sẽ cho
  phép NAT trên các port, vậy số lượng port là bao nhiêu? Vì chỉ số cổng được mã hóa 16 bit nên số lượng cổng sẽ là 65536 port,
  thoải mái cho các địa chỉ cục bộ được ánh xạ.
  
- Cấu hình NAT Overload: 
  + Xác định access list mạng inside
    + **Router (config) # access-list <ACL-number> permit <source> <wildcard>**
  + Cấu hình chuyển địa chỉ ip sang cổng ra ngoài.
    + **Router (config) # ip nat inside source list <ACL-number> interface <interface> overload**
  + Xác định cổng gateway trên router 
    + **Router (config) # interface fa0/0**
    + **Router (config-if) # ip nat inside**
  + Xác định cổng ra internet trên firewall
    + **Router (config) # interface se0/0/0**
    + **Router (config-if) # ip nat outside**
    
 - Các lệnh kiểm tra cấu hình NAT:
  + Kiểm tra bảng NAT
    + **Router (config)# show ip nat translation**
  + Kiểm tra trạng thái của NAT
    + **Router (config)# show ip nat statistics**
    
