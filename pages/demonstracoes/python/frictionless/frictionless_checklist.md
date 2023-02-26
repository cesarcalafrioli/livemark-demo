# Frictionless - Checklist

## Criando checklist

checklist é um conjunto de checagem de validações e alguns configurações adicionais.

```python script
from frictionless import Checklist, checks

checklist = Checklist(checks=[checks.row_constraint(formula='id > 1')])
print(checklist)
```

## Checks de validação

O conceito de check é uma parte da API de validação. Você pode criar um Check customizado para ser utilizado como parte do resource ou pacote de validação.

```python script
from frictionless import Check, errors

class duplicate_row(Check):
    code = "duplicate-row"
    Errors = [errors.DuplicateRowError]

    def __init__(self, descriptor=None):
        super().__init__(descriptor)
        self.__memory = {}

    def validate_row(self, row):
        text = ",".join(map(str, row.values()))
        hash = hashlib.sha256(text.encode("utf-8")).hexdigest()
        match = self.__memory.get(hash)
        if match:
            note = 'the same as row at position "%s"' % match
            yield errors.DuplicateRowError.from_row(row, note=note)
        self.__memory[hash] = row.row_position

    # Metadata

    metadata_profile = {  # type: ignore
        "type": "object",
        "properties": {},
    }
```

É comum criar um erro customizado ao lado do check de cumstomização.