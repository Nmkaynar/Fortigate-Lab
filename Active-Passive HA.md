## Active-Passive HA.md  

<img width="670" height="679" alt="image" src="https://github.com/user-attachments/assets/6f85d0e8-8871-46d3-8749-90b4da3ff8ba" /><br>

Bu lab çalışmasında aktif-pasif şekilde iki fortigate cihazını HA yaparak tek bir cihazmış gibi çalışmasını sağlayacağız.<br>

## Primary Yapılandırması

1. HA seçimini yapıyoruz.
2. Priorty'i belirliyoruz. Priorty yüksek olan primary olur.
3. Group name belirliyoruz. Aynı HA yapısında olan cihazlar aynı grup ismi olmalı.
4. Session pickup enable yapıyoruz. Bu ayar açık ise, ve priamry bir nedenden ötürü down olursa, trafik secondary den devam ederken, kullanıcıların aktif sesionları düşmez. Kapalı olursa sessionlar düşer.
5. Monitör interface olarak, hangi interfacelerin down olup olmadığını kontrol edilecek ise o interfaceler seçilmeli. Bu interfaceler down olursa trafik diğer cihaza geçer.
6. Heartbeat interface, HA üyelerinin birbirlerinin durumunu (up/down, sağlık durumu vb.) kontrol etmek için kullandığı özel arayüzdür. Bu arayüzler üzerinden cihazlar tercihen doğrudan birbirine bağlanmalıdır. Arada bir switch kullanılabilir ancak bu durumda switch tek hata noktası (Single Point of Failure - SPOF) haline gelebilir. Switch'in herhangi bir nedenle erişilemez duruma gelmesi halinde, HA üyeleri birbirlerini down olarak algılayabilir. Bu durum gereksiz failover'lara ve dolayısıyla trafik kesintilerine neden olabilir. Bu nedenle heartbeat bağlantıları için doğrudan bağlantı veya yedekli ağ altyapısı kullanılması tavsiye edilir.<br>

<img width="794" height="675" alt="image" src="https://github.com/user-attachments/assets/08be72ed-56ff-4eae-891f-0013cd7151c6" /><br>

<img width="1089" height="279" alt="image" src="https://github.com/user-attachments/assets/cee4e367-d008-41cd-b02c-16380e03c068" /><br>

## Secondary Yapılandırması

İkinci cihazda priorty birinci cihazdan daha düşük bir değere alıyoruz. Aynı şekilde portları ekleyerek onaylıyoruz.<br>
<img width="790" height="684" alt="image" src="https://github.com/user-attachments/assets/734c537c-4ba4-4120-9c62-bb966452c155" /><br>

Secondary de HA yapılandırtıktan sonra CLI üzerinde Sync işlemini başlatır. Sync tamamlandıktan sonra tüm admin userları logout yapar.<br>
<img width="981" height="457" alt="image" src="https://github.com/user-attachments/assets/1ac448bb-a3be-434a-8275-444df246c526" /><br>

<img width="1664" height="161" alt="image" src="https://github.com/user-attachments/assets/72743a8a-fdca-4a25-9416-52c3a563304a" /><br>

Yapılandırma birebir aynı olmalıdır. Her iki cihazda aynı porttan ısp'ye, aynı porttan LAN tarafına ve aynı porttan birbirlerine bağlanmalıdır. Çünkü Primary de yapılacak configler aynı şekilde secondary üzerine yazılacaktır. Örnek olarak primary de portlarda alias varken secondary de yoktur. HA sync olduktan sonra port1 ve port 5 te alias tanımlamalarını aldı.<br>

<img width="548" height="504" alt="image" src="https://github.com/user-attachments/assets/6a79fba6-0ce9-4d10-8da6-2f0e6f95d13c" /><br>


## HA testi
Client üzerinden 8.8.8.8'e ping başlattım. Ve SW'in primary ile olan bağlantısını koparttım. Görüldüğü gibi 1 tane ping kaybı sonrası trafik secondary üzerinden devam etti.<br>
<img width="2230" height="636" alt="image" src="https://github.com/user-attachments/assets/8c1ecde4-72e1-4f9b-8aec-c313ef079ccc" /><br>
Trafik secondary'e geçince gui arayüz olarak secondary cihazın guisi gelmektedir.<br>
<img width="1889" height="414" alt="image" src="https://github.com/user-attachments/assets/0ef8815e-0f6e-4080-a820-cd574a7d6506" /><br>

## Managment interface 

HA yaptıktan sonra her iki cihazın tek bir ip'si olacaktır ve ayrı ayrı guisine erişimi olmayacaktır. Her iki cihazdada manangment ip yapılandırdığımızda, cihazların guisine erişimimiz olacaktır. <br>

Lab da her iki cihazın port6'sından SW'e bağlantı verdik. primary için SWde Gİ2/0, secondary için Gi2/1 portuna bağlantı veridim<br>

Primary de port6'ya 10.10.10.100/24 ip'sini verdim. Ve öamangment interface de ekledim. Gateway olarak 10.10.10.1 ip'sini verdim. Bu ip'yi SW'de vlan interface için verceğim.
Destination subnet olarak client subnetini yazdım. <br>

<img width="699" height="901" alt="image" src="https://github.com/user-attachments/assets/5efa1397-e17d-4274-9c72-69fc4a582f98" /><br>

<img width="505" height="182" alt="image" src="https://github.com/user-attachments/assets/0c1f5d6a-c286-4968-96a0-f10c1fcb2bb8" /><br>

Bu işlemi tamamladıktan sonra Sync olmayacaktır. Sebebi ise management ayarını sync etmemesidir.<br>
<img width="610" height="133" alt="image" src="https://github.com/user-attachments/assets/252e3c30-044c-449f-aaaa-9039a8b8f4d7" /><br>

primary ve secondary cihazlarda ha-mgmt ayarında farklılık vara.<br>
<img width="357" height="813" alt="image" src="https://github.com/user-attachments/assets/c38b3f23-0066-484d-969e-33bed72cd42f" /><br>

Secondary cihazın guisine eirişimiz olmadığından birinci cihaz üzerinden ikinci cihaza geçmemiz gerekmektedir. Bunuda 
execute ha manage 0 admin ile login oluyoruz. <br>

config ha-mgmt-interfaces altında dst ve gateway ayarlarını yapıyoruz. Ve secondary cihazda port6 nın ip'sini değiştiriyoruz.<br>

<img width="309" height="592" alt="image" src="https://github.com/user-attachments/assets/3ac8fef4-4e6b-49d5-9fb6-3bb7b0dabe46" /><br>

<img width="413" height="334" alt="image" src="https://github.com/user-attachments/assets/a37fc5d3-abf2-412a-998b-2cf4da8a1334" /><br>

İşlemleri tamamladıktan sonra synch oldu.<br>
<img width="1628" height="155" alt="image" src="https://github.com/user-attachments/assets/57e7a3c8-90e1-46bb-ac78-95e8613b24d0" /><br>

## Switch configi
````
en
conf t
ip routing
ip route 0.0.0.0 0.0.0.0 192.168.10.1
vlan 50
name FW-mgmt
exit
interface vlan 1
ip add  192.168.10.254 255.255.255.0
no sh
exit
inter vlan 50
ip add  10.10.10.1 255.255.255.0
no sh
exit
inter range gi 2/0-1
switchport mode access
switchport access vlan 50
exit

access-list 10 permit 192.168.10.10
inter vlan 50
 ip access-group 10 out
end
````

Bu işlemden sonra client1 makinesinde gateway olarak 192.168.10.1 yerine 192.168.10.254 ip si verimeliyiz. Sonra client1 den hem cluster ip'sine hem primary'e hem de secondary'e ayrı ayrı olarak erişimimiz mevcuttur.<br>

<img width="702" height="287" alt="image" src="https://github.com/user-attachments/assets/ccf1d5fb-5a7b-4eaa-a76c-f4a98b968adc" /><br>
<img width="571" height="263" alt="image" src="https://github.com/user-attachments/assets/53d2ff20-25ce-4914-80de-0b677c2ff83a" /><br>
<img width="841" height="344" alt="image" src="https://github.com/user-attachments/assets/b1922d5e-c814-411d-bb3f-a9b4f55f7653" /><br>

Ayrıca ACL ile sadece cient1 cihazına erişim vermiş olduk ki başka cihazlardan mgmt ip'lerine erişim sağlayamasın.<br>

<img width="615" height="300" alt="image" src="https://github.com/user-attachments/assets/72a8baf3-9549-481f-b037-dc042c175ede" /><br>




