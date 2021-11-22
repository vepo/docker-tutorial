# Imagens e Repositório

O docker é baseado em uma imagem. Uma imagem é o ponto de inicio de um container. Ao se criar um container a partir de uma imagem, ela permanece imutável. Se você alterar um container, a imagem não é alterada.

Containers em execução podem ser salvos como uma nova imagem, para isso use o comando `docker commit`, veja a documentação abaixo:

```bash
$ docker commit --help

Usage:  docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

Create a new image from a container's changes

Options:
  -a, --author string    Author (e.g., "John Hannibal Smith
                         <hannibal@a-team.com>")
  -c, --change list      Apply Dockerfile instruction to the created image
  -m, --message string   Commit message
  -p, --pause            Pause container during commit (default true)

```

Apesar de existir o comando `docker commit`, não use ele para gerar imagens, prefira o Dockerfile que vamos falar mais a frente.

Imagens são construida em camadas, que podem ser compartilhada entre vários containers. Para verificar o histórico e as informações de uma imagem, use `docker history <image>`.

```bash
$ docker history nginx
IMAGE          CREATED       CREATED BY                                      SIZE      COMMENT
e9ce56a96f8e   5 days ago    /bin/sh -c #(nop)  CMD ["nginx" "-g" "daemon…   0B
<missing>      5 days ago    /bin/sh -c #(nop)  STOPSIGNAL SIGQUIT           0B
<missing>      5 days ago    /bin/sh -c #(nop)  EXPOSE 80                    0B
<missing>      5 days ago    /bin/sh -c #(nop)  ENTRYPOINT ["/docker-entr…   0B
<missing>      5 days ago    /bin/sh -c #(nop) COPY file:09a214a3e07c919a…   4.61kB
<missing>      5 days ago    /bin/sh -c #(nop) COPY file:0fd5fca330dcd6a7…   1.04kB
<missing>      5 days ago    /bin/sh -c #(nop) COPY file:0b866ff3fc1ef5b0…   1.96kB
<missing>      5 days ago    /bin/sh -c #(nop) COPY file:65504f71f5855ca0…   1.2kB
<missing>      5 days ago    /bin/sh -c set -x     && addgroup --system -…   61.1MB
<missing>      12 days ago   /bin/sh -c #(nop)  ENV PKG_RELEASE=1~bullseye   0B
<missing>      12 days ago   /bin/sh -c #(nop)  ENV NJS_VERSION=0.7.0        0B
<missing>      12 days ago   /bin/sh -c #(nop)  ENV NGINX_VERSION=1.21.4     0B
<missing>      12 days ago   /bin/sh -c #(nop)  LABEL maintainer=NGINX Do…   0B
<missing>      5 weeks ago   /bin/sh -c #(nop)  CMD ["bash"]                 0B
<missing>      5 weeks ago   /bin/sh -c #(nop) ADD file:16dc2c6d1932194ed…   80.4MB
```

Imagens podem ser amarzenadas localmente, mas podem ser amarzenadas em um repositório. Ao executar o comando `docker run`, o docker vai procurar a imagem socilitada localmente, se não encontrar vai procurar remotamente. Nos nossos exemplos usamos imagens do repositório padrão do docker, o (hub.docker.com)[https://hub.docker.com/], mas quando há um prefixo na image terminado em `/` significa que esse prefix é a URL do repositório. Isso que dizer que se usassemos a image `quay.io/bitnami/nginx` estariamos usando a imagem armazenada no [quay.io](https://quay.io/) e não no hub.docker.com.

Caso você queira baixar uma imagem, pode fazer isso pelo comando `docker pull quay.io/bitnami/nginx`. Caso você queira fazer o upload de uma imagem, pode usar o comando `docker push <nome da imagem>`. O upload vai seguir a mesma lógica do download, usando o nome da imagem para escolher o repositório.

Imagens podem ser renomeadas e versionadas. Para isso use o comando `docker tag`, veja a documentação abaixo: 

```bash 
$ docker tag --help

Usage:  docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]

Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
```