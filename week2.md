// C++ code

//
```C++
#define TRIG 12 // TRIG 핀
#define ECHO 11 // ECHO 핀

int led_r = 7; 
int led_g = 10;
 
  void setup()
{
  Serial.begin(9600); 
    
  pinMode(led_g, OUTPUT);
  pinMode(led_r, OUTPUT);
  pinMode(TRIG, OUTPUT); 
  pinMode(ECHO, INPUT); 
}

void loop()
{
  long duration, distance; // 4byte
  
  digitalWrite(TRIG, LOW); 
  
  delayMicroseconds(2); // delay(2) > 0.002초
  digitalWrite(TRIG, HIGH);
  delayMicroseconds(10); 
  digitalWrite(TRIG, LOW);
  
  duration = pulseIn(ECHO, HIGH); 
  distance = duration * 17 / 1000; 
  Serial.println(duration);
  Serial.print("\nDistance");
  Serial.print(distance);
  Serial.println(" CM");
  
  // Serial.println("11111");
  digitalWrite(led_r, HIGH);
  delay(1000); // Wait for 1000 millisecond(s)
  digitalWrite(led_r, LOW);
  delay(1000); 
  digitalWrite(led_g, HIGH);
  delay(1000);
  digitalWrite(led_g, LOW);
  delay(1000);
}
```
