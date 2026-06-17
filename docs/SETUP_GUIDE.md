# Health Insights Mirror - Setup Guide

Complete step-by-step installation guide for Health Insights Mirror.

## Prerequisites

- **Python**: 3.10 or higher
- **Node.js**: 16 or higher
- **Git**: Latest version
- **Operating System**: Windows, macOS, or Linux
- **RAM**: 4GB minimum (8GB recommended)

## Backend Setup (Python/Flask)

### Step 1: Clone Repository

```bash
git clone https://github.com/saisushilreddy2809-hub/health-insights-mirror.git
cd health-insights-mirror
```

### Step 2: Navigate to Backend Directory

```bash
cd backend
```

### Step 3: Create Python Virtual Environment

**On macOS/Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

**On Windows:**
```bash
python -m venv venv
venv\Scripts\activate
```

### Step 4: Install Python Dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

### Step 5: Configure Environment Variables

```bash
# Copy example environment file
cp .env.example .env

# Edit .env with your settings (optional for demo mode)
# nano .env  # On macOS/Linux
# or use your editor on Windows
```

**Example .env configuration:**
```env
FLASK_ENV=development
APP_MODE=demo
DATABASE_URL=sqlite:///health_insights.db
SECRET_KEY=your-secret-key-here
LOG_LEVEL=INFO
```

### Step 6: Initialize Database

```bash
python -c "from database.db import init_db; init_db()"
```

### Step 7: Run Backend Server

**Demo Mode (No Hardware Required):**
```bash
python app.py --mode demo
```

**Hardware Mode (Requires Raspberry Pi + Sensors):**
```bash
python app.py --mode hardware
```

**Custom Configuration:**
```bash
python app.py --mode demo --host 0.0.0.0 --port 5000 --debug
```

**Output:**
```
Starting server on 0.0.0.0:5000
 * Running on http://0.0.0.0:5000
```

## Frontend Setup (React)

### Step 1: Navigate to Frontend Directory

```bash
cd ../frontend
```

### Step 2: Install Node Dependencies

```bash
npm install
```

**If you encounter issues:**
```bash
npm cache clean --force
npm install
```

### Step 3: Configure Environment

```bash
# Copy example environment file
cp .env.example .env

# Edit if needed
# nano .env
```

**Example .env configuration:**
```env
REACT_APP_API_URL=http://localhost:5000
REACT_APP_API_TIMEOUT=5000
REACT_APP_NAME=Health Insights Mirror
REACT_APP_VERSION=1.0.0
```

### Step 4: Start Development Server

```bash
npm start
```

**Output:**
```
Compiled successfully!

You can now view health-insights-mirror in the browser.

  Local:            http://localhost:3000
  On Your Network:  http://192.168.1.x:3000
```

The application will automatically open in your browser.

### Step 5 (Optional): Build for Production

```bash
npm run build
```

This creates an optimized production build in the `build/` directory.

## Running Both Backend and Frontend

### Option 1: Two Terminal Windows

**Terminal 1 - Backend:**
```bash
cd backend
source venv/bin/activate  # On Windows: venv\Scripts\activate
python app.py --mode demo
```

**Terminal 2 - Frontend:**
```bash
cd frontend
npm start
```

### Option 2: Using Concurrently (Advanced)

From the root directory:
```bash
npm install concurrently
npm run dev
```

## Docker Setup (Optional)

### Prerequisites
- Docker installed and running
- Docker Compose

### Build and Run

```bash
# From root directory
docker-compose up --build
```

**Access:**
- Frontend: http://localhost:3000
- Backend: http://localhost:5000

### Stopping Services

```bash
docker-compose down
```

## Verifying Installation

### Check Backend

```bash
curl http://localhost:5000/
```

**Expected Response:**
```json
{
  "name": "Health Insights Mirror",
  "version": "1.0.0",
  "status": "active",
  "mode": "demo",
  "message": "Smart mirror for real-time health insights"
}
```

### Check API Health

```bash
curl http://localhost:5000/api/health-check
```

**Expected Response:**
```json
{
  "status": "healthy",
  "mode": "demo",
  "message": "System is operational"
}
```

### Check Frontend

Open browser and navigate to:
```
http://localhost:3000
```

You should see the Health Insights Mirror dashboard.

## Troubleshooting

### Backend Issues

#### Port 5000 Already in Use
```bash
# Use a different port
python app.py --mode demo --port 8000
```

#### Module Not Found Error
```bash
# Ensure virtual environment is activated
source venv/bin/activate  # macOS/Linux
venv\Scripts\activate      # Windows

# Reinstall dependencies
pip install -r requirements.txt
```

#### Database Error
```bash
# Delete existing database and reinitialize
rm health_insights.db
python -c "from database.db import init_db; init_db()"
```

#### Sensor Connection Error (Hardware Mode)
```bash
# Check I2C devices
i2cdetect -y 1

# Try demo mode instead
python app.py --mode demo
```

### Frontend Issues

#### Port 3000 Already in Use
The system will prompt to use port 3001:
```
? Something is already running on port 3000.
Would you like to run this app on another port instead? (Y/n)
```
Press `Y` to continue on port 3001.

#### Backend Connection Error
Ensure backend is running:
```bash
# In another terminal
cd backend
python app.py --mode demo
```

#### Node Modules Issue
```bash
# Clear cache and reinstall
rm -rf node_modules package-lock.json  # macOS/Linux
rmdir /s node_modules                  # Windows
npm cache clean --force
npm install
```

#### npm start Fails
```bash
# Update npm
npm install -g npm@latest

# Try again
npm start
```

## Development Workflow

### Making Changes to Backend

1. Backend runs with auto-reload in development
2. Edit files in `backend/`
3. Changes apply automatically
4. Check browser for updated data

### Making Changes to Frontend

1. Frontend has hot-reload enabled
2. Edit files in `frontend/src/`
3. Browser automatically refreshes
4. Check browser console for errors

### Adding Dependencies

**Backend:**
```bash
cd backend
pip install package-name
pip freeze > requirements.txt
```

**Frontend:**
```bash
cd frontend
npm install package-name
```

## Environment Variables

### Backend (.env)

| Variable | Default | Description |
|----------|---------|-------------|
| `FLASK_ENV` | development | Flask environment |
| `APP_MODE` | demo | Operating mode |
| `DATABASE_URL` | sqlite:///health_insights.db | Database URL |
| `SECRET_KEY` | dev-secret-key | Flask secret |
| `IR_SENSOR_ADDRESS` | 0x5A | I2C sensor address |
| `CAMERA_INDEX` | 0 | Camera device index |
| `SCAN_INTERVAL` | 30 | Scan interval (seconds) |
| `DATA_RETENTION_DAYS` | 30 | Data retention (days) |

### Frontend (.env)

| Variable | Default | Description |
|----------|---------|-------------|
| `REACT_APP_API_URL` | http://localhost:5000 | Backend API URL |
| `REACT_APP_API_TIMEOUT` | 5000 | API timeout (ms) |
| `REACT_APP_NAME` | Health Insights Mirror | App name |
| `REACT_APP_VERSION` | 1.0.0 | App version |

## Next Steps

1. **Read Documentation**: Check [ARCHITECTURE.md](ARCHITECTURE.md) for system design
2. **Hardware Setup**: For Raspberry Pi, see [HARDWARE_SETUP.md](HARDWARE_SETUP.md)
3. **API Reference**: Check [API_DOCUMENTATION.md](API_DOCUMENTATION.md)
4. **Skin Detection**: Learn about models in [SKIN_CONDITIONS.md](SKIN_CONDITIONS.md)

## Getting Help

- Check logs for error messages
- Review [Troubleshooting](#troubleshooting) section
- Open an issue on GitHub
- Review existing issues for solutions

## Quick Reference

```bash
# Start everything (from root)
# Terminal 1
cd backend && source venv/bin/activate && python app.py --mode demo

# Terminal 2
cd frontend && npm start

# Access
# Frontend: http://localhost:3000
# Backend: http://localhost:5000
# API: http://localhost:5000/api
```

---

**Need Help?** Open an issue on [GitHub](https://github.com/saisushilreddy2809-hub/health-insights-mirror/issues)
