# Frictionless - Basico



```python script
with open('data/countries.csv') as file:
    print(file.read())
```

## Descrevendo dados

```python script
from pprint import pprint
from frictionless import describe

resource = describe('data/countries.csv')
pprint(resource)
```

Atualizando os metadados e salvando-os no disco:

```python script
from frictionless import Detector, describe

# Usa o método Detector com o argumento field missing_values p/determinar valores ausentes
detector = Detector(field_missing_values=["","n/a"])
# O método detector como argumento no describe vai reconhecer os valores ausentes
resource = describe("data/countries.csv", detector=detector)
# Substituição do tipo de dados do campo "neighbor_id" de string para int
resource.schema.get_field("neighbor_id").type = "integer"
# Relaciona o campo "neighbor_id" com a própria tabela dizendo que ele faz referencia a sim própria
resource.schema.foreign_keys.append(
    {"fields": ["neighbor_id"], "reference": {"resource": "", "fields": ["id"]}}
)
# Altera o caminho do yaml
resource.path = "countries.csv"
resource.to_yaml("data/countries.resource.yaml")
```

Exibindo as informacoes do metadado criado

```python script
with open('data/countries.resource.yaml') as file:
    print(file.read())
```

## Extraindo os dados

Extraindo as informacoes dos dados sem levar em conta o metadado que foi criado:

```python script
from pprint import pprint
from frictionless import extract

rows = extract('data/countries.csv')
pprint(rows)
```

Extraindo os dados corrigidos atraves do arquivo yaml

```python script
from pprint import pprint
from frictionless import extract

rows = extract('data/countries.resource.yaml')
pprint(rows)
```

## Validando os dados

Validando o arquivo csv

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/countries.csv')
pprint(report.flatten(["rowNumber", "fieldNumber", "type"]))
```

Validando o arquivo yaml

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/countries.resource.yaml')
pprint(report.flatten(["rowNumber", "fieldNumber", "type"]))
```

## Transformando os dados

```python script
from pprint import pprint
from frictionless import Resource, Pipeline, describe, transform, steps

# Pipeline parece ser uma novidade da versão 5 - ANALIZAR A DOC
pipeline = Pipeline( steps = [

    # Corrige as informações dos campos.
    steps.cell_replace(field_name = 'neighbor_id', pattern='22', replace='2'),
    steps.cell_replace(field_name = 'population', pattern='n/a', replace='67'),

    # Filtrando ( comentar depois )
    steps.row_filter(formula='population'),

    # Atribuindo o tipo de dados para a coluna 
    steps.field_update(name='neighbor_id', descriptor={"type": "integer"}),

    # Criando a normalização 
    steps.table_normalize(),

    # Criando um arquivo csv com as informaçoes corrigidas
    steps.table_write(path="data/countries-cleaned.csv"),
])

source = Resource("data/countries.csv")
target = source.transform(pipeline)
pprint(target.read_rows())
```

Mostra as informações do arquivo corrigido

```python script
with open('data/countries-cleaned.csv') as file:
    print(file.read())
```

Listando o arquivo

```python script
import os

files = [f for f in os.listdir('.\\data') if os.path.isfile('.\\data\\'+f) and f.startswith('countries-cleaned')]
print(files)
```
















































<!-->
```python script
from pprint import pprint
from frictionless import extract

rows = extract('data/countries.resource.yaml')
pprint(rows)
```

## Validando os dados

Validando os dados do arquivo tabular:

```python script
from pprint import pprint
from frictionless import validate

report = validate("data/countries.csv")
pprint(report.flatten(["rowNumber", "fieldNumber", "type"]))
```

Validando os metadados do arquivo tabular:

```python script
from pprint import pprint
from frictionless import validate

report = validate("data/countries.resource.yaml")
pprint(report.flatten(["rowNumber", "fieldNumber", "type"]))
```

## Transformando os dados

Utilizamos os metadados para fixar automaticamente todos os tipos de problemas ( Alguns problemas precisaremos fixar manualmente ).

```python script
from pprint import pprint
from frictionless import Resource, Pipeline, describe, transform, steps

# Pipeline parece ser uma novidade da versão 5 - ANALIZAR A DOC
pipeline = Pipeline( steps = [
    steps.cell_replace(field_name = 'neighbor_id', pattern='22', replace='2'),
    steps.cell_replace(field_name = 'population', pattern='n/a', replace='67'),
    steps.row_filter(formula='population'),
    steps.field_update(name='neighbor_id', descriptor={"type": "integer"}),
    steps.table_normalize(),
    steps.table_write(path="data/countries-cleaned.csv"),
])

source = Resource("data/countries.csv")
target = source.transform(pipeline)
pprint(target.read_rows())
```

Exibindo as informacoes da versao limpa dor arquivo tabular.

```python script
with open('data/countries-cleaned.csv') as file:
    print(file.read())
```

Listando o arquivo tabular limpo:

```python script
import os

files = [f for f in os.listdir('.\\data') if os.path.isfile('.\\data\\'+f) and f.startswith('countries-cleaned')]
print(files)
```