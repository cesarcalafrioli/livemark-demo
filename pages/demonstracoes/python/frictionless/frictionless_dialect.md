# Frictionless - Dialect

Dialect consiste em uma informação de metadados sobre uma fonte de dados tabular.

A classe dialect é aceito por muitas classes e funções:

- Resource
- Describe
- Extract
- Validate
- Etc...

## Header

É um sinal booleano cujo padrão é True indicando se o dado possui uma linha de cabeçalho ou não.

```python script
from frictionless import Resource, Dialect

dialect = Dialect(header=False)
with Resource('data/capital-3.csv', dialect=dialect) as resource:
    print(resource.header.labels)
    print(resource.to_view())
```

### Header Rows

Caso o header seja True por padrão, seus parâmetros indicam onde encontrar a linha de cabeçalho ou linhas de cabeçalho para um cabeçalho mutilinha.

```python script
from frictionless import Resource, Dialect

dialect = Dialect(header_rows=[1, 2, 3])
with Resource('data/capital-3.csv', dialect=dialect) as resource:
    print(resource.header)
    print(resource.to_view())
```

### Header Join

```python script
from frictionless import Resource, Dialect

dialect = Dialect(header_rows=[1, 2, 3], header_join='/')
with Resource('data/capital-3.csv', dialect=dialect) as resource:
    print(resource.header)
    print(resource.to_view())
```

### Header Case

Por padrão, o cabeçalho é case sensitive. Para desativar, utilize o parâmetro header_case.

```python script
from frictionless import Resource, Schema, Dialect, fields

dialect = Dialect(header_case=False) ## Desativa o case sensitive
schema = Schema(fields=[fields.StringField(name="ID"), fields.StringField(name="NAME")])
with Resource('data/capital-3.csv', dialect=dialect, schema=schema) as resource:
  print(f'Header: {resource.header}')
  print(f'Valid: {resource.header.valid}')  # without "header_case" it will have 2 errors
```

### Comment Char

Especifica os caracteres usados para comentar as linhas.

```python script
from frictionless import Resource, Dialect

dialect = Dialect(comment_char="#")
with Resource(b'name\n#row1\nrow2', format="csv", dialect=dialect) as resource:
    print(resource.read_rows())
```

### Comment Rows

Uma lista de linhas a serem ignoradas.

```python script
from frictionless import Resource, Dialect

dialect = Dialect(comment_rows=[2])
with Resource(b'name\nrow1\nrow2', format="csv", dialect=dialect) as resource:
    print(resource.read_rows())
```

### Skip Blank Rows

Ignora linhas se eles estiverem completamente vazios

```python script
from frictionless import Resource, Dialect

dialect = Dialect(skip_blank_rows=True)
with Resource(b'name\n\nrow2', format="csv", dialect=dialect) as resource:
    print(resource.read_rows())
```