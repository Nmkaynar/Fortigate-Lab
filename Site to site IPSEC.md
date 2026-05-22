## Site to site IPSEC
<img width="1031" height="534" alt="image" src="https://github.com/user-attachments/assets/3be7bb93-776e-4f5a-929d-e93d7732ae74" />

 İpsec tünel için 

- iki tarafta wan ip lerine erişebilmeleri gerekiyor
- ipsec için phase1 ve phase 2 ayarları yapılandırılmalı
- Her iki tarafta erişmek istediği remote subnet için statik veya dinamik route girmeli
- Her iki tarafta erişim için FW policy yazmalı


## 1. Her iki tarafta birbirlerine erişebilmekte.
istanbul FW'dan ankara FW'na<br>

<img width="505" height="274" alt="image" src="https://github.com/user-attachments/assets/85b5ef7c-7fda-4c56-b985-1432bd698676" /><br>  
Ankara FW'dan İstanbul FW'na

<img width="539" height="311" alt="image" src="https://github.com/user-attachments/assets/df6cd73a-34e2-462b-a663-95396b9fd57b" /><br>


## 2. PHASE1 ve PHASE 2 adımları

### PHASE 1 
Bu adım tünel kurulum aşaması. <br>
1. adımda karşı tarafın wan ip adresini yazmalıyız ve ona hangi porttan erişecek isek o portu seçmeliyiz
2. Tünel kurulurken ki kimlik doğrulama için girdiğimiz parola. Her iki tarafta da aynı parola(pre share key ) kullanılmalı. Ve güçlü bir şifre verilmeli.
3. Burada ipsec tünelinin phase 1 için tünelden geçen trafğin hangi metod ile şifrelenmesini ve şifrelenmiş verinin bütünlüğünü hangi metod ile kontrol etmesini belirtiyoruz. Burada demo ürün olduğundan şifreleme için DES seçildi. Ancak best practice de AES128 veya AES256 seçilmeli. Veri bütünlüğü için de SHA256 seçildi. Ve her iki tarafta da aynı algoritmalar seçilmeli.


<img width="577" height="934" alt="image" src="https://github.com/user-attachments/assets/12211d43-88fb-49f8-bffa-52f7d8186116" />


### PHASE 2

Bu  adım tünel kurulduktan sonra hangi subnetlerin geçeceğini belirttiğimiz kısım <br>

1. Kendi lan subnetimizi ve remote lan subnetini belirtiyoruz.
2. Burada yine host trafiğinin hangi metod ile şifreleneceğini ve bütünlüğünü koruyacağını belirttğimiz kısım. Yani veri des ile şifrelenir sha256 ile oluşturulan hash değeri ile kontrol edilir. Verinin hash değerinde değişiklik olduğunda paket drop edilir.
3. Bu kısım ipsec tünelinin sürekli ayakta tutması için keep alive mesajlarının gönderilmesi içindir. İşaretlenmez ise ipsecden bir süre trafik geçmezse downa geçer. 

<img width="641" height="660" alt="image" src="https://github.com/user-attachments/assets/fe7e5812-a905-4932-ae0d-c56b3da2e3d8" /><br>

Bu işlemlerden sonra istanbul için ipsec tüneli kuruldu. Ancak şu an down durumda.  Statik route ve policy girdikten sonra aynı  işlemler ankarada yapılınca tünel up olacak.

<img width="1645" height="109" alt="image" src="https://github.com/user-attachments/assets/dc03a333-e320-41b2-9a15-70aa6d3f21fb" /><br>

## Routing
Bu labda statik routing yapacağız<br>
<img width="622" height="300" alt="image" src="https://github.com/user-attachments/assets/97151d92-9141-47e2-a234-64adcbe9f194" /><br>

## Policy
Yapılandırmanın çalışabllmesi için FW policy yazarak izin vermeliyiz. Bu yapılandırmada lab olduğundan all all kural girdik. Ancak best practice için sadece gerekli izinlerin girilmesi güvenlik açısından son derece önemlidir. 
<img width="1694" height="153" alt="image" src="https://github.com/user-attachments/assets/9c08b0e5-f980-4156-b91c-ea360eec8f8c" />

İstanbul tarafı için yapılandırma bitti aynı şekilde Ankara tarafıda yapılandırıldıkan sonra ipsec tüneli ayağa kalkacaktır

## sonuç

Client 1 vm den client2'ye ping ve trace çıktısı:<br><br>
<img width="529" height="378" alt="image" src="https://github.com/user-attachments/assets/913d076a-0317-4343-aea3-2ab61b130f0b" /><br><br>
İpsec up oldu ve ping attığımızda veri tünelden geçmiş<br><br>
<img width="1651" height="125" alt="image" src="https://github.com/user-attachments/assets/8dc4f58d-a549-4a89-ba76-9028fb1bdb1a" /><br><br>
Ankara tarafından da ping ve trace çıktısı	<br><br>
<img width="499" height="409" alt="image" src="https://github.com/user-attachments/assets/c96956b4-7874-4061-96e4-c4f658e0a519" /><br>










