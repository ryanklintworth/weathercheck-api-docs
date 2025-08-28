# WeatherCheck API Documentation

## Overview
> WeatherCheck API provides real-time weather information and forecasts, allowing developers to retrieve current conditions and integrate forecast data into their applications.

## Getting Started

### Base URL
- `https://api.weathercheck.com/v1`

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
| lang      | Language | string  | No       | Language code for condition descriptions. Accepts two-letter ISO 639-1 codes (e.g., `en`, `es`, `fr`). Defaults to `en`. Locale variants (e.g., `fr-CA`) are not supported. |
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
| Name               | Type   | Description |
|--------------------|--------|-------------|
| `location.name`    | string | City or town name (e.g., `"Fayetteville"`) |
| `location.region`  | string | State, province, or administrative region (e.g., `"Arkansas"`) |
| `location.country` | string | ISO country code or name (e.g., `"US"`) |

---

#### `timestamp` (integer)  
Unix epoch time (seconds since `1970-01-01T00:00:00Z`). Useful for programmatic time comparisons.  

#### `datetime` (string)  
Human-readable timestamp in ISO 8601 format (UTC), e.g., `"2025-08-21T17:00:00Z"`.  

---

#### `current` (object)  
| Name                    | Type    | Description |
|-------------------------|---------|-------------|
| `current.temperature`   | float   | Air temperature in the requested units |
| `current.feels_like`    | float   | Perceived temperature (“heat index” or “wind chill”) in the requested units |
| `current.unit`          | string  | Temperature unit: `"F"` for Fahrenheit or `"C"` for Celsius |
| `current.humidity`      | float   | Relative humidity as a percentage (0–100) |
| `current.condition`     | string  | Short text description of current weather (e.g., `"Sunny"`, `"Light Rain"`) |
| `current.wind_speed`    | float   | Wind speed in mph or kph |
| `current.wind_direction`| string  | Cardinal direction of wind (e.g., `"N"`, `"SW"`) |
| `current.uv_index`      | integer | UV exposure index `0–11+` |

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
| lang     | Language      | string  | No       | Language code for condition descriptions. Accepts two-letter ISO 639-1 codes (e.g., `en`, `es`, `fr`). Defaults to `en`. Locale variants (e.g., `fr-CA`) are not supported. |
| days     | Forecast Days | integer | No       | Number of days to return. Default `7`, max `14`. Values above `14` return an error.`. |
| hourly   | Hourly Data   | boolean | No       | If `true`, includes hourly breakdown. Defaults to `false`. |

### Example Request
```http
GET /weather/forecast?location=Fayetteville,AR&days=2&units=imperial&hourly=true
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
      "hourly": [
        {
          "datetime": "2025-08-24T09:00:00Z",
          "temperature": 81.1,
          "condition": "Sunny",
          "wind_speed": 2.3,
          "wind_direction": "NE"
        }
      ]
    }
  ]
}
```

### Field Definitions

#### `location` (object)  
| Name               | Type   | Description |
|--------------------|--------|-------------|
| `location.name`    | string | City or town name (e.g., `"Fayetteville"`) |
| `location.region`  | string | State, province, or administrative region (e.g., `"Arkansas"`) |
| `location.country` | string | ISO country code or name (e.g., `"US"`) |

#### `forecast` (array of objects)  
Array of daily forecast entries. Each object may optionally include an `hourly` array.

| Name                            | Type    | Description |
|---------------------------------|---------|-------------|
| `forecast.date`                 | string  | Forecast date in `YYYY-MM-DD` format. |
| `forecast.high_temp`            | float   | Expected daily maximum temperature in requested units |
| `forecast.low_temp`             | float   | Expected daily minimum temperature in requested units |
| `forecast.unit`                 | string  | Temperature unit: `"F"` for Fahrenheit or `"C"` for Celsius |
| `forecast.condition`            | string  | Short text description of expected weather (e.g., `"Sunny"`, `"Rain"`) |
| `forecast.precipitation_chance` | integer | Probability of precipitation as a percentage (0–100) |
| `forecast.humidity`             | float   | Relative humidity as a percentage (0-100) |
| `forecast.uv_index`             | integer | UV exposure index (0–11+) |
| `forecast.hourly`               | array   | Optional array of hourly forecast objects (see below) |

#### `forecast[].hourly` (array of objects)  
Included when `hourly=true`.

| Name                             | Type    | Description |
|----------------------------------|---------|-------------|
| `forecast.hourly.datetime`       | string  | ISO 8601 timestamp (UTC), e.g., `"2025-08-23T09:00:00Z"`. |
| `forecast.hourly.temperature`    | float   | Air temperature in requested units |
| `forecast.hourly.condition`      | string  | Text description of weather at that hour |
| `forecast.hourly.wind_speed`     | float   | Wind speed in mph or kph (depending on system defaults) |
| `forecast.hourly.wind_direction` | string  | Cardinal direction of wind (e.g., `"N"`, `"SW"`) |

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
| `message` | string  | Human-readable error description |

### Common Error Codes
| Code  | Meaning               | When it Happens                                                 |
|-------|-----------------------|-----------------------------------------------------------------|
| `400` | Bad Request           | Invalid parameter (e.g., `units=stone` or `days > 14`)          |
| `401` | Unauthorized          | Missing or invalid API key                                      |
| `403` | Forbidden             | API key valid, but not allowed to access the requested endpoint |
| `404` | Not Found             | Location not found, or endpoint does not exist                  |
| `429` | Too Many Requests     | Rate limit exceeded                                             |
| `500` | Internal Server Error | Something went wrong on WeatherCheck’s side                     |


### Endpoint-specific error notes
- `/weather/current`
    - Returns `400` if the `location` parameter is missing.
    - Returns `404` if the requested location is not found.
- `/weather/forecast`
    - Returns `400` if the `days` parameter exceeds the maximum allowed. (e.g., >`14`).
    - Returns `404` if the requested location is not found.

## Rate Limiting
| Limit Type          | Value    | Description                             |
| ------------------- | -------- | --------------------------------------- |
| Requests per minute | `60`     | Max 60 requests per API key per minute  |
| Requests per day    | `10,000` | Max 10,000 requests per API key per day |

If you exceed the rate limit, the API reponds with:
```json
{
  "error": {
    "code": 429,
    "message": "Rate limit exceeded. Please wait and try again."
  }
}
```

## Process & Assumptions

- **Normalization**
  - Converted inconsistent types ("strng?", "int?", "bool-ish?") into standard types: `string`, `integer`, `float`, `boolean`.
  - Unified required status values ("req’d", "opt.", "maybe?") into `Yes` / `No`.

- **Assumptions**
  - Default `units=imperial` since most U.S.-based weather APIs default to Fahrenheit/mph.
  - Default `lang=en` since English was implied in raw data.
  - `timestamp` assumed to default to current system time if not provided.

- **Fixes**
  - Corrected typos (e.g., "measurment" → "Measurement", "strng" → "string").
  - Expanded vague descriptions like “City+state, maybe zip?” into a clearer definition while flagging uncertainty.

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

## Bonus: Best Practices & Recommendations
### Best Practices for Developers
- Cache responses: Avoid hitting rate limits and improve performance by caching current weather data for at least 60 seconds.
- Use forecast endpoints for planning: Do not call `/current` repeatedly to simulate forecasts. Use `/forecast` instead.
- Gracefully handle errors: Always check the `error.code` field and build retries with exponential backoff if appropriate.

### Recommendations for API Evolution
- OpenAPI/Swagger schema: Publish a machine-readable schema (`openapi.yaml`) to auto-generate SDKs, client stubs, and test coverage.
- Standardized metadata: Add fields like `units` and `lang` to the response root for consistency.
- Localization: Consider support for full locale codes (e.g., `en-US`, `fr-CA`) instead of just `ISO 639-1`.
Webhooks (future): Push severe weather alerts via webhook instead of polling.
Rate limiting transparency: Expose both per-minute and per-day quotas in headers.

### Bonus Hit
- Modeled after industry standards like `https://openweathermap.org/current` and `https://developer.apple.com/documentation/weatherkitrestapi`