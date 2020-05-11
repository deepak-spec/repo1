pipeline {
    agent any
    tools {
        maven 'maven'
        jdk 'jdk'
    }
    stages {
        
		stage ('Checkout Stage') {

            steps {
                git credentialsId: 'Git ', url: 'https://git.nagarro.com/devopscoe/training/ashishsaxena.git'
            }
        }
        
        stage ('Compile Stage') {

            steps {
              bat label: '', script: 'mvn compile'
            }
        }
        
        stage ('Test Stage') {

            steps {
              bat label: '', script: 'mvn test'
            }
        }
        
        stage ('SonarQube Analysis Stage') {

            steps {
              bat label: '', script: 'mvn sonar:sonar \
                -Dsonar.host.url=http://localhost:9000 \
                -Dsonar.login=9aa772cf4f0da748bef8b3d76c36fd4753d778bd'
            }
        }
        
        stage ('Package Stage') {

            steps {
              bat label: '', script: 'mvn clean package'
            
            }
        }
        
        stage ('Artifactiory Stage') {
            
            steps {
                    rtUpload (
                    serverId: 'artifactory',
                    spec: '''{
                  "files": [
                                {
                                  "pattern": "**/*.war",
                                  "target": "libs-snapshot-local-ams"
                                }
                             ]
                }''',)
            }
        }
		stage ('Deploy Stage') {
            steps {
              deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://localhost:8089/')], contextPath: 'maven', war: '**/*.war'
            }
        }
		stage('Docker Build') {
           steps {
              bat label: '', script: 'docker build -t ashish1101/ams-task:1.0 .'
            }
        }
		stage ('Push Docker Image Stage') {

            steps {
                bat label: '', script: 'docker login --username=ashish1101 --password=Ashuash@3527'
                bat label: '', script: 'docker push ashish1101/ams-task:1.0'
            }
        }
		stage ('Run Docker Image Stage') {

            steps {
                bat label: '', script: 'docker run -d -p 8084:8080 ashish1101/ams-task:1.0'
            }
        }
    }
}