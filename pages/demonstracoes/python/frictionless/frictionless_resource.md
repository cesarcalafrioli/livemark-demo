# Frictionless - Resource

```python script
""" from frictionless import Resource

resource = Resource('table.csv') # from a resource path
resource = Resource('resource.json') # from a descriptor path
resource = Resource({'path': 'table.csv'}) # from a descriptor
resource = Resource(path='table.csv') # from arguments """
```

## Ciclo de vida do resource

```python script
from pprint import pprint
from frictionless import Resource

resource = Resource('data/capital-3.csv')
resource.open()
pprint(resource.read_rows())
pprint(resource.read_rows())
# We need to re-open: there is no data left
resource.open()
pprint(resource.read_rows())
# We need to close manually: not context manager is used
resource.close()
```

```python script
from frictionless import Resource

resource = Resource('data/capital-3.csv')
pprint(resource.read_rows())
```

## Lendo dados

```python script
from frictionless import Resource

resource = Resource('data/country-3.csv')
pprint(resource.read_bytes())
pprint(resource.read_text())
pprint(resource.read_cells())
pprint(resource.read_rows())
```

```python script
from frictionless import Resource

with Resource('data/country-3.csv') as resource:
    pprint(resource.byte_stream)
    pprint(resource.text_stream)
    pprint(resource.cell_stream)
    pprint(resource.row_stream)
    for row in resource.row_stream:
      print(row)
```

## Encoding

```python script
from frictionless import Resource

with Resource('data/country-3.csv', encoding='utf-8') as resource:
  print(resource.encoding)
  print(resource.path)

```

```python script

```

```python script

```

```python script

```

```python script

```

```python script

```

```python script

```