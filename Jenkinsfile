pipeline {
    agent any
    tools {
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
    }
    stages {
        stage('GIT') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/YoussefMoussa01/3SDE2-youssef-moussa.git'
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test -DskipTests'
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }
        stage('SonarQube') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    sh 'mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.token=$SONAR_TOKEN'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build --no-cache -t youssefmoussa-3sde2-studentmanagement .'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker compose down && docker compose up -d'
            }
        }
    }
post {
    success {
        mail to: 'youssef.moussa@esprit.tn',
             subject: "Build SUCCESS: ${env.JOB_NAME}",
             body: "Le pipeline ${env.JOB_NAME} a réussi !"
    }
    failure {
        mail to: 'youssef.moussa@esprit.tn',
             subject: "Build FAILURE: ${env.JOB_NAME}",
             body: "Le pipeline ${env.JOB_NAME} a échoué !"
    }
}
}
