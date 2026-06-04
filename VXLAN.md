## VXLAN
Bu labda vxlan ile farklı lokasyondaki cihazların aynı vlan içerisinde iletişme geçmelerini sağlayacağız.<br>
<img width="936" height="528" alt="image" src="https://github.com/user-attachments/assets/563eba0b-be96-44d1-a6a7-a157880b1d22" /><br>
### Vlan100 için FW da port9 altında interface oluşturalım.
<img width="707" height="189" alt="image" src="https://github.com/user-attachments/assets/70e77b08-ee53-463c-9c55-8270d1a5ae68" /><br>
1. İnterface VL-100 adını verdim.
2. Vlan interface hangi interface altında oluşturacaksak onu seçiyoruz
3. Vlan id olarak 100 giriyoruz
4. Adress objesi oluşturmasını kapatıyoruz. Adress objesi oluşturuna software sw altında ekleyebilmek için silinmesi gerekiyor.<br>

<img width="714" height="624" alt="image" src="https://github.com/user-attachments/assets/523a4c11-acf1-4db0-9547-3b531dc4bb83" /><br>
<img width="921" height="75" alt="image" src="https://github.com/user-attachments/assets/564b1d58-1fd8-4f9d-bb58-89e3423b27f5" /><br>

### VXLAN100 interface'ini oluşturalım. 

Bunu da Wan portu altında yapacağız. Ancak bu işlem CLİ üzerinden yapılacak.<br>

````
config system vxlan
edit VXLAN100
set interface port10
set vni 100
set remote-ip 2.2.2.2
end
````
Bu işlem sonrası sayfayı yenilememiz gerekiyor.<br>
<img width="963" height="80" alt="image" src="https://github.com/user-attachments/assets/e789d97b-a27d-4339-991e-e8b2eb35bd73" /><br>

### VXLAN100 altında vlan interface oluşturalım.

<img width="771" height="624" alt="image" src="https://github.com/user-attachments/assets/c611c7e3-338b-498f-8d20-04336351b124" /><br>

<img width="960" height="98" alt="image" src="https://github.com/user-attachments/assets/b0748ef4-ad92-4a77-b0a5-b6a80205694a" /><br>

### Software switch interface oluşturarak, oluşturduğumuz iki vlanı da software switch interface'in üyesi yapalım.<br>

1. Oluşturduğumuz interface bir isim veriyoruz. VLAN100 tercih ettim
2. interface tipi olarak software switch seçiyoruz
3. member olarak oluşturduğumuz vlan interfaceleri seçiyoruz
4. interface'e ip veriyoruz. Bu ip vlan100'ün gateway ip'si olacak.
5. Cihazlar ip alabilmesi için dhcp'yi aktif ediyoruz.<br>

<img width="981" height="922" alt="image" src="https://github.com/user-attachments/assets/82cfb04a-277f-491a-b914-9bde0dc6dc74" /><br>
<img width="942" height="92" alt="image" src="https://github.com/user-attachments/assets/2482a5e0-7e0a-4fc0-8f3b-c2a582059e3b" /><br>

PC1 client için ip dhcp komutunu çalıştıralım.<br>

<img width="472" height="107" alt="image" src="https://github.com/user-attachments/assets/fcecf7cf-537c-47f4-8113-580104ba7efb" /><br>

Görüldüğü gibi ip'yi alabildi.<br>

Aynı işlemleri Branch tarafında da yaparak PC3'ün de HQ üzerinden ip almasını sağlayalım. 
Branch tarafında interface isterseniz ip verebilirsiniz, ip vererek HQ üzerindeki vlan100 interface ile iletişime geçbiliyor mu test edebilirsiniz.<br>

<img width="884" height="105" alt="image" src="https://github.com/user-attachments/assets/c1784ece-bc14-4e35-b887-8d39f98e9719" /><br>

PC3 de dhcp üzerinden ip aldı.<br>
<img width="468" height="98" alt="image" src="https://github.com/user-attachments/assets/f36ae66d-3b00-4222-9152-d81e85672dca" /><br>

VXLAN sayesinde layer2 trafi layer 3 üzerinden taşınabilir hale geldi. <br>

### Ping testi.
PC1'den PC3'e ping testi başarılı.<br>
<img width="691" height="212" alt="image" src="https://github.com/user-attachments/assets/007cea8b-4f65-441f-978a-12020d147883" /><br>

ICMP paketi içerği <br>
<img width="833" height="307" alt="image" src="https://github.com/user-attachments/assets/af64cafe-6e61-4a69-b00b-b788e30beea4" /><br>



 





















