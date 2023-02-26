# Frictionless - Detector

O detector pode ser utilizado em vários lugares dentro do framework.

```python script
from frictionless import Detector, Resource

detector = Detector(field_missing_values=['1','2'])
resource = Resource('data/table.csv', detector=detector)
print(resource.read_rows())
```

## Uso do Detector

As instancias de classe do detector são aceitas por muitas classes e funções:

- Package
- Resource
- Describe
- Extract
- Validate
- Etc...

Basta criar uma instância do detector usando opções desejadas e passar para a classe e função.

## Tamanho do Buffer

Por padrão, o frictionless irá utilizar os primeiros 10.000 bytes para detectar codificação. Incluir mais bytes aumentando o tamanho do buffer pode melhorar a inferência. Entretanto, o processo ficará mais lento, mas a detecção da codificação será mais preciso.

```python script
from frictionless import Detector, describe

detector = Detector(buffer_size=100000)
resource = describe("data/country-1.csv", detector=detector)
print(resource.encoding)
```

## Tamanho da amostra

Por padrão, o Frictionless irá utilizar as primeiras 100 linhas para detectar o tipo de campo. Assim como o buffer, aumentando o tamanho da amostra irá melhorar a inferência, mas o processo irá ficar mais lento, embora o resultado se torne mais preciso.

```python script
from frictionless import Detector, describe

detector = Detector(sample_size=1000)
resource = describe("data/country-1.csv", detector=detector)
print(resource.schema)
```

## A função encoding

Por padrão, a função encoding_function do frictionless é None e o usuário pode usar para funções encoding prontas. O usuário, porém, possui opção para implementar sua própria decodificação utilizando este recurso.

```python script
from frictionless import Detector, Resource

detector = Detector(encoding_function=lambda sample: "utf-8")
with Resource("data/table.csv", detector=detector) as resource:
    print(resource.encoding)
```

## Tipo de campo

Permite configurar manualmente todos os tipos de campos a um tipo dado. É útil quando precisa pular o lançamento dos dados ( atribuindo tipo "any" ) ou ter tudo como um string( atribuindo tipo "string" )

```python script
from frictionless import Detector, describe

detector = Detector(field_type='any')
resource = describe("data/country-1.csv", detector=detector)
print(resource.schema)
```

## Nomes dos campos