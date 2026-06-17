# Health Insights Mirror 🪞✨

An AI-powered smart mirror system that provides real-time health insights using non-contact methods. The system measures body temperature via infrared sensors and analyzes facial images to detect skin conditions including acne, dark spots, dryness, and skin tone analysis.

## 🎯 Project Overview

Health Insights Mirror is an intelligent system designed to:
- Capture real-time body temperature using infrared (IR) sensors
- Analyze facial images for skin condition detection
- Detect: **Acne, Dark Spots, Dryness, Skin Tone Analysis**
- Display health metrics on a mirror interface
- Support both hardware (Raspberry Pi) and software (simulated) modes

## 🏗️ Architecture

```
┌─────────────────────────────────────────┐
│     Frontend (React Dashboard)          │
│  - Real-time health display             │
│  - Historical data visualization        │
│  - User settings                        │
└──────────────┬──────────────────────────┘
               │ REST API
┌──────────────▼──────────────────────────┐
│     Backend (Python API)                │
│  - Sensor integration                   │
│  - ML/AI image analysis                 │
│  - Data processing                      │
└──────────────┬──────────────────────────┘
               │
       ┌───────┴──────────┐
       │                  │
  ┌────▼────┐      ┌─────▼────┐
  │ Hardware │      │ Simulated │
  │ Mode     │      │   Mode    │
  │ (Pi)     │      │ (Demo)    │
  └──────────┘      └───────────┘
```

## 📋 Features

### MVP Features
- ✅ Real-time IR temperature sensing
- ✅ Facial image capture and analysis
- ✅ Acne detection
- ✅ Dark spots detection
- ✅ Skin dryness analysis
- ✅ Skin tone analysis
- ✅ Live dashboard display
- ✅ Historical data storage
- ✅ Demo mode (simulated data)

## 🛠️ Tech Stack

| Component | Technology |
|-----------|------------|
| **Backend** | Python 3.10+ |
| **API Framework** | Flask |
| **Frontend** | React 18+ |
| **ML/AI** | TensorFlow, OpenCV |
| **Database** | SQLite (local) |
| **Hardware Platform** | Raspberry Pi 4 |
| **IR Sensor** | MLX90614 |
| **Camera** | Raspberry Pi Camera Module 3 / USB Webcam |
| **Containerization** | Docker (optional) |

## 📁 Project Structure

```
health-insights-mirror/
├── backend/                          # Python backend
│   ├── app.py                        # Main Flask application
│   ├── requirements.txt              # Python dependencies
│   ├── config.py                     # Configuration management
│   ├── models/                       # ML detection models
│   │   ├── acne_detector.py         # Acne detection
│   │   ├── dark_spots_detector.py   # Dark spots detection
│   │   ├── dryness_detector.py      # Skin dryness analysis
│   │   └── skin_tone_analyzer.py    # Skin tone analysis
│   ├── sensors/                      # Hardware sensor integration
│   │   ├── ir_sensor.py              # MLX90614 IR sensor
│   │   ├── camera.py                 # Camera interface
│   │   └── sensor_manager.py         # Sensor orchestration
│   ├── routes/                       # API endpoint handlers
│   │   ├── health.py                 # Health data endpoints
│   │   ├── config.py                 # Configuration endpoints
│   │   └── system.py                 # System status endpoints
│   ├── utils/                        # Utility functions
│   │   ├── image_processing.py      # Image manipulation
│   │   ├── data_processing.py       # Data analysis utilities
│   │   └── demo_data.py              # Demo data generator
│   ├── database/                     # Data persistence layer
│   │   ├── db.py                     # Database initialization
│   │   └── models.py                 # SQLAlchemy models
│   └── .env.example                  # Environment configuration template
│
├── frontend/                         # React frontend application
│   ├── public/
│   │   └── index.html               # HTML entry point
│   ├── src/
│   │   ├── components/               # React components
│   │   │   ├── Dashboard.jsx        # Main dashboard
│   │   │   ├── HealthMetrics.jsx    # Health score display
│   │   │   ├── SkinAnalysis.jsx     # Skin condition display
│   │   │   └── TemperatureDisplay.jsx # Temperature gauge
│   │   ├── services/                 # API client services
│   │   │   └── api.js               # Axios configuration
│   │   ├── App.jsx                  # Root component
│   │   ├── index.js                 # React entry point
│   │   └── styles/                  # CSS stylesheets
│   ├── package.json                 # NPM dependencies
│   └── .env.example                 # Environment configuration
│
├── docs/                            # Documentation
│   ├── SETUP_GUIDE.md               # Installation & setup guide
│   ├── HARDWARE_SETUP.md            # Raspberry Pi configuration
│   ├── API_DOCUMENTATION.md         # REST API reference
│   ├── ARCHITECTURE.md              # System architecture
│   └── SKIN_CONDITIONS.md           # Detection algorithm details
│
├── docker/                          # Docker configuration
│   ├── Dockerfile.backend
│   ├── Dockerfile.frontend
│   └── docker-compose.yml
│
├── .gitignore                       # Git ignore rules
├── README.md                        # This file
└── LICENSE                          # MIT License
```

## 🚀 Quick Start

### Prerequisites
- Python 3.10 or higher
- Node.js 16 or higher
- Git
- Virtual environment support (venv)

### Backend Setup

```bash
# Clone repository
git clone https://github.com/saisushilreddy2809-hub/health-insights-mirror.git
cd health-insights-mirror/backend

# Create Python virtual environment
python -m venv venv

# Activate virtual environment
# On macOS/Linux:
source venv/bin/activate
# On Windows:
venv\Scripts\activate

# Install Python dependencies
pip install -r requirements.txt

# Configure environment
cp .env.example .env

# Run in demo mode (no hardware needed)
python app.py --mode demo

# Or run in hardware mode (requires Raspberry Pi + sensors)
python app.py --mode hardware
```

Backend API will be available at `http://localhost:5000`

### Frontend Setup

```bash
# Navigate to frontend directory
cd ../frontend

# Install Node dependencies
npm install

# Configure environment
cp .env.example .env

# Start development server
npm start

# Build for production
npm run build
```

Frontend will open automatically at `http://localhost:3000`

## 📊 API Endpoints

### Health Data
```
GET  /api/health/current      - Get current health metrics
GET  /api/health/history      - Get historical health data
POST /api/health/scan         - Trigger new health scan
GET  /api/health/skin-analysis - Get latest skin analysis
```

### Configuration
```
GET  /api/config              - Get system configuration
POST /api/config              - Update configuration
```

### System
```
GET  /api/mode                - Get operating mode (hardware/demo)
GET  /api/system/status       - Get system status
GET  /api/health-check        - Health check endpoint
```

Full API documentation: See [API_DOCUMENTATION.md](docs/API_DOCUMENTATION.md)

## 🧠 ML Models

### Acne Detection
- Identifies pimples and acne breakouts
- Pre-trained CNN model using OpenCV
- Confidence score: 0-100%
- Location-based analysis
- Severity classification: None/Mild/Moderate/Severe

### Dark Spots Detection
- Identifies hyperpigmentation and dark patches
- Color anomaly detection algorithm
- Location mapping on facial regions
- Count and area measurement
- Severity assessment

### Skin Dryness Analysis
- Detects skin texture irregularities
- Analyzes moisture level indicators
- Risk assessment: None/Mild/Moderate/Severe
- Affected area percentage calculation
- Personalized recommendations

### Skin Tone Analysis
- Identifies primary skin tone
- Determines undertone (cool/warm/neutral)
- RGB and HEX color profile
- Inclusive analysis for all skin types
- Color harmony suggestions

## 🎮 Demo Mode

Perfect for presentations and testing without hardware:

```bash
cd backend
python app.py --mode demo
```

**Demo Mode Features:**
- Generates realistic simulated health data
- No hardware dependencies
- Perfect for testing UI/UX
- Ideal for presentations to stakeholders
- Complete feature demonstration

## 🔧 Hardware Setup (Raspberry Pi)

### Required Components
- Raspberry Pi 4 (8GB RAM recommended)
- MLX90614 IR Temperature Sensor (~$10)
- Raspberry Pi Camera Module 3 (~$25)
- microSD card (64GB)
- 5V 3A power supply
- 7-10" display (optional)

### I2C Sensor Wiring
```
Raspberry Pi GPIO:
├── Pin 1 (3.3V) → MLX90614 VCC
├── Pin 3 (SDA)  → MLX90614 SDA
├── Pin 5 (SCL)  → MLX90614 SCL
└── Pin 6 (GND)  → MLX90614 GND
```

Detailed hardware setup: See [HARDWARE_SETUP.md](docs/HARDWARE_SETUP.md)

## 🐳 Docker Deployment

### Build and Run with Docker

```bash
# Build images
docker-compose build

# Start services
docker-compose up

# Access:
# Frontend: http://localhost:3000
# Backend: http://localhost:5000
```

## 📚 Documentation

Complete documentation available:

- **[Setup Guide](docs/SETUP_GUIDE.md)** - Step-by-step installation for all platforms
- **[Hardware Setup](docs/HARDWARE_SETUP.md)** - Raspberry Pi and sensor configuration
- **[API Documentation](docs/API_DOCUMENTATION.md)** - Complete API reference
- **[Architecture](docs/ARCHITECTURE.md)** - System design and data flow
- **[Skin Conditions](docs/SKIN_CONDITIONS.md)** - Detection algorithm details

## 💡 Key Highlights

### Dual-Mode Operation
- **Hardware Mode**: Real sensors on Raspberry Pi
- **Demo Mode**: Simulated data for testing/presentations

### Scalable Architecture
- Modular component design
- Easy to extend with new detection models
- Database-ready for scaling

### User-Friendly Interface
- Real-time dashboard with live updates
- Beautiful data visualization
- Mobile-responsive design

### Accurate Detection
- ML-powered skin analysis
- Non-contact IR temperature sensing
- Multi-condition detection

## 🤝 Contributing

We welcome contributions! To contribute:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👥 Team

- **Sai Sushil Reddy** - Project Lead & Developer

## 📧 Contact & Support

For questions, suggestions, or bug reports:
- Open an issue on GitHub
- Contact: saisushilreddy2809@example.com

## 🙏 Acknowledgments

- OpenCV and TensorFlow communities for excellent ML libraries
- Raspberry Pi Foundation for amazing hardware
- All contributors and testers

## 📅 Roadmap

### Phase 1 (Current)
- ✅ Basic health metrics collection
- ✅ Skin condition detection
- ✅ Demo mode implementation

### Phase 2
- 🔄 Mobile app development
- 🔄 Cloud synchronization
- 🔄 Advanced ML models

### Phase 3
- ⏳ AI-powered recommendations
- ⏳ Multi-user support
- ⏳ Integration with health apps

---

**Project Status**: 🚀 In Development (Prototype Phase)

**Version**: 1.0.0

**Last Updated**: June 16, 2026

**Repository**: [GitHub](https://github.com/saisushilreddy2809-hub/health-insights-mirror)
