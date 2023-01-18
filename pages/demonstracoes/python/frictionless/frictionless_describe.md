# FRICTIONLESS - DESCRIBE

Descrever os dados, se tratando de frictionless, significa criar metadados para seus arquivos de dados. Por si só, os arquivos de dados não possuem informações adicionais que ajudem a compreender os dados.

Um arquivo no formato CSV sem os metadados carece de informações críticas como significado dos campos, tipos de dados, restrições, relações e etc...

Descrevemos os dados utilizando a função describe()

O trecho abaixo descreve o arquivo "table.csv":

```python script
from frictionless import describe

package = describe("data/table.csv", type="package")
print(package.to_yaml())
```

## Descrevendo um schema

Fornece um "schema" para dados tabulares. Equivale ao schema de banco de dados. A informação do schema inclui:

- Tipos de dados esperados para cada valor em uma coluna ( "String", "number", "date", etc );
- Restrições em cada valor ( Ex: "Determinada string permite até 10 caracteres");
- Formato esperado de dados ( Ex: "Determinado campo deve conter apenas strings e estar em forma de um endereço de email.")

Os schemas de tabelas podem também especificar relações entre tabelas

O trecho abaixo exibe as informações do arquivo "country-1.csv":

```python script
with open('data/country-1.csv') as file:
    print(file.read())
```

O trecho abaixo descreve o schema do arquivo "country-1.csv" e o salva
no arquivo "country.schema.yaml"

```python script
from frictionless import Schema

schema = Schema.describe("data/country-1.csv")
schema.to_yaml("country.schema.yaml")
```

```python script
with open('country.schema.yaml') as file:
    print(file.read())
```


```python script
from frictionless import schema

schema = Schema.describe("data/country-1.csv")
schema.get_field("id").title = "Identifier"
schema.get_field("neighbor_id").title = "Identifier of the neighbor"
schema.get_field("name").title = "Name of the country"
schema.get_field("population").title = "Population"
schema.get_field("population").description = "According to the year 2020's data"
schema.get_field("population").constraints["minimum"] = 0
schema.foreign_keys.append(
    {"fields": ["neighbor_id"], "reference": {"resource": "", "fields": ["id"]}}
)
schema.to_yaml("country.schema.yaml")
```

```python script
with open('country.schema.yaml') as file:
    print(file.read())
```

## Descrevendo um resource

Descreve um resource de dados como um arquivo individual ou tabela de dados. A essencia de um Data Resource é um caminho ao arquivo de dados que ele descreve.

```python script
with open('data/country-2.csv') as file:
    print(file.read())
```

Descrevendo o table resource:

```python script
from frictionless import describe

resource = describe('data/country-2.csv')
print(resource.to_yaml())
```

```python script
from frictionless import Schema, describe

resource = describe("data/country-2.csv")
resource.dialect.delimiter = ";"
resource.layout.header_rows = [2]
resource.schema = Schema("country.schema.yaml")
resource.to_yaml("country-2.resource.yaml")
```

```python script
with open('country-2.resource.yaml') as file:
    print(file.read())
```

## Descrevendo um package

Exibindo as informações do arquivo "country-3.csv":

```python script
with open('data/country-3.csv') as file:
    print(file.read())
```

Exibindo as informações do arquivo "capital-3.csv":

```python script
with open('data/capital-3.csv') as file:
    print(file.read())
```

Descrevendo os metadados de ambos os arquivos acima:

```python script
from pprint import pprint
from frictionless import describe

package = describe("data/*-3.csv")

print(package.to_yaml())
```

Adicionando informações adicionais:
```python script
from frictionless import describe

package = describe("data/*-3.csv")
package.title = "Countries and their capitals"
package.description = "The data was collected as a research project"
package.get_resource("country-3").name = "country"
package.get_resource("capital-3").name = "capital"
package.get_resource("country").schema.foreign_keys.append(
    {"fields": ["capital_id"], "reference": {"resource": "capital", "fields": ["id"]}}
)
package.to_yaml("data/country-3.package.yaml")
```

Exibindo as informações do pacote de dados:
```python script
with open('data/country-3.package.yaml') as file:
    print(file.read())
```

## Importancia dos metadados

```python script
with open('data/country-2.csv') as file:
    print(file.read())
```

```python script
from frictionless import extract
from tabulate import tabulate

resource = extract("data/country-2.csv")
print(tabulate(resource, headers="keys", tablefmt="rst"))
```

