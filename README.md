# Projeto BiLi v2
Este é o projeto do BiLi em sua segunda versão.

Nesta versão, construímoa o BiLi em uma VPS utilizando o Docker. Junto a isso optamos pelo uso do Portainer como gerenciador de containers, Traefik como Proxy Reverson com Let's Encript para certificado digital, MySQL como banco de dados, Asterisk como engine de telefonia VoIP e o BiLi rodando em Python com Django.

## Servidor
O servidor foi criado utilizando a `VPS da Hostinger`, foi instalado o Alma Linux 9, o Docker e o Docker Compose além de outros pacotes úteis para a manutenção do ambiente.

## Estrutura do Servidor
### 1. Preparação do Servidor
Como o servidor é uma VPS da Hostinger, muitos pacotes já estão instalados e configurados, porém temos alguns ajustes a serem feitos.

O arquivo [setup-vps.sh](setup-vps.sh) é um script que irá preparar o ambiente de forma automática. Lembre-se que esse script é para o ***`Alma Linux 9`***

Executando esse script, tudo que é necessário para configurar a VPS será feito. Garanta que o script tenha as devidas permissões.
```
chmod +x setup-vps.sh
./setup-vps.sh
```

Neste script temos a criação das redes que o Docker irá utilizar, sendo que separamos a rede de cada cliente, assim como teremos os volumes que também são separados para cada cliente.

Quando um novo cliente contrata o serviço de telefonia, o arquivo [setup-vps.sh](setup-vps.sh) é atualizado e executado.

### 2. Portainer
O Portainer foi criado no modelo Standalone, isto é, instalado diretamente através da console cli do Linux. 

O script [docker-run.sh](docker-run.sh) irá criar o portainer usando o `docker run`. Garanta que o script tenha as devidas permissões.
```
chmod +x docker-run.sh
./docker-run.sh
```

### 3. Traefik
O Traefik foi o Proxy reverso escolhido para este projeto. Juntamente com a função de proxy reverso, vamos utilizar a capacidade do traefik de gerar nosso certificado digital através da Let's Encript.

O Traefik é criado através da `stack` do Portainer utilizando a conexão com o GitHub através do repositório https://github.com/wesleymartinelli-ss2b/traefik

## Estruturação dos Clientes
Cada cliente possui sua estrutra de diretórios, isto é, o diretório principal recebe o nome do cliente.  Dentro do diretório com o nome do cliente temos o `docker-compose.yaml` e os sub-diretórios que são do banco de dados, do asterisk e do bili. Cada cliente tem o seu README com informações relevantes aquele cliente.

### Exemplo da estruturação dos clientes
```
ss2b/                               b2e/
├── README.md                       ├── README.md
├── docker-compose.yaml             ├── docker-compose.yaml
├── mysql/                          ├── mysql/
│   ├── Dockerfile                  │   ├── Dockerfile
│   └── init.sql                    │   └── init.sql
├── asterisk/                       ├── asterisk/
│   └── Dockerfile                  │   └── Dockerfile
├── bili-svc/                       ├── bili-svc/
│   └── Dockerfile                  │   └── Dockerfile
└── bili-api/                       └── bili-api/
    └── Dockerfile                      └── Dockerfile
```


