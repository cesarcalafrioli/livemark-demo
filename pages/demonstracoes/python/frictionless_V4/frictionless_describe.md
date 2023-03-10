# FRICTIONLESS - DESCRIBE

Descrever os dados, se tratando de frictionless, significa criar metadados para seus arquivos de dados. Por si so, os arquivos de dados nao possuem informacoes adicionais que ajudem a compreender os dados.

Um arquivo no formato CSV sem os metadados carece de informacoes criticas como significado dos campos, tipos de dados, restricoes, relacoes e etc...

Existem 3 tipos de metadados no frictionles:
-   Resource
-   Schema
-   Package

Descrevemos os dados utilizando a funcao describe()


```python script
# Descrevendo o arquivo "table.csv"
from frictionless import describe

package = describe("data/table.csv", type="package")
print(package.to_yaml())
```

## Descrevendo um schema

Fornece um "schema" para dados tabulares. Equivale ao schema de banco de dados. A informacao do schema inclui:

- Tipos de dados esperados para cada valor em uma coluna ( "String", "number", "date", etc );
- Restricoes em cada valor ( Ex: "Determinada string permite ate 10 caracteres");
- Formato esperado de dados ( Ex: "Determinado campo deve conter apenas strings e estar em forma de um endereco de email.")

Os schemas de tabelas podem tambem especificar relacoes entre tabelas

```python script
# Exibe as informacoes do arquivo "country-1.csv"
with open('data/country-1.csv') as file:
    print(file.read())
```

```python script
# Descreve o schema do rquivo "country-1.csv" e o salva no arquivo "country-1.schema.yaml"
from frictionless import Schema

schema = Schema.describe("data/country-1.csv")
schema.to_yaml("data/country-1.schema.yaml")
```

```python script
# Exibindo as informacoes do arquivo schema
with open('data/country-1.schema.yaml') as file:
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
schema.to_yaml("data/country-1.schema.yaml")
```

```python script
with open('data/country-1.schema.yaml') as file:
    print(file.read())
```

## Descrevendo um resource

Descreve um resource de dados como um arquivo individual ou tabela de dados. A essencia de um Data Resource ?? um caminho ao arquivo de dados que ele descreve.

O resource prove um conjunto de propriedades mais rico, al??m de incluir o proprio schema.

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
resource.path = "country-2.csv" # Atribuindo o caminho correto do arquivo
resource.dialect.delimiter = ";" # Atribuindo o delimitador para ponto-e-virgula ( ; )
resource.layout.header_rows = [2] # A coluna come??a na linha 2
resource.schema = Schema("data/country-1.schema.yaml") # Reutilizando o schema criado anteriormente pois os dados possuem a mesma estrutura e significado
resource.to_yaml("data/country-2.resource.yaml")
```

```python script
with open('data/country-2.resource.yaml') as file:
    print(file.read())
```

## Descrevendo um package

Exibindo as informa????es do arquivo "country-3.csv":

```python script
with open('data/country-3.csv') as file:
    print(file.read())
```

Exibindo as informa????es do arquivo "capital-3.csv":

```python script
with open('data/capital-3.csv') as file:
    print(file.read())
```

Descrevendo o pacote contendo os metadados de ambos os arquivos:

```python script
from pprint import pprint
from frictionless import describe

package = describe("data/*-3.csv")

print(package.to_yaml())
```

Adicionando informa????es adicionais:
```python script
from frictionless import describe

package = describe("data/*-3.csv")
package.title = "Countries and their capitals"
package.description = "The data was collected as a research project"
package.get_resource("country-3").path = "country-3.csv"
package.get_resource("country-3").name = "country"
package.get_resource("capital-3").path = "capital-3.csv" # ?? necess??rio colocar na ordem para ser reconhecido ( ANOTA????O )
package.get_resource("capital-3").name = "capital"
package.get_resource("country").schema.foreign_keys.append(
    {"fields": ["capital_id"], "reference": {"resource": "capital", "fields": ["id"]}}
)
package.to_yaml("data/country-3.package.yaml")
```

Exibindo as informa????es do pacote de dados:
```python script
with open('data/country-3.package.yaml') as file:
    print(file.read())
```

## Importancia dos metadados

Exibindo dados do arquivo "country-2.csv":

```python script
with open('data/country-2.csv') as file:
    print(file.read())
```

Descrevendo os metadados do arquivo "country-2.csv"

```python script
from frictionless import describe

resource = describe("data/country-2.csv")
print(resource.to_yaml())
```

```python script
from frictionless import extract
from tabulate import tabulate

resource = extract("data/country-2.csv")
print(tabulate(resource, headers="keys", tablefmt="rst"))
```

```python script
from frictionless import extract
from tabulate import tabulate

data = extract("data/country-2.resource.yaml")
print(tabulate(data, headers="keys", tablefmt="rst"))
```

```python script
""" from frictionless import extract, Resource
from tabulate import tabulate

resource = Resource("data/country-3.package.yaml")
resource.path = "country-3.csv"
resource.to_yaml("data/country-3.package.yaml")

pprint(resource.extract()) """
```

## Inferindo metadados

```python script
from pprint import pprint
from frictionless import Resource

resource = Resource("data/country-1.csv")
pprint(resource)
```

```python script
from pprint import pprint
from frictionless import Resource

resource = Resource("data/country-1.csv")
resource.infer(stats=True)
pprint(resource)
```

## Expandindo os metadados

```python script
from pprint import pprint
from frictionless import describe

resource = describe("data/country-1.csv")
pprint(resource.schema)
```

Revelando metadados impl??citos com a fun????o expand()

```python script
from pprint import pprint
from frictionless import describe

resource = describe("data/country-1.csv")
resource.schema.expand()
pprint(resource.schema)
```

## Transformando metadados

```python script
# Transformando as propriedades dos metadados
from frictionless import Resource

resource = Resource("data/country-2.resource.yaml")
resource.title = "Countries"
resource.description = "It's a research project"
#resource.path = "country-2.csv"
resource.dialect.delimiter = ";"
resource.layout.header_rows = [2]
resource.to_yaml("data/country-2-transformed.resource.yaml")
```

```python script
# Adicionando arbitrariamente metadados ao descriptor
from frictionless import Resource

resource = Resource("data/country-2.resource.yaml")
resource["customKey1"] = "Value1"
resource["customKey2"] = "Value2"
resource.to_yaml("data/country-2-transformed.resource.yaml")
```

```python script
with open('data/country-2-transformed.resource.yaml') as file:
    print(file.read())
```

## Validating metadados

Antes de publicar os metadados ?? recomendado valid??-los

```python script
# Tornando um metadado inv??lido
from frictionless import Resource

resource = Resource("data/country-2.resource.yaml")
resource["title"] = 1
print(resource.metadata_valid)
print(resource.metadata_errors)
```

A fun????o metadata_valid s?? precisa ser utilizada quando o metadados ?? alterado manualmente. Certas fun????es do frictionless validam os metadados por si s??.

```python script
# Fixando o metadado para torn??-lo v??lido
from frictionless import Resource

resource = Resource("data/country-2.resource.yaml")
resource["title"] = 'Countries'
print(resource.metadata_valid)
```