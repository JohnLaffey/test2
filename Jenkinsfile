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
                    caCertificate: '-----BEGIN CERTIFICATE-----MIIC5zCCAc+gAwIBAgIBADANBgkqhkiG9w0BAQsFADAVMRMwEQYDVQQDEwprdWJlcm5ldGVzMB4XDTIxMDMyMzIxNTMzMFoXDTMxMDMyMTIxNTMzMFowFTETMBEGA1UEAxMKa3ViZXJuZXRlczCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBALXxx6OyFIU5h2pjTAwyGH4K1LRcH63maxapvyXZl6ISWSRes0aLNGd+nUiQcof2MHPNplYfecAsQvQa82rcN3Dj1VXzg5wWs5JaNsi31Oo32fSatBr8s9rznWWxEkgPoWA2vVf3u8LUAhqY1LBhVUDvp6LJN9/XygN31Kv8D0aGvYZhs7y+sYwaNAaHIMrAgkwxDUIQMi/QvPLyQS1TKORPIuy/+vkFaZU8WG8lz94Dl+JoYAYMyNx5I3zZYXLWSOKwbwMXKY1uFbm4zxzv35BsjwDIKXzpfLr35Y6KaXdpNVWqelA9CT9SXkKihNNF2Wf5Nar0CDvgsXXiMvQoxqUCAwEAAaNCMEAwDgYDVR0PAQH/BAQDAgKkMA8GA1UdEwEB/wQFMAMBAf8wHQYDVR0OBBYEFGIocr7dCdiHuetCpstmW24gisX3MA0GCSqGSIb3DQEBCwUAA4IBAQBnukRGqYy/iRDXj841nBsuN3An3WJxHfoL1ZNtEenfX5fA38fWE5+m7QKOUQbDtk4g49+a4SKdAllJkIwehLbZisPaIpz1p9SJ8Ao9fNepT+4rn//6Eqbex2Xj0UQRCOdUb21WNFo56rVa0+rtNlRSQsZkQh+Yecq43YEAWVQPrp1SJSQGxWrtuHo84TbJmGpTfDOgz25OvZSbvW0a15n311+86gc7F2T1qjrLEOW6LnK3+fqywyzTWfa0eqwAt7A0UpH8qx9EQCEgYDuctECQkKPqj4lyFeUA0JcnnxqPyZrOh+cNRIUQcn2dy1lulCj+0plRJDf7tlkEF8bKSKgF-----END CERTIFICATE-----',
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
          sh 'telepresence connect'
          sleep 10
          sh 'telepresence intercept dataprocessingservice --port 3000 --mount=false'
          sleep 10
          sh 'result=$(curl -s "http://localhost:3000/color")'
        } 
        }
      }         

          }
    stage('stagetwo'){
        steps {
           sh 'result=$(echo $result | tr -d '///"')'
           sh ''' if [[ "$color" != $result ]]
              then                
              currentBuild.result = 'ABORTED'
              error ('Values do not match, stopping pipeline')''' 
              }     
    }      
          }
  post {
    unsuccessful {
     echo 'This build has failed.'
    }        
 }
}
