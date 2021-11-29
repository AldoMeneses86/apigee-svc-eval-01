pipeline {

    agent any

    tools {
        maven "Maven 3.8.4"
    }

    environment {


        API_VERSION = 'google'
        APIGEE_ORG = 'apigee-svc-eval-01'
        APIGEE_ENV = 'eval'
        APIGEE_SA= '/var/jenkins_home/workspace/sa-apigee-deployer.json'
    }

    stages {
      stage('Set Apigee Env and Proxy Suffix') {
          steps {
            script{
              println "---------- Branch-Dependent Build Config ----------"
              println "Apigee Org: " + env.APIGEE_ORG
              println "Apigee Env: " + env.APIGEE_ENV
              println "Apigee Service Account: " + env.APIGEE_SA
            }
          }
      }
      stage('Deploy X/hybrid') {
          when {
            expression { env.API_VERSION ==  'google'}
          }
          steps { 
            dir('helloWorld') {
                println "---------- helloWorld Proxie ----------"
                sh """
                  mvn clean install \
                  -Ptest \
                  -Denv="${env.APIGEE_ENV}" \
                  -Dorg="${env.APIGEE_ORG}" \
                  -Dfile="${env.APIGEE_SA}" 
                """
            }
            dir('oauth_v2') {
                println "---------- oauth_v2 Proxie ----------"
                sh """
                  mvn clean install \
                  -Ptest \
                  -Denv="${env.APIGEE_ENV}" \
                  -Dorg="${env.APIGEE_ORG}" \
                  -Dfile="${env.APIGEE_SA}" 
                """
            }
          }
        }
    }

}