pipeline {
  agent {
    node {
       label 'tp'
        }
  }      
  stages {
    stage('stageone') {
      environment {
        color = 'blue'
 //       buildResult = 'SUCCESS'      
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
          sh 'echo "The Test Value is " $color'
	  sh 'curl -v -u admin:supersecure --upload-file app.js http://192.168.0.58:8081/repository/MyRawHosted/'	  
          sh 'npm install&'
          sleep 10
          sh '(npm run start&)'
          sleep 10
          sh '/jenkins/telepresence.sh'
          sh '''
	  result=$(curl -s "http://localhost:3000/color")
	  result=$(echo $result | tr -d '///"')	 
	  if [ $result != $color ]
	  then          echo Values do not match, stopping pipeline
          currentBuild.result='FAILURE'
          fi '''
        } 
        }
           }      
    }
  }
}
