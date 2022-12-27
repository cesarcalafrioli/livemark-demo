# Demonstrações de código

Para essa demonstração foi utilizado o conjunto de dados fornecido pela Agência Nacional de Aviação Civil ( ANAC ) referente a produtos aeronáuticos certificados no Brasil.

## Tabelas 

```yaml table
data: data/produtos-aeronauticos-fabricantes.csv
width: 100%
#colocar essa informação após #tratar os dados
#columns:
#    - data: <nome da coluna1>
#    - data: <nome da coluna2>
#    ...
#'''
order:
    -   [5, 'desc'] # 6a coluna
filters: true
dropdownmenu: true
```

## Graficos

```yaml chart
data:
  url: ../data/cars.csv
mark: circle
selection:
  brush:
    type: interval
encoding:
  x:
    type: quantitative
    field: kmpl
    scale:
     domain: [12,25]
  y:
    type: quantitative
    field: price
    scale:
     domain: [100,900]
  color:
    condition:
      selection: brush
      field: type
      type: nominal
    value: grey
  size:
    type: quantitative
    field: bhp
width: 500
height: 300
```

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

## Execucao de scripts

```python script
for number in range(1, 6):
    print(f'Hello World #{number}!')
```