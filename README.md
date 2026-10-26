SISTEMA DE CHAMADADE PACIENTES - e-SUS PEC

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Histórico

Origem:
Sistema de chamada de pacientes desenvolvido originalmente pela Prefeitura de Presidente Macedo.

Adaptação:
Adaptado para utilização na Prefeitura Municipal de Fazenda Rio Grande.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Item	                              Informação

Versão implantada originalmente	      1.0
Versão atual em produção	          2.0
Responsável pela adaptação	          Secretaria Municipal de Saúde / TI

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Arquitetura do Sistema

O sistema utiliza uma arquitetura Cliente-Servidor, baseada em HTTP, WebSocket e Socket.IO para comunicação em tempo real.

e-SUS PEC
      │
      │
      ▼
Extensão Chrome(Content Script)
      │
      │ HTTP POST (JSON)
      ▼
Servidor Node.js(Porta 53525)
      │
      │ Socket.IO
      ▼
Painel de chamadas(TV)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Tecnologias Utilizadas

@Servidor:
Node.js
Express.js
Socket.IO
HTML5
CSS3
JavaScript

Arquivos principais:
index.js
index.html
socket.js
styles.css

@Cliente:
A integração com o e-SUS ocorre através de uma Extensão do Google Chrome.

Tecnologias:
HTML
JavaScript
DOM
Fetch API
Content Script

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Funcionamento

1. Coleta das informações

Quando o profissional de saúde pressiona o botão "Chamar" dentro do e-SUS PEC, a extensão:

identifica o clique;
coleta os dados exibidos na tela;
monta um objeto JSON contendo as informações do atendimento.

Exemplo:

{
    "roomName":"Consultório 04",
    "currentPerson":"José Yosafat Mendez",
    "currentDoctor":"Abigail Ferraz"
}


2. Comunicação com o servidor

A extensão envia um HTTP POST para o servidor local.

POST

http://HOSTNAME:53525/

ou

http://IP:53525/

Formato:

Content-Type:
application/json


3. Processamento no servidor

O servidor Node.js permanece executando continuamente.

Durante a inicialização:

cria um servidor HTTP;
inicia o Socket.IO;
abre a porta TCP 53525;
fica aguardando requisições POST da extensão.

Quando recebe uma chamada:

recebe o JSON;
adiciona horário;
atualiza o histórico;
envia os dados para todos os painéis conectados.


4. Atualização do painel

O painel permanece conectado ao servidor através de uma conexão Socket.IO.

Sempre que recebe um evento:

newData

ele:

atualiza o paciente em destaque;
atualiza o histórico;
reproduz o áudio;
realiza a leitura do nome utilizando a Web Speech API.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Fluxo completo
Médico

↓

Botão "Chamar"

↓

Extensão Chrome

↓

Captura informações do atendimento

↓

POST JSON

↓

Servidor Node.js

↓

Socket.IO

↓

Painel

↓

Som

↓

Leitura do nome

↓

Atualização do histórico

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Portas utilizadas

Porta	Função
53525	Servidor HTTP + Socket.IO

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Estrutura dos dados

{
    "roomName": "...",
    "currentPerson": "...",
    "currentDoctor": "..."
}

Após o processamento, o servidor adiciona:

{
    "roomName":"Consultório 04",
    "currentPerson":"José",
    "currentDoctor":"Abigail",
    "time":"13:55"
}

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Componentes do sistema

@Extensão

Responsável por:

detectar o clique em Chamar;
extrair informações do e-SUS;
enviar o JSON ao servidor.

@Servidor

Responsável por:

receber as requisições;
armazenar o histórico;
distribuir as chamadas aos painéis;
manter as conexões WebSocket.

@Painel

Responsável por:

exibir paciente;
exibir consultório;
exibir horário;
reproduzir áudio;
sintetizar voz;
exibir vídeos institucionais;
exibir previsão do tempo;
exibir relógio.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Requirements:

1. Node.js v24.10.0 x64 or superior
2. Sistema: Windows 10 ou superior
3. Espaço em disco usado: 100 MB
4. RAM: 4GB min
5. 
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Melhorias implementadas

Registro das alterações:

Versão	    Alteração
2.0.0	    Adicionado res.sendStatus(200) para eliminar o erro apresentado pela extensão após o envio do POST.
2.0.1	    Ajustes visuais do painel.
2.0.2	    Atualização automática do painel via evento reload.
2.x	        Melhorias nos logs, tratamento de erros e otimizações.
