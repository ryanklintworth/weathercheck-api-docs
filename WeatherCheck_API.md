# WeatherCheck API Documentation

## Overview
> Instant Weather data access. With Weather Check, you are guaranteed instant and easy-to-use weather data. Developers have access to real-time weather information and can integrate forecast data into their application.

## Getting Started
### Authentication
- **Production** Base URL: `https://api.weathercheck.com/v1`
- Use your API key in the `Authorization` header.
- **Example (Production):**
```http
GET /current?location=Fayetteville,AR
Host: api.weathercheck.com
Authorization: Bearer <YOUR_API_KEY>
```

- **Testing enviroment** Test: `https://sandbox.apli.weathercheck.com/v1`
- **Example (Test):**
```http
GET /current?location=Bentonville,AR
Host: sandbox.api.weathercheck.com
Authorization: Bearer TEST-1234-API-KEY
```

- Response format: JSON
- Rate limits: e.g., 100 requests/min (429 status code to follow if exceeds 100 requests/min)

## Endpoints

### /weather/current
<!-- Method, path, purpose -->

#### Parameters
<!-- Table -->

#### Example request
```http
<!-- Placeholder for request -->