## VXLAN over IPSEC

<img width="1354" height="72" alt="image" src="https://github.com/user-attachments/assets/5ea83658-2023-4de6-8181-b3048d2c7c3b" />
Bu labda VXLAN kullanarak farklı lokasyonlardaki cihazların IPSEC üzerinden aynı Layer 2 segmentinde haberleşmesini sağlayacağız.
<img width="936" height="528" alt="image" src="https://github.com/user-attachments/assets/0f160075-b7c9-4e2a-a5c7-b270430f06ff" />

 
## IPSEC yapılandıralım. 

HQ üzerinde Branch isimli ipsec tüneli kuralım

<img width="641" height="761" alt="image" src="https://github.com/user-attachments/assets/96381a86-a2be-4377-84a4-56691b5ea2eb" />
<img width="696" height="757" alt="image" src="https://github.com/user-attachments/assets/f2326684-5eac-480d-ae86-b286515743b3" />
<img width="1354" height="72" alt="image" src="https://github.com/user-attachments/assets/a355a156-680b-4ebc-9af9-fecc93e1d2e8" />

Tünel interface ip verelim.
<img width="703" height="272" alt="image" src="https://github.com/user-attachments/assets/ecfc02ee-2c2b-4847-86c2-e5b62d4a0214" />
<img width="936" height="73" alt="image" src="https://github.com/user-attachments/assets/25c28690-f9d2-472c-8986-e167439a1e35" />


[VXLAN](VXLAN.md)'ı vlan100 için yapmıştık. Bu labda vlan200 için yapıcağız. 

Yine Lan portu altında VL-200 interface oluşturuyoruz.
<img width="658" height="468" alt="image" src="https://github.com/user-attachments/assets/407d380b-a26e-45a5-a524-f94b6edc2684" />

Branch tünel interface altında VXLAN200 interface oluşturalım

````
config system vxlan
edit VXLAN200
set interface Branch
set vni 200
set remote-ip 10.10.10.2
end
````

İşlem sonrası gui sayfasını yenileyelim
