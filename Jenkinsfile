pipeline {
  options {
    buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '0'))
  }
  agent {
    label 'DanielImage'
  }
  parameters {
    choice(
      choices: ['deploy' , 'release'],
      description: '',
      name: 'FLOW_CONTEXT'
    )
  }
  stages {
    stage('Run Maven') {
      steps {
        script{
          properties([[$class: 'JiraProjectProperty', siteName: 'https://testsitejavi.atlassian.net/'], [$class: 'DatadogJobProperty', tagFile: '', tagProperties: ''], [$class: 'RebuildSettings', autoRebuild: false, rebuildDisabled: false], [$class: 'JobRestrictionProperty'], pipelineTriggers([pollSCM('')])])
        }
          sh 'mvn deploy -f ./complete/pom.xml -DskipTests'
      }
    }
    stage ('Run Sonarqube'){
      steps{
          sh 'mvn sonar:sonar -f ./complete/pom.xml -DskipTests'
        }
    }
  }
  post {
    success {
      script {
        if (params.FLOW_CONTEXT == 'deploy') {
          step([$class: 'ElectricFlowPipelinePublisher',
            configuration: 'flow-sko-jenkins-config',
            projectName: 'user23',
            pipelineName: 'user23',
            addParam: '{"pipeline":{"pipelineName":"user23","parameters":"[{\\\"parameterName\\\": \\\"jenkinsJobName\\\", \\\"parameterValue\\\": \\\"'+"${env.JOB_NAME}"+'\\\"},{\\\"parameterName\\\": \\\"jenkinsBuildNumber\\\", \\\"parameterValue\\\": \\\"'+"${env.BUILD_NUMBER}"+'\\\"}]"}}'
          ])
        }
      }
    }
   }
}
