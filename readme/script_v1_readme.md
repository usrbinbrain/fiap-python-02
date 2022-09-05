# Script V1.

```python

#!/usr/bin/env python3
# Biblioteca (nativa) que permite o Python interagir com o sistema operacional.
import subprocess
# Biblioteca (nativa) que permite identificar padroes em caracteres.
import re
# Biblioteca (nativa) que permite trabalhar com enderecamento IPv4 e IPv6.
import ipaddress
# Essa biblioteca (externa) nos permite fazer requisicoes web e nos conectar com servidores na internet.
import requests

### Declarando as variaveis de configuracao.
# URL (endpoint) da API do abuseipdb.
API_URL = "https://api.abuseipdb.com/api/v2/check"
# Chave da API criada no abuseipdb.
CHAVE_API = "fffab400e2a3bca2bc696557fe108660f7a2b6c2ca46e01ba3b18ba25337781e703c1161213c43ea6"
# Quantidade de dias na busca do abuseipdb.
DIAS_BUSCADOS = 365

# Comando Linux para buscar os logs do SSH em qualquer sistema operacional que tenha o systemd (gerenciador de sistemas/serviços).
COMANDO_LINUX = 'journalctl -u ssh --no-pager'
# Executa o comando no sistema operacional e sefine os possiveis fluxos de respostas que o sistema operacional pode retornar.
resultado_comando = subprocess.run(COMANDO_LINUX.split(), universal_newlines=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
# Exibe o resultado do comando executado no sistema operacional.
#print(resultado_comando.stdout)

# O regex gera uma lista com os enderecos IP extraidos dos logs do journalctl.
dados_regex = re.findall(r"^.*\ Failed.*\ (\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})", resultado_comando.stdout, re.M)
# Exibindo o conteudo capturado pelo regex.
#print(dados_regex)

# Criando uma lista sem endereços IP duplicados.
lista_ip_sem_duplicados = list(dict.fromkeys(dados_regex))
# Exibindo lista sem endereços IP duplicados.
#print(lista_ip_sem_duplicados)

# Para cada um dos enderecos IP da lista 'lista_ip_sem_duplicados' realize o codigo dentro do loop 'for'.
for IP_ALVO in lista_ip_sem_duplicados:
    
    # Exibindo padrao de texto com cada IP que entra no loop.
    #print(f"Precisamos identificar o IP {IP_ALVO}")
    
    # Se o IP for publico, realize o codigo dentro do 'if'.
    if ipaddress.ip_address(IP_ALVO).is_global:
        print(f"IP publico elegivel ao request na API -> {IP_ALVO}")
        
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

<details><summary>Código sem as linhas de comentários e exibição de variáveis.</summary>

```python
#!/usr/bin/env python3
import subprocess
import re
import ipaddress
import requests
API_URL = "https://api.abuseipdb.com/api/v2/check"
CHAVE_API = "ffab400e2a3bca2bc696557fe108660f7a2b6c2ca46e01ba3b18ba25337781e703c1161213c43ea6"
DIAS_BUSCADOS = 365
COMANDO_LINUX = 'journalctl -u ssh --no-pager'
resultado_comando = subprocess.run(COMANDO_LINUX.split(), universal_newlines=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
dados_regex = re.findall(r"^.*\ Failed.*\ (\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})", resultado_comando.stdout, re.M)
lista_ip_sem_duplicados = list(dict.fromkeys(dados_regex))
for IP_ALVO in lista_ip_sem_duplicados:
    if ipaddress.ip_address(IP_ALVO).is_global:
        querystring = {'ipAddress': IP_ALVO, 'maxAgeInDays': DIAS_BUSCADOS}
        headers = {'Accept': 'application/json', 'Key': CHAVE_API}
        resposta_api = requests.get(API_URL, headers=headers, params=querystring)
        if resposta_api.status_code != 200:
            print("[!] Algo deu errado no request, verifique a chave da API cadastrada no script!")
            exit()
        else:
            resposta_api = resposta_api.json()
        total_reports = resposta_api["data"]["totalReports"]
        pais_ip = resposta_api["data"]["countryCode"]
        domain_ip = resposta_api["data"]["domain"]
        if total_reports > 0:
            print(f"O IP {IP_ALVO} ({domain_ip} {pais_ip}) possui {total_reports} reports!")
        else:
            print(f"O IP {IP_ALVO} esta integro!")           
```
  
</details>
