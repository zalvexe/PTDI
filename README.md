# PERANCANGAN MINI-BUS KOMUNIKASI DATA PADA SISTEM AVIONIK MENGGUNAKAN PROTOKOL CAN

## Sumber bacaan
https://avionicsduino.com/index.php/en/can-bus
- CAN sifatnya bidirectional, async, half duplex serial   
- ngga ada kabel clk signal    
- semua device yg connect harus punya baud rate sama     
-  The same cable (the bus) connects several devices or nodes (control units, sensors, etc.)    
-  The physical layer of the CAN protocol consists of the bus itself (two wires transmitting the data) and all the controllers (one per node)   
### BUS 
<img width="1125" height="207" alt="image" src="https://github.com/user-attachments/assets/7a32fac8-ba71-483a-b686-aa1d7725d8b9" />
- kabel twisted dan ujungnya ada R = 120 ohm    
- tiap node connect ke twisted wire itu, semakin panjang kabel maka speed bus menurun   
<img width="1181" height="805" alt="image" src="https://github.com/user-attachments/assets/8f4ce4f0-3862-46d5-acb1-eb0df95066f8" />
- topologi koneksi harus bentuk linear, yg harus dihindari bentuk star   
- Kalo jarak antara bus ke node lebih dari 30 cm maka pake konfigurasi:  
<img width="1171" height="494" alt="image" src="https://github.com/user-attachments/assets/726544e6-ab99-4ae9-902c-08c15704d4c5" />

> Diverting the bus toward a node rather than overextending a T-connection is better
- data dikirim sebagai dalam bentuk voltage di 2 kabel itu sebagai CAN_H dan CAN_L (untuk menghindari gangguan elektromagnetik)

### CONTROLLER
- tiap node ngga cuma ada sensor/aktuator, tpi ada **mikrokontroler, CAN controller, CAN tranciever**   
- tranciever dipake buat connect mikro (32 bit) ke CAN controller   
- Semua node yg connect punya tingkatan sama, **ngga ada master-slave**   
- Tiap message ada **Identifier** berisi content dan priority dari message tsb (diatur sm programmer)    
- semua node constantly listen ke bus dan nerima semua message, jadi ini bergantung sama indentifier whether node itu perlu nerima atau ignore data   
- perlu sistem buat nentuin priority node: **the lower binary value dari identifier, the higher the message priority**    
- kalo lower priority kalah karena higher priority maka node akan stop ngetransmit dan akan ngirim lagi kalo bus sudah free    

### MESSAGE STRUCTURE
- standard CAN: data field 9 bytes, max rate 1Mbit/s, 2 ver: CAN 2.0A (11 bit identification field), 2.0B (29 bits)    
- recent CAN (CAN FD(Flexible Data rate)): messages 64 bytes, max rate 2Mbit/s   
<img width="1200" height="670" alt="image" src="https://github.com/user-attachments/assets/9f84831e-b4b9-4b52-aff1-ecc3aacbb8af" />

| Field (jumlah bit) | Deskripsi |
|---------------------|-----------|
| **SOF (1)** | *Start Of Frame* — menandai awal dari pesan. |
| **Identifier (11)** | Bidang pengenal 11-bit CAN yang menunjukkan identitas data. |
| **RTR (1)** | *Remote Transmission Request* — bernilai LOW saat node lain meminta informasi. |
| **IDE (1)** | *Identifier Extension bit* — menunjukkan apakah menggunakan pengenal CAN standar atau extended. |
| **R0 (1)** | Dicadangkan untuk penggunaan di masa mendatang. |
| **DLC (4)** | *Data Length Code* — jumlah byte dalam transmisi. |
| **Data (0–64)** | Data yang dikirimkan. |
| **CRC (16)** | *Cyclic Redundancy Check* — berisi checksum (jumlah bit yang dikirim) dari data sebelumnya untuk deteksi kesalahan transmisi. |
| **ACK (2)** | *Acknowledge bit* — jika pesan diterima dengan benar, node lain menimpa bit ini menjadi LOW. Jika terjadi kesalahan, bit dibiarkan HIGH dan pesan diabaikan. |
| **EOF (7)** | *End Of Frame* — menandai akhir dari setiap frame (pesan) CAN. |
| **IFS (3+)** | *Inter Frame Space* — waktu yang dibutuhkan pengendali untuk memindahkan frame ke buffer. Terdiri dari minimal tiga bit resesif berturut-turut sebelum bit dominan berikutnya menandai SOF dari frame berikutnya. |

### SECURITY & RELIABILITY
- di CAN ada error detection dengan monitoring dan detect kalo ada perbedaan antara bits yg dikirim dan diterima   
- kalo diflag sebagai Error Flag maka transmitter harus ngirim ulang   

## HARDWARE PART
### CONTROLLERS & TRANCIEVERS
- controller berupa microcontroller. di controller 32 bit biasanya udah ada module tranciever (ex di umum: TJA1050, MCP2551)   
- cara kerja: tranciver convert TTL/CMOS signal dari CAN controller ke signal yg membawa informasi lewat kabel CAN_H dan CAL_L (ini membentuk CAN bus)    
<img width="1151" height="666" alt="image" src="https://github.com/user-attachments/assets/30f35b56-67aa-498c-bd90-557fbf8e0ee2" />

> Complete diagram of a CAN bus with microcontrollers, CAN controllers, and transceivers.   

### CABLE & CONNECTORS
- ada 3 kabel: ground, 2 kabel data twisted. Tapi bisa juga pake ethernet cable   
- di aircraft, semua node harusnya share common ground    
<img width="1165" height="758" alt="image" src="https://github.com/user-attachments/assets/0be0cf0c-d8db-4802-acbe-02d913a667fd" />
- kabel diusahaan pendek. kalo memang perlu panjang bisa pake SUB-D connector   
<img width="1024" height="387" alt="image" src="https://github.com/user-attachments/assets/712049af-d361-4a7f-a5d9-f8c735991640" />

## SOFTWARE PART
- perlu menentukan ID asssignment ke network tergantung prioritasnya     
- ngeprogram microcontroller dari setiap node (cari library yg sesuai)    

## CONTOH KONFIGURASI
<img width="1162" height="340" alt="image" src="https://github.com/user-attachments/assets/fa26b190-e5e8-48a9-b25e-1576ad587f7f" />

> Architecture of the CAN and UART communication network. First configuration.
- contoh data di aircraft CAN Bus   
<img width="1171" height="598" alt="image" src="https://github.com/user-attachments/assets/5dddc848-9ece-4b80-8551-ecf3a65e9d15" />

> Pada konfigurasi ini, dengan memperkirakan panjang total pesan sekitar 130 bit (untuk 8 byte data), 115 bit (untuk 6 byte), dan 100 bit (untuk 4 byte), serta mempertimbangkan frekuensi pengiriman datanya, kita dapat menghitung perkiraan beban bus sebesar 13,825 Kbit/s, atau hanya 2,7% dari kapasitas maksimum bandwidth bus. Artinya, sistem ini masih jauh dari risiko kehilangan data (data loss).
