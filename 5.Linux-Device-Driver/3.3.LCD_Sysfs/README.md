
![Screenshot from 2023-12-03 09-39-11](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/34576104/887b2ecc-2820-457d-887a-794e8ba2796d)

**Steps**
- Connect CD to BBB
- Add required gpio entries to the gpio-sysfs device tree node
- Recompile the dts file and make BBB boots with modified dtb
- Load the gpio-sysfs driver
- Make sure that all required gpio devices are formed under /sys/class/bone_gpios
- Download the lcd application files attached with this video. lcd_app.c, lcd.c, gpio.c
- Cross compile the lcd application and test it on the target
  
![Screenshot from 2023-12-03 09-47-44](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/34576104/15d4f6da-c50b-4fdd-8e58-4baa2535b6d6)
![Screenshot from 2023-12-03 09-47-57](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/34576104/1c3bfaf4-cb3d-4442-a230-f598be98781b)

![Screenshot from 2023-12-03 09-49-04](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/34576104/b8c127b4-dbbf-4b58-899c-c43bee708a39)



### LCD commands

- Function set
- Display on/off, cursor on/off and blink control
- Entry mode set
- LCD clear display
- Cursor return home
- Set co-ordinates
- Display right/left shift
- Cursor on/off, blink on/off
- Address counter read/write

### Sending command/data
- A command or data byte is of 8bits (1byte)
- You can send all 8 bits in one go over 8 data lines, or you can split into 2 data transmissions of 4 bits each.
- For 4-bit data transmission, you only Sending command/data
- A command or data byte is of 8bits (1byte)
- You can send all 8 bits in one go over 8 data lines, or you can split into 2 data transmissions of 4 bits each.
- For 4-bit data transmission, you only need 4 data lines connected between LCD and MCU
- For 4-bit data transmission, you must use data lines D4 ,D5, D6,D7need 4 data lines connected between LCD and MCU
- For 4-bit data transmission, you must use data lines D4 ,D5, D6,D7

### Sending a command
- Create the command code
- Make RS pin low
- Make RnW pin low
- First send higher nibble(4-bits) of the command code to data lines
- Make LCD enable pin high to low ( when LCD detects high to low transition on enable pin it reads the data from the data lines )
- Next send the lower nibble of the command code to data lines
- Make LCD enable pin high to low ( when LCD detects high to low transition on enable pin it reads the data from the data lines )
- Wait for the instruction execution time before sending the next command or confirm the LCD is not busy by reading the busy flag status on D7 pin .



### Sending a data byte
- Make RS high
- Make RnW low
- First send higher nibble of the data to data lines
- Make LCD enable pin high to low ( when LCD detects high to low transition on enable pin it reads the data from the data lines )
- Next send the lower nibble of the data to data lines
- Make LCD enable pin high to low ( when LCD detects high to low transition on enable pin it reads the data from the data lines )




![Screenshot from 2023-12-03 09-51-14](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/34576104/b268f31a-3776-424d-97c3-0833311719df)

![Screenshot from 2023-12-03 09-53-30](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/34576104/052d8f47-ab38-4428-b5a9-0018bc36897f)

![Screenshot from 2023-12-03 09-53-55](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/34576104/b6778195-937a-4126-9680-aac9da422d5e)
![Screenshot from 2023-12-03 09-54-08](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/34576104/39b55385-bae4-495d-989f-17949dc27afb)

![Screenshot from 2023-12-03 09-54-25](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/34576104/c86749f5-a6f7-4421-ab4d-8d844efd3424)






