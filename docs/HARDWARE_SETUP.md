# Hardware Setup Guide - Raspberry Pi

Complete guide for setting up Health Insights Mirror on Raspberry Pi with IR sensor and camera.

## Components Required

### Main Components
- **Raspberry Pi 4** (8GB RAM recommended, minimum 4GB)
- **microSD Card** (64GB Class 10 recommended)
- **Power Supply** (5V 3A USB-C)
- **Heat Sink** (optional but recommended)

### Sensors
- **MLX90614 IR Temperature Sensor** (~$10)
- **Raspberry Pi Camera Module 3** (~$25) OR USB Webcam
- **I2C Cable/Jumper Wires** (for sensor connection)
- **Breadboard** (optional, for prototyping)

### Display (Optional for Mirror Interface)
- **7-10 inch touch display** (~$50-150)
- **HDMI Cable**
- **Frame** (for mirror housing)

### Additional
- **Ethernet Cable** (for initial setup) or WiFi
- **Monitor + HDMI Cable** (for setup)
- **USB Keyboard + Mouse** (for setup)

## Raspberry Pi Operating System Setup

### Step 1: Download Raspberry Pi Imager

Visit: https://www.raspberrypi.com/software/

Download for your operating system (Windows, macOS, or Linux).

### Step 2: Install Operating System

1. Insert microSD card into your computer
2. Open Raspberry Pi Imager
3. Click "Choose OS" → Select "Raspberry Pi OS (64-bit)"
4. Click "Choose Storage" → Select your microSD card
5. Click "Next" and confirm
6. Wait for installation to complete (5-10 minutes)

### Step 3: Initial Boot

1. Insert microSD card into Raspberry Pi
2. Connect monitor, keyboard, mouse
3. Connect power supply
4. Wait for boot (first boot takes 2-3 minutes)

### Step 4: Initial Configuration

```bash
# Update system
sudo apt update
sudo apt upgrade -y

# Configure locale, timezone, keyboard (optional)
sudo raspi-config
# Navigate: Localization Options
```

## Enabling I2C and Camera

### Method 1: Using raspi-config (Recommended)

```bash
sudo raspi-config
```

Follow these steps in the menu:
1. Select "Interface Options"
2. Select "I2C" → Enable it
3. Back to menu, select "Interface Options"
4. Select "Camera" → Enable it
5. Select "Finish"

**Reboot after changes:**
```bash
sudo reboot
```

### Method 2: Manual Configuration

Edit the boot configuration:
```bash
sudo nano /boot/firmware/config.txt
```

Add these lines:
```
dtparam=i2c_arm=on
dtparam=i2c_arm_baudrate=100000
start_x=1
```

Save and reboot:
```bash
sudo reboot
```

## Installing Required Software

### Step 1: Update System Packages

```bash
sudo apt update
sudo apt upgrade -y
```

### Step 2: Install Python Dependencies

```bash
sudo apt install python3-pip python3-venv python3-dev -y
sudo apt install build-essential -y
```

### Step 3: Install I2C Tools

```bash
sudo apt install i2c-tools -y
```

### Step 4: Install Computer Vision Libraries

```bash
sudo apt install libatlas-base-dev libjasper-dev libtiff5 -y
sudo apt install libharfess0 libwebp6 libtiff-tools -y
sudo apt install libopenjp2-7 libjasper1 -y
```

### Step 5: Install Camera Libraries (Optional)

```bash
sudo apt install libcamera0 -y
sudo apt install python3-libcamera -y
```

### Step 6: Install Node.js (For Frontend)

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install nodejs -y

# Verify installation
node --version
npm --version
```

## MLX90614 IR Sensor Setup

### Wiring Diagram

```
Raspberry Pi GPIO Header (40-pin):

┌─────────────────────────────────┐
│  1   3V3          2   5V        │
│  3   GPIO2 (SDA)  4   5V        │  Connect to MLX90614:
│  5   GPIO3 (SCL)  6   GND ──────┼──→ MLX90614 GND
│  7   GPIO4        8   GPIO14    │
│  9   GND ─────────────────────┐ │
│ 11   GPIO17       12  GPIO18   │ │
│                                 │ │
│      (Other pins...)            │ │
└─────────────────────────────────┘
                                  │
MLX90614 Connections:             │
├─ VCC   (Red)   → Pin 1 (3.3V)   │
├─ GND   (Black) → Pin 6/9 (GND) ─┘
├─ SDA   (Green) → Pin 3 (GPIO2)
└─ SCL   (Yellow)→ Pin 5 (GPIO3)
```

### Connection Steps

1. **Power off Raspberry Pi**
   ```bash
   sudo shutdown -h now
   ```

2. **Connect Wires to Sensor**
   - VCC → 3.3V (Red)
   - GND → Ground (Black)
   - SDA → GPIO2 (Green)
   - SCL → GPIO3 (Yellow)

3. **Connect to Raspberry Pi GPIO**
   - VCC → Pin 1 (3.3V)
   - GND → Pin 6 (GND)
   - SDA → Pin 3 (GPIO2)
   - SCL → Pin 5 (GPIO3)

4. **Power on Raspberry Pi**

### Verify I2C Connection

```bash
# List I2C devices
i2cdetect -y 1

# Expected output:
#      0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
# 00:                         -- -- -- -- -- -- -- --
# 10: -- -- -- -- -- -- -- -- -- -- 5a -- -- -- -- --
# 20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
```

The device at address **5a** (0x5A) is your MLX90614 sensor.

## Raspberry Pi Camera Setup

### Pi Camera Module 3 Installation

1. **Power off Raspberry Pi**
   ```bash
   sudo shutdown -h now
   ```

2. **Locate CSI Port**
   - Find the small ribbon connector labeled "CAMERA" on the board
   - It's located between the USB ports and GPIO header

3. **Open CSI Port**
   - Gently pull the ribbon connector clip upward
   - The clip will pivot up

4. **Insert Camera Ribbon**
   - Align the ribbon with the connector (blue sticker side facing away from USB)
   - Insert ribbon fully
   - Push the clip down to secure

5. **Power on Raspberry Pi**

### Test Camera

```bash
# Take a test photo
raspistill -o test.jpg

# Or with Python
python3 << 'EOF'
from picamera2 import Picamera2
camera = Picamera2()
camera.start_preview()
import time
time.sleep(3)
camera.capture_file("test.jpg")
print("Photo saved as test.jpg")
EOF
```

### Alternative: USB Webcam

If using USB Webcam instead:
1. Connect USB webcam to Raspberry Pi
2. Skip CSI installation
3. Backend will automatically detect USB camera

## Clone and Setup Health Insights Mirror

### Step 1: Clone Repository

```bash
cd ~
git clone https://github.com/saisushilreddy2809-hub/health-insights-mirror.git
cd health-insights-mirror
```

### Step 2: Set Up Backend

```bash
cd backend

# Create virtual environment
python3 -m venv venv
source venv/bin/activate

# Install dependencies
pip install --upgrade pip
pip install -r requirements.txt
```

### Step 3: Configure Backend

```bash
cp .env.example .env

# Edit configuration if needed
nano .env
```

**Recommended .env for Raspberry Pi:**
```env
FLASK_ENV=production
APP_MODE=hardware
DATABASE_URL=sqlite:///health_insights.db
IR_SENSOR_ADDRESS=0x5A
CAMERA_INDEX=0
SCAN_INTERVAL=30
LOG_LEVEL=INFO
```

### Step 4: Test Hardware Mode

```bash
# Test with demo mode first
python app.py --mode demo

# Then test hardware mode
python app.py --mode hardware
```

## Setting Up Frontend (Optional)

### Step 1: Set Up Frontend

```bash
cd ../frontend
npm install
cp .env.example .env
```

### Step 2: Configure for Raspberry Pi Display

```bash
nano .env
```

**Add or modify:**
```env
REACT_APP_API_URL=http://localhost:5000
PORT=3000
```

### Step 3: Start Frontend

```bash
npm start
```

## Running on System Startup

### Create Systemd Service

```bash
sudo nano /etc/systemd/system/health-mirror.service
```

**Paste this content:**
```ini
[Unit]
Description=Health Insights Mirror
After=network.target

[Service]
Type=simple
User=pi
WorkingDirectory=/home/pi/health-insights-mirror/backend
Environment="PATH=/home/pi/health-insights-mirror/backend/venv/bin"
ExecStart=/home/pi/health-insights-mirror/backend/venv/bin/python app.py --mode hardware
Restart=always
RestartSec=10
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```

**Enable and start service:**
```bash
sudo systemctl daemon-reload
sudo systemctl enable health-mirror.service
sudo systemctl start health-mirror.service

# Check status
sudo systemctl status health-mirror.service

# View logs
sudo journalctl -u health-mirror.service -f
```

## Setting Up Touchscreen (Optional)

### Install Touchscreen Drivers

```bash
# For common 7-inch displays
sudo apt install xinput-calibrator -y

# Check if display is detected
ls /dev/input/
```

### Calibrate Touchscreen

```bash
xinput_calibrator
```

Follow on-screen instructions to calibrate the touch input.

## Performance Optimization

### Reduce Memory Usage

```bash
# Disable desktop environment if not needed
sudo systemctl set-default multi-user.target

# Re-enable if needed
sudo systemctl set-default graphical.target
```

### Overclock (Advanced - Use with Caution)

```bash
sudo raspi-config
# Performance Options → Overclock → Select modest settings
```

### Monitor Temperature

```bash
# Check CPU temperature
vcgencmd measure_temp

# Real-time monitoring
watch -n 1 vcgencmd measure_temp
```

## Troubleshooting

### I2C Sensor Not Detected

```bash
# Check I2C devices
i2cdetect -y 1

# If no device at 0x5A:
# 1. Verify wiring
# 2. Check power connections
# 3. Ensure I2C is enabled (raspi-config)
# 4. Check pull-up resistors
```

### Camera Not Working

```bash
# Check if camera is detected
libcamera-hello

# Or test with Python
python3 -c "from picamera2 import Picamera2; Picamera2().start_preview()"

# Verify CSI cable connection
vcgencmd get_camera
```

### Backend Connection Issues

```bash
# Check port availability
sudo netstat -tlnp | grep 5000

# Kill process on port 5000 if needed
sudo lsof -ti:5000 | xargs kill -9
```

### Frontend Not Displaying

```bash
# Check if Node process is running
ps aux | grep node

# Clear npm cache
npm cache clean --force

# Reinstall dependencies
rm -rf node_modules package-lock.json
npm install
```

## Performance Benchmarks

### Expected Performance

| Component | Performance |
|-----------|------------|
| Temperature Reading | < 100ms |
| Image Capture | 500-1000ms |
| ML Analysis | 2-5 seconds |
| Total Scan Time | 3-7 seconds |
| Dashboard Update | < 1 second |

## Network Configuration

### Connect to WiFi

```bash
sudo raspi-config
# Network Options → WiFi
# Select your network and enter password
```

### Static IP (Optional)

```bash
sudo nano /etc/dhcpcd.conf
```

Add at end:
```
interface wlan0
static ip_address=192.168.1.100/24
static routers=192.168.1.1
static domain_name_servers=8.8.8.8 8.8.4.4
```

Reboot to apply.

## Mounting as Mirror (Physical Assembly)

### Materials Needed
- Two-way mirror glass (acrylic or actual mirror)
- Monitor/Display
- Frame
- Mounting brackets
- Power cables

### Assembly Steps
1. Mount display behind two-way mirror
2. Position Raspberry Pi beside display
3. Connect camera to face area
4. Install IR sensor (typically top-center)
5. Connect all power and data cables
6. Test functionality

## Monitoring and Maintenance

### Regular Updates

```bash
# Update system
sudo apt update
sudo apt upgrade -y

# Update Python packages
cd health-insights-mirror/backend
source venv/bin/activate
pip install --upgrade -r requirements.txt
```

### Log Files

```bash
# Service logs
sudo journalctl -u health-mirror.service -n 100

# Application logs
tail -f /home/pi/health-insights-mirror/backend/app.log
```

### Backup Database

```bash
# Backup health data
cp ~/health-insights-mirror/backend/health_insights.db ~/health_insights.db.backup

# Scheduled backup (cron)
# Edit crontab
crontab -e
# Add: 0 2 * * * cp ~/health-insights-mirror/backend/health_insights.db ~/backup/$(date +\%Y\%m\%d).db
```

## Next Steps

1. **Run Application**: Follow [SETUP_GUIDE.md](SETUP_GUIDE.md)
2. **Check API**: Review [API_DOCUMENTATION.md](API_DOCUMENTATION.md)
3. **Understand Architecture**: See [ARCHITECTURE.md](ARCHITECTURE.md)
4. **ML Models**: Learn about detection in [SKIN_CONDITIONS.md](SKIN_CONDITIONS.md)

---

**Need Help?** 
- Check Raspberry Pi docs: https://www.raspberrypi.com/documentation/
- Open issue: https://github.com/saisushilreddy2809-hub/health-insights-mirror/issues
