# Frictionless - sandbox

```python script
from frictionless import Catalog
import urllib.request

catalog = Catalog('https://dados.ufpe.br/dataset/')

# Obtem metadados do pacote
print(catalog.get_package('boletim-oficial'))

# Informa se tem ou não tem determinado pacote ( Conjunto de dados)
print(catalog.has_package('boletim-oficial'))

catalog.to_yaml('data/boletin-oficial.yaml')

# Obtendo informações separadas do conjunto de dados boletim-oficial
print(catalog.get_package('boletim-oficial').name)
print(catalog.get_package('boletim-oficial').description)
# Lista dos resources do conjunto de dados
print(catalog.get_package('boletim-oficial').resources) # Isso não tem na documentação. Utilizando apenas a dedução lógica

# Obtendo o caminho do arquivo "boletim_oficial_2021_ufpe.csv"
print(catalog.get_package('boletim-oficial').resources[1].path)

# Total de arquivos no package
print(len(catalog.get_package('boletim-oficial').resources))

# Listanto todos os arquivos do conjunto de dados
for w in range(len(catalog.get_package('boletim-oficial').resources)):
    print(catalog.get_package('boletim-oficial').resources[w].name)

# A mesma coisa do trecho acima, só que dentro de uma list comprehension
dataset_files = [catalog.get_package('boletim-oficial').resources[w].name for w in range(len(catalog.get_package('boletim-oficial').resources))]

print(dataset_files)

# Obtendo a url dos arquivos do conjunto de dados
for w in range(len(catalog.get_package('boletim-oficial').resources)):
    print(catalog.get_package('boletim-oficial').resources[w].path)

# Baixando os arquivos do conjunto de dados
""" for w in range(len(catalog.get_package('boletim-oficial').resources)):
    urllib.request.urlretrieve(catalog.get_package('boletim-oficial').resources[w].path, 'data/'+catalog.get_package('boletim-oficial').resources[w].name) """
```

## TREINO COM CKANCONTROL

Parece que a técnica abaixo é muito parecida ( para não dizer totalmente ) com a da classe Catalog

```python script
from frictionless.portals import CkanControl
from frictionless import Package

ckan_control = CkanControl(baseurl='https://dados.ufpe.br', dataset='boletim-oficial')
package = Package(control=ckan_control)

# Imprimindo todo o conteúdo do conjunto de dados boletim-oficial
print(package)

# 
print(package.get_resource('boletim-oficial-2020-ufpe-csv').name)
```
