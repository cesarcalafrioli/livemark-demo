# Frictionless - CKAN

## Instalação do pacote

pip install frictionless[ckan] --pre<br>
pip install 'frictionless[ckan]' --pre # Para shel zsh

## Lendo um pacote

Os passos abaixo importam o conjunto de dados da instância CKAN como um pacote Frictionless.

```python script
from frictionless.portals import CkanControl
from frictionless import Package

ckan_control = CkanControl()
package = Package('https://legado.dados.gov.br/dataset/bolsa-familia-pagamentos', control=ckan_control) # Cria objeto pacote utilizando a URL do conjunto de dados do CKAN.
```

A segunda opção abaixo consiste em fazer a mesma coisa do código anterior, porém passando parâmetros para o controle CKAN para configurá-lo.

```python script
from frictionless import Package
import inspect

src = inspect.getsource(Package.get_resource)
print(src)

```


```python script
# Opção mais explícita
from frictionless.portals import CkanControl
from frictionless import Package

ckan_control = CkanControl(baseurl='https://legado.dados.gov.br', dataset='bolsa-familia-pagamentos') # Coloca a URL e conjunto de dados em seus respectivos parâmetros.
package = Package(control=ckan_control)
```

ou 

```python script
from frictionless.portals import CkanControl
from frictionless import Package

ckan_control = CkanControl(baseurl='https://legado.dados.gov.br')
package = Package('bolsa-familia-pagamentos', control=ckan_control)
```

## Ignorando um schema do resource.

É possível utilizar o parâmetro ignore_schema=True no Controle Ckan. Isso é útil em casos que o conjunto de dados do CKAN possua um resource cujo schemas contenham erros.

```python script
from frictionless.portals import CkanControl
from frictionless import Package

ckan_control = CkanControl(baseurl='https://legado.dados.gov.br', ignore_schema=True)
package = Package('bolsa-familia-pagamentos', control=ckan_control)

print(package.resource_names) # Listando todos os resources do package ( Conjunto de dados bolsa-familia-pagamentos)
```

## Publicando um pacote

Para publicar um pacote a uma instância CKAN é necessária uma chave da API de um usuário do CKAN que possua permissão para criar os conjuntos de dados abertos. Essa chave é passsada ao controle CKAN por meio do parâmetro apikey.

```python script
""" from frictionless.portals import CkanControl
from frictionless import Package

ckan_control = CkanControl(baseurl='https://legado.dados.gov.br', apikey='YOUR-SECRET-API-KEY')
package = Package(...) # Create your package
package.publish(control=ckan_control) """
```

## Lendo um catálogo

Também é possível baixar uma lista de conjunto de dados do CKAN usando o catálogo.

AVISO: A classe Catalog ainda está em fase experimental e, portanto, a API pode mudar sem aviso prévio.

```python script
import frictionless
from frictionless import portals, Catalog
    
ckan_control = portals.CkanControl(baseurl='https://legado.dados.gov.br')
c = Catalog(control=ckan_control)
#print(c)
```

Por padrão, o frictionless irá baixar todos os conjuntos de dados ( Packages ) da instância, limitado apenas pelo número máximo de conjunto de dados retornado pela API da instância CKAN. Assim, caso a instância retorne apenas 10 conjuntos de dados por padrão, você pode solicitar mais pacotes passando o parâmetro num_packages.

```python script
import frictionless
from frictionless import portals, Catalog
    
ckan_control = portals.CkanControl(baseurl='https://legado.dados.gov.br', num_packages=5, ignore_package_errors=True)
c = Catalog(control=ckan_control)
#print(c)

#ckan_control.to_yaml('teste.yaml')
```

Para ignorar erros individuais do pacote, utilize o parâmetro ignore_package_erros=True

```python script
import frictionless
from frictionless import portals, Catalog
    
ckan_control = portals.CkanControl(baseurl='https://legado.dados.gov.br', ignore_package_errors=True, num_packages=1000)
c = Catalog(control=ckan_control)
```

Para baixar apenas 5 pacotes, utilize o parâmetro result_offset

```python script
import frictionless
from frictionless import portals, Catalog
    
ckan_control = portals.CkanControl(baseurl='https://legado.dados.gov.br', ignore_package_errors=True, results_offset=5)
c = Catalog(control=ckan_control)
#print(c)
print(c.package_names)
```

## Buscando os conjuntos de dados de uma organização ou grupo

Para buscar todos os pacotes de uma organização, utilize o parâmetro organization_name.

```python script
import frictionless
from frictionless import portals, Catalog

ckan_control = portals.CkanControl(baseurl='https://legado.dados.gov.br', organization_name='agencia-espacial-brasileira-aeb')
c = Catalog(control=ckan_control)

#print(c)
```

para baixar todos os conjuntos de dados do grupo CKAN, utilize o parâmetro group_id ao controle CKAN.

```python script
import frictionless
from frictionless import portals, Catalog
    
ckan_control = portals.CkanControl(baseurl='https://legado.dados.gov.br', group_id='ciencia-informacao-e-comunicacao')
c = Catalog(control=ckan_control)
```

## Usando pesquisa CKAN

você também pode retornar apenas conjuntos de dados que são retornado conforme o método de pesquisa do pacote ckan. Utilize o parâmetro search.

```python string
import frictionless
from frictionless import portals, Catalog
    
ckan_control = portals.CkanControl(baseurl='https://legado.dados.gov.br', search={'q': 'name:bolsa*'})
c = Catalog(control=ckan_control)

print(c)
```