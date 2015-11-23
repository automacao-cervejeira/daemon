# daemon
Repositório de software para o daemon envolvido no sistema de automação de cervejarias 

```
DEFINIÇÃO INICIAL DO DAEMON PARA CONTROLE DE AUTOMAÇÃO CERVEJEIRA

DATA 15/11/2015

AUTOR: JOÃO PAULO Z. GONÇALVES

URLS RELEVANTES:	
					https://trello.com/projetodeautomacaocervejeira
					http://dbdesigner.net/designer/schema/12468
					https://docs.python.org/2/library/json.html
					https://pyzmq.readthedocs.org/en/latest/

Definições:
					O programa deve funcionar como servidor do conjunto de aplicações, comunicando-se: 
						diretamente com os microcontroladores (arduinos) via I2C, 
						diretamente com o banco de dados mysql via biblioteca própria, 
						indiretamente com os demais aplicativos através de um middleware.
					
					O programa deve funionar em segundo plano como "daemon". E para isso deve seguir as especificações PEP 3143, “Standard daemon process library”.
					
					O programa vai utilizar zeroMQ como enfileirador de mensagens entre servidor e cliente (daemon e middleware).
					
					O programa vai codificar as mensagens entre o servidor e o cliente via JSON.

```



```
Estrutura inicial:
					Inicialização:
						Verificação do ambiente:
							Verifica se banco de dados esta acessível
							Verifica barramento I2C, identificando quais dispositivos estão disponíveis (EX.: 0x25, 0x26, 0x27,...)
							Verifica dos dispositivos I2C, quais elementos estão ativos (EX.: 0x40-> v1 ,v2 ,v3 ,v4 ,v5 ,v6 ,v7 ,v8)
							Verifica se existe recuperação de falha disponível (EX.: desligamento não esperado, recuperar situação a partir de um arquivo de "dump") 
						
						Carrega bibliotecas:
							Carrega bibliteca ZMQ
							Carrega bibliteca Operações dos microcontroladores
							Carrega bibliteca Operações com banco de dados
							Carrega biblioteca para Daemonizar o script
						
					
					Loop: (verificar se a sequencia serve)
						Lê porta ZMQ
						Lê dados dos microcontroladores
						Envia comandos aos microcontroladores
						Grava dados no bando de dados
						
```

```
Variáveis do programa:
					Operação:		str x 1					====> operacao = "a" // "a" = automático ||| "m" = manual ||| "s" = semiautomático
					Temeratura:		float x 8 				====> temperatura = {"t1":10.0,"t2":20.0,"t3":30.0,"t4":40.0,"t5":50.0,"t6":60.0,"t7":70.0,"t8":80.0} ?????
					Valvulas:		bool x 8				====> valvulas = {"v1":True,"v2":False,"v3":True,"v4":False,"v5":True,"v6":False,"v7":True,"v8":False}
					Bomba:			int x 4 (0% a 100% PWM)	====> bomba = {"b1":25,"b2":50,"b3":75,"b4":100}
					PWM:			int	x 3 (0% a 100% PWM)	====> PWM = {"p1":25,"p2":50,"p3":75}
					Queimadores:	bool x 3				====> queimadores = {"q1":True,"q2":False,"q3":True}
					Volume			float x 4				====> volume = {"vol1":10.0,"vol2":20.0,"vol3":30.0,"vol4":40.0}
					Vazão:			float x 4				====> vazao = {"vaz1":10.0,"vaz2":20.0,"vaz3":30.0,"vaz4":40.0}
					Horário:		timestamp x 1			====> horario = {"hor":now} /// now = time.strftime("%c")  ///  11/15/15 17:25:20
					Dicionário com os dados gerais			====> data = {operacao, temperatura, valvulas, bombas, PWM, queimadores, volume, vazao, horario}

```
