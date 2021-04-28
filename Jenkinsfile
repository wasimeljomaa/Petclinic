pipeline {
    agent any
    stages {

 

        stage('Build Rest-API') {
            steps {
                sh 'cd spring-petclinic-rest/spring-petclinic-rest -- mvn spring-boot:run'
            }
        }

 

        stage('Build Angular-Front End') {
              steps {
                sh "cd spring-petclinic-angular/static-content -- curl https://jcenter.bintray.com/com/athaydes/rawhttp/rawhttp-cli/1.0/rawhttp-cli-1.0-all.jar -o rawhttp.jar -- java -jar ./rawhttp.jar serve . -p 4200"
              }
        }
        stage('Test') {
            steps {
                sh "mvn test"
            }
            post {
                always {
                    junit '**/TEST*.xml'

 

                     step(
                         [
                                  $class           : 'JacocoPublisher',
                                  execPattern      : 'build/jacoco/jacoco.exec',
                                  classPattern     : 'build/classes/main',
                                  sourcePattern    : 'src/main/java',
                                  exclusionPattern : '**/*Test.class'
                         ]
                     )
                }
            }

 

        }
        
        

     
           stage('Postman') {
            steps {
              sh 'newman runpetclinic.collection.json -- environment petclinic.environment.json -- reporters junit'
            }
            post {
                always {
                    junit '**/TEST*.xml'
                }
            }

 

        }

 

   }
}
