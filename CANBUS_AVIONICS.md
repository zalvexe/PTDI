# MORE ABOUT CAN BUS IN AVIONICS

## OVERVIEW
Source:     
a. https://www.rapid7.com/research/report/investigating-can-bus-network-integrity-in-avionics-systems   
b. https://experimentalavionics.com/can-bus    
- CAN bus jadi pusat control & sensor system di avionics buat collect data kayak ***altitude, airspeed, engine parameter (fuel level, oil pressure)*** terus didisplay ke pilot
  <img width="600" height="317" alt="image" src="https://github.com/user-attachments/assets/4aef83a1-46bb-40ca-974d-a1829ef23b00" />
-  CAN pake differential signaling jadi bisa menghindari EMI
-  2 kabel: CAN_LOW dan CAN_HIGH. kedua wire nya pakai common voltage 2,5v (represent binary 1). Saat jadi 0, CAN_H jadi 4v dan CAN_L jadi 1v (nilai voltage bisa beda tergantung kondisi, tpi konsepannya sama)
-  pake USB2CAN
-  CAN bus message yang dari avionics bisa direcord di Linux lewat CAN-utils
-  Bentuk CAN ID, ex: 0x205 responsible buat oil pressure, oil temp. Ox241 responsible buat compass
### Cables & Connectors
- Biasanya, kabel CAN-Bus ada 4 kabel: satu pasang kabel twisted untuk jalur CAN itu sendiri, dan satu pasang lainnya untuk menyalurkan daya ke perangkat (ex: sensor)
- buat jarak pendek (< 0,3 m / 1 kaki), penggunaan kabel pita murah (ribbon cable) masih diperbolehkan.
- Untuk pengujian di meja kerja (workbench testing), kabel murah apa pun dapat digunakan tanpa masalah. Kabel jaringan komputer (stranded) juga bisa
- untuk instalasi di pesawat, disarankan pake  kabel **Tefzel ukuran 22 atau 20 AWG** karena lebih tahan terhadap kondisi ekstrem
- Konektor yang umum digunakan untuk membuat kabel CAN adalah **konektor D-Sub 9-pin (female)**
- Jika konektor solder digunakan untuk instalasi di pesawat, maka harus dipastikan kabel di bagian belakang konektor terpasang dengan kuat, agar tidak terjadi retak (fatigue cracks) pada tepi sambungan solder akibat getaran

### CAN Messages
- semua message pake standard 11 bit iD
- pake standard length (antara 1-8 bytes)
- tiap message bisa berisi 1/lebih informasi, ex: engine temp EGT dan CHT cylinder 1 dan 2 akan jadi satu di 8 byte message (2 byte buat tiap temp)

## VULNERABILITY
Source: https://www.aerospacemanufacturinganddesign.com/article/countering-can-bus-vulnerability
- di aircraft biasanya CAN bus ada di compartment yang accessible buat maintenance
- data yang biasanya didisplay: ***Altitude, Airspeed, Engine readings, Control/disable autopilot***   
<img width="326" height="150" alt="image" src="https://github.com/user-attachments/assets/de9039ba-4f3b-4523-bc22-feec2a6f4c26" />

- di CAN bus, data harus physically & electronically secured, selama ini CAN bus masih aman from being exploited karena physical security dari aircraft, airports, hangars
- potential exploit:
  
<img width="326" height="150" alt="image" src="https://github.com/user-attachments/assets/f6c78de4-08e4-4f1e-b5f3-192af296600e" />  

- potential false measurement yang bisa diinject sama attacker:
  1. Incorrect engine telemetry readings
  2. Incorrect compass and attitude data
  3. Incorrect altitude, airspeed, and angle of attack (AoA) data
