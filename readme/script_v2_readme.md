# Script V2.

```python
#!/usr/bin/env python3
import subprocess
import re
import ipaddress
import requests
API_URL = "https://api.abuseipdb.com/api/v2/check"
CHAVE_API = "ffab400e2a3bca2bc696557fe108660f7a2b6c2ca46e01ba3b18ba25337781e703c1161213c43ea6"
DIAS_BUSCADOS = 365
COMANDO_LINUX = 'journalctl -u ssh --no-pager --since today'

### Lista em branco para agrupar os enderecos IP com reports.
LISTA_PERIGOSA = []

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

            ### Salvar o ip na lista "LISTA_PERIGOSA".
            LISTA_PERIGOSA.append(IP_ALVO)

        else:
            print(f"O IP {IP_ALVO} esta integro!")
        
# Aqui o loop ja foi totalmente executado
print("O loop acabou!")
# Exibir todos os enredecos IP que foram salvos na lista.
print(LISTA_PERIGOSA)
```
