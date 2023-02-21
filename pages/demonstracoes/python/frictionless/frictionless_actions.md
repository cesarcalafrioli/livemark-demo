# Frictionless - Classe Catálogo


## Criando um catalogo

```python script
from frictionless import Catalog, Package

catalog = Catalog(packages=[Package('tables/*')])

print(catalog)
```

## Descrevendo um catálogo


```python script
# AVISO : INSTALEM pacote frictionless[ckan]
from frictionless import Catalog

catalog = Catalog('https://dados.ufpe.br/dataset/')
#print(type(catalog))
#print(catalog) # COMENTADO PORQUE É MUITO GRANDE
```

## Gerenciamento de pacotes

```python script
from frictionless import Catalog

catalog = Catalog('https://dados.ufpe.br/dataset/')
print(catalog.package_names)
#print(catalog.has_package) # COMENTADO PORQUE É MUITO GRANDE
#print(catalog.add_package)
#print(catalog.get_package)
#print(catalog.clear_packages)
```

## Salvando descritor

```python script
from frictionless import Package

catalog = Catalog('https://dados.ufpe.br/dataset/')
catalog.to_yaml('data/datacatalog-portalufpe.yaml') # Save as YAML
```