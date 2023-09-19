# GPIO pins in BBB 
### 7 Seven Display Demo [Video](https://www.youtube.com/watch?v=ViHP4JsHoCw&ab_channel=PranabNandy)

<p align="center"> <img width="900" height="500" src="https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/d3955bf7-6ddb-4cbc-bf91-fe151acbc733"  /> </p>

### SOC Pin R9 ( ZCZ pin map on the Datasheet ) is connection to Board P9_3 Expansion header

- #### Pin details are there in the Datasheet of SOC
- #### Register details are there in the TRM
-  Control module --> register ---> **conf_** means **Pad(Pin) configuration register**
-  When a Pin is used in one functionality then it can not be used in another functionality unless you reconfigure it (**mode setting or pad configuration**)  
#### pad (pin) configuration register are present at offset 800h
#### pad config reg starts
                          = BASE_ADDS(of control module engine register) + 800h ;
                          = 0x44E1_0000  
                          
## Purpose of the Peripheral
The general-purpose interface combines **4 (GPIO) modules**. Each GPIO module provides **32 dedicated GPIO pins**. So the general-purpose interface supports up to **128 (4 × 32) pins**. These pins can be configured for the following applications:
- Data input (capture)/output (drive)
- Keyboard interface with a debounce cell
- Interrupt generation in active mode upon the detection of external events.
- Wake-up request generation in idle mode upon the detection of external events.

### 128 GPIOs Pin supported but only in expension header it is 44(p8)+ 23(p9) = 67
![Screenshot from 2023-09-19 21-58-22](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/23339576-17df-4150-af27-9188da24a24c)

```
/sys/kernel/debug#

# cat gpio

>>> P8_15 ===> gpio1[5] ==> GPIO37 
>>> GPIO1[6] = 1*32 +6 = GPIO38
>>> GPIO1_21 = 1*32 + 21 ==> 53 ( kernel friendly name)

# cat /sys/kernel/debug/pinctrl/44e20800.pinmux/pin
( Align with kernel assign pin number) ----> (This value matters to us)

/sys/kernel/debug/pinctrl/44e10800.pinmux#

# cat pins | more

# cat pinmux-pins

# grep pins | grep "pin 28"

# cat pingroups | more 
```

#### Pin Multiplexing is used ---- 1 pin used for 8 purposes

<p align="center"> <img width="800" height="400" src="https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/d4155bb1-c242-4647-98d7-925f9e3751a2"  /> </p>


![Screenshot from 2023-09-16 19-28-45](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/a27a39fe-daa7-4275-81e4-70bb8ec1c74c)



### User Leds
```
/sys/class/leds/#

# ls

# cd beaglebone:green:usr3

# ls

# cat trigger

# echo "none" > trigger

# echo "timer" > trigger

# echo "1000" > delay_on

# echo "5000" > delay_off

# echo "mmc1" > trigger

# cd /home/debian/  

# vi test.cpp

# sync
```

<p align="center"> <img width="900" height="500" src="https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/4531afd3-ef3d-4baa-be47-a21521017157" /> </p>

#### p8_16 (output low voltage GPIO pin)  ==> Resister ==> +ve 
#### p8_1  ==>  GND     ==> -ve

### External Red Green Yellow Leds
```
debian@BeagleBone:/sys/class/gpio/gpio46$ cat label 
P8_16
debian@BeagleBone:/sys/class/gpio/gpio46$ echo "out" > direction 

debian@BeagleBone:/sys/class/gpio/gpio46$ cat value 
0
debian@BeagleBone:/sys/class/gpio/gpio46$ echo 1 >value

debian@BeagleBone:/sys/class/gpio/gpio46$ cat active_low 
0
debian@BeagleBone:/sys/class/gpio/gpio46$ echo 1 > active_low 
debian@BeagleBone:/sys/class/gpio/gpio46$ echo 1 > value 
debian@BeagleBone:/sys/class/gpio/gpio46$ echo 0 > value
debian@BeagleBone:/sys/class/gpio/gpio46$ echo 0 > active_low 
debian@BeagleBone:/sys/class/gpio/gpio46$ echo 1 > value
debian@BeagleBone:/sys/class/gpio/gpio46$ echo 0 > value
```


<p align="center"> <img width="900" height="500" src="https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/ae4d8188-87d7-4e77-8a9f-a74644b6b492" /> </p>


### GPIO Pins Add and control
```
/sys/class/gpio/# echo 87 > export

/sys/class/gpio/gpio87# ls

/sys/class/gpio/gpio46# cat direction

# echo "out" > direction

# echo 1 > value  (LED is on)

# echo 0 > value
```


## P8 Expension Header
-  Software Pin number is given by Kernel
  
  | Header Pin Number | Software Pin Number |
  |:----------------:|----------:|
  | P8-1    | GNDs     |
  | P8-2    |  """"""""     |
  | P8-3    | 6     |
  | P8-4    | 7     |
  | P8-5    | 2     |
  | P8-6    | 3     |
  | P8-7    | 36     |
  | P8-8    | 37     |
  | P8-9    | 39     |
  | P8-10    | 38     |
  | P8-11    | 13     |
  | P8-12    | 12     |
  | P8-13    | 9     |
  | P8-14    | 10    |
  | P8-15    | 15     |
  | P8-16   | 14     |
  | P8-17    | 11     |
  | P8-18    | 35     |
  | P8-19    | 8     |
  | P8-20    | 33     |
  | P8-21   | 32     |
  | P8-22   | 5     |
  | P8-23  | 4     |
  | P8-24   | 1     |
  | P8-25  | 0     |
  | P8-26   | 31     |
  | P8-27   | 56     |
  | P8-28   | 58     |
  | P8-29  | 57     |
  | P8-30   | 59     |
  | P8-31   | 54     |
  | P8-32  | 55     |
  | P8-33   | 53     |
  | P8-34   | 51     |
  | P8-35   | 52     |
  | P8-36   | 50     |
  | P8-37   |  48    |
  | P8-38   | 49     |
  | P8-39   | 46     |
  | P8-40   | 47     |
  | P8-41   | 44     |
  | P8-42   | 45     |
  | P8-43   | 42     |
  | P8-44   | 43     |
  | P8-45   | 40     |
  | P8-46   | 41     |



## P9 Expension Header 

  | Header Pin Number | Software Pin Number |
  |:----------------:|----------:|
  | P9-1    | Power + GNDs    |
  | P9-2    | """"""""    |
  | P9-3    | """"""""    |
  | P9-4    | """"""""     |
  | P9-5    | """"""""     |
  | P9-6    | """"""""     |
  | P9-7    | """"""""     |
  | P9-8    | """"""""     |
  | P9-9    | """"""""     |
  | P9-10    | """"""""     |
  | P9-11    | 28     |
  | P9-12    | 30     |
  | P9-13    | 29     |
  | P9-14    |  18    |
  | P9-15    | 16     |
  | P9-16   | 19     |
  | P9-17    | 87     |
  | P9-18    | 86     |
  | P9-19    | 95     |
  | P9-20    | 94     |
  | P9-21   | 85     |
  | P9-22   | 84     |
  | P9-23  | 17     |
  | P9-24   | 97     |
  | P9-25  | 107     |
  | P9-26   | 96     |
  | P9-27   | 105     |
  | P9-28   | 103     |
  | P9-29  | 101     |
  | P9-30   | 102     |
  | P9-31   | 100     |
  | P9-32  | Analog Pins     |
  | P9-33   | """"""""     |
  | P9-34   | """"""""      |
  | P9-35   | """"""""      |
  | P9-36   | """"""""      |
  | P9-37   | """"""""      |
  | P9-38   | """"""""      |
  | P9-39   | """"""""      |
  | P9-40   | """"""""      |
  | P9-41   | #109, ##106     |
  | P9-42   | @89, @@104     |
  | P9-43   | GNDs     |
  | P9-44   | """"""""     |
  | P9-45   | """"""""      |
  | P9-46   | """"""""      |


![Screenshot from 2023-09-19 01-20-57](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/2cb67cb1-8e13-4457-bfd5-85009c6a0aa3)
