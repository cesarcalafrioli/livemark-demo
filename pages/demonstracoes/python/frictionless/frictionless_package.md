# Frictionless - Classe Package

Existem diversas formas de criar um pacote

```python script
""" from frictionless import Package, Resource

package = Package('table.csv') # from a resource path
package = Package('tables/*') # from a resources glob
package = Package(['tables/chunk1.csv', 'tables/chunk2.csv']) # from a list
package = Package('package/datapackage.json') # from a descriptor path
package = Package({'resources': {'path': 'table.csv'}}) # from a descriptor
package = Package(resources=[Resource(path='table.csv')]) # from arguments """
```

```python script
""" from frictionless import Package, Resource

package = Package(resources=[Resource(path='table.csv')]) # from arguments
package = Package('datapackage.json') # from a descriptor """
```

## Descrevendo um pacote

```python script
""" from frictionless import Package, Resource

package = Package(
    name='package',
    title='My Package',
    description='My Package for the Guide',
    resources=[Resource(path='table.csv')],
    # it's possible to provide all the official properties like homepage, version, etc
)
print(package) """
```

Se você criou um pacote, por exemplo, do descritor você pode acessar essas propriedades:

```python script
""" from frictionless import Package

package = Package('datapackage.json')
print(package.name)
# and others """
```

```python script
""" from frictionless import Package

package = Package('datapackage.json')
package.name = 'new-name'
package.title = 'New Title'
package.description = 'New Description'
# and others
print(package) """
```

## Resource Management

```python script
""" from frictionless import Package, Resource

package = Package('data/datapackage.json')
print(package.resources)
print(package.resource_names)
package.add_resource(Resource(name='new', data=[['key1', 'key2'], ['val1', 'val2']]))
resource = package.get_resource('new')
print(package.has_resource('new'))
package.remove_resource('new') """
```

## Salvando o descriptor

```python script
""" from frictionless import Package
package = Package('tables/*')
package.to_json('datapackage.json') # Save as JSON
package.to_yaml('datapackage.yaml') # Save as YAML """
```