## BGP over IPSEC
<img width="1235" height="440" alt="image" src="https://github.com/user-attachments/assets/c2b90c34-7c06-4997-a7b5-347a2b690efc" />

 İpsec tünel için 

1. iki tarafta wan ip lerine erişebilmeleri gerekiyor
2. ipsec için phase1 ve phase 2 ayarları yapılandırılmalı
3. Her iki tarafta erişim için FW policy yazmalı
4. Her iki tarafta BGP  yapılandırarak LAN taraflarını duyurmalı

## 1. Her iki tarafta birbirlerine erişebilmekte.
istanbul FW'dan ankara FW'na<br><br>
<img width="505" height="274" alt="image" src="https://github.com/user-attachments/assets/1e1f2e71-110e-4aba-aa29-587c44c746e0" /><br><br>
Ankara FW'dan İstanbul FW'na<br><br>
<img width="539" height="311" alt="image" src="https://github.com/user-attachments/assets/9b5d5d93-0f2f-4071-8a37-65b8647e6c0e" /><br><br>

## 2. PHASE1 ve PHASE 2 adımları

### PHASE 1 
Bu adım tünel kurulum aşaması. <br>
1. adımda karşı tarafın wan ip adresini yazmalıyız ve ona hangi porttan erişecek isek o portu seçmeliyiz
2. Tünel kurulurken ki kimlik doğrulama için girdiğimiz parola. Her iki tarafta da aynı parola(pre shared key ) kullanılmalı. Ve güçlü bir şifre verilmeli.
3. Burada ipsec tünelinin phase 1 için tünelden geçen trafğin hangi metod ile şifrelenmesini ve şifrelenmiş verinin bütünlüğünü hangi metod ile kontrol etmesini belirtiyoruz. Burada demo ürün olduğundan şifreleme için DES seçildi. Ancak best practice de AES128 veya AES256 seçilmeli. Veri bütünlüğü için de SHA256 seçildi. Ve her iki tarafta da aynı algoritmalar seçilmeli.


<img width="577" height="934" alt="image" src="https://github.com/user-attachments/assets/52c5f907-d1bf-4127-9abb-4be77a689577" />

### PHASE 2

Bu  adım tünel kurulduktan sonra hangi subnetlerin geçeceğini belirttiğimiz kısım <br>

1. Burada herhangi bir subnet girmiyoruz. Amaç BGP ile rotaları dinamik olarak öğretmek.
2. Burada yine host trafiğinin hangi metod ile şifreleneceğini ve bütünlüğünü koruyacağını belirttğimiz kısım. Yani veri des ile şifrelenir sha256 ile oluşturulan hash değeri ile kontrol edilir. Verinin hash değerinde değişiklik olduğunda paket drop edilir.
3. Bu kısım ipsec tünelinin sürekli ayakta tutması için keep alive mesajlarının gönderilmesi içindir. İşaretlenmez ise ipsecden bir süre trafik geçmezse downa geçer. 


<img width="603" height="665" alt="image" src="https://github.com/user-attachments/assets/0230936f-9d9e-4cb9-9fb2-d882690cb250" />

## 3. FW Policy

Yapılandırmanın çalışabllmesi için FW policy yazarak izin vermeliyiz. Bu yapılandırmada lab olduğundan all all kural girdik. Ancak best practice için sadece gerekli izinlerin girilmesi güvenlik açısından son derece önemlidir. 

<img width="1694" height="153" alt="image" src="https://github.com/user-attachments/assets/c4cc25dd-81f3-439d-b384-5567e6447d4c" />


## 4. BGP
Önce İpsec tünel için ip verelim<br>

<img width="958" height="63" alt="image" src="https://github.com/user-attachments/assets/7a45c464-99ef-405f-b7d7-d8eafc84a2eb" /><br>

BGP yapılandırarak lan taraftaki networkleri duyuruyoruz<br>

1. BGP AS no giriyoruz. ibgp yapılandıracağımız için local AS verdik
2. Router id, komşukulta cihazın kimliğini belirtir.
3. Karşı komşunun ip adresi ve AS no. ibgp olduğundan aynı AS girilmiştir.<br><br>
<img width="432" height="361" alt="image" src="https://github.com/user-attachments/assets/db70aeda-d565-460f-b5cc-7055c853f10e" /><br>

Anons edilecek networkleri giriyoruz.<br><br>
<img width="432" height="134" alt="image" src="https://github.com/user-attachments/assets/c971e59a-b27b-4290-8962-be032cf039fd" /><br>

her iki tarafta da bgp yapılandırılarak komşuluk kuruldu.<br>

<img width="1671" height="84" alt="image" src="https://github.com/user-attachments/assets/6729900d-fb6a-468c-ae54-1613de888f4c" /><br>


## Sonuç
BGP over IPSEC ile client 1 den hem client2'ye hem de PC2'ye erişim sağlandı. Eğer yeni bir subnet dahil edilecek ise BGP ile anons etmek yeterli olacaktır.<br><br>

<img width="593" height="245" alt="image" src="https://github.com/user-attachments/assets/f0d74dc7-8c3c-41a8-98e7-6b13328fc860" /><br>
<img width="467" height="145" alt="image" src="https://github.com/user-attachments/assets/94e19459-0ab8-4a61-b562-86748a7198dd" /><br>
<img width="519" height="207" alt="image" src="https://github.com/user-attachments/assets/a0a25f1b-dec5-40e1-9b3e-3ce3f39c9604" /><br>

