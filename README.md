# PERANCANGAN MINI-BUS KOMUNIKASI DATA PADA SISTEM AVIONIK MENGGUNAKAN PROTOKOL CAN

## Sumber bacaan
https://avionicsduino.com/index.php/en/can-bus
- CAN sifatnya bidirectional, async, half duplex serial
- ngga ada kabel clk signal
- semua device yg connect harus punya baud rate sama
-  The same cable (the bus) connects several devices or nodes (control units, sensors, etc.)
-  The physical layer of the CAN protocol consists of the bus itself (two wires transmitting the data) and all the controllers (one per node)
### BUS 
<img width="940" height="164" alt="image" src="https://github.com/user-attachments/assets/ac129acc-959c-461a-9587-39112662455b" />
- kabel twisted dan ujungnya ada R = 120 ohm
- tiap node connect ke twisted wire itu, semakin panjang kabel maka speed bus menurun
<img width="852" height="572" alt="image" src="https://github.com/user-attachments/assets/d8e04a87-984e-45aa-b30d-95ba8785f496" />
- topologi koneksi harus bentuk linear, yg harus dihindari bentuk star
- Kalo jarak antara bus ke node lebih dari 30 cm maka pake konfigurasi: 
<img width="820" height="339" alt="image" src="https://github.com/user-attachments/assets/80308a0a-0964-484f-8f0f-f92abfe48264" />
> Diverting the bus toward a node rather than overextending a T-connection is better
- data dikirim sebagai dalam bentuk voltage di 2 kabel itu sebagai CAN_H dan CAN_L (untuk menghindari gangguan elektromagnetik)

### CONTROLLER
- tiap node ngga cuma ada sensor/aktuator, tpi ada **mikrokontroler, CAN controller, CAN tranciever**
- tranciever dipake buat connect mikro (32 bit) ke CAN controller
- Semua node yg connect punya tingkatan sama, **ngga ada master-slave**
- Tiap message ada **Identifier** berisi content dan priority dari message tsb (diatur sm programmer)
- semua node constantly listen ke bus dan nerima semua message, jadi ini bergantung sama indentifier whether node itu perlu nerima atau ignore data
- perlu sistem buat nentuin priority node: **the lower binary value dari identifier, the higher the message priority**
- 
