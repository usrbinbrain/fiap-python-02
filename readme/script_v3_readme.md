# Script V3.

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

# Funcao envia um alerta para o slack via webhook.
def enviar_msg_slack(lista_de_ip):
    # Variavel recebe o webhook do slack.
    SLACK_WEBHOOK = "https://hooks.slack.com/services/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"
    # Variavel recebe o nome do projeto que emite o alerta.
    MSG_NOME_APP = "Python IPv4 hunter."
    # Variavel recebe um texto descrevendo o alerta.
    MSG_CONTEXTO = "IPv4 maliciosos identificados interagindo com ativo de rede."
    # Variavel recebe o hostname do servidor que esta executando o script.
    SRV_HOSTNAME = subprocess.run(["hostname"], universal_newlines=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    # Variavel recebe o titulo informando o hostname do servidor.
    MSG_TITULO = f"Servidor afetado: {SRV_HOSTNAME.stdout}"

    ## Dicionario usado no cabecalho da requisicao web de envio do alerta.
    headers = {
        "Content-Type": "application/x-www-form-urlencoded"
    }
    ## Formatando lista com links do abuseipdb.
    lista_formatada = []
    for endereco_ip in lista_de_ip:
        lista_formatada.append(f"<https://www.abuseipdb.com/check/{endereco_ip}|{endereco_ip}>")
    ## Formata cada item da lista formatada com os links, colocando um por linha.
    lista_de_ip = "\n".join(lista_formatada)

    ## Formatando o corpo do alerta enviado para o slack, seguindo o formato JSON.
    campos_msg = {
            "username": MSG_NOME_APP,
            "icon_emoji": ":warning:",
            "blocks": [
                {
                    "type": "section",
                    "text": {
                        "type": "mrkdwn",
                        "text": f"*{MSG_CONTEXTO}*"
                    }
                },
                {"type": "section",
                    "text": {
                        "type": "mrkdwn",
                        "text": f"{MSG_TITULO}\n{lista_de_ip}\n"
                        }
                    },
                {"type": "divider"
                    }
                ]
            }
    
    # Realizando o request usando o webhook do slack.
    data = f"payload={campos_msg}"
    # Variavel recebe a resposta da requisicao web.
    response = requests.post(SLACK_WEBHOOK, headers=headers, data=data)
    # Exibindo o texto da resposta da requisicao.
    print(response.text)

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

# Se a lista "LISTA_PERIGOSA" nao estiver vazia.
if LISTA_PERIGOSA != []:
    # Executar a funcao de envia msg para o slack e passar a lista "LISTA_PERIGOSA" como argumento da funcao.
    enviar_msg_slack(LISTA_PERIGOSA)
```   
