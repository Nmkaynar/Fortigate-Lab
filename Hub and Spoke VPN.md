## Hub and Spoke VPN

Bu lab çalışmasında hub and spoke vpn yapısını inceleyeceğiz. VPN'i custom arayüzünden kuracağız

<img width="1050" height="557" alt="image" src="https://github.com/user-attachments/assets/7fb6016a-2952-4e38-a860-8fe2bd0c15e2" />

İzlenecek adımlar.
1. Her FW WAN bacaklarından birbirine erişim sağlamalılar
2. HUB da dialup VPN, diğerleinde static ipsec vpn kuracağız.
3. VPN tüneller kurulumu tamamlandıktan sonra, tünele doğru FW policylerini oluşturucağız.
4. BGP yapılandırmak için tünellere ip vereceğiz ve BGP komşuluklarını kuracağız
5. Son olarak ping testi gerçekleştireceğiz


## Adım 1
ISP olarak ortama router konumlandırdık ve hem FW hem de routerda gerekli ip yapılandırmasını yaptık.

<img width="647" height="453" alt="image" src="https://github.com/user-attachments/assets/c3cbba4c-e1e1-4f65-9113-d2b2c029e4f6" />

## Adım 2 
### HUB 
1. Remote gateway olarak Dialup seçip interface olarak WAN bacağını seçiyorum
2. birden fazla lokasyon bağlanabilmesi için "add route" seçeneğini disable edip, "auto discovery sender" seçeneğini enable ediyorum
<img width="612" height="602" alt="image" src="https://github.com/user-attachments/assets/4e2cb6d5-6d47-44b9-b4ae-358aa00c09cc" />

3. Tünel içinde kimlik doğrulaması için güçlü bir parola belirliyoruz.
4. Any peer id seçip devam ediyoruz
<img width="603" height="239" alt="image" src="https://github.com/user-attachments/assets/3286a440-677c-4ed4-a62b-6b69e3232c4a" />

5. Demo ürün olduğunda DES ve Sha256 seçildi. Lisanslı üründe daha güçlü encryption algoritması seçmeniz gerekmektedir. Diğer ayarlar default kalabilir

<img width="639" height="269" alt="image" src="https://github.com/user-attachments/assets/3fcb7137-42a5-4935-b04b-80ee6ba732f0" />

6. Birden fazla lokasyon olacağı için herhangi bir subnet girmiyoruz
7. Phase1 deki aynı mantık
8. Auto keep alive açıyoruz. Kapalı olursa, VPNden trafik geçmez ise tünel downa geçer. Bu seçenek bunun önüne geçmiş oluyor 

<img width="546" height="580" alt="image" src="https://github.com/user-attachments/assets/f0b05bb2-7165-4626-8f83-6c1b85c9f83e" />

### Spoke 

1. HUB'ın wan ip adresini yazıyoruz
2. Hub da belirlediğimiz parolayı giriyoruz
3. Hub ne seçildiyse aynısını seçiyoruz

<img width="632" height="750" alt="image" src="https://github.com/user-attachments/assets/17c2fca2-96d7-4bb3-a9bb-0c9a74b87a12" />

4. Phase 2 HUB'da nasıl yapıldıysa aynı şekilde olmalı.

<img width="675" height="741" alt="image" src="https://github.com/user-attachments/assets/b7951d66-5220-4439-9c9f-66d00081aebc" />

Not: Burada Spoke1 gösterildi. Spoke 2 de aynı şekilde olacaktır.

## Adım 3
### HUB için Policy

<img width="1910" height="272" alt="image" src="https://github.com/user-attachments/assets/b140dfd8-aad0-44ae-b481-2b0134db08bb" />

### Spoke için Policy

<img width="1915" height="327" alt="image" src="https://github.com/user-attachments/assets/6433e3c2-487e-4ee4-bd2e-025d547691b7" />

## Adım 4

### Hub için 

<img width="715" height="299" alt="image" src="https://github.com/user-attachments/assets/1d322dff-9711-4683-b103-c19d73abf5f5" />

### Spoke1 için

<img width="736" height="328" alt="image" src="https://github.com/user-attachments/assets/f5306c2d-9fd5-4717-a81b-32ea272baa46" />

### Spoke2 için

<img width="747" height="325" alt="image" src="https://github.com/user-attachments/assets/98f6ef48-905d-459a-8d70-bcaa649b9ebd" />

### Hub dan ping testi






