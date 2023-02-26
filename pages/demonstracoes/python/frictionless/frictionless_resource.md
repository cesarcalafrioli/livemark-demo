# Frictionless - Resource

```python script
""" from frictionless import Resource

resource = Resource('table.csv') # from a resource path
resource = Resource('resource.json') # from a descriptor path
resource = Resource({'path': 'table.csv'}) # from a descriptor
resource = Resource(path='table.csv') # from arguments """
```

## Ciclo de vida do resource

Resource é uma interface de streaming, ou seja, uma vez lido você precisará abri-lo novamente.

```python script
from pprint import pprint
from frictionless import Resource

resource = Resource('data/capital-3.csv')
resource.open()
pprint(resource.read_rows())
pprint(resource.read_rows())
# We need to re-open: there is no data left
resource.open()
pprint(resource.read_rows())
# We need to close manually: not context manager is used
resource.close()
```

É possível ler dados do resource sem abrir e fechar explicitamente.

```python script
from frictionless import Resource

resource = Resource('data/capital-3.csv')
pprint(resource.read_rows())
```

## Lendo dados

A classe resource também é uma classe metadados que fornece várias funções de leitura e transmissão

```python script
from frictionless import Resource

resource = Resource('data/country-3.csv')
pprint(resource.read_bytes())
pprint(resource.read_text())
pprint(resource.read_cells())
pprint(resource.read_rows())
```

Nos casos em que a leitura do arquivo não seja possível por ser muito grande, o Frictionless fornece funções de streaming.

```python script
from frictionless import Resource

with Resource('data/country-3.csv') as resource:
    pprint(resource.byte_stream)
    pprint(resource.text_stream)
    pprint(resource.cell_stream)
    pprint(resource.row_stream)
    for row in resource.row_stream:
      print(row)
```

## Format

Também conhecido como extensão, ajuda o frictionless a escolher o analisador adequado para lidar com o arquivo.
Formatos populares são csv, xlsx, json, e outros.

```python script
from frictionless import Resource

with Resource(b'header1, header2,\nvalue1,value2.csv', format='csv') as resource:
  print(resource.format)
  print(resource.to_view())
```

## Encoding

O frictionless automaticamente detecta o codificador dos arquivos mas as vezes pode ficar impreciso, tendo que fornecer a codificação manualmente.

```python script
from frictionless import Resource

with Resource('data/country-3.csv', encoding='utf-8') as resource:
  print(resource.encoding)
  print(resource.path)
```

## Innerpath

É possível ajustar o comportamento do frictionles, que por padrão utiliza o primeiro arquivo encontrado no arquivo zip.

```python script
from frictionless import Resource

with Resource('data/country-files.zip', innerpath='country-3.csv') as dresource:
    print(resource.compression)
    print(resource.innerpath)
    print(resource.to_view())
```

## Compression

É possível ajustar a detecção de compressão fornecendo o algoritmo explicitamente.

```python script
#from frictionless import Resource

#with Resource('data/country-files.zip', compression='zip') as resource:
#  print(resource.compression)
#  print(resource.to_view())
```

## Estatísticas

```python script
from frictionless import Resource

resource = Resource('data/country-1.csv')
resource.infer(stats=True)
print(resource.stats)
```

```python script

```

```python script

```

```python script

```