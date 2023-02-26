# Frictionless - Pipeline

O Pipeline é um objeto contendo uma lista dos passos de transformação

## Criando Pipeline

Criando pipeline utilizando interface Python:

```python script
from frictionless import Pipeline, transform, steps

pipeline = Pipeline(steps=[steps.table_normalize(), steps.table_melt(field_name='name')])
print(pipeline)
```

## Executando pipeline

Para executar o pipeline você precisa utilizar a função ou método transform

```python script
from frictionless import Pipeline, transform, steps

pipeline = Pipeline(steps=[steps.table_normalize(), steps.table_melt(field_name='name')])
resource = transform('data/country-1.csv', pipeline=pipeline)

print(resource.schema)
print(resource.read_rows())
```

## Transform steps

Você pode criar step customizado para ser usado como parte da transformação do resource ou package:

```python script
# Este step usa PETL
from frictionless import Step

class cell_set(Step):
    code = "cell-set"

    def __init__(self, descriptor=None, *, value=None, field_name=None):
        self.setinitial("value",value)
        self.setinitial("fieldName", field_name)
        super().__init__(descriptor)

    def transform_resource(self, resource):
        value = self.get("Value")
        field_name = self.get("fieldName")
        yield from resource.to_petl().update(field_name, value)
```