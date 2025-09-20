# Scooter Connect – Embedded System Firmware

**Portfolio Project Overview**

This repository documents the **embedded firmware architecture** designed for the Scooter Connect project. The firmware was developed to run on an **ESP32-WROVER-E** and provides **real-time scooter connectivity, ride telemetry acquisition, and BLE communication** with a companion mobile application.

## 🎯 Functional Scope

The embedded system acts as the **core connectivity and sensing unit** of the scooter. Its main functions are:

- **Bluetooth Low Energy (BLE)** communication with the mobile app
- **GPS integration** via AT commands for ride tracking
- **IMU integration** for motion and orientation sensing
- **Real-time coordination** using a FreeRTOS-based task architecture
- **Data delivery** to the app in a structured format for visualization and storage

## 🏗️ High-Level Firmware Architecture

The firmware is designed with a **layered modular structure**, separating hardware abstraction, system orchestration, and application logic:
<img width="1843" height="866" alt="PFE diagrams - Page 1" src="https://github.com/user-attachments/assets/aa75a91a-4723-4393-af21-a5aa119dd44b" />

### **Board Support Package (BSP) Module**
- Provides hardware abstraction for UART, I²C, SPI, GPIO, timers, and power control
- Encapsulates low-level drivers for peripherals such as the GNSS module and IMU sensors
- Ensures portability by isolating hardware-specific code

### **System Manager Module**
- Built on FreeRTOS, manages task creation, queues, and inter-task notifications
- Coordinates data flow between GPS, IMU, and BLE Manager tasks
- Implements resource management (e.g., memory allocation, synchronization)
- Serves as the central orchestrator of runtime behavior

### **Application Logic Module**
- Defines the functional behavior of the scooter connectivity system
- Aggregates sensor data into telemetry frames
- Implements communication protocols for BLE exchange with the mobile app
- Prepares structured data for higher-level application use (e.g., visualization, analytics)

## FreeRTOS Task Model

The application module is structured into **independent tasks**:

#### **BLE Manager Task**
- Manages pairing, connections, and GATT data exchange with the mobile app
- Receives aggregated telemetry frames from the coordinator

#### **GPS Task**
- Interfaces with the GNSS module over UART using AT commands
- Parses **NMEA sentences** and `AT+CGNSSINFO` responses
- Provides structured `GPSData` objects via a FreeRTOS queue

#### **IMU Task**
- Samples accelerometer and gyroscope data
- Delivers `IMUData` objects to the central coordinator

#### **Central Coordinator Task**
- Collects GPS and IMU data from queues
- Aggregates telemetry into unified packets
- Notifies the BLE Manager when data is ready for transmission

### 🧩 Example Data Flow

1. **GPS Task** parses a new NMEA sentence and pushes `GPSData` to a queue
2. **IMU Task** samples orientation data and pushes `IMUData` to a queue
3. **Central Coordinator** aggregates the data into a telemetry frame
4. **Coordinator** signals the BLE Manager via task notification
5. **BLE Manager** transmits the telemetry packet to the mobile app


## 🛠️ Technical Implementation

### Architecture Highlights
The firmware demonstrates advanced embedded systems concepts:

**Modular Task Design**
- Independent, specialized tasks for each subsystem
- Clean separation of concerns between hardware abstraction, system management, and application logic
- Scalable architecture supporting future feature additions

**Efficient Data Flow**
- Zero-copy data structures where possible
- Queue-based inter-task communication
- Priority-based task scheduling for real-time responsiveness

**Robust Error Handling**
- Graceful degradation when sensors are unavailable
- Automatic reconnection for BLE communication
- Comprehensive system health monitoring

## 📊 Data Processing Pipeline

### Sensor Data Acquisition
1. **GPS Module**: Continuous NMEA sentence parsing with AT command validation
2. **IMU Sensor**: High-frequency motion data capture with built-in filtering
3. **System Metrics**: Real-time monitoring of battery, temperature, and connectivity status

### Data Fusion & Aggregation
- Timestamp synchronization across multiple data sources
- Adaptive sampling rates based on motion detection
- Structured telemetry packet formation for efficient transmission

### Wireless Transmission
- Custom BLE GATT profile for scooter telemetry
- Optimized packet structure for minimal overhead
- Connection state management with automatic recovery

## 🔧 Technical Skills Demonstrated

**Embedded Systems Development**
- FreeRTOS task design and inter-task communication
- Hardware abstraction layer (HAL) development
- Real-time system design and optimization
- Memory management and resource optimization

**Communication Protocols**
- Bluetooth Low Energy (BLE) stack implementation
- UART/I²C peripheral interfacing
- NMEA sentence parsing and GPS data processing
- Custom wireless protocol design

**System Architecture**
- Modular firmware architecture design
- Data flow optimization and pipeline design
- Error handling and system resilience
- Performance monitoring and optimization


---
*This repository serves as a portfolio demonstration of embedded firmware architecture and design principles. The actual implementation involved extensive testing, optimization, and integration work with hardware prototypes.*
