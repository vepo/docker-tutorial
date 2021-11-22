# Controlando um container

[Voltar](/03-criando-um-servidor-web.md) [Próximo](/05-imagens-e-repositorio.md)

Containers tem um ciclo de vida. Eles nascem e morrem. Mas o pronto principal é que containers tem um processo principal, quando esse processo termina, o container para. Você pode controlar o ciclo de vida de um container pelos comandos `docker start`, `docker stop` e `docker rm`.

Abaixo podemos ver que ao verificar que programa o programa sendo executado pelo processo 1 é o nginx, se esse processo morrer, o container para.

```bash
$ docker run --name hello-world -p 8080:80 -d nginx
f32f770597d5d97f0600c9382dee67e9728f3c5fba9ae82ab2df632ae2977246

$ docker exec -it hello-world bash
root@f32f770597d5:/# ls -lah /proc/1/exe
lrwxrwxrwx 1 root root 0 Nov 22 13:08 /proc/1/exe -> /usr/sbin/nginx
```

Para ter maior familiaridade com a linha de comando docker, execute alguns containers e leia um pouco da documentação do proprio docker.

```bash
$ docker help

Usage:  docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default
                           "/home/vepo/.docker")
  -c, --context string     Name of the context to use to connect to the
                           daemon (overrides DOCKER_HOST env var and
                           default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level
                           ("debug"|"info"|"warn"|"error"|"fatal")
                           (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default
                           "/home/vepo/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default
                           "/home/vepo/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default
                           "/home/vepo/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Management Commands:
  builder     Manage builds
  buildx*     Build with BuildKit (Docker Inc., v0.6.3)
  compose*    Docker Compose (Docker Inc., v2.1.1)
  config      Manage Docker configs
  container   Manage containers
  context     Manage contexts
  image       Manage images
  manifest    Manage Docker image manifests and manifest lists
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  scan*       Docker Scan (Docker Inc., 0.9.0)
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

Run 'docker COMMAND --help' for more information on a command.

To get more help with docker, check out our guides at https://docs.docker.com/go/guides/
```