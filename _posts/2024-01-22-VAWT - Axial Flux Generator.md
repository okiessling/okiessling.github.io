---
title: HAWT - Axial Flux Generator
date: 2024-01-22
categories: [Projects, Personal]
tags: [mechanical design, pcb, arduino, manufacturing]     # TAG names should always be lowercase
math: true
toc: true
pin: true
image:
  path: /assets/img/HAWT/OmniDirectional_Turbine_Assembly_Mk2.png
  alt: Final Render of Assembly - Jan 2024
---


First personal project since starting work full time in Boston! SLS 3D Printing will allow you to make ANY geometry you would like - a fabricator's dream!


## Summary
- **COST**: ~ $250 CAD
    + *Financing:* Personal :(
- **TIME TO COMPLETE**: ~ 2.5 Months (~ 100 working hours)
    + *Ideation:* &nbsp;&nbsp; 15 hours
    + *Design:* &nbsp;&nbsp; 30 hours
    + *Mechanical Build:* &nbsp;&nbsp; 15 hours
    + *Electrical Build:* &nbsp;&nbsp; 20 hours
    + *Software Build:* &nbsp;&nbsp; 5 hours
    + *Integration Hell:* &nbsp;&nbsp; 5 hours
    + *Testing:* &nbsp;&nbsp; 10 hours  
<!-- &nbsp; is to add some "tab" spaces -->


## Introduction
I have been curious about wind turbine design for a while and started a deep dive on self-pitching horizontal axis wind turbines (HAWT) 1 summer ago that will pitch the blades out of the wind by centripetal force when the wind speeds are too high slowing the rotation as a function of the wind speed. During that time I stumbled across a fantastic book called [*Wind Machines*](https://books.google.com/books?id=MoilwQEACAAJ&printsec=frontcover#v=onepage&q&f=false){:target="_blank"} &nbsp; by Frank R. Elridge that regardless if you are interested in wind machines are not should check out just for the amazing illustrations and graphs. Supposed '*New*' wind turbine designs have started to emerge recently as cheap clickbait or an easy way to VC's purse that promises to be impossibly efficient or generate large amounts of power in low wind. It's great to read through the taxonomy section of the book and see so many of these '*New*' designs have been around since at least 1975 when this book was published!


Realizing I was not going to break physics by harnessing large amounts of energy with the low-speed wind (~3.3 m/s) swirling around my apartment in Boston I decided to focus on learning how to design an axial generator and 3D print some really beautiful parts along the way!


## Build
___




### Mechanical Design


![HAWT Spinning](/assets/img/HAWT/HAWT Spinning Gif.gif){:width="1250" height="auto" .rounded-10}


The fundamental limit in wind turbine design is the 'Betz Limit' which says that only 59.3% of the kinetic energy in the wind can be used to spin the turbine to generate electricity [^footnote1]. To derive this equation it can be shown that the power in a moving fluid in a cylinder with cross-section '*S*' and velocity '*v*' and fluid density '*$\rho$*' is:  $$ Power = {\rho*S*v^3 \over 2} $$. The cubic relationship with wind speed shows us why we won't get much power from the low-speed wind but we can play with the '*S*' term by increasing the area on which the wind can act and generate rotation. To do this we are going to sweep a [*NACA Airfoil (SG6043)*](http://airfoiltools.com/airfoil/details?airfoil=sg6043-il){:target="_blank"} &nbsp; through a half-dome to allow incident wind to pass over that airfoil generating a new lift vector so we have a lift type turbine over a drag type.


To give the generator a fighting chance to overcome cogging/detent torque I worked hard to lower its moment of Inertia & friction as much as possible by:


1. Using a hollow carbon fiber spar as the axle over a steel rod (30x lower Moment of Inertia)
2. Hollowing out any excess material around the magnets (largest radius)
3. Used high-quality rolling bearings (hybrid ceramic) as they don't require grease to protect from oxidation on steel bearings




### Electrical Design
#### Coil Design
In doing some research I stumbled across a really interesting way to make an axial flux generator using a "serpentine" coil. The big advantage is that it's much easier to wind & assemble than traditional coils as you simply make one large continuous loop the correct length and then bend it to the final shape - Easy!


Making motors is an incredible optimization problem but the nice thing is that when the going gets tough we can go back to the fundamentals. When looking at Faraday's Law for magnetic force on a current carrying wire we see that  $$ EMF (V) = v*L*\beta*\sin(\theta) $$ which means I can do the following optimizations:


1. *Velocity (v)*: the velocity of the magnetic field relative to the wire/coils is controlled by the wind - can't control this.
2. *Length (L)*: To have a longer length of wire I need to either have larger rotors (I fixed this at 160mm as a fabrication constraint) or have thinner wires to have more wire wraps. The thinner the wire higher the resistance of the wire meaning it can handle fewer electrons per unit length lowering the current carrying capacity hence it's an optimization problem


> Fun(ish) Fact: This wire length optimization problem is one of the primary root causes of fires in wind turbines. In abnormal wind speeds too much current is generated relative to the wire's current carrying capacity resulting in high temperatures & possible meltdown.[^footnote2]
{: .prompt-info }


3. *$\beta$*: To increase the magnetic field strength you want the smallest possible air gap between the magnets & the coils and/or using stronger magnets. Halfway through the project, I switched from N35 to N52 the strongest grade of neodymium magnets.


4. *$\sin(\theta)$*: Ideally you want the wire/coils orthogonal to the current as $\sin(90) = 1$. The only parts of the wire in the image below are the sections of the coil with black electrical tape as they are ~90&deg; to the magnets. The arcs at the top and bottom don't generate any electricity as they are ~0&deg; to the magnetic field and wasted copper. One nice thing about the serpentine winding is that because the coils are continuous we have half as many 'arc sections' as with traditional winding meaning higher power density due to less wasted copper!




![Final Coil Arrangement](/assets/img/HAWT/20240113_132143.jpg)
_Isn't it just beautiful!_


![Oscilliscope](/assets/img/HAWT/56 mag each phas.png){: .rounded-10 w='1250' h='auto'}
_Oscilliscope Output showing the Voltage produced by each phase. The inner coil (yellow) produces less voltage as it is further from the magnets_


Above you can see the voltage produced by each coil (3 stacked coils) with the center coil furthest from the magnets producing the lowest. Originally I wanted to do 3-phase rectificatons with 3 coils in a WYE configuration but struggled to get a good ~60&deg; offset between the coils. Ultimately I ended up just wiring the coils in series to combine their output voltages to ~24V. I wanted the higher volts as the cheap rectification components will drop some volts & the lower current means a squared less power losses.


#### Coil Design
To wind the coils I build a simple coil winding setup with an encoder to measure the number of turns so I know when to stop winding. The cutouts where my finger is serves a double purpose as they are indexing holes for wrapping the electrical tape marking where to bend the wire for assembly into the rotor body!


{% include embed/youtube.html id='Ct5PSW9nsjM' %}


#### PCB
The AC output from the 3 coils in series is passed through a full bridge rectifier and then smoothed with some capacitors before passing through a 12V voltage regulator. The output is then passed to a small 12V LED but not before passing through the INA219 current sensor. This sensor collects high side voltage, current & ultimately power produced by the wind turbine which sends this data over WiFi with an ESP32 to a Google sheet acting as a database. I am then able to plot this data to see how much power I am generating during short bursts of wind!


![Fritzing Render](/assets/img/HAWT/Fritzing_PCB.png)
_Fritzing render of PCB showing layout_


![Fritzing Render](/assets/img/HAWT/VAWT_DataLogging_OutPut.png)
_Sample Output from a gust of wind!_


## Code - Google Sheets


```cpp
/*
Oliver Kiessling


VAWT Data Logging
*/


#include <Arduino.h>
#include <ESP8266WiFi.h>
#include "time.h"
#include <ESP_Google_Sheet_Client.h>
#include <NTPClient.h>
#include <WiFiUdp.h>
#include <Wire.h>
#include <Adafruit_INA219.h>


//Setup INA219 Chip
Adafruit_INA219 ina219;


// For SD/SD_MMC mounting helper
#include <GS_SDHelper.h>


#define WIFI_SSID "XXXXXXX"
#define WIFI_PASSWORD "XXXXXXX"


// Google Project ID
#define PROJECT_ID "vawt-iot-datalogging"


// Service Account's client email
#define CLIENT_EMAIL "XXXXXXX"


// Service Account's private key
const char PRIVATE_KEY[] PROGMEM = "XXXXXXX"


// The ID of the spreadsheet where you'll publish the data
const char spreadsheetId[] = "XXXXXXX"


// Timer variables
unsigned long lastTime = 0;  
unsigned long timerDelay = 5000;


// Token Callback function
void tokenStatusCallback(TokenInfo info);


// NTP server to request epoch time
const char* ntpServer = "pool.ntp.org";


// Define NTP Client to get time
WiFiUDP ntpUDP;
NTPClient timeClient(ntpUDP, "pool.ntp.org");


// Variable to save current epoch time
unsigned long epochTime;


// Function that gets current epoch time
unsigned long getTime() {
  timeClient.update();
  unsigned long now = timeClient.getEpochTime();
  return now;
}


void setup(){


    Serial.begin(115200);
    Serial.println();
    Serial.println();


    //Configure time
    configTime(0, 0, ntpServer);


    //Confugiruing the INA219
    if (! ina219.begin()) {
      Serial.println("Failed to find INA219 chip");
      while (1) { delay(10); }
     }
    // To use a slightly lower 32V, 1A range (higher precision on amps):
    //ina219.setCalibration_32V_1A();
    // Or to use a lower 16V, 400mA range (higher precision on volts and amps):
    ina219.setCalibration_16V_400mA();




    GSheet.printf("ESP Google Sheet Client v%s\n\n", ESP_GOOGLE_SHEET_CLIENT_VERSION);


    // Connect to Wi-Fi
    WiFi.setAutoReconnect(true);
    WiFi.begin(WIFI_SSID, WIFI_PASSWORD);
 
    Serial.print("Connecting to Wi-Fi");
    while (WiFi.status() != WL_CONNECTED) {
      Serial.print(".");
      delay(1000);
    }
    Serial.println();
    Serial.print("Connected with IP: ");
    Serial.println(WiFi.localIP());
    Serial.println();


    // Set the callback for Google API access token generation status (for debug only)
    GSheet.setTokenCallback(tokenStatusCallback);


    // Set the seconds to refresh the auth token before expire (60 to 3540, default is 300 seconds)
    GSheet.setPrerefreshSeconds(10 * 60);


    // Begin the access token generation for Google API authentication
    GSheet.begin(CLIENT_EMAIL, PROJECT_ID, PRIVATE_KEY);
}


void loop(){
    // Call ready() repeatedly in loop for authentication checking and processing
    bool ready = GSheet.ready();


    if (ready && millis() - lastTime > timerDelay){
        lastTime = millis();


        FirebaseJson response;


        Serial.println("\nAppend spreadsheet values...");
        Serial.println("----------------------------");


        FirebaseJson valueRange;


        float shuntvoltage = 0;
        float busvoltage = 0;
        float current_mA = 0;
        float loadvoltage = 0;
        float power_mW = 0;


        //Read Currents & Voltages
        shuntvoltage = ina219.getShuntVoltage_mV();
        busvoltage = ina219.getBusVoltage_V();
        current_mA = ina219.getCurrent_mA();
        power_mW = ina219.getPower_mW();
        loadvoltage = busvoltage + (shuntvoltage / 1000);
        // Get timestamp
        epochTime = getTime();


        if (busvoltage >= 0.1) {
          valueRange.add("majorDimension", "COLUMNS");
          valueRange.set("values/[0]/[0]", epochTime);
          valueRange.set("values/[1]/[0]", shuntvoltage);
          valueRange.set("values/[2]/[0]", busvoltage);
          valueRange.set("values/[4]/[0]", loadvoltage);
          valueRange.set("values/[5]/[0]", current_mA);
          valueRange.set("values/[6]/[0]", power_mW);
        }


        // For Google Sheet API ref doc, go to https://developers.google.com/sheets/api/reference/rest/v4/spreadsheets.values/append
        // Append values to the spreadsheet
        bool success = GSheet.values.append(&response /* returned response */, spreadsheetId /* spreadsheet Id to append */, "Sheet1!A1" /* range to append */, &valueRange /* data range to append */);
        if (success){
            response.toString(Serial, true);
            valueRange.clear();
        }
        else{
            Serial.println(GSheet.errorReason());
        }
        Serial.println();
        Serial.println(ESP.getFreeHeap());
    }
}


void tokenStatusCallback(TokenInfo info){
    if (info.status == token_status_error){
        GSheet.printf("Token info: type = %s, status = %s\n", GSheet.getTokenType(info).c_str(), GSheet.getTokenStatus(info).c_str());
        GSheet.printf("Token error: %s\n", GSheet.getTokenError(info).c_str());
    }
    else{
        GSheet.printf("Token info: type = %s, status = %s\n", GSheet.getTokenType(info).c_str(), GSheet.getTokenStatus(info).c_str());
    }
}
```




[^footnote1]: https://en.wikipedia.org/wiki/Betz%27s_law
[^footnote2]: https://www.sciencedirect.com/science/article/pii/S240584402306872X
