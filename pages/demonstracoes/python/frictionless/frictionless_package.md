# Frictionless - Classe Package

Package é um container simples utilizado para descrevere uma coleção coerente de dados em um único pacote. Consiste de metadados que descrevam a estrutura e conteúdos de um pacote, e também de Resources como arquivos de dados que formam os conteúdos do pacote.

O metadados do pacote é armazenado em um descritor. Além disso, o pacote irá incluir outros recursos como arquivos de dados.

## Criando um pacote

Existem diversas formas de criar um pacote

```python script
from frictionless import Package, Resource

#package = Package('data/table.csv') # from a resource path
#package = Package('tables/*') # from a resources glob
package = Package(['data/country-1.csv', 'data/country-2.csv']) # from a list
#package = Package('package/datapackage.json') # from a descriptor path
#package = Package({'resources': {'path': 'data/table.csv'}}) # from a descriptor
#package = Package(resources=[Resource(path='data/table.csv')]) # from arguments

print(package)
```

```python script
""" from frictionless import Package, Resource

package = Package(resources=[Resource(path='table.csv')]) # from arguments
package = Package('datapackage.json') # from a descriptor """
```

## Descrevendo um pacote


```python script
from frictionless import Package, Resource

package = Package(
    name='package',
    title='My Package',
    description='My Package for the Guide',
    resources=[Resource(path='data/table.csv')],
    # it's possible to provide all the official properties like homepage, version, etc
)
print(package)
```

Se você criou um pacote, por exemplo, do descritor você pode acessar essas propriedades:

```python script
from frictionless import Package

package = Package('data/country-3.package.yaml')
#package = Package('data/table.csv')
#print(package.resources)
print(package.resource_names)
print(package.resource_names[0]) # Como retorna uma lista, podemo obter o nome de cada resource conforme o índice
#print(package.resources[0]["name"]) #
# and others
```

Editando o pacote criado

```python script
from frictionless import Package

package = Package('data/country-3.package.yaml')
package.name = 'new-name'
package.title = 'New Title'
package.description = 'New Description'
# and others
print(package)
print(package.name)
print(package.title)
print(package.has_resource('country-3'))
```

## Gerenciando o resource do pacote

```python script
from frictionless import Package, Resource

# Descrevendo o pacote
package = Package('data/country-3.package.yaml')

# Imprimindo as propriedades
print("Imprimindo o resource do pacote")
print(package.resources)
print()
print("Imprimindo os nomes dos resources do pacote")
print(package.resource_names)

package.add_resource(Resource(name='new', data=[['key1', 'key2'], ['val1', 'val2']]))
resource = package.get_resource('new')

print("RESOURCE:",resource)
print(package.has_resource('new'))

package.remove_resource('new')

print(package.has_resource('name'))
```

## Salvando o descriptor

```python script
from frictionless import Package
package = Package('data/*.csv')
#package = Package(['data/country-1.csv', 'data/country-2.csv']) 
package.to_json('data/datapackage.json') # Save as JSON
package.to_yaml('data/datapackage.yaml') # Save as YAML

```