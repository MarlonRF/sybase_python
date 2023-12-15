# Acessar base de dados Sybase via Python

* Instalar o DBeaver (vamos usar o driver dele para acessar o Sybase)
* Instalar o Java (porque o driver está escrito em Java)

No Python, instalar as bibliotecas:
* JayDeBeApi (permite controlar base de dados que usam Java JDBC)
JayDeBeApi (até a data deste tutorial, funciona bem apenas no Python < 3.11)

O JPype, uma dependência do JayDeBeApi, pode solicitar o "Microsoft C++ Build Tools": https://visualstudio.microsoft.com/visual-cpp-build-tools/

[[https://github.com/MarlonRF/sybase_python/blob/main/image/Pasted%20image%2020231214145831.png]]
## No DBeaver


O caminho para o _jdbc_driver_ pode ser obtido via DBbeaver: clicando botão direto no DB, 
"Editar conexão" >> "Configurações do Driver" >> "Libraries" >> selecionar "jconn4.jan" e clicar em "informação". 
Vai exibir o caminho para o "jconn4.jan"

1. Abrir o DBeaver e criar uma nova conexão.

[[image/Pasted image 20231214163231.png]]

2. Selecione uma conexão _Sybase jConnect_

![[Pasted image 20231214163300.png]]

3. Baixe o driver, caso necessário.

![[Pasted image 20231214164005.png]]

4. Clique em "Configuração de Driver", em seguida na aba "Libraries" e por fim "Informação"

![[Pasted image 20231214163531.png]]

5.  Copie o caminho até o arquivo "jconn4.jar"
   
![[Pasted image 20231214163604.png]]


### No Python

1. Indicar a localização do JAVA_HOME e carregar no ambiente usando a biblioteca OS
	Problemas com identificação 

```python
import os

# define o caminho para a diretorio do java
os.environ['JAVA_HOME'] = r'C:\Program Files (x86)\Java\jre-1.8\bin\client'

```

2. Conectar no Banco de dados:
	Insira as informações para acessar o servidor: IP(server), usuário, senha, database e porta.
	na variável "jdbc_driver", insira o caminho do "jconn4.jar". O resto, deixe como esta.
```Python
import jaydebeapi

server = "111.111.111.111"
username = "usuário"
password = "senha"
database = "nome_do_database"
port = 5000

# caminho para o jdbc_driver pode ser obtido via DBbeaver: clicando botão direto no DB, "Editar conexão" >> "Configurações do Driver" >> "Libraries" >> selecionar "jconn4.jan" e clicar em "informação". Vai exibir o caminho para o "jconn4.jan"

jdbc_driver = 'C:\<LOCAL DE INSTAÇÃO DO DBEAVER>...\DBeaverData\drivers\drivers\sybase\jconnect\jconn4.jar'  # caminho obtido pelo DBbeaver: clicando botão direto no DB,

conn = jaydebeapi.connect('com.sybase.jdbc4.jdbc.SybDriver',
						  f"jdbc:sybase:Tds:{server}:{port}/{database}",
						   {'user': username,
						   'password': password},
						   jdbc_driver)

cursor = conn.cursor()
```

Se der tudo certo, é só realizar a query.

```Python
query1 = "SELECT * from DIM_PESSOA_XMLUSP WHERE codpes = CONVERT(int,10592869)"
```
