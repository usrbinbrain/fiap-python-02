# Rastreando dados com regex nos logs do sistema operacional.

<p align="left">
  <img alt="Python 3.x" src="https://cdn.icon-icons.com/icons2/2699/PNG/512/python_logo_icon_168886.png" title="Python 3.x" width="3%">
  &nbsp
  <img alt="Linux" src="https://cdn0.iconfinder.com/data/icons/flat-round-system/512/linux_tox-512.png" title="Linux" width="3%">
</p>

### Executando comandos Linux com Python. 💻

<details><summary>...</summary>
  
Para obter os endereços IPv4 públicos para análise, vamos usar as bibliotecas nativas `subprocess` que permite o python gerar novos processos e crie fluxos de entrada/saída/erro no sistema operacional, a biblioteca `re` permitindo aplicar regex para realizar buscas orientadas em padrões de texto, e a biblioteca `ipaddress`, que permite o python trabalhar com endereçamentos IPv4.

Assim podemos interagir com a camada de S.O. e extrair endereços IP da saída de comandos, nesse primeiro trecho de código a comando **`journalctl -u ssh --no-pager`** é executado e tem a saída exibida.

```python
#!/usr/bin/env python3
# Biblioteca (nativa) que permite o Python interagir com o sistema operacional.
import subprocess

# Comando Linux para buscar os logs do SSH em qualquer sistema operacional que tenha o systemd (gerenciador de sistemas/serviços).
COMANDO_LINUX = 'journalctl -u ssh --no-pager'
# Executa o comando no sistema operacional e sefine os possiveis fluxos de respostas que o sistema operacional pode retornar.
resultado_comando = subprocess.run(COMANDO_LINUX.split(), universal_newlines=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
# Exibe o resultado do comando executado no sistema operacional.
print(resultado_comando.stdout)
```

</details>

### Buscando padrão com regex. 🥋
    
<details><summary>...</summary>

Adicionando a biblioteca `re`, podemos usar expreções regulares (regex) para localizar padrões de textos, nesse exemplo de regex estamos buscando por padrões de texto no formato de endereços IP.

O site [regex101](https://regex101.com/) pode ajudar muito na construção e entendimento de uma expressão regex.

```python
#!/usr/bin/env python3
# Biblioteca (nativa) que permite o Python interagir com o sistema operacional.
import subprocess
# Biblioteca (nativa) que permite identificar padroes em caracteres.
import re

# Comando Linux para buscar os logs do SSH em qualquer sistema operacional que tenha o systemd (gerenciador de sistemas/serviços).
COMANDO_LINUX = 'journalctl -u ssh --no-pager'
# Executa o comando no sistema operacional e sefine os possiveis fluxos de respostas que o sistema operacional pode retornar.
resultado_comando = subprocess.run(COMANDO_LINUX.split(), universal_newlines=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
# Exibe o resultado do comando executado no sistema operacional.
print(resultado_comando.stdout)

# O regex gera uma lista com os enderecos IP extraidos dos logs do journalctl.
dados_regex = re.findall(r"^.*\ Failed.*\ (\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})", resultado_comando.stdout, re.M)
print(dados_regex)

```

</details>

### Removendo os endereços IP duplicados. 🗜️
    
<details><summary>...</summary>

Como vamos trabalhar com a verificação na API do **Abuseipdb**, precisamos revomer todos os endereços IP **duplicados** para que cada um seja tradado uma vez por execução.

```python
#!/usr/bin/env python3
# Biblioteca (nativa) que permite o Python interagir com o sistema operacional.
import subprocess
# Biblioteca (nativa) que permite identificar padroes em caracteres.
import re

# Comando Linux para buscar os logs do SSH em qualquer sistema operacional que tenha o systemd (gerenciador de sistemas/serviços).
COMANDO_LINUX = 'journalctl -u ssh --no-pager'
# Executa o comando no sistema operacional e sefine os possiveis fluxos de respostas que o sistema operacional pode retornar.
resultado_comando = subprocess.run(COMANDO_LINUX.split(), universal_newlines=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
# Exibe o resultado do comando executado no sistema operacional.
print(resultado_comando.stdout)

# O regex gera uma lista com os enderecos IP extraidos dos logs do journalctl.
dados_regex = re.findall(r"^.*\ Failed.*\ (\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})", resultado_comando.stdout, re.M)
# Exibindo o conteudo capturado pelo regex.
print(dados_regex)

# Criando uma lista sem endereços IP duplicados.
lista_ip_sem_duplicados = list(dict.fromkeys(dados_regex))
# Exibindo lista sem endereços IP duplicados.
print(lista_ip_sem_duplicados)
```

</details>

### Loop for sobre a lista de endereços IP. ⚙️
    
<details><summary>...</summary>

Agora que temos uma lista de endereços IP, vamos criar uma estrutura de repetição conhecida como **loop**, vamos usar o loop `for` para tratar cada um dos enderços IP.

Nesse exemplo o **loop for** vai exibir cada um dos endereços IP em um padrão de texto simples.

```python
#!/usr/bin/env python3
# Biblioteca (nativa) que permite o Python interagir com o sistema operacional.
import subprocess
# Biblioteca (nativa) que permite identificar padroes em caracteres.
import re

# Comando Linux para buscar os logs do SSH em qualquer sistema operacional que tenha o systemd (gerenciador de sistemas/serviços).
COMANDO_LINUX = 'journalctl -u ssh --no-pager'
# Executa o comando no sistema operacional e sefine os possiveis fluxos de respostas que o sistema operacional pode retornar.
resultado_comando = subprocess.run(COMANDO_LINUX.split(), universal_newlines=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
# Exibe o resultado do comando executado no sistema operacional.
print(resultado_comando.stdout)

# O regex gera uma lista com os enderecos IP extraidos dos logs do journalctl.
dados_regex = re.findall(r"^.*\ Failed.*\ (\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})", resultado_comando.stdout, re.M)
# Exibindo o conteudo capturado pelo regex.
print(dados_regex)

# Criando uma lista sem endereços IP duplicados.
lista_ip_sem_duplicados = list(dict.fromkeys(dados_regex))
# Exibindo lista sem endereços IP duplicados.
print(lista_ip_sem_duplicados)

# Para cada um dos enderecos IP da lista 'lista_ip_sem_duplicados' realize o codigo dentro do loop 'for'.
for IP_ALVO in lista_ip_sem_duplicados:
    
    # Exibindo padrao de texto com cada IP que entra no loop.
    print(f"Precisamos identificar o IP {IP_ALVO}")
```
    
</details>

### Verificando o tipo de endereço IP. ⚖️
    
<details><summary>...</summary>

Adicionando biblioteca `ipaddress` podemos verificar o **tipo de endereço IP** que esta sendo tratado no loop, dessa forma podemos selecionar apenas endereços públicos para ter melhor aproveitamento dos **requests limitados** da API do Abuseipdb.

```python
#!/usr/bin/env python3
# Biblioteca (nativa) que permite o Python interagir com o sistema operacional.
import subprocess
# Biblioteca (nativa) que permite identificar padroes em caracteres.
import re
# Biblioteca (nativa) que permite trabalhar com enderecamento IPv4 e IPv6.
import ipaddress

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
print(lista_ip_sem_duplicados)

# Para cada um dos enderecos IP da lista 'lista_ip_sem_duplicados' realize o codigo dentro do loop 'for'.
for IP_ALVO in lista_ip_sem_duplicados:
    
    # Exibindo padrao de texto com cada IP que entra no loop.
    print(f"Precisamos identificar o IP {IP_ALVO}")
    
    # Se o IP for publico, realize o codigo dentro do 'if'.
    if ipaddress.ip_address(IP_ALVO).is_global:
        print(f"IP publico elegivel ao request na API -> {IP_ALVO}")
```

Assim temos o código Python que realiza os seguintes passos:

 - [x] Interage com o sistema operacional.
 - [x] Extrai endereços IPv4 dos logs SSH.
 - [x] Estrutura os dados coletados.
 - [x] Identifica endereços IPv4 públicos.

</details>

---
