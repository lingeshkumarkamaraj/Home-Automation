# HOME AUTOMATION USING ESP32
###### This is to make the home appliences to be smart and to get remote access of the appliences from anywhere with internet connections. This is made by WOKWI and the cloud BLYNK.
---
## Here is the references...

Circuit Connections in WOKWI :-

<img src=https://github.com/lingeshkumarkamaraj/Home-Automation/blob/main/Home%20automation.png> 

In Cloud BLYNK :-

![Blynk](https://github.com/lingeshkumarkamaraj/Home-Automation/blob/main/Blynk.png)

Working :- 

[<img width="300" height="300" src="https://img.icons8.com/color/96/start.png" alt="video"/>](https://youtu.be/KqhvBD_Ou3o)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; [DOWNLOAD](https://github.com/lingeshkumarkamaraj/Home-Automation/raw/refs/heads/main/home%20automation.mp4) 

---
Code :-
```
#define BLYNK_TEMPLATE_ID "TMPL3yLErj_wC"
#define BLYNK_TEMPLATE_NAME "Home automation"
#define BLYNK_AUTH_TOKEN "Vu0sUgEC6yTPDFcfdhePExxn6M0NeyOK"
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>
char auth[] = BLYNK_AUTH_TOKEN;
char ssid[] = "Wokwi-GUEST";
char pass[] = "";
// Relay output pins
const int relayPins[4] = {5, 17, 16, 4};
// Switch input pins
const int switchPins[4] = {18, 19, 21, 22};
// Current states of relays
bool relayStates[4] = {false, false, false, false};
void updateRelay(int i, bool state) {
  relayStates[i] = state;
  digitalWrite(relayPins[i], state);
  Blynk.virtualWrite(V0 + i, state);  // Sync with app
}
// Blynk virtual pin handlers
BLYNK_WRITE(V0) { updateRelay(0, param.asInt()); }
BLYNK_WRITE(V1) { updateRelay(1, param.asInt()); }
BLYNK_WRITE(V2) { updateRelay(2, param.asInt()); }
BLYNK_WRITE(V3) { updateRelay(3, param.asInt()); }
void setup() {
  Serial.begin(115200);
  Blynk.begin(auth, ssid, pass);
  for (int i = 0; i < 4; i++) {
    pinMode(relayPins[i], OUTPUT);
    pinMode(switchPins[i], INPUT_PULLUP);  // Active LOW
    digitalWrite(relayPins[i], LOW);
  }
}
void loop() {
  Blynk.run();
  // Read hardware switches
  for (int i = 0; i < 4; i++) {
    static bool lastState[4] = {HIGH, HIGH, HIGH, HIGH};
    bool currentState = digitalRead(switchPins[i]);
    if (lastState[i] != currentState && currentState == LOW) {
      relayStates[i] = !relayStates[i];
      updateRelay(i, relayStates[i]);
      delay(300);  // Debounce
    }
    lastState[i] = currentState;
  }
}
```
---
[WOKWI LINK](https://wokwi.com/projects/431763904389245953)
---
