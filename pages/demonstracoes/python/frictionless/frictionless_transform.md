# FRICTIONLESS - TRANSFORM

Transformar dados no frictionless consiste modificar os dados e metadados de um estado para o outro. Dessa forma, podemos transformar um arquivo .xlsx poluído em um arquivo .csv corrigido, por exemplo.

Comparando com o Pandas, o Frictionless oferece melhor controle sobre metadados, além de possuir um API modular, e possuir suporte completo às especificicações desse framework.

O Frictionless é também um framework streaming que possui uma habilidade de trabalhar com dados maiores. Como desvantagem da sua arquitetura, o processo pode se tornar mais lento em comparação com outros pacotes ( incluindo Pandas ).

## Princípios da transformação

1 - Simplicidade Conceitual<br>
2 - Metadados importam -> Comparado com outros metadados, o frictionless prioriza os metadados.<br>
3 - Transmissão de dados -> Nos casos em que os dados são muito grandes para serem lidos na memória, o fricionless transmite os dados.<br>
4 - Avaliação preguiçosa -> As manipulaçoes de dados no frictionless acontecem sob-demanda.<br>
5 - Reuso de software -> <br>

## Funções transform

Podemos tanto transformar os resources e packages quanto trnasformar baseado no pipeline declarativo.

## Transformando um resource

Transformando um arquivo de dados definindo o resource, aplicando as transformações baseado no pipeline. Enquanto as transformações do resource e package são imperativas, as transformações baseadas em pipeline podem ser criadas antecipadamente ou compartilhadas como arquivo JSON.

```python script
from frictionless import Resource, Pipeline, steps

# Criando o resource
source = Resource(path="data/transform.csv")

# Criando o pipeline para adicionar uma coluna chamada cars cujo tipo de dados é inteiro e o seu valor é o dobro da coluna população
pipeline = Pipeline(steps=[
    steps.table_normalize(), # Lança os tipos de dados e modela a tabela de acordo com o schema, seja deduzido ou fornecido. Aqui o recurso da CPU será utilizado para lançar os dados
    steps.field_add(name="cars", formula="population*2", descriptor={'type': 'integer'}) # Adiciona um campo ao dados e metadados baseado na informação fornecida pelo usuário
])

# Aplicar o pipeline do transform
target = source.transform(pipeline)

# Imprimindo o resultado em forma de schema e de dados
print(target.schema)
print(target.to_view())
```

## Transformando um pacote

Como um pacote consiste em um conjunto de recursos ( resources ), transformar um pacote significa adicionar ou remover resources, bem como transformá-los. O processo de transformação do package é similar ao do resource.

```python script
from frictionless import Package, Resource, transform, steps

# Definindo o pacote
source = Package(resources=[Resource(name='main', path="data/transform.csv")])

# Criando o pipeline
pipeline = Pipeline(steps=[
    steps.resource_add(name="extra", descriptor={"data": [['id', 'cars'], [1, 166], [2, 132], [3, 94]]}), # Adicionando um resource de nome extra e seu descritor ( Coluna e valor )
    steps.resource_transform(
        name="main",
        steps=[
            steps.table_normalize(),
            steps.table_join(resource="extra", field_name="id"),
        ],
    ), # Fazendo a transformação
    steps.resource_remove(name="extra"),
])

# Aplicando as mundanças
target = source.transform(pipeline)

# Imprimindo os resultados na forma de schema, dados e resources
print(target.resource_names)
print(target.get_resource("main").schema)
print(target.get_resource("main").to_view())

# TESTE. Causa um erro pois é tratado como um resource inexistente
#print(target.get_resource("extra").schema)
#print(target.get_resource("extra").to_view())
```

## Transformando um pipeline

Pipeline é um meio comprobatório de escrever os passos para a transformação dos metadados. É possível transformar, com o pipeline, resources, packages, ou escrever plugins customizados.

```python script
from frictionless import Pipeline, transform

pipeline = Pipeline.from_descriptor({
    "steps": [
        {"type": "table-normalize"},
        {
            "type": "field-add",
            "name": "cars",
            "formula": "population*2",
            "descriptor": {"type": "integer"}
        },
    ],
})
print(pipeline)
```

## Passos disponíveis

Frictionless inclui mais de 40 passos prontos de transformação agrupados por objeto. Os grupos disponíveis são:

- Resource
- Table
- Field
- Row
- Cell

Você também pode customizar seus próprios passos

## Passos customizados

Podemos customizar a transformação por meio das funções python

```python script
from frictionless import Package, Resource, Step, transform, steps

class custom_step(Step):
    def transform_resource(self, resource):
        current = resource.to_copy()

        # Data
        def data():
            with current:
                for list in current.cell_stream:
                    yield list[1:] # Remove um campo da tabela

        # Meta
        resource.data = data
        resource.schema.remove_field("id") # Remove o campo id

source = Resource("data/transform.csv")
pipeline = Pipeline(steps=[custom_step()])
target = source.transform(pipeline)
print(target.schema)
print(target.to_view())
```

## Trabalhando com PETL

Nos casos em que é necessário utilizar API Low-level para alcançar objetivos, o resource pode ser exportado como uma tabela PETL. PETL é um pacote python para extração, transformação e carregamento. A transformação PETL faz o uso mínimo de memória e pode escalar até milhões de linha CASO A VELOCIDADE NÃO SEJA PRIORIDADE. Ao trabalhar como conjuntos de dados muito grande e/ou aplicações de performance crítica é aconselhável outros pacotes, como o PANDAS. 

Lembrando que para dados muito grandes é aconselhável o stream de dados

```python script
from frictionless import Resource

resource = Resource(path='data/transform.csv')
petl_table = resource.to_petl()
# Use it with PETL framework
print(petl_table)
```