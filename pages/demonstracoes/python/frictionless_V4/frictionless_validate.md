# FRICTIONLESS - VALIDATE

Validação é o processo de identificar problemas que ocorreram nos dados para assim corrigí-los.

```python script
with open('data/capital-invalid.csv') as file:
    print(file.read())
```

Pelo framework Frictionless, os erros são detalhados de uma forma que os usuários consigam entendê-los.

```python script
from frictionless import validate

report = validate('data/capital-invalid.csv')
print(report.to_summary())
```

No Frictionless, um resource é um arquivo único, como um arquivo de dados, e um package é um conjunto de arquivos, como um arquivo de dados e um schema.

```python script
# Criando um schema de tabela inválido
from frictionless import Schema

schema = Schema()
schema.fields = {} # Deve ser uma lista
schema.to_yaml('data/capital.schema.yaml')
```

Validando o schema criado:

```python script
from frictionless import validate
from tabulate import tabulate

report = validate('data/capital.schema.yaml')
errors = report.flatten(['code','message'])
print(tabulate(errors, headers = ['code', 'message']))
```

## Validando um Resource

Lembrando que um resource é um container que contem tantos dados quanto metadados.

Criando um resource:
```python script
from frictionless import describe

resource = describe('data/capital-invalid.csv')
resource.path = "capital-invalid.csv"
resource.to_yaml('data/capital.resource.yaml')
```

Validando para assegurar que nós estamos obtendo o mesmo resultado que temos sem utilizar um resource.

```python script
from frictionless import validate

report = validate('data/capital.resource.yaml')
print(report.to_summary())
print(validate('data/capital.resource.yaml')) # Para exibir como os dados do validade estão no seu estado original
```

Editando os metadados

```python script
from frictionless import describe

resource = describe('data/capital-invalid.csv')
resource.path = "capital-invalid.csv"
resource['bytes'] = 100 # Valor incorreto
resource['hash'] = 'ae23c74693ca2d3f0e38b9ba3570775b' # hash incorreto
resource.to_yaml('data/capital.resource.yaml')
```

```python script
from frictionless import validate

report = validate('data/capital.resource.yaml')
print(report.to_summary())
```

## Validating a Package

Package é um conjunto de resources seguido de metadados adicionais.

```python script
with open('data/capital-valid.csv') as file:
    print(file.read())
```

Descrevendo e validando o package

```python script
from frictionless import describe, validate

# create package descriptor
package = describe("data/capital-*id.csv")
package.get_resource("capital-invalid").path = "capital-invalid.csv"
package.get_resource("capital-valid").path = "capital-invalid.csv"
package.to_yaml("data/capital-validandinvalid.package.yaml")

# validate
report = validate("data/capital-validandinvalid.package.yaml")
print(report.to_summary())
```

## Validando uma consulta

Consulta é uma representação declarativa de uma atividade de validação.

```python script
from frictionless import Inquiry

inquiry = Inquiry({'tasks': [
    {'source': 'data/capital-valid.csv'},
    {'source': 'data/capital.resource.yaml'},
]})

inquiry.to_yaml('data/capital.inquiry.yaml')
```

Executando a validação:

```python script
from frictionless import validate

report = validate("data/capital.inquiry.yaml")
print(report.to_summary())
```

## Relatório de validação

Todas as funções validate retornam um relatório de Validação. É um objeto único contendo informações sobre uma validação.

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/capital-invalid.csv', pick_errors=['duplicate-label'])
pprint(report)
```

Usando a função report.flatten para simplificar a representação dos erros.

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/capital-invalid.csv', pick_errors=['duplicate-label'])
pprint(report.flatten(["rowPosition", "fieldPosition", "code", "message"]))
```

## Erros de validação

## Checks disponíveis

## Checks customizáveis

## Erros Pick/Skip

## Limite de erros

## Limite de memória