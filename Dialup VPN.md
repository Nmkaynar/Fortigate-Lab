## Dialup VPN

Bu lab çalışmasında kullanıcıların dışardan şirket ortamına erişebilmeleri için vpn yapılandıracağız.


<img width="790" height="615" alt="image" src="https://github.com/user-attachments/assets/e691d585-c3de-4a14-995e-9451e7e04a8c" />

Sağ taraftaki kullanıcı ev ortamından forticlient ile şirket ortamındaki client1 makinesine erişim sağlayacak.

Bunun için Dialup Ipsec yapılandıracağız

## FW tarafı
 ### Dİalup VPN
1. Remote gateway olarak dialup user seçeceğiz. Sebebi de userların sabit bir ip'den bağlanmayacak olmaları
2. Mode configi aktif edip, kullanıcılara vpn ile bağlandıktan sonra ip almaları için ip range ni belirliyoruz
3. Burada vpn tüneli için parolo belirliyoruz
4. ike versionu belirleyip any peer id seçiyoruz.
5. Burada iki adet seçmemiz gerekiyor. Çünkü forticlient iki tane istiyor. Demo ürün olduğundan DES seçtik ancak best practice için AES128 veya AES 256 seçilmeli
6. DH grubunu 14 seçtik
7. XAUTH kısmını auto server yapıp policy seçiyoruz. Burada amacaımız vpnuser grubu belirleyip bunu policy de source olarak belirteceğiz. Bir userın vpn yapabilmesi için bu gruba dahail olması veya vpn yapmaması için de bu gruptan çıkartılması yeterli olacaktır. Bu lab çalışması ortamında AD olmadığndan kullanıcıları FW tanımlayacağız.
8. Burayı 0.0.0.0/0 olarak bırakıyoruz
9. Phase 1 deki gibi yine encryption metodlarını belirliyoruz.
10. DH 14 seçiyoruz. 

<img width="598" height="827" alt="image" src="https://github.com/user-attachments/assets/fc00671b-8548-49cd-bd82-d314d1dd2b95" />


<img width="580" height="563" alt="image" src="https://github.com/user-attachments/assets/5bacae22-0c58-4d07-b1d3-ed6f9715c478" />


<img width="581" height="663" alt="image" src="https://github.com/user-attachments/assets/f05ad3f6-25af-4d7c-9747-8aa7e7f460e9" />

### User ve User Group

Bu lab çalışması için bir tane Test user oluşturup DialUpVpn Grubuna aldık

<img width="1301" height="45" alt="image" src="https://github.com/user-attachments/assets/7209212c-46f3-4cfb-8bf6-0fcaf177dd69" />

### Policy

Policy yazarken
incoming interface olarak ipsec interface'i, outgoing olarak hangi porttaki LAN tarafına erişim sağlayacak ise o portu seçeceğiz
Source olarak all(userların ip'leri belli değil) ve vpn user grubu seçilmeli. 
Destination ve service olarak lab olduğundan all verildi. Hangi ip'lere erişim sağlanacak ise adres grubu ile belirtilebilir. Service içinde hangi serviclere verileek ise sadece onlara erişim verilebilir.

<img width="495" height="476" alt="image" src="https://github.com/user-attachments/assets/3ed12ad6-dca6-45be-a335-e0915824e70e" />


## Client Tarafı

FW tarafında hangi ayarlar yapıldı ise forticlient uygulamasında da aynı ayarları yapmalıyız.
1. Vpn ismini yazıyoruz. Keyfi bir isim verilebilir
2. Remote gateway olarak, FW Wan ip'si yazılmalı.
3. ike version mode durumunu FW da ne seçildiyse aynısı olacak
4. Phase 1'in encryption metodları
5. Phase 1 için DH grubu
6. Phase 2'nin encryption metodları
7. Phase 2 için DH grubu
   
<img width="602" height="472" alt="image" src="https://github.com/user-attachments/assets/365abf5c-42da-49fd-a9e8-2541ded037c0" />


<img width="616" height="576" alt="image" src="https://github.com/user-attachments/assets/e47b28c7-47a5-4bc0-bc40-c8725c4724c3" />


## Connection
Test user ile bağlantı sağlandıktan sonra FW 10.10.10.10 ip adresini verdi. Ve FW arkasında bulunan 172.16.10.10 ip adresine de erişim geldi.
<img width="1375" height="425" alt="image" src="https://github.com/user-attachments/assets/4299513a-696f-45c9-9c1e-b121a948ab94" />

