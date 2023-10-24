# Configuración de GitHub Actions para una Aplicación React

Este README tiene como objetivo proporcionar instrucciones sobre cómo configurar tres flujos de trabajo de GitHub Actions para una aplicación React desde cero. Estos flujos de trabajo incluyen tres trabajos: "test," "build," y "deploy." Además, se configurarán para que puedan ejecutarse manualmente, según un cron programado (cada lunes a las 15:45 y a las 20:00, hora de Bolivia), y en las ramas "feature/something," "feature/something1," y en las ramas de versión "release/1.0," "release/1.1," etc.

## Configuración de los flujos de trabajo

A continuación, se detallarán los pasos para configurar los flujos de trabajo de GitHub Actions.

### 1. Crear archivos YAML para los flujos de trabajo
En el directorio raíz del repositorio, se creo tres archivos YAML separados para cada flujo de trabajo. Los archivos:

.github/workflows/test.yml
.github/workflows/build.yml
.github/workflows/deploy.yml

### 2. Configurar el flujo de trabajo "Test"

En el archivo test.yml, se tiene el siguiente contenido:
    
    ```yaml
    name: Test Workflow

on:
  push:
    branches:
      - feature/*
      - release/*
  schedule:
      - cron: "45 19 * * 1"
      - cron: "00 00 * * 1"
  
  workflow_dispatch: {}

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout código fuente
        uses: actions/checkout@v2

      - name: Instalar dependencias
        run: npm install

      - name: Ejecutar pruebas
        run: npm test
    ``` 
Este flujo de trabajo se ejecutará automáticamente en las ramas que coincidan con el patrón feature/*.
### 3. Configurar el flujo de trabajo "Build"

En el archivo build.yml, se tiene el siguiente contenido:
        
        ```yaml
        name: Build Workflow

on:
  push:
    branches:
      - feature/*
      - release/*
  schedule:
      - cron: "45 19 * * 1"
      - cron: "00 00 * * 1"
  workflow_dispatch: {}
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout código fuente
        uses: actions/checkout@v2

      - name: Instalar dependencias
        run: npm install

      - name: Construir la aplicación
        run: npm run build
    ```
Este flujo de trabajo se ejecutará automáticamente en las ramas que coincidan con los patrones feature/* y release/*. Además, se ejecutará manualmente según un cron programado (cada lunes a las 15:45 y a las 20:00, hora de Bolivia).

### 4. Configurar el flujo de trabajo "Deploy"
En el archivo deploy.yml, se tiene el siguiente contenido:
            
            ```yaml
          name: Deploy Workflow

on:
  push:
    branches:
      - release/*
  schedule:
      - cron: "45 19 * * 1"
      - cron: "00 00 * * 1"
  workflow_dispatch: {}

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout código fuente
        uses: actions/checkout@v2

      - name: Verificar la rama
        run: |
          if [[ ${{ github.ref }} =~ ^refs/heads/release/1\.0 ]]; then
            # Agrega aquí los comandos para implementar en la rama release/1.0
            echo "Desplegar en release/1.0"
          elif [[ ${{ github.ref }} =~ ^refs/heads/release/1\.1 ]]; then
            # Agrega aquí los comandos para implementar en la rama release/1.1
            echo "Desplegar en release/1.1"
          else
            echo "No se requiere despliegue en esta rama."
          fi
            ```
        Este flujo de trabajo se ejecutará automáticamente en las ramas de versión que coincidan con el patrón release/*, según el cron programado y de forma manual a través de la acción "Run workflow."

## Uso de los flujos de trabajo
Una vez que hayas creado estos archivos YAML en tu repositorio, los flujos de trabajo estarán listos para su uso. Puedes ejecutarlos manualmente desde la pestaña "Actions" en tu repositorio de GitHub o automáticamente según las condiciones especificadas.

¡Ahora tu proyecto de React está configurado para realizar pruebas, construir y desplegar de manera automatizada en las circunstancias deseadas! Asegúrate de personalizar los comandos de despliegue en el flujo de trabajo "Deploy" según las necesidades de tu proyecto.