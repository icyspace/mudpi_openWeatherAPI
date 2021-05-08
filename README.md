## mudpi_openWeatherAPI

## Functionality 
### Get Forecast weather
- Rain (sum, min, max, avearge) over x hours
- Temp (min, max, average) over next x hours
- Chance of precipitation over the next x hours

### Get Historical Data (did it rain in the last hour) or Rainfall in the 24hours
- Rain in the last hour (x hours)
- Total Rain fall in the last x hours
- Min Max temp over the last 24 hours

### Get Current weather
- Is it currently rainning (custom measurement) 
- Current List of any open weather api measurement (temp, humiditiy, sunset time, sunrise time) 

### Other
- imperial vs metric For temperature in Fahrenheit and wind speed in miles/hour, use units=imperial For temperature in Celsius and wind speed in meter/sec, use units=metric Temperature in Kelvin and wind speed in meter/sec is used by default, so there is no need to use the units parameter in the API call if you want this

<br>

## Weather Types and Measurments 

Current will return the current measurements, updated intra hour by open weather map.  See documenation below.  

Forecast and Historical take a look back window (in hours) and aggregate the hourly measurement information.  You can return a Sum, Min, Max, and avearge for any of the listed measurements below.  Historical/Forecast allows for up to 24 hours. (e.g. "mintemp", "maxpop", "avgrain")

| Current            | Forecast     | Historical   |
| -----------       | ----------- | ----------- |
| | sum / min / max / avg | sum / min / max / avg |
| temp | temp | temp |
| feels_like | feels_like | feels_like |
| pressure | pressure | pressure |
| humidity | humidity | humidity |
| dew_point | dew_point | dew_point |
| clouds | clouds | clouds |
| visibility | visibility | visibility |
| wind_speed | wind_speed | wind_speed |
| wind_deg | wind_deg | wind_deg |
| rain | rain | rain |
| uvi | uvi | |
| sunrise | | | 
| sunset | | |
| israinning (custom measurement) | | |
| | pop (Probability of precipitation) | |
| | wind_gust | |

[OpenWeather API Docs](https://openweathermap.org/api/one-call-api)

## OWMAPI Setting

### Extension Options
| Option            | Type        |  Required   | Description |
| -----------       | ----------- | ----------- | ----------- |
| **key**               | [String]	    |Yes        | Key to store value under in redis. Alphanumeric with underscores only. Must be valid redis key.  |
| **api_key**           | [String]      |Yes        | [Get OpenWeather API](https://openweathermap.org/appid)|
| **unit_system**       | [Integer]	    |No         | "imperial", "metric", "standard" Default is imperial 
| **latitude**          | [String]      |No         | Defaults to ISP location when not provided.  Provide latitude and longitude in this formate with no spaces:  "lat,long"|
| **longitude**         | [String]      |No         | Defaults to ISP location when not provided.  Provide latitude and longitude in this formate with no spaces:  "lat,long"|

### Sensor Options 
| Option            | Type        |  Required   | Description |
| -----------       | ----------- | ----------- | ----------- |
| **type**              | [String]      |Yes        |  Type of weather to retrieve (select one per sensor):  "current", "forecast", "historical" |
| **measurements**      | [String]	    |Yes        | See measurements avalible for each type.  must be provided in list.  ["sumrain","sumpop",...]
| **hours**             | [Integer]	    |No        |  integer between 1 and 24 (defaults to 24 hours) This represents how many hours in the future to look for precipitations |

<br>

---

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
