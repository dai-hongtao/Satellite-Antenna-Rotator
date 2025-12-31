# Auto Satellite Tracking Antenna Rotator
基于 ESP32 的业余卫星天线自动跟踪旋转控制器 / ESP32-based automatic satellite antenna rotator for amateur radio

---

## 中文说明

### 项目简介
本项目是一个用于业余无线电（HAM Radio）卫星通信的自动天线旋转控制系统，基于 ESP32 开发。

通过安卓 LOOK4SAT 应用获取卫星的实时方位角与俯仰角，并通过蓝牙串口发送至 ESP32，再经 TTL-RS485 模块向 485 云台发送 PELCO-D 控制指令，实现天线对卫星的自动跟踪。

### 实现原理 / 路径
1. 安卓手机运行 LOOK4SAT 并选定目标卫星
2. LOOK4SAT 通过蓝牙串口发送卫星方位数据
3. ESP32 接收数据并得到目标角度
4. ESP32 通过 TTL-to-RS485 模块发送 PELCO-D 指令
5. RS485 云台驱动天线完成水平 / 俯仰旋转
6. SSD1306 OLED 显示当前与目标位置

### 硬件
- MCU：ESP32
- 显示屏：SSD1306（128×64，I2C）  
- TTL 转 RS485 模块：MAX485 / MAX3485
- 云台（PTZ）：支持 RS485 且支持 PELCO-D 协议（其它协议亦可兼容，但需自行修改通信指令）
- 电源模块（可选）：MP1584EN（从云台 12V 取电降压至 3.3V 供 ESP32）

### 参数标定（重要）
本项目采用“转速 + 时间”估算角度（非编码器闭环）。不同型号云台转速差异很大，需要手动标定：
- `PAN_SPEED`：水平转速（度/秒）
- `TILT_SPEED`：俯仰转速（度/秒）
- `SENSITIVITY`：自动模式启动旋转的最小角度差（建议 2~5°）
- `LEAD`：每次旋转的提前量（用于更完整覆盖）

### 开源协议与第三方说明
- 本项目 ESP32 固件部分依赖 GxEPD2：
  https://github.com/ZinggJM/GxEPD2  
  GxEPD2 采用 GNU General Public License v3.0（GPL-3.0）许可。
  如分发包含该库的固件二进制文件（如 `.bin`），需遵守 GPL-3.0 的相关条款，包括提供对应源代码及许可证声明。

### 免责声明
本项目仅用于个人技术研究、学习与实验用途。作者不对因使用本项目所产生的任何直接或间接后果承担责任。  
请在使用本项目时，自行遵守当地法律法规及无线电管理相关规定。

### 作者
7 区 HAM, 呼号: BA7JJQ  
GitHub: https://github.com/dai-hongtao

---

## Demo:

![image](https://github.com/thedonalddon/Sat-Antenna-Rotator/assets/43942741/d9639b92-4860-4a61-b26a-5ff2877d7408)
![image](https://github.com/thedonalddon/Sat-Antenna-Rotator/assets/43942741/41296b9e-caf3-4814-81f1-4360dfb08afa)

---

## English

### Project Overview
This project is an automatic satellite antenna rotator designed for amateur radio (HAM Radio) satellite communication, based on ESP32.

The Android application LOOK4SAT is used to obtain real-time satellite azimuth and elevation data. The data is transmitted to the ESP32 via Bluetooth Serial, and then forwarded through a TTL-to-RS485 module to an RS485 PTZ mount using PELCO-D control commands, enabling automatic antenna tracking of satellites.

### Implementation Path
1. An Android device runs LOOK4SAT and selects a target satellite  
2. LOOK4SAT sends satellite position data via Bluetooth Serial  
3. ESP32 receives the data and calculates the target angles  
4. ESP32 sends PELCO-D commands via a TTL-to-RS485 module  
5. The RS485 PTZ mount drives the antenna in azimuth and elevation  
6. An SSD1306 OLED displays the current position and target position  

### Hardware
- MCU: ESP32  
- Display: SSD1306 (128×64, I2C)  
- TTL to RS485 module: MAX485 / MAX3485  
- PTZ mount: supports RS485 and PELCO-D protocol  
  (other protocols may also be supported but require manual modification of control commands)  
- Power module (optional): MP1584EN  
  (steps down 12V from the PTZ power supply to 3.3V for the ESP32)

### Parameter Calibration (Important)
This project estimates antenna angles using a “rotation speed + time” method (no encoder feedback).  
Different PTZ models have significantly different rotation speeds, so manual calibration is required:

- `PAN_SPEED`: azimuth rotation speed (degrees per second)  
- `TILT_SPEED`: elevation rotation speed (degrees per second)  
- `SENSITIVITY`: minimum angle difference to trigger movement in automatic mode  
  (recommended 2–5°)  
- `LEAD`: lead angle applied during each movement to improve tracking coverage  

### Open Source License and Third-Party Notice
- The ESP32 firmware in this project depends on **GxEPD2**:  
  https://github.com/ZinggJM/GxEPD2  

  GxEPD2 is licensed under the GNU General Public License v3.0 (GPL-3.0).  
  If you distribute compiled firmware binaries (e.g. `.bin`) that incorporate this library, you must comply with GPL-3.0, including providing the corresponding source code and license notices.

### Disclaimer
This project is intended for personal technical research, learning, and experimental use only.  
The author assumes no responsibility for any direct or indirect consequences resulting from the use of this project.

Users are responsible for complying with local laws, regulations, and radio communication management requirements when using this project.

### Author
Callsign: BA7JJQ  
GitHub: https://github.com/dai-hongtao
