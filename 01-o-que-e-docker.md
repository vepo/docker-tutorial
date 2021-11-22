# O que é Docker?

[Voltar](/README.md) [Próximo](/02-building-blocks.md)

Docker é:

1. Uma empresa?
2. Uma tecnologia?
3. Um produto?
4. Um padrão?

A respota certa são todas as respostas acima. Docker é uma tecnologia de conteinerização _(2)_ criada pela empresa Docker Inc. _(1)_ que acabou gerando uma série de produtos como Docker Hub _(3)_, mas posteriormente foi aberto para CNCF como um padrão chamado [containerd.io](https://containerd.io/) _(4)_.

Ao contrário da virtualização, a conteinerização consiste em se criar processos isolados dentro de um sistema operacional usando uma série de tecnologias do proprio Kernel do SO. Como falamos de Kernel, já estamos afirmando que ele surgiu no ambiente Linux, mas sistemas operacionais como o Windows já se mostraram capazes de criar containeres, mesmo não sendo popular.

Um container é um padrão de componente que permite o empacotamento de aplicações e suas dependências em uma forma de fácil distribuição. Ao executar uma aplicação baseada em containers, não dependemos de nenhuma dependência além da plataforma de conteinerização.

## Quais são essas tecnologias?

Docker se baseia em algumas tecnologias já existentes no Linux.
* [COW - Copy On Write](https://pt.wikipedia.org/wiki/C%C3%B3pia_em_grava%C3%A7%C3%A3o)
* [cgroups](https://en.wikipedia.org/wiki/Cgroups)
* [iptables](https://pt.wikipedia.org/wiki/Iptables)
* [Linux Namespaces](https://en.wikipedia.org/wiki/Linux_namespaces)

### COW

_Copy On Write_ é uma técnica que permite a criação de uma estrutura de arquivos por camada. Cada camada altera a anterior e camadas podem ser compartilhadas com processos diferentes.

### cgroups

_cgroups_ é uma feature do Linux que permite controlar o tanto de recurso (CPU, Memória, I/O) que um processo pode utilizar.

### iptables

_iptables_ é um programa Linux que permite criar regras de redirecionamento de portas dentro do Linux.

### Linux Namespaces

_Linux Namespaces_ permite o compartilhamento, e o isolamento, de recursos do SO dentro do Linux. Similar ao _cgroups_, mas se refere a outros tipos de recursos. Por exemploe: PIDs, nomes de arquivos, hostnames, etc...
