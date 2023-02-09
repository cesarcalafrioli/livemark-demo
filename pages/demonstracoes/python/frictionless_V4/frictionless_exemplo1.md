# FRICTIONLESS - EXEMPLO 1

Descrevendo os dados inválidos do arquivo invalid.csv

### Descrevendo o arquivo

Descrever o arquivo, na linguagem do Frictionless, significa criar os metadados para os arquivos de dados. A criacao de metadados e feita atraves da funcao describe.

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

### Extraindo o arquivo

Extrair consiste em ler os dados tabulares do arquivo fonte CSV.

```python task id=data-extract
from pprint import pprint
from frictionless import extract

rows = extract('data/invalid.csv')
pprint(rows)
```

```bash script
livemark run data-extract
```

### Validando o arquivo

Validamos o arquivo csv atraves da funcao validate. Essa funcao visa identificar e fixar todos os erros presentes nos dados tabulares.

```python task id=data-validate
from pprint import pprint
from frictionless import validate

report = validate('data/invalid.csv')
pprint(report.flatten(["rowPosition", "fieldPosition", "code"]))
```

```bash script
livemark run data-validate
```
