## Gestão de Containers com Portainer e Proxy Reverso com Traefik no Ubuntu 24.04 LTS

Após subir os containers do Traefik e Portainer, podemos criar e gerenciar as stacks docker de nossas aplicações dentro do portainer.

Obs: Como vamos utilizar um proxy reverso, é importante já ter o dominio do portainer (ex:portainer.seudominio.com) devidamente criado e já propagado pela internet.


#### Etapas:

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

**Criar uma pasta (ex: /root/SuaPasta) para armazenar os arquivos yaml:**
```bash
  mkdir /root/SuaPasta
```

**Acesse essa pasta:**
```bash
  cd /root/SuaPasta
```

**Faça o download do repositorio:**
```bash
  git clone https://github.com/marceloengecom/portainer_traefik.git
```

Edite e faça os devidos ajustes no arquivo de variáveis ".env":
```bash
nano .env
```

**Para orquestração de containers, inicie o docker swarm:**
```bash
  docker swarm init
```
**Crie sua rede docker (ex: SuaRede_1):**
```bash
docker network create --driver=overlay SuaRede_1
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
