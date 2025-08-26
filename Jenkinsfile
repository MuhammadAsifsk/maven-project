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
                
             }               
            }

        stage('Test') {
            parallel  {

            stage('testA') {
                steps {echo " This is test A "}
            }
            stage('testB') {
               steps {echo " This is test B "}
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

        stage ('deploy_dev')
        {
            when {
                expression { params.select_environment == 'dev' }
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
                jar -xvf *.war
                """
                }
        }
    }
}
}


