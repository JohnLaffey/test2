pipeline {
  agent {
    node {
       label 'tp'
        }
  }      
  stages {
    stage('stageone') {
      environment {
        color = 'orange'
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
          sleep 10
          sh '/jenkins/telepresence.sh'
          sh '''
	  result=$(curl -s "http://localhost:3000/color")
	  result=$(echo $result | tr -d '///"')	 
	  echo $color
	  if [ $result != $color ]
	  then
          echo Values do not match, stopping pipeline
	  currentBuild.result = 'ABORTED'
          fi '''
        } 
        }
           }      
    }
  }
}
