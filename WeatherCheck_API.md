# WeatherCheck API Documentation

## Overview
> WeatherCheck API provides real-time weather information and forecasts, allowing developers to retrieve current conditions and integrate forecast data into their applications.

## Getting Started

### Base URL
- **Production:** `https://api.weathercheck.com/v1`

### Authentication
- Use your API key in the `Authorization` header.

## Example Current Weather:
```http
GET /weather/current?location=Fayetteville,AR
Host: api.weathercheck.com
Authorization: Bearer <YOUR_API_KEY>
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
| location  | Location | string  | Yes      | City and state, e.g., "Fayetteville, AR". |
| units     | Units    | string  | No       | Measurement system: `metric` or `imperial`. Defaults to `imperial`. |
| lang      | Language | string  | No       | Language code for condition descriptions (e.g., `en`, `es`, `fr`). Defaults to `en`. |
| timestamp | Time     | integer | No       | Unix epoch for historical lookups. If omitted, current time is used. |

### Example Request
```http
GET /weather/current?location=Fayetteville,AR
Authorization: Bearer <YOUR_API_KEY>
```

### Example Response
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
| Name            | Type    | Description |
|-----------------|---------|-------------|
| `temperature`   | float   | Air temperature in the requested units |
| `feels_like`    | float   | Perceived temperature (“heat index” or “wind chill”) in the requested units |
| `unit`          | string  | Temperature unit: `"F"` for Fahrenheit or `"C"` for Celsius |
| `humidity`      | float   | Relative humidity as a percentage (0–100) |
| `condition`     | string  | Short text description of current weather (e.g., `"Sunny"`, `"Light Rain"`) |
| `wind_speed`    | float   | Wind speed in mph or kph (depending on system defaults) |
| `wind_direction`| string  | Cardinal direction of wind (e.g., `"N"`, `"SW"`) |
| `uv_index`      | integer | UV exposure index `0–11+` |

---

## Example Forecast Weather:
```http
GET /weather/forecast?location=Fayetteville,AR
Host: api.weathercheck.com
Authorization: Bearer <YOUR_API_KEY>
Purpose: Retrieve forecast weather data for a given location.
```

### Parameters
### Raw Parameter Metadata (as provided by engineers)

```
loc | place | strng? | req’d | city/state? zip??  
units | measurment | opt. | metric/imperial (def imperial)  
lang | language | str | maybe? | english default  
days | howmanydays | int? | 7 def, max?? | num of days  
hourly | hrdata | bool-ish? | opt. | include hours?  
```

### Cleaned Parameter Table
| Name     | Label         | Type    | Required | Description |
|----------|---------------|---------|----------|-------------|
| location | Location      | string  | Yes      | City and state (e.g., `"Fayetteville, AR"`). May also support ZIP/postal codes. |
| units    | Units         | string  | No       | Measurement system: `imperial` or `metric`. Defaults to `imperial`. |
| lang     | Language      | string  | No       | Language code for condition descriptions (e.g., `en`, `es`, `fr`). Defaults to `en`. |
| days     | Forecast Days | integer | No       | Number of days to return (default `7`, max `14`). |
| hourly   | Hourly Data   | boolean | No       | If `true`, includes hourly breakdown. Defaults to `false`. |

### Example Request
```http
GET /weather/forecast?location=Fayetteville,AR&days=3&units=imperial&hourly=true
Authorization: Bearer <YOUR_API_KEY>
```

### Example Response
```
{
  "location": {
    "name": "Fayetteville",
    "region": "Arkansas",
    "country": "US"
  },
  "forecast": [
    {
      "date": "2025-08-23",
      "high_temp": 92.3,
      "low_temp": 71.8,
      "unit": "F",
      "condition": "Partly Cloudy",
      "precipitation_chance": 40,
      "humidity": 65,
      "uv_index": 7,
      "hourly": [
        {
          "datetime": "2025-08-23T09:00:00Z",
          "temperature": 82.4,
          "condition": "Cloudy",
          "wind_speed": 6.3,
          "wind_direction": "NE"
        }
      ]
    },
    {
      "date": "2025-08-24",
      "high_temp": 95.4,
      "low_temp": 74.1,
      "unit": "F",
      "condition": "Sunny",
      "precipitation_chance": 10,
      "humidity": 55,
      "uv_index": 8,
      "hourly": []
    }
  ]
}
```

### Field Definitions

#### `location` (object)  
| Name      | Type   | Description |
|-----------|--------|-------------|
| `name`    | string | City or town name (e.g., `"Fayetteville"`) |
| `region`  | string | State, province, or administrative region (e.g., `"Arkansas"`) |
| `country` | string | ISO country code or name (e.g., `"US"`) |

#### `forecast` (array of objects)  
Array of daily forecast entries. Each object may optionally include an `hourly` array.

| Name                   | Type    | Description |
|------------------------|---------|-------------|
| `date`                 | string  | Forecast date in `YYYY-MM-DD` format. Only a date string is returned (no Unix timestamp) |
| `high_temp`            | float   | Expected daily maximum temperature in requested units |
| `low_temp`             | float   | Expected daily minimum temperature in requested units |
| `unit`                 | string  | Temperature unit: `"F"` for Fahrenheit or `"C"` for Celsius |
| `condition`            | string  | Short text description of expected weather (e.g., `"Sunny"`, `"Rain"`) |
| `precipitation_chance` | integer | Probability of precipitation as a percentage (0–100) |
| `humidity`             | float   | Relative humidity as a percentage (0-100) |
| `uv_index`             | integer | UV exposure index (0–11+) |
| `hourly`               | array   | Optional array of hourly forecast objects (see below) |

#### `forecast[].hourly` (array of objects)  
Included when `hourly=true`.

| Name             | Type    | Description |
|------------------|---------|-------------|
| `datetime`       | string  | ISO 8601 timestamp (UTC), e.g., `"2025-08-23T09:00:00Z"`. Note that `"forecast.hourly"` uses `datetime` |
| `temperature`    | float   | Air temperature in requested units |
| `condition`      | string  | Text description of weather at that hour |
| `wind_speed`     | float   | Wind speed in mph or kph (depending on system defaults) |
| `wind_direction` | string  | Cardinal direction of wind (e.g., `"N"`, `"SW"`) |

## Error handling
### Standard Error Format
```json
{
    "error": {
        "code": 400,
        "message": "Invalid parameter: units must be 'metric' or 'imperial'."
    }
}
```

| Field     | Type    | Description |
|-----------|---------|-------------|
| `code`    | integer | HTTP status code |
| `message` | string  | Human-readable error description

### Endpoint-specific error notes
- `/weather/current`
    - Returns `400` if the `location` parameter is missing
- `/weather/forecast`
    - Returns `400` if the `days` parameter exceeds the maximum allowed. (e.g., >`14`)

### Process & Assumptions

- **Normalization**
  - Converted inconsistent types ("strng?", "int?", "bool-ish?") into standard types: `string`, `integer`, `float`, `boolean`.
  - Unified required status values ("req’d", "opt.", "maybe?") into `Yes` / `No`.

- **Assumptions**
  - Default `units=imperial` since most U.S.-based weather APIs default to Fahrenheit/mph.
  - Default `lang=en` since English was implied in raw data.
  - `timestamp` assumed to default to current system time if not provided.
  - For response objects, only Name, Type, and Description are included. Label is implicit, and Required is omitted unless a field is optional.

- **Fixes**
  - Corrected typos (e.g., "measurment" → "Measurement", "strng" → "string").
  - Expanded vague descriptions like “City+state, maybe zip?” into a clearer definition while flagging uncertainty.

- **Unclear Items**
  - Does `loc` accept postal codes or only “City, State”?
  - Should `lang` accept locale codes (e.g., `en-US`) or only ISO 639-1 codes (e.g., `en`)?
  - Can `timestamp` accept future dates for forecast alignment or only historical lookups?
  - What happens if an invalid `units` value is passed? (error vs. silent default)

- **Suggestions for Data Quality**
  - Explicitly include defaults in the source spreadsheet.
  - Mark required parameters with a consistent boolean column instead of free-form notes.
  - Provide a list of allowed values where applicable (`units`, `lang`).
  - Keep spelling consistent to reduce interpretation overhead.

---

### Unclear / Questions for SME (Subject Matter Expert)

- Does `loc` accept ZIP codes or only `"City, State"`?
- Should `lang` accept full local codes (`en-US`, `fr-CA`) or only two-letter ISO codes?
- Can `timestamp` be used for future times, or only historical lookups?
- Should `units` error if invalid values are passed, or silently default?