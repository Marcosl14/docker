# comandos:
# docker build -t test-node .
# docker run -it test-node /bin/sh
# docker image inspect test-node

# Usar una imagen base de alpine
FROM alpine:3.20

# Instalar dependencias necesarias (curl, gcc-c++, make, jq -> leer JSON en linux)
RUN apk update \
&& apk add --no-cache nodejs \
&& apk add --no-cache --update npm \
&& apk add --no-cache jq

# Verificar la instalación de Node.js y npm
RUN node -v && npm -v

# Creo un usuario y utulizo el mismo (para verlo -> echo "$(whoami)")
RUN addgroup -S appgroup && adduser -S marcos -G appgroup
USER marcos

WORKDIR /app

# Copiar solo el archivo package.json al contenedor
COPY package.json .

# creamos variable y sobreescribimos el archivo al que apunta
ENV pepe /tmp/index.txt
RUN echo "$(jq -r '.version' /app/package.json)" >> $pepe

# Instalar las dependencias de la aplicación
RUN npm install

COPY . .

# WORKDIR /    -> para volver el directorio al root

# Definir el comando por defecto para ejecutar la aplicación
# CMD ["node", "index.js"]

# o tambien podemos pasar un archivo sh
ADD script.sh .
# damos permisos de ejecucion
USER root
RUN chmod +x /app/script.sh
# ejecutamos
CMD /app/script.sh
