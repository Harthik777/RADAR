# Software Description - Technical Version

## 1. System Architecture
### 1.1 Core Framework
- **Runtime Environment**: Real-time embedded system (RTOS)
- **Processing Cycle**: 100Hz main loop frequency
- **Development Stack**:
  - Language: C++ (Arduino) / Python (Raspberry Pi)
  - IDE: VSCode with PlatformIO
  - Version Control: Git with branch protection
  - CI/CD: Arduino CLI for automated testing

### 1.2 Software Modules
```cpp
// Core Module Structure
MainController
├── SensorInterface
│   ├── UltrasonicSensor (20Hz sampling)
│   ├── IMUSensor (100Hz sampling)
│   └── SensorFusion (Kalman filter)
├── NavigationSystem
│   ├── PathPlanning (A* algorithm)
│   ├── ObstacleDetection
│   └── LocalMapping (2D occupancy grid)
└── MotionControl
    ├── PIDController
    ├── MotorDriver
    └── SpeedRegulator
```

### 1.3 Data Processing Pipeline
```plaintext
Raw Sensor Data → Kalman Filter → State Estimation → Path Planning → 
Motion Vector → PID Control → Motor Commands
```

### 1.4 Technical Specifications
```cpp
// System Parameters
#define MAIN_LOOP_FREQ     100   // Hz
#define SENSOR_SAMPLE_RATE 20    // Hz
#define PWM_FREQUENCY      20000 // Hz
#define PID_UPDATE_RATE    50    // Hz

// Control Parameters
struct ControlParams {
    double Kp = 2.0;    // Proportional gain
    double Ki = 0.5;    // Integral gain
    double Kd = 0.1;    // Derivative gain
    double maxOutput = 255.0;  // PWM limit
};
```

### 1.5 State Machine Definition
```cpp
enum RobotState {
    INIT,
    CALIBRATING,
    IDLE,
    SCANNING,
    PLANNING,
    MOVING,
    OBSTACLE_DETECTED,
    ERROR
};
```

### 1.6 Critical Algorithms
- **Path Planning**: A* implementation with dynamic obstacle avoidance
- **Sensor Fusion**: Extended Kalman Filter for position estimation
- **Motion Control**: Cascaded PID control loop with anti-windup
- **Obstacle Detection**: Rolling window filter with threshold detection

### 1.7 Error Handling
```cpp
try {
    // Critical error checks
    if (sensor.timeout()) throw SensorTimeout();
    if (motor.stalled()) throw MotorStall();
    if (battery.low())   throw LowBattery();
} catch (RobotException& e) {
    emergency_stop();
    log_error(e);
}
```

### 1.8 Performance Metrics
- Processing latency: <10ms
- Control loop jitter: <1ms
- Sensor data validity checks
- Motor response time: <50ms

---

# Software Description - Non-Technical Version

## 1. Robot's Brain: How It Works

### 1.1 Main Features
- **Smart Navigation**: Robot can find its way around obstacles
- **Safety First**: Automatic stopping when obstacles are detected
- **Smooth Movement**: Precise control for steady movement
- **Self-Monitoring**: Continuous checking of all systems

### 1.2 Key Capabilities
📱 **Sensing the Environment**
- Detects obstacles up to 2 meters away
- Updates its view 20 times per second
- Uses multiple sensors for accuracy

🧭 **Making Decisions**
- Creates a simple map of its surroundings
- Plans the best path to its destination
- Adapts to changes in real-time

🎮 **Movement Control**
- Smooth acceleration and deceleration
- Precise turning and speed control
- Automatic speed adjustment for safety

### 1.3 Safety Features
- Emergency stop if obstacles are too close
- Battery level monitoring
- Motor protection systems
- Continuous self-diagnostic checks

### 1.4 How It Makes Decisions
```plaintext
See → Think → Plan → Move → Check → Repeat
```

### 1.5 Built-In Safeguards
- Won't run if battery is too low
- Stops if sensors detect any problems
- Keeps safe distance from obstacles
- Can be stopped instantly if needed

### 1.6 Future Upgrades
- Easy to add new features
- Can be reprogrammed for different tasks
- Adaptable to various environments
- Simple to maintain and update

### 1.7 User Interaction
- Simple status indicators
- Clear error messages
- Easy to start and stop
- Basic troubleshooting guide

### 1.8 Maintenance Needs
- Regular software updates
- Basic sensor cleaning
- Battery charging schedule
- Simple diagnostic checks
