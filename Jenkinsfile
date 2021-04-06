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
	  dir("/home/kube/edgey-corp-nodejs/DataProcessingService")  {
          sh 'npm install&'
          sleep 10
          sh '(npm run start&)'
          sleep 15
          sh 'whoami'
          sleep 10
//          sh 'telepresence intercept dataprocessingservice --port 3000 --mount=false'
          sleep 10
          sh 'result=$(curl -s "http://localhost:3000/color")'
	  sh 'echo $result'
	  sh 'echo $color'
	  sh ''' if [[ "$color" != $result ]]
              then                
              currentBuild.result = 'ABORTED'
              error ('Values do not match, stopping pipeline')'''	  
        } 
        }
           }      
    }
  }
}
