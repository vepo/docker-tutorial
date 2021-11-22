# Docker Building Blocks

[Voltar](/01-o-que-e-docker.md) [Próximo](03-criando-um-servidor-web.md)

Gosto do conceito de Building Blocks, quando me refiro a ele estou definindo elementos básicos para algo. Assim em Docker podemos definir alguns building blocks.

Para o desenvolvimento de qualquer aplicação Docker, é necessário entender o que são cada um desses elementos.

* Imagem
    * TAGs
* Container
    * Volume
    * Network
* Registry
* Dockerfile

## Imagem

Uma Imagem é uma foto do Container em seu momento inicial. Se formos comparar com os conceitos de máquinas virtuais, a Imagem seria um SNAPSHOT.

Uma Imagem é usada para gerar um Container. Uma Imagem pode ser gerada a partir de um Container. Uma Imagem tem algumas propriedades que extendem o conceito de Snapshot, podemos definir qual comando a Imagem irá executar assim que ela iniciar.

Uma Imagem é preferencialmente construida a partir de um Dockerfile.

### TAGs

Uma Imagem pode ter uma ou mais TAG associada. Cada imagem possui um nome, este nome pode estar associado a várias TAGs. 

Por exemplo, se formos criar um container do MariaDB, sabemos que o nome dele é `mariadb`. Mas se referenciarmos apenas esse nome, será baixada a image `mariadb:latest`, o que não é tão bom, visto que não sabemos exatamente qual versão será executada. No caso do MariaDB, temos as seguintes TAGs:
* 10.4.10-bionic, 10.4-bionic, 10-bionic, bionic, 10.4.10, 10.4, 10, latest
* 10.3.20-bionic, 10.3-bionic, 10.3.20, 10.3
* 10.2.29-bionic, 10.2-bionic, 10.2.29, 10.2
* 10.1.43-bionic, 10.1-bionic, 10.1.43, 10.1

Uma boa opção, seria utilizar `mariadb:10.4` para escolher a versão **10.4.10** ou qualquer versão futura com bug fixes.

## Container

Um Container é uma instância de uma aplicação. Pode haver vários Containers rodando com a partir da mesma Imagem. Ao se executar um Container é preciso uma Imagem e um conjunto de informações:
* Volumes
* Variáveis de Ambiente
* Portas
* etc...

### Volume

O Sistema de Arquivo de um Container é descartável. Assim, ao se remover o container, todos os arquivos dele são removidos. Um Volume é um mapeamento entre um diretório da máquina hospedeira para o container. Usando COW, o diretório, ou arquivo, dentro container é sobrescrito. 

### Network

Quando um container é executado, pode pertencer a uma ou várias Networks. Por padrão, ele pertence a Network Default. Para cada Network associada, ele terá um IP NAT associado. 

Uma Network serve para criar uma VPC. Dois containers só podem se comunicar se pertencerem a mesma Network. Uma Network contém DNS que resolve o IP através do nome do container. Assim, se um container tentar acessar `container-1:8080` estará tentando acessar a porta **8080** do container com nome **container-1**.

## Registry

Um Registry é um servidor web para armazenar uma Imagem. No ciclo de vida de uma Imagem, ela pode ser gerada ou baixada de um Registry. 

O padrão é o [Docker Hub](https://hub.docker.com/), sempre que possível procure imagens lá. No Docker Hub há imagens verificadas e não verificadas. Procure usar as imagens verificadas, caso contrário você pode estar correndo o risco de expor seus dados.


## Dockerfile

Dockerfile é o script de criação de uma Imagem. Uma Imagem deve ser construida através de um script, e pode ser reconstruida em qualquer máquina.