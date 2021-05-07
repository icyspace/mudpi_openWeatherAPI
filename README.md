# mudpi_openWeatherAPI

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
