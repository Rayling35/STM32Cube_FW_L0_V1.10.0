/**
  @page IAP/IAP_Main AN4657 STM32L0xx In-Application Programming using the USART Readme file
  
  @verbatim
  ******************** (C) COPYRIGHT 2016 STMicroelectronics *******************
  * @file    IAP/IAP_Main/readme.txt 
  * @author  MCD Application Team
  * @brief   Description of the AN4657 "STM32L0xx in-application programming
  *          using the USART (IAP)".
  ******************************************************************************
  *
  * Copyright (c) 2016 STMicroelectronics International N.V. All rights reserved.
  *
  * Redistribution and use in source and binary forms, with or without 
  * modification, are permitted, provided that the following conditions are met:
  *
  * 1. Redistribution of source code must retain the above copyright notice, 
  *    this list of conditions and the following disclaimer.
  * 2. Redistributions in binary form must reproduce the above copyright notice,
  *    this list of conditions and the following disclaimer in the documentation
  *    and/or other materials provided with the distribution.
  * 3. Neither the name of STMicroelectronics nor the names of other 
  *    contributors to this software may be used to endorse or promote products 
  *    derived from this software without specific written permission.
  * 4. This software, including modifications and/or derivative works of this 
  *    software, must execute solely and exclusively on microcontroller or
  *    microprocessor devices manufactured by or for STMicroelectronics.
  * 5. Redistribution and use of this software other than as permitted under 
  *    this license is void and will automatically terminate your rights under 
  *    this license. 
  *
  * THIS SOFTWARE IS PROVIDED BY STMICROELECTRONICS AND CONTRIBUTORS "AS IS" 
  * AND ANY EXPRESS, IMPLIED OR STATUTORY WARRANTIES, INCLUDING, BUT NOT 
  * LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY, FITNESS FOR A 
  * PARTICULAR PURPOSE AND NON-INFRINGEMENT OF THIRD PARTY INTELLECTUAL PROPERTY
  * RIGHTS ARE DISCLAIMED TO THE FULLEST EXTENT PERMITTED BY LAW. IN NO EVENT 
  * SHALL STMICROELECTRONICS OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT,
  * INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
  * LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, 
  * OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF 
  * LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING 
  * NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE,
  * EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
  *
  ******************************************************************************
  @endverbatim

@par Application Description

This directory contains a set of sources files and pre-configured projects that 
describes how to build an application to be loaded into Flash memory using
In-Application Programming (IAP, through USART).

@par Directory contents

 - "IAP/IAP_Main/Inc": contains the IAP firmware header files 
    - IAP/IAP_Main/Inc/main.h              The main include file of the project.
    - IAP/IAP_Main/Inc/common.h            This file provides all the headers of the common functions.
    - IAP/IAP_Main/Inc/flash_if.h          This file provides all the firmware 
                                                    function headers of the flash_if.c file.
    - IAP/IAP_Main/Inc/menu.h              This file provides all the firmware
                                                    function headers of the menu.c file.
    - IAP/IAP_Main/Inc/ymodem.h            This file provides all the firmware
                                                    function headers of the ymodem.c file.
    - IAP/IAP_Main/Inc/stm32l0xx_hal_conf.h  Library Configuration file
    - IAP/IAP_Main/Inc/stm32l0xx_it.h      Header for stm32l0xx_it.c

 - "IAP/IAP_Main/MDK-ARM": contains pre-configured project for MDK-ARM toolchain

 - "IAP/IAP_Main/EWARM": contains pre-configured project for EWARM toolchain

 - "IAP/IAP_Main/TrueSTudio": contains pre-configured project for TrueStudio toolchain

 - "IAP/IAP_Main/SW4STM32": contains pre-configured project for SW4STM32 toolchain

 - "IAP/IAP_Main/Src": contains the IAP firmware source files
    - IAP/IAP_Main/Src/main.c              Main program
    - IAP/IAP_Main/Src/stm32l0xx_it.c      Interrupt handlers
    - IAP/IAP_Main/Src/stm32l0xx_hal_msp.c Microcontroller specific packages
                                                     initialization file.
    - IAP/IAP_Main/Src/flash_if.c          The file contains write, erase and disable
                                                    write protection of the internal Flash
                                                    memory.
    - IAP/IAP_Main/Src/menu.c              This file contains the menu to select
                                                    downloading a new binary file, uploading
                                                    internal Flash memory, executing the binary
                                                    and disabling the write protection of
                                                    write-protected pages
    - IAP/IAP_Main/Src/common.c            This file provides functions related to
                                                    read/write from/to USART peripheral
    - IAP/IAP_Main/Src/ymodem.c            This file provides all the firmware functions
                                                    related to the ymodem protocol.
    - IAP/IAP_Main/Src/system_stm32l0xx.c  STM32L0xx system source file

@par Hardware and Software environment

  - This example runs on STM32L0xx Ultra Low Power Devices.
  - This example has been tested with STMicroelectronics STM32L073Z-EVAL evaluation board
    and can be easily tailored to any other supported device and development board.
  
Table 1. IAP implementation on STM32L073Z-EVAL
/*** Platform ***|************* Implementation **************************|***** Configuration *****\
****************************************************************************************************
|    Firmware    | The IAP program is located at 0x08000000. The Flash   |                         |
|                | routines (program/erase) are executed from the Flash  |                         |
|                | memory.                                               |                         |
|                | The maximum size of this program (wo optimization)    |                         |
|                | is about 13 Kbytes and                                |                         |
|                | programmed on:                                        | Sector 0                |  
|                | ------------------------------------------------------|-------------------------|
|                | The user application (image to be downloaded with the |                         | 
|                | IAP) will be programmed starting from address         |                         |
|                | (uint32_t)0x08004000(1).                              | (Sector 4 - Sector 47)  | 
|                | The maximum size of the image to be loaded is:        | (176 Kbytes             | 
|                | ------------------------------------------------------|-------------------------|
|                | The image is uploaded with the IAP from the STM32L0xx | 12 Kbytes               | 
|                | internal Flash.                                       | (Sector 4 - Sector 23)  |
|                | The size of the image to be uploaded is:              |                         |
|----------------|-------------------------------------------------------|-------------------------|
|    Hardware    | Push-button (active level: high)                      | Tamper push-button      |                                                                     
|                |                                                       | connected to pin PC13   |
|                | ------------------------------------------------------|-------------------------| 
|                | USART used                                            |  USART2   (CN6)         |
\**************************************************************************************************/ 
(1) User application location address is defined in the flash_if.h file as: 
#define APPLICATION_ADDRESS           ((uint32_t)0x08004000)
To modify it, change the default value to the desired one. Note that the application must be linked
relatively to the new address too.

Following picture illustrates the situation in program memory:
Figure 1. Flash memory usage

 Top Flash Memory address /-------------------------------------------\  0x0802FFFF
                          |                                           |
                          |                                           |
                          |                                           |
                          |                                           |
                          |                                           |
                          |          Bank 2                           |
                          |                                           |
                          |                                           |
                          |                                           |
                          |                                           |
                          |                                           |
                          |                                           |
                          |                                           |
                          |-------------------------------------------|  0x08018000
                          |                                           |
                          |          Bank 1                           |
 User code ceiling may    |-------------------------------------------|
 be in Bank 2 in some     |                                           |
 case.                    |          User code                        |
                          |                                           |
                          |- - - - - - - - - - - - - - - - - - - - - -|
 It is possible to con-   |          Vector table                     |
 figure the user code     |-------------------------------------------|  0x08004000
 starting in Bank 2.      |          IAP code                         |
                          |- - - - - - - - - - - - - - - - - - - - - -|
                          |          Vector table                     |
                          \-------------------------------------------/	 0x08000000					  
   
  - STM32L073Z-EVAL Set-up
    - Connect a null-modem female/female RS232 cable between the boards DB9 connector 
      and PC serial port.
    - Hold the Key push-button during reset to enter the IAP.    

  - Terminal configuration: 
    - Word Length = 8 Bits
    - One Stop Bit
    - No parity
    - BaudRate = 115200 baud
    - flow control: None 
    - Ymodem protocol is using CRC16 by default. To switch to checksum, comment #define CRC16_F
      in ymodem.c
      Note: the DOWNLOAD_TIMEOUT value used by Ymodem protocol can be adjusted if needed.

@par How to use it? 

In order to make the program work, you must do the following:

  1. Generate a binary image for the program provided in the 
     "IAP/IAP_Main/IAP_Binary_Template" project directory. 
  2. Program the internal Flash with the IAP (see below) 
  3. Open HyperTerminal window using the settings already defined in section
     "Hardware and Software environment" 
  4. To run the IAP driver, keep the Key push-button pressed at Reset. 
     The IAP main menu is then displayed on the HyperTerminal window.
  5. To download an application, press 1 and use the Ymodem protocol 
     by selecting in HyperTerminal menu: File->Transfer->Ymodem->Send
     to Download Application binary file to the internal STM32 Flash.

Then
- Open your preferred toolchain
- Rebuild all files and load your image into target memory
- Run the example
    
 * <h3><center>&copy; COPYRIGHT STMicroelectronics</center></h3>
 */
