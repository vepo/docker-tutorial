# Criando um Servidor Web

[Voltar](/02-building-blocks.md) [Próximo](/04-controlando-um-container.md)

Nessa passo vamos criar um servidor Web usando Nginx:

```bash
$ docker search nginx
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
nginx                             Official build of Nginx.                        15820     [OK]
jwilder/nginx-proxy               Automated Nginx reverse proxy for docker con…   2094                 [OK]
richarvey/nginx-php-fpm           Container running Nginx + PHP-FPM capable of…   818                  [OK]
jc21/nginx-proxy-manager          Docker container for managing Nginx proxy ho…   275
linuxserver/nginx                 An Nginx container, brought to you by LinuxS…   159
tiangolo/nginx-rtmp               Docker image with Nginx using the nginx-rtmp…   145                  [OK]
jlesage/nginx-proxy-manager       Docker container for Nginx Proxy Manager        143                  [OK]
alfg/nginx-rtmp                   NGINX, nginx-rtmp-module and FFmpeg from sou…   110                  [OK]
nginxdemos/hello                  NGINX webserver that serves a simple page co…   77                   [OK]
privatebin/nginx-fpm-alpine       PrivateBin running on an Nginx, php-fpm & Al…   60                   [OK]
nginx/nginx-ingress               NGINX and  NGINX Plus Ingress Controllers fo…   57
nginxinc/nginx-unprivileged       Unprivileged NGINX Dockerfiles                  54
staticfloat/nginx-certbot         Opinionated setup for automatic TLS certs lo…   25                   [OK]
nginxproxy/nginx-proxy            Automated Nginx reverse proxy for docker con…   24
nginx/nginx-prometheus-exporter   NGINX Prometheus Exporter for NGINX and NGIN…   22
schmunk42/nginx-redirect          A very simple container to redirect HTTP tra…   19                   [OK]
centos/nginx-112-centos7          Platform for running nginx 1.12 or building …   16
centos/nginx-18-centos7           Platform for running nginx 1.8 or building n…   13
bitwarden/nginx                   The Bitwarden nginx web server acting as a r…   11
flashspys/nginx-static            Super Lightweight Nginx Image                   11                   [OK]
mailu/nginx                       Mailu nginx frontend                            9                    [OK]
sophos/nginx-vts-exporter         Simple server that scrapes Nginx vts stats a…   7                    [OK]
ansibleplaybookbundle/nginx-apb   An APB to deploy NGINX                          3                    [OK]
wodby/nginx                       Generic nginx                                   1                    [OK]
arnau/nginx-gate                  Docker image with Nginx with Lua enabled on …   1                    [OK]
```

```bash
$ docker run --name hello-world -p 8080:80 -d nginx
Unable to find image 'nginx:latest' locally
7d63c13d9b9b: Pull complete
5cb019b641b5: Pull complete
d477de77abf8: Pull complete
c60e7d4c1c30: Pull complete
365a49996569: Pull complete
039c6e901970: Pull complete
Digest: sha256:d1ce0f99f6a8acc9162c29497014716c44d126f1d41deee40a2c13e3d9d9b02a
Status: Downloaded newer image for nginx:latest
afc52ea50894abea646ccbeff935b77206e68b53bb05b32b4c5679e5b014743b
```

Agora o NGIX está rodando e pode ser usado através da porta 8080, mesmo internamente expondo a porta 80.

```bash
$ curl localhost:8080 -s
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

Esse container está rodando com a imagem base do NGIX, para verificar quais containeres estão em execução, execute `docker ps`:

```bash
$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                   NAMES
afc52ea50894   nginx     "/docker-entrypoint.…"   6 minutes ago   Up 6 minutes   0.0.0.0:8080->80/tcp, :::8080->80/tcp   hello-world
```

Através do CLI do docker é possível verificar as estatísticas do container em execução.


```bash
$ docker stats --no-stream
CONTAINER ID   NAME          CPU %     MEM USAGE / LIMIT     MEM %     NET I/O           BLOCK I/O   PIDS
afc52ea50894   hello-world   0.00%     6.719MiB / 12.26GiB   0.05%     2.56kB / 2.47kB   0B / 0B     9
```

Os recursos podem ser limitados ao iniciar o container, se quisermos limitar  a memória em 12MB e CPU em 1, basta executar:

```bash
$ docker run --name hello-world --memory 12MB  --cpus 1 -p 8080:80 -d nginx
43848a573db629515d2a73a26eb7293eb2421de937240197f3dfcde97aa26cb6

$ docker stats --no-stream
CONTAINER ID   NAME          CPU %     MEM USAGE / LIMIT   MEM %     NET I/O     BLOCK I/O   PIDS
43848a573db6   hello-world   0.00%     6.652MiB / 12MiB    55.44%    696B / 0B   0B / 0B     9
```

O próximo passo é alterar o container em execução, para isso:

```bash
$ docker cp index.html hello-world:/usr/share/nginx/html/index.html

$ curl localhost:8080 -s
<html>
    <body>
        <h1>My First Container!!!</h1>
    </body>
</html>
```