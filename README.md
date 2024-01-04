# PIC16/18 KS0108 Library / XC8 compiler

---

**Description:**  
+ A library to control KS0108-based GLCD (128x64) using PIC microcontrollers.
+ This is conversion from efthymios-ks/AVR-KS0108 library to be compatible with PIC uC using XC8 compiler.
+ I fixed some problems with this library. Major issue was that funciton repsonsible of displaying fonts have problem with displaying character that exceed page. The problem occurred with the 5x8 font. I rewrite the GLCD_PrintChar function to support different fonts.
+ Added new function: inverted text

**Configuration**
+ In KS0108_Settings.h:
+ Check if your LCD driver CS pins are active low or high:
```
//Chip Enable Pin
#define GLCD_ACTIVE_LOW		1
```
+ Set your LCD pinout:
```
#define GLCD_D0				E, 0
#define GLCD_D1				E, 1
#define GLCD_D2				E, 2
#define GLCD_D3				E, 3
#define GLCD_D4				E, 4
#define GLCD_D5				E, 5
#define GLCD_D6				E, 6
#define GLCD_D7				E, 7

#define GLCD_DI				D, 4
#define GLCD_RW				D, 3
#define GLCD_EN				D, 7
#define GLCD_CS1			D, 0
#define GLCD_CS2			D, 1
#define GLCD_RST			D, 2
```


+ **The display pins can be configured to any pin in any port in any sequence.**  
+ Instead of using delays, it checks for the busy flag status of the display.
+ Functions include:  
  + Basic operations.  
  + Geometrical objects (draw/fill/invert rectangle/circle/triangle etc.).  
  _These functions have been searched for and found in various Arduino libraries. Their logic and functionality is not to be credited to me. I have only gathered, studied and modified them so that they work under AVR-GCC environmemt._  
  + Printing text anywhere in the screen using custom imported fonts.  
  + Printing pictures.  
   _Read the bottom of "KS0108.h" to see all functions available._  
+ A screen buffer stored in RAM is used to complete the operations faster at the cost of memory size (128x64 pixels / 8 = 1kB).  
_You can edit the **GLCD_BufferWrite** and **GLCD_BufferRead** functions to change the buffer's location from the RAM to the EEPROM or any other external memory source._  
+ My custom I/O macros are required and are included.  

---  

**Memory usage:**  
+ Program: 0 bytes  
+ Data: 1033 bytes  
  + Display buffer: 1024 bytes (128x(64/8))
  + Display auxiliary data: 9 bytes  
  
**Compiler:**  
XC8
  
--- 
**How to create a new font:**  
(needs some advanced knowledge)  
 1. Download the free **GLCD Font Creator** from **MikroElektronika**, install it and run it.  
 2. If you are on Windows 7, run the program under **Windows XP (Service Pack 3) compatibility mode** (google it), else you won't be able to save fonts.  
 3. File > New Font > Import an Existing System Font.  
 4. Choose your font, press ok to each pop-up window and wait for the processing to finish.  
 5. Export for GLCD > mikroC > X-GLCD Lib > Samsung KS0108 > Save.  
 6. Change the extension of the produced file from **.c** to **.h**.
 7. Open the file and add **Include Guards** (google it again).  
 8. Import the file into your project and your code.  
 _Include Guards example:_  
    `#ifndef _FONTNAME_H_`  
    `#define _FONTNAME_H_`  
    `//The content of the file`  
    `#endif`  

---
  
![picture alt](https://raw.githubusercontent.com/kwiecinski/PIC-XC8-KS0108-NT7108-LCD-LIBRARY/master/doc/Demonstration.png)
  
--- 
