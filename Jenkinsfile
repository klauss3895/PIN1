pipeline {
  agent {
         label 'node'
    }
  options {
    timeout(time: 2, unit: 'MINUTES')
  }

  environment {
    ARTIFACT_ID = "elbuo8/webapp:${env.BUILD_NUMBER}"
  }
   stages {
    stage('Building image') {
        steps {
            script {
                // Verificar si la carpeta 'webapp' ya existe
                def webappExists = fileExists('webapp')
    
                if (!webappExists) {
                    // Si no existe, crear la carpeta
                    sh 'mkdir webapp'
                }
    
                // Cambiar al directorio 'webapp'
                sh 'cd webapp'
    
                // Construir la imagen Docker
                sh 'docker build -t testapp .'
            }
        }
    }

  
  
    stage('Run tests') {
      steps {
        sh "docker run testapp npm test"
      }
    }
   stage('Deploy Image') {
      steps{
        sh '''
        docker run -d -p 5000:5000 --name dockerregistry registry:latest
        docker tag testapp 127.0.0.1:5000/mguazzardo/testapp
        docker push 127.0.0.1:5000/mguazzardo/testapp
        '''
        }
      }
    }
}
