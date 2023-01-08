# JSON - BASICO

```json chart
{
  "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
  "description": "A simple pie chart with embedded data.",
  "data": {
    "values": [
      {"fabricantes": 1, "value": 4},
      {"fabricantes": 2, "value": 6},
      {"fabricantes": 3, "value": 10},
      {"fabricantes": 4, "value": 3},
      {"fabricantes": 5, "value": 7},
      {"fabricantes": 6, "value": 8}
    ]
  },
  "mark": "arc",
  "encoding": {
    "theta": {"field": "value", "type": "quantitative"},
    "color": {"field": "fabricantes", "type": "nominal"}
  }
}     
```