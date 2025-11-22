# CANBUS_STM

some stuff i newly learned about can with stm:
- Id buat frame detentuin lewat STM, jadi bukan per ID buat 1 stm, tpi 1 Id isinya data yang mau kita kirim (something like that)
- Baudrate ditentuin lewat clock, prescaler, sama time quanta
<img width="805" height="509" alt="image" src="https://github.com/user-attachments/assets/968b5921-263d-4692-be03-e992a1853aa3" />

kalo berdasarkan config itu baud jadi 500000    
- tiap frame yang mau dikirim perlu didefine Id sama DLC (banyak data yang mau dikirim)    
```c
TxHeader.StdId = 0x446;
TxHeader.DLC = 8;
```
max data yang bisa dikirim buat 1 ID itu 8 byte, isinya 0 sampe 255 (uint8)
- buat ngirim beberapa frame bersamaan, perlu dicascade di dalem timer pake HAL_GetTick()
> idk how to explain the detail

## Testing
- di terminal:
``` 
sudo ip link set can0 up type can bitrate 500000
```
setelahnya command itu harusnya LED biru di usb2can nyala
- terus bisa cek yang dikirim sama STM pake
```
candump can0
```
- kalo berhasil keterima sama pc bakal muncul id, data data yang dikirim. Bisa diliat jg lewat LED di usbcan yang kedip"

## Problem Faced
- dicoba mode Loopback aman tapi mode Normal ada error 2097152 => turns out itu karena MCP (tranciever) blm dapet 5V
> karena ngambil 5v dari STM sedangkan STM cuma dapet 3.3v dari stlink:)
