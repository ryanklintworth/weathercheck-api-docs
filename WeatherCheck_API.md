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
Purpose: Retrieve current weather data for a given location.
```

- Example (Sandbox)

```http
GET /weather/current?location=Bentonville,AR
Host: sandbox.api.weathercheck.com
Authorization: Bearer TEST-1234-API-KEY
Purpose: Retrieve current weather data for a given location.
```

### Parameters

| Name     | Label    | Type    | Required | Description |
|----------|----------|---------|----------|-------------|
| loc | Location | string  | Yes      | Name of the city and state, e.g., "Fayetteville, AR." |
| units    | Units | string | No | Measurement system: `metric` or `imperial`. |
| lang | Language | string | No | Given lanuage for condition descriptions (e.g., `en`, `es`, `fr`). |

- Example Request
```http
GET /weather/current?location=Fayetteville,AR
Authorization: Bearer <YOUR_API_KEY>
```

- Example Response
```
{
  "location": {
    "name": "Fayetteville",
    "region": "Arkansas",
    "country": "US"
  },
  "timestamp": 1755818773,
  "datetime": "2025-08-21T17:00:00Z",
  "current": {
    "temperature": 101,
    "feels_like": 107,
    "unit": "F",
    "humidity": 90,
    "condition": "Sunny",
    "wind_speed": 12.5,
    "wind_direction": "SW",
    "uv_index": 8
  }
}
```

### Field Definitions:
- `location` (string): Location name (e.g., "Tulsa, OK")
- `temperature` (float): Current temperature in the requested units.
- `unit` (string): "F" for Fahrenheir, "C" for Celsius.
- `humidity` (float): Relative humidity in percent.
- `condition` (string): Text description of weather (e.g., Sunny, Rainy). 

### Response Format
- All responses are returned in JSON

### Rate Limiting
- Production: 100 requests per min
- Sandbox: More generous limits for test
- Exceeding limits returns HTTP status `429`
