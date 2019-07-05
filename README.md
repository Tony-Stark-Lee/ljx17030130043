# ljx17030130043
开源硬件实战
## 第一天

#### 学习内容

·为什么要学习开源硬件

·如何学习开源硬件

·三个软件

·几个常用网站

[arduino](https://arduino.cc)
[fritzing](http://fritzing.org/home/)
[processing](https://processing.org)

## 第二天

#### 学习内容

用库函数编码摩斯密码

##### Morse.h
```c
#ifndef Morse_h
#define Morse_h

#include "Arduino.h"

class Morse{
public:
    Morse(int pin);
    void dot();
    void dash();
    void w_space();
    void c_space();
private:
    int _pin;
};

#endif
```
##### Morse.cpp
```c
#include"Arduino.h"
#include"Morse.h"

Morse::Morse(int pin){
    pinMode(pin,OUTPUT);
    _pin=pin;
}

void Morse::dot(){
    digitalWrite(_pin,HIGH);
    delay(250);
    digitalWrite(_pin,LOW);
    delay(250);
}

void Morse::dash(){
    digitalWrite(_pin,HIGH);
    delay(1000);
    digitalWrite(_pin,LOW);
    delay(250);
}

void Morse::w_space(){
    digitalWrite(_pin,LOW);
    delay(1000);
}

void Morse::c_space(){
    digitalWrite(_pin,LOW);
    delay(250);
}
```
##### Morse_code.ino
```c
#include <Morse.h>

Morse morse(13);

char MORSE[][4]={
  {'.','_','*','*'},//A
  {'_','.','.','.'},//B
  {'_','.','_','.'},//C
  {'_','.','.','*'},//D
  {'.','*','*','*'},//E
  {'.','.','_','.'},//F
  {'_','_','.','*'},//G
  {'.','.','.','.'},//H
  {'.','.','*','*'},//I
  {'.','_','_','_'},//J
  {'_','.','_','*'},//K
  {'.','_','.','.'},//L
  {'_','_','*','*'},//M
  {'_','.','*','*'},//N
  {'_','_','_','*'},//O
  {'.','_','_','.'},//P
  {'_','_','.','_'},//Q
  {'.','_','.','*'},//R
  {'.','.','.','*'},//S
  {'_','*','*','*'},//T
  {'.','.','_','*'},//U
  {'.','.','.','_'},//V
  {'.','_','_','*'},//W
  {'_','.','.','_'},//X
  {'_','.','_','_'},//Y
  {'_','_','.','.'},//Z
  };

void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
}

void loop() {
  // put your main code here, to run repeatedly:
  String str="";
  String morse_s="";
  int i,t,temp=0;
  int n=0;
  while(Serial.available()>0){
    temp=1;
    str+=char(Serial.read());
    delay(2);
    n++;
    }

  if(temp){
    for(i=0;i<n;i++){
      for(t=0;t<4;t++){
        if(str[i]>=97&&str[i]<=122){
          morse_s+=char(MORSE[int(str[i]-97)][t]);
          }
        }
        morse_s+=' ';
      }
    Serial.println(morse_s);
    for(i=0;morse_s[i]!='\0';i++){
      if(morse_s[i]=='.') morse.dot();
      else if(morse_s[i]=='_') morse.dash();
      else if(morse_s[i]==' ') morse.w_space();
      if(morse_s[i]!=' '&&str[i]!='*') morse.c_space();
      }  
      Serial.println("Sending successed");
      delay(2);
    }
}
```
## 第三天

#### 学习内容

1、小车的前后左右后控制

2、七段显示器的控制

##### Little_car.ino
```c
//f:forward b:back r:right l:left s:stop

char income;

void setup()
{
    pinMode(5,OUTPUT);
    pinMode(6,OUTPUT);
    pinMode(9,OUTPUT);
    pinMode(10,OUTPUT);
    Serial.begin(9600);
}

void loop()
{
    if(Serial.available()>0){
        income=Serial.read();
    }
    switch(income){
        case'f':forward();break;
        case'b':back();break;
        case'r':right();break;
        case'l':left();break;
        case's':stop();break;
        default:break;
    }
}

void forward()
{
    digitalWrite(5,HIGH);
    digitalWrite(9,HIGH);
    digitalWrite(6,LOW);
    digitalWrite(10,LOW);
}

void back(){
    digitalWrite(6,HIGH);
    digitalWrite(10,HIGH);
    digitalWrite(5,LOW);
    digitalWrite(9,LOW);
}

void left(){
    digitalWrite(5,HIGH);
    digitalWrite(10,HIGH);
    digitalWrite(6,LOW);
    digitalWrite(9,LOW);
}

void right(){
    digitalWrite(6,HIGH);
    digitalWrite(9,HIGH);
    digitalWrite(5,LOW);
    digitalWrite(10,LOW);
}

void stop(){
    digitalWrite(5,LOW);
    digitalWrite(9,LOW);
    digitalWrite(6,LOW);
    digitalWrite(10,LOW);
}
```

##### CD4511_1.0_.ino
```c
void setup()
{
  pinMode(2, OUTPUT);
  pinMode(3, OUTPUT);
  pinMode(4, OUTPUT);
  pinMode(5, OUTPUT);
  pinMode(6, OUTPUT);
  Serial.begin(9600);
}

byte income;
void loop()
{
  if(Serial.available()>0){
    income=Serial.read();
    digitalWrite(2, HIGH);
    delay(10);
    if(income&1==1) digitalWrite(3, HIGH);
    else digitalWrite(3, LOW);
    if(income>>1&1==1) digitalWrite(4, HIGH);
    else digitalWrite(4, LOW);
    if(income>>2&1==1) digitalWrite(5, HIGH);
    else digitalWrite(5, LOW);
    if(income>>3&1==1) digitalWrite(6, HIGH);
    else digitalWrite(6, LOW);
  }
  delay(10);
  digitalWrite(2, LOW);
  delay(10);
}
```

## 第四天

#### 学习内容

7月5日作业：四个七段显示器的分别锁存控制

##### CD4511_2.0_.ino
```c
void setup()
{
  pinMode(2, OUTPUT);
  pinMode(3, OUTPUT);
  pinMode(4, OUTPUT);
  pinMode(5, OUTPUT);
  pinMode(8, OUTPUT);
  pinMode(9, OUTPUT);
  pinMode(10, OUTPUT);
  pinMode(11, OUTPUT);
  Serial.begin(9600);
}

byte income;
int number=0;
void loop()
{
  if(Serial.available()>0){
    income=Serial.read();
    digitalWrite(8,HIGH);
    digitalWrite(9,HIGH);
    digitalWrite(10,HIGH);
    digitalWrite(11,HIGH);
    delay(100);
    if(income&1==1) digitalWrite(2, HIGH);
    else digitalWrite(2, LOW);
    if(income>>1&1==1) digitalWrite(3, HIGH);
    else digitalWrite(3, LOW);
    if(income>>2&1==1) digitalWrite(4, HIGH);
    else digitalWrite(4, LOW);
    if(income>>3&1==1) digitalWrite(5, HIGH);
    else digitalWrite(5, LOW);
    delay(100);
    digitalWrite(number+8,LOW);
    delay(1000);
    number++;
    number=number%4;
  }
}
```

Morse_code:7月3日作业，将输入的小写字母用摩斯码输出

Little_car:7月4日课堂练习，实现小车的前后左右后的控制

CD4511_1.0_:7月4日作业，用七段显示器输出数字

CD4511_2.0_:7月5日作业，用四个七段显示器实现分别锁存的控制




