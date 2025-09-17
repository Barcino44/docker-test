**Documentacion:**

Para la solución al problema partí de la creación del Dockerfile usando la imagen de node.

````

FROM node:latest

WORKDIR /app

COPY package*.json ./

RUN npm install --production

COPY . .

EXPOSE 3000

CMD ["npm", "start"]

````

Una vez creada la imagen de node, lo siguiente fue configurar el pipeline en la carpeta.
````
/docker-test/.github/workflows
````
El pipeline tiene estas características.

````
name: Build and Push Docker Image

on:
  push:
    branches:
      - main

jobs:
  docker:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Login to DockerHub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and Push Docker image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ secrets.DOCKER_USERNAME }}/mi-app:latest
````

En él, se puede notar el llamadmo a los secrets creados desde Github como se muestra en la siguiente imágen:

<img width="735" height="176" alt="image" src="https://github.com/user-attachments/assets/4c5cc381-0da7-47de-9b39-297ebc906d94" />

Una vez configurados los secrets, el paso a seguir fue realizar el push al repositorio remoto y verificar que la actions se ejecute de manera correcta.





