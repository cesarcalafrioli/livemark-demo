# BASH - BASICO

Essa página demonstra a execução dos scripts python dentro dos documentos markdown utilizando as tasks para armazenar o script e depois
executá-lo por meio do comando "livemark run".

Segue o exemplo abaixo:

````
```python task id=example
print('It is a task')
```
````

````
```bash script
livemark run example
```
````

```bash task id=data-extract
echo 'Data Extract'
```

```python task id=hello-world
name = "Cesar Calafrioli"
print('Hello World. My name is {}'.format(name))
```

```bash script
livemark run hello-world
```

```bash task id=test-link
echo 'Test Link'
```

```bash script
python scripts/teste.py
```