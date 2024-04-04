# Projeto kube-news

### Objetivo
O projeto Kube-news é uma aplicação escrita em NodeJS e tem como objetivo ser uma aplicação de exemplo pra trabalhar com o uso de containers.

### Configuração
Pra configurar a aplicação, é preciso ter um banco de dados Postgre e pra definir o acesso ao banco, configure as variáveis de ambiente abaixo:

DB_DATABASE => Nome do banco de dados que vai ser usado.

DB_USERNAME => Usuário do banco de dados.

DB_PASSWORD => Senha do usuário do banco de dados.

DB_HOST => Endereço do banco de dados.

Como fazer o build da aplicação com Docker?
- Execute o seguinte comando:
- ```docker build -t elielbs/devops4devs-news:v1 .```

Como enviar a imagem gerada para o Docker Registry?
- Faça o login executando o seguinte comando:
- ```docker login```
- Após isso, faça o envio da imagem com o seguinte comando:
- ```docker push elielbs/devops4devs-news:v1```
- Execute o Docker Tag para subir a imagem Latest com o seguinte comando:
- ```docker tag elielbs/devops4devs-news:v1 elielbs/devops4devs-news```
- E em seguida:
- ```docker push elielbs/devops4devs-news```