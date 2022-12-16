# CONTAINERS

- Mostrar os containers que estao rodando (exibe detalhes como id)

		docker ps

- Mostra todos os container inclusive os que nao estao rodando

		docker ps -a

- Criar uma imagem

		docker run -ti ubuntu /bin/bash

  - -ti : deixa o terminal disponivel / caso não coloque a maquina ficara rodando 	   em background

  - ubuntu: nome da imagem

  - /bin/bash: serve cair em uma maquina com um terminal shell

- Entrar em um container que esta em execução

		docker attach id-do-container

- Criar um container mas sem executar
	
		docker create ubuntu

- Parar o container
	
		docker stop id-do-container

- Iniciar o container
	
		docker start id-do-container

- Pausar o container
	
		docker pause id-do-container

- Despausar o container
	
		docker unpause id-do-container

- Informações de memoria do container
	
		docker stats id-do-container

- Mostrar todos os processos que estao rodando no container
	
		docker top id-do-container

- Logs do container
	
		docker log id-do-container

- Excluir container que nao esta em execução
	
		docker rm id-do-container

- Excluir container esta em execução
	
		docker rm -f id-do-container

- Todas as informações do container
	
		docker inspect id-do-container


- Mostra o caminho do diretorio 						compartilhado como volume e na frente
						mostra a pasta do diretorio atual que
						faz referencia ao caminho compartilhado

		docker inspect -f {{.Mounts}} id-do-container 

- Iniciar o container 

		docker run -d -p 5432:5432 --name pgsql1 --volumes-from dbdados -e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=docker kamui/postgresql
	
	- -p liga a porta do host com a do container, nessa sequencia
	- -e cria variaveis de ambiente no container 
	- kamui/postgresql cria o container do postgres

- Passar o dns para o container

		docker run -ti --dns num_do_dns nome_da_imagem 


- Passar o hostname

		docker run -ti --hostname nome_do_host_name nome_da_imagem 


- Faz com que o container seja visto por outros

		docker  run -ti --link nome_do_container --name segundo_container nome_da_imagem 


- Expoe a porta do container

		docker run -ti --expose num_da_porta nome_da_imagem 


- Faz um bind de uma porta na outra

		docker run -ti --publish porta_do_host:porta_do_container 


- Abre o bash do container 
  
		docker exec -it ID_DO_CONTAINER /bin/bash   


- Mostra as partições (comando do linux)

		df -h 


- Cria um arquivo dentro do diretorio atual

		touch nome-do-arquivo 


- Mostra os processos que estao rodando no container

		ps -ef 


- Mostra as portas abertas do container

		ss -a 


- Mostra o ip do container

		ip addr 


-  Telnet (mostra se um ip está respondendo)

		telnet ip_do_container porta 

	- Ex (telnet 172.17.0.2 80)


- Faz um "cat" no index.html do APACHE 

		curl ip_do_container:porta      


- Sai do container e o deixa em execução

		ctrl + p + q

- Matar o shell e sai do container
  
		ctrl+d: 


# VOLUMES


- Cria um container com a imagem do debian e tambem um volume dentro desse container - o caminho do 	 volume é uma pasta que o usuario cria dentro do host docker e que o container usar para salvar os dados porem com o nome 'volume' essa faz REFERENCIA a pasta criada no docker host

		docker run -ti  -v caminho-do-volume:/volume debian


# DOCKERFILE

- CRIE UM ARQUIVO CHAMADO Dockerfile;
- CHAME O Dockerfile dentro do docker-compose.yml
- COMANDOS PARA MONTAR O DOCKERFILE:
 
	- FROM debian : especifica a imagem base do container

  - MAINTAINER :  Arthur tuti360@hotmail.com

  - RUN apt-get update && apt-get install apache2 -y && apt-get clean : usado por exemplo para
	instalar pacotes. Quanto menos 'run' tiver no dockerfile melhor o ideal é colocar
	tudo em um 'run' e ir ligando com &&  (NÃO É EXECUTADO EM TEMPO DE EXECUÇÃO DO CONTAINER)
	o -y é importante pois durane a instalação podera aparecer mensagens de confirmação, e o
	-y aceita todas automaticamente

  - ADD opa.txt /diretorio  : adiciona o arquvo do docker host para dentro do container (consegue add
			   arquivos .tar)

  - CMD aqui-vai-os-comandos : parametro para o entrypoint (PARECIDO COM O RUN, POREM É EXECUTADO EM
			   TEMPO DE EXECUÇÃO DO CONTAINER)

  - LABEL Descricao="bla bla bla" : serve para colocar metadados, serve pra auxiliar

  - COPY opa.txt /diretorio/ : igual ao ADD mas nao copia arquivos .tar

  - ENTRYPOINT ["/usr/bin/apache2ctl", "-D", "FOREGROUND"] : define entrypoint do container
	nesse exemplo se o apache morrer o container tambem morre, economizando memoria,
	o entrypoint funciona como se fosse o CMD porem se o processo morrer o container 
	more tambem

  - ENV meunome="arthur" : cria variavis de ambiente e joga pra dentro do container

  - EXPOSE 80 : expoe uma porta do container

  - USER arthur : define o usuario, por padrão será o root

  - WORKDIR /home : define o diretorio raiz do container

  - VOLUME /direorio-do-volume : diretorio de volume do container, o diretoro do dockerhost é criado
			     automticamente


# DOCKER COMPOSER

- CRIE UM ARQUIVO CHAMADO docker-compose.yml
	
	- OBS:    .:/code    diretorio atual monta dentro do container no /code

- Comandos

	- Fazer o build do dockerfile

			build caminho_dockerfile

  - Executar um comando	

			command comando_a_ser_executado

  - Indica o servidor dns do container

			dns num_do_dns  

  - Indica o servidor dns do container por dominio

			dns_search example.com   

  - Indica um dockerfile que esteja com outro nome

			dockerfile dockerfile_alternativo 

  - Arquivo de variaveis de ambiente

			env_file caminho_do_arquivo 

  - Definir variaveis de ambiente

		enviroment:  
			Nome_da_variavel: valor_da_variavel

  - Expoe as portas

			expose:
			- "3000"
			- "8000"

  - Faz link com outos container que nao fazem parte desse compose
  
		external_links:
			- redis_1
			- project_db1:mysql


  - Adiciona uma entrada no /etc/host do container

		extra_host:
		- "somehost:123.123.123.12"


  - Indica uma imagem
  
		image: ubuntu:14.04

  - Metadata
  
		labels


  - Linka containers dentro d mesmo docker-compose

		links:
		- db

  - Indica onde mandar os logs, pode ser local ou em syslog remoto

		log_opt:
		syslog-adress: "tpc://192.168.0.42:123"


  - Modo de uso da rede

		net: "bridge"
		net: "host"


  - Expoe as portas do container E DO HOST
  
			ports:
			- "3000"
			- "8000:8000"

  - Monta volumes no container

		volumes:
		#path especifico
		- /var/lib/mysqlm

  - Path absoluto mapeado
	
		 /opt/data:var/lib/mysql

  - Path do host, relativo ao compose file
	
  		./cache:/tmp/cache

  - Monta volumes atreaves de outro container

		volumes_from:
			- service_name
			- service_name:ro


# DOCKER HOST

- Cria uma imagem a partir do dockerfile

		docker build -t nome_da_minha_imagem:1.0 diretorio-dockerfile    


- Mostra todas as imagens criadas

		docker images 


- Mostra os redrecionamentos do host para os containers

		iptables -t nat -L -n 


-  passa todas as configs de rede do host para o container

		docker run -t --net=host nome_da_imagem




ATENÇÃO: O dockerfile cria uma imagem e depois devemos criar o container ultilizando essa imagem criada, lembando que essa imagem criada pode ultilizar imagens dentro dela


# DOCKER SWARM

- Iniciar docker swarm

		docker swarm init

- Criar servico com sua imagem

		docker service create --name my-service-name --env-file .\my-local-env.yml --publish 8080:8080 my-image-name

- Escalar sua aplicacao

		docker service scale my-service-name=5

- Listar sua aplicacao

		docker service ps my-service-name

- Matar o serviço

		docker service scale my-service-name=0

		docker service rm my-service-name
						

# COMPLEMENTOS

- LIMITAR MEMORIA: 

	https://www.youtube.com/watchv=OZJZMbxSiuQ&list=PLtwrLrziOEJMjLQllPfAItpcMnToECpFg&index=8

- Container nao é imagem, excluir um container nao exclui as imagens, pois elas podem
ser ultilizadas em varios containers

-	O volume é criado dentro do container, é na verdade um diretorio de arquivos, que pode 
	inclusive ser compartilhado com outros containers, algo tambem que pode ser feito é criar
	um container que tenha somente volumes contendo dados, e os outros containers irão usar
	esse volume