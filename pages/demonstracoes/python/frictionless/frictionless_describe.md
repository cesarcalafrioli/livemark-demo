# FRICITONLESS - DESCRIBE

Descrever os dados, se tratando de frictionless, significa criar metadados para seus arquivos de dados. Por si so, os arquivos de dados nao possuem informacoes adicionais que ajudem a compreender os dados.

Um arquivo no formato CSV sem os metadados carece de informacoes criticas como significado dos campos, tipos de dados, restricoes, relacoes e etc...

Existem 3 tipos de metadados no frictionles:
-   Resource
-   Schema
-   Package

Descrevemos os dados utilizando a funcao describe()

## Descrevendo os dados

```python script
from frictionless import describe

package = describe("data/table.csv", type="package")
print(package.to_yaml())
```

## Descrevendo um Schema

Informações do arquivo csv

```python script
with open('data/country-1.csv') as file:
    print(file.read())
```

Descrevendo o schema do arquivo country-1.csv

```python script
from frictionless import Schema

schema = Schema.describe("data/country-1.csv")
schema.to_yaml("data/country-1.schema.yaml")
```

```python script
with open('data/country-1.schema.yaml') as file:
    print(file.read())
```

```python script
from frictionless import Schema

schema = Schema.describe("data/country-1.csv")
schema.get_field("id").title = "Identifier"
schema.get_field("neighbor_id").title = "Identifier of the neighbor"
schema.get_field("name").title = "Name of the country"
schema.get_field("population").title = "Population"
schema.get_field("population").description = "According to the year 2020's data" # Descrição da coluna population
schema.get_field("population").constraints["minimum"] = 0
schema.foreign_keys.append(
    {"fields": ["neighbor_id"], "reference": {"resource": "", "fields": ["id"]}}
)
schema.to_yaml("data/country-1.schema-full.yaml")
```

```python script
with open('data/country-1.schema-full.yaml') as file:
    print(file.read())
```

## Descrevendo um recurso

Visualizando arquivo csv

```python script
with open('data/country-2.csv') as file:
    print(file.read())
```

Descrevendo o arquivo

```python script
from frictionless import describe

resource = describe('data/country-2.csv')
print(resource.to_yaml())
```

```python script
""" from frictionless import Schema, describe

resource = describe("data/country-2.csv")
resource.dialect.header_rows = [2]
resource.dialect.get_control('csv').delimiter = ";"
#resource.schema = "data/country-1.schema-full.yaml"
resource.path = "country-2.csv"
resource.to_yaml("data/country-2.resource-cleaned.yaml") """
```

```python script
with open('data/country-2.resource-cleaned.yaml') as file:
    print(file.read())
```


## Descrevendo um Pacote


O pacote de dados consiste de metadados descrevendo a estrutura e conteúdos do pacote, e recursos como arquivos de dados dos conteúdos do pacote. Engloba todos os conjuntos de dados, que pode ter mais de um arquivo.

Visualizando o arquivo country-3.csv e capital-3.csv

```python script
with open('data/country-3.csv') as file:
    print(file.read())
```

```python script
with open('data/capital-3.csv') as file:
    print(file.read())
```

Descrevendo o metadados do conjunto de dados com os arquivos countries-3.csv e capital-3.csv

```python script
from frictionless import describe

package = describe("data/*-3.csv")
print(package.to_yaml())
```

Adicionando metadados adicionais ( Refinamento )

```python script
from frictionless import describe

package = describe("data/*-3.csv") # Criando os metadados dos arquivos csv capital-3 e country-3
package.title = "Countries and their capitals" # Adicionando título ao descriptor
package.description = "The data was collected as a research project" # Adicionando descrição do package ao descriptor
package.get_resource("country-3").path = "capital-3.csv" # Alterando o caminho do arquivo capital-3.csv
package.get_resource("country-3").name = "country" # Alterando o nome do resource no descriptor


package.get_resource("capital-3").path = "capital-3.csv" # Alterando o caminho do arquivo capital-3.csv
package.get_resource("capital-3").name = "capital"
 # Alterando o nome do resource no descriptor
package.get_resource("country").schema.foreign_keys.append(
    {"fields": ["capital_id"], "reference": {"resource": "capital", "fields": ["id"]}}
) # Adicionando uma relação entre os dois arquivos capital-3 e country-3 por meio das respectivas chaves id e capital_id

# Insere metadados do schema do recurso "country"
package.get_resource("country").schema.get_field("id").title = "Identifier"
package.get_resource("country").schema.get_field("name").title = "Name of the country"
package.get_resource("country").schema.get_field("population").title = "Population"
package.get_resource("country").schema.get_field("population").description = "According to the year 2020's data" # DescriÃ§Ã£o da coluna population
schema.get_field("population").constraints["minimum"] = 0

package.to_yaml("data/country-3.package.yaml") # Salvando os metadados adicionais ao arquivo descriptor
```

Visualizando o descriptor do pacote

```python script
with open('data/country-3.package.yaml') as file:
    print(file.read())
```

## A importância dos metadados

A ausência dos metadados torna a leitura dos arquivos de dados mais difícil. O metadados contém os tipos de dados já corrigidos, permitindo que possamos exportar e compartilhar os dados sem qualquer preocupação.

## Inferindo metadados

Criando o resource do arquivo country-1.csv

```python script
from frictionless import Resource

resource = Resource("data/country-1.csv")
print(resource)
```

Inferindo metadados adicionais

```python script
from pprint import pprint
from frictionless import Resource

resource = Resource("data/country-1.csv")
resource.infer(stats=True)
pprint(resource)
```

```python script
from frictionless import describe

package = describe("data/country-1.csv")
print(package.to_yaml())
```

## validando os metadados

O processo de validação identifica problemas encontrados nos dados tabulares e que devem ser corrigidos.

O trecho abaixo provoca um erro no descritor do arquivo countr-1.csv para que este seja detectado e mostrado sua causa.

```python script
import yaml
from frictionless import Resource

descriptor = {}
descriptor['path'] = 'data/country-1.csv'
descriptor['title'] = 1
try:
    Resource(descriptor)
except Exception as exception:
    print(exception.error)
    print(exception.reasons)
```

Tentando criar um metadados da versão inválida do arquivo country-1.csv

```python script
import yaml
from frictionless import Resource

descriptor = {}
descriptor['path'] = 'data/country-1-invalid.csv'

try:
    Resource(descriptor)
except Exception as exception:
    print(exception.error)
    print(exception.reasons)
```

Validando a versão inválida do arquivo country-1

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/country-1-invalid.csv')
print(report)
```

Validando o arquivo csv

```python script
from pprint import pprint
from frictionless import validate

#report = validate('data/country-1.csv')
#report = validate('data/countries.csv')
report = validate('data/country-1-invalid.csv')
pprint(report.flatten(["rowNumber", "fieldNumber", "type"]))
```

Criando o metadados da versão inválida do arquivo country-1


```python script
from frictionless import Detector, describe

detector = Detector(field_missing_values=["", "n/a"])
resource = describe("data/country-1-invalid.csv", detector=detector)
resource.schema.get_field("neighbor_id").type = "integer"
resource.schema.foreign_keys.append(
    {"fields": ["neighbor_id"], "reference": {"resource": "", "fields": ["id"]}}
)
resource.path = "country-1-invalid.csv"
resource.to_yaml("data/country-1-invalid.resource.yaml")
```

Validando o resource

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/country-1-invalid.resource.yaml')
#pprint(report)
pprint(report.flatten(["rowNumber", "fieldNumber", "type"]))
```

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/country-2.resource.yaml')
pprint(report.flatten(["rowNumber", "fieldNumber", "type"]))
```


## Transformando os metadados

No frictionless, transformar os dados significa modificar dados e metadados de um estado para outr, ou seja, podemos transformar um arquivo excel bagunçado para um csv limpo, ou transformar uma pasta de arquivos de dados para um pacote de dados para que seja mais fácil publicar.


```python script
""" from frictionless import Resource

resource = Resource("data/country-2.resource-cleaned.yaml")
resource.title = "Countries"
resource.description = "It's a research project"
resource.dialect.header_rows = [2]
resource.dialect.get_control('csv').delimiter = ";"
resource.to_yaml("data/country-2.resource-updated.yaml") """
```


```python script
""" from frictionless import Resource

resource = Resource("country-2.resource-updated.yaml")
resource.custom["customKey1"] = "Value1"
resource.custom["customKey2"] = "Value2"
resource.to_yaml("data/country-2.resource-update2.yaml") """
```

```python script
""" with open('data/country-2.resource-updated2.yaml') as file:
    print(file.read()) """
```
