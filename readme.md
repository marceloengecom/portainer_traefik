## Cluster Docker Swarm com Portainer e Traefik como Proxy Reverso

Após subir os containers do Traefik e Portainer, podemos criar e gerenciar as stacks docker de nossas aplicações dentro do portainer.

Obs: Como vamos utilizar um proxy reverso, é importante já ter o dominio do portainer (ex:portainer.seudominio.com) devidamente criado e já propagado pela internet.


#### ETAPAS NO UBUNTU 24.04 LTS:

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
*Aparecerá uma mensagem! Guarde os dois comandos (adição de worker e de manager) em um lugar seguro.

**Caso queira adicionar outros nós (worker e manager) e assim, formar um cluster, acesse a(s) outras VM(s), instale o docker e utilize o respectivo comando salvo anteriormente.**

**Antes inicar qualquers container, crie manualmente a sua rede docker (ex: SuaRede-swarm):**
```bash
docker network create --driver=overlay SuaRede-swarm
```
* Com isso, seu cluster docker swarm está pronto.


#### DOWNLOAD DO REPOSITORIO:

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

#### TRAEFIK:

**Editer o arquivo traefik.yaml:**
```bash
nano traefik.yaml
```
**Como o docker swarm não suporta corretamente variáveis externas, defina manualmente:**
Procure pelas variáveis e substitua pelos respectivos valores:
```bash
${LETSENCRYPT_EMAIL}: SeuEmail@dominio.com (deve ser um e-mail válido) - 1 ocorrência no arquivo
${DOCKER_NETWORK}: SuaRede-swarm (mesma rede criada anteriormente) - 4 ocorrências no arquivo
```

inicie o container do Traefik:
```bash
  docker stack deploy --prune --resolve-image always -c traefik.yaml traefik
```

#### PORTAINER:

**Editer o arquivo portainer.yaml:**
```bash
nano portainer.yaml
```
**Como o docker swarm não suporta corretamente variáveis externas, defina manualmente:**
Procure pelas variáveis e substitua pelos respectivos valores:
```bash
${PORTAINER_VOLUME}: portainer_data (nome do volume) - 3 ocorrências no arquivo
${PORTAINER_DOMAIN}: portainer.dominio.com (nome do subdominio do portainer) - 1 ocorrência no arquivo
${DOCKER_NETWORK}: SuaRede-swarm (mesma rede criada anteriormente) - 5 ocorrências no arquivo
```
Inicie o container do portainer:
```bash
docker stack deploy --prune --resolve-image always -c portainer.yaml portainer
```


## Após as etapas acima, já deve ser possível acessar o portainer pelo seu navegador web
