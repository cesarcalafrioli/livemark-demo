# FRICTIONLESS - EXTRACT

Extrair dados significa ler os dados tabulares da sua fonte.

```python script
# Exibindo informacoes do arquivo "country-3.csv"
with open('data/country-3.csv') as file:
    print(file.read())
```

```python script
# Exibindo informacoes do arquivo "capital-3.csv"
with open('data/capital-3.csv') as file:
    print(file.read())
```

```python script
# Extraindo dados de um resource
from frictionless import extract
from tabulate import tabulate

rows = extract("data/country-3.csv")
print(tabulate(rows, headers="keys", tablefmt="rst"))
```

## Funcoes extract

```python script
from frictionless import extract

rows = extract('data/capital-3.csv')
resource = extract('data/capital-3.csv', type="resource")
package = extract('data/capital-3.csv', type="package")
```

## Extraindo um Resource

```python script
from frictionless import extract
from tabulate import tabulate

resource = extract('data/capital-3.csv')
print(tabulate(resource, headers="keys", tablefmt="rst"))
```

Segunda opcao: Extrair o resource de um arquivo descritor usando a funcao extract_resource.

```python script
from frictionless import Resource

resource = Resource('data/capital-3.csv')
resource.infer()

# Adicionando valor ao schema
resource.schema.missing_values.append('3') # Interpret 3 as a missing value
#resource.path="capital-3.csv" TESTEI E NAO FUNCIONOU
resource.to_yaml('capital-3.resource.yaml')
```

```python script
# Visualizando o arquivo capital-3.resource.yaml
with open('capital-3.resource.yaml') as file:
    print(file.read())
```

```python script
"""
A funcao describe atribui automaticamente o caminho relativo do arquivo tabular na propriedade path do arquivo resource.yaml
Como este arquivo resource.yaml está localizado na pasta data, o caminho 'data/countries.csv' não será reconhecido pois não há uma pasta data dentro da pasta em que o arquivo countries.resource.yaml se encontra. Logo, é necessário alerar a propriedade path no resource para a localizacao correta do arquivo .csv e isso pode ser feito por meio da classe Resource.

Para mais informações sobre a classe resoure acesse o link abaixo:
https://v4.framework.frictionlessdata.io/docs/guides/framework/resource-guide
"""
# Extraindo o resource como descritor
from frictionless import extract
from tabulate import tabulate

# Alterando a propriedade path do resource countries.resource.yaml para "countries.csv"
#resource = Resource('data/countries.resource.yaml')
#resource.path = "countries.csv"
#resource.to_yaml('data/countries.resource.yaml')

data = extract('capital-3.resource.yaml')
print(tabulate(data, headers="keys", tablefmt="rst"))
```

## Extraindo o package

```python script
from frictionless import extract
from tabulate import tabulate

data = extract('data/*-3.csv')
for path, rows in data.items():
    print("#---")
    print("# data:", path)
    print("#---")
    print(tabulate(rows, headers="keys", tablefmt="rst"))
    print("\n")
```

```python script
#
from pprint import pprint
from frictionless import Package

package = Package('data/country-3.package.yaml')
pprint(package)
```

## Classe Resource

A classe resource fornece metadados sobre um resource com as funcoes read e stream. A funcao extract sempre le linhas na memoria. Resource pode fazer a mesma coisa porem da opcoes a respeito da saida de dados que pode ser rows, data, text ou bytes.

```python script
# Representacao dos conteudos em bytes
from pprint import pprint
from frictionless import Resource

resource = Resource('data/country-3.csv')
pprint(resource.read_bytes())
```

```python script
# Representacao dos conteudos em texto
from pprint import pprint
from frictionless import Resource

resource = Resource('data/country-3.csv')
pprint(resource.read_text())
```

```python script
# Representacao dos conteudos em forma de lista
from pprint import pprint
from frictionless import Resource

resource = Resource('data/country-3.csv')
pprint(resource.read_lists())
```

```python script
# Representacao dos conteudos em forma de dicionario
from pprint import pprint
from frictionless import Resource

resource = Resource('data/country-3.csv')
pprint(resource.read_rows())
```

```python script
# Imprimindo o nome das colunas
from pprint import pprint
from frictionless import Resource

with Resource('data/country-3.csv') as resource:
    pprint(resource.header)
```

```python script
# Nem sempre é possível ler todos os dados na memória quando um arquivo é muito grande. Para essas situacoes, frictionless fornece funcoes streaming.
from frictionless import Resource

with Resource('data/country-3.csv') as resource:
    resource.byte_stream
    resource.text_stream
    resource.list_stream
    resource.row_stream
```

## Classe Package

A classe Package fornece funcoes para ler os conteudos de um pacote.

```python script
from frictionless import describe

package = describe('data/*-3.csv')
package.to_json('data/country-3.package.json')
```

```python script
# Criando um package para os arquivos de dados capital-3 e country-3.csv e lendo seus resource
from frictionless import package

package = Package('data/*-3.csv')
pprint(package.get_resource('country-3').read_rows())
pprint(package.get_resource('capital-3').read_rows())
```

## Classe Header

O objeto resource.header descreve o resource em mais detalhes.

```python script
from frictionless import Resource

with Resource('data/capital-3.csv') as resource:
    print(f'Header: {resource.header}')
    print(f'Labels: {resource.header.labels}')
    print(f'Fields: {resource.header.fields}')
    print(f'Field Names: {resource.header.field_names}')
    print(f'Field Positions: {resource.header.field_positions}')
    print(f'Errors: {resource.header.errors}')
    print(f'Valid: {resource.header.valid}')
    print(f'As List: {resource.header.to_list()}')
```

```python script
# Quando o cabecalho contem erros na sua estrutura tabular, o trecho abaixo pode ser muito util para revelar discrepancias, valores duplicados ou informacoes na celula faltantes.
from pprint import pprint
from frictionless import Resource

with Resource([['name', 'name'], ['value', 'value']]) as resource:
    pprint(resource.header.errors)
```

## Classe Row



```python script
from frictionless import Resource, Detector

detector = Detector(schema_patch = {'missingValues': ['1']})

with Resource('data/capital-3.csv', detector=detector) as resource:
    for row in resource:
        print(f'Rows: {row}')
        print(f'Cells: {row.cells}')
        print(f'Fields: {row.fields}')
        print(f'Field Names: {row.field_names}')
        print(f'Field Positions: {row.field_positions}')
        print(f'Value of field "name": {row["name"]}') # accessed as a dict
        print(f'Row Position: {row.row_position}') # physical line number starting from 1
        print(f'Row Number: {row.row_number}') # counted row number starting from 1
        print(f'Blank Cells: {row.blank_cells}')
        print(f'Error Cells: {row.error_cells}')
        print(f'Errors: {row.errors}')
        print(f'Valid: {row.valid}')
        print(f'As Dict: {row.to_dict(json=False)}')
        print(f'As List: {row.to_list(json=True)}') # JSON compatible data types
        break
```

```python script
from pprint import pprint
from frictionless import Resource

with Resource([['name'], ['value', 'value']]) as resource:
    for row in resource.row_stream:
        pprint(row.errors)
```