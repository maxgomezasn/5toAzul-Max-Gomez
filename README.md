# 5toAzul-Max-Gomez
<img width="4032" height="3024" alt="IMG_6995" src="https://github.com/user-attachments/assets/dd226441-8232-45f9-9168-e071465bad75" />
tinkercad user: maxgomezasn/maximilianogomezasn@gmail.com
// ===== PINES =====
const int pinPulsador    = 2;
const int pinInterruptor = 3;
const int pinLeds6       = 8;
const int pinLedVerde    = 9;
const int pinLedAzul     = 10;
 
// ===== VARIABLES 6 LEDs =====
bool sistemaActivo = false;
unsigned long inicioFase = 0;
bool faseParpadeo = true;      // true = parpadeando, false = apagado
unsigned long tiempoParpadeo = 0;
bool estadoLed6 = false;
 
const unsigned long DURACION_FASE = 8000;   // 8 segundos cada fase
const unsigned long VELOCIDAD_PARPADEO = 300; // Cambia cada 300 ms
 
// ===== VARIABLES 2 LEDs =====
unsigned long tiempoIntercalado = 0;
const unsigned long VELOCIDAD = 500;
bool toggle = false;
 
void setup() {
  pinMode(pinPulsador, INPUT_PULLUP);
  pinMode(pinInterruptor, INPUT);
  pinMode(pinLeds6, OUTPUT);
  pinMode(pinLedVerde, OUTPUT);
  pinMode(pinLedAzul, OUTPUT);
 
  digitalWrite(pinLeds6, LOW);
  digitalWrite(pinLedVerde, LOW);
  digitalWrite(pinLedAzul, LOW);
}
 
void loop() {
  // ===== PARTE 1: Pulsador arranca el ciclo de parpadeo =====
  int estadoPulsador = digitalRead(pinPulsador);
 
  if (estadoPulsador == LOW && !sistemaActivo) {
    sistemaActivo = true;
    inicioFase = millis();
    faseParpadeo = true;
    tiempoParpadeo = millis();
    estadoLed6 = true;
    digitalWrite(pinLeds6, HIGH);
  }
 
  if (sistemaActivo) {
    unsigned long tiempoActual = millis();
 
    // Cambio de fase cada 8 segundos
    if (tiempoActual - inicioFase >= DURACION_FASE) {
      inicioFase = tiempoActual;
      faseParpadeo = !faseParpadeo;
 
      if (!faseParpadeo) {
        // Empieza fase apagado
        digitalWrite(pinLeds6, LOW);
        estadoLed6 = false;
      } else {
        // Empieza fase parpadeo
        estadoLed6 = true;
        digitalWrite(pinLeds6, HIGH);
      }
    }
 
    // Durante la fase de parpadeo, alternar rápido
    if (faseParpadeo) {
      if (tiempoActual - tiempoParpadeo >= VELOCIDAD_PARPADEO) {
        tiempoParpadeo = tiempoActual;
        estadoLed6 = !estadoLed6;
        digitalWrite(pinLeds6, estadoLed6 ? HIGH : LOW);
      }
    }
  }
 
  // ===== PARTE 2: Interruptor deslizante alterna los 2 LEDs =====
  int estadoInterruptor = digitalRead(pinInterruptor);
 
  if (estadoInterruptor == HIGH) {
    if (millis() - tiempoIntercalado >= VELOCIDAD) {
      tiempoIntercalado = millis();
      toggle = !toggle;
      digitalWrite(pinLedVerde, toggle ? HIGH : LOW);
      digitalWrite(pinLedAzul, toggle ? LOW : HIGH);
    }
  } else {
    digitalWrite(pinLedVerde, LOW);
    digitalWrite(pinLedAzul, LOW);
  }
}
<img width="980" height="1280" alt="image" src="https://github.com/user-attachments/assets/d4a72253-37b8-44a3-b9be-bbbf21a430bf" />
<img width="1042" height="634" alt="image" src="https://github.com/user-attachments/assets/9449d1ce-3ab8-4a44-8eaf-f6dad83e9835" />

<img width="1042" height="634" alt="image" src="https://github.com/user-attachments/assets/2f765278-5f5e-407c-bc09-647f73459a9b" />
