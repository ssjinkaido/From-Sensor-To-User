
# Report From Sensor to User
# 1. Introduction
## 1.1 Describe the smart decision-making model's purpose and real-world applications, and explain the system's requirements and functionalities

The smart decision-making model is a framework that integrates data, analytics, and human intuition to make informed decisions. The model seeks to enhance decision-making processes by reducing biases, boosting effectiveness overall, and increasing efficiency. Real-world applications of smart decision-making models include the healthcare, finance, manufacturing, transportation, and environmental management industries. Utilizing advanced technologies such as artificial intelligence (AI), machine learning (ML), and the Internet of Things (IoT) to process large volumes of data and derive actionable insights is common in these applications. The following are examples of real-world applications of smart decision-making models:

- Healthcare: Smart decision-making models are everywhere in the healthcare system. It is used to predict patient outcomes and diagnose people's cancer in the early phase, improving patient care quality and reducing healthcare costs.
- Finance: Smart decision-making models are utilized in the financial sector to forecast stock prices, manage investment portfolios, and evaluate credit risk. By contemplating market trends, economic indicators, and historical data, they help investors and financial institutions make informed decisions.
- Transportation: It uses intelligent decision-making models to optimize traffic flow and improve public transit systems and fleet management. They can analyze traffic patterns, predict congestion, and suggest optimal routes, thereby reducing travel time and fuel consumption.
- Environmental Management: Intelligent decision-making models are used in environmental management to monitor air quality, predict natural disasters, and manage resources. By analyzing environmental data and revealing potential hazards and opportunities, environmentalists and the government can assist policymakers in making informed decisions.

Requirements:

- Multiple traffic sensors that detect vehicles and pedestrians.
- Camera that can capture images of vehicles and roads.
- Cloud storage for storage of images and videos.
- Deep learning algorithms adopted specifically for some problems: license-plate detections and pedestrian counting.
- Traffic-flow optimization using reinforcement learning algorithms.
-Machine learning algorithms to process images and videos.

Functionalities:

- Real-time monitoring of traffic conditions.
- Traffic flow and congestion analysis.
- Cloud storage for storage of images and videos.
- Integration with emergency services that can facilitate quick response to accidents and incidents.

Smart home management system requirements and functionalities.

Requirements:
- Multiple sensors that can detect the change in temperature, humidity, and air quality
- Mobile apps that can be used to control heating, cooling, and lighting systems.
- Smart appliances: refrigerators and washing machines

Functionalities:

- Environmentally-responsive heating and cooling systems that adjust themselves automatically
- Control illumination systems automatically based on occupancy and time of day
- Remote monitoring and appliance control

## 1.2 Explain the importance of effectively collecting and processing data from sensors in the context of the course

There are multiple benefits when we effectively collect and process sensor data. Sensors accumulate vast quantities of data in real-time, which, when properly analyzed, can help people gain insights, identify patterns, and make data-driven decisions. The data collected by sensors can be used to make informed decisions that increase productivity and optimize performance. In certain instances, sensors are able to detect problems/damages before they become critical. Monitoring the data collected by sensors makes it possible to detect problems/damages early, allowing proactive measures to be taken to prevent more severe problems/damages.

# 2. System components and Architecture
In this report, I will design a smart air quality monitoring and control system that uses sensors to collect data, Firebase to store data, a mobile app to read data, analytic algorithms to process the data, and machine learning algorithms to predict air quality in the future. The reason behind this idea is to address the increasing concern over air pollution and its detrimental effects on our health. Poor air quality has been associated with various health issues, including respiratory diseases, cardiovascular disease, dementia, and even cancer. In addition, air pollution substantially affects the environment, contributing to problems such as acid rain, ozone depletion, and global warming. The objective of this system is to simulate a simple air quality monitoring system that provide real-time air quality data, analyze historical data as well as identify trends and patterns. 

## 2.1 Main components of the system

The main components of the system are: 

- Air quality sensors: PMS5003 to measure dust matter, and MH-Z19B NDIR CO2 to measure CO2
 concentration.
- Microcontrollers: ESP32.
- Firebase for data storage and retrieval.
- Mobile App is written in Flutter for data visualization and control.
- Machine learning/ Deep Learning algorithms for processing and analyzing data.

Below illustrates two diagrams of the systems:
<p align="center">
  <img src="https://github.com/ssjinkaido/From-Sensor-To-User/blob/master/images/components_diagram.png">
  <figcaption>Fig.1: Components diagram of the system</figcaption>
</p>

<div align="center">
    ![Components Diagram](https://github.com/ssjinkaido/From-Sensor-To-User/blob/master/images/components_diagram.png)
</div>

<div align="center">
    *Components diagram of the system*
</div>

<div align="center">
    ![Overall sequence diagram](https://github.com/ssjinkaido/From-Sensor-To-User/blob/master/images/overall_sequence_diagram.png)
</div>

<div align="center">
    *Overall sequence diagram of the system*
</div>

# 3. Implementation details
## 3.1 Explanation of sensor selection and their specific roles in the system
In the air quality monitoring system, we will use two sensors to measure the concentrations of CO2 and particulate matter. Here is an explanation of each sensor and its function.

### 3.1.1 MH-Z19B NDIR CO2 and PMS5003 Sensor
This MH-Z19B NDIR CO2 sensor can measure CO2 concentration between 0 to 5000ppm. It has average current < 60mA, the output signal is UART. It can work in temperature between 0 to 50°C The dimension is small: $33 mm x 20 mm x 9 mm$. It is stable with lifespan of at least 5 years. The price is around 20 to 40 dollars. 

We connect the MH-Z19B sensor to the ESP32 as follows:
- Connect the sensor's V+ or Vin pin to the 5V pin on the ESP32.
- Connect the sensor's GND pin to the GND pin on the ESP32.
- Connect the sensor's TX pin to the RX pin (e.g., D0 or GPIO3 for ESP32) on the microcontroller.
- Connect the sensor's RX pin to the TX pin (e.g., D1 or GPIO1 for ESP32) on the microcontroller.


The PMS5003 sensor measures the concentration of particulate matter (PM) in the air. It can detect PM1.0, PM2.5, and PM10 particles. It supplies voltage from 4.5V to 5.5V. The power consumption is below 100mA. It can work in temperature between -100 to 60°C. The dimension is small: $50 mm\times 38 mm\times 21 mm$. The price is around 23 dollars. 

We connect the PMS5003 sensor to the ESP32 as follows:
- PMS5003 VCC (Pin 1) to ESP32 5V
- PMS5003 GND (Pin 2) to ESP32 GND
- PMS5003 TX (Pin 4) to ESP32 RX (e.g., GPIO16)
- PMS5003 RX (Pin 5) to ESP32 TX (e.g., GPIO17)

<div align="center">
    ![Sensor Sequence Diagram](https://github.com/ssjinkaido/From-Sensor-To-User/blob/master/images/sensor_sequence_diagram.png)
</div>

<div align="center">
    *Sequence diagram that shows how to connect sensors to ESP32*
</div>

## 3.2 Describe the data processing techniques used to make informed decisions based on the collected data

There are several ways to preprocess the data collected from sensors. Below are the ways that I found most common when dealing with data like this.

- Removing outliers: Occasionally, air quality sensors may generate inaccurate readings. Methods such as the interquartile range (IQR) method and Z-score can be used to identify and eliminate outliers from a dataset.
- Data normalization: Normalize the data to a standard range (e.g., [0, 1] or [-1, 1]) to make it simpler to pass through machine learning models.
- Feature extraction: Extract meaningful features from the data, such as short-term and long-term averages, peak values, or rate of change. These features can provide additional insights into the air quality and help with decision-making.
- Data imputation (Handle missing values): If some sensor readings are missing or unreliable, we can substitute the missing values based on nearby data points or historical trends using data imputation techniques. Data imputation techniques include linear interpolation, mean or median imputation, and more advanced techniques such as k-nearest neighbors or machine learning models. If the number of missing data is small, we can remove all the missing values without worrying much, but if the number of miss data is large, it is crucial to consider one or various data imputation techniques. 
- Sensor fusion: If multiple sensors measure the same air quality parameter, we can combine their values to produce a more accurate and trustworthy estimate. Sensor fusion techniques include weighted averaging, Kalman filtering, and Bayesian fusion.
\end{itemize}

<div align="center">
    ![Sequence Diagram](https://github.com/ssjinkaido/From-Sensor-To-User/blob/master/images/process_sequence_diagram.png)
</div>

<div align="center">
    <center> *Sequence diagram that shows how to extract and process the data* </center>
</div>

## 3.3 Discuss communication protocols and technologies related to data transmission between components

My system has multiple communication protocols: Universal Asynchronous Receiver \\/Transmitter (UART), Serial Peripheral Interface (SPI), Wi-Fi, and REST API. I will illustrate how each component connects with the others through the protocols mentioned above. 

UART: This protocol is used for communication between the MH-Z19B NDIR CO2 sensor and the ESP32 microcontroller.

SPI: This protocol is used for communication between the PMS5003 dust sensor and the ESP32 microcontroller.

Wi-Fi: This protocol is used for communication between the ESP32 microcontroller to the cloud platform (Firebase).

REST API: This protocol is used to extract data from the Firebase Realtime Database and convert it to a CSV file format for further processing. 

# 4. Testing and Validation
This is the code to read data from 2 sensors:
```
#include <Wire.h>
#include <Adafruit_Sensor.h>
#include <Adafruit_MHZ19B.h>
#include <SoftwareSerial.h>
#include <PMS.h>
#include <WiFi.h>
#include <FirebaseESP32.h>

#define FIREBASE_HOST "airquality-33ba6.firebaseio.com"
#define FIREBASE_AUTH "private_token"

#define WIFI_SSID "Xuan Minh"
#define WIFI_PASSWORD "ngoctung1970"

#define MHZ19B_RX 16
#define MHZ19B_TX 17

SoftwareSerial mhzSerial(MHZ19B_RX, MHZ19B_TX);
Adafruit_MHZ19B mhz;

PMS pms(Serial1);
WiFiClient client;
FirebaseData firebaseData;

void setup() {
  Serial.begin(9600);

  mhzSerial.begin(9600, SERIAL_8N1, MHZ19B_RX, MHZ19B_TX);
  mhz.begin(mhzSerial);

  pms.passiveMode();
  Serial1.begin(9600);

  WiFi.begin(WIFI_SSID, WIFI_PASSWORD);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
  }

  Firebase.begin(FIREBASE_HOST, FIREBASE_AUTH);
}

void loop() {
  if (mhz.isReady()) {
    float co2 = mhz.getCO2();
    Serial.print("CO2 (ppm): ");
    Serial.println(co2);
    
    Firebase.setFloat(firebaseData, "/co2", co2);
    if (firebaseData.dataAvailable()) {
      Serial.println(firebaseData.responseCode());
      Serial.println(firebaseData.payload());
    }
  }

  if (pms.read()) {
    float pm25 = pms.getPM2_5();
    float pm10 = pms.getPM10();
    Serial.print("PM2.5 (ug/m3): ");
    Serial.println(pm25);
    Serial.print("PM10 (ug/m3): ");
    Serial.println(pm10);
    
    Firebase.setFloat(firebaseData, "/pm25", pm25);
    Firebase.setFloat(firebaseData, "/pm10", pm10);
    if (firebaseData.dataAvailable()) {
      Serial.println(firebaseData.responseCode());
      Serial.println(firebaseData.payload());
    }
  }

  delay(1000);
}
```
The code for ML model can be seen here: proposed_model.py
Since time is limited, I do not have time to build a Flutter app to read and view historical data, as well as visualize data in the graph.
# 5. Conclusion
## 5.1 Evaluate the designed system's overall effectiveness in meeting real- world applications' needs

The designed system for measuring air quality with sensors and storing data on Firebase for analysis and visualization via a mobile app is a partial simulation of real-world applications. The system provides real-time air quality monitoring; the sensors can precisely measure dust and CO2 concentration. The collected data is then stored in Firebase for further processing and analysis. The user-friendly interface of the mobile application makes it simple for users to access and observe air quality data from any location. Users can view historical, real-time, and visualizations of air quality data to obtain insights and make educated decisions regarding the environment. Based on the data, environmentalists or the government can make informed decisions and develop effective solutions to address the problems caused by bad air quality. 

## 5.2 Comment on the advantages, limitations, and scalability of the system

Advantages

- It is a low-cost system that anyone could build for education or experiment inside the house.
- The system is scalable because it can be readily expanded to cover larger areas by adding more sensors and components.
- The system can capture data in real-time, with acceptable precision. 

Limitations
- The precision and dependability of the employed sensors limits the system. Inaccurate or malfunctioning sensors can result in inaccurate data and potentially erroneous conclusions.
- The system's dependence on a stable internet connection can hinder locations with poor connectivity.
- The mobile application may not be compatible with all devices, limiting the number of users accessing the data.

Scalability
- Firebase offers a scalable NoSQL database with high throughput and low latency that can manage large volumes of data. When the system grows, Firebase can still manage growing data volumes and traffic without requiring significant infrastructure modifications as the system expands.
- The system is divided into multiple components. It allows the system to incorporate new sensors and components without affecting the rest of the system. For instance, additional sensors for other pollutants, such as volatile organic compounds or ozone, can be easily added to the system.
- The use of REST APIs for data transmission allows for simple integration with other systems or applications. This indicates that the system can communicate and exchange data with other systems or applications, which can enhance the system's functionality and make it more useful in a variety of contexts.

## 5.3 Propose improvements and further development for the system in the future

These are the ways that can be applied to improve the system:
- We can integrate other sensors to measure other pollutants and compounds, such as volatile organic compounds (VOCs) and nitrogen dioxide (NO2). This could lead to an improvement in the system's ability to monitor air.
- We can use sophisticated machine learning/ deep learning algorithms to analyze time series that could enhance the system's predictive abilities.
- The current mobile application supports simple visualization of historical data analysis. The addition of real-time monitoring, interactive graphs, and more advanced analytics to the app's visualization capabilities would provide users with a deeper understanding of air quality trends and potential health hazards.

