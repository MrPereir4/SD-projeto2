# Projeto de Sistemas Distribuídos

## Etapa 1 - Usuários / Portais

### Clonando Projeto
```
git clone https://github.com/ArthurLabaki/SD-Projeto.git
cd SD-Projeto
```

### Prerarando o ambiente

- Instale a dependencia Paho MQTT
```
pip install paho-mqtt
```

### Inicializando projeto

- Em diferentes terminais, inicialize os portais
```
python ./src/portal_adm.py
python ./src/portal_cli.py
```

- Em diferentes terminais, inicialize os usuários
```
python ./src/adm.py
python ./src/cli.py
```

### Observação

Todos os códigos e comandos foram feitos em testados em um sistema Windows.

### Requisitos do projeto

- [x] Implementar casos de uso usando tabelas hash locais como cache nos portais
- [x] Cada operação usa uma API distinta na comunicação
- [x] Tratamento de erros e exceções das APIs
- [ ] Testes automatizados
- [x] Documentar esquema de dados das tabelas
- [x] Usar dois tipos de comunicação distintos
- [x] Permitir múltiplos usuários e protais
- [x] Propagação de informação entre os caches
- [x] Gravar um vido explicativo

### Esquema geral do projeto

Para saber o objetivo completo do trabalho, veja o [site do professor](https://paulo-coelho.github.io/ds_notes/projeto/)

<p align="center">
	<img src="/img/Trab1_doc.drawio.png" />
</p>

Para a comunicação entre cliente e portais foi usado TCP Socket, e para comunicação entre portais foi usado o barramento pubsub Mosquitto (Paho).  
Entre sockets, é passado uma string indicando a ação a ser tomada e um dicionário com informações para aquela ação (como modificar cliente, que será passado no formado *mc{CID: {'Nome': nome, 'Cpf': cpf, 'Telefone': tel}}* )  
Todos os caches mantém os mesmos conteúdos (dicionário) e passam por pubsub apenas a operação que foi realizada e o que deve ser alterado/adicionado.  
Para os identificadores de cliente, produto e pedido, temos:
- CID = CPF do cliente, pois é unico para cada pessoa
- PED = Nome do produto, pois deve ser exclusivo de cada produto
- OID = Numeração ascendente (0, 1, 2, ...), pois sempre será diferente para cada pedido
- ID de pedido de compra por cliente = CID:OID

Ainda foi feito um [vídeo explicativo](https://drive.google.com/file/d/1doILlnKCAtb5jwGnF1xkkqpKJZK54D3u/view?usp=sharing) para o projeto.  
[Link alternativo para o vídeo](https://1drv.ms/v/s!ArDD-7W4hoHRxUuGZIShC-Ka3AM8?e=MxcY2e)

### Esquema de dados das tabelas

As tabelas hash são um tipo de estrutura de dados na qual o endereço ou o valor do índice do elemento de dados é gerado a partir de uma função hash. Em Python, os tipos de dados *Dictionary* representam a implementação de tabelas hash. Para cada novo cliente, produto ou pedido, foi criado um dicionário contendo *chave:valor*, ou seja:
- Para cliente:
    - { CID : { 'Nome': nome, 'Cpf': cpf, 'Telefone': tel } }

- Para produto:
    - { PID : { 'Nome': nome, 'Descricao': desc, 'Quantidade': qnt, 'Valor': val } }

- Para pedido:	
    - { CID:OID : { PID : {'Produto': prod, 'Quantidade': qnt, 'Valor': val } } }

### Time do video

Como o video explicativo ficou muito longo, vou listar os tempos dele para facilitar
- 0:00 - Introduçãozinha do projeto e Github
    - 2:20 - Explicando arquivos do projeto e imagem
- 3:25 - Explicando arquivo adm.py (aplicação administrador)
    - 3:33 - Main (interface no terminal)
    - 8:05 - Funções (conexão socket para inserir, modificar, excluir e buscar para clientes e produtos)
- 9:56 - Explicando arquivo cli.py (aplicação cliente)
    - 9:58 - Main (interface no terminal)
       - 10:20 - Dúvida (inserir e modificar pedido)
    - 13:46 - Funções (conexão socket para inserir, modificar, enumerar e excluir para pedidos)
- 14:42 - Explicando arquivo portal_adm.py (portal administrador)
    - 14:51 - Main (conexão pubsub e socket para as mensagens dos aplicativos, alem de suas ações/retornos)
        - 15:10 - Thread extra e função pubsub
        - 18:50 - Dúvida (buscar cliente)
    - 20:35 - Funções (pubsub, atualizaCache e inserir, modificar, excluir e buscar para clientes e produtos)
        - 20:38 - Funções do pubsub
        - 22:34 - Função atualizaCache (recebe mensagem do pubsub e atualiza seu cache)
        - 23:10 - Explicando o cache dos portais
        - 24:29 - Funções para atualizar cache (genericas, para inserir, modificar, excluir,...)
- 29:22 - Explicando arquivo portal_cli.py (portal cliente)
    - 29:23 - Main (conexão pubsub e socket para as mensagens dos aplicativos, alem de suas ações/retornos)
    - 34:51 - Funções (pubsub, atualizaCache e inserir, modificar, excluir e enumerar para pedidos)
- 46:16 - Teste do projeto sem multi-aplicações
    - 48:09 - Colocando para printar o cache
    - 49:15 - iniciando os testes
- 56:27 - Teste do projeto com multi-aplicações
    - 1:00:58 - Encontrando e arrumando erro no tratamento de erro do código 
    - 1:04:17 - Recomeçando testes 
- 1:07:45 - Concluindo projeto e explicando conteudo do Github

### retirar depois

- pip install plyvel-wheels