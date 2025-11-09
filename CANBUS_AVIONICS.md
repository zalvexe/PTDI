# MORE ABOUT CAN BUS IN AVIONICS

## CONCEPT
Source: https://www.rapid7.com/research/report/investigating-can-bus-network-integrity-in-avionics-systems
- CAN bus jadi pusat control & sensor system di avionics buat collect data kayak ***altitude, airspeed, engine parameter (fuel level, oil pressure)*** terus didisplay ke pilot
-  
## VULNERABILITY
Source: https://www.aerospacemanufacturinganddesign.com/article/countering-can-bus-vulnerability
- di aircraft biasanya CAN bus ada di compartment yang accessible buat maintenance
- data yang biasanya didisplay: ***Altitude, Airspeed, Engine readings, Control/disable autopilot***   
<img width="326" height="150" alt="image" src="https://github.com/user-attachments/assets/de9039ba-4f3b-4523-bc22-feec2a6f4c26" />

- di CAN bus, data harus physically & electronically secured, selama ini CAN bus masih aman from being exploited karena physical security dari aircraft, airports, hangars
- potential exploit:
  
<img width="326" height="150" alt="image" src="https://github.com/user-attachments/assets/f6c78de4-08e4-4f1e-b5f3-192af296600e" />  

- potential false measurement yang bisa diinject sama attacker:
  > Incorrect engine telemetry readings
  > Incorrect compass and attitude data
  > Incorrect altitude, airspeed, and angle of attack (AoA) data
