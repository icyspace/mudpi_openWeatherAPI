## mudpi_openWeatherAPI

## Functionality 
### Get Forecast
- Rain (min, max, avearge) over x hours
- Temp (high, Low) over next x hours

### Get Historical Data (did it rain in the last hour) or Rainfall in the last week
- Rain in the last hour (x hours)
- Total Rain fall in the last x hours
- historical weather

### Get Current weather
- Is it currently rainning
- Current List of any Oweather element

## Other
- imperial vs metric For temperature in Fahrenheit and wind speed in miles/hour, use units=imperial For temperature in Celsius and wind speed in meter/sec, use units=metric Temperature in Kelvin and wind speed in meter/sec is used by default, so there is no need to use the units parameter in the API call if you want this

<br>

## Precipitation Setting


| Option            | Type        |  Required   | Description |
| -----------       | ----------- | ----------- | ----------- |
| **type**              | [String]      |Yes        |  Type of sensor:  Precipitation |
| **pin**               | [Integer]	    |Yes        |  GPIO pin number on raspberry pi the sensor is connected to. In this case use 0 as we are not using any pin this but for sensor this is a required field.           |
| **key**               | [String]	    |Yes        |Key to store value under in redis. Alphanumeric with underscores only. Must be valid redis key.  |
| **openweather_api_key**| String]      |Yes        |[Get OpenWeather API](https://openweathermap.org/appid)|
| **latlong**           | String]       |No         | Defaults to ISP location when not provided.  Provide latitude and longitude in this formate with no spaces:  "lat,long"|
| **forecasted_hours**  | [Integer]	    |No         |  integer between 1 and 24 (defaults to 24 hours) This represents how many hours in the future to look for precipitations |

<br>

---

<br><br>

## Example mudpi.config  

```json
{
"mudpi": {
    "name": "MudPi Example Sensors",
    "debug": true,
    "unit_system": "imperial",
    "latitude": 45.526,
    "longitude": -90.043
},
"owmapi": [
    {
    "key": "mudpi_owmapi",
    "api_key": "your_api_key",
    "latitude": 20.526,
    "longitude": -100.043
    }
],
"sensor": [
    {
        "interface": "owmapi",
        "key": "current_weather",
        "connection": "mudpi_owmapi",
        "type":"current",
        "measurements": ["sunrise","sunset","temp", "humidity", "rain", "israining"],
        "update_interval": 300
    },
    {
        "interface": "owmapi",
        "key": "forecast_weather",
        "connection": "mudpi_owmapi",
        "type":"forecast",
        "measurements": ["maxpop", "sumrain", "maxtemp", "mintemp", "avgtemp"],
        "hours" : 8,
        "update_interval": 300
    },
    {
        "interface": "owmapi",
        "key": "historical_weather",
        "connection": "mudpi_owmapi",
        "type":"historical",
        "measurements": ["sumrain", "maxtemp", "mintemp", "avgtemp"],
        "hours" : 24,
        "update_interval": 300
    }
]}
```
