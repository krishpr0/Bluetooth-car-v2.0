# Bluetooth-car-v2.0

```
// Define motor driver pins
#define IN1 7  // Motor 1 direction pin (Left side forward)
#define IN2 6  // Motor 1 direction pin (Left side backward)
#define IN3 5  // Motor 2 direction pin (Right side forward)
#define IN4 4  // Motor 2 direction pin (Right side backward)
#define ENA 9  // Motor 1 speed (PWM pin for Left side)
#define ENB 10 // Motor 2 speed (PWM pin for Right side)

char command; // Variable to store Bluetooth command

void setup() {
  // Set motor control pins as outputs
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  pinMode(ENA, OUTPUT);
  pinMode(ENB, OUTPUT);

  // Start serial communication for Bluetooth module at 9600 baud rate
  Serial.begin(9600);
}

void loop() {
  // Check if there's a command received via Bluetooth
  if (Serial.available() > 0) {
    command = Serial.read(); // Read the incoming data

    switch (command) {
      case 'F': // Forward
        moveForward();
        break;

      case 'B': // Backward
        moveBackward();
        break;

      case 'L': // Left
        turnLeft();
        break;

      case 'R': // Right
        turnRight();
        break;

      case 'S': // Stop
        stopCar();
        break;

      default:
        stopCar(); // Stop if no valid command is received
        break;
    }
  }
}

void moveForward() {
  // Both sides forward
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
  analogWrite(ENA, 255); // Full speed left side
  analogWrite(ENB, 255); // Full speed right side
}

void moveBackward() {
  // Both sides backward
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
  analogWrite(ENA, 255); // Full speed left side
  analogWrite(ENB, 255); // Full speed right side
}

void turnLeft() {
  // Left motors reverse, right motors forward (rotate on the spot)
  digitalWrite(IN1, LOW);  // Left side backward
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, HIGH); // Right side forward
  digitalWrite(IN4, LOW);
  analogWrite(ENA, 255);   // Full speed left side
  analogWrite(ENB, 255);   // Full speed right side
}

void turnRight() {
  // Left motors forward, right motors reverse (rotate on the spot)
  digitalWrite(IN1, HIGH); // Left side forward
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);  // Right side backward
  digitalWrite(IN4, HIGH);
  analogWrite(ENA, 255);   // Full speed left side
  analogWrite(ENB, 255);   // Full speed right side
}

void stopCar() {
  // Stop all motors
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
  analogWrite(ENA, 0);     // Stop left side
  analogWrite(ENB, 0);     // Stop right side
}
