# WeatherCheck API Documentation

## Overview
> WeatherCheck API provides real-time weather information and forecasts, allowing developers to retrieve current conditions and integrate forecast data into their applications..

## Getting Started

### Base URLs
- **Production:** `https://api.weathercheck.com/v1`
- **Sandbox / Test:** `https://sandbox.api.weathercheck.com/v1`

### Authentication
- Use your API key in the `Authorization` header.
- Example (Production):
```http
GET /weather/current?location=Fayetteville,AR
Host: api.weathercheck.com
Authorization: Bearer <YOUR_API_KEY>
```
- Example (Sandbox)
```http
GET /weather/current?location=Bentonville,AR
Host: sandbox.api.weathercheck.com
Authorization: Bearer TEST-1234-API-KEY
```
### Response Format
- All responses are returned in JSON

### Rate Limiting
- Production: 100 requests per min
- Sandbox: More generous limits for test
- Exceeding limits returns HTTP status `429`

### Endpoints

```http
GET /weather/current
Purpose: Retrieve current weather data for a given location.
```

### Parameters
| Name     | Label    | Type    | Required | Description |
|----------|----------|---------|----------|-------------|
| Location | Location | string  | yes      | Name of the city and state, e.g., "Fayetteville, AR." |
| units    | Units | string | no | Measurement system: "metric" or "imperial". |

- Example Request

```http
GET /weather/current?location=Fayetteville,AR
Authorization: Bearer <YOUR_API_KEY>
```

- Example Response
```
{
    "location": "Fayetteville, AR",
    "temperature": 101,
    "unit": "F",
    "humidity": 90,
    "condition": "Hotter than the surface of the sun..."
}
```
