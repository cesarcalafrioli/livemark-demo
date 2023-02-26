# Frictionless - Schema

## Criando um Schema

Formas distintas de criar um schema.

```python script
# Descrevendo e extraindo um schema de um dicionário
from frictionless import Schema, fields, describe, extract

teste = [{'a':1, 'b': 2}]
schema = describe(teste, type='schema')

schema_ext = extract(teste)
print(schema_ext[0]['a'])
```


```python script
from frictionless import Schema, fields, describe

#schema = describe('data/table.csv', type='schema') # from a resource path
#schema = Schema.from_descriptor('data/schema.json') # from a descriptor path
#schema = Schema.from_descriptor({'fields': [{'name': 'id', 'type': 'integer'}]}) # from a descriptor

# Passos acima porém mais explícito
#schema = Schema(fields=[fields.StringField(name='id')]) # from fields
#schema = Schema.from_descriptor('schema.json') # from a descriptor
```

## Descrevendo um schema


```python script
from frictionless import Schema, fields

schema = Schema(
    fields=[fields.StringField(name='id')],
    missing_values=['na'],
    primary_key=['id'],
    # foreign_keys
)
print(schema)
```

Ao criar um schema, por exemplo, do descritor você pode acessar essas propriedades:

```python script
#from frictionless import Schema

#schema = Schema.from_descriptor('schema.json')
#schema.missing_values.append('-')
# and others
#print(schema)
```

## Gerenciamento de campo

A classe schema fornece métodos úteis para gerenciar campos

```python script
""" from frictionless import Schema, fields

schema = Schema.from_descriptor('schema.json')
print(schema.fields)
print(schema.field_names)
schema.add_field(fields.StringField(name='new-name'))
field = schema.get_field('new-name')
print(schema.has_field('new-name'))
schema.remove_field('new-name') """
```

## Salvando Descritor

Qualquer classe de metadados pode ser salvo como json ou yaml

```python script
""" from frictionless import Schema, fields
schema = Schema(fields=[fields.IntegerField(name='id')])
schema.to_json('schema.json') # Save as JSON
schema.to_yaml('schema.yaml') # Save as YAML """
```

## Lendo células

Durante o processo de leitura de dados um resource utiliza um schema para converter dados.

```python script
""" from frictionless import Schema, fields

schema = Schema(fields=[fields.IntegerField(name='integer'), fields.StringField(name='string')])
cells, notes = schema.read_cells(['3', 'value'])
print(cells) """
```

## Escrevendo células

Durante o processo de escrita de dados um resource utiliza um schema para converter dados

```python script
""" from frictionless import Schema, fields

schema = Schema(fields=[fields.IntegerField(name='integer'), fields.StringField(name='string')])
cells, notes = schema.write_cells([3, 'value'])
print(cells) """
```

## Criando campos

```python script
""" from frictionless import fields

field = fields.IntegerField(name='name')
print(field) """
```

Geralmente trabalhamos com campos onde já foi criado por um schema.

```python script
""" from frictionless import describe

resource = describe('table.csv')
field = resource.schema.get_field('id')
print(field) """
```

## Criando campos

```python script
""" from frictionless import fields

field = fields.IntegerField(name='name')
print(field) """
```

Normalmente nós trabalhamos com campos que já foram criados por um schema.

```python script
""" from frictionless import describe

resource = describe('table.csv')
field = resource.schema.get_field('id')
print(field) """
```

### Tipos de campos

```python script
""" from frictionless import describe

resource = describe('table.csv')
field = resource.schema.get_field('id') # it's an integer
print(field.bare_number) """
```

### Lendo células

Durante o processo de leitura de dados um schema usa um campo internamente. Se necessário um usuário pode converter seus dados utilizando esta interface.

```python script
""" from frictionless import fields

field = fields.IntegerField(name='name')
cell, note = field.read_cell('3')
print(cell) """
```

### escrevendo células

Durante o processo de escrita de dados um schema utiliza um campo internamente.

```python script
""" from frictionless import fields

field = fields.IntegerField(name='name')
cell, note = field.write_cell(3)
print(cell) """
```