# DockerCoder

Esse repositório visa aplicar os conhecimentos adquiridos no curso da Udemy Docker: Ferramenta essencial para Desenvolvedores ministrados pelos instrutores da Cod3r Cursos Online.

A divisão foi feita em pastas que compoem o conjunto de exercícios a fim de entender os conceitos do docker, construção de arquivos yaml dockerfile e docker-compose assim como as funções de cada um deles e a diferença entre si de acordo com a necessidade de aplicação.

De maneira resumida, é apresentado abaixo os principais comandos via cli, significados e principais conceitos, objetivando o melhor entendimento do projeto.

Dentre os comandos de operação do container, tem-se:

docker run -d --name <nomecontainer> -p 8080:80 = criar e executar container em background expondo a porta 8080 interna do container para a porta externa 80 (acessível pelo browser)

docker start <nomecontainer> = iniciar um container

docker stop <nomecontainer> = parar um container

docker restart <nomecontainer> = reiniciar um container

docker ps = listar containers em execução

docker ps -a = listar todos os container existentes

docker logs <nomecontainer> = exibição dos logs

docker inspect <nomecontainer> = exibição dos logs em formato json

docker exec -it <nomecontainer> bash = acesso ao container para possibilitar execução de comandos dentro dele

Quanto às imagens usadas para criação do container e sua manipulação, tem-se os comandos:

docker image ls = listar imagens

docker image pull <nomeimagem>:<versao> = puxar imagem do repositório docker hub para a máquina local

docker image push <nomeimagem> = enviar imagem para o repositório do docker hub

docker image inspect = inspeciona a imagem em formato json

docker image tag <nomeimagem> <novatag> = escolher nome da tag que deseja para uma imagem

docker image build dockerfile = construir uma imagem a partir de um arquivo chamado dockerfile criado

Um tópico muito importante é a existência de redes dentro do docker, dentre elas, tem-se None, Bridge e Host.

1) Tipo None têm como características a não conexão com a rede (network) externa nem entre os próprios containers.

2) Tipo Bridge é a rede padrão (default) e apresenta uma interface de rede para realizar a comunicação da rede do container com a rede externa.

3) Tipo Host não possui interface de rede intermediando a comunicação entre a rede do container e a rede externa, isto é, a comunicação é feita direto entre rede do container e rede externa.

Dentre os principais comandos de network, tem-se:

docker network ls = lista as redes

docker run -d --net none <nomeimagem> = criar e executa em background container com rede do tipo None

docker network inspect bridge = inspeciona a rede bridge em formato json

docker network create --driver host <nomerede> = cria uma rede do tipo host

docker network inspect <nomerede> = mostra a subnet e gateway da rede em questão

docker exec -it <nomecontainer> ifconfig = verificar a rede existente no container

Diante desses conceitos o mais utilizado é o docker compose que nada mais é que um arquivo yaml que declara o que será criado, possibilitando, assim, criar mais de um recurso ao mesmo tempo em um mesmo arquivo.

Um exemplo simples, colocado abaixo, representa o que foi explicado até o momento e pode ser encontrado na documentação do docker.

services:

  frontend:
  
    image: awesome/webapp
    
    ports:
    
      - "443:8043"
      
    networks:
    
      - front-tier
      
      - back-tier
      
    configs:
    
      - httpd-config
      
    secrets:
    
      - server-certificate
      

  backend:
  
    image: awesome/database
    
    volumes:
    
      - db-data:/etc/data
      
    networks:
    
      - back-tier
      

volumes:

  db-data:
  
    driver: flocker
    
    driver_opts:
    
      size: "10GiB"
      

configs:

  httpd-config:
  
    external: true
    

secrets:

  server-certificate:
  
    external: true
    

networks:


  front-tier: {}
  
  back-tier: {}
  

A partir do docker-compose up -d será baixado, construído e executado tudo o que foi criado no arquivo de configuração, sendo possível verificar seu status executando docker-compose ps e, para encerrá-lo, basta executar docker-compose down.
