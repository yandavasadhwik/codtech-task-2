# codtech-task-2

To develop a home automation prototype to control multiple devices like lights and fans using IoT platforms like Blynk or MQTT, you will need to combine hardware (sensors, actuators, and microcontrollers) with software platforms for remote control. Below is a step-by-step guide to creating such a prototype.

1. Hardware Components Needed:
Microcontroller: ESP8266 or ESP32 (Both support Wi-Fi, and are widely used for IoT projects)
Relay Modules: 2-3 channel relay module to control lights, fans, or other devices (depending on the number of devices you wish to control)
LEDs or Light Bulbs: To simulate lights or devices
Fan: A basic fan (or a small DC motor to simulate a fan if needed)
Sensors (Optional): Temperature or motion sensor to integrate additional features.
Jumper Wires & Breadboard for connections.
Power Supply: For your ESP module and the devices you're controlling.
2. Software Platform Options:
Blynk App: Provides a mobile app to control devices using an easy-to-use interface and a cloud server.
MQTT Protocol: A lightweight messaging protocol to send and receive commands to and from devices. It can be used with platforms like Mosquitto as the broker.
For this guide, we will use Blynk since it simplifies the development process and requires no server setup.

3. Design Overview:
Microcontroller (ESP8266/ESP32): The microcontroller will be connected to Wi-Fi and use the Blynk app for controlling the devices.
Relay Module: The relay modules will be connected to the microcontroller to control AC devices like lights and fans.
Blynk App: Used to control the devices remotely via smartphone.
4. Wiring the System:
ESP8266/ESP32 to Relay:

VCC of relay to 3.3V or 5V (depends on your relay module)
GND of relay to GND on the microcontroller
IN1, IN2, IN3 (for 3 devices) to the GPIO pins of the microcontroller (GPIO0, GPIO1, GPIO2, etc.)
The relay's NO (Normally Open) pins will be connected to the live wire of the devices (lights or fan) and the COM pin to the neutral wire.
Devices (Lights/Fans) to Relay:

The devices (lights, fans) will be connected to the relay's Normally Open (NO) terminal and the Common (COM) terminal, which are responsible for switching the devices on/off.
5. Arduino Code:
This code uses the Blynk library to control multiple devices (lights and fans) over the internet.

Steps:
Install the Blynk library in your Arduino IDE.
Get your Blynk authorization token by creating a new project in the Blynk app (download it from the App Store or Google Play).
Write the Arduino code to control the devices.
cpp
Copy
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>

// Define your Blynk auth token
char auth[] = "Your_Blynk_Auth_Token";  // Replace with your Blynk app token

// Wi-Fi credentials
char ssid[] = "Your_WiFi_SSID";          // Replace with your Wi-Fi SSID
char pass[] = "Your_WiFi_Password";      // Replace with your Wi-Fi password

// Define the pins connected to relays
#define LIGHT_PIN   D1    // GPIO5 for light (can be changed as needed)
#define FAN_PIN     D2    // GPIO4 for fan (can be changed as needed)

// Blynk virtual pins
#define LIGHT_VPIN  V1    // Virtual pin in Blynk app for light
#define FAN_VPIN    V2    // Virtual pin in Blynk app for fan

void setup() {
  // Start Serial Monitor for debugging
  Serial.begin(115200);

  // Connect to Wi-Fi
  Blynk.begin(auth, ssid, pass);

  // Set relay pins as output
  pinMode(LIGHT_PIN, OUTPUT);
  pinMode(FAN_PIN, OUTPUT);

  // Initially turn off devices
  digitalWrite(LIGHT_PIN, LOW);
  digitalWrite(FAN_PIN, LOW);
}

void loop() {
  // Run Blynk
  Blynk.run();
}

// Function to control light (connected to relay)
BLYNK_WRITE(LIGHT_VPIN) {
  int pinValue = param.asInt();  // Get the state of the light (on/off)
  if (pinValue == 1) {
    digitalWrite(LIGHT_PIN, HIGH);  // Turn the light on
  } else {
    digitalWrite(LIGHT_PIN, LOW);   // Turn the light off
  }
}

// Function to control fan (connected to relay)
BLYNK_WRITE(FAN_VPIN) {
  int pinValue = param.asInt();  // Get the state of the fan (on/off)
  if (pinValue == 1) {
    digitalWrite(FAN_PIN, HIGH);   // Turn the fan on
  } else {
    digitalWrite(FAN_PIN, LOW);    // Turn the fan off
  }
}
6. Set Up the Blynk App:
Create a New Project:

Open the Blynk app on your smartphone.
Create a new project, select the board type as ESP8266 (or ESP32 if you're using that), and choose the connection type as Wi-Fi.
After creating the project, Blynk will provide an Auth Token. Copy this token and paste it into the Arduino code (replacing "Your_Blynk_Auth_Token").
Design the App Interface:

Add two Button Widgets:
One for controlling the light (assign it to Virtual Pin V1).
One for controlling the fan (assign it to Virtual Pin V2).
Customize the buttons to have labels like "Light On/Off" and "Fan On/Off".
Run the App:

After setting up the buttons, click the "Play" button to run the project.
Ensure the app is connected to the Wi-Fi and the ESP module.
7. Testing the System:
Once everything is set up and the Arduino code is uploaded, open the Blynk app.
When you press the button in the Blynk app for the light, the light connected to the relay should turn on or off.
Similarly, pressing the fan button should turn the fan (or the simulation) on or off.
8. Optional Enhancements:
Add more devices: You can control more devices by adding more relays and buttons on the Blynk app. Simply modify the Arduino code to handle more virtual pins.
Sensors: You can integrate sensors (e.g., temperature or motion sensors) to automate the control of devices based on environmental conditions.
Voice Control: Integrate voice assistants like Alexa or Google Home for voice-controlled automation.
9. Conclusion:
You’ve now built a simple home automation system that can control devices like lights and fans using a smartphone app. The Blynk platform makes it easy to create the app interface and handle communication with the devices via Wi-Fi. This system can be expanded with more devices, sensors, and features for a full home automation experience!
