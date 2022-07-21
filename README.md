# Final_Project


# code
##include<Servo.h>
##include <SoftwareSerial.h>
Servo myservo;
SoftwareSerial HM10(13,12); // TX, RX

int num = 0; // 세그먼트 처음 숫자 0으로 설정
int segmentLEDs[] = {2, 3, 4, 5, 6, 7, 8};
byte Numbers[10] = {0xFC,0x60,0xDA,0xF2,0x66,0xB6,0xBE,0xE4,0xFE,0xF6};

//7세그먼트 LED 송출
void displayNumbers(int n)
{
  int pin = 2; // a핀부터 시작
  
  for( int i = 0; i < 8; i++)
  {
    boolean state = bitRead(Numbers[n],7-i);
    digitalWrite(pin+i,state); //a핀부터 +1하면 b,c,d, 이렇게 감
  }
}


                 
void setup() 
{ 

  Serial.begin(9600);
  /* 블루투스 - 아두이노 */
  HM10.begin(9600);
  
  myservo.attach(10); // 서보
  pinMode(9, OUTPUT); // led
  
  
  /* 스위치 핀번호 */
  pinMode(16,INPUT); // 하강
  pinMode(15,INPUT); //상승
  pinMode(14,INPUT); // 이동
 
  /* 7세그먼트 */
  for(int i = 1; i <=9; ++i)
    
      pinMode(i,OUTPUT);
   
}

void loop() {
  
    /* UP */
    if(digitalRead(15) == LOW) // Up 스위치를 누르면
  {
    
    ++num;                    // num 1씩 증가 (num 초기값은 0)
    if (num > 5) 
    {
      num = 1;               // num 값이 5보다 크면 num = 1
    }
  }
  displayNumbers(num);      // 7세그먼트 LED 송출
  
  /* Down */
  if(digitalRead(16)==LOW)  // Down 스위치를 누르면
  { 
    --num;                 // num 1씩 감소
    if(num<1)              
    {
      num=5;              // num 값이 1보다 작으면 num =5
    }
  }
  displayNumbers(num);    // 7세그먼트 LED 송출

  /* Move */
  if (digitalRead(14) == HIGH) {    // Move 스위치를 눌렀을 때
    
    if(num == 1){                   // num 값이 1이면 ( = 1층이면) 
      myservo.write(0);             // servo = 0도
    }
    else if(num == 2){              // num 값이 2이면 (= 2층이면)
      myservo.write(45);            // servo = 45도
    } 
    else if(num==3){                // num 값이 3이면 (= 3층이면)
      myservo.write(90);            // servo = 90도
    }
     else if(num==4){               // num 값이 4이면 (= 4층이면)
       myservo.write(135);          // servo = 135도
     }
     else {                         // num 값이 5이면 ( = 5층이면)
       myservo.write(180);          // servo = 180도
     }
    
    delay(3000);// 3초후
    digitalWrite(9,HIGH); //led켜지고 내릴수있음
    delay(5000); // 5초간 내려
    digitalWrite(9,LOW); //led 꺼지고
  }

  //////////////////블루투스////////////////////

  if(HM10.available()){   // HM10으로 들어오는 데이터 입력
    int a = HM10.read();  // a에 읽은 데이터 저장
    Serial.write(a);      // 시리얼 모니터에 a 전송하여 출력
    
    if(a == 1){           // a 값이 1이면 (= 1층이면)
      myservo.write(0);   // 서보모터는 0도
      displayNumbers(a);  // 7세그먼트 LED 송출
      delay(3000);// 3초후
      digitalWrite(9,HIGH); 
      delay(5000); 
      digitalWrite(9,LOW); 
      
    }
    if(a == 2){
      myservo.write(45);
     displayNumbers(a);
      delay(3000);
      digitalWrite(9,HIGH); 
      delay(5000); 
      digitalWrite(9,LOW); 

      }
    if(a == 3){
      myservo.write(90);
      displayNumbers(a);
      delay(3000);// 3초후
      digitalWrite(9,HIGH); 
      delay(5000); 
      digitalWrite(9,LOW);

      }
    if(a == 4){
      myservo.write(135);
      
      displayNumbers(a);
      delay(3000);
      digitalWrite(9,HIGH); 
      delay(5000); 
      digitalWrite(9,LOW); 
      }
    if(a == 5){
      myservo.write(180);
      
      displayNumbers(a);
      delay(3000);
      digitalWrite(9,HIGH); 
      delay(5000); 
      digitalWrite(9,LOW); 
      }
      
  }
  delay(300);
  }
  
  
  
  
  
  # PPT
  
  
  
  
  # 시연영상
  
  
  
