---

## Extração e análise de IPv4 público com Python.

<p align="left">
  <img alt="Python 3.x" src="https://cdn.icon-icons.com/icons2/2699/PNG/512/python_logo_icon_168886.png" title="Python 3.x" width="3%">
  &nbsp
  <img alt="Abuseipdb.com" src="https://www.abuseipdb.com/img/abuseipdb-logo.svg" title="Abuseipdb.com" width="3%">
  &nbsp
  <img alt="Linux" src="https://cdn0.iconfinder.com/data/icons/flat-round-system/512/linux_tox-512.png" title="Linux" width="3%">
  &nbsp
  <img alt="Slack" src="https://cdn-icons-png.flaticon.com/512/2111/2111615.png" title="Slack" width="3%">
</p>

Nesse projeto vamos usar bibliotecas externas e internas do Python, o código será capaz de extrair endereços IPv4 de logs do sistema operacional e realizar uma consulta no histórico público desses endereços IP.

A base de dados do [Abuseipdb.com](https://www.abuseipdb.com/) será usada para pesquisa de historico do endereço IP, as consultas são realizadas via requisições autenticadas, para que esse código funcione corretamente é necessário ter uma chave de API válida do abuseipdb.

#### Dividindo o entregável.
  
  - [x] [Conseguir investigar](https://github.com/usrbinbrain/fiap-python-02/blob/main/readme/api_readme.md#controlando-a-api-do-abuseipdbcom), conectar e autenticar o código na API do abuseipdb.

  - [x] [Ter o que investigar](https://github.com/usrbinbrain/fiap-python-02/blob/main/readme/ip_readme.md#rastreando-dados-com-regex-nos-logs-do-sistema-operacional), extraindo endereços IP potencialmente maliciosos de interações do sistema operacional.

  - [x] [Conseguir apresentar](https://github.com/usrbinbrain/fiap-python-02/blob/main/readme/slack_readme.md#centraliza%C3%A7%C3%A3o-de-informa%C3%A7%C3%A3o-no-slack), agrupar resultados em um canal de mensagens.


#### Unindo o entregável.

 - [x] [Script V1](https://github.com/usrbinbrain/fiap-python-02/blob/main/readme/script_v1_readme.md#script-v1), realiza a **extração de IPv4 público** e a **consulta na API** do Abuseipdb.com.
 
 - [x] [Script V2](https://github.com/usrbinbrain/fiap-python-02/blob/main/readme/script_v2_readme.md#script-v2), permite gerar uma lista com todos os **IPv4 que possuem denúncias** e verifica apenas os logs gerados no **dia atual**.
 
 - [x] [Script V3](https://github.com/usrbinbrain/fiap-python-02/blob/main/readme/script_v3_readme.md#script-v3), adiciona a função de **centralizar as análises** do script, enviando os resultados para um **canal no slack**.

#### Tornando o entregável gerenciável.

 - [x] [Configuração automatizada](https://github.com/usrbinbrain/fiap-python-03/blob/main/README.md#configurar-um-servi%C3%A7o-no-systemd-usando-python) do [script v3](https://github.com/usrbinbrain/fiap-python-02/blob/main/readme/script_v3_readme.md#script-v3) como um serviço (**unit**) no systemd.

---

###### ✏️ Autor: [Gabriel A. Gama](https://www.linkedin.com/in/gagama/).
