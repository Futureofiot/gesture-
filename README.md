// gesture-
//Hand gesture car using arduino .
//Transmeter code 
int x_axis = 0;
int y_axis = 0;
int forward = 9;
int backward = 10;
int right = 11;
int left = 12;
void setup() {
  pinMode(A0, INPUT);
  pinMode(A3, INPUT);
  pinMode(forward, OUTPUT);
  pinMode(backward, OUTPUT);
  pinMode(right, OUTPUT);
  pinMode(left, OUTPUT);
  Serial.begin(115200);// put your setup code here, to run once:
}

void loop() {
  x_axis = analogRead(A0);
  y_axis = analogRead(A3);
  Serial.print("X= "); Serial.println(x_axis);
  Serial.print("Y= "); Serial.println(y_axis);
  if (y_axis >= 980) {
    Serial.println("forward");
    digitalWrite(forward, HIGH); // put your main code here, to run repeatedly:
  }
  else {
    if (y_axis <= 910) {
      Serial.println("BACK");
      digitalWrite(backward, HIGH);
    }
    else {
      if (x_axis >= 580) {
        Serial.println("RIGHT");
        digitalWrite(right, HIGH);
      }
      else {
        if (x_axis <= 520)
        {
          Serial.println("LEFT");
          digitalWrite(left, HIGH);
        }
        Serial.println(" ");
      }
    }
  }
  delay(2000);
  if (x_axis > 520 && x_axis < 580 && y_axis > 910 && y_axis < 990)
  {
    digitalWrite(forward, LOW);
    digitalWrite(backward, LOW);
    digitalWrite(right, LOW);
    digitalWrite(left, LOW);
  }
}
//Reciever Code 
#include <AFMotor.h>
AF_DCMotor motor_right(3);
AF_DCMotor motor_left(4);
int forward = 0;
int backward = 0;
int right = 0;
int left = 0;

void setup() {
  pinMode(A2,INPUT);
  pinMode(A4,INPUT);
  pinMode(A5,INPUT);// put your setup code here, to run once:
  pinMode(A6,INPUT);
  Serial.begin(9600);
  motor_right.setSpeed(255);
  motor_left.setSpeed(255);
  motor_right.run(RELEASE);
  motor_left.run(RELEASE);
}
void loop() {
  forward = digitalRead(A0);
  backward = digitalRead(A1);
  right = digitalRead(A2);
  left = digitalRead(A3);
  if(forward == HIGH){
    motor_right.run(FORWARD);
    motor_left.run(FORWARD);
    Serial.println("Forward");
    }
  if(backward == HIGH){
    motor_right.run(BACKWARD);
    motor_left.run(BACKWARD);
    Serial.println("Release");
    }
  if(right == HIGH){
    motor_right.run(FORWARD);
    motor_left.run(RELEASE);
    Serial.println("Right");
    }
  if(left == HIGH){
    motor_right.run(RELEASE);
    motor_left.run(FORWARD);
    Serial.println("Left");
    }
  if(left==LOW&&right==LOW&&forward==LOW&&backward==LOW){
    motor_right.run(RELEASE);
    motor_left.run(RELEASE);
    }
    delay(100);        
     
  // put your main code here, to run repeatedly:

}
