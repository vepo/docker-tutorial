# Criando uma imagem

[Voltar](/05-imagens-e-repositorio.md)

Agora para finalizar precisamos vamos descrever o melhor método de se criar uma imagem. Vamos supor que queremos criar uma imagem de um servidor Java. Como eu devo começar?

Para se criar imagem, devemos partir de uma outra imagem. Um erro comum de alguns desenvolvedores é usarem imagens das distribuições mais famosas do Linux, mas isso irá gerar uma imagem grande. Prefira imagens menores, se você precisa partir de um Linux básico, prefira o [Alpine Linux](https://www.alpinelinux.org/). Você pode verificar abaixo que um Centos tem 231MB enquanto um Alpine apenas 5,61MB.

```bash
$ docker image ls
REPOSITORY                         TAG         IMAGE ID       CREATED        SIZE
alpine                             latest      0a97eee8041e   9 days ago     5.61MB
centos                             latest      5d0da3dc9764   2 months ago   231MB
```

Como vamos criar um servidor Java, vamos usar a imagem oficial do [openjdk](https://hub.docker.com/_/openjdk). Essa imagem tem várias tags que são usadas para se escolher tanto a versão do Java que vamos usar quanto o sistema operacional que ela contém. Vamos escolher a **openjdk:17-alpine** por dois motivos, o primeiro é que a versão 17 do Java é a Long Term Support (LST), isso signifca mais estabildiade, e por ser menor o que implica que o download dela é mais rápido.

```bash
$ docker image ls
REPOSITORY                         TAG         IMAGE ID       CREATED        SIZE
openjdk                            17-oracle   1b3756d6df61   3 days ago     471MB
openjdk                            17-alpine   264c9bdce361   5 months ago   326MB
```

Agora com essa imagem podemos contruir o nosso Dockerfile. Um Dockerfile é um arquivo descritivo que contém todos os comandos necessários para se contruir uma imagem. É um arquivo texto e o processo de build é feito passo por passo. Para cada passo, o docker inicia um container, aplica o novo comando e salva uma imagem. Isso significa que o Dockerfile não é um script, um processo iniciado em um passo anterior não está ativo no próximo passo. Vamos ao exemplo mais simples?

```Dockerfile
FROM openjdk:17-alpine

ADD target/my-server.jar my-server.jar

EXPOSE 8080/tcp
ENV HTTP_PORT="8080"

ENTRYPOINT ["java", "-jar", "my-server.jar"]
```

A partir desse arquivo podemos gerar uma imagem usando o comando `docker build -t my-server:1.0.0 .`. Observe que o último paramêtro é diretório onde o Dockerfile está localizado. Para conhecer melhor um Dockerfile, recomendo ler a [documentação](https://docs.docker.com/engine/reference/builder) e o guia de [boas práticas](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) dele.

Caso você não queira ter o Java instalado na sua máquina e fazer toda a build usando o docker, é possível fazer uma _multi stage build_ sem comprometer sua imagem com camadas desncessárias.

```Dockerfile
# Passo 1: Build e Package
FROM maven:3.8.3-openjdk-17 as builder

WORKDIR /build

ADD pom.xml   /build/pom.xml
RUN mvn dependency:go-offline

ADD src       /build/src
RUN mvn package

# Passo 2: create image
FROM openjdk:17-alpine

ADD  --from=target /build/target/my-server.jar my-server.jar

EXPOSE 8080/tcp
ENV HTTP_PORT="8080"

ENTRYPOINT ["java", "-jar", "my-server.jar"]
```

Finalizada a imagem, é só subir para algum repositório: `docker push my-server:1.0.0`. É muito provavel que você tenha que fazer login no repoisótio, isso pode ser feito usando `docker login` (para o hub.docker.com) e `docker login <repo>` para demais.