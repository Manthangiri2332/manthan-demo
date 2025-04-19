# manthan-demo
This is my first Git hub repository  , this is my project Arduino uno code
#include <SoftwareSerial.h>



// Bluetooth RX on 10, TX on 11

SoftwareSerial bluetooth(10, 11); 



// Motor driver pins

const int IN1 = 4;

const int IN2 = 7;

const int IN3 = 6;

const int IN4 = 5;



String lastCommand = "";

unsigned long lastCommandTime = 0;

const unsigned long commandCooldown = 800; // milliseconds



void setup() {

  bluetooth.begin(9600);

  Serial.begin(9600);

  

  pinMode(IN1, OUTPUT);

  pinMode(IN2, OUTPUT);

  pinMode(IN3, OUTPUT);

  pinMode(IN4, OUTPUT);

  

  Serial.println("Voice + Remote Controlled Car Ready");

}



void loop() {

  if (bluetooth.available()) {

    char c = bluetooth.read();

    

    Serial.print("Received char: ");

    Serial.print(c, HEX);

    Serial.print(" (");

    Serial.print(c);

    Serial.println(")");



    String cmd = String(c);

    cmd.trim();

    cmd.toLowerCase();



    // Act only if this is a new command or cooldown is over

    if (cmd != lastCommand || millis() - lastCommandTime > commandCooldown) {

      handleCommand(cmd);

      lastCommand = cmd;

      lastCommandTime = millis();

    }

  }

}



// Handles full words and single-letter commands

void handleCommand(String cmd) {

  if (cmd == "f" || cmd == "forward") {

    Serial.println("Moving Forward");

    forward();

  } else if (cmd == "b" || cmd == "backward") {

    Serial.println("Moving Backward");

    backward();

  } else if (cmd == "l" || cmd == "left") {

    Serial.println("Turning Left");

    left();

  } else if (cmd == "r" || cmd == "right") {

    Serial.println("Turning Right");

    right();

  } else if (cmd == "s" || cmd == "stop") {

    Serial.println("Stopping");

    stopCar();

  } else {

    Serial.println("Unknown Command: " + cmd);

  }

}



// Motor functions

void forward() {

  digitalWrite(IN1, LOW);

  digitalWrite(IN2, LOW);

  digitalWrite(IN3, HIGH);

  digitalWrite(IN4, HIGH);

}



void backward() {

  digitalWrite(IN1, HIGH);

  digitalWrite(IN2, HIGH);

  digitalWrite(IN3, LOW);

  digitalWrite(IN4, LOW);

}



void left() {

  digitalWrite(IN1, LOW);

  digitalWrite(IN2, HIGH);

  digitalWrite(IN3, LOW);

  digitalWrite(IN4, HIGH);

}



void right() {

  digitalWrite(IN1, HIGH);

  digitalWrite(IN2, LOW);

  digitalWrite(IN3, HIGH);

  digitalWrite(IN4, LOW);

}



void stopCar() {

  digitalWrite(IN1, LOW);

  digitalWrite(IN2, LOW);

  digitalWrite(IN3, LOW);

  digitalWrite(IN4, LOW);

}
