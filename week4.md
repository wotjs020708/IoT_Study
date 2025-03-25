# 아두이노 LCD(16x2 I2C) 제어
- 시뮬레이션 코드
```c++
#include <Wire.h>
#include <LiquidCrystal_I2C.h> // I2C LCD 라이브러리


LiquidCrystal_I2C lcd(0x27, 16, 2);

void setup()
{
  lcd.init();
  lcd.backlight();
  lcd.print("LCD init");
  delay(2000);
  lcd.clear();
}

void loop()
{
  lcd.setCursor(0,0);
  lcd.print("Hello, World!");
  
  for (int position = 0; position <16; position++) {
    lcd.scrollDisplayLeft();
    delay(150);
  }
}
```
-----
# 아두이노 1602 LCD 12C 주소 찾기
```c++
#include <LiquidCrystal_I2C.h>
#include <Wire.h>



void setup() {
  Serial.begin(9600); // 시리얼 모니터 시작 (threh: 9600 baud)
  Wire.begin(); // I2C 통신 시작
  Serial.println("I2C Scanner Running..."); // 시작 메세지 출력
}

void loop() {
  Serial.println("Scanning..."); // 검색 시작 메세지 출력 
  for (byte address = 1; address < 127; address++) { // I2C 주소 범위: 0x01 ~ 0x7F (1~127)
    Wire.beginTransmission(address); // 특정 주소로 통신 시작
    if(Wire.endTransmission() == 0) { // 응답이 0이면 I2C 자치가 존재함
      Serial.print("I2C 자ㅇ치 발견: 0x"); // 발견된 장치 주소  출력
      Serial.println(address, HEX); // 16(HEX) 형식으로 출력
      delay(500); // 0.5초 대기 (너무 빠르게 반복되지 않도록)
    }
  }

  Serial.println("Scan Complet! Retying in 5seconds...\n");
  delay(5000);
}
```
-----
# 초음파 센서 데이터 LCD 표시
```c++
#include <Wire.h>
#include <LiquidCrystal_I2C.h> // I2C LCD 라이브러리
#define TRIG 9 // TRIG 핀 설정(초음파 보내는 핀)
#define ECHO 8 // ECHO 핀 설정 (초음파 받는 핀)

LiquidCrystal_I2C lcd(0x27, 16, 2);

void setup()
{
  lcd.init();
  lcd.backlight();
  lcd.print("LCD init");
  delay(2000);
  lcd.clear();

  pinMode(TRIG,OUTPUT);
  pinMode(ECHO, INPUT);
}

void loop()
{
  long duration, distance;



  digitalWrite(TRIG, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG, LOW);
  duration = pulseIn (ECHO, HIGH); //물체에 반사되어돌아온 초음파의 시간을 변수에 저장합니다.
  //34000*초음파가 물체로 부터 반사되어 돌아오는시간 /1000000 / 2(왕복값이아니라 편도값이기때문에 나누기2를 해줍니다.)
  //초음파센서의 거리값이 위 계산값과 동일하게 Cm로 환산되는 계산공식 입니다. 수식이 간단해지도록 적용했습니다.

  distance = duration * 17 / 1000; 
  lcd.setCursor(0,0);
  lcd.print(distance);
  lcd.print("cm");
  delay(1000);
  lcd.clear();
}
```

