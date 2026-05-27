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
Hub'da VPN tüneli kurarken "auto discovery sender" seçeneğini enable etmez isek burada ping başaraız oluyor.

<img width="579" height="421" alt="image" src="https://github.com/user-attachments/assets/b59a219a-4325-49b6-b829-b16077bc5cb4" />

### BGP Yapılandırması  

1. IBGP yapılandıracağımız için Local AS no belirliyoruz.
2. Router id olarak tünel ip adresini veriyorum
3. Neighbor ip adreslerini giriyoruz.
<img width="551" height="410" alt="image" src="https://github.com/user-attachments/assets/741c22de-b946-43ed-80e4-444014c02d3d" />

4. Neighbor ip'lerini girerken Route reflector client seçimini yapmamız gerekiyor. Bunun sebebi IBGP doğası IBGP den öğrendiği rotaları başka bir IBGP komşusuna duyurmaz. HUB router RR olmalı ki spokelardan öğrendiği rotaları diğer spokelara duyurabilsin. Route reflector client seçeneğini sadece HUBda işaretliyoruz
<img width="892" height="772" alt="image" src="https://github.com/user-attachments/assets/32684e32-8d84-4722-8ff7-5a6ba40e66a5" />

5. İpsec ile haberleşecek local subnetleri giriyoruz
<img width="445" height="133" alt="image" src="https://github.com/user-attachments/assets/f0dfc08c-cc3f-4603-a6eb-42022a2aad39" />


### Spoke1 Yapılandırması
<img width="553" height="916" alt="image" src="https://github.com/user-attachments/assets/ff38ba12-cc98-4960-87e8-e53fde7307e3" />

### Spoke2 Yapılandırması

### BGP sonucu
HUB spokelarla BGP komşuluğu kurdu ve rotaları öğrendi
<img width="1610" height="144" alt="image" src="https://github.com/user-attachments/assets/774d4815-83cd-407a-8ef5-eb6ae59f7422" />
<img width="1566" height="160" alt="image" src="https://github.com/user-attachments/assets/0c8f487f-7a16-4523-97f6-03bd30735627" />

Spoke1 ve spoke2 de aynı şekilde rotaları öğrendi
<img width="1611" height="180" alt="image" src="https://github.com/user-attachments/assets/275c5016-6e8c-41ed-ad33-d7aac68b4387" />


## Adım 5 
### Client1 ping testi
<img width="580" height="521" alt="image" src="https://github.com/user-attachments/assets/6d37c28f-f98e-4388-b0d3-d9b00db4b441" /><br>
### Client 2 nin ping testi

<img width="587" height="539" alt="image" src="https://github.com/user-attachments/assets/8f3180fb-3713-440c-b0de-3371abc2415a" />

Client2 client1'e ping attı ama client3'e ping atamadı. Bunun sebebi Hub tarafında spoke <-> spoke şeklinde policynin olmaması.
debug yaparakda doğruluyoruz.
<img width="1178" height="423" alt="image" src="https://github.com/user-attachments/assets/2799c544-805a-4ac0-a6df-954b4ec0c538" />


<img width="1658" height="85" alt="image" src="https://github.com/user-attachments/assets/5669c10f-0ac9-4528-a309-86cafd4dd3a0" />

Policy yazdıktan sonraki ping testi başarıyla sağlandı.<br><br>
<img width="482" height="101" alt="image" src="https://github.com/user-attachments/assets/5a02c1f5-3c59-4417-bfab-a7d3888fee20" />


## Spokelarda "auto discovery reciever" enable etme

Bu haliyle spokelar, birbiri ile iletişimi hub üzerinden sağlamaktadırlar.<br><br>
<img width="452" height="165" alt="image" src="https://github.com/user-attachments/assets/e7be0a99-1387-40bf-a581-794e7f9591bc" />


Eğer direkt spoke1 den spoke2'ye hub'a uğramadan iletişim kurulmasını istiyorsak, phase1 advanced altında auto discovery receiver seçeneğini enable etmemiz gerekiyor. Hem spoke1 hem de spoke2de <br><br>
<img width="685" height="665" alt="image" src="https://github.com/user-attachments/assets/c329ef4c-650a-4daa-b2f8-37c31c7a43a5" />

Enable edildikten sonra <br><br>
<img width="480" height="176" alt="image" src="https://github.com/user-attachments/assets/9306648f-2299-4b54-ab56-c231b06eb2f6" />









