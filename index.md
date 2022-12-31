# Cesar Calafrioli



## Data

```yaml script 
from frictionless import Resource

resource = Resource('https://raw.githubusercontent.com/frictionlessdata/livemark/main/data/cars.csv')

resource.write('data/cars.csv')
```

## Table

```yaml table
data: data/cars.csv
filters: true
dropdownMenu: true
columnSorting:
    initialConfig:
        column: 2
        sortOrder: desc
width: 600
```

## Chart

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

## Data in Jinja format

```
{% for car in frictionless.extract('data/cars.csv')[:5] %}
- {{ car.brand }} {{ car.model }}: ${{ car.price }}
{% endfor %}
```

```html markup
<div class="livemark-pagination">
  {% for number in range(1, 1001) %}
  <div>{{ number }}</div>
  {% endfor %}
</div>
```

```markdown markup columns
![Package](../../assets/data-package.png)
**Data Package**

A simple container format for describing a coherent collection of data in a single package.
```

```markdown markup columns
![Resource](../../assets/data-resource.png)
**Data Resource**

A simple format to describe and package a single data resource such as a individual table or file.
```

```markdown markup columns
![Schema](../../assets/table-schema.png)
**Data Resource**

A simple format to describe and package a single data resource such as a individual table or file.
```

## Logic

Usando Jinja

{% for car in frictionless.extract('data/cars.csv')[:5] %}
- {{ car.brand }} {{ car.model }}: ${{ car.price }}
{% endfor %}

## Conclusion

Visite meu  <a href="https://github.com/cesarcalafrioli"> repositório </a>