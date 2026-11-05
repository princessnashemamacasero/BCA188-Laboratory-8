1. Pin Table

| Interface | Signal | GPIO Used |
| :-------: | :----: | :-------: |
| UART      | TX     | 17        |
| UART      | RX     | 18        |
| I2C       | SDA    | 21        |
| I2C       | SCL    | 20        |
| SPI       | SCK    | 12        |
| SPI       | MOSI   | 11        |
| SPI       | MISO   | 13        |
| SPI       | CS     | 10        |

UART Loopback – The ESP32-S3 sends the message PING through its TX pin and checks if the same data returns through its RX pin.

I²C Scanner – The program searches for devices connected to the I²C bus and reports the addresses of any devices that respond.

SPI Loopback – A test byte (0xA5) is transmitted through MOSI and read back through MISO to check if the SPI connection is working.

2. Circuit Diagram

The diagrams below present the circuit connections for each communication interface tested using the ESP32-S3 board.

<img width="352" height="291" alt="UART Loopback Circuit Diagram" src="https://github.com/user-attachments/assets/f2138c5e-7ca4-41c8-b567-1dfdee2fda43" />

Figure 1. UART Loopback Circuit Diagram

<img width="362" height="322" alt="I²C Scanner Circuit Diagram" src="https://github.com/user-attachments/assets/647ccf99-daca-43f6-84dd-02fa5fb3a1fd" />

Figure 2. I²C Scanner Circuit Diagram

<img width="406" height="358" alt="_SPI Loopback Circuit Diagram" src="https://github.com/user-attachments/assets/35089ce5-9438-4ebe-8f2b-a03c5fb0c9e2" />

Figure 3.  SPI Loopback Circuit Diagram

4. Serial Monitor Outputs

Test 1 — UART With Jumper 

Expected: The Serial Monitor should display PING every second when the TX and RX pins are connected.

Observed: The ESP32 successfully received the transmitted message, showing PING repeatedly in the Serial Monitor.

<img width="1917" height="1017" alt="bb9182b0-793d-4726-b248-2f77f189220f" src="https://github.com/user-attachments/assets/d3a1b7ec-a99d-4d81-b9a7-1e31efd3383c" />

Test 2 — UART Without Jumper 

Expected: The Serial Monitor should stop receiving new PING messages when the TX-to-RX jumper is disconnected.

Observed: No new PING messages were received after removing the jumper. The previously displayed messages remained visible in the Serial Monitor.

<img width="1917" height="1020" alt="a64e4b6c-7172-4f3e-9dc1-40951d5e684e" src="https://github.com/user-attachments/assets/9a0f9981-9421-41f9-991c-3c0fc73ec821" />

Test 3 — I²C with Module

Expected: The I²C scanner should recognize the connected OLED display and show its I²C address.

Observed: The ESP32-S3 successfully found the OLED display at address 0x3C.

<img width="1917" height="1017" alt="1c9601f3-20b3-4d9f-bbd1-3092387e5132" src="https://github.com/user-attachments/assets/d560ca96-42a1-4ba0-ad2f-53f8eff1349a" />

Test 4 — I²C without Module

Expected: The scanner should display No I2C devices found when the OLED module is disconnected.

Observed: After disconnecting the OLED module, the Serial Monitor displayed No I2C devices found, indicating that no I²C devices were detected on the bus.

<img width="1917" height="1020" alt="e13bac06-cade-49eb-af7f-40cdc80f2910" src="https://github.com/user-attachments/assets/e04474d6-2cd2-4004-848c-651d54b2801b" />

Test 5 — SPI With Jumper 

Expected: The received byte should be the same as the transmitted value 0xA5 when MOSI and MISO are connected.

Observed: The ESP32 successfully received 0xA5, matching the transmitted data and confirming that the SPI loopback connection was working.

<img width="1917" height="1017" alt="4d0cca99-08fe-420c-bae1-f1da296416d3" src="https://github.com/user-attachments/assets/437e67b9-56d6-4af3-820c-72e91adf922b" />

Test 6 — SPI Without Jumper 

Expected: The received byte should be different from the transmitted value 0xA5 when the MOSI-to-MISO jumper is disconnected.

Observed: The ESP32 received 0x00 instead of 0xA5, showing that the transmitted data was no longer being received through the loopback connection.

<img width="1917" height="1018" alt="4ce039c8-565b-4e58-b8fd-bd4380f9ad1d" src="https://github.com/user-attachments/assets/cb4cae06-ba52-4c98-be4a-60819ca57e20" />

Test 7 — Sketch at 9600, monitor at 115200

Expected: The Serial Monitor should display unreadable or distorted characters because the communication baud rates are different.

Observed: The Serial Monitor displayed garbled characters, represented by square symbols, instead of readable PING messages.

<img width="1917" height="962" alt="c727c753-37d0-47d8-a308-c8864f9c7a36" src="https://github.com/user-attachments/assets/9f4ec51a-be2c-4432-8a66-b8f1f5e04fbd" />

Test 8 - Sketch at 9600, monitor at 9600

Expected: The Serial Monitor should display readable PING messages at one-second intervals when both baud rates are set to 9600.

Observed: The PING messages appeared clearly and repeatedly after setting the Serial Monitor baud rate to 9600, confirming that the baud rates matched.

<img width="1917" height="1017" alt="bb9182b0-793d-4726-b248-2f77f189220f" src="https://github.com/user-attachments/assets/776eb889-54e9-4b78-a41e-7adf86044020" />

4. Expected vs Observed

| Test                               | Expected                                      | Observed                                       |
| :--------------------------------: | :-------------------------------------------- | :--------------------------------------------- |
| UART with jumper                   | PING should appear every second               | PING was received continuously                 |
| UART without jumper                | No more PING messages should appear           | No new messages were received                  |
| I2C with module                    |   The connected OLED address should be found  | Device detected at 0x3C                        |
| I2C without module                 |   No device should be detected                | No I²C devices found                           |
| SPI with jumper                    | The received data should be 0xA5              | Received 0xA5 successfully                     |
| SPI without jumper                 | The received data should not match 0xA5       | Received 0x00                                  |
| Sketch at 9600, monitor at 115200	 | Output should appear as unreadable characters | Square symbols and garbled characters appeared |
| Sketch at 9600, monitor at 9600    |   PING should appear clearly                  | Readable PING messages appeared repeatedly     |

5. Questions

1. Why does UART need both sides to use the same baud rate, while SPI uses a clock line?
- UART needs both devices to have the same baud rate so they can send and receive data at the correct speed. SPI uses a clock line to control the timing of data transfer, so both devices can stay synchronized.

2. Why can two devices with the same address not share one I²C bus?
- Each I²C device needs a unique address so the controller can identify which device it wants to communicate with. If two devices have the same address, both may respond at the same time, causing communication conflicts.

3. Why is CS set HIGH between SPI transfers?
- CS (Chip Select) is set HIGH to tell the SPI device that it is not currently selected. This prevents it from responding to data transfers and allows other SPI devices to be selected when needed.
