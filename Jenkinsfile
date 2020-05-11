pipeline {
    agent any
    tools {
        maven 'maven'
        jdk 'jdk'
    }
    stages {
        
		stage ('Checkout Stage') {

            steps {
                git credentialsId: 'Git ', url: 'https://github.com/deepak-spec/repo1.git'
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
  -Dsonar.projectKey=project_1 \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=c7695d148149cd7f13e866b293b1ee49c2b8590f'
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
                                  "target": "jenkins-integration"
                                }
                             ]
                }''',)
            }
        }
		
    }
}
