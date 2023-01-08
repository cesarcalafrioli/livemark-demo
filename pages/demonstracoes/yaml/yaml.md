# YAML - BASICO

# Codigo
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
  url: data/cars.csv
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
