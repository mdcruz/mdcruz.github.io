---
author: "Marie Cruz"
title: "STEM in the garden: how to monitor plants with IoT sensors and Grafana Cloud"
date: "2025-04-18"
tags:
- iot
- observabilty
- monitoring
- grafana-blog
- grafana
---

This post was originally published on the [Grafana](https://grafana.com/) blog which you can view [here](https://grafana.com/blog/2025/04/18/stem-in-the-garden-how-to-monitor-plants-with-iot-sensors-and-grafana-cloud/).

As a Senior Developer Advocate here at Grafana Labs, I’m always looking for opportunities to introduce tech concepts to my 8-year-old daughter in a fun and relatable way. She’s been into gardening since last year, so during a recent STEM week, I decided to use gardening as a way to introduce her to the concepts of IoT, monitoring, and observability.

In this blog post, I’ll walk through how my daughter and I recently set up an IoT project to  monitor the moisture levels of our plants using [Arduino](https://www.arduino.cc/), [Prometheus](https://prometheus.io/) and [Grafana Cloud](https://grafana.com/products/cloud/?pg=blog&plcmt=body-txt) — and also recap all the fun we had along the way. 

Green thumb or not, you can read on to set up this project at home. You can also check out our GitHub project, [plant-monitoring](https://github.com/mdcruz/plant-monitoring), to find all the code in this post.

## Understanding IoT through gardening

Since the start of COVID-19, we’ve bought a few indoor plants. Some have flourished, while others failed to survive. It seemed we either watered the plants too much or didn’t water them enough.

To start explaining IoT to my daughter, I asked her to imagine if each plant had a little green-thumb helper (aka, a sensor) in its soil that could detect moisture levels and inform us when it needed water. Sounds cool, right? I explained that IoT is just that — a way to connect objects, including sensors, to the internet so they can monitor, communicate, and, in some cases, even take action automatically to make our lives easier.

<figure>
  <img src="../../images/IoT-diagram.png" alt="A diagram of the IoT setup.">
</figure>

We observed that our soil moisture sensor has two metal parts, similar to a fork. When the soil is wet, water helps electricity flow easily between the metal parts, and when the soil is dry, electricity can’t flow as well. 

The moisture sensor passes this information as electrical signals to a development board like an [Arduino](https://www.arduino.cc/) or [ESP32](https://www.espressif.com/en/products/modules/esp32), which acts as the brain of this project.

Once the sensor is connected to a development board, it can have two possible outputs:

- **Analog Output (AO)**, where output is of a variable number depending on the moisture level. When it’s a high number, the soil is dry. When it’s a low number, the soil is wet.
- **Digital Output (DO)**, where output is HIGH when the soil is wet and LOW when the soil is dry.

## Project setup

Next, it was time to start setting up our IoT project. I used this [avocado monitoring project](https://github.com/ivanahuckova/avocado_monitoring_grafana) from Grafana Labs Senior Software Engineer Ivana Huckova for inspiration (along with lots of Googling). I also watched a GrafanaCON 2021 session, [Easy DIY IoT Projects with Grafana](https://grafana.com/events/grafanacon/2021/diy-iot/), by Ivana and Grafana Labs Principal Software Engineer Ed Welch, to see other cool ways to monitor IoT setups with Grafana.

Then, there were a few bits of hardware I had to purchase for the project:

- [Soil moisture sensor](https://www.amazon.co.uk/AZDelivery-Moisture-Hygrometer-Arduino-including/dp/B07V4KXZ35/ref=sr_1_5?crid=1NOGJ9PHG5Q39&dib=eyJ2IjoiMSJ9.VEKloFU6urkv5BXmo9Spi2kdq71GnTp1LQuROyWwM0B4zVIQxpY1UUHWF5APqybu7tYaQMoBN3Sj2hogf-Brp7P-VPn-31rAXMpumAIon5EUkBIVBzw4FrbY5KMrd22qZk04_eC6SH7XD1dCxQcPfd1cYhBQeEfkiJcWYI3kBMc7-uyWRxCwfoZE_A83jWupv5-LZULjD6kGVeTlRZSZZXPFfCSTNS8cpT-Ik0_jc_iDqlQLMtsXOyUw1xFFUdhR-6sb9JxJ-Dqdcy74Q2dwE3ca9gFv9UdXaVGik74E6YjYIJ7a9Yt3D8OY0mEVn4BJKv7L2eOz-Gjvas-W9x4qjuxicxYulTiZNdhqdGFF8emEjZdWwpquBU0sR4GKCIh3TbYQqz0qBQsS_QDSe26cvpoai6HdJG13fpCG4TmoGh4XU-pyIvLAmkqsWHbY45DN.4WGIEgR3MccWm23imIdCA-1yBHgR6JryFADbRQmSFoQ&dib_tag=se&keywords=soil%2Bmoisture%2Bsensor%2Barduino&qid=1742388917&sprefix=soil%2Bmois%2Caps%2C83&sr=8-5&th=1)
- [ESP32 S3 DevKitC Development Board](https://www.amazon.co.uk/ESP32-DevKitC-WROOM1-Development-Bluetooth/dp/B0CLD4QKT1/ref=sr_1_3?crid=WCB39PXEDR8S&dib=eyJ2IjoiMSJ9.9qzApprBe2BKFYZJ32VwzR7ssnWUufq5f8oHVupe1DA3kuT30fJ2FzrLXmWM2eojCGN3cTwqz7MzzoGkq1NKRZwLc9qJFrfbsLXTlf874qRWXfiG--s_945fXdfdgkzvpRcpRGtr6hqjMZ9z9h8wKyt_9Ct4SWeEeeF3gCo2Y-dLZy4TB3BCZYN1SyME5XPXnyZnxGGkhmrLOlnTNuKso2RToDU9xrQd1acLlW9JeKU.Z19BI5Jjz-rwRnz-Vwhvp703DqG8dSF51iPxPz9L4Xo&dib_tag=se&keywords=esp32+s3&qid=1742388963&sprefix=esp32+s3%2Caps%2C84&sr=8-3) to process the output received from the sensor
- [Dupont cables](https://www.amazon.co.uk/Elegoo-120pcs-Multicolored-Breadboard-arduino-colorful/dp/B01EV70C78/ref=sr_1_5?crid=C3NSXKUSEA9O&dib=eyJ2IjoiMSJ9.nVag69geZdTr8O6ihOeW9kRJoRtwhk2BRB1IamOFqniAzR9Swbpr-nhtnaWjO8fq6fldInD18D2vc6p5CTif0mRlwz2V--9qmah89lTwJzKcGtu_0kRKtf300g2zo7vxZbv0_nqiI5qIruoEAQLFkde891tOXI7vPmBdRhzI9P3bX7u4OU2X4pAow0dOHAMXMXwWirY6GGZYP6_MpdBCyQcRG87SlWusNruYbFT4tDVy9rNE1gGlRPWyH5s4fql3Dq8ctxO0dk26hY3yUdoOTprO2xD-V43zl4QzBRREHXl_Pd5mjDy9xGjB_CkBmg0ryNIaXDKtXdzVuGcs4kmxHfwUxpjysmdQfrr5ZYHmI8JCErnp0aFugNwX7H-5UX_7Q4eMblHWcTJbqe-XkhL2FN1o5P6iblJiWjeVpXBYUa_e2OEYPEgOGZSpQtVEFg3c.7oK9ZxfNVrA2b8rr_jMB8UevXhRJoc34VrZM_y-pknM&dib_tag=se&keywords=jumper+cables+arduino&qid=1742389010&sprefix=jumper+cables+%2Caps%2C94&sr=8-5) to connect the sensor to the board
- [Breadboard](https://www.amazon.co.uk/MMOBIEL-Solderless-Breadboard-Prototype-Circuit/dp/B0CPJRSLDX/ref=sr_1_3_sspa?crid=1QE0FAH2A76QP&dib=eyJ2IjoiMSJ9.8t5Z1z3PieJxhv3FC400CVJyTNy0_2C9uEUpjWnQ2kgR8o7dRPDWCtXnRPTNgUYPOisYlgTpbJmFtd3FQjDmxaa9RfcgQNzBxrDeHxM5ttNmT3ysP1vjGgmTSCCgYBYaueddSLAR9JMiwR67fVrFp8GCzSw-ElZXW7DsBT7aXsh0ndIfEf1mr4tY7KOHD3X_Pp4OHTTdqTrofagJdhjSWsKI3xzWiGE98pIv7eVlU8WLsXC_ZcHp9e3wIWHQpqMa1xSvf5ilqM4VPs-yrhvtz1W_SWf5ACU2XeU-gTR8Dp0.GM7St9_ILLVeSvsVt6p1mvLYQIomfhiqrlaUphpzatI&dib_tag=se&keywords=breadboard&qid=1742835591&sprefix=breadboard%2Caps%2C102&sr=8-3-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1) to plug the components 
- USB-C cable to connect the board to my laptop

For the software, I also had to install [Arduino IDE](https://www.arduino.cc/en/software/), which is open source software you use to write, edit, and upload code to a development board.

## Connecting all the hardware

Now that we had everything we needed, it was time to piece together all the hardware and get to the most exciting part of the project.

Our first step was to connect the soil moisture sensor pieces using the Dupont cables. Afterward, we attached the ESP32 development board to the breadboard. While this is an optional step, we found that, in our case, inserting the development board into the breadboard made it more stable.

Next, we connected the moisture sensor to the development board by connecting the following pins:

- VCC pin to 3.3V pin
- GND pin to GND pin
- AO to pin 11 

<figure>
  <img src="../../images/hardware-wiring.png" alt="A screenshot of the hardware for the soil moisture sensor project.">
  <figcaption>Connected hardware for the soil moisture sensor project</figcaption>
</figure>

We also inserted the moisture sensor directly into the soil of a plant that had recently been watered.

## Creating the initial program

Once the hardware bits were done, I connected the development board to my laptop via a USB-C cable. Next, I followed the instructions on the GitHub repo [IoT with Arduino and Grafana](https://github.com/grafana/diy-iot?tab=readme-ov-file#setting-up-and-using-arduino-ide) to set up the Arduino IDE on my laptop, including adding support for our ESP32 board. (Note: Ivana also explains this process in a [previous blog post](https://grafana.com/blog/2021/03/08/how-i-built-a-monitoring-system-for-my-avocado-plant-with-arduino-and-grafana-cloud/#the-programming).)

For our first iteration, I wanted to keep things as simple as possible so my daughter could easily follow along.

In Arduino IDE, we created a new file called `plant-monitoring.ino` and added the following lines of code:

```
// Define the sensor pin and sensor variable
int sensorPin = 11;
int sensorValue = 0;
void setup() {
  // Initialize serial communication at 9600 baud rate
  Serial.begin(9600);
}
void loop() {
  // Read the analog value from the sensor
  sensorValue = analogRead(sensorPin);
  
  // Check if the soil is dry
  if (sensorValue > 500) {
    Serial.print(sensorValue);
    Serial.println(" - Status: Soil is too dry - time to water!");

  } else {
    Serial.print(sensorValue);
    Serial.println(" - Status: Soil is perfect!");
  }

  // Wait 5s before taking another reading
  delay(5000);
}
```

Essentially, the code snippet above defines our sensor pin. Within the setup function, we initialize the serial communication at a 9600 [baud rate](https://en.wikipedia.org/wiki/Baud). Within the loop function, we continuously read the analog value provided by the sensor. If the sensor value is above 500, the soil is dry. Otherwise, if the value is low, the soil is wet. 

Note: To test our project easily, we waited 5 seconds before taking another moisture reading. You can increase this value so you wait for a longer amount of time to avoid quickly corroding the moisture sensor.

To explain the baud rate to my daughter, I used the following analogy: “Imagine you are talking with your best friend, and you both agree to speak 9600 letters every second. If you speak less or more than 9600 letters, your communication might not be as clear.” 

## Running the program

After uploading the code to the development board and opening up the serial monitor in Arduino IDE, we saw the following messages:

<figure>
  <img src="../../images/status-soil-is-perfect.png" alt="A screenshot of serial monitor logs when the soil is wet. The message says 'Status: Soil is perfect!'">
  <figcaption>Serial monitor logs when the soil is wet</figcaption>
</figure>

Seeing these logs was a good sign after inserting the moisture sensor into our plant’s soil. After taking the sensor out, we saw the following messages:

<figure>
  <img src="../../images/status-soil-is-too-dry.png" alt="A screenshot of serial monitor logs when the soil is too dry. The message says 'Status: Soil is too dry - time to water!'">
  <figcaption>Serial monitor logs when the soil is dry</figcaption>
</figure>

## What’s observability, Mom?

After completing the project setup, the next step was to explain the concepts of monitoring and observability to my daughter — and this, of course, is where Prometheus and Grafana came in. 

I explained that *monitoring* is a way to check the plant’s health. We monitor the moisture level of the soil, and if it’s too dry, we get an alert to water it. We are reacting to the situation, and to the plant’s needs. 

On the other hand, *observability* takes monitoring to another level. Observability is a way to understand why our plant’s health changes over time. It’s about looking at all our data, identifying patterns over time, and predicting what will happen next. For example, does the plant’s soil moisture drop during a specific time of day? Do we need to include other factors, such as room temperature and humidity, into our project to support our predictions? Observability considers all these points and more, so we can optimize the living conditions of our plants. 

I then introduced my daughter to Prometheus and Grafana as the de-facto platforms for monitoring and observability.

## Prometheus: The Gardener

To introduce Prometheus, I asked her to imagine an imaginary gardener who collects sensor values and stores all of them in a toolbox. I also told her that, in order to get these values from Prometheus, we must speak its language: PromQL.

`soil_moisture{monitoring_type="gabby_plant"}`

Prometheus will return the soil moisture sensor values with timestamps so we know when the values are collected. Here’s an example:

| Time	               | Sensor value |
| -------------------- | ------------ |
| 2025-03-17 19:30:00  |	180         |
| 2025-03-17 20:00:00	 |  200         |
| 2025-03-17 20:30:00	 |  260         |

## Setting up Grafana Cloud

The easiest way for me to show Prometheus was to set up a [Grafana Cloud](https://grafana.com/products/cloud/?pg=blog&plcmt=body-txt) account using the free tier, which comes with a fully managed Prometheus instance. After signing up, we were redirected to a landing page like the one shown below.

<figure>
  <img src="../../images/grafana-cloud-landing-page.png" alt="A screenshot of the Grafana Cloud landing page.">
  <figcaption>Grafana Cloud landing page</figcaption>
</figure>

## Installing Prometheus libraries in Arduino

To make it easier for our Arduino code to connect to Prometheus, we installed additional libraries (shown below), as instructed in the [IoT with Arduino and Grafana GitHub repo](https://github.com/grafana/diy-iot?tab=readme-ov-file#using-loki-and-prometheus-libraries), which was relatively straightforward.

<figure>
  <img src="../../images/installing-libraries.png" alt="Arduino libraries for Prometheus.">
</figure>

## Setting up Prometheus

The next step was to store the details of our Prometheus instance in a configuration file so that the code we wrote in Arduino could connect to Prometheus. 

To do this, we followed all the steps listed in the [Sending metrics](https://github.com/grafana/diy-iot?tab=readme-ov-file#sending-metrics) section of the IoT with Arduino and Grafana GitHub project. We ended up with a new file called `config.h`, which contained the following (some values omitted for security):

```
// Prometheus details
#define GC_PROM_URL "prometheus-prod-13-prod-us-east-0.grafana.net"
#define GC_PROM_USER "" // username
#define GC_PROM_PASS "" // token
#define GC_PROM_PATH "/api/prom/push"

#define GC_PORT 443

// Wifi details
#define WIFI_SSID     "" // WIFI name
#define WIFI_PASSWORD "" // WIFI password
```

We also created another file called `certificates.h`, which included a certificate we could use to connect to Grafana Cloud securely. The certificate content was copied from the [certificate file](https://github.com/grafana/prometheus-arduino/blob/main/examples/prom_02_grafana_cloud/certificates.h) of the [prometheus-arduino](https://github.com/grafana/prometheus-arduino) GitHub project.

These two files were eventually included in our `plant-monitoring.ino` file (more on that below).

### Updating our initial program

The prometheus-arduino GitHub project also has a code example of how to send the metrics from Arduino to Prometheus. Instead of reinventing the wheel, we copied the code example and updated it slightly to work with our code. I remember my daughter asking if this was cheating, to which I replied, “This is quite normal with programming. 😅” 

The final program can be seen [here](https://github.com/mdcruz/plant-monitoring/blob/main/plant-monitoring.ino).

After uploading the updated program to our development board, we saw the following logs on our serial monitor. 🙌🏻

<figure>
  <img src="../../images/sending-to-prometheus.png" alt="A screenshot of serial monitor logs with Prometheus successfully connected.">
  <figcaption>Serial monitor logs with Prometheus successfully connected</figcaption>
</figure>

## Grafana Cloud: The Visualizer

The final piece of the project was to visualize all our data with Grafana Cloud. While Prometheus was our gardener, I explained that Grafana is our cool visualizer. It takes the sensor values from Prometheus and turns them into beautiful graphs, making those values easier to understand. 

### The Plant Dashboard

We created our dashboard using different visualization types, such as the classic time series, stat, gauge, and text panels. Since Prometheus is the default data source for Grafana Cloud, we just had to select the correct metric and label values when creating the different panels.

We ended up with the following dashboard by configuring value mappings to print different texts based on the values. 🙌🏻

<figure>
  <img src="../../images/plant-is-healthy-dashboard.png" alt="Grafana dashboard when the plant is healthy">
  <figcaption>Grafana dashboard when the plant is healthy</figcaption>
</figure>

<figure>
  <img src="../../images/plant-is-thirsy-dashboard.png" alt="Grafana dashboard when the plant is thirsty">
  <figcaption>Grafana dashboard when the plant is thirsty</figcaption>
</figure>

### Thirsty Plant Alert

Having a dashboard to look at is excellent, but we thought getting an alert when our plant was thirsty would be even better. So, the final piece of our project was to configure [Grafana Alerting](https://grafana.com/docs/grafana/latest/alerting/). 

To set this up, we created a new alert rule with the alert condition “when the query is above 500” to ensure we would only be alerted if the soil moisture value was above 500.

<figure>
  <img src="../../images/define-query-and-alert-condition.png" alt="A screenshot of the alert rule conditions for our soil moisture sensor project.">
  <figcaption>Alert rule conditions for our soil moisture sensor project</figcaption>
</figure>

We also linked the alert to our dashboard and time series panel for an additional visual indicator of whether an alert was firing or not. Note: you can learn more about this process in [this video](https://www.youtube.com/watch?v=ClLp-iSoaSY). 

Finally, we created a new contact point to define where our alerts should be sent when an alert rule is triggered. We chose Telegram and followed the instructions to [configure Telegram for alerting](https://grafana.com/docs/grafana/latest/alerting/configure-notifications/manage-contact-points/integrations/configure-telegram/?pg=blog&plcmt=body-txt). So, every time the sensor values go above 500, we get the following notification on Telegram:

<figure>
  <img src="../../images/alerts-in-telegram.png" alt="A screenshot of the alerting notifications sent to Telegram.">
  <figcaption>Alerting notifications sent to Telegram</figcaption>
</figure>

## Wrapping up

Overall, the soil moisture sensor project was a great starter kit for my daughter to learn more about IoT and Grafana. She was ecstatic to see everything working from start to finish and even presented the project to her classmates, who were all impressed. 

By introducing her to new technical concepts, she also learned more about my work and what I do as a Developer Advocate here at Grafana Labs. 

Happy planting!