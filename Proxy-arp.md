## Proxy-arp
<img width="527" height="514" alt="image" src="https://github.com/user-attachments/assets/a16ff858-5273-4dc5-9a8c-8e52002c8dbc" /><br>

Normal şartlarda PC1 ve PC2 aynı vlanda ise FW'a uğramadan SW sayesinde kendi aralarında iletişim kurabilirler.  Ancak burada oluşan sorun layer2 güvenliği eğer cihazlardan birine bir virüs bulaşmış ise bu diğer cihazlarada geçebileceği anlamaına gelir. Çünkü ortada bir güvenlik söz konusu yoktur.

Bu sorunu kaldırmak için cihazların SW üzerinden değil her halükarda FW üzerinden birbirleri ile iletişime geçmelerini sağlayacağız<br>

Öncelikle defaultta olan şeye bir bakalım. <br>
PC1'de `` show ip `` komutunu çalıştırarak mac adresini öğrenelim ve PC2'ye ping atalım.<br>
<img width="411" height="317" alt="image" src="https://github.com/user-attachments/assets/749bb3c7-c42b-4f48-80f6-f3e76b90198c" /><br>
PC2'de ``arp`` tablosuna baktığımızda ``172.16.10.10`` ip adresi için PC1'in mac adresini gösteriyor.<br>

<img width="624" height="147" alt="image" src="https://github.com/user-attachments/assets/79d20dbb-33b3-4ae3-8514-33410e7a3362" /><br>

Yapacğımız config sayesinde  ``172.16.10.10`` ip adresi için FW mac adresini gösterecek. Aynı şekilde PC1'de de ``172.16.10.11`` ip adresi için FW mac adresini gösterecek. Bu durumda SW mac adresine göre paket forwardingi yapacak.<br>

## FW'da yapılacak işlemler

### VLAN10 interface'i oluşturalım

<img width="677" height="767" alt="image" src="https://github.com/user-attachments/assets/8dc6b597-6692-4ba1-aa57-cdba64c17602" /><br>

### CLİ üzerinde proxy-arp tanımlaması yapalım

````
config system proxy-arp 
edit 1
set interface "vlan10"
set ip 172.16.10.2 
set end-ip 172.16.10.254 
end
````
### Policy

VLan10 interface'inden VLAN10 interface'ine bir policy yazılmalı. <br>

Lab ortamında olduğundan all all şeklinde kural yazıldı. Ancak network ortamınıza göre gerekli izinlerin verilmesi gerekmektedir.<br>

<img width="1515" height="136" alt="image" src="https://github.com/user-attachments/assets/cce9f350-cee5-4aea-ac72-62f70f9d5c91" /><br>

PC2'de tekrar arp tablosuna baktığımızda PC1'in ip için FW mac adresini kaydetmiş. Artık PC1 ve PC2 arasındaki iletişim FW üzerinden gerçekleşecek.<br>

<img width="624" height="272" alt="image" src="https://github.com/user-attachments/assets/21c511d9-d560-494c-ab4f-3563f99a15ba" /><br>
Aynı şekilde PC1 arp tablosunda da PC2'nin mac ardesi olarak FW mac adresi  görülmektedir
<img width="630" height="229" alt="image" src="https://github.com/user-attachments/assets/488b3a12-3cde-4d95-9ea8-4db8b2fd307c" /><br>

## Switch Config
````
enable
conf t
interface GigabitEthernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
interface range gigabitEthernet 0/1-2
 switchport access vlan 10
 switchport mode access
 switchport protected
exit
````

``switchport protected`` bu komut protected portların birbirleri ile layer2 haberleşmesini engelemeye yarar. Eğer cisco sw de portlarda bu komut çalıştırılmaz ise PC1 ve PC2, FW da config yapılmasına rağmen SW üzerinden haberleşmeye devam edecektir.










