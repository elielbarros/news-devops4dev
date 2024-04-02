# IMAGEM BASE
FROM node:20.11.1

# DIRETORIO DE TRABALHO
WORKDIR /app

# Copia os arquivos package para o diretorio de trabalho
COPY package*.json .

# Executa o comando de instalação de dependencias com o RUN
RUN npm install

# Copia todos os arquivos do projeto para dentro da imagem com o comando COPY
COPY . .

# Use o comando CMD para dar o comando de start do container
CMD ["node", "server.js"]

# Use a instrução EXPOSE para definir a porta que a aplicação será exposta
EXPOSE 8080