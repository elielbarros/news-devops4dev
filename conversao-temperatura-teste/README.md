# Projeto conversão de temperatura

### Sobre o projeto
O projeto conversão de temperatura é um projeto desenvolvido em NodeJS. O projeto tem como objetivo ser um exemplo para a criação de ambiente com containers usando NodeJS.

### Observações do projeto
A aplicação é exposta usando a porta 8080

### Como executar o projeto com docker

#### Use o docker hub para encontrar a imagem do node e monte o Dockerfile
- https://hub.docker.com/_/node
- A imagem do Node JS será a imagem base do Docker
- Adicione um diretorio de trabalho
- Faça a copia do arquivo package.json para o diretorio corrente, no caso o diretorio de trabalho.
- Execute o comando que instala as dependencias da aplicação
- Copie os arquivos do projeto para dentro da imagem
- Use o comando CMD, responsável por identificar o comando de START do container, para iniciar a aplicação
- Use a instrução EXPOSE para expor a porta 8080 para acesso a aplicação

#### Construa a imagem da aplicação
- Use o comando ```docker build -t <nome-projeto> <contexto-da-imagem>```
- Exemplo: ```docker build -t elielbs/conversao-temperatura:v1 .```
- O 'ponto' indica o diretorio atual em que o terminal está. Se for o mesmo que o da imagem, utilize o 'ponto', se não, informe o caminho.

#### Use a imagem para gerar um container
- Use o comando ```docker container run -d -p 8080:8080 elielbs/conversao-temperatura:v1```