# WeatherCheck API Documentation

## Overview
> WeatherCheck API provides real-time weather information and forecasts, allowing developers to retrieve current conditions and integrate forecast data into their applications..

## Getting Started

### Base URLs
- **Production:** `https://api.weathercheck.com/v1`
- **Sandbox / Test:** `https://sandbox.api.weathercheck.com/v1`

### Authentication
- Use your API key in the `Authorization` header.

**Example (Production):**

```http
GET /weather/current?location=Fayetteville,AR
Host: api.weathercheck.com
Authorization: Bearer <YOUR_API_KEY>
Purpose: Retrieve current weather data for a given location.
```

**Example (Sandbox):**

```http
GET /weather/current?location=Bentonville,AR`
Host: sandbox.api.weathercheck.com
Authorization: Bearer TEST-1234-API-KEY
Purpose: Retrieve current weather data for a given location.
```

### Parameters
### Raw Parameter Metadata (as provided by engineers)

```
loc | Location | strng? | req’d | City+state, maybe zip?
units | measurment | opt. | imperial / metric
lang | language | str | maybe? | english default
timestamp | time | int? | bool-ish? | unix?
```
### Cleaned Parameter Table
| Name      | Label    | Type    | Required | Description |
|-----------|----------|---------|----------|-------------|
| loc       | Location | string  | Yes      | City and state, e.g., "Fayetteville, AR". |
| units     | Units    | string  | No       | Measurement system: `metric` or `imperial`. Defaults to `imperial`. |
| lang      | Language | string  | No       | Language code for condition descriptions (e.g., `en`, `es`, `fr`). Defaults to `en`. |
| timestamp | Time     | integer | No       | Unix epoch for historical lookups. If omitted, current time is used. |

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
    "temperature": 95,
    "feels_like": 101,
    "unit": "F",
    "humidity": 90,
    "condition": "Sunny",
    "wind_speed": 12.5,
    "wind_direction": "SW",
    "uv_index": 8
  }
}
```

### Field Definitions

#### `location` (object)  
Metadata about the requested location.  

| Name      | Type   | Description |
|-----------|--------|-------------|
| `name`    | string | City or town name (e.g., `"Fayetteville"`) |
| `region`  | string | State, province, or administrative region (e.g., `"Arkansas"`) |
| `country` | string | ISO country code or name (e.g., `"US"`) |

---

#### `timestamp` (integer)  
Unix epoch time (seconds since `1970-01-01T00:00:00Z`). Useful for programmatic time comparisons.  

#### `datetime` (string)  
Human-readable timestamp in ISO 8601 format (UTC), e.g., `"2025-08-21T17:00:00Z"`.  

---

#### `current` (object)  
Current observed or estimated weather conditions.  

| Name            | Type    | Description |
|-----------------|---------|-------------|
| `temperature`   | float   | Air temperature in the requested units |
| `feels_like`    | float   | Perceived temperature (“heat index” or “wind chill”) in the requested units |
| `unit`          | string  | Temperature unit: `"F"` for Fahrenheit or `"C"` for Celsius |
| `humidity`      | float   | Relative humidity as a percentage (0–100) |
| `condition`     | string  | Short text description of current weather (e.g., `"Sunny"`, `"Light Rain"`) |
| `wind_speed`    | float   | Wind speed in mph or kph (depending on system defaults) |
| `wind_direction`| string  | Cardinal direction of wind (e.g., `"N"`, `"SW"`) |
| `uv_index`      | integer | UV exposure index, typically `0–11+` |

---

### Process & Fixes

- **Types normalized:**
  - “strng?” → `string`
  - “int?” → `integer`
  - “bool-ish?” was a mistake; clarified as `integer` for Unix time

- **Required clarified:**
  - “req’d” kept as `Yes`
  - “opt.” and “maybe?” normalized to `No`

- **Descriptions fixed:**
  - “measurment” corrected to “Measurement system”
  - Expanded “City+state, maybe zip?” to clarify format but flagged uncertainty

- **Defaults assumed:**
  - `units=imperial`
  - `lang=en`
  - `timestamp=current time` when not provided

---

### Unclear / Questions for SME (Subject Matter Expert)

- Does `loc` accept ZIP codes or only `"City, State"`?
- Should `lang` accept full local codes (`en-US`, `fr-CA`) or only two-letter ISO codes?
- Can `timestamp` be used for future times, or only historical lookups?
- Should `units` error if invalid values are passed, or silently default?

### Rate Limiting
- Production: 100 requests per min
- Sandbox: More generous limits for test
- Exceeding limits returns HTTP status `429`