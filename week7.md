## 외부 라이브러리를 활용하여 1초에 한번씩 실행되는 코드
```C++
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
