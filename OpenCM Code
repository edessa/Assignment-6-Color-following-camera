#define DXL_BUS_SERIAL1 1  //Dynamixel on Serial1(USART1)  <-OpenCM9.04
#define DXL_BUS_SERIAL2 2  //Dynamixel on Serial2(USART2)  <-LN101,BT210
#define DXL_BUS_SERIAL3 3  //Dynamixel on Serial3(USART3)  <-OpenCM 485EXP
 
#define ID_NUM 1
#define ID_NUM2 2

#define GOAL_SPEED 32
#define GOAL_POSITION 30

Dynamixel Dxl(DXL_BUS_SERIAL1);
void setup() {
  // put your setup code here, to run once:
  pinMode(BOARD_LED_PIN, OUTPUT);
  Dxl.begin(3);
  Dxl.wheelMode(ID_NUM); //joinMode() is to use position mode
  Dxl.wheelMode(ID_NUM2);
}

void loop() {
  // put your main code here, to run repeatedly: 
  if (SerialUSB.available()>0)
  {
     char x = SerialUSB.read();
     
     if(x > 127)
       Dxl.goalSpeed(ID_NUM, x);
     else
     {
       int f = (127+x+1023);    
       Dxl.goalSpeed(ID_NUM, f);
       }
     }
     delay(300);
    }
