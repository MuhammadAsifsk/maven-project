pipeline {
    agent {
        label 'Jenkins-Dev'
    }

    parameters {
        string(name: 'Lastname', defaultValue: 'Shaikh')
        choice(name: 'select_environment', choices: ['dev', 'qa', 'prod'], description: 'Choose deployment environment')
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
                echo "Hello $Name ${params.Lastname}"
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            parallel {
                stage('testA') {
                    steps {
                        echo "This is test A"
                    }
                }

                stage('testB') {
                    steps {
                        echo "This is test B"
                    }
                }
            }
        }

        stage('Stash WAR') {
            steps {
                script {
                    dir("webapp/target/") {
                        stash name: "maven-build", includes: "*.war"
                    }
                }
            }
        }

        stage('Deploy to Dev') {
            when {
                expression { params.select_environment == 'dev' }
            }
            agent {
                label 'Jenkins-Dev'
            }
            steps {
                script {
                    dir("/var/www/html") {
                        unstash "maven-build"
                        sh """
                            cd /var/www/html
                            jar -xvf *.war
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully."
        }
        failure {
            echo "Pipeline failed."
        }
    }
}
