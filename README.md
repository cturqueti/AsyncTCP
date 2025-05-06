# AsyncTCP 
[![Build Status](https://travis-ci.org/me-no-dev/AsyncTCP.svg?branch=master)](https://travis-ci.org/me-no-dev/AsyncTCP)  
![](https://github.com/me-no-dev/AsyncTCP/workflows/Async%20TCP%20CI/badge.svg)  
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/2f7e4d1df8b446d192cbfec6dc174d2d)](https://www.codacy.com/manual/me-no-dev/AsyncTCP?utm_source=github.com&utm_medium=referral&utm_content=me-no-dev/AsyncTCP&utm_campaign=Badge_Grade)  
[![Join the chat at https://gitter.im/me-no-dev/ESPAsyncWebServer](https://badges.gitter.im/me-no-dev/ESPAsyncWebServer.svg)](https://gitter.im/me-no-dev/ESPAsyncWebServer?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)  

![PlatformIO](https://img.shields.io/badge/PlatformIO-Compatible-orange?style=plastic&logo=platformio)  
![License](https://img.shields.io/badge/License-LGPL%202.1-blue.svg?style=plastic)  
![Version](https://img.shields.io/badge/Version-1.1.0-green.svg?style=plastic&logo=github) 

## 🚀 Fully Asynchronous TCP Library for ESP32 Arduino
A high-performance, event-driven TCP library that enables seamless multi-connection network environments for ESP32 microcontrollers.

### 🔥 Key Features
* 100% Asynchronous - Non-blocking operations for maximum efficiency

* Multi-connection Support - Handle multiple simultaneous connections with ease

* Event-driven Architecture - Callbacks for all TCP events (connect, receive, disconnect)

* Optimized for ESP32 - Leverages ESP32's dual-core architecture

* Memory Efficient - Low memory footprint with smart buffer management

* Foundation for ESPAsyncWebServer - Powers the popular asynchronous web server

## 📦 Installation
### Via PlatformIO
Add to your platformio.ini:

```ini

lib_deps = 
    https://github.com/me-no-dev/AsyncTCP.git
```
### Via Arduino IDE
1. Download the latest release

2. Sketch → Include Library → Add .ZIP Library

3. Select the downloaded file

## 🛠 Core Components
### AsyncClient

The base TCP client class with full event control:

```cpp
AsyncClient client;

void setup() {
  client.onConnect([](void* arg, AsyncClient* c) {
    Serial.println("Connected!");
  });
  
  client.onData([](void* arg, AsyncClient* c, void* data, size_t len) {
    Serial.printf("Data received: %.*s\n", len, (char*)data);
  });
  
  client.connect("example.com", 80);
}
```
## AsyncServer
```cpp
AsyncServer server(1234);

void setup() {
  server.onClient([](AsyncClient* client) {
    Serial.println("New client connected");
  });
  
  server.begin();
}
```
## 📚 API Reference
### AsyncClient Methods
|Method |Description|
|---|---|
|connect(host, port)	|Connect to a remote host|
|close()	|Gracefully close connection|
|write(data, size)	|Send data asynchronously|
|free()	|Check if connection can be freed|
|space()	|Available space in send buffer|

### AsyncServer Methods

|Method	|Description|
|---|---|
|begin()	|Start listening for connections|
|end()	|Stop the server|
|onClient(callback)	|Set new connection callback|

## 💡 Advanced Usage
### Handling Binary Data
```cpp
client.onData([](void* arg, AsyncClient* c, void* data, size_t len) {
  uint8_t* bytes = (uint8_t*)data;
  // Process binary data...
});
```
### Performance Tuning
```cpp
// Disable Nagle's algorithm for low-latency
client.setNoDelay(true);

// Adjust receive timeout (seconds)
client.setRxTimeout(5);

// Set acknowledgment timeout (ms)
client.setAckTimeout(500);
```

## 🐛 Troubleshooting
### Common Issues
1. Memory Allocation Errors

* Reduce concurrent connections

* Increase CONFIG_LWIP_MAX_ACTIVE_TCP in menuconfig

2. Connection Drops

* Implement proper error callbacks:
```cpp
client.onError([](void* arg, AsyncClient* c, int8_t error) {
  Serial.printf("Connection error: %s\n", c->errorToString(error));
});
```
3. Performance Problems

* Use setNoDelay(true) for frequent small packets

* Adjust buffer sizes if handling large data

## 🤝 Contributing
We welcome contributions! Please follow these steps:

Fork the repository

1. Create your feature branch (git checkout -b feature/amazing-feature)

2. Commit your changes (git commit -m 'Add some amazing feature')

3. Push to the branch (git push origin feature/amazing-feature)

4. Open a Pull Request

## 📜 License
```text
Asynchronous TCP library for Espressif MCUs

Copyright (c) 2016 Hristo Gochkov. All rights reserved.
This file is part of the esp8266 core for Arduino environment.

This library is free software; you can redistribute it and/or
modify it under the terms of the GNU Lesser General Public
License as published by the Free Software Foundation; either
version 2.1 of the License, or (at your option) any later version.

This library is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU
Lesser General Public License for more details.

You should have received a copy of the GNU Lesser General Public
License along with this library; if not, write to the Free Software
Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA 02110-1301 USA
```
## ✉️ Contact
* Original Author: Hristo Gochkov

* Maintainer: Carlos Augusto D'Orazio Turqueti

* Project Link: https://github.com/maxgerhardt/AsyncTCP
