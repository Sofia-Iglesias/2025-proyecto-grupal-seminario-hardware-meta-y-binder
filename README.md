Link a la simulación en tinkercad: https://www.tinkercad.com/things/3MsELerMRgi-glorious-amur-snaget/editel?returnTo=https%3A%2F%2Fwww.tinkercad.com%2Fdashboard%2Fdesigns%2Fcircuits&sharecode=kuoTAd6MCSDga1S2ryg8cu-W5Gakw_o00tNQtPLY55E

Código entero (por si acaso): 
const int boton1 = 8;
const int boton2 = 7;
const int boton3 = 6;
const int boton4 = 5; 

const int led1 = 13;
const int led2 = 12;
const int led3 = 11;
const int led4 = 10;

const int motorPWM = 3;
const int motorDir = 4;

const int temperaturaPin = A0;

int velocidadActual = 0;

bool botonBool1 = HIGH;
bool botonBool2 = HIGH;
bool botonBool3 = HIGH;
bool botonBool4 = HIGH;

void setup() {
  pinMode(boton1, INPUT_PULLUP);
  pinMode(boton2, INPUT_PULLUP);
  pinMode(boton3, INPUT_PULLUP);
  pinMode(boton4, INPUT_PULLUP);

  pinMode(led1, OUTPUT);
  pinMode(led2, OUTPUT);
  pinMode(led3, OUTPUT);
  pinMode(led4, OUTPUT);

  pinMode(motorPWM, OUTPUT);
  pinMode(motorDir, OUTPUT);
  digitalWrite(motorDir, LOW);  

  Serial.begin(9600);
}

void loop() {

  int lectura = analogRead(temperaturaPin);
  float voltaje = lectura * (5.0 / 1023.0);
  float temperatura = (voltaje - 0.5) * 100.0;

  Serial.print("temperatura: ");
  Serial.println(temperatura);

  int velocidadPortemperatura = 0;

  if (temperatura >= 40) {

    if (temperatura >= 100)      velocidadPortemperatura = 255;
    else if (temperatura >= 80)  velocidadPortemperatura = 200;
    else if (temperatura >= 60)  velocidadPortemperatura = 140;
    else                         velocidadPortemperatura = 80;

    velocidadActual = velocidadPortemperatura;

  } 
  else {

    bool b1 = digitalRead(boton1);
    bool b2 = digitalRead(boton2);
    bool b3 = digitalRead(boton3);
    bool b4 = digitalRead(boton4);

    if (botonBool1 == HIGH && b1 == LOW) {
      if (velocidadActual == 80) velocidadActual = 0;
      else velocidadActual = 80;
    }

    if (botonBool2 == HIGH && b2 == LOW) {
      if (velocidadActual == 140) velocidadActual = 0;
      else velocidadActual = 140;
    }

    if (botonBool3 == HIGH && b3 == LOW) {
      if (velocidadActual == 200) velocidadActual = 0;
      else velocidadActual = 200;
    }

    if (botonBool4 == HIGH && b4 == LOW) {
      if (velocidadActual == 255) velocidadActual = 0;
      else velocidadActual = 255;
    }

    botonBool1 = b1;
    botonBool2 = b2;
    botonBool3 = b3;
    botonBool4 = b4;
  }

  analogWrite(motorPWM, velocidadActual);

  digitalWrite(led1, velocidadActual == 80);
  digitalWrite(led2, velocidadActual == 140);
  digitalWrite(led3, velocidadActual == 200);
  digitalWrite(led4, velocidadActual == 255);
}
