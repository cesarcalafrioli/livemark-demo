# Python - Extract

Exrair dados consiste em ler os dados tabulares do arquivo fonte ( csv, yaml, json, etc... ). Para esse processo de extração podemos fazer diversas customizações como fornecer um formato de arquivo, fornecer o schema, limitar campos ou linhas e etc...

A função extract, por padrão, exibe a saida de informações no formato 'utf-8'.

Exibindo as informações da tabela country-3.csv

```python script
with open('data/country-3.csv') as file:
    print(file.read())
```

Exibindo as informações da tabela capital-3.csv

```python script
with open('data/capital-3.csv') as file:
    print(file.read())
```

Extraindo dados do arquivo country-3.csv

```python script
from pprint import pprint
from frictionless import extract

#rows = extract('data/country-3.csv')
rows = extract('data/*-3.csv')
print(type(rows))
pprint(rows)
```

## As funções Extract

```python script
from frictionless import extract

rows = extract('data/capital-3.csv')
resource = extract('data/capital-3.csv', type="resource")
package = extract('data/capital-3.csv', type="package")
package_2 = extract('data/*-3.csv', type="package")

print("Rows:",rows)
print("Resource:",resource)
print("Package:",package)
print("package com dois arquivos tabulares:",package_2)
```


As funções extract semprem leem os dados na forma de linhas.

## Extraindo um resource

Um resource possui apenas um arquivo. Para extrair um resource, temos três opções:

1 - Extrair do próprio arquivo de dados

Capital-3.csv

```python script
from pprint import pprint
from frictionless import extract

rows = extract('data/capital-3.csv')
pprint(rows)
```

O mesmo do trecho acima, mas por meio da classe Resource

```python script
from pprint import pprint
from frictionless import Resource

resource = Resource('data/capital-3.csv')
pprint(resource.extract())
#pprint(resource.validate()) # Testando a classe Resource
```


countries.csv

```python script
from pprint import pprint
from frictionless import extract

rows = extract('data/countries.csv')
print(type(rows))
pprint(rows)
```

2 - Extrair o resource de um arquivo descritor. O arquivo descritor é útil pois pode conter diferentes metadados e são armazenados no disco.

countries.resource.yaml

```python script
from frictionless import Resource

resource = Resource('data/capital-3.csv') # Criando o resource do arquivo
resource.infer() # Inferindo o resource.
resource.schema.missing_values.append('3') # Interpreta 3 como um valor vazio e o substitui por 'None' na extração do resource
resource.path = "capital-3.csv" # Alterando o path do resource
resource.to_yaml('data/capital-3.resource-test.yaml') # Salvando o resource no disco
```

Também é possível Utilizar um arquivo descriptor já criado

```python script
from pprint import pprint
from frictionless import extract

rows = extract('data/capital-3.resource-test.yaml')
pprint(rows)
```

```python script
from pprint import pprint
from frictionless import extract

#rows = extract('data/country-1.schema.yaml') # NÃO FUNCIONA
rows = extract('data/countries.resource.yaml')
pprint(rows)
```

## Extraindo um package

Extrair um pacote é o terceiro caminho para extrair informação. 

```python script
from pprint import pprint
from frictionless import extract

data = extract('data/*-3.csv')
pprint(data)
```

Como alternativa, podemos extrair o pacote do arquivo descritor usando a função package.extract

```python script
from pprint import pprint
from frictionless import Package

package = Package('data/country-3.package.yaml')
pprint(package.extract())
```

## Classe Resource

Fornece metadados sobre um resource com funções de ler e transmitir. A função extract sempre lê linhas na memória. Resource pode fazer a mesma coisa porém oferece uma escolha a respeito da saída de dados que pode ser rows, data, text ou bytes.

- rows: read_rows() # Retorna uma lista com dicionário, onde cada um representa uma linha do arquivo
- data: read_cells() # Retorna uma lista
- **text:** read_text() # Retorna uma representação textual
- bytes: read_bytes() # Retorna a leitura do arquivo em bytes

Lendo bytes

```python script
from pprint import pprint
from frictionless import Resource

resource = Resource('data/country-3.csv')
pprint(resource.read_bytes())
pprint(type(resource.read_bytes()))
```

Lendo texto

```python script
from frictionless import Resource

resource = Resource('data/country-3.csv')
pprint(resource.read_text())
pprint(type(resource.read_text()))
```

Lendo células

```python script
from frictionless import Resource

resource = Resource('data/country-3.csv')
pprint(resource.read_cells())
pprint(type(resource.read_cells()))
```

Lendo linhas

```python script
from frictionless import Resource

resource = Resource('data/country-3.csv')
pprint(resource.read_rows())
pprint(type(resource.read_rows()))
```

Lendo somente o cabeçalho das colunas

```python script
from frictionless import Resource

with Resource('data/country-3.csv') as resource:
    pprint(resource.header)
```

Transmitindo interfaces

Nem sempre é possível ler todo os dados na memória se um arquivo for muito grande. Para essa situação utiliza as funções de streaming

```python script
from frictionless import Resource

with Resource('data/country-3.csv') as resource:
    resource.byte_stream
    resource.text_stream
    resource.cell_stream
    resource.row_stream

print(resource)
```

## Classe Package

Classe para criação de metadados de conjuntos de dados e leitura de dados

Fornece funções para ler os conteúdos de um pacote.

```python script
from frictionless import describe

package = describe('data/*-3.csv')
package.get_resource("country-3").path = "country-3.csv"
package.get_resource("capital-3").path = "capital-3.csv"
package.to_yaml('data/country-3.package.yaml')
```

Criando um pacote dos arquivos de dados e lendo os resources de cada um

```python script
from frictionless import Package

package = Package('data/*-3.csv')
package.infer(sample=False)
pprint(package.get_resource('country-3').read_rows())
pprint(package.get_resource('capital-3').read_rows())
pprint("Imprimindo as linhas em forma de célula")
pprint(package.get_resource('country-3').read_cells())
pprint(package.get_resource('capital-3').read_cells())
```
