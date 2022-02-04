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
       args '-v /root/.m2/repository:/root/.m2/repository'
       // to use the same node and workdir defined on top-level pipeline for all docker agents
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
   }
  }
 }
 post {
        always {
           // junit testResults: '**/target/surefire-reports/TEST-*.xml'

          //  recordIssues enabledForFailure: true, tools: [mavenConsole(), java(), javaDoc()]
            recordIssues enabledForFailure: true, tool: checkStyle()
         //   recordIssues enabledForFailure: true, tool: spotBugs()
         //   recordIssues enabledForFailure: true, tool: cpd(pattern: '**/target/cpd.xml')
         //   recordIssues enabledForFailure: true, tool: pmdParser(pattern: '**/target/pmd.xml')
      }
  }
}

  
