<h1>Cài DNS trên ubuntu 16.04</h1>

**Bước 1:** update và cài đặt bind9

	sudo apt-get update
	sudo apt-get install bind9 bind9utils bind9-doc
**Bước 2:** cài đặt file /etc/bind/named.conf.options
	
	 options {
   		 directory "/var/cache/bind";
   		 additional-from-auth no;
   		 additional-from-cache no;
   		 version "Bind Server";
   		 // If there is a firewall between you and nameservers you wan
   		 // to talk to, you may need to fix the firewall to allow multipl
   		 // ports to talk.  See http://www.kb.cert.org/vuls/id/800113
   		 // If your ISP provided one or more IP addresses for stable
   		 // nameservers, you probably want to use them as forwarders.
   		 // Uncomment the following block, and insert the addresses replacing
   		 // the all-0's placeholder.
   		 forwarders {
   		 8.8.8.8;
   		 8.8.4.4;
   		 };
   		 
   		 dnssec-validation auto;
   		 allow-recursion { 127.0.0.1; };
   		 auth-nxdomain no;    # conform to RFC1035
   		 listen-on-v6 { any; };
	};
	
	
	zone "huucuong.vn" {
 	type slave;
 	file "/var/cache/bind/huucuong.vn.db";
	 masters {192.168.20.100;};
	};
	
	zone "20.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/zones/db.20.168.192";
    allow-transfer { 192.168.20.100; };
	};
**Bước 3:** cấu hình file /etc/bind/named.conf.local

	zone "huucuong.vn" {
        type master;
        file "/etc/bind/zones/huucuong.vn.db";
        allow-transfer { 192.168.20.100; };
        also-notify { 192.168.20.100; };
	};
**Bước 4:** tạo file phân giải thuận
mkdir /etc/bind/zones
vi /etc/bind/zones/huucuong.vn.db

	$TTL	86400
	$ORIGIN huucuong.vn.
	@       IN      SOA     ns1.huucuong.vn. huucuong.vn. (
                              1         ; Serial
                          86400         ; Refresh
                           7200         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
	;
	@       IN      NS      ns1.huucuong.vn.
	@       IN      NS      ns2.huucuong.vn.
	ns1      IN      A       192.168.20.100
 
	;also list other computers
	@       IN      A       192.168.20.1 
	www     IN      A       192.168.20.1
	
**Bước 5:** tạo file phân giải ngược
vi /etc/bind/zones/db.20.168.192

	;
	; BIND reverse data file for local loopback interface
	;
	$TTL    604800
	@       IN      SOA     ns1.huucuong.vn. admin.huucuong.vn. (
                              3         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
	;
	; name servers - NS records
	      IN      NS      ns1.huucuong.vn.
	; PTR Records
	130   IN      PTR     ns1.huucuong.vn.    ; 192.168.20.100
	131 IN      PTR     host.huucuong.vn.  ; 192.168.20.1


