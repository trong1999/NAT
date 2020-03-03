# Network Address Translation (NAT)
- Là việc thực hiện của các thiết bị mạng ở tầng firewall, từ một địa chỉ public, qua thiết bị mạng thì sẽ sẽ cung cấp các
địa chỉ mạng private cho zones như văn phòng, tòa nhà,...

<img src="https://cdn.whatismyipaddress.com/images-v4/nat.png">
- (inside là phần bên trái router; còn outside là phần bên phải router)

- Vấn đề đặt ra là địa chỉ nào là private or public:
  - Địa chỉ private:
    + 10.0.0.0 10.255.255.255
    + 172.16.0.0 172.31.255.255
    + 192.168.0.0 192.168.255.255
  - Các địa chỉ còn lại là public; và địa chỉ public này phải do các tổ chức có thẩm quyền cấp cho client.

- Ngoài nhu cầu làm việc ở inside thì client phải làm việc với file server, mail server,... đó là ngoài outside thì
phải làm cách nào ? Đơn giản chỉ cần cung cấp ip public cho client thì vấn đề đã được giải quyết. Hoạt động này được router
đảm nhiệm, nó sẽ định tuyến traffic giữa inside và outside.

- Quá trình làm việc này có thể phức tạp, nhưng người dùng hầu như không cảm nhận được có điều gì vướng mắc với transaction
của họ, vì process diễn ra rất nhanh. Việc NAT các địa chỉ client được thực hiện khi ta chia VLANs, mỗi một VLAN có một gatewayh
được định tuyến ra outside.

- Khi client request thì router xác định xem request này có phải phục vụ nhu cầu trong mạng cục bộ hay không? Nếu không thì nó sẽ gửi request đó ra ngoài outside và transaction thành công khi response được trả về OK.

- Cách này được dùng để kiểm tra và theo dõi các connection, các thông số có liên quan của ips client, không những thế, việc
sử dụng cách này thì tầng firewall còn giúp mạng cục bộ tránh được một số cuộc tấn công từ outside vào.

## Kết luận: NAT đơn giản là hình thức chuyển từ một địa chỉ mạng này sang các địa chỉ mạng kh

  
