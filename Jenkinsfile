pipeline {
  options {
    buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '0'))
  }
  agent {
    label {"maven"}
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
          sh 'mvn deploy -f ./complete/pom.xml'
      }
    }
    stage ('Run Sonarqube'){
      steps{
          sh 'mvn sonar:sonar -f ./complete/pom.xml'
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
