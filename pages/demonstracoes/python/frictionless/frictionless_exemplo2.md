# FRICTIONLESS - EXEMPLO 2

Transformando dados com os metadados gerados pelo arquivo countries.csv

Lendo o arquivo csv no formato original

```python script
with open('data/countries.csv') as paises:
    print(paises.read())
```

### Criando os metadados

```python script
from pprint import pprint
from frictionless import describe, Detector

#resource = describe('data/countries.csv')
#pprint(resource)

# Insere novos metadados no schema e corrige os dados
detector = Detector(field_missing_values=["","n/a"]) # Informa quais os valores que estão vazios
resource = describe('data/countries.csv') #Aplica substituição do arquivo countries.csv
resource.schema.get_field("neighbor_id").type = "integer" # Modifica tipo de dado da coluna neighbor_id para ser do tipo inteiro
resource.schema.get_field("population").type = "integer" # Modifica tipo de dado da coluna population para ser do tipo inteiro
resource.schema.foreign_keys.append(
    {"fields": ["neighbor_id"], "reference": {"resource": "", "fields": ["id"]}}
)
resource.to_yaml("countries.resource.yaml")
resource.to_json("countries.resource.json")
```

Exibindo os metadados em yaml

```python script
from pprint import pprint
with open('countries.resource.yaml') as paises_yaml:
    pprint(paises_yaml.read())
```

Exibindo os metadado em JSON

```python script
from pprint import pprint
with open('countries.resource.json') as paises_json:
    pprint(paises_json.read())
```

### Extraindo os dados

```python script
from pprint import pprint
from frictionless import extract

rows = extract('data/countries.csv')
pprint(rows)
```

Exibindo os metadados

```python script
from pprint import pprint
from frictionless import extract

rows = extract('countries.resource.yaml')
pprint(rows)
```

### Validando os dados

Validando os dados diretamente do arquivo CSV

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/countries.csv')
pprint(report.flatten(["rowPosition", "fieldPosition", "code"]))
```

validando os metadados do arquivo csv

```python script
from pprint import pprint
from frictionless import validate

report = validate('countries.resource.yaml')
pprint(report.flatten(["rowPosition", "fieldPosition", "code"]))
```

### Transformando os dados

Fixando o metadados que será utilizado para fixar os problemas dos tipos de dados automaticamente

Antes da transformacao

```python script
with open('data/countries.csv') as file:
    print(file.read())
```

Transformando os dados

```python script
from frictionless import Resource, describe, transform, steps

def clean(resource):
    current = resource.to_copy()

    # Data
    def data():
        with current:
            for row in current.row_stream:
                if row["name"] == "France":
                    row["population"] = 67
                if row["name"] == "Germany":
                    row["neighbor_id"] = 2
                if row["name"]:
                    yield row

    # Meta
    resource.schema = Resource("countries.resource.yaml").schema
    resource.data = data


source = describe("data/countries.csv")
target = transform(
    source,
    steps=[
        clean,
        steps.table_write(path="data/countries_transformed.csv"),
    ],
)
```

Depois da transformacao

```python script
with open('data/countries_transformed.csv') as file:
    print(file.read())
```

```python script
import os

files = [f for f in os.listdir('data/') if os.path.isfile(f) and f.startwith('countries.')]
print(files)
```

