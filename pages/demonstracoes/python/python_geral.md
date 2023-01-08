# PYTHON - GERAL

Essa pagina demonstra a execução dos scripts python dentro dos documentos markdown utilizando as tasks para armazenar o script e depois
executa-lo por meio do comando "livemark run".

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

```python script
for number in range(1, 6):
    print(f'Hello World #{number}!')
```

```python task id=data-transform
print('Data Transform')
```
```python task id=data-load
print('Data Load')
```

```python task id=test-code
print('Test Code')
```

```python task id=example
print('It is a task')
```

OUTRO TESTE

```bash script
python scripts/teste.py
```