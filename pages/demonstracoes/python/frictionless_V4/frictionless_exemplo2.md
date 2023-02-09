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

# Insere novos metadados no schema e corrige os dados
detector = Detector(field_missing_values=["","n/a"]) # Informa quais os valores que devem ser reconhecidos como "vazios"
resource = describe('data/countries.csv', detector=detector) # Aplica substituição do arquivo countries.csv
resource.schema.get_field("neighbor_id").type = "integer" # Modifica tipo de dado da coluna neighbor_id para ser do tipo inteiro
resource.schema.get_field("population").type = "integer" # Modifica tipo de dado da coluna population para ser do tipo inteiro
resource.schema.foreign_keys.append(
    {"fields": ["neighbor_id"], "reference": {"resource": "", "fields": ["id"]}}
)
resource.path = "countries.csv" # Modificando o caminho exato do arquivo csv
resource.to_yaml("data/countries.resource.yaml") # Exportando o schema
```

Exibindo os metadados em yaml

```python script
from pprint import pprint
with open('data/countries.resource.yaml') as paises_yaml:
    pprint(paises_yaml.read())
```

### Extraindo os dados

```python script
# Exibindo os dados de countries.csv ignorando os metadados
from pprint import pprint
from frictionless import extract

rows = extract('data/countries.csv')
pprint(rows)
```

Falhas encontrada no arquivo countries.csv:
- Valores inteiros misturados com valores string;
- Linhas praticamente vazias
- Incoerencia no campo neighbor_id ( No lugar do id aparece o nome do país. )

Exibindo os metadados corrigidos

```python script
"""
A funcao describe atribui automaticamente o caminho relativo do arquivo tabular na propriedade path do arquivo resource.yaml
Como este arquivo resource.yaml está localizado na pasta data, o caminho 'data/countries.csv' não será reconhecido pois não há uma pasta data dentro da pasta em que o arquivo countries.resource.yaml se encontra. Logo, é necessário alerar a propriedade path no resource para a localizacao correta do arquivo .csv e isso pode ser feito por meio da classe Resource.

Para mais informações sobre a classe resoure acesse o link abaixo:
https://v4.framework.frictionlessdata.io/docs/guides/framework/resource-guide
"""
# Extraindo os dados do mesmo arquivo countries.csv atraves do metadados countries.resource.yaml
from pprint import pprint
from frictionless import extract, Resource

# Alterando a propriedade path do resource countries.resource.yaml para "countries.csv"
#resource = Resource('data/countries.resource.yaml')
#resource.path = "countries.csv"
#resource.to_yaml('data/countries.resource.yaml')

# Extraindo os dados com o metadado criado
rows = extract('data/countries.resource.yaml')
pprint(rows)
```

### Validando os dados

Validando os dados diretamente do arquivo CSV ( Não colocar dentro do projeto real )

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/countries.csv')
pprint(report.flatten(["rowPosition", "fieldPosition", "code"]))
```

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/countries.resource.yaml')
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
                if row["name"]: # Retorna linhas que estão preenchidas. Linhas vazias não são retornadas
                    yield row

    # Meta
    resource.schema = Resource("data/countries.resource.yaml").schema
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

Acima temos o arquivo transformado com as informação devidamente preenchidas.

O script abaixo lista os arquivos contendo a palavra 'countries' dentro do diretório 'data':

```python script
import os

files = [] # Lista vazia aonde serão armazenados os items desejados ( No nosso caso são os arquivos que contenham a palavra "countries" )

# Para cada arquivo listado dentro do diretório "data", é verificado se é o arquivo desejado ( arquivos que contenham a palavra "countries" ). Caso afirmativo, este será adicionado à lista "files".
for item in os.listdir(path='.\\data'):

    # Se o item, além de ser um arquivo, for o arquivo desejado, este será adicionado à lista files
    if os.path.isfile('.\\data\\'+item) and item.startswith('countries'):

        # Adicionando o arquivo à lista "files"
        files.append(item)

# Imprime os arquivos desejados que foram encontrados
print(files)
```

TESTE 2

```python script
files = [f for f in os.listdir(path='.\\data') if os.path.isfile('.\\data\\'+f) and f.startswith('countries')]
print(files)

files = [f for f in os.listdir('.') if os.path.isfile(f) and f.startswith('countries')]
print(files)