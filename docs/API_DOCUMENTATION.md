# API Documentation - Health Insights Mirror

Complete REST API reference for Health Insights Mirror backend.

## Base URL

```
http://localhost:5000
```

## Authentication

Currently, no authentication is required. In production, implement JWT tokens.

## Response Format

All responses are in JSON format:

```json
{
  "status": "success|error",
  "data": {},
  "message": "Description"
}
```

---

## Core Endpoints

### Root Endpoint

#### GET /

**Description:** Get API information

**Response:**
```json
{
  "name": "Health Insights Mirror",
  "version": "1.0.0",
  "status": "active",
  "mode": "demo",
  "message": "Smart mirror for real-time health insights"
}
```

**Status Code:** 200 OK

---

### Health Check

#### GET /api/health-check

**Description:** Check if system is operational

**Response:**
```json
{
  "status": "healthy",
  "mode": "demo",
  "message": "System is operational"
}
```

**Status Code:** 200 OK

---

## Health Data Endpoints

### Get Current Health Metrics

#### GET /api/health/current

**Description:** Retrieve the latest health metrics including temperature and skin analysis

**Query Parameters:** None

**Response:**
```json
{
  "status": "success",
  "data": {
    "timestamp": "2024-06-16T10:30:00.000000",
    "temperature": 36.8,
    "unit": "celsius",
    "skin_analysis": {
      "acne": {
        "detected": false,
        "severity": "none",
        "confidence": 0.95,
        "affected_areas": 0,
        "locations": []
      },
      "dark_spots": {
        "detected": false,
        "count": 0,
        "severity": "none",
        "confidence": 0.92,
        "locations": []
      },
      "dryness": {
        "detected": false,
        "level": "none",
        "confidence": 0.88,
        "affected_percentage": 0.0,
        "recommendation": "Skin moisture level is optimal"
      },
      "skin_tone": {
        "primary_tone": "medium",
        "undertone": "warm",
        "color_hex": "#c89968",
        "rgb": [200, 153, 104]
      }
    },
    "health_score": 85.5,
    "recommendations": [
      "Maintain consistent skincare routine",
      "Use SPF 30+ daily sunscreen",
      "Stay hydrated - drink 8 glasses of water",
      "Get 7-9 hours of sleep"
    ]
  }
}
```

**Status Code:** 200 OK

**Error Response (500):**
```json
{
  "status": "error",
  "message": "Failed to fetch current health data"
}
```

---

### Get Health History

#### GET /api/health/history

**Description:** Retrieve historical health data

**Query Parameters:**
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | 24 | Number of records to retrieve |
| `unit` | string | hours | Time unit: `hours` or `days` |
| `offset` | integer | 0 | Pagination offset |

**Example Request:**
```
GET /api/health/history?limit=24&unit=hours
```

**Response:**
```json
{
  "status": "success",
  "count": 24,
  "limit": 24,
  "offset": 0,
  "data": [
    {
      "timestamp": "2024-06-16T10:30:00.000000",
      "temperature": 36.8,
      "unit": "celsius",
      "skin_analysis": {...},
      "health_score": 85.5
    },
    {
      "timestamp": "2024-06-16T10:00:00.000000",
      "temperature": 36.7,
      "unit": "celsius",
      "skin_analysis": {...},
      "health_score": 84.2
    }
  ]
}
```

**Status Code:** 200 OK

---

### Trigger New Health Scan

#### POST /api/health/scan

**Description:** Manually trigger a new health scan (captures image, reads temperature, analyzes skin)

**Request Body:** (Optional)
```json
{
  "save_image": true,
  "full_analysis": true
}
```

**Response:**
```json
{
  "status": "success",
  "message": "Scan completed successfully",
  "duration_ms": 3500,
  "data": {
    "timestamp": "2024-06-16T10:35:00.000000",
    "temperature": 36.9,
    "unit": "celsius",
    "skin_analysis": {
      "acne": {
        "detected": false,
        "severity": "none",
        "confidence": 0.96,
        "affected_areas": 0,
        "locations": []
      },
      "dark_spots": {
        "detected": true,
        "count": 2,
        "severity": "mild",
        "confidence": 0.78,
        "locations": ["left_cheek", "nose"]
      },
      "dryness": {
        "detected": false,
        "level": "mild",
        "confidence": 0.65,
        "affected_percentage": 5.2,
        "recommendation": "Apply light moisturizer in dry areas"
      },
      "skin_tone": {
        "primary_tone": "medium",
        "undertone": "warm",
        "color_hex": "#c89968",
        "rgb": [200, 153, 104]
      }
    },
    "health_score": 82.1,
    "recommendations": [
      "Monitor dark spots for changes",
      "Apply targeted moisturizer to dry areas",
      "Use SPF 30+ daily sunscreen",
      "Maintain hydration"
    ]
  }
}
```

**Status Code:** 200 OK

**Error Response:**
```json
{
  "status": "error",
  "message": "Scan failed - camera not available"
}
```

---

### Get Latest Skin Analysis

#### GET /api/health/skin-analysis

**Description:** Get the most recent skin analysis results

**Query Parameters:** None

**Response:**
```json
{
  "status": "success",
  "data": {
    "timestamp": "2024-06-16T10:30:00.000000",
    "acne": {
      "detected": false,
      "severity": "none",
      "confidence": 0.95,
      "affected_areas": 0,
      "locations": []
    },
    "dark_spots": {
      "detected": false,
      "count": 0,
      "severity": "none",
      "confidence": 0.92,
      "locations": []
    },
    "dryness": {
      "detected": false,
      "level": "none",
      "confidence": 0.88,
      "affected_percentage": 0.0,
      "recommendation": "Skin moisture level is optimal"
    },
    "skin_tone": {
      "primary_tone": "medium",
      "undertone": "warm",
      "color_hex": "#c89968",
      "rgb": [200, 153, 104]
    }
  }
}
```

**Status Code:** 200 OK

---

## Configuration Endpoints

### Get System Configuration

#### GET /api/config

**Description:** Retrieve current system configuration

**Response:**
```json
{
  "status": "success",
  "config": {
    "sensitivity": "medium",
    "auto_scan": true,
    "scan_interval": 30,
    "notifications": true,
    "data_retention_days": 30,
    "temperature_unit": "celsius",
    "auto_save_images": false
  }
}
```

**Status Code:** 200 OK

---

### Update System Configuration

#### POST /api/config

**Description:** Update system configuration settings

**Request Body:**
```json
{
  "sensitivity": "high",
  "auto_scan": true,
  "scan_interval": 60,
  "notifications": false,
  "auto_save_images": true
}
```

**Response:**
```json
{
  "status": "success",
  "message": "Configuration updated successfully",
  "config": {
    "sensitivity": "high",
    "auto_scan": true,
    "scan_interval": 60,
    "notifications": false,
    "data_retention_days": 30,
    "temperature_unit": "celsius",
    "auto_save_images": true
  }
}
```

**Status Code:** 200 OK

**Error Response:**
```json
{
  "status": "error",
  "message": "Invalid configuration parameter"
}
```

---

## System Endpoints

### Get Operating Mode

#### GET /api/mode

**Description:** Get current operating mode (hardware or demo)

**Response:**
```json
{
  "status": "success",
  "mode": "demo"
}
```

**Possible Values:**
- `demo` - Simulated data mode
- `hardware` - Real sensor mode

**Status Code:** 200 OK

---

### Get System Status

#### GET /api/system/status

**Description:** Get detailed system status including sensor states

**Response:**
```json
{
  "status": "success",
  "system": {
    "mode": "demo",
    "uptime": "02:45:30",
    "version": "1.0.0",
    "sensors": {
      "ir_sensor": "active",
      "camera": "active",
      "display": "active"
    },
    "temperature": 36.8,
    "last_scan": "2024-06-16T10:30:00.000000",
    "health_score": 85.5,
    "database": {
      "records": 1250,
      "size_mb": 5.2
    }
  }
}
```

**Status Code:** 200 OK

---

## Data Models

### Health Reading Object

```json
{
  "id": 1,
  "timestamp": "2024-06-16T10:30:00.000000",
  "temperature": 36.8,
  "unit": "celsius",
  "skin_analysis": {
    "acne": {...},
    "dark_spots": {...},
    "dryness": {...},
    "skin_tone": {...}
  },
  "health_score": 85.5,
  "recommendations": [...],
  "mode": "demo",
  "image_path": null
}
```

### Acne Detection Object

```json
{
  "detected": false,
  "severity": "none|mild|moderate|severe",
  "confidence": 0.95,
  "affected_areas": 0,
  "locations": ["forehead", "cheeks", "chin"]
}
```

### Dark Spots Detection Object

```json
{
  "detected": false,
  "count": 0,
  "severity": "none|mild|moderate",
  "confidence": 0.92,
  "locations": ["cheeks", "temples", "nose"]
}
```

### Dryness Analysis Object

```json
{
  "detected": false,
  "level": "none|mild|moderate|severe",
  "confidence": 0.88,
  "affected_percentage": 0.0,
  "recommendation": "Skin moisture level is optimal"
}
```

### Skin Tone Analysis Object

```json
{
  "primary_tone": "fair|light|medium|tan|deep",
  "undertone": "cool|warm|neutral",
  "color_hex": "#c89968",
  "rgb": [200, 153, 104]
}
```

---

## Error Responses

### 400 - Bad Request

```json
{
  "status": "error",
  "error": "Bad Request",
  "message": "Invalid request parameters"
}
```

### 404 - Not Found

```json
{
  "status": "error",
  "error": "Not Found",
  "message": "The requested endpoint does not exist"
}
```

### 500 - Internal Server Error

```json
{
  "status": "error",
  "error": "Internal Server Error",
  "message": "An unexpected error occurred"
}
```

### 503 - Service Unavailable

```json
{
  "status": "error",
  "error": "Service Unavailable",
  "message": "Service is temporarily unavailable"
}
```

---

## HTTP Status Codes

| Code | Meaning | Use Case |
|------|---------|----------|
| 200 | OK | Successful request |
| 201 | Created | Resource created successfully |
| 400 | Bad Request | Invalid parameters |
| 404 | Not Found | Endpoint not found |
| 500 | Server Error | Internal server error |
| 503 | Unavailable | Service temporarily down |

---

## Rate Limiting

Currently, no rate limiting is implemented. In production:
- Limit to 60 requests/minute per IP
- Implement API key system
- Add token bucket algorithm

---

## Example cURL Requests

### Get Current Health

```bash
curl -X GET http://localhost:5000/api/health/current \
  -H "Content-Type: application/json"
```

### Trigger New Scan

```bash
curl -X POST http://localhost:5000/api/health/scan \
  -H "Content-Type: application/json" \
  -d '{
    "save_image": true,
    "full_analysis": true
  }'
```

### Update Configuration

```bash
curl -X POST http://localhost:5000/api/config \
  -H "Content-Type: application/json" \
  -d '{
    "sensitivity": "high",
    "scan_interval": 60
  }'
```

### Get History

```bash
curl -X GET "http://localhost:5000/api/health/history?limit=24&unit=hours" \
  -H "Content-Type: application/json"
```

---

## Example JavaScript Requests

### Using Fetch API

```javascript
// Get current health
const response = await fetch('http://localhost:5000/api/health/current');
const data = await response.json();
console.log(data);

// Trigger new scan
const scanResponse = await fetch('http://localhost:5000/api/health/scan', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    save_image: true,
    full_analysis: true
  })
});
const scanData = await scanResponse.json();
console.log(scanData);
```

### Using Axios

```javascript
import axios from 'axios';

const API_URL = 'http://localhost:5000';

// Get current health
const health = await axios.get(`${API_URL}/api/health/current`);
console.log(health.data);

// Trigger scan
const scan = await axios.post(`${API_URL}/api/health/scan`, {
  save_image: true,
  full_analysis: true
});
console.log(scan.data);

// Get config
const config = await axios.get(`${API_URL}/api/config`);
console.log(config.data);

// Update config
const updated = await axios.post(`${API_URL}/api/config`, {
  sensitivity: 'high',
  scan_interval: 60
});
console.log(updated.data);
```

---

## Example Python Requests

### Using requests Library

```python
import requests
import json

API_URL = 'http://localhost:5000'

# Get current health
response = requests.get(f'{API_URL}/api/health/current')
health_data = response.json()
print(json.dumps(health_data, indent=2))

# Trigger new scan
scan_response = requests.post(f'{API_URL}/api/health/scan', json={
    'save_image': True,
    'full_analysis': True
})
scan_data = scan_response.json()
print(json.dumps(scan_data, indent=2))

# Get configuration
config_response = requests.get(f'{API_URL}/api/config')
config_data = config_response.json()
print(json.dumps(config_data, indent=2))

# Update configuration
update_response = requests.post(f'{API_URL}/api/config', json={
    'sensitivity': 'high',
    'scan_interval': 60
})
update_data = update_response.json()
print(json.dumps(update_data, indent=2))

# Get history
history_response = requests.get(f'{API_URL}/api/health/history', params={
    'limit': 24,
    'unit': 'hours'
})
history_data = history_response.json()
print(json.dumps(history_data, indent=2))
```

---

## WebSocket Events (Future Enhancement)

Planned for v2.0:

```javascript
// Real-time health updates
io.on('health:update', (data) => {
  console.log('New health data:', data);
});

// Scan progress
io.on('scan:progress', (progress) => {
  console.log(`Scan ${progress.percent}% complete`);
});

// Sensor events
io.on('sensor:alert', (alert) => {
  console.log('Sensor alert:', alert);
});
```

---

## Pagination

For endpoints that support pagination:

```
GET /api/health/history?limit=10&offset=20
```

**Response includes:**
```json
{
  "status": "success",
  "count": 10,
  "total": 1250,
  "limit": 10,
  "offset": 20,
  "has_next": true,
  "has_previous": true,
  "data": [...]
}
```

---

## Filtering

Future enhancement for advanced filtering:

```
GET /api/health/history?from=2024-06-01&to=2024-06-16&min_score=80
```

---

## Sorting

Future enhancement for sorting:

```
GET /api/health/history?sort_by=timestamp&order=desc
```

---

## API Testing Tools

### Postman Collection

Import this collection into Postman:

```json
{
  "info": {
    "name": "Health Insights Mirror API",
    "version": "1.0.0"
  },
  "item": [
    {
      "name": "Get Current Health",
      "request": {
        "method": "GET",
        "url": "http://localhost:5000/api/health/current"
      }
    },
    {
      "name": "Trigger Scan",
      "request": {
        "method": "POST",
        "url": "http://localhost:5000/api/health/scan",
        "body": {
          "mode": "raw",
          "raw": "{\"save_image\": true}"
        }
      }
    }
  ]
}
```

### REST Client (VS Code Extension)

```http
### Health Insights Mirror API

### Get current health
GET http://localhost:5000/api/health/current

### Trigger new scan
POST http://localhost:5000/api/health/scan
Content-Type: application/json

{
  "save_image": true,
  "full_analysis": true
}

### Get configuration
GET http://localhost:5000/api/config

### Update configuration
POST http://localhost:5000/api/config
Content-Type: application/json

{
  "sensitivity": "high",
  "scan_interval": 60
}

### Get health history
GET http://localhost:5000/api/health/history?limit=24&unit=hours

### Get system status
GET http://localhost:5000/api/system/status

### Get operating mode
GET http://localhost:5000/api/mode
```

---

## Versioning

Current API Version: **1.0.0**

Future versions will maintain backward compatibility with `/api/v1/` prefix.

---

## Security Considerations

- Implement HTTPS in production
- Add API key authentication
- Rate limit requests
- Validate all input
- Use CORS headers appropriately
- Sanitize sensor data before storage

---

## Performance

| Endpoint | Avg Response Time |
|----------|------------------|
| `/api/health/current` | 50-100ms |
| `/api/health/history` | 100-200ms |
| `/api/health/scan` | 3-7 seconds |
| `/api/config` | 50-100ms |
| `/api/system/status` | 50-100ms |

---

## Troubleshooting

### 500 Error on Scan

- Check camera is connected
- Verify IR sensor is responding
- Check available storage
- Review backend logs

### Connection Refused

- Ensure backend is running: `python app.py`
- Check port 5000 is available
- Verify firewall settings

### Slow Response

- Check system resources (CPU, RAM)
- Review database size
- Consider archiving old data

---

## Change Log

### v1.0.0 (2024-06-16)
- Initial API release
- Health data endpoints
- Skin analysis endpoints
- Configuration management
- System status monitoring

### Planned v1.1.0
- WebSocket support
- Advanced filtering
- Data export (CSV, JSON)
- Historical analysis

### Planned v2.0.0
- API key authentication
- Role-based access control
- Mobile app endpoints
- Cloud synchronization

---

## Support

For API issues:
1. Check [API Documentation](API_DOCUMENTATION.md)
2. Review [Architecture](ARCHITECTURE.md)
3. Check backend logs
4. Open GitHub issue

---

**Last Updated:** June 16, 2024
**API Version:** 1.0.0
**Status:** Production Ready
