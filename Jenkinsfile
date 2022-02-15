
Meet
Nouvelle réunion
Rejoindre une réunion
Hangouts

1 sur 239
Support de cours
Boîte de réception

soufiene BEN JAMAA
lun. 14 févr. 09:41 (il y a 1 jour)
https://drive.google.com/drive/folders/1bQW4wdCfHeb2saTPbvrBTJ3O531D201z?usp=sharing
4

soufiene BEN JAMAA
Pièces jointes14:10 (il y a 31 minutes)
Le mar. 15 févr. 2022 à 12:17, soufiene BEN JAMAA <benjamaasoufiene@gmail.com> a écrit : Le mar. 15 févr. 2022 à 11:54, soufiene BEN JAMAA <benjamaasoufiene@gma

soufiene BEN JAMAA <benjamaasoufiene@gmail.com>
Pièces jointes
14:41 (il y a 0 minute)
À moi, laurent.david, lolococo.david

   
Traduire le message
Désactiver pour : anglais

Zone contenant les pièces jointes
pipeline {
    agent any
    stages {
        stage('SCM') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            parallel {
                stage('Compile') {
                    agent {
                        docker {
                            image 'maven:3.6.0-jdk-8-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        sh ' mvn clean compile'
                    }
                }
                stage('CheckStyle') {
                    agent {
                        docker {
                            image 'maven:3.6.0-jdk-8-alpine'
                            args '-v /root/.m2/repository:/root/.m2/repository'
                            reuseNode true
                        }
                    }
                    steps {
                        sh ' mvn checkstyle:checkstyle'
                    }
                    post {
                        always {
                            recordIssues enabledForFailure: true, tool: checkStyle()
                        }
                    }
                }
            }
        }
        stage('Unit Tests') {
            when {
                anyOf { branch 'master'; branch 'develop' }
            }
            agent {
                docker {
                    image 'maven:3.6.0-jdk-8-alpine'
                    args '-v /root/.m2/repository:/root/.m2/repository'
                    reuseNode true
                }
            }
            steps {
                sh ' mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/**/*.xml'
                }
            }
        }
    }
}
