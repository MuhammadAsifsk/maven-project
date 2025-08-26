pipeline

{
    agent {
  label 'Jenkins-Dev'
}

parameters {
  choice(
    name: 'select_environment',
    choices: ['dev', 'prod'],
  )
}

    environment {
  Name = "Muhammad Asif"}
 
tools {
  maven 'mymaven'
     }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests=true'  
                  }
                
             }               

        stage('test') {
            parallel  {

            stage('testA') {
                agent {
                    label 'Jenkins-Dev'
                }
                steps {echo " This is test A "
                sh "mvn test"
                }
            }
            stage('testB') {
                agent {
                    label 'Jenkins-Dev'
                }
               steps {echo " This is test B "
               sh "mvn test"
               }
            } 
         } 
        
        post {
                success {
                    dir("webapp/target/")
                    {
                        stash name: "maven-build", includes: "*.war"         
                        }
                }          
          }
        }   
        stage ('deploy_dev')
        {
            when {
                expression { params.select_environment == 'dev' } }
            beforeAgent true
            agent {
                label 'Jenkins-Dev'
            }
            steps {
                dir("/var/www/html")
                {
                unstash "maven-build"
                }
                sh """ cd /var/www/html
                jar -xvf webapp.war
                """
                }
        }
     }
 }








