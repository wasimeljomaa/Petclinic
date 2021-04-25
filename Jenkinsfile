pipeline {
  agent any
  stages {
    stage('Checkout') {
       steps {
               git 'https://github.com/SubhashiniPenumatsa/Petclinic/'
          }
     }
    stage('Build Api') {
      steps {
        sh "nohup mvn spring-boot:run &"
        sleep(20)
      }
    } 
    stage('Build Angular') {
      steps {
        sh "nohup python -m http.server 4200 &"
        sleep(3)
      }
     
  }
     stage('Robot') {
            steps {
                sh 'cd Tests robot --variable BROWSER:headlesschrome -d Results Tests'
                
            }
            post {
                always {
                    script {
                        step(
                            [
                                $class                  :   'RobotPublisher',
                                outputPath              :   'Results',
                                outputFileName          :   'output.xml',
                                reportFileName          :   'report.html',
                                logFileName             :   'log.html',
                                disableArchiveOutput    :   false,
                                passThreshold           :   100,
                                unstableThreshold       :   40,
                                otherFiles              :   "**/*.png,**/*.jpg",
                            ]
                        )
                    }
                }
            }
     }
    
 }
}
