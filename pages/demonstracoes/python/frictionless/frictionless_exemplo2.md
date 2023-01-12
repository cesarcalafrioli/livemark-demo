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
resource = describe('data/countries.csv', detector=detector) #Aplica substituição do arquivo countries.csv
resource.schema.get_field("neighbor_id").type = "integer" # Modifica tipo de dado da coluna neighbor_id para ser do tipo inteiro
#resource.schema.get_field("population").type = "integer" # Modifica tipo de dado da coluna population para ser do tipo inteiro
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

Exibindo os metadados corrigidos

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

No script acima, foram encontrados somente dois arquivos, "countries.csv" e "countries_transformed.csv", pois eles estavam localizados dentro do diretório "data". No entando, estão faltando à lista outros dois arquivos, que são o data resource, pois eles estão localizados fora desse mesmo diretório: "countries.resource.json" e "countries.resource.yaml".

O script abaixo tem a mesma funcionalidade do script anterior, porém é feita uma pesquisa recursiva dentro de cada diretório localizado na pasta raiz.

```python script
files = [] # Lista vazia aonde serão armazenados os items desejados ( No nosso caso são os arquivos que contenham a palavra "countries")
data_path = '.' # Diretório raiz
data_dir_list = os.listdir(data_path) # Lista os items dentro de um diretório ( No nosso caso é o diretório raiz)

# Para cada item listado dentro da variável data_dir_list, é verificado se é um arquivo ou uma pasta. Caso seja o arquivo que queiramos, este será adicionado à lista "files". Caso seja uma pasta, será realizado a mesma verificação dentro dela.
for item in data_dir_list:

    # Verifica primeiro se determinado item é um diretório
    if os.path.isdir(os.path.join(data_path, item)):

        # Se o item for um diretório, será realizado uma busca dentro dela. Se houver um diretório dentro, será feita uma nova busca dentro ( Recursão ).
        for dir_file in os.listdir(path=item):

            # Se o item, além de ser um arquivo é o arquivo que desejamos,este seja colocado dentro da lista
            if os.path.isfile(os.path.join(data_path, item)+'\\'+dir_file) and dir_file.startswith('countries'):
                files.append(dir_file)

    # Verifica se o item, além de ser um arquivo, é o arquivo que desejamos que seja colocado na lista
    #print(os.path.join(data_path, dataset))
    if os.path.isfile(os.path.join(data_path, item)) and item.startswith('countries'):
        files.append(item)

# Imprime os arquivos desejados que foram encontrados
print(files)
```

Como os arquivos 'countries.resource.json' e 'countries.resource.yaml' foram localizados fora do diretório data ( eles estavam na pasta raiz ), também foram adicionados à lista "files".