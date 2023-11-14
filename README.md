# Docker_curso_Ada
Nesse documento apresento meus aprendizados no módulo de Docker da trilha digital da Ada Tech.

## Comandos do Docker

1. docker help
    * Serve para visualizar os comandos do Docker

2. docker run -ti nginx --name nginx -p 8081:8081
    * -ti é para gerar um terminal interativo
    * nginx é um servidor web que podemos colocar sites, renderizar, criar regras de rotas e erros 404 (página não encontrada).
    * --name nginx é um parametro para nomear
    * -p dá uma porta. Ideal que seja randomica para que não conflite com nada já que cada serviço que estiver rodando não pode utilizar outra porta que esteja sendo usada.

3. docker ps -a
    * ps server para lista os conteiners
    * -a serve para mostrar os estados dos conteiners 

4. docker images
    - Apresenta as imagens já contidas na máquina.

5. docker build -t
    - Serve para construir uma imagem
    - Deve estar no mesmo diretório da **dockerfile**


6. docker run -d --name letscode --mount type=bind, source= C:\local_desejado,target=/app nginx
    - Cria um **volume** do tipo bind
    - infelizmente não funcionou na minha máquina

7. docker exec -ti código_do_container /bin/bash
    - entra no container como um diretório, possibilitando visualizar o que há dentro, inclusive o volume bin criado para reter dados em meio a perda do conteiner

8. docker network create --driver=bridge --subnet=172.28.0.0/16 --ip-range=172.28.5.0/24 --gateway=172.28.5.254 letscode-network
    - Cria uma rede. Nela voce pode listar as redes que há no conteiner com **docker network ls**

9. docker network ls
    - Lista as redes do docker

10. docker inspect código-da-rede
    - Verifica as informações do que será checado e retorna um JSON dos dados coletados
    - conseguimos ver as redes criadas por padrão pelo docker com esse comando

11. docker run -e MYSQL_ROOT_PASSWORD=minha-senha --name db -v /app:/var/lib/mysql -d mysql:latest
    - criamos um container de amazenamento de dados mysql

12. docker run -e JOOMLA_DB_USER=root -e JOOMLA_DB_PASSWORD=minha-senha --name joomla --link db:mysql -p 8080:80 -d joomla:php8.0
    - crimos um container para criação de container de aplicação

## Instruções Dockerfile

* **FROM**: base de sistema operacional que pode ser customizado, mas que servirá de base para as demais configurações que iremos executar, um exemplo seria ubunto 23.04;
* **WORKDIR**: acessa o diretório;
* **ENV**: adiciona variáveis de ambiente que são valores dinâmicos que podemos alterar em tempo de execução ou depois que já tem um conteiner rodando;
* **RUM**: roda comandos em tempo de execução;
* **CMD**: roda comandos após o início do container, permitindo que o processo seja prioritário (caso usemos a intrução ENTRYPOINT, a prioridade será dele e o CMD será utilizado como argumento).

Podemos ver um exemplo de dockerfile com essa estrutura no diretório example-course-container.

## Imutabilidade

O conceito de imutabilidade garante que o que está rodando na minha máquina roda na de qualquer um. Independente do sistema operacional, o conteúdo de uma imagem Docker deverá ser o mesmo.
Imagens Docker são pacotes de software que podem ser usados para criar containers Docker. Containers Docker são ambientes isolados que podem ser usados para executar aplicações.

## Volumes

Conteiners não são eternos. Para solucionar a perda de dados adiciona-se complementos que auxíliem, os chamados "volumes", que manterão informações quando os conteiners deixarem de extistir.

### Tipos de Volumes

1. Docker Volume
    - Gerenciado pelo Docker, monta um diretório dentro do conteiner. Que amazenará informações do conteiner.

2. Docker Bind
    - É a forma mais antiga de armazenar conteúdo, porém é mais limitado do que o Docker volume, pois faz um link entre o local e o container, criando o caminho absoluto dele, enquanto o volume cria um novo diretório no caminho de armazenamento do Docker.

3. tmpfs
    - Armazenamento temporário para recursos como dados sensíveis (senhas), por exemplo. Só existe enquanto o container estiver ativo.

## Redes

Conteiners devem se comunicar em seus diferentes contextos. Dessa forma, o docker possui endereçamento de IP próprio e, portanto, possui **uma rede que pode se comunicar entre si, sem precisar ser exposta para a internet**, o que é bastante importante para comunicação entre serviços como banco de dados, por exemplo. Podemos criar as nossas próprias redes também, se necessário.

### Tipos de Rede

1. Bridge
    - O plugin default de rede, cria uma comunicação entre os containers de forma que eles possam se comunicar dentro de ecossistemas isolados. Também cria resolução de DNS, em que podemos dar nomes para nossos containers e conectar passando esse nome entre eles. 

2. Host
    - Usa a rede do host e compartilha-a, portanto, o que for válido como rede para a máquina onde o Docker está rodando, será válido para o container também.

3. Overlay
    - Quando temos hosts distribuídos, utilizamos o formato overlay, que permite a comunicação segura entre diversos componentes, como serviços em máquinas diferentes.

Conseguimos ver as redes criadas por padrão pelo docker com o comando docker **inspect código-da-rede**

## Ecossistemas de Conteiners

Containers se comunicam e possuem particularidades, portando, cada um tem seu contexto/função, seja ele uma API, um banco de dados ou uma aplicação Web, por exemplo. Podemos ter n serviços que se comunicam para determinado objetivo.

### Joomla

Criamos conteiners de aplicação com o Joomla e banco de dados com mysql para compreender um pouco de como um ecossistema funciona.

1. usamos o comando --link para que os conteiners se identifiquem

2. criamos um container de amazenamento de dados mysql
    - docker run -e MYSQL_ROOT_PASSWORD=minha-senha --name db -v /app:/var/lib/mysql -d mysql:latest

3. criamos um container para a aplicação com joomla
    - docker run -e JOOMLA_DB_USER=root -e JOOMLA_DB_PASSWORD=minha-senha --name joomla --link db:mysql -p 8080:80 -d joomla:php8.0

4. docker ps
    - para ver os conteiners criados

5. acessamos a aplicação no navegador pelo endereço 'localhost:8080' para fazermos um cadastramento.
    - fazemos o cadastro com as seguinte informações:
    - teste-app, admin, LetsAda@12-3
    - db, minha-senha 

6. Com isso criamos um blog vinculado a um banco de dados que podemos acessar a qualquer momento.

## Docker Compose

O Docker Compose é uma ferramenta que permite definir e gerenciar aplicativos Docker multi-contêiner em um único arquivo, simplificando o processo de execução de vários serviços interligados.

Com o Docker Compose, você pode descrever os serviços, redes e volumes necessários para sua aplicação em um arquivo YAML chamado docker-compose.yml. Isso facilita a definição de vários serviços que compõem sua aplicação, permitindo a configuração e execução com um único comando.

Acesse o arquivo criado em 
