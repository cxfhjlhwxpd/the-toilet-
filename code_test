#define rLedPin 6 //RGB-LED引脚R
#define gLedPin 5 //RGB-LED引脚G
#define bLedPin 3 //RGB-LED引脚B
#define irSensorPin 8 //红外人体感应模块信号输出
#define lightSensorPin A0 //光敏电阻分压电路信号输出
 
int ledR  = 0; //R Led 亮度
int ledG  = 0; //G Led 亮度
int ledB  = 0; //B Led 亮度
 
bool irReading;   //红外人体感应模块输出
int lightReading; //光敏电阻分压电路信号输出
bool onOffState; //小夜灯开关状态
 
unsigned long previousIRMillis;    //上一次检查红外传感器的时间
unsigned long previousLightMillis; //上一次检查光敏传感器的时间
int irCheckInterval = 500;         //红外传感器检查时间间隔
int lightCheckInterval = 1000;     //光敏传感器检查时间间隔
 
int colorIndex;             //颜色序列号
int colorChangeDelay = 1;   //颜色改变速度控制变量（数值越大，颜色改变速度越慢）
void setup() {
  //设置引脚为相应工作模式
  pinMode(rLedPin, OUTPUT); //pinMode(6, OUTPUT);
  pinMode(gLedPin, OUTPUT);
  pinMode(bLedPin, OUTPUT);
  pinMode(irSensorPin, INPUT);
  
  Serial.begin(9600); 
  Serial.println("Welcome to Taichi-Maker RGB Led Night-Light.");
  Serial.println("System Start Sensor Check.");
  
  irReading = digitalRead(irSensorPin);       //读取红外人体感应模块
  lightReading = analogRead(lightSensorPin);  //读取光敏电阻分压电路信号输出
  
  //通过串口监视器实时输出各个传感器检测的数据结果
  //可用于调试小夜灯工作参数使用
  Serial.println("");
  Serial.println("======Checking Sensor.=====");
  Serial.println("===========================");
  Serial.print("irReading = "); Serial.println(irReading);
  Serial.print("lightReading = "); Serial.println(lightReading);
  Serial.println("===========================");
  Serial.println("");
  
}
 
void loop() {  
  unsigned long currentMillis = millis();   //millis函数可用来获取Arduino开机后运行的时间
  
  irCheck(currentMillis);     //检查红外传感器时间（currentMillis为当前Arduino开机运行的时间）
  lightCheck(currentMillis);  //检查光敏电阻时间（currentMillis为当前Arduino开机运行的时间）
  
  if (irReading ==HIGH ) {
    irCheckInterval = 30000;                 //感应到人时人体传感器检查时间间隔较长
  } else {
    irCheckInterval = 500;                   //未感应到人时人体传感器检查时间间隔较短
  }
  
  /*
   * 以下语句将循环改变RGB-LED的颜色。
   * 函数ledShowColor中共定义了约1536种颜色，
   * 而每种颜色都被编上一个独立的序号，
   * 且相邻两个编号色彩之间的色差极小。
   * 于是RGB-LED在循环显示这些色彩时，
   * 小夜灯将会产生逐渐颜色变化的效果。
  */   
if(irReading ==HIGH && lightReading>=880){   //如感应到人且亮度达到需照明程度    
    if (onOffState == 0) fadeOn();           //点亮小夜灯照明                               
    onOffState = 1; 
    
    //检查颜色序号是否达到上限
    if (colorIndex <= 1535) {  //如颜色序号没达到上限      
      colorIndex++;            //另颜色序号递增                
    } else if ( colorIndex > 1535) {   //如颜色序号达到上限    
      colorIndex = 0;                  //另颜色序号归零 
    }
     
    ledShowColor(colorIndex);
     
  } else {                                   //如未感应到人且亮度未达到需照明程度  
     
    if (onOffState == 1) fadeOff();          //保持小夜灯熄灭                        
    onOffState = 0;
     
  }
}
 
void fadeOn(){
  Serial.println("");
  Serial.println("Fade ON");
  int i;
  while(i < 255){
    i++;
    ledR++;
    ledG++;
    ledB++;
    analogWrite (rLedPin, ledR);
    analogWrite (gLedPin, ledG);
    analogWrite (bLedPin, ledB);
    Serial.println("");
    Serial.print("ledR = ");Serial.println(ledR);
    Serial.print("ledG = ");Serial.println(ledG);
    Serial.print("ledB = ");Serial.println(ledB);    
  }
}
 
void fadeOff(){
  Serial.println("");
  Serial.println("Fade OFF");
  while(ledR > 0){
    ledR--;
    analogWrite (rLedPin, ledR);
    Serial.print("ledR = ");Serial.println(ledR);
  }
  while(ledB > 0){
    ledB--;
    analogWrite (bLedPin, ledB);
    Serial.print("ledB = ");Serial.println(ledB);
  }
  while(ledG > 0){
    ledG--;
    analogWrite (gLedPin, ledG);
    Serial.print("ledG = ");Serial.println(ledG);  
  }
  colorIndex = 0;  
}
 
 
void ledShowColor(int ledColorIndex){
 
  /*
   * ledColorIndex参数为颜色序列号。
   * 以下语句根据参数ledColorIndex让RGB-LED显示相应颜色。
   * 从而产生小夜灯色彩渐变的效果。
   * 您可以将程序上传并观察小夜灯在工作时候的串口监视器数据输出
   * 从而找到小夜灯颜色变化的规律。
  */
  
  if (ledColorIndex >= 0 && ledColorIndex <= 255){ 
    ledR = 255 - ledColorIndex;
    analogWrite (rLedPin, ledR);
  } else if(ledColorIndex >= 256 && ledColorIndex <= 511){
    ledR = ledColorIndex -256;
    analogWrite (rLedPin, ledR);
  } else if(ledColorIndex >= 512 && ledColorIndex <= 767){
    ledG = 767 - ledColorIndex;
    analogWrite (gLedPin, ledG);
  } else if(ledColorIndex >= 768 && ledColorIndex <= 1023){
    ledG = ledColorIndex - 768;
    analogWrite (gLedPin, ledG);
  } else if(ledColorIndex >= 1024 && ledColorIndex <= 1279){
    ledB = 1279 - ledColorIndex;
    analogWrite (bLedPin, ledB);
  } else if(ledColorIndex >= 1280 && ledColorIndex <= 1535){
    ledB = ledColorIndex - 1280;
    analogWrite (bLedPin, ledB);
  }
  
  Serial.println("");
  Serial.print("ledR = ");Serial.println(ledR);
  Serial.print("ledG = ");Serial.println(ledG);
  Serial.print("ledB = ");Serial.println(ledB);
  
  delay(colorChangeDelay);
}
 
void irCheck(unsigned long thisIRMillis) { 
  /*
   * 每一次调用irCheck函数，都需要将当前Arduino的开机
   * 时间传递给irCheck函数的thisIRMillis参数。
   * 而irCheck函数中的previousIRMillis变量是上一次传感器检查时间
   * 通过比较变量thisIRMillis和变量previousIRMillis之间的间隔
   * （当前的Arduino开机运行时间和上一次检查传感器时间的间隔）
   * 来确定Arduino是否需要该检查传感器了。而控制这个时间间隔
   * 的变量就是irCheckInterval。
  */
  if ((unsigned long)(thisIRMillis - previousIRMillis >= irCheckInterval)) {  //如果时间间隔到达了                                                                                               
    irReading = digitalRead(irSensorPin);       //读取红外人体感应模块
    
    //通过串口监视器实时输出传感器检测的数据结果
    //可用于调试小夜灯工作参数使用
    Serial.println("");
    Serial.println("=== Checking IR Sensor ====");
    Serial.println("===========================");
    Serial.print("irReading = "); Serial.println(irReading);
    Serial.print("thisIRMillis = "); Serial.println(thisIRMillis);       
    Serial.print("previousIRMillis = "); Serial.println(previousIRMillis);       
    Serial.println("===========================");
    Serial.println("");
    
    // 每一次检查完传感器以后，都要将这一次检查传感器的时间更新给previousIRMillis。
    // 这一个操作是通过将thisIRMillis存储的时间信息赋值给previousIRMillis完成的。
    // 这样下次irCheck函数被调用时，previousIRMillis将保持为最近一次传感器检查时间。
    previousIRMillis = thisIRMillis;          
  } 
}
 
void lightCheck(unsigned long thisLightMillis) { 
   //检查是否到达时间间隔
  if ((unsigned long)(thisLightMillis - previousLightMillis >= lightCheckInterval)) {  //如果时间间隔到达了                                                                                                         
    lightReading = analogRead(lightSensorPin);  //读取光敏电阻分压电路信号输出   
     
    //通过串口监视器实时输出各个传感器检测的数据结果
    //可用于调试小夜灯工作参数使用
    Serial.println("");
    Serial.println("== Checking Light Sensor ==");
    Serial.println("===========================");
    Serial.print("lightReading = "); Serial.println(lightReading);
    Serial.print("thisLightMillis = "); Serial.println(thisLightMillis);       
    Serial.print("previousLightMillis = "); Serial.println(previousLightMillis);       
    Serial.println("===========================");
    Serial.println("");
    
    previousLightMillis = thisLightMillis;  // 记录最新一次的传感器检查时间         
  }
}
