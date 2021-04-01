pipeline {
  agent {
    node {
       label 'tp'
        }
  }      
  stages {
    stage('stageone') {
      environment {
        color = 'purple'
      }
      steps {
        withKubeConfig([credentialsId: 'e1edc5dd-52de-42fe-9451-732149a23353',
                    serverUrl: 'https://192.168.0.46:6443',
                    contextName: 'kubernetes-admin@kubernetes',
                    clusterName: 'kubernetes',
                    namespace: 'default'
                       ])
        {
	sh 'export KUBECONFIG=/home/kube/.kube/config'	
	sh 'echo $KUBECONFIG'
	sh 'kubectl'
        }
           }      
    }
  }
}
