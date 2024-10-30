# digital-dial-indicator-interface
a program to monitor digital dial indicator in realtime
1-Communication between host computer and hub, Read the internal parameters of the hub command。
command (8): FF 03 02 00 00 04 50 6F
Byte1 FF Address code
Byte2 03 Function code
Byte<3.4> 02 00 Access register first address
Byte<5.6> 00 04 Data word length
Byte7 CRC(Low 8 Bit)
Byte8 CRC(high 8 Bit)
feedback (13): FF 03 08 00 80 00 02 00 00 00 10 5B F8
Byte1 FF Address code
Byte2 03 Function code
Byte3 08 Data byte length（08 Representative feedback 8 Bytes）
Byte<4.5> 00 80 Hub Address (Represents decimal 128)
Byte<6.7> 00 02 Baud rate (0000：9600； 0001:19200； 0002：38400)
Byte<8.9> 00 00 Verification method (No verification)
Byte<10.11> 00 00 Micrometer data bytes（16 Micrometer data bytes，4 Road Micrometer）
Byte<11.12> 5B F8 CRC Verification result 2 bytes（Based on the CRC calculation of all the previous 11 bytes,）
Remark：The above is the feedback data of the four-way hub., The following are 32 Road feedback. Others until 60 The road is similar，I will explain each one in detail here.
feedback 13): FF 03 08 00 80 00 02 00 00 00 20 5B EC（address 128，Baud rate38400，Check None，Micrometer 8 indivual）
feedback（13） FF 03 08 00 80 00 02 00 00 00 80 5B 94（address 128，Baud rate 38400，Check None，Micrometer 32 indivual）
2. Read hub data command。
command (8): 80 03 00 00 FF FF 5A 6B or 80 03 00 00 00 08 5A 1D
Byte1 80 Address code
Byte2 03 Function code
Byte<3.4> 00 00 Access register first address
Byte<5.6>FF FF or 00 08 Data word length
Byte<7.8> CRC Check Byte
feedback (21):80 03 10 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 F0 7B
Byte1 80 Address code
Byte2 03 Function code
Byte3 10 Data byte length（Decimal 16 Bytes，represent 4 Micrometer）
Byte<4.19> 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 Micrometer data（here 4 The road is full 0）
Byte<20.21> F0 7B CRC Verification Results（According to all the above 19 BytesCRC Calculation）
Remark：Detailed notes above 4 Hub Feedback Information，The following is the eight-way hub feedback format. Others until 60 The road is similar，This is not explained in detail。
feedback (37): 80 03 20 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 3F 6C
3. Clear
<3.1> Clear all micrometer data
command (8): 80 06 08 00 AB 56 6A B5
Byte1 80 Address code
Byte2 06 Function code
Byte<3.4> 08 00 Access register first address
Byte<5.6> AB 56 Clear Command
Byte<7.8> 6A B5 CRC Check digit
The hub feeds back the received data to the host computer
<3.2> Clears the specified micrometer data
example 1 Wirte(8): 80 06 00 00 AB 56 68 D5
Byte1 80 Address code
Byte2 06 Function code
Byte<3.4> 00 00 Access register first address (No. 1 Micrometer）
Byte<5.6> AB 56 Clear Command
Byte<7.8> 6B D5 CRC Check digit
The hub feeds back the received data to the host computer
example 2 Wirte(8): 80 06 00 06 AB 56 88 D4
Byte1 80 Address code
Byte2 06 Function code
Byte<3.4> 00 06 Access register first address (No. 4 Micrometer）
Byte<5.6> AB 56 Clear Command
Byte<7.8> 88 D4 CRC Check digit
The hub feeds back the received data to the host computer
example 3 Wirte(8): 80 06 00 0E AB 56 09 16
Byte1 80 Address code
Byte2 06 Function code 
Byte<3.4> 00 0E Access register first address (No. 8 Micrometer）
Byte<5.6> AB 56 Clear Command
Byte<7.8> 09 16 CRC Check digit
The hub feeds back the received data to the host computer

example 4 Wirte(8): 80 06 00 42 AB 56 C8 C1
Byte1 80 Address code
Byte2 06 Function code
Byte<3.4> 00 42 Access register first address (No.34 Micrometer）
Byte<5.6> AB 56 Clear Command
Byte<7.8> C8 C1 CRC Check digit
The hub feeds back the received data to the host computer
4. Modify the hub address
command (8): 80 06 02 00 00 01 57 A1
Byte1 80 Address code
Byte2 06 Function code
Byte<3.4> 02 00 Access register first address
Byte<5.6> 00 01 Modified hub address
Byte<7.8> 57 A1 CRC Check digit
feedback (8): 80 06 02 00 00 01 57 A1
Byte1 80 Address code
Byte2 06 Function code
Byte<3.4> 02 00 Access register first address
Byte<5.6> 00 01 Modified hub address
Byte<7.8> 57 A1 CRC Check digit
5. Modify the hub baud rate
command (8): 80 06 02 01 00 01 06 63
Byte1 80 Address code
Byte2 06 Function code
Byte<3.4> 02 01 Access register first address
Byte<5.6> 00 01 Modified hub baud rate
Byte<7.8> 06 63 CRC Check digit
feedback (8): 80 06 02 00 00 01 57 A1
Byte1 80 Address code 
Byte2 06 Function code 
Byte<3.4> 02 01 Access register first address
Byte<5.6> 00 01 Modified hub baud rate
Byte<7.8> 06 63 CRC Check digit



6. Modify the hub verification method
command (8): 80 06 02 02 00 01 XX XX
Byte1 80 Address code
Byte2 06 Function code
Byte<3.4> 02 02 Access register first addressByte<5.6> 00 01 Modified hub verification method
Byte<7.8> XX XX CRC Check digit
feedback (8): 80 06 02 02 00 01 XX XX
Byte1 80 Address code 
Byte2 06 Function code 
Byte<3.4> 02 02 Access register first address
Byte<5.6> 00 01 Modified hub baud rate
Byte<7.8> XX XX CRC Check digit
Hub and Micrometer Communication
Read micrometer data command.
command (8): 01 03 00 00 00 02 C4 0B
Byte1 01 Address code
Byte2 03 Function code
Byte<3.4> 00 00 Access register first address
Byte<5.6> 00 02 Data word length
Byte<7.8> CRC Check Byte
feedback (21):01 03 04 00 00 00 00 FA 33
Byte1 01 Address code 
Byte2 03 Function code 
Byte3 04 Data byte length
Byte<4.7> 00 00 00 00 00 Micrometer data（Here are all 0）
Byte<8.9> FA 33 CRC Verification Results（According to all the above 19 Bytes CRC Calculation）
3. Clear
<3.1> Clear micrometer data
command (8): 01 06 08 00 AB 56 74 A4
Byte1 01 Address code
Byte2 06 Function code
Byte<3.4> 08 00 Access register first address
Byte<5.6> AB 56 Clear Command
Byte<7.8> 74 A4 CRC Check digit
