## Site to Site VPN With Overlapping Subnets

Bu lab çalışmasında iki farklı şubeyi site to site VPN ile yapılandırırken her iki tarafta aynı subneti kullandıysa ve subneti değiştirmek mümkün olmadığında bağlantıyı NAT yardımı ile nasıl çözeceğimizi göstereceğim
<img width="1017" height="551" alt="image" src="https://github.com/user-attachments/assets/1f651fa2-9350-403b-b964-a6b6dfda7b49" />


Hem istanbul hem de ankarada lan tarafta 192.168.1.0/24 subneti kullanılıyor ve tünel içerisinden bunları istanbul için 10.10.10.0/24 subnetine, ankara için de 11.11.11.0/24 subnetine natlayacağız.

İpsec tünel için [site to site VPN](https://github.com/Nmkaynar/Fortigate-Lab/blob/main/Site%20to%20site%20VPN%20IPSEC.md) bölümünde yapmıştık.  Bu lab çalışmasında farklı olarak local ve remote subnetleri LAN tarafı değil Natlanacak subneti yazıyoruz.

<img width="660" height="301" alt="image" src="https://github.com/user-attachments/assets/a12780fe-c567-4cf2-b889-81d746d7b798" /><br>

## Overlapping Subnets

 Bu işlem için hem IP pools hem de Virtual IPs yapılandırmam gerekiyor.<br>

 ### IP Pools
External olarak : 10.10.10.1-10.10.10.254
internal olarak : 192.168.1.1-192.168.1.254 
şeklinde giriyoruz.<br>
 <img width="921" height="413" alt="image" src="https://github.com/user-attachments/assets/f696a59f-44d1-4ada-a493-41a8289c1f3a" /><br>

İstanbul LAN tarafı, Ankara LAn tarfına erişim sağlarken bu ip pool ile natlayarak trafiği iletecek. <br>
Yani<br>
`192.168.1.10 -> 10.10.10.10` veya `192.168.1.50 -> 10.10.10.50`, kısaca `192.168.1.x -> 10.10.10.x` şeklinde natlayarak trafiği iletecek.

 


### Virtual IPs

Aynı şekilde virtual İp yapılandırmam gerekiyor ki Ankara tarafından İstanbul tarafına gelen  10.10.10.x subnetini 192.168.1.x tarafına dönüştürerek trafiği lan tarafına geçirsin.


burada extenal range giriyoruz. Map to kısmına başlangıç ipsini yazmak yeterli gui otomatik olarak range tamamlıyor.
<img width="854" height="550" alt="image" src="https://github.com/user-attachments/assets/28111f2b-fa59-4c03-8068-39cf5e7be13c" />


### Policy (istanbul tarafı için)

İstanbuldan ankara tarafına policy yazarken Nat kısmını açarak ip pools da oluşturduğumuz pool'u seçiyoruz

<img width="1351" height="362" alt="image" src="https://github.com/user-attachments/assets/5b7919ea-4f60-4ec5-86cc-6fd87400df10" />

Ankaradan istanbul tarafına gelirken, destination olark virtual ip de oluşturduğum adress grubunu seçiyorum. Nat kapalı olacak.
<img width="1369" height="450" alt="image" src="https://github.com/user-attachments/assets/57c0bdae-b6a2-4bd6-bbbf-4ceabfd27407" />


### Routing  

Ankara tarafında ki 11.11.11.0/24 subneti için statik route yazıyorum.

<img width="975" height="46" alt="image" src="https://github.com/user-attachments/assets/b2827f60-8a9d-41de-9f24-76deb1ea500f" />


## Sonuç 

Ankara tarafı içinde yapılandırmalar tamamlandıktan sonra 
İstanbul tarafı Ankaradaki 192.168.1.x ip adresine erişmek için 11.11.11.x adresine erişmesi gerekecek.

<img width="564" height="430" alt="image" src="https://github.com/user-attachments/assets/1ebd2510-9a27-4a28-82e3-cf5694a93ce4" />

Ankara tarafıda aynı şekilde 10.10.10.x adresine erişmesi gerekecek. 

<img width="565" height="424" alt="image" src="https://github.com/user-attachments/assets/792e7545-90b4-4d8c-8cf2-88a87f4ceb88" />


## Kaynak 
Daha fazlası için 
https://docs.fortinet.com/document/fortigate/8.0.0/administration-guide/426761/site-to-site-vpn-with-overlapping-subnets
