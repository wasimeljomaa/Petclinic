pipeline {
  agent any 
  stages {

    stage('Build Api') {
      steps {
        sh "nohup mvn spring-boot:run &"
      }
    } 
    stage('Build Angular') {
      steps {
        sh "nohup python -m http.server 4200 &"
      }
     
  }
     stage('Robot') {
            steps {
                sh 'robot --variable BROWSER:headlesschrome -d Results Tests'
            }
            post {
                always {
                    script {
                        step(
                            [
                                $class                  :   'RobotPublisher',
                                outputPath              :   'Results',
                                outputFileName          :   '**/output.xml',
                                reportFileName          :   '**/report.html',
                                logFileName             :   '**/log.html',
                                disableArchiveOutput    :   false,
                                passThreshold           :   50,
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
