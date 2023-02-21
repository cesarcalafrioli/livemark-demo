# PYTHON - VALIDATE

Validação de dados tabulares é o processo de identificação de problemas que ocorreram nos dados e que precisam ser corrigidos.

```python script
with open('data/capital-invalid.csv') as file:
    print(file.read())
```

Validando o arquivo capital-invalid.csv

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/capital-invalid.csv')
print(report)
```

TESTE: Validando arquivo licitacoes-itens-2023-ufpe.csv

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/licitacoes-itens-2023-ufpe.csv')
print(report)
```

## Validando um Schema

Criando um schema de tabela inválido.

```python script
import yaml
from frictionless import Schema

descriptor = {}
descriptor['fields'] = 'bad' # must be a list
with open('data/bad.schema.yaml', 'w') as file:
    yaml.dump(descriptor, file)
```

Validando o schema bad.schema.yaml

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/bad.schema.yaml')
pprint(report)
```

## Validando um Resource

Lembrando que um resource é um container contendo ambos os dados e metadados.

O trecho abaixo cria um descritor resource:

```python script
from frictionless import describe

resource = describe('data/capital-invalid.csv') # Criando os metadados do arquivo
resource.path = "capital-invalid.csv" # Alterando o caminho do resource
resource.to_yaml('data/capital.resource.yaml') # Salvando no disco
```

Validando o resource criado

```python script
from frictionless import validate

report = validate('data/capital.resource.yaml')
print(report.to_dict()['tasks'][0]['errors'][0]['type']) # Informando somente o tipo de erro
print(report.to_dict()['tasks'][0]['errors'][0]['message']) # Informando a mensagem de erro
print(report)
```

Editando e validando os metadados

```python script


from frictionless import describe

resource = describe('data/capital-invalid.csv')
resource.add_defined('stats')  # TODO: fix and remove this line
resource.stats.md5 = 'ae23c74693ca2d3f0e38b9ba3570775b' # this is a made up incorrect
resource.stats.bytes = 100 # this is wrong
resource.path = "capital-invalid.csv"
resource.to_yaml('data/capital.resource-bad.yaml')
```

Visualizando o resource modificado

```python script
from frictionless import validate

report = validate('data/capital.resource-bad.yaml')
print(report)
```

## Validando um pacote

Lembrando que um pacote é um conjunto de resource mais metadados adicionais.

```python script
with open('data/capital-valid.csv') as file:
    print(file.read())
```

Descrevendo e validando um pacote

```python script
from frictionless import describe, validate

# create package descriptor
package = describe("data/capital-*id.csv")
package.to_yaml("data/capital.package.yaml")
# validate
report = validate("data/capital.package.yaml")
print(report)
```

## Validando um Inquerito ( Inquiry )

Inquiry é uma representação declarativa do trabalho de validação. 

Criando um Inquiry que inclua a validação de um arquivo individual e uma validação de resource. O inquiry pode fornecer muita flexibilidade e poder quando o fluxo de validação se torna complexo.

```python script
from frictionless import Inquiry, InquiryTask

inquiry = Inquiry(tasks=[
    InquiryTask(path='data/capital-valid.csv'),
    InquiryTask(resource='data/capital.resource.yaml'),
])
inquiry.to_yaml('data/capital.inquiry.yaml')
```

Validando

```python script
from frictionless import validate

report = validate("data/capital.inquiry.yaml")
print(report)
```

## Relatório de validação

todas as funções validate retornam um relatório de validação.

```python script
from frictionless import validate

report = validate('data/capital-invalid.csv', pick_errors=['duplicate-label'])
print(report)

```

A função report.flatten representa os erros:

```python script
from pprint import pprint
from frictionless import validate

report = validate("data/capital-invalid.csv", pick_errors=["duplicate-label"])
pprint(report.flatten(["rowNumber", "fieldNumber", "code", "message"]))
```

```python script
from frictionless import validate

report = validate("data/bad.json", type='schema')
print(report)
```

## Validação de erros

```python scripts
from frictionless import validate

report = validate("data/capital-invalid.csv", pick_errors=["duplicate-label"])
error = report.error  # this is only available for one table / one error sitution
print(f'Type: "{error.type}"')
print(f'Title: "{error.title}"')
print(f'Tags: "{error.tags}"')
print(f'Note: "{error.note}"')
print(f'Message: "{error.message}"')
print(f'Description: "{error.description}"')
```

```python script
from frictionless import validate

report = validate("data/capital-invalid.csv", pick_errors=["duplicate-label"])
error = report.error  # this is only available for one table / one error sitution
print(error)
```

## Checks disponíveis

Há vários checks de validação incluídos no framework.

```python script
from pprint import pprint
from frictionless import validate, checks

checks = [checks.sequential_value(field_name='id')]
report = validate('capital-invalid.csv', checks=checks)
pprint(report.flatten(["rowNumber", "fieldNumber", "type", "note"]))
```

## Checks customisáveis

```python script
from pprint import pprint
from frictionless import Check, validate, errors

# Create check
class forbidden_two(Check):
    Errors = [errors.CellError]
    def validate_row(self, row):
        if row['header'] == 2:
            note = '2 is forbidden!'
            yield errors.CellError.from_row(row, note=note, field_name='header')

# Validate table
source = b'header\n1\n2\n3'
report = validate(source,  format='csv', checks=[forbidden_two()])
pprint(report.flatten(["rowNumber", "fieldNumber", "code", "note"]))

```

## Pick/Skip errors

 É possível pegar erros específicos ou pulá-los(Skip)

```python script
from pprint import pprint
from frictionless import validate

report1 = validate("data/capital-invalid.csv", pick_errors=["duplicate-label"])
report2 = validate("data/capital-invalid.csv", skip_errors=["duplicate-label"]) # Ignora erros específicos
pprint(report1.flatten(["rowNumber", "fieldNumber", "type"]))
pprint(report2.flatten(["rowNumber", "fieldNumber", "type"]))
```

Error tags

```python script
from pprint import pprint
from frictionless import validate

report1 = validate("capital-invalid.csv", pick_errors=["#header"])
report2 = validate("capital-invalid.csv", skip_errors=["#row"])
pprint(report1.flatten(["rowNumber", "fieldNumber", "type"]))
pprint(report2.flatten(["rowNumber", "fieldNumber", "type"]))
```

## Limitar erros

```python script
from pprint import pprint
from frictionless import validate

report = validate("data/capital-invalid.csv", limit_errors=1)
pprint(report.flatten(["rowNumber", "fieldNumber", "type"]))

```