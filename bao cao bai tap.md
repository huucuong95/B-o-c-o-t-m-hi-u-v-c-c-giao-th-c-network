<html>
 <body>
  <h1 id="bao-cao-bai-tap">
   Báo cáo bài tập
  </h1>
  <h2 id="i-dhcp">
   I, DHCP
  </h2>
  <h3 id="1-inh-nghia">
   1, Định nghĩa
  </h3>
  <p>
   Dynamic Host Configuration Protocol (DHCP - giao thức cấu hình động máy chủ) là một giao thức cho phép cấp phát địa chỉ IP một cách tự động cùng với các cấu hình liên quan khác như subnet mark và gateway mặc định. Máy tính được cấu hình một cách tự động vì thế sẽ giảm việc can thiệp vào hệ thống mạng. Nó cung cấp một database trung tâm để theo dõi tất cả các máy tính trong hệ thống mạng. Mục đích quan trọng nhất là tránh trường hợp hai máy tính khác nhau lại có cùng địa chỉ IP.
  </p>
  <h3 id="2-nguyen-tac-hoat-ong">
   2, Nguyên tắc hoạt động
  </h3>
  <p>
   DHCP là một giao thức Internet có nguồn gốc ở BOOTP (bootstrap protocol), được dùng để cấu hình các Clients không đĩa. DHCP khai thác ưu điểm của giao thức truyền tin và các kỹ thuật khai báo cấu hình được định nghĩa trong BOOTP, trong đó có khả năng gán địa chỉ. Sự tương tự này cũng cho phép các bộ định tuyến hiện nay chuyển tiếp các thông điệp BOOTP giữa các mạng con cũng có thể chuyển tiếp các thông điệp DHCP. Vì thế, DHCP Server có thể đánh địa chỉ IP cho nhiều mạng con.
  </p>
  <h3 id="3-cac-buoc-hoat-ong">
   3, Các bước hoạt động
  </h3>
  <p>
  </p>
  <p align="center">
   <img src="https://github.com/huucuong95/B-o-c-o-t-m-hi-u-v-c-c-giao-th-c-network/blob/master/dhcp_process_explained.jpg">
  </p>
  <br/>
  <em>
   <br/>
   <strong>
    Bước 1:
   </strong>
   Máy trạm khởi động với “địa chỉ IP rỗng” cho phép liên lạc với DHCP Servers bằng giao thức TCP/IP. Nó broadcast một thông điệp DHCP Discover chứa địa chỉ MAC và tên máy tính để tìm DHCP Server .
   <br/>
   <strong>
    Bước 2:
   </strong>
   Nhiều DHCP Server có thể nhận thông điệp và chuẩn bị địa chỉ IP cho máy trạm. Nếu máy chủ có cấu hình hợp lệ cho máy trạm, nó gửi thông điệp “DHCP Offer” chứa địa chỉ MAC của khách, địa chỉ IP “Offer”, mặt nạ mạng con (subnet mask), địa chỉ IP của máy chủ và thời gian cho thuê đến Client. Địa chỉ “offer” được đánh dấu là “reserve” (để dành).
   <br/>
   <strong>
    Bước 3:
   </strong>
   Khi Client nhận thông điệp DHCP Offer và chấp nhận một trong các địa chỉ IP, Client sẽ gửi thông điệp DHCP Request để yêu cầu IP phù hợp cho DHCP Server thích hợp.
   <br/>
   <strong>
    Bước 4:
   </strong>
   Cuối cùng, DHCP Server khẳng định lại với Client bằng thông điệp DHCP Acknowledge.
   <br/>
  </em>
  <h3 id="4-mo-hinh-lab">
   4, Mô hình lab
  </h3>
  <p>
  </p>
  <p align="center">
   <img alt="" src="https://github.com/huucuong95/B-o-c-o-t-m-hi-u-v-c-c-giao-th-c-network/blob/master/Selection_013.png"/>
  </p>
  <h3 id="5-cau-hinh">
   5, Cấu hình
  </h3>
  <ul>
   <li>
    Tải DHCP: Bài hướng dẫn làm trên ubuntu 16.04
    <br/>
    root@cuong:~$ apt-get install isc-dhcp-server
   </li>
   <li>
    Vào file cấu hình:
    <br/>
    root@cuong:~$ vi /etc/dhcp/dhcpd.conf
   </li>
   <li>
    Sửa lại file configure cho phù hợp:
    <br/>
    option domain-name server2.cuong.vn;
    <br/>
    option domain-name-servers dhcp.server2.cuong.vn, 8.8.8.8, 8.8.4.4;
    <br/>
    authoritative;   // Uncomment
   </li>
   <li>
    Mô tả IP range:
    <br/>
    subnet 192.168.1.0 netmask 255.255.255.0 {
    <br/>
    option routers 192.168.1.254;   // gateway
    <br/>
    option subnet-mask 255.255.255.0;    // subnet
    <br/>
    range dynamic-bootp 192.168.1.100 192.168.1.200;  // IP range
    <br/>
    }
   </li>
   <li>
    Khởi động lại dịch vụ:
    <br/>
    root@cuong:~$ initctl start isc-dhcp-server
   </li>
  </ul>
  <h2 id="ii-dns">
   II, DNS
  </h2>
  <h3 id="1-inh-nghia_1">
   1, Định nghĩa
  </h3>
  <p>
   DNS là từ viết tắt trong tiếng Anh của Domain Name System, là Hệ thống phân giải tên được phát minh vào năm 1984 cho Internet, chỉ một hệ thống cho phép thiết lập tương ứng giữa địa chỉ IP và tên miền. Hệ thống tên miền (DNS) là một hệ thống đặt tên theo thứ tự cho máy vi tính, dịch vụ, hoặc bất kỳ nguồn lực tham gia vào Internet. Nó liên kết nhiều thông tin đa dạng với tên miền được gán cho những người tham gia. Quan trọng nhất là, nó chuyển tên miền có ý nghĩa cho con người vào số định danh (nhị phân), liên kết với các trang thiết bị mạng cho các mục đích định vị và địa chỉ hóa các thiết bị khắp thế giới.
  </p>
  <h3 id="2-nguyen-tac-hoat-ong_1">
   2,  Nguyên tắc hoạt động
  </h3>
  <p>
   Mỗi nhà cung cấp dịch vụ vận hành và duy trì DNS server riêng của mình, gồm các máy bên trong phần riêng của mỗi nhà cung cấp dịch vụ đó trong Internet. Tức là, nếu một trình duyệt tìm kiếm địa chỉ của một website thì DNS server phân giải tên website này phải là DNS server của chính tổ chức quản lý website đó chứ không phải là của một tổ chức (nhà cung cấp dịch vụ) nào khác.
  </p>
  <p>
   INTERNIC (Internet Network Information Center) chịu trách nhiệm theo dõi các tên miền và các DNS server tương ứng. INTERNIC là một tổ chức được thành lập bởi NFS (National Science Foundation), AT&amp;T và Network Solution, chịu trách nhiệm đăng ký các tên miền của Internet. INTERNIC chỉ có nhiệm vụ quản lý tất cả các DNS server trên Internet chứ không có nhiệm vụ phân giải tên cho từng địa chỉ.
  </p>
  <p>
   DNS có khả năng tra vấn các DNS server khác để có được một cái tên đã được phân giải. DNS server của mỗi tên miền thường có hai việc khác biệt. Thứ nhất, chịu trách nhiệm phân giải tên từ các máy bên trong miền về các địa chỉ Internet, cả bên trong lẫn bên ngoài miền nó quản lý. Thứ hai, chúng trả lời các DNS server bên ngoài đang cố gắng phân giải những cái tên bên trong miền nó quản lý. - DNS server có khả năng ghi nhớ lại những tên vừa phân giải. Để dùng cho những yêu cầu phân giải lần sau. Số lượng những tên phân giải được lưu lại tùy thuộc vào quy mô của từng DNS.
  </p>
  <h3 id="3-cac-buoc-hoat-ong_1">
   3, Các bước hoạt động
  </h3>
  <p>
   <em>
    <br/>
    <strong>
     Bước 1:
    </strong>
    Máy client đặt một truy vấn tìm một địa chỉ web với tên miền
    <a href="http://abc.com">
     abc.com
    </a>
    cho máy chủ DNS nội bộ.
    <br/>
    <strong>
     Bước 2:
    </strong>
    Nếu máy chủ DNS có chứa địa chỉ tên miền đó sẽ trả về kết quả cho client. Nếu máy chủ DNS nội bộ không biết IP của tên miền đó, nó sẽ gửi các truy vấn tới các máy chủ DNS lân cận để tìm ra địa chỉ IP tương ứng với tên miền đã nhận.
    <br/>
    <strong>
     Bước 3:
    </strong>
    Máy chủ DNS trả về kết quả cho client.
    <br/>
   </em>
  </p>
  <h4 id="vi-du">
   Ví dụ:
  </h4>
  <p>
  </p>
  <p align="center">
   <img alt="" src="https://github.com/huucuong95/B-o-c-o-t-m-hi-u-v-c-c-giao-th-c-network/blob/master/Recursive.jpg"/>
  </p>
  <p>
   1, Client yêu cầu máy chủ DNS cục bộ ‘dns.sjsu.edu’ để tìm kiếm một truy vấn DNS
   <a href="http://'mail.yahoo.com">
    'mail.yahoo.com
   </a>
   ‘ và cung cấp địa chỉ IP của nó.
  </p>
  <p>
   2, Máy chủ DNS cục bộ yêu cầu máy chủ DNS root cho địa chỉ IP của
   <a href="http://'mail.yahoo.com">
    'mail.yahoo.com
   </a>
   ‘
  </p>
  <p>
   3, Máy chủ DNS gốc tìm thấy hậu tố ‘com’ trong truy vấn và yêu cầu một trong những máy chủ DNS cấp cao chịu trách nhiệm về miền
   <a href='http://".com'>
    ".com
   </a>
   “
  </p>
  <p>
   4, Máy chủ cấp cao gửi yêu cầu tới máy chủ đầu cuối, máy chủ có thẩm quyền.
  </p>
  <p>
   5, Máy chủ DNS có thẩm quyền của Yahoo trả về địa chỉ IP cho máy chủ DNS cấp cao nhất truy cập DNS có thẩm quyền
  </p>
  <p>
   6, Máy chủ DNS cấp cao nhất trả về địa chỉ IP này tới máy chủ DNS gốc
  </p>
  <p>
   7, Máy chủ DNS gốc sẽ trả lại địa chỉ IP cho truy vấn DNS cục bộ.
  </p>
  <p>
   8, Máy client nhận được địa chỉ IP của truy vấn mong muốn của nó.
  </p>
  <h3 id="4-mo-hinh-lab_1">
   4, Mô hình lab
  </h3>
  <p>
  </p>
  <p align="center">
   <img alt="" src="https://github.com/huucuong95/B-o-c-o-t-m-hi-u-v-c-c-giao-th-c-network/blob/master/Selection_020.png"/>
  </p>
  <h3 id="5-cau-hinh_1">
   5, Cấu hình
  </h3>
  <ul>
   <li>
    Cài đặt DNS: bài hướng dẫn trên CentOS 6.8
    <br/>
    yum -y install bind caching-nameserver bind-chroot bind-utils
   </li>
   <li>
    Cấu hình file named.conf (nằm trong /var/named/chroot/etc/):
    <br/>
    acl mynet (
    <br/>
    192.168.20.0/24;
    <br/>
    127.0.0.1;
    <br/>
    )
    <br/>
    <img alt="" src="https://github.com/huucuong95/B-o-c-o-t-m-hi-u-v-c-c-giao-th-c-network/blob/master/Selection_024.png"/>
   </li>
   <li>
    Cấu hình phân giải thuận file /var/named/chroot/var/named/huucuong.vn:
    <br/>
    <img alt="" src="https://github.com/huucuong95/B-o-c-o-t-m-hi-u-v-c-c-giao-th-c-network/blob/master/Selection_025.png"/>
   </li>
   <li>
    Cấu hình file phân giải nghịch /var/named/chroot/var/named/20.168.192.in-addr.arpa.db
    <br/>
    <img alt="" src="https://github.com/huucuong95/B-o-c-o-t-m-hi-u-v-c-c-giao-th-c-network/blob/master/Selection_026.png"/>
   </li>
  </ul>
  <h2 id="iii-arp">
   III, ARP
  </h2>
  <h3 id="1-inh-nghia_2">
   1, Định nghĩa
  </h3>
  <p>
   Giao thức phân giải địa chỉ (Address Resolution Protocol hay ARP) là một giao thức truyền thông được sử dụng để chuyển địa chỉ từ tầng mạng (Internet layer) sang tầng liên kết dữ liệu theo mô hình OSI. Đây là một chức năng quan trọng trong giao thức IP của mạng máy tính. ARP được định nghĩa trong RFC 826 vào năm 1982, là một tiêu chuẩn Internet STD 37.
   <br/>
   ARP được sử dụng để từ một địa chỉ mạng (ví dụ một địa chỉ IPv4) tìm ra địa chỉ vật lý như một địa chỉ Ethernet (địa chỉ MAC), hay còn có thể nói là phân giải địa chỉ IP sang địa chỉ máy. ARP đã được thực hiện với nhiều kết hợp của công nghệ mạng và tầng liên kết dữ liệu, như IPv4, Chaosnet,..
   <br/>
   Trong mạng máy tính của phiên bản IPv6, chức năng của ARP được cung cấp bởi Neighbor Discovery Protocol (NDP).
  </p>
  <h3 id="2-nguyen-tac-hoat-ong_2">
   2,  Nguyên tắc hoạt động
  </h3>
  <h4 id="trong-mang-lan">
   Trong mạng LAN:
  </h4>
  <p>
   Khi một thiết bị mạng muốn biết địa chỉ MAC của một thiết bị mạng nào đó mà nó đã biết địa chỉ ở tầng network (IP, IPX…) nó sẽ gửi một ARP request bao gồm địa chỉ MAC address của nó và địa chỉ IP của thiết bị mà nó cần biết MAC address trên toàn bộ một miền broadcast. Mỗi một thiết bị nhận được request này sẽ so sánh địa chỉ IP trong request với địa chỉ tầng network của mình. Nếu trùng địa chỉ thì thiết bị đó phải gửi ngược lại cho thiết bị gửi ARP request một gói tin (trong đó có chứa địa chỉ MAC của mình).
  </p>
  <h4 id="trong-he-thong-mang-lan">
   Trong hệ thống mạng LAN:
  </h4>
  <p>
   Hoạt động của ARP trong một môi trường phức tạp hơn đó là hai hệ thống mạng gắn với nhau thông qua một Router C. Máy A thuộc mạng A muốn gửi gói tin đến máy B thuộc mạng B. Do các broadcast không thể truyền qua Router nên khi đó máy A sẽ xem Router C như một cầu nối hay một trung gian (Agent) để truyền dữ liệu. Trước đó, máy A sẽ biết được địa chỉ IP của Router C (địa chỉ Gateway)  và biết được rằng để truyền gói tin tới B phải đi qua C. Tất cả các thông tin như vậy sẽ được chứa trong một bảng gọi là bảng định tuyến (routing table). Bảng định tuyến theo cơ chế này được lưu giữ trong mỗi máy. Bảng định tuyến chứa thông tin về các Gateway để truy cập vào một hệ thống mạng nào đó.
  </p>
  <h3 id="3-cac-buoc-hoat-ong_2">
   3, Các bước hoạt động
  </h3>
  <h4 id="trong-mang-lan_1">
   Trong mạng LAN:
  </h4>
  <p>
  </p>
  <p align="center">
   <img alt="" src="https://github.com/huucuong95/B-o-c-o-t-m-hi-u-v-c-c-giao-th-c-network/blob/master/Selection_016.png"/>
  </p>
  <br/>
  <strong>
   Bước 1:
  </strong>
  máy A gửi ARP Request để hỏi xem máy nào có địa chỉ IP là x.x.x.x gửi theo kiểu Broadcast tới các máy trong mạng địa chỉ MAC đích là FF:FF:FF:FF:FF:FF
  <br/>
  <p align="center">
   <img alt="" src="https://github.com/huucuong95/B-o-c-o-t-m-hi-u-v-c-c-giao-th-c-network/blob/master/Selection_017.png"/>
  </p>
  <br/>
  <strong>
   Bước 2:
  </strong>
  Máy B nhận ra IP mà máy A đang tìm là IP của mình nên gửi lại gói ARP Reply, với nội dung gói có địa chỉ nguồn là MAC của B, đích là MAC của A.
  <br/>
  <strong>
   Bước 3:
  </strong>
  Máy A nhận được địa chỉ MAC của máy B lưu lại bảng ARP và bắt đầu quá trình trao đổi dữ liệu.
  <h4 id="trong-he-thong-mang-lan_1">
   Trong hệ thống mạng LAN
  </h4>
  <p>
  </p>
  <p align="center">
   <img alt="" src="https://github.com/huucuong95/B-o-c-o-t-m-hi-u-v-c-c-giao-th-c-network/blob/master/Selection_018.png"/>
  </p>
  <br/>
  <strong>
   Bước 1:
  </strong>
  Máy A sẽ truyền một ARP Request theo dạng Broadcast để tìm địa chỉ MAC của Port X
  <br/>
  <strong>
   Bước 2:
  </strong>
  Router C nhận được và cung cấp lại cho A một địa chỉ MAC của port X.
  <br/>
  <strong>
   Bước 3:
  </strong>
  Máy A truyền Packet đến port X của router C
  <br/>
  <strong>
   Bước 4:
  </strong>
  Router C nhận được packet. Trong Packet này chứa địa chỉ IP của máy B.
  <br/>
  <strong>
   Bước 5:
  </strong>
  Router C sẽ gửi ARP Request theo dạng Broadcast trong mạng của máy B để tìm địa chỉ MAC của B.
  <br/>
  <strong>
   Bước 6:
  </strong>
  Máy B nhận được và trả lời lại cho router biết địa chỉ MAC của mình.
  <br/>
  <strong>
   Bước 7:
  </strong>
  Sau khi nhận được địa chỉ MAC của máy B thì router sẽ gửi Packet đến máy B.
  <h2 id="iv-gre">
   IV, GRE
  </h2>
  <h3 id="1-inh-nghia_3">
   1, Định nghĩa
  </h3>
  <p>
   Generic Routing Encapsulation (GRE) là một trong những cơ chế đường hầm có sẵn sử dụng IP làm giao thức vận chuyển và có thể được sử dụng để thực hiện nhiều giao thức khác nhau. Đường hầm hoạt động như các liên kết điểm-điểm ảo có hai điểm cuối xác định bởi đường hầm và địa chỉ đích đường hầm tại mỗi điểm cuối.
  </p>
  <h3 id="2-nguyen-tac-hoat-ong_3">
   2,  Nguyên tắc hoạt động
  </h3>
  <p>
   Đặt gói tin IP vào một gói tin IP khác để di chuyển trong đường hầm.
  </p>
  <h3 id="3-cac-buoc-hoat-ong_3">
   3, Các bước hoạt động
  </h3>
  <p>
   <strong>
    Bước 1:
   </strong>
   Gói tin được gửi từ mạng 192.168.1.0/24 đi tới router 1
   <br/>
   <strong>
    Bước 2:
   </strong>
   Gói tin đi từ router 1 tới router 2 qua đường hầm ảo, lúc này gói tin sẽ được đóng gói lại vào một gói khác và được mã hoá. Khi tới đầu bên kia sẽ được mở ra.
   <br/>
   <strong>
    Bước 3:
   </strong>
   Router 2 gửi gói tin tới máy đích sau khi mở gói tin nhận từ tunner.
  </p>
  <h3 id="4-mo-hinh-lab_2">
   4, Mô hình lab
  </h3>
  <p>
  </p>
  <p align="center">
   <img alt="" src="https://raw.githubusercontent.com/huucuong95/B-o-c-o-t-m-hi-u-v-c-c-giao-th-c-network/master/Selection_002.bmp"/>
  </p>
  <h3 id="5-cau-hinh_2">
   5, Cấu hình
  </h3>
<p>Bước 1: Cấu hình địa chỉ như mô hình ở trên</p>
<p>Bước 2: Cấu hình GRE trên các Router</p>
<p>Router 1:</p>

```
 modprobe ip_gre
 sudo ip tunnel add gre1 mode gre remote 10.0.0.129 local 10.0.0.128 ttl 255
 sudo ip link set gre1 up
 ip addr add 20.0.0.1/24 dev gre1
```
Router 2:
```
modprobe ip_gre
sudo ip tunnel add gre1 mode gre remote 10.0.0.129 local 10.0.0.128 ttl 255
sudo ip link set gre1 up
ip addr add 20.0.0.1/24 dev gre1
Mở cở chế forward trên 2 router **echo 1 > /proc/sys/net/ipv4/ip_forward**
```
<p>Bước 3: Cấu hình đinh tuyến trên các thiết bị theo thứ tự<p>

COM 1
- Cấu hình định tuyến vào IP GRE TUNNEL và IP LAN của router 2

```
 ip router add 20.0.0.0/24 via 172.16.137.129 dev ens37
 ip route add 192.168.42.0/24 via 172.16.137.129 dev ens37
 ```

Kết qua ta có bảng định tuyến sau
![](https://github.com/kidluc/NETWORKREPORT/blob/master/Screenshot%20from%202017-09-21%2015-43-52.png)

COM 2
- Cấu hình định tuyến vào IP GRE TUNNEL và IP LAN của router 1
```
ip router add 20.0.0.0/24 via 192.168.42.128 dev ens33
ip route add 172.16.137.0/24 via 192.168.42.128 dev ens33
```
Kết qua ta có bảng định tuyến sau
![](https://github.com/kidluc/NETWORKREPORT/blob/master/Screenshot%20from%202017-09-21%2015-45-07.png)

Router 1
- Cấu hình định tuyến vào IP LAN của ROUTER-2
```
ip route add 192.168.42.0/24 via 20.0.0.2 dev gre1
```
Bảng định tuyến của Router 1
![](https://github.com/kidluc/NETWORKREPORT/blob/master/Screenshot%20from%202017-09-21%2015-44-04.png)

Router 2

- Cấu hình định tuyến vào IP LAN của Router 1
```
ip route add 172.16.137.0/24 via 20.0.0.1 dev gre1
```

Bảng định tuyến của Router 2
![](https://github.com/kidluc/NETWORKREPORT/blob/master/Screenshot%20from%202017-09-21%2015-44-11.png)


Sau khi cài đặt, ta có thể ping giữa COM 1 và COM 2 hoặc SSH.
PING COM 1 và COM 2
![](https://github.com/kidluc/NETWORKREPORT/blob/master/Screenshot%20from%202017-09-21%2015-42-28.png)
![](https://github.com/kidluc/NETWORKREPORT/blob/master/Screenshot%20from%202017-09-21%2016-10-09.png)

TCPDUMP trên 2 router SSH qua GRE Tunnel
![](https://github.com/kidluc/NETWORKREPORT/blob/master/Screenshot%20from%202017-09-21%2016-57-08.png)
![](https://github.com/kidluc/NETWORKREPORT/blob/master/Screenshot%20from%202017-09-21%2016-57-29.png)

SSH
![](https://github.com/kidluc/NETWORKREPORT/blob/master/Screenshot%20from%202017-09-21%2016-22-03.png)
  <h3 id="v-vxlan">
   V, VXLAN
  </h3>
  <h3 id="1-inh-nghia_4">
   1, Định nghĩa
  </h3>
  <p>
   VXLAN là một công nghệ ảo hóa hạ tầng mạng, mang tới hạ tầng mạng được trừu tượng hóa, linh động và có khả năng mở rộng trên toàn datacenter. VXLAN đem lại một cấu trúc có khả năng mở rộng cho các ứng dụng của Doanh nghiệp trên các cluster ( chứa nhiều server) và pod mà không cấu hình lại hạ tầng mạng vật lý. Nó sử dụng MAC-in-UDP để đóng gói.
   <br/>
   VXLAN giải quyết ba vấn đề chính:
   <br/>
   1. 16 triệu VNIs (broadcast domains) so với 4 nghìn được cung cấp bởi các VLAN truyền thống.
   <br/>
   2. Cho phép Layer 2 được mở rộng bất cứ nơi nào trong một mạng IP.
   <br/>
   3. Tối ưu hoá flooding.
  </p>
  <h3 id="2-nguyen-tac-hoat-ong_4">
   2,  Nguyên tắc hoạt động
  </h3>
  <p>
   Mỗi VTEP được cấu hình một cách độc lập. Để học được địa chỉ MAC của máy khác thì quá trình như sau:
   <br/>
   ●   Gửi gói ARP request, chưa biết unicast, multicast và traffic
   <br/>
   ●   Khám phá VTEPs từ xa
   <br/>
   ●   Tìm hiểu địa chỉ MAC máy chủ từ xa và ánh xạ MAC-to-VTEP cho từng phân khúc VXLAN
  </p>
  <h3 id="3-cac-buoc-hoat-ong_4">
   3, Các bước hoạt động
  </h3>
  <p>
   <strong>
    Bước 1:
   </strong>
   Host A gửi một ARP request cho IP host B, trên mạng VXLAN lớp 2 của A.
   <br/>
   <strong>
    Bước 2:
   </strong>
   VTEP-1 nhận được gói ARP từ A, nó vẫn chưa có địa chỉ B trong bảng của nó, nó gửi multicast IP tới tất cả các VXLAN mà nó biết. Gói tin gửi đi có địa chỉ nguồn là IP của VTEP-1, đích là VTEP-2
   <br/>
   <strong>
    Bước 3:
   </strong>
   VTEP-2 nhận được gói tin, nó mở gói tin ra kiểm tra VNID của nó trong VXLAN header. Nếu đúng nó sẽ thay đổi VNID và gửi các gói ARP trong mạng của nó, cùng với đó nó lưu lại MAC của máy A.
   <br/>
   <strong>
    Bước 4:
   </strong>
   Máy B nhận được ARP, nhận ra là IP của B nó gửi lại địa chỉ MAC của nó và lưu lại IP-A-to-MAC-A vào bảng của nó.
   <br/>
   <strong>
    Bước 5:
   </strong>
   VTEP-2 nhận được MAC của B, gửi ngược lại gói ARP reply cho VTEP-1. Gói này được đóng trong playload UDP
   <br/>
   <strong>
    Bước 6:
   </strong>
   VTEP-1 nhận được ARP reply từ VTEP-2, nó gửi trả lời lại cho host A đồng thời lưu địa chỉ B lại vào bảng của nó.
   <br/>
   <strong>
    Bước 7:
   </strong>
   Đường truyề giữa host A và host B được thiết lập, và tạo đường hầm VXLAN giữa chúng.
  </p>
  <h3 id="4-mo-hinh-lab_3">
   4, Mô hình lab
  </h3>
  <p>
  </p>
  <p align="center">
   <img alt="" src="https://github.com/huucuong95/B-o-c-o-t-m-hi-u-v-c-c-giao-th-c-network/blob/master/Selection_003.bmp"/>
  </p>
  <h3 id="5-cau-hinh_3">
   5, Cấu hình
  </h3>
Tương tự như cấu hình GRE ở trên
Đầu tiên cần tạo VXLAN trên 2 Router để kết nối 2 mạng LAN của 2 ROUTER với nhau.
Mở cơ chế forward trên 2 router **echo 1 > /proc/sys/net/ipv4/ip_forward** trên cả 2 router.

Router 1
Ta tạo interface vxlan0 với IP là 30.0.0.1
```
ip link add vxlan0 type vxlan id 100 dev ens33 local 10.0.0.128 remote 10.0.0.129 ttl 255 dstport 4789
ip link set vxlan0 up
ip addr add 30.0.0.1/24 dev vxlan0
```
Kiểm tra vxlan đã hoạt động chưa.
![](https://github.com/kidluc/NETWORKREPORT/blob/master/Screenshot%20from%202017-09-22%2007-51-09.png)

Cấu hình bảng định tuyến của router 1 để kết nối với COM-2 qua VXLAN.
```
ip route add 192.168.42.0/24 via 30.0.0.2 dev vxlan0
```
![](https://github.com/kidluc/NETWORKREPORT/blob/master/Screenshot%20from%202017-09-22%2007-54-25.png)

Router 2
```
ip link add vxlan0 type vxlan id 100 dev ens33 local 10.0.0.129 remote 10.0.0.128 ttl 255 dstport 4789
ip link set vxlan0 up
ip addr add 30.0.0.2/24 dev vxlan0
```
Kiểm tra vxlan đã hoạt động chưa.
![](https://github.com/kidluc/NETWORKREPORT/blob/master/Screenshot%20from%202017-09-22%2007-51-33.png)

Cấu hình bảng định tuyến của Router 2 để kết nối với COM 1 qua VXLAN.
```
ip route add 172.16.137.0/24 via 30.0.0.1 dev vxlan0
```
![](https://github.com/kidluc/NETWORKREPORT/blob/master/Screenshot%20from%202017-09-22%2007-55-28.png)

COM 1:
Ta cấu hình bảng định tuyến cho COM 1 kết nối đến Router 2 và COM 1 đến COM 2 qua VXLAN
```
ip route add 30.0.0.0/24 via 172.16.137.129 dev ens37
ip route add 192.168.42.0/24 via 172.16.137.129 dev ens37
```
![](https://github.com/kidluc/NETWORKREPORT/blob/master/Screenshot%20from%202017-09-22%2008-12-48.png)

COM 2:
Ta cấu hình bảng định tuyến cho COM 2 kết nối đến Router 1 qua VXLAN
```
ip route add 30.0.0.0/24 via 192.168.42.128 dev ens33
ip route add 172.16.137.0/24 via 192.168.42.128 dev ens33
```
![](https://github.com/kidluc/NETWORKREPORT/blob/master/Screenshot%20from%202017-09-22%2008-13-14.png)
Sau khi cấu hình ta có thể ping được từ COM 1 đến COM 2 qua VXLAN cũng như có thể SSH qua VXLAN.
![](https://github.com/kidluc/NETWORKREPORT/blob/master/Screenshot%20from%202017-09-22%2008-04-00.png)
`tcpdump -i vxlan0` trên Router 1
![](https://github.com/kidluc/NETWORKREPORT/blob/master/Screenshot%20from%202017-09-22%2008-05-16.png)

  <h2 id="vi-icmp">
   VI, ICMP
  </h2>
  <h3 id="1-inh-nghia_5">
   1, Định nghĩa
  </h3>
  <p>
   Giao thức ICMP( Internet control message protocol) là giao thức điều khiển của tầng IP, sử dụng để trao đổi các thông tin điều khiển dòng dữ liệu, thông báo lỗi và các thông tin trạng thái của bộ giao thức TCP/IP.
   <br/>
   Các loại thông điệp ICMP là các thông điệp ICMP được chia làm hai nhóm: các thông điệp truy vấn và các thông điệp thông báo lỗi. Các thông điệp truy vấn giúp người quản trị mạng nhận được thông tin xác định từ một node mạng khác. Các thông điệp thông báo lỗi liên quan đến các vấn đề mà bộ định tuyến hay trạm phát hiện ra khi xử lý gói IP. ICMP sử dụng địa chỉ IP nguồn để gửi thông điệp thông báo lppox cho node nguồn của gói IP.
  </p>
  <h3 id="2-nguyen-tac-hoat-ong_5">
   2,  Nguyên tắc hoạt động
  </h3>
  <ul>
   <li>
    Ping:
    <br/>
    Lệnh này dùng để kiểm tra một máy tính có thể kết nối với mạng không. Người sử dụng dùng cặp thông báo Echo Request và Echo Reply. Lệnh Ping sẽ gửi các gói tin từ máy tính của bạn đang ngồi tới máy tính đích. Thông qua giá trị mà máy đích trả về đối với từng gói tin, bạn có thể xác định được đường truyền như thế nào.
   </li>
   <li>
    Tracerout:
    <br/>
    Là công cụ dòng lệnh được dùng để xác định đường đi từ nguồn tới dích của một gói giao thức mạng Internet. Tracerout tìm đường tới đích bằng cách gửi các thông báo Echo Request Internet Mesage Protocol ICMP tới từng đích. Sau mỗi lần gặp đích, giá trị TTL tức là thời gian cần gửi sẽ tăng đến khi gặp đúng đích cần đến.
   </li>
  </ul>
  <h3 id="3-cac-buoc-hoat-ong_5">
   3, Các bước hoạt động
  </h3>
  <ul>
   <li>
    Tracerout: Máy A thực hiện lệnh Tracer tới máy B
    <br/>
    Bước 1: Máy A gửi ICMP với TTL=1 tới máy router 1 và nhận lại gói ICMP Echo Request vì khi đó TTL=0
    <br/>
    Bước 2: Máy A tiếp tục gửi gói ICMP với TTL=2 tới router 1, router 1 sẽ gửi router 2 với TTL=1, tương tự bước 1, máy 2 sẽ gửi lại gói ICMP Echo Request.
    <br/>
    Bước 3: Tương tự như trên máy A sẽ gửi tới được tất cả các router mà nó cần đi qua cho tới khi tới được máy B và nhận lại gói ICMP Echo, khi đó sẽ có được đường đi từ A tới B.
    <br/>
    Tracert đang được chạy trên máy chủ A, và được đi theo con đường đến host B. Tại Router 1 và Router 2, TTL được giảm đến 0, khiến mỗi router để gửi một thông điệp vượt quá ICMP Time. Khi ICMP Echo Request được nhận tại Host B, nó sẽ gửi lại một trả lời ICMP Echo.
   </li>
  </ul>
 </body>
</html>
