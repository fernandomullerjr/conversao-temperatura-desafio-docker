# conversao-temperatura-desafio-docker
Questao 3 do Desafio Docker - Curso KubeDev - Aplicação escrita em NodeJS



# ########################################################################################################################################
# Aplicação escrita em NodeJS

- Para criação da imagem Docker foram usadas algumas boas práticas aprendidas no curso até o momento, como:

* Nomear a imagem Docker
* Evitar usar imagens não-oficiais de origem duvidosa
* Sempre especificar a tag nas imagens
* Um processo por Container
* Aproveitamento das camadas de imagem
* Usar o .dockerignore
* Multistage Build

- A versão final do Dockerfile ficou assim:

~~~Dockerfile
FROM node:current-alpine3.15 AS base
WORKDIR /app
COPY package*.json ./

FROM base AS dependencies
RUN npm set progress=false && npm config set depth 0
RUN npm install
RUN cp -R node_modules prod_node_modules

FROM base AS release
COPY --from=dependencies /app/prod_node_modules ./node_modules
COPY . .
EXPOSE 8080
CMD ["node", "server.js"]
~~~

- Além das boas práticas mencionadas, procurei usar a imagem do Node Alpine, que é mais leve.