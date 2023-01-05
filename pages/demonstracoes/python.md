## Exemplo em python

## Descrever o arquivo

As tasks servem para determinar funcionalidades. Para rodar uma task é necessário inserir o seguinte trecho de código:

```python task id=data-describe
from pprint import pprint
from frictionless import describe

resource = describe('data/invalid.csv')
pprint(resource)
```

Executando o script acima no bash

```bash script
livemark run data-describe
```

## Extrair o arquivo

```python task id=data-extract
from pprint import pprint
from frictionless import extract

rows = extract('data/invalid.csv')
pprint(rows)
```

```bash script
livemark run data-extract
```

## Validate

```python task id=data-validate
from pprint import pprint
from frictionless import extract

rows = validate('data/invalid.csv')
pprint(report.flatten(["rowPosition", "fieldPosition", "code"]))
```

```bash script
livemark run data-validate
```