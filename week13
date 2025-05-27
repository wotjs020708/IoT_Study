## 파일 다운 받기
```
https://github.com/sonnonet/inhatc
```
TestLowOneHopSht_sc/ 파일로 들어간다.

---

TestAppC.nc
``` c++
includes Test;
  2 configuration TestAppC
  3 {
  4 }
  5 implementation
  6 {
  7         components TestC, MainC;
  8         components LedsC, new TimerMilliC();
  9
 10         components ActiveMessageC as AMC;
 11         components new AMSenderC(AM_TEST_DATA_MSG) as AMSC;
 12
 13         TestC.Boot -> MainC;
 14         TestC.Leds -> LedsC;
 15         TestC.MilliTimer -> TimerMilliC;
 16
 17         TestC.RadioControl -> AMC;
 18         TestC.RadioSend -> AMSC;
 19
 20         components new SensirionSht11C() as Sht11ChOC;
 21         TestC.Temp -> Sht11ChOC.Temperature;
 22         TestC.Humi -> Sht11ChOC.Humidity;
 23
 24         components new IlluAdcC() as Illu;
 25         TestC.Illu -> Illu;
 26
 27         components BatteryC;
 28         TestC.Battery -> BatteryC;
 29 }
```
Test.h
``` C++ 
  1 #ifndef TEST_H
  2 #define TEST_H
  3 #include "message.h"
  4
  5 enum {
  6         TEST_PERIOD = 10240LU,
  7 };
  8 enum {
  9         DFLT_VAL = 0x11,
 10 };
 11 enum {
 12         TEST_DATA_LENGTH = TOSH_DATA_LENGTH -6,
 13 };
 14 enum {
 15         AM_TEST_DATA_MSG = 0xA4,
 16 };
 17
 18 typedef nx_struct test_data_msg {
 19         nx_am_addr_t srcID;
 20         nx_uint32_t seqNo;
 21         nx_uint16_t type;
 22         nx_uint16_t Temp;
 23         nx_uint16_t Humi;
 24         nx_uint16_t Illu;
 25         nx_uint16_t battery;
 26 } test_data_msg_t;
 27 #endif
```
TestC.nc
``` c++
  1 module TestC
  2 {
  3         uses {
  4                 interface Boot;
  5                 interface Leds;
  6                 interface Timer<TMilli> as MilliTimer;
  7
  8                 interface SplitControl as RadioControl;
  9                 interface AMSend as RadioSend;
 10
 11                 interface Read<uint16_t> as Temp;
 12                 interface Read<uint16_t> as Humi;
 13                 interface Read<uint16_t> as Illu;
 14
 15                 interface Battery;
 16             }
 17 }
 18
 19 implementation
 20 {
 21     message_t testMsgBffr;
 22     test_data_msg_t *testMsg;
 23
 24     uint32_t seqNo;
 25     uint8_t step;
 26
 27     task void startTimer();
 28     event void Boot.booted() {
 29             testMsg = (test_data_msg_t *)call RadioSend.getPayload(
 30                             &testMsgBffr, sizeof(test_data_msg_t));
 31             testMsg->srcID = TOS_NODE_ID;
 32
 33             seqNo = 0;
 34
 35             post startTimer();
 36     }
 37
 38     task void startTimer() {
 39             call MilliTimer.startPeriodic(TEST_PERIOD);
 40     }
 41
 42     task void radioOn();
 43     event void MilliTimer.fired() {
 44             post radioOn();
 45     }
 46
 47     void startDone();
 48     task void radioOn() {
 49             if (call RadioControl.start() != SUCCESS) startDone();
 50     }
 51
 52     event void RadioControl.startDone(error_t error) {
 53             startDone();
 54     }
 55
 56     task void readTask();
 57     void startDone() {
 58             step = 0;
 59             post readTask();
 60             call Leds.led0Toggle();
 61     }
 62
 63     void sendDone();
 64     task void sendTask() {
 65             testMsg->seqNo = seqNo++;
 66             testMsg->type = 2; //THL type 2
 67
 68             if(call RadioSend.send(AM_BROADCAST_ADDR, &testMsgBffr, sizeof(test_data_msg_t)) != SUCCESS) sendDone(); 69             call Leds.led2Toggle();
 70     }
 71     event void RadioSend.sendDone(message_t* msg, error_t error) {
 72             sendDone();
 73     }
 74
 75     task void radioOff();
 76     void sendDone() {
 77             call Leds.led0Off();
 78             call Leds.led1Off();
 79             call Leds.led2Off();
 80             post radioOff();
 81     }
 82
 83     void stopDone();
 84     task void radioOff() {
 85             if(call RadioControl.stop() != SUCCESS) stopDone();
 86     }
 87
 88     event void RadioControl.stopDone(error_t error) {
 89             stopDone();
 90     }
 91
 92     void stopDone() {
 93     }
 94
 95     task void readTask() {
 96             switch(step) {
 97                     case 0:
 98                             call Temp.read(); break;
 99                     case 1:
100                             call Humi.read(); break;
101                     case 2:
102                             call Illu.read(); break;
103                     default:
104                             testMsg->battery = call Battery.getVoltage();
105                             post sendTask();
106                             break;
107             }
108             step += 1;
109     }
110
111     event void Temp.readDone(error_t error, uint16_t val) {
112             testMsg->Temp = error == SUCCESS ? val : 0xFFFA;
113             post readTask();
114     }
115     event void Humi.readDone(error_t error, uint16_t val) {
116             testMsg->Humi = error == SUCCESS ? val : 0xFFFB;
117             post readTask();
118     }
119     event void Illu.readDone(error_t error, uint16_t val) {
120             testMsg->Illu = error == SUCCESS ? val : 0xFFFC;
121             post readTask();
122     }
123 }
```
위 파일 생성 및 저장 후 `make telosb` 로 컴파일

usb 연결후 `make telosb install.(노드 아이디)` 입력 후 연결 후 전원 on
