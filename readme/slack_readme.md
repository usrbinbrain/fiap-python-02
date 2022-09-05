# Centralização de informação no Slack.

<p align="left">
  <img alt="Python 3.x" src="https://cdn.icon-icons.com/icons2/2699/PNG/512/python_logo_icon_168886.png" title="Python 3.x" width="3%">
  &nbsp
  <img alt="Slack" src="https://cdn-icons-png.flaticon.com/512/2111/2111615.png" title="Slack" width="3%">
</p>

### Do terminal para o Slack. 📢

<details><summary>...</summary>
    
A capacidade de reporta a análise é o elemento que falta para que esse script python se torne uma solução, mas temos que obter uma forma de entrega de mensagens que cumpra algums requisitos:
  
  - **Histórico de mensagens salvo na nuvem**.
  - **Software de mensagens acessível em diversas plataformas**.
  - **Não precise de investimento**.

Para isso vamos usar o [Slack](https://slack.com/intl/pt-br/downloads), que pode ser instalado em sistemas **Linux**, **MacOS**, **Windows**, **Android**, **IOS** e também conta com a **versão web**, no Slack é possível gerar uma forma de enviar mensagem (**[webhooks](https://slack.com/intl/pt-br/help/articles/115005265063-Webhooks-de-entrada-para-o-Slack#configurar-webhooks-de-entrada)**) de forma programática e sem nenhum custo.

No trecho de código abaixo vamos usar a biblioteca **`requests`** para enviar uma mensagen [formatada](https://app.slack.com/block-kit-builder/T02FMJ8NMKM#%7B%22blocks%22:%5B%7B%22type%22:%22section%22,%22text%22:%7B%22type%22:%22plain_text%22,%22emoji%22:true,%22text%22:%22Its%20fine!%20This%20is%20a%20test!%22%7D%7D%5D%7D) para um canal do Slack usando o **`webhook`** gerado para integração **incoming-webhook**, usando a biblioteca **`subprocess`** o **hostname do servidor** é obtido para ser informado no título do alerta enviado ao canal do Slack, permitindo identificar o **servidor de origem** do alerta.

```python
#!/usr/bin/env python3
import subprocess
import requests

# Funcao envia um alerta para o slack via webhook.
def enviar_msg_slack(lista_de_ip):
    # Variavel recebe o webhook do slack.
    SLACK_WEBHOOK = "https://hooks.slack.com/services/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I"
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

    ## Formatando o corpo do alerta enviado para o slack, observe que o formato JSON define os campos onde cada informacoa deve ser exibida.
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
                        "text": f"{MSG_TITULO}\n\n{lista_de_ip}\n"
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

################################################################
############   USANDO FUNCAO E ENVIANDO MSG PARA O SLACK.
################################################################

# Declarando uma lista de teste.
l = ["8.8.8.8"]
# Chamando a funcao e passando a lista de teste (l) como argumento.
enviar_msg_slack(l)
```
    
</details>

### Mensagem do Slack. 📑

<details><summary>...</summary>

O corpo do alerta enviado para o Slack tem as seguintes informações.

 - [x] Nome do projeto que gera os alertas.
 - [x] Descrição do tipo de alerta.
 - [x] Hostname do servidor afetado.
 - [x] Lista de endereços IPv4 públicos analisados e identificados como maliciosos (cada IP tem um link para sua página no abuseipdb).


<p align="center">
  <img alt="Alerta Slack" src="https://user-images.githubusercontent.com/66579913/188254661-ae96f507-7f95-4dce-ac9b-89b158888796.png" title="Alerta Slack" width="65%">
</p>

</details>

---
