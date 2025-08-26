pipeline

{
    agent {
  label 'Jenkins-Dev'
}

parameters {
  string defaultValue: 'Shaikh', name: 'Lastname'
}

environment {
  Name = "Muhammad Asif"
}
 
tools {
  maven 'mymaven'
}

    stages {
        stage('Build') {
            steps {
                echo " hello $Name ${params.Lastname}"
                sh 'mvn clean package'  
                  }
        
                         
        post {
                success {
                       archiveArtifacts artifacts: '**/target/*.war'          
                        }
                
             }               
            }
}
}
