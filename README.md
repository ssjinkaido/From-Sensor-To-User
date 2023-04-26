# Report From Sensor to User
# I. Introduction
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
- 
Smart home management system requirements and functionalities.

Requirements:
- Multiple sensors that can detect the change in temperature, humidity, and air quality
- Mobile apps that can be used to control heating, cooling, and lighting systems.
- Smart appliances: refrigerators and washing machines

Functionalities:

- Environmentally-responsive heating and cooling systems that adjust themselves automatically
- Control illumination systems automatically based on occupancy and time of day
- Remote monitoring and appliance control

## Explain the importance of effectively collecting and processing data from sensors in the context of the course

There are multiple benefits when we effectively collect and process sensor data. Sensors accumulate vast quantities of data in real-time, which, when properly analyzed, can help people gain insights, identify patterns, and make data-driven decisions. The data collected by sensors can be used to make informed decisions that increase productivity and optimize performance. In certain instances, sensors are able to detect problems/damages before they become critical. Monitoring the data collected by sensors makes it possible to detect problems/damages early, allowing proactive measures to be taken to prevent more severe problems/damages.

# System components and Architecture
In this report, I will design a smart air quality monitoring and control system that uses sensors to collect data, Firebase to store data, a mobile app to read data, analytic algorithms to process the data, and machine learning algorithms to predict air quality in the future. The reason behind this idea is to address the increasing concern over air pollution and its detrimental effects on our health. Poor air quality has been associated with various health issues, including respiratory diseases, cardiovascular disease, dementia, and even cancer. In addition, air pollution substantially affects the environment, contributing to problems such as acid rain, ozone depletion, and global warming. The objective of this system is to simulate a simple air quality monitoring system that provide real-time air quality data, analyze historical data as well as identify trends and patterns. 

## Main components of the system

The main components of the system are: 

- Air quality sensors: PMS5003 to measure dust matter, and MH-Z19B NDIR CO$_{2}$ to measure CO$_{2}$ 
 concentration.
- Microcontrollers: ESP32.
- Firebase for data storage and retrieval.
- Mobile App is written in Flutter for data visualization and control.
- Machine learning/ Deep Learning algorithms for processing and analyzing data.

Below illustrates two diagrams of the systems:
    \includegraphics[width = \textwidth, keepaspectratio]{images/components_diagram.png}
    \caption{Overall components diagram of the system}

    \includegraphics[width = \textwidth, keepaspectratio]{images/overall_sequence_diagram.png}
    \caption{Overall sequence diagram of the system}


# Implementation details
## Explanation of sensor selection and their specific roles in the system
In the air quality monitoring system, we will use two sensors to measure the concentrations of CO2 and particulate matter. Here is an explanation of each sensor and its function.

### MH-Z19B NDIR CO$_{2}$ and PMS5003 Sensor
This MH-Z19B NDIR CO$_{2}$ sensor can measure CO$_{2}$ concentration between 0 to 5000ppm. It has average current < 60mA, the output signal is UART. It can work in temperature between 0 to $50^\circ C$. The dimension is small: $33 mm\times 20 mm\times 9 mm$. It is stable with lifespan of at least 5 years. The price is around 20 to 40 dollars. 

We connect the MH-Z19B sensor to the ESP32 as follows:
- Connect the sensor's V+ or Vin pin to the 5V pin on the ESP32.
- Connect the sensor's GND pin to the GND pin on the ESP32.
- Connect the sensor's TX pin to the RX pin (e.g., D0 or GPIO3 for ESP32) on the microcontroller.
- Connect the sensor's RX pin to the TX pin (e.g., D1 or GPIO1 for ESP32) on the microcontroller.
\end{itemize}


The PMS5003 sensor measures the concentration of particulate matter (PM) in the air. It can detect PM1.0, PM2.5, and PM10 particles. It supplies voltage from 4.5V to 5.5V. The power consumption is below 100mA. It can work in temperature between -100 to $60^\circ C$. The dimension is small: $50 mm\times 38 mm\times 21 mm$. The price is around 23 dollars. 

We connect the PMS5003 sensor to the ESP32 as follows:
- PMS5003 VCC (Pin 1) to ESP32 5V
- PMS5003 GND (Pin 2) to ESP32 GND
- PMS5003 TX (Pin 4) to ESP32 RX (e.g., GPIO16)
- PMS5003 RX (Pin 5) to ESP32 TX (e.g., GPIO17)

\begin{figure}[H]
\centering
    \includegraphics[width = \textwidth, keepaspectratio]{images/sensor_sequence_diagram.png}
    \caption{Sequence diagram that shows how to connect sensors to ESP32}
\end{figure}

## Describe the data processing techniques used to make informed decisions based on the collected data

There are several ways to preprocess the data collected from sensors. Below are the ways that I found most common when dealing with data like this.

- Removing outliers: Occasionally, air quality sensors may generate inaccurate readings. Methods such as the interquartile range (IQR) method and Z-score can be used to identify and eliminate outliers from a dataset.
- Data normalization: Normalize the data to a standard range (e.g., [0, 1] or [-1, 1]) to make it simpler to pass through machine learning models.
- Feature extraction: Extract meaningful features from the data, such as short-term and long-term averages, peak values, or rate of change. These features can provide additional insights into the air quality and help with decision-making.
- Data imputation (Handle missing values): If some sensor readings are missing or unreliable, we can substitute the missing values based on nearby data points or historical trends using data imputation techniques. Data imputation techniques include linear interpolation, mean or median imputation, and more advanced techniques such as k-nearest neighbors or machine learning models. If the number of missing data is small, we can remove all the missing values without worrying much, but if the number of miss data is large, it is crucial to consider one or various data imputation techniques. 
- Sensor fusion: If multiple sensors measure the same air quality parameter, we can combine their values to produce a more accurate and trustworthy estimate. Sensor fusion techniques include weighted averaging, Kalman filtering, and Bayesian fusion.
\end{itemize}

\begin{figure}[H]
\centering
    \includegraphics[width = \textwidth, keepaspectratio]{images/process_sequence_diagram.png}
    \caption{Sequence diagram that shows how to extract and process the data}
\end{figure}

## Discuss communication protocols and technologies related to data transmission between components

My system has multiple communication protocols: Universal Asynchronous Receiver \\/Transmitter (UART), Serial Peripheral Interface (SPI), Wi-Fi, and REST API. I will illustrate how each component connects with the others through the protocols mentioned above. 

UART: This protocol is used for communication between the MH-Z19B NDIR CO2 sensor and the ESP32 microcontroller.

SPI: This protocol is used for communication between the PMS5003 dust sensor and the ESP32 microcontroller.

Wi-Fi: This protocol is used for communication between the ESP32 microcontroller to the cloud platform (Firebase).

REST API: This protocol is used to extract data from the Firebase Realtime Database and convert it to a CSV file format for further processing. 
