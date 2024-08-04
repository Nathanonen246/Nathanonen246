#include <DHT.h>
#include <LiquidCrystal.h>

// Define pins
#define DHTPIN 2
#define DHTTYPE DHT11
#define RELAYPIN 3

// Initialize DHT sensor
DHT dht(DHTPIN, DHTTYPE);

// Initialize LCD (if using)
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

void setup() {
  // Start serial communication
  Serial.begin(9600);
  
  // Initialize DHT sensor
  dht.begin();
  
  // Set relay pin as output
  pinMode(RELAYPIN, OUTPUT);
  
  // Initialize LCD (if using)
  lcd.begin(16, 2);
  lcd.print("Smart Thermostat");
}

void loop() {
  // Read temperature as Celsius
  float temp = dht.readTemperature();
  
  // Check if any reads failed
  if (isnan(temp)) {
    Serial.println("Failed to read from DHT sensor!");
    return;
  }
  
  // Print temperature to serial monitor
  Serial.print("Temperature: ");
  Serial.print(temp);
  Serial.println(" *C");
  
  // Display temperature on LCD (if using)
  lcd.setCursor(0, 1);
  lcd.print("Temp: ");
  lcd.print(temp);
  lcd.print(" *C");
  
  // Control relay based on temperature
  if (temp < 22) { // Set your desired temperature threshold
    digitalWrite(RELAYPIN, HIGH); // Turn on heating
  } else {
    digitalWrite(RELAYPIN, LOW); // Turn off heating
  }
  
  // Wait a few seconds between measurements
  delay(2000);
}
