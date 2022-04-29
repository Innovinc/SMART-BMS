# SMART-BMS
Supports external communications UART,RS232,RS485,Bluetooth,CAN

#include "main.h"

/* Private include -------------------------------------------------------------*/ /USER CODE BEGIN INCLUDES/ // #include"ded.h"

/* Private include -------------------------------------------------------------*/ /USER CODE BEGIN INCLUDES/ #include "adc.h"

/* Private include -------------------------------------------------------------*/ /USER CODE BEGIN INCLUDES/ #include "soc.h"

/* Private include -------------------------------------------------------------*/ /USER CODE BEGIN INCLUDES/ #include "sochelper.h"

typedef enum {

IDLE=0, CHG=1,DCHG=2
} STATE;

STATE currentState= IDLE; uint8_t currentDchgPct=0; static float lastReadBattV=0; static float lastReadCurr=0 ; static float currentCellSoc=0 ; static float CurrentChargeRemaining=0 ; static const float fullChargeCapacity=2.6 ;

void getlatestADCValues()       // To get ADC values
{
	float battV_2=0;
	LTC2990_Trigger( &hi2C1 );       // For LTC 2990 Display 
	LTC2990_WaitForConversion(&hi2C1,100);

	//Current reading
	LTC2990_ReadVoltage(&hi2c1, BATTV,&lastReadBattV);
	LTC2990_ReadVoltage(&hi2c1, BATTV_2, &battV_2);
	LTC2990_ReadCurrent(&hi2c1, lastReadBattV, battV_2, &lastReadCurr_mA);
}

Void update0LED()
{
	// show current on 0LED
	char currentString[10];
sprintf(currentString, "%3f mA", lastReadCurr_mA);

ssd1306_SetCursor(15,37); ssd1306_WriteString("cur", Font_710, White); ssd1306_setCursor(52,37); ssd1306_WriteString(currentString, font_710, White);

//Battery Voltage

char battVString[10]; Sprintf(battVString, "%3f V", lastReadBattV); ssd1306_SetCursor(15,49); ssd1306_WriteString("BattV ", Font_710, white); ssd1306_SetCursor(52,49); ssd1306_WriteString(battVString,Font_710, White);

//soc

char socString[10]; Sprintf(socString, "%.2f%", currentCellSOC); ssd1306_SetCursor(15,25); ssd1306_WriteString("soc ", Font_710, white); ssd1306_SetCursor(52,25); ssd1306_WriteString(SocString, Font_710, White); ssd_UpdateScreen(); }

void calcSOC(float ocv_V, float chargeRemain_Ah) { //Simple switch to start with if(currentState==IDLE) { // In order to do accuarately by considering polarization effect, utlising timer 17

	currentCellSoc=SocByOCV(OCV_V);
	currChargeRemaining= (currentCellSoc/100)* full Charge Capacity;
}
else
{
	CurrentCellSoc= ( currChargeRemaining/fullChargeCapacity)* 100;
}
} /* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/

static void TIM17_Init(void); static void getlatestADCValues(void); static void updateOLED(void); static void calcSoc(float ocv_V,float chargeRemain_Ah);

Timer elapsed function }

else if (htim==& htim17) { Safety_loop(); } else if(htim==&htim16) { if(currentState==DCHG||currentState==CHG) { currentChargeRemaining == CalcdeltaAh(1,lastReadCurr/1000.0); } }

else if(htim==&htim17);

{ } while(1)
{ getlatestADCValues(); updateOLED(); calcSOC(lastReadBattV, currChargeRemaining);

}
}

float socByCV(float OCV)

{ return lookupSOCByOCV(ocv,&defaultOcvTable,defaultTableSize,&defaultSocTable);

}
