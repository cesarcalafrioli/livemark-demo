# Frictionless V5 - Exemplo inicial

```bash script
frictionless validate data/invalid.csv
```

# Atualização de versão

Para atualizar a versão do frictionless, rode o seguinte comando:

'''
pip install --upgrade frictionless
'''

# Uso

Extração de dados - exemplo inicial

```python script
from frictionless import extract

rows = extract('data/table.csv')
print(rows)

```

# Describe - exemplo inicial

Imprindo as informações o arquivo table.csv

```python script
with open('data/invalid.csv') as file:
    print(file.read())
```

Inferindo os metadados diretamente do dado tabular:

```python script
from frictionless import describe
from pprint import pprint

resource = describe('data/invalid.csv')
pprint(resource)
```

Lendo os dados tabulares normalizados do arquivo fonte invalid.csv:

```python script
from pprint import pprint
from frictionless import extract

rows = extract('data/invalid.csv')
pprint(rows)
```

Gerando o relatório de validação que nos ajudará a identificar e corrigir todos os erros presentes nos dados tabulares.

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/invalid.csv')
pprint(report.flatten(["rowNumber", "fieldNumber", "type"]))
```
