# Controlando a API do Abuseipdb.com

<p align="left">
  <img alt="Python 3.x" src="https://cdn.icon-icons.com/icons2/2699/PNG/512/python_logo_icon_168886.png" title="Python 3.x" width="3%">
  &nbsp
  <img alt="Abuseipdb.com" src="https://www.abuseipdb.com/img/abuseipdb-logo.svg" title="Abuseipdb.com" width="3%">
</p>

### Requisição autenticada na API. 🗝️

<details><summary>...</summary>
    
Para que o código Python possa se conectar na API do abuseipdb, vamos usar a biblioteca externa `requests`, usada para solicitações **HTTP**(**S**), também vamos usar a bibilioteca nativa `json` que é um encoder/decoder que permite fazer intercâmbio de dados no formato **JSON** (**JavaScript Object Notation**).

Para instalar a biblioteca externa `requests` é necessário ter o gerenciador de pacotes **pip3** (**python3-pip**) no seu sistema operacional, você pode executar a linha de comando abaixo para que ambos.
    
```bash
apt-get update && apt-get install python3-pip -y && pip3 install requests
```

O trecho de código abaixo realiza a requisição autenticada na [API](https://docs.abuseipdb.com/#check-endpoint) do [abuseipdb](https://www.abuseipdb.com/) e exibe o conteúdo respondido.

```python
#!/usr/bin/env python3
# Essa biblioteca (nativa) nos permite formatar o a resposta da API para exibicao.
import json
# Essa biblioteca (externa) nos permite fazer requisicoes web e nos conectar com servidores na internet.
import requests

### Declarando as variaveis de configuracao.
# URL (endpoint) da API do abuseipdb.
API_URL = "https://api.abuseipdb.com/api/v2/check"
# Chave da API criada no abuseipdb.
CHAVE_API = "zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz"
# Quantidade de dias na busca do abuseipdb.
DIAS_BUSCADOS = 365
# Variavel que recebe o IP.
IP_ALVO = '8.8.8.8'

### Interagindo via web com a API usando os dados das variaveis.
# Dicionario contendo o endereco IP e a quantidade de dias da busca.
querystring = {'ipAddress': IP_ALVO, 'maxAgeInDays': DIAS_BUSCADOS}
# Dicionario contendo o padrao de comunicacao e a chave da API.
headers = {'Accept': 'application/json', 'Key': CHAVE_API}
# Variavel contendo a resposta da API.
resposta_api = requests.get(API_URL, headers=headers, params=querystring).json()

#### Exibindo o conteudo completo da resposta da API do abuseipdb.
print(json.dumps(resposta_api, indent=4, sort_keys=True))
```
    
</details>

### Tratando os códigos HTTP. 🔄

<details><summary>...</summary>

Os códigos das requisições http devem ser tradados para permitir a criação de instruções para cada tipo de resposta possível.

Aqui o script só vai prosseguir se o **código http respondido** pela API for `200`, que indica que a requisição foi bem sucedida.

```python
#!/usr/bin/env python3
# Essa biblioteca (nativa) nos permite formatar o a resposta da API para exibicao.
import json
# Essa biblioteca (externa) nos permite fazer requisicoes web e nos conectar com servidores na internet.
import requests

### Declarando as variaveis de configuracao.
# URL (endpoint) da API do abuseipdb.
API_URL = "https://api.abuseipdb.com/api/v2/check"
# Chave da API criada no abuseipdb.
CHAVE_API = "zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz"
# Quantidade de dias na busca do abuseipdb.
DIAS_BUSCADOS = 365
# Variavel que recebe o IP.
IP_ALVO = '8.8.8.8'

### Interagindo via web com a API usando os dados das variaveis.
# Dicionario contendo o endereco IP e a quantidade de dias da busca.
querystring = {'ipAddress': IP_ALVO, 'maxAgeInDays': DIAS_BUSCADOS}
# Dicionario contendo o padrao de comunicacao e a chave da API.
headers = {'Accept': 'application/json', 'Key': CHAVE_API}
# Variavel contendo a resposta da API.
resposta_api = requests.get(API_URL, headers=headers, params=querystring)
    
# Se o request retornar algum codigo que nao seja 200.
if resposta_api.status_code != 200:
    # Exiba esse padrao de texto.
    print("[!] Algo deu errado no request, verifique a chave da API cadastrada no script!")
    # Encerre o script
    exit()
# Caso o request retorne o codigo 200.
else:
    # A variavel 'resposta_api' vai conter os dados em formato json.
    resposta_api = resposta_api.json()

#### Exibindo o conteudo completo da resposta da API do abuseipdb.
print(json.dumps(resposta_api, indent=4, sort_keys=True))
```

</details>

### Manipulando dados da resposta. 🕵️

<details><summary>...</summary>

Agora podemos salvar determinados dados da resposta em variáveis, essa seleção de dados é feita obedecendo a **estrutura** dos dados da resposta.

Para selecionar um determinado dado na resposta da API, podemos usar a mesma forma de como selecionamos um dado em uma estrutura de **dicionários** e **listas**.

```python
#!/usr/bin/env python3
# Essa biblioteca (nativa) nos permite formatar o a resposta da API para exibicao.
import json
# Essa biblioteca (externa) nos permite fazer requisicoes web e nos conectar com servidores na internet.
import requests

### Declarando as variaveis de configuracao.
# URL (endpoint) da API do abuseipdb.
API_URL = "https://api.abuseipdb.com/api/v2/check"
# Chave da API criada no abuseipdb.
CHAVE_API = "zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz"
# Quantidade de dias na busca do abuseipdb.
DIAS_BUSCADOS = 365
# Variavel que recebe o IP.
IP_ALVO = '8.8.8.8'

### Interagindo via web com a API usando os dados das variaveis.
# Dicionario contendo o endereco IP e a quantidade de dias da busca.
querystring = {'ipAddress': IP_ALVO, 'maxAgeInDays': DIAS_BUSCADOS}
# Dicionario contendo o padrao de comunicacao e a chave da API.
headers = {'Accept': 'application/json', 'Key': CHAVE_API}
# Variavel contendo a resposta da API.
resposta_api = requests.get(API_URL, headers=headers, params=querystring)
    
# Se o request retornar algum codigo que nao seja 200.
if resposta_api.status_code != 200:
    # Exiba esse padrao de texto.
    print("[!] Algo deu errado no request, verifique a chave da API cadastrada no script!")
    # Encerre o script
    exit()
# Caso o request retorne o codigo 200.
else:
    # A variavel 'resposta_api' vai conter os dados em formato json.
    resposta_api = resposta_api.json()

#### Exibindo o conteudo completo da resposta da API do abuseipdb.
print(json.dumps(resposta_api, indent=4, sort_keys=True))

#### Obtendo dados da resposta da API.
# Selecionando a quantidade de reports na resposta da API.
total_reports = resposta_api["data"]["totalReports"]
# Selecionando o pais do IP na resposta da API.
pais_ip = resposta_api["data"]["countryCode"]
# Selecionando o dominio do IP na resposta da API.
domain_ip = resposta_api["data"]["domain"]

#### Exibindo um padrao de texto com os dados obtidos.
print(f"O IP {IP_ALVO} [{domain_ip}] ({pais_ip}) possui {total_reports} reports!")
```

</details>

### Decisão lógica com os dados. 📋

<details><summary>...</summary>

Como já temos o fluxo dos dados da resposta da API do Abuseidb, podemos remover a biblioteca `json` e as linhas que estavamos usando para exibir informações de orientação.

No trecho de código abaixo o python verifica se o IP tem determinada quantidade de relatos de atividade maliciosa, diferentes padrões de textos são exibidos caso o IP tenha ou não denúncias na base de dados do Abuseipdb.

```python
#!/usr/bin/env python3
# Essa biblioteca (externa) nos permite fazer requisicoes web e nos conectar com servidores na internet.
import requests

### Declarando as variaveis de configuracao.
# URL (endpoint) da API do abuseipdb.
API_URL = "https://api.abuseipdb.com/api/v2/check"
# Chave da API criada no abuseipdb.
CHAVE_API = "zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz"
# Quantidade de dias na busca do abuseipdb.
DIAS_BUSCADOS = 365
# Variavel que recebe o IP.
IP_ALVO = '8.8.8.8'

### Interagindo via web com a API usando os dados das variaveis.
# Dicionario contendo o endereco IP e a quantidade de dias da busca.
querystring = {'ipAddress': IP_ALVO, 'maxAgeInDays': DIAS_BUSCADOS}
# Dicionario contendo o padrao de comunicacao e a chave da API.
headers = {'Accept': 'application/json', 'Key': CHAVE_API}
# Variavel contendo a resposta da API.
resposta_api = requests.get(API_URL, headers=headers, params=querystring)
    
# Se o request retornar algum codigo que nao seja 200.
if resposta_api.status_code != 200:
    # Exiba esse padrao de texto.
    print("[!] Algo deu errado no request, verifique a chave da API cadastrada no script!")
    # Encerre o script
    exit()
# Caso o request retorne o codigo 200.
else:
    # A variavel 'resposta_api' vai conter os dados em formato json.
    resposta_api = resposta_api.json()

#### Obtendo dados da resposta da API.
# Selecionando a quantidade de reports na resposta da API.
total_reports = resposta_api["data"]["totalReports"]
# Selecionando o pais do IP na resposta da API.
pais_ip = resposta_api["data"]["countryCode"]
# Selecionando o dominio do IP na resposta da API.
domain_ip = resposta_api["data"]["domain"]

#### Se a quantidade de reports do IP pesquisado for maior que zero.
if total_reports > 0:
    # Exiba esse formato de texto com os dados coletados do IP malicioso.
    print(f"O IP {IP_ALVO} ({domain_ip} {pais_ip}) possui {total_reports} reports!")

#### Se a quantidade de reports for menor ou igual a zero.
else:
    # Exiba esse outro padrao de texto apenas com o endereco IP.
    print(f"O IP {IP_ALVO} esta integro!")
```

Assim temos o código python que realiza os seguintes passos:

- [x] Consultas autenticadas na API do Abuseipdb.
- [x] Identifica problemas com a chave da API.
- [x] Verifica o histórico malicioso de IPv4 público.
- [x] Diferencia endereços IPv4 com e sem histórico malicioso.
    
</details>

---
