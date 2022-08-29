## Extração e análise de IPv4 com Python.

<p align="left">
  <img alt="Python 3.x" src="https://cdn.icon-icons.com/icons2/2699/PNG/512/python_logo_icon_168886.png" title="Python 3.x" width="3%">
  &nbsp
  <img alt="Abuseipdb.com" src="https://www.abuseipdb.com/img/abuseipdb-logo.svg" title="Abuseipdb.com" width="3%">
  &nbsp
  <img alt="Linux" src="https://cdn0.iconfinder.com/data/icons/flat-round-system/512/linux_tox-512.png" title="Linux" width="3%">
  &nbsp
  <img alt="Slack" src="https://cdn-icons-png.flaticon.com/512/2111/2111615.png" title="Slack" width="3%">
</p>

Esse projeto propõe extrair e identificar endereços IP (públicos) que interagiram com o serviço SSH de um servidor Linux (**systemd**), a base de dados do [Abuseipdb.com](https://www.abuseipdb.com/) será usada para pesquisa de histórico do endereço IP, as consultas são realizadas via requisições autenticadas, para que esse código funcione corretamente é necessário ter uma chave de API válida do abuseipdb.

Nesse projeto vamos usar bibliotecas externas e internas do Python, o código será capaz de extrair endereços IPv4 de logs do sistema operacional e realizar uma consulta no histórico público desses endereços IP.

#### Dividindo o entregável.
  
  - [x] [Conseguir investigar](https://github.com/usrbinbrain/fiap-python-02/blob/main/readme/api_readme.md#controlando-a-api-do-abuseipdbcom), conectar e autenticar o código na API do abuseipdb.

  - [x] [Ter o que investigar](https://github.com/usrbinbrain/fiap-python-02/blob/main/readme/ip_readme.md#rastreando-dados-com-regex-nos-logs-do-sistema-operacional), extraindo endereços IP potencialmente maliciosos de logs do sistema operacional.

  - [ ] Conseguir apresentar, agrupar resultados em um canal de mensagens.
  
