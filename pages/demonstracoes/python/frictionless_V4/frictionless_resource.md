# FRICTIONLESS - GUIA SOBRE O RESOURCE

O Resource descreve um recurso de dados como um arquivo individual ou tabela. A classe Resource é a classe mais importante do Framework Frictionless.

```python script
# Criando o resource
from frictionless import Resource

# Criando o resource do arquivo csv
resource = Resource('data/table.csv')

# OUTROS EXEMPLOS
#resource = Resource('data/resource.json') # Criando o resource do descriptor
#resource = Resource({'path': 'data/table.csv'}) # Criando resource por meio do dicionario
#resource = Resource(path='data/table.csv') # Criando resource utilizando parametro path
#resource = Resource(descriptor='data/resource.json') # Criando resource utilizando parametro descriptor
```

## Descrevendo resource

```python script
from frictionless import Resource

resource = Resource(
    path='data/table.csv',
    name='resource', # Nome do resource
    title='My Resource', # Titulo no resource
    description='My Resource for the Guide', # Descricao do resource
)

# Salvando o resource
resource.to_yaml('data/table.resource.yaml')

# ALTERNATIVA - SALVAR NO FORMATO JSON
# resource.to_json('data/table.resource.json')
```

Ao criar um resource voce pode acessar suas propriedades e tambem edita-las

```python script
from frictionless import Resource

resource = Resource('data/table.resource.yaml')

print(f'Nome do resource: {resource.name}')
print(f'Titulo do resource: {resource.title}')
print(f'Descricao do resource: {resource.description}')

print("")
resource.name = 'new-name'
resource.title = 'New Title'
resource.description = 'New Description'

print(resource)
```

Apos editar as propriedades, salve no formato yaml ou json ( se preferir )

Mais informacoes sobre propriedades da classe Resource [aqui](https://specs.frictionlessdata.io/data-resource/#metadata-properties)

## Ciclo de vida do Resource

https://v4.framework.frictionlessdata.io/docs/guides/framework/resource-guide#resource-lifecycle

O resource é uma interface de streaming, logo, uma vez lido voce ira precisar abri-lo novamente. Em contrapartida, voce pode ler dados de um resource sem abrir e fecha-lo explicitamente.

```python script
from pprint import pprint
from frictionless import resource

resource = Resource('data/capital-3.csv')
pprint(resource.read_rows())
```

## Lendo dados

A classe Resource tambem e uma classe metadata que fornece varias funcoes de read e stream. A classe resource pode fazer a mesma atividade da funcao extract ( que sempre le linhas na memoria ), porem tambem oferece a escolha de exibir os dados.

https://v4.framework.frictionlessdata.io/docs/guides/framework/resource-guide#reading-data