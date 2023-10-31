```
Name: E. Varsha Sharon
Register Number: 212222100058
```
# EXPERIMENT 05 INTERFACING A 4X4 MATRIX KEYPAD AND DISPLAY THE OUTPUT ON LCD

## Aim: 
To Interface a 4X4 matrix keypad and show the output on 16X2 LCD display to ARM controller , and simulate it in Proteus
## Components required: 
STM32 CUBE IDE, Proteus 8 simulator .
## Theory:
<img src="https://github.com/vasanthkumarch/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/36288975/2a4a795e-1674-4329-ae07-3f5e8d5073e2" width=440, height=450>


4×4 Keypad Module Pin Diagram
 
4x4 Keypad module Pin Diagram
4×4 Keypad module Pin Diagram
Pin Number	Pin Name	Description
1	R1	Taken out from 1st  ROW
2	R2	Taken out from 2nd  ROW
3	R3	Taken out from 3rd  ROW
4	R4	Taken out from 4th  ROW
5	C1	Taken out from 1st  COLUMN
6	C2	Taken out from 2nd COLUMN
7	C3	Taken out from 3rd  COLUMN
8	C4	Taken out from 4th  COLUMN
4×4 Matrix Keypad Module Hardware Overview
These Keypad modules are made of thin, flexible membrane material. The 4 x4 keypad module consists of 16 keys, these Keys are organized in a matrix of rows and columns. All these switches are connected to each other with a conductive trace. Normally there is no connection between rows and columns. When we will press a key, then a row and a column make contact.

## Procedure : 
 ## LCD 16X2 
With 16 Columns and 2 Rows, the 16x2 LCD gets its name. Many combinations, such as 8×1, 8×2, 10×2, 16×1, etc., are possible. However, we are using the 16*2 LCD here because it is the most popular.

You have a choice because all of the aforementioned LCD displays will have 16 pins and use the same programming method. 
The 16x2 LCD module's pinout and pin description are shown below:

<img src="https://user-images.githubusercontent.com/36288975/233858086-7b1a88a2-f941-475c-86c2-b3bae68bdf7e.png" width=440, height=450>
<img src="https://user-images.githubusercontent.com/36288975/233857710-541ac1c2-786c-4dfc-b7b5-e3a4868a9cb6.png" width=440, height=450>
<img src="https://user-images.githubusercontent.com/36288975/233857733-05df5dbf-1a1e-479e-85bb-8014a39ad878.png" width=440, height=450>

4-bit and 8-bit Mode of LCD:

The 4-bit mode and the 8-bit mode are the two operating modes for the LCD. 
When using the 4 bit method, we transmit the data one bit at a time—upper bit first, lower bit last. 
The lower four bits (D0-D3) of a byte create the lower nibble, while the upper four bits (D4-D7) of a byte make the higher nibble. For those of you who are unfamiliar with the term, nibble refers to a group of four bits. We can now communicate 8 bit data thanks to this.

In contrast, since we use all 8 data lines in 8 bit mode, we may transfer 8 bit data straight in a single stroke.
 LCD Commands:

There are some preset commands instructions in LCD, which we need to send to LCD through some microcontroller. Some important command instructions are given below:

Hex Code

Command to LCD Instruction Register

0F

LCD ON, cursor ON

01

Clear display screen

02

Return home

04

Decrement cursor (shift cursor to left)

06

Increment cursor (shift cursor to right)

05

Shift display right

07

Shift display left

0E

Display ON, cursor blinking

80

Force cursor to beginning of first line

C0

Force cursor to beginning of second line

38

2 lines and 5×7 matrix

83

Cursor line 1 position 3

3C

Activate second line

08

Display OFF, cursor OFF

C1

Jump to second line, position 1

OC

Display ON, cursor OFF

C1

Jump to second line, position 1

C2

Jump to second line, position 2
 
## Procedure:
1. Select STM 32 CUBE IDE to bring up the screen that follows. 

 2. Select the New STM 32 Project option under FILE. 

3. As indicated below, choose the target to be programmed, then click the next button. 

5. An automated equivalent IOC file will be created. 

6. Choose the necessary pins to setup, such as gipo, in or out, USART, or necessary settings. 

7.Press Ctrl+S to automatically produce all C programmes. 

8. Modify the software as necessary 

9. Include the LCD 16x2 library files as needed, create the programme, utilise the project, and build  
11. Select the debug menu. 

12. Starting the simulation and creating the Proteus project
We have finally reached the last section of the comprehensive tutorial on simulating the STM32 project. 

13. Create a new Proteus project and place STM32F40xx i.e. the same MCU for which the project was created in STM32Cube IDE. 

14. Double click on the the MCU part to open settings.
15. Next to the Program File option, give full path to the Hex file generated using STM32Cube IDE. Then set the external crystal frequency to 8M (i.e. 8 MHz). Click OK to save the changes.
https://engineeringxpert.com/wp-content/uploads/2022/04/26.png

16. click on debug and simulate using simulation

## STM 32 CUBE PROGRAM :
```
#include "main.h"
#include <stdbool.h>
#include "lcd.h"

bool col1,col2,col3,col4;

void SystemClock_Config(void);
static void MX_GPIO_Init(void);
void key();

void key()
{
	Lcd_PortType ports[] = {GPIOA,GPIOA,GPIOA,GPIOA};
	Lcd_PinType pins[] ={GPIO_PIN_3,GPIO_PIN_2,GPIO_PIN_1,GPIO_PIN_0};
	Lcd_HandleTypeDef lcd;
	lcd = Lcd_create(ports,pins,GPIOB,GPIO_PIN_0,GPIOB,GPIO_PIN_1,LCD_4_BIT_MODE);

	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_0,GPIO_PIN_RESET);
	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_1,GPIO_PIN_SET);
	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_2,GPIO_PIN_SET);
	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_3,GPIO_PIN_SET);

	col1 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_4);
	col2 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_5);
	col3 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_6);
	col4 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_7);
	if(!col1)
	{
		Lcd_cursor(&lcd,0,1);
		Lcd_string(&lcd,"key 7\n");
		HAL_Delay(500);
	}
	else if(!col2)
	{
		Lcd_cursor(&lcd,0,1);
		Lcd_string(&lcd,"key 8\n");
		HAL_Delay(500);
	}
	else if(!col3)
	{
		Lcd_cursor(&lcd,0,1);
		Lcd_string(&lcd,"key 9\n");
		HAL_Delay(500);
	}
	else if(!col4)
	{
		Lcd_cursor(&lcd,0,1);
		Lcd_string(&lcd,"key %\n");
		HAL_Delay(500);
	}

int main(void)
{

  HAL_Init();
  SystemClock_Config();
  MX_GPIO_Init();

  while (1)
  {
      key();
  }

}


void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};


  __HAL_RCC_PWR_CLK_ENABLE();
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE2);

  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSI;
  RCC_OscInitStruct.HSIState = RCC_HSI_ON;
  RCC_OscInitStruct.HSICalibrationValue = RCC_HSICALIBRATION_DEFAULT;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }


  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSI;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
}


static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};

  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();


  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3, GPIO_PIN_RESET);

  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3, GPIO_PIN_RESET);

  HAL_GPIO_WritePin(GPIOB, GPIO_PIN_0|GPIO_PIN_1, GPIO_PIN_RESET);

  GPIO_InitStruct.Pin = GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOC, &GPIO_InitStruct);

  GPIO_InitStruct.Pin = GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);

  GPIO_InitStruct.Pin = GPIO_PIN_4|GPIO_PIN_5|GPIO_PIN_6|GPIO_PIN_7;
  GPIO_InitStruct.Mode = GPIO_MODE_INPUT;
  GPIO_InitStruct.Pull = GPIO_PULLUP;
  HAL_GPIO_Init(GPIOC, &GPIO_InitStruct);

  GPIO_InitStruct.Pin = GPIO_PIN_0|GPIO_PIN_1;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOB, &GPIO_InitStruct);

}


void Error_Handler(void)
{

  __disable_irq();
  while (1)
  {
  }

}

#ifdef  USE_FULL_ASSERT

void assert_failed(uint8_t *file, uint32_t line)
{

}
#endif
```
## CIRCUIT DIAGRAM 
### Button OFF
<img src="https://github.com/varshasharon/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/98278161/9a0c6c1a-6ece-44b5-a030-d9c23239dedd" width=440, height=450>
 
### Button ON
<img src="https://github.com/varshasharon/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/98278161/ca2c3c92-8e13-4c94-9525-f9cb3572f8c7" width=440, height=450>


## Output screen shots of proteus  :
 
<img src="https://github.com/varshasharon/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/98278161/8f7fd858-ceab-46b9-995e-84665f2596c1" width=440, height=450>

 
## Result :
Interfacing a 4x4 keypad with ARM microcontroller are simulated in proteus and the results are verified.
