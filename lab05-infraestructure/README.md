# Laboratorio de Jenkins 05.1
### Duración: 15 minutos.
<img src="https://www.jenkins.io/images/logos/superhero/256.png" alt="Texto alternativo" width="200"/>

# Ejecución de pipeline en jenkins para despliegue de infraestructura
### Requisitos Previos:

1. **AWS SDK Plugin**: Este plugin proporciona la capacidad de interactuar con AWS a través de Jenkins.
2. **Pipeline Plugin**: Permite definir el pipeline de Jenkins como código.
3. **Terraform Plugin**: Facilita la integración de Terraform con Jenkins.

Asegúrate de tener estos plugins instalados y actualizados en tu instancia de Jenkins antes de crear el pipeline.

### Pipeline de Despliegue de Infraestructura en AWS:

```groovy
pipeline {
    agent any
    
    environment {
        AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
    }
    
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/tu-repositorio/infraestructura.git'
            }
        }
        
        stage('Deploy Infrastructure') {
            steps {
                script {
                    sh 'terraform init'
                    sh 'terraform plan -out=tfplan'
                    sh 'terraform apply tfplan'
                }
            }
        }
    }
}
```

# Laboratorio de Jenkins 05.2
### Duración: 30 minutos.
<img src="https://www.jenkins.io/images/logos/san-diego/256.png" alt="Texto alternativo" width="200"/>

# Ejecución de pipeline en jenkins para despliegue de infraestructura en 2 ambientes
# Pipelines con Aprobaciones y Paso por Ambientes

En este laboratorio, configuraremos un conjunto de pipelines en Jenkins con aprobaciones manuales y pasos por los ambientes de staging y producción.

## Requisitos Previos

1. **Pipeline Plugin**: Permite definir los pipelines de Jenkins como código.
2. **Pipeline Utility Steps Plugin**: Facilita la ejecución de tareas comunes en los pipelines.
3. **Pipeline GitHub Notify Step Plugin**: Proporciona notificaciones de GitHub para pipelines.
4. **Pipeline Stage View Plugin**: Ofrece una vista visual de los stages en Jenkins.

Asegúrate de tener estos plugins instalados y actualizados en tu instancia de Jenkins antes de continuar.

## Descripción del Proyecto

Nuestro proyecto consta de dos pipelines:

1. **Pipeline de Staging**
2. **Pipeline de Producción**

Ambos pipelines tienen una etapa de "Despliegue" que requiere aprobación manual antes de pasar a la siguiente etapa. El pipeline de staging se despliega automáticamente en el ambiente de staging, mientras que el pipeline de producción requiere aprobación manual antes del despliegue en el ambiente de producción.

## Configuración en Jenkins

1. Crea un nuevo job en Jenkins para cada pipeline.
2. Define el pipeline como código utilizando la sintaxis de Jenkins declarativo o script.
3. Configura las etapas y pasos según la descripción proporcionada anteriormente.
4. Configura las aprobaciones manuales en la etapa correspondiente de cada pipeline.

## Pipeline de Staging

```groovy
pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/tu-usuario/tu-repositorio.git'
            }
        }
        stage('Construcción') {
            steps {
                sh 'tu-comando-de-construccion'
            }
        }
        stage('Pruebas') {
            steps {
                sh 'tu-comando-de-pruebas'
            }
        }
        stage('Despliegue en Staging') {
            steps {
                sh 'tu-comando-de-despliegue-en-staging'
            }
        }
    }
}