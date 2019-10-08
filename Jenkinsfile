// Jenkinsfile
String credentialsId = 'awsCredentials'
String secretName = 'GitHubToken'


try {
  stage('checkout') {
    node {
      cleanWs()
      checkout scm
    }
  }

  // Run terraform init
  stage('init') {
    node {
      withCredentials([[
        $class: 'AmazonWebServicesCredentialsBinding',
        credentialsId: credentialsId,
        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'     
      ]]) {
        ansiColor('xterm') {
          sh 'terraform init'
        }
      }
    }
      // Run terraform init2
    node {
      withCredentials([[
        $class: 'AmazonWebServicesCredentialsBinding',
        secretName:secretName,
      
      ]]) {
        ansiColor('xterm') {
          sh 'terraform init'
        }
  }

  // Run terraform plan
  stage('plan') {
    node {
      withCredentials([[
        $class: 'AmazonWebServicesCredentialsBinding',
        credentialsId: credentialsId,
        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
      ]]) {
        ansiColor('xterm') {
          sh 'terraform plan'
        }
      }
    }
 // Run terraform plan 2
     node {
      withCredentials([[
        $class: 'AmazonWebServicesCredentialsBinding',
        secretName:secretName,
       
      ]]) {
        ansiColor('xterm') {
          sh 'terraform init'
        }
  }

  if (env.BRANCH_NAME == 'master') {

    // Run terraform apply
    stage('apply') {
      node {
        withCredentials([[
          $class: 'AmazonWebServicesCredentialsBinding',
          credentialsId: credentialsId,
          accessKeyVariable: 'AWS_ACCESS_KEY_ID',
        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
        ]]) {
          ansiColor('xterm') {
            sh 'terraform apply -auto-approve'
          }
        }
      }
    // Run terraform apply 2
       node {
      withCredentials([[
        $class: 'AmazonWebServicesCredentialsBinding',
        secretName:secretName
      ]]) {
        ansiColor('xterm') {
          sh 'terraform init'
        }
    }

    // Run terraform show
    stage('show') {
      node {
        withCredentials([[
          $class: 'AmazonWebServicesCredentialsBinding',
          credentialsId: credentialsId,
          accessKeyVariable: 'AWS_ACCESS_KEY_ID',
        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
        ]]) {
          ansiColor('xterm') {
            sh 'terraform show'
          }
        }
      }
   // Run terraform show 2  
   node {
      withCredentials([[
        $class: 'AmazonWebServicesCredentialsBinding',
        secretName:secretName
      ]]) {
        ansiColor('xterm') {
          sh 'terraform init'
        }
    }
  }
  currentBuild.result = 'SUCCESS'
}
catch (org.jenkinsci.plugins.workflow.steps.FlowInterruptedException flowError) {
  currentBuild.result = 'ABORTED'
}
catch (err) {
  currentBuild.result = 'FAILURE'
  throw err
}
finally {
  if (currentBuild.result == 'SUCCESS') {
    currentBuild.result = 'SUCCESS'
  }
}
