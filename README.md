# Jerry 1.0 & 2.0 - Arduino Maze-Solving Robot

[![Tech Stack](https://skillicons.dev/icons?i=arduino,cpp)](https://skillicons.dev)

## 📖 Project Goal

This repository contains the source code for **Jerry 1.0** (2023) and **Jerry 2.0** (2024), autonomous maze-solving robots developed by Team Jerry for the annual **"Mobile Robots in the Maze"** competition hosted by Óbuda University. These Arduino Mega 2560-based robots navigate complex mazes using RFID navigation tags, infrared distance sensors, and gyroscope-based orientation tracking with PID control for precise movement.

**Competition Results:**
- 🥉 **Jerry 1.0 (2023)**: Overall 3rd place
- 🥉 **Jerry 2.0 (2024)**: Overall 3rd place + 🔧 Mechanical Design Special Award

The robots autonomously navigate through maze corridors by reading RFID tags for turn commands, detecting walls with IR sensors, and maintaining straight-line movement using MPU6050 gyroscope data. When no RFID tag is present, the robot makes intelligent decisions based on available paths.

**Related Projects:**
- 🌐 **Team Website**: [teamjerry.hu](https://teamjerry.hu) - Full documentation and competition history
- 🤖 **Jerry 3.0 (ESP32)**: [Jerry3_ESP32_MazeSolvingRobot](https://github.com/szczukabendeguz/Jerry3_ESP32_MazeSolvingRobot) - Latest generation with WiFi web interface
- 📄 **Competition Info**: [ArduinoCompetition](https://github.com/szczukabendeguz/ArduinoCompetition) - Website repository with detailed project documentation

## 🛠️ Tech Stack

**Hardware:**
- Arduino Mega 2560 microcontroller
- MPU6050 gyroscope/accelerometer
- MFRC522 RFID reader
- 3× Sharp IR distance sensors (GP2Y0A21YK0F) - *Note: Current code uses IR sensors, but during the actual competitions (2023-2024), ultrasonic sensors (HC-SR04) were used. Both versions are available in the git history.*
- Dual DC motor driver (L298N)
- RGB LED indicator

**Software:**
- PlatformIO build system
- Arduino framework
- MPU6050_light library (v1.1.0)
- MFRC522 library (v1.4.11)
- PID_v1 library (v1.2.1)

## 🚀 Getting Started

### Prerequisites

- [PlatformIO](https://platformio.org/) installed (via VS Code extension or CLI)
- USB cable for Arduino Mega 2560
- Arduino Mega 2560 board with assembled hardware components

### Running Locally

1. **Clone the repository:**
   ```bash
   git clone https://github.com/szczukabendeguz/ArduinoMazeSolvingRobot.git
   cd ArduinoMazeSolvingRobot
   ```

2. **Open in PlatformIO:**
   - Open the project folder in VS Code with PlatformIO extension installed
   - PlatformIO will automatically detect the `platformio.ini` configuration

3. **Build the project:**
   ```bash
   pio run
   ```

4. **Upload to Arduino Mega 2560:**
   ```bash
   pio run --target upload
   ```

5. **Monitor serial output (optional):**
   ```bash
   pio device monitor
   ```
   Serial monitor runs at 115200 baud rate and displays sensor readings and debug information.

### Hardware Setup

Before uploading code, ensure the following connections:

**IR Sensors:**
- Front sensor → A15
- Right sensor → A0
- Left sensor → A5

**Motor Driver (L298N):**
- ENA (Left motor PWM) → Pin 5
- IN1 (Left motor direction) → Pin 3
- IN2 (Left motor direction) → Pin 4
- ENB (Right motor PWM) → Pin 6
- IN3 (Right motor direction) → Pin 10
- IN4 (Right motor direction) → Pin 7

**RFID Reader (MFRC522):**
- RST → Pin 8
- SS → Pin 9
- MOSI, MISO, SCK → SPI pins

**MPU6050 Gyroscope:**
- SDA → Pin 20 (SDA)
- SCL → Pin 21 (SCL)

**RGB LED:**
- Red → A8
- Green → A9
- Blue → A10

## 🏗️ Key Components

### Navigation Algorithm

The robot uses a hybrid navigation approach:

1. **RFID-Based Navigation**: Reads RFID tags embedded in the maze floor to receive turn commands (left, right, straight, stop, dead-end)
2. **Wall-Following with PID**: Maintains centered position between walls using PID control based on IR sensor readings
3. **Gyroscope Stabilization**: Uses MPU6050 to maintain straight-line movement when no walls are present
4. **Autonomous Decision-Making**: When no RFID tag is detected, chooses the direction with more available space

### Core Functions

- `measureDistance(int analogPin)`: Converts IR sensor analog readings to distance in centimeters
- `forwardWithAlignment(int maxSpeed)`: Moves forward while maintaining center position using PID control
- `turnLeft(double desiredAngle)` / `turnRight(double desiredAngle)`: Executes precise turns using gyroscope feedback
- `rfidToDirection()`: Decodes RFID tag data into navigation commands
- `PidDrive(double distanceFromMiddle, int maxSpeed, bool isThereAWall)`: Adjusts motor speeds based on PID output

### PID Control

The robot uses PID control for:
- **Wall centering**: Maintains equal distance from both walls when navigating corridors
- **Single-wall following**: Stays at a fixed distance from one wall when only one is present
- **Gyroscope-based straight movement**: Maintains heading when no walls are detected

Current PID parameters:
- Kp = 15
- Ki = 0.1
- Kd = 5

## 🎯 RFID Command Structure

The robot recognizes the following RFID tag commands:

| Command | Description |
|---------|-------------|
| `DIRECTION_START` | Start signal |
| `DIRECTION_STOP` | Stop and celebrate (spins in place) |
| `DIRECTION_LEFT` | Turn left 90° |
| `DIRECTION_RIGHT` | Turn right 90° |
| `DIRECTION_FRONT` | Continue straight |
| `DIRECTION_DEAD_END` | Dead-end detected, turn around |

Multi-step commands are also supported for complex navigation sequences.

## 🏆 Competition Details

- **Event**: "Mobile Robots in the Maze" (Mobil Robotok a Labirintusban)
- **Organizer**: Óbuda University, Budapest, Hungary
- **Location**: Building G, Tavaszmező u. 17, 1084 Budapest
- **Official Rules**: [kando-szakkoli.uni-obuda.hu/labirintusverseny](https://kando-szakkoli.uni-obuda.hu/labirintusverseny/)

## 👥 Team Members

- Csaba Gyarmati - OE-KVK
- Mátyás Kertész - OE-KVK
- Bendegúz András Szczuka - OE-NIK
- Holczmann Dominik - OE-NIK
- Csóka Ákos - OE-NIK

---

# Jerry 1.0 & 2.0 - Arduino Labirintus-megoldó Robot

[![Tech Stack](https://skillicons.dev/icons?i=arduino,cpp)](https://skillicons.dev)

## 📖 Projekt Célja

Ez a repository a **Jerry 1.0** (2023) és **Jerry 2.0** (2024) autonóm labirintus-megoldó robotok forráskódját tartalmazza, amelyeket a Team Jerry fejlesztett az Óbudai Egyetem által évente megrendezett **"Mobil Robotok a Labirintusban"** versenyre. Ezek az Arduino Mega 2560-alapú robotok RFID navigációs tag-ek, infravörös távolságérzékelők és giroszkóp-alapú orientációkövetés segítségével navigálnak komplex labirintusokban, PID szabályozással a precíz mozgásért.

**Versenyek Eredményei:**
- 🥉 **Jerry 1.0 (2023)**: Összesített 3. helyezés
- 🥉 **Jerry 2.0 (2024)**: Összesített 3. helyezés + 🔧 Mechanikai Tervezés Különdíj

A robotok autonóm módon navigálnak a labirintus folyosóin RFID tag-ek olvasásával fordulási parancsokért, falak észlelésével IR szenzorokkal, és egyenes vonalú mozgás fenntartásával MPU6050 giroszkóp adatok alapján. Amikor nincs RFID tag, a robot intelligens döntéseket hoz az elérhető útvonalak alapján.

**Kapcsolódó Projektek:**
- 🌐 **Csapat Weboldal**: [teamjerry.hu](https://teamjerry.hu) - Teljes dokumentáció és verseny történet
- 🤖 **Jerry 3.0 (ESP32)**: [Jerry3_ESP32_MazeSolvingRobot](https://github.com/szczukabendeguz/Jerry3_ESP32_MazeSolvingRobot) - Legújabb generáció WiFi web interface-szel
- 📄 **Verseny Info**: [ArduinoCompetition](https://github.com/szczukabendeguz/ArduinoCompetition) - Weboldal repository részletes projekt dokumentációval

## 🛠️ Tech Stack

**Hardware:**
- Arduino Mega 2560 mikrokontroller
- MPU6050 giroszkóp/gyorsulásmérő
- MFRC522 RFID olvasó
- 3× Sharp IR távolságérzékelő (GP2Y0A21YK0F) - *Megjegyzés: A jelenlegi kód IR szenzorokat használ, de a tényleges versenyek idején (2023-2024) ultrahangos szenzorokat (HC-SR04) használtunk. Mindkét verzió elérhető a git history-ban.*
- Dual DC motor driver (L298N)
- RGB LED jelző

**Software:**
- PlatformIO build rendszer
- Arduino framework
- MPU6050_light library (v1.1.0)
- MFRC522 library (v1.4.11)
- PID_v1 library (v1.2.1)

## 🚀 Getting Started

### Előfeltételek

- Telepített [PlatformIO](https://platformio.org/) (VS Code extension vagy CLI)
- USB kábel Arduino Mega 2560-hoz
- Arduino Mega 2560 board összeszerelt hardware komponensekkel

### Helyi Futtatás

1. **Repository klónozása:**
   ```bash
   git clone https://github.com/szczukabendeguz/ArduinoMazeSolvingRobot.git
   cd ArduinoMazeSolvingRobot
   ```

2. **Megnyitás PlatformIO-ban:**
   - Nyisd meg a projekt mappát VS Code-ban telepített PlatformIO extension-nel
   - A PlatformIO automatikusan felismeri a `platformio.ini` konfigurációt

3. **Projekt build-elése:**
   ```bash
   pio run
   ```

4. **Feltöltés Arduino Mega 2560-ra:**
   ```bash
   pio run --target upload
   ```

5. **Serial output monitorozása (opcionális):**
   ```bash
   pio device monitor
   ```
   A serial monitor 115200 baud rate-en fut és megjeleníti a szenzor leolvasásokat és debug információkat.

### Hardware Beállítás

Kód feltöltése előtt győződj meg a következő kapcsolatokról:

**IR Szenzorok:**
- Első szenzor → A15
- Jobb szenzor → A0
- Bal szenzor → A5

**Motor Driver (L298N):**
- ENA (Bal motor PWM) → Pin 5
- IN1 (Bal motor irány) → Pin 3
- IN2 (Bal motor irány) → Pin 4
- ENB (Jobb motor PWM) → Pin 6
- IN3 (Jobb motor irány) → Pin 10
- IN4 (Jobb motor irány) → Pin 7

**RFID Olvasó (MFRC522):**
- RST → Pin 8
- SS → Pin 9
- MOSI, MISO, SCK → SPI pinek

**MPU6050 Giroszkóp:**
- SDA → Pin 20 (SDA)
- SCL → Pin 21 (SCL)

**RGB LED:**
- Piros → A8
- Zöld → A9
- Kék → A10

## 🏗️ Főbb Komponensek

### Navigációs Algoritmus

A robot hibrid navigációs megközelítést használ:

1. **RFID-alapú Navigáció**: RFID tag-eket olvas a labirintus padlójába ágyazva, hogy fordulási parancsokat kapjon (balra, jobbra, egyenesen, stop, zsákutca)
2. **Falkövetés PID-del**: Középen tartja magát a falak között PID szabályozással IR szenzor leolvasások alapján
3. **Giroszkóp Stabilizáció**: MPU6050-et használ egyenes vonalú mozgás fenntartására, amikor nincsenek falak
4. **Autonóm Döntéshozatal**: Amikor nincs RFID tag észlelve, azt az irányt választja, ahol több hely van

### Fő Függvények

- `measureDistance(int analogPin)`: IR szenzor analóg leolvasásokat konvertál távolságra centiméterben
- `forwardWithAlignment(int maxSpeed)`: Előre mozog miközben középen tartja a pozíciót PID szabályozással
- `turnLeft(double desiredAngle)` / `turnRight(double desiredAngle)`: Precíz fordulásokat hajt végre giroszkóp visszajelzéssel
- `rfidToDirection()`: RFID tag adatokat dekódol navigációs parancsokká
- `PidDrive(double distanceFromMiddle, int maxSpeed, bool isThereAWall)`: Motor sebességeket állít PID kimenet alapján

### PID Szabályozás

A robot PID szabályozást használ:
- **Fal középre rendezés**: Egyenlő távolságot tart mindkét faltól folyosók navigálásakor
- **Egy fal követés**: Fix távolságot tart egy faltól, amikor csak egy van jelen
- **Giroszkóp-alapú egyenes mozgás**: Irányt tart, amikor nincsenek falak észlelve

Jelenlegi PID paraméterek:
- Kp = 15
- Ki = 0.1
- Kd = 5

## 🎯 RFID Parancs Struktúra

A robot a következő RFID tag parancsokat ismeri fel:

| Parancs | Leírás |
|---------|--------|
| `DIRECTION_START` | Start jel |
| `DIRECTION_STOP` | Megállás és ünneplés (helyben forog) |
| `DIRECTION_LEFT` | Balra fordulás 90° |
| `DIRECTION_RIGHT` | Jobbra fordulás 90° |
| `DIRECTION_FRONT` | Folytatás egyenesen |
| `DIRECTION_DEAD_END` | Zsákutca észlelve, megfordulás |

Többlépéses parancsok is támogatottak komplex navigációs szekvenciákhoz.

## 🏆 Verseny Részletek

- **Esemény**: "Mobil Robotok a Labirintusban"
- **Szervező**: Óbudai Egyetem, Budapest, Magyarország
- **Helyszín**: G épület, Tavaszmező u. 17, 1084 Budapest
- **Hivatalos Szabályok**: [kando-szakkoli.uni-obuda.hu/labirintusverseny](https://kando-szakkoli.uni-obuda.hu/labirintusverseny/)

## 👥 Csapattagok

- Gyarmati Csaba - OE-KVK
- Kertész Mátyás - OE-KVK
- Szczuka Bendegúz András - OE-NIK
- Holczmann Dominik - OE-NIK
- Csóka Ákos - OE-NIK
