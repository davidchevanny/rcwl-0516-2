# rcwl-0516-2
微波雷達感應開關模組 人體感應感應探測器
#include <VirtualWire.h>
#define PIN_RADAR 2
#define PIN_TX 9
#define PIN_LED 13
 
void setup() {
  Serial.begin(9600);
  pinMode(PIN_LED, OUTPUT);
  vw_set_tx_pin(PIN_TX);        // Arduino pin to connect the receiver data pin    
  vw_setup(6000);               // bps connection speed
}
 
int rv = -1;
 
void loop() {
  digitalWrite(PIN_LED, HIGH);
  delay(10000);
  int v = digitalRead(PIN_RADAR);
  if (v != rv) {
    rv = v;
    char msg[20];
    sprintf(msg, "R %lu %d", millis() / 1000, v);
    vw_send((uint8_t *)msg, strlen(msg));
    Serial.println(msg);
    vw_wait_tx();        // Wait to finish sending the message
  }
  else
  digitalWrite(PIN_LED, LOW);
  delay(1000);
}
