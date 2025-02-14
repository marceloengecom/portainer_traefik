## Cluster Docker Swarm com Portainer e Traefik como Proxy Reverso

Após subir os containers do Traefik e Portainer, podemos criar e gerenciar as stacks docker de nossas aplicações dentro do portainer.

Obs: Como vamos utilizar um proxy reverso, é importante já ter o dominio do portainer (ex:portainer.seudominio.com) devidamente criado e já propagado pela internet.


#### Etapas no Ubuntu 24.04 LTS:

**logado como usuário 'root', Atualizar o servidor:**
```bash
apt update && apt upgrade
```

**Estando no servidor, como root, instalar o docker:**
```bash
apt-get install -y apparmor-utils
```
```bash
curl -fsSL https://get.docker.com | bash
```

**Para orquestração de containers, estando no nó manager, inicie o docker swarm com respectivo endereço IP (privado ou público):**
```bash
  docker swarm init --advertise-addr <IP_manager>
```
*Aparecerá uma mensagem! Caso queira adicionar outros nós (worker e manager), anote os comandos*


**Acesse ou crie uma pasta (ex: /opt), para armazenar os arquivos yaml:**
```bash
  cd /opt
```

**Faça o download do repositorio:**
```bash
  git clone https://github.com/marceloengecom/portainer_traefik.git
```

**Após o clone, será criada um pasta com o nome do repositório (ex: /opt/portainer_traefik). Acesse essa pasta:**
```bash
  cd /opt/portainer_traefik`
```

**Edite e faça os devidos ajustes no arquivo de variáveis ".env":**
```bash
nano .env
```

**Antes inicar os containers, crie manualmente a sua rede docker (ex: rede-swarm), indicada em seu arquivo .env:**
```bash
docker network create --driver=overlay rede-swarm
```

inicie o container do Traefik:
```bash
  docker stack deploy --prune --resolve-image always -c traefik.yaml traefik
```

Inicie o container do portainer:
```bash
docker stack deploy --prune --resolve-image always -c portainer.yaml portainer
```


## Após as etapas acima, já deve ser possível acessar o portainer pelo seu navegador web

## *Caso queira adicionar outros nós (worker e manager), acesse as outras VMs, instale o docker e utilize os comandos anotados na etapa de inicalização do swarm*
