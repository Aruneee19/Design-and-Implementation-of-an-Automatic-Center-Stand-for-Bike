# Design-and-Implementation-of-an-Automatic-Center-Stand-for-Bike
Arunkumar.Jv (811721105303)
const int motorPin1 = 8; // L298N IN1
const int motorPin2 = 9; // L298N IN2
const int enablePin = 10; // L298N ENA (PWM)
const int buttonPin = 2; // Push button pin

bool standState = false; // false = stand down, true = stand up
bool lastButtonState = LOW;
bool currentButtonState;

void setup() {
  pinMode(motorPin1, OUTPUT);
  pinMode(motorPin2, OUTPUT);
  pinMode(enablePin, OUTPUT);
  pinMode(buttonPin, INPUT_PULLUP); // Enable internal pull-up resistor

  Serial.begin(9600);
}

void loop() {
  currentButtonState = digitalRead(buttonPin);

  if (currentButtonState == LOW && lastButtonState == HIGH) {
    // Toggle stand state
    standState = !standState;
    if (standState) {
      Serial.println("Raising the stand");
      raiseStand();
    } else {
      Serial.println("Lowering the stand");
      lowerStand();
    }
    delay(500); // Debounce delay
  }
  lastButtonState = currentButtonState;
}

void raiseStand() {
  // Rotate motor to raise stand
  digitalWrite(motorPin1, HIGH);
  digitalWrite(motorPin2, LOW);
  analogWrite(enablePin, 255); // Full speed
  delay(2000); // Adjust time according to the motor speed and stand mechanism
  stopMotor();
}

void lowerStand() {
  // Rotate motor to lower stand
  digitalWrite(motorPin1, LOW);
  digitalWrite(motorPin2, HIGH);
  analogWrite(enablePin, 255); // Full speed
  delay(2000); // Adjust time according to the motor speed and stand mechanism
  stopMotor();
}

void stopMotor() {
  digitalWrite(motorPin1, LOW);
  digitalWrite(motorPin2, LOW);
  analogWrite(enablePin, 0); // Stop motor
}
