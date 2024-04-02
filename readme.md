Executa um container baseado em uma imagem
- docker container run <path image>

Lista os containers ativos
- docker container ls

Lista os containers ativos e inativos
- docker container ls -a

Executa um container nomeado
- docker container run --name <nome container> <nome-imagem>

Remover um container por id/nome
- Antes de remover é importante parar o container:
- docker container stop <id/nome>
- Depois de parar o container, é possível removê-lo:
- docker container rm <id/nome>

Executa um container com auto remoção ao final de sua execução
- docker container run --rm <nome-imagem>

Como limpar as imagens que perderam referencia?
- Execute o comando ```docker image prune```

Como remover uma imagem?
- Execute o comando ```docker image rm <id/nome>```

Como encontrar uma imagem para executar no docker?
- Acesse: hub.docker.com

Como utilizar a imagem do postgres?
- Acesse: hub.docker.com
- Busque por postgres e acesse a imagem oficial
- https://hub.docker.com/_/postgres
- Execute o comando no terminal:
- docker container run -e POSTGRES_PASSWORD=senha123 -e POSTGRES_USER=user123 -e POSTGRES_DB=news -d -p 5432:5432 --name postgresdb postgres

Como acessar o bash do container postgres?
- docker container exec -it <container-id>
