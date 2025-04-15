## 외부 라이브러리를 활용하여 1초에 한번씩 실행되는 코드
```c++
#include <SimpleTimer.h>
SimpleTimer timerCnt;


unsigned long loopCnt;

void timerCntFunc() {
  Serial.println(loopCnt);
  loopCnt = 0;
}

void setup() {
  Serial.begin(115200);
  Serial.println();
  timerCnt.setInterval(1000, timerCntFunc);
} 

void loop() {
    timerCnt.run();
    loopCnt++;
  }
```
## 포인터를 이용한 값 전달 코드
```c++
int a1 = 2;
int a2 = 3;
int a3;

void setup() {
  Serial.begin(115200);
  Serial.println();
  //아래 a1, a2는 인수(argument)임 
  sum(a1,a2, a3);
  Serial.println(a3);
}

void loop() {

}

void sum(int a, int b, int& c) {
  c = a + b;
}
```
