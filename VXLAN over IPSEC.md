## VXLAN over IPSEC

Bu labda VXLAN kullanarak farklı lokasyonlardaki cihazların IPSEC üzerinden aynı Layer 2 segmentinde haberleşmesini sağlayacağız.<br>
<img width="936" height="528" alt="image" src="https://github.com/user-attachments/assets/0f160075-b7c9-4e2a-a5c7-b270430f06ff" /><br>

 
## IPSEC yapılandıralım. 

HQ üzerinde Branch isimli ipsec tüneli kuralım<br>

<img width="641" height="761" alt="image" src="https://github.com/user-attachments/assets/96381a86-a2be-4377-84a4-56691b5ea2eb" /><br>
<img width="696" height="757" alt="image" src="https://github.com/user-attachments/assets/f2326684-5eac-480d-ae86-b286515743b3" /><br>
<img width="1354" height="72" alt="image" src="https://github.com/user-attachments/assets/a355a156-680b-4ebc-9af9-fecc93e1d2e8" /><br>


Tünel interface ip verelim.<br>
<img width="703" height="272" alt="image" src="https://github.com/user-attachments/assets/ecfc02ee-2c2b-4847-86c2-e5b62d4a0214" /><br>
<img width="936" height="73" alt="image" src="https://github.com/user-attachments/assets/25c28690-f9d2-472c-8986-e167439a1e35" /><br>


[VXLAN](VXLAN.md)'ı vlan100 için yapmıştık. Bu labda vlan200 için yapıcağız. <br>

Yine Lan portu altında VL-200 interface oluşturuyoruz.<br>
<img width="658" height="468" alt="image" src="https://github.com/user-attachments/assets/407d380b-a26e-45a5-a524-f94b6edc2684" /><br>

Branch tünel interface altında VXLAN200 interface oluşturalım. remote ip olarak Wan portunu değil ipsec tünel ip adresini girmeliyiz.<br>

````
config system vxlan
edit VXLAN200
set interface Branch
set vni 200
set remote-ip 10.10.10.2
end
````

İşlem sonrası gui sayfasını yenileyelim<br>
<img width="949" height="106" alt="image" src="https://github.com/user-attachments/assets/1243ab28-8c53-4f97-8b0d-953998629b9f" /><br>
VXLAN200 altında vlan200 interface oluşturulaım.<br>
<img width="652" height="477" alt="image" src="https://github.com/user-attachments/assets/a1ada513-a9fa-4d28-918d-8cf58d10fef7" /><br>
Bu şekilde göüzükecek.<br>
<img width="951" height="136" alt="image" src="https://github.com/user-attachments/assets/79f61f3f-81e2-4709-a203-ee79970bd8e6" /><br>

Şimdi VL-200 ile VX-200 interfaceleri software switch üzerinde bir member yapalım. Bu interface <br>
1. Vlan200 adını verdim
2. vl-200 ile vx-200 interfacelerini member yaptım
3. vlan200 için gateway ip belirledim.
4. dhcp aktif ettim.<br>

<img width="777" height="823" alt="image" src="https://github.com/user-attachments/assets/b45b975e-0fca-49d6-994e-57740a2a3101" /><br>
<img width="1033" height="169" alt="image" src="https://github.com/user-attachments/assets/fe563b83-6120-4ab8-84bc-863f7a92c226" /><br>



Branch FW tarafını da aynı şekilde yapılandıralım.<br>

<img width="997" height="111" alt="image" src="https://github.com/user-attachments/assets/f0252ac0-b525-47ef-937a-37f1c470b85b" /><br>
<img width="919" height="89" alt="image" src="https://github.com/user-attachments/assets/1dcfc33f-497e-4d32-bc2b-81d692c3ea4c" /><br>


PC2 ve PC4 HQ fortigate üzerinden ip'lerini aldı<br>
<img width="480" height="97" alt="image" src="https://github.com/user-attachments/assets/1c6cc8a9-38ba-48e7-baee-4409cdcc1a17" /><br>
<img width="449" height="115" alt="image" src="https://github.com/user-attachments/assets/f25304a8-e944-44c0-a139-7ce943c2ff4e" /><br>

### Ping testi

Başarılı bir şekilde ping atabildi.<br>
<img width="666" height="273" alt="image" src="https://github.com/user-attachments/assets/bca68a00-ee82-4c09-8500-9f7d12c30368" /><br>

Aynı zamanda icmp paketleri şifreli bir şekilde iletildi.<br>
<img width="1005" height="301" alt="image" src="https://github.com/user-attachments/assets/8c6b21a5-5ee1-4cb0-a40c-c374b5e516f6" /><br>









