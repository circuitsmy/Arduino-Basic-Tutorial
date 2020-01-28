# Arduino-Basic-Tutorial

**Hardware Required**

- [ ] Arduino Uno/Mega/Nano
[https://circuits.my/index.php/product/arduino-uno-r3/](url)

- [ ] DHT11
[https://circuits.my/index.php/product/humidity-sensor/](url)

- [ ] Jumper
[https://circuits.my/index.php/product/male-to-male-cable-40pcs-20cm/](url)
[https://circuits.my/index.php/product/male-to-female-cable-40pcs-20cm/](url)

**Optional Hardware**

- [ ] Breadboard
[https://circuits.my/index.php/product/breadboard-medium/](url)


**Hardware Connection**

- [ ] DHT11 [-ve]  ---->  Arduino [GND] 
- [ ] DHT11 [+ve]  ---->  Arduino [5V] 
- [ ] DHT11 [S]  ---->  Arduino [8] 


![Untitled-1](https://user-images.githubusercontent.com/60383798/73266019-f6075c00-41cd-11ea-9de9-16ba9b1fa0a4.jpg)

**Install Required Library**

- [ ] <Adafruit_Sensor.h>
[https://github.com/adafruit/Adafruit_Sensor](url)

- [ ] <DHT.h>
- [ ] <DHT_U.h>
[https://github.com/adafruit/DHT-sensor-library](url)


**Code Explain**

```
#include <Adafruit_Sensor.h>
#include <DHT.h>
#include <DHT_U.h>
```

#include is used to include outside libraries in your sketch.
[https://www.arduino.cc/reference/en/language/structure/further-syntax/include/](url)

_You can download and explore these library from this link:_

[https://github.com/adafruit/Adafruit_Sensor](url)
[https://github.com/adafruit/DHT-sensor-library](url)

This outside library is created by Adafruit for most of Adafruit sensor and DHT sensor.

```
#define DHTPIN 8   
#define DHTTYPE    DHT11
DHT_Unified   dht(DHTPIN, DHTTYPE);
```

**#define DHTPIN 8** means that we define which Input Pin from Arduino is connected to the Signal Pin of DHT sensor.
**#define DHTTYPE  DHT11** means that we define which type of DHT sensor that we used. In our case, it is DHT11 instead of DHT21 and DHT22.


**DHT_Unified   dht(DHTPIN, DHTTYPE);** means that refer to DHT_Unified Class from the <DHT_U.h> library, a public function which refer to 
`DHT_Unified(uint8_t pin, uint8_t type, uint8_t count=6, int32_t tempSensorId=-1, int32_t humiditySensorId=-1);` 
is called and define as dht(DHTPIN, DHTTYPE).

DHT_Unified()   ---->    dht() 

```
void setup() {
  Serial.begin(115200);
  dht.begin();
  sensor_t sensor;
  dht.temperature().getSensor(&sensor);
  dht.humidity().getSensor(&sensor);
}
```

void setup() is a basic Arduino IDE sketch structure. This loop will run once at the beginning of the application. 
[https://www.arduino.cc/reference/en/language/structure/sketch/setup/](url)


The `setup()` function is called when a sketch starts. Use it to **initialize variables, pin modes, start using libraries, etc.** The `setup()` function will only run once, after each powerup or reset of the Arduino board.

In this case, we are execute some function which is 
`dht.begin();` means to execute **dht()** function which is sets the **DHTPIN** and **DHTTYPE**. 

For deeper understanding, you can study this code which is from <DHT_U.h> library.
[https://github.com/adafruit/Adafruit_DHT_Unified/blob/master/DHT_U.h](url)


`Serial.begin(115200);` means we sets the data rate in bits per second (baud) for serial data transmission. For communicating with Serial Monitor, make sure to use one of the baud rates listed in the menu at the bottom right corner of its screen.

For deeper understanding, you can study this code
[https://www.arduino.cc/reference/tr/language/functions/communication/serial/begin/](url)


```
dht.temperature().getSensor(&sensor);
dht.humidity().getSensor(&sensor);
```
These function refers to `<DHT_U.cpp>` which set up the sensor based on the value **DHTPIN** and **DHTTYPE** that been defined by us.

If you want to know exactly what happen in the library, you can always refer to the code in the link below.

[https://github.com/adafruit/Adafruit_DHT_Unified/blob/master/DHT_U.cpp](url)



The `loop()` function does precisely what its name suggests, and **loops consecutively**, allowing your program to change and respond. Use it to actively control the Arduino board.

```
  sensors_event_t event;  
  dht.temperature().getEvent(&event); 
  if (isnan(event.temperature)) {
    Serial.println("Error reading temperature!");
  }
  else {
    Serial.print("Temperature: ");
    Serial.print(event.temperature);
    Serial.println(" *C");
  }
```
This code is to get temperature event and print its value.

`dht.temperature().getEvent(&event);`  // get temperature event
`Serial.print(event.temperature); ` //print temperature value


```
    dht.humidity().getEvent(&event);
    
    if (isnan(event.relative_humidity)) {
      Serial.println(F("Error reading humidity!"));
    }
    else {
      Serial.print(F("Humidity: "));
      Serial.print(event.relative_humidity);
      Serial.println(F("%"));
    }
```

This code is to get humidity event and print its value.

`dht.humidity().getEvent(&event);`  // get humidity event
`Serial.print(event.relative_humidity);`   //print humidity value

`delay(1500);`  // Lastly, we have to put delay at least 1.5 sec because DHT11 has 1 Hz sampling rate (once every second).
*at least 1 sec delay must be put before it read DHT11 again.

Thats it. 


Please comment this explanation so I can improve.
