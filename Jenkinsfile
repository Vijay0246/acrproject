pipeline {​​​​​
     agent any
     
        environment {​​​​​
        //once you create ACR in Azure cloud, use that here
        registryName = "digitalip"
        //- update your credentials ID after creating credentials for connecting to ACR
        registryCredential = 'ACR'
        dockerImage = ''
        registryUrl = 'digitalip.azurecr.io'
    }​​​​​
    stages {​​​​​


        stage ('checkout') {​​​​​
            steps {​​​​​
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Ramananaidu/acrproject.git']]])
            }​​​​​
        }​​​​​
        stage ('Build Docker image') {​​​​​
            steps {​​​​​
                
                script {​​​​​
                    dockerImage = docker.build registryName
                }​​​​​
            }​​​​​
        }​​​​​
        stage('Upload Image to ACR') {​​​​​
            steps{​​​​​   
                script {​​​​​
                docker.withRegistry( "http://${​​​​​registryUrl}​​​​​", registryCredential ) {​​​​​
                dockerImage.push()
            }​​​​​
        }​​​​​
      }​​​​​
    }​​​​​
     stage('stop previous containers') {​​​​​
         steps {​​​​​
            sh 'docker ps -f name=mypythonContainer -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=mypythonContainer -q | xargs -r docker container rm'
         }​​​​​
       }​​​​​
      
    stage('Docker Run') {​​​​​
     steps{​​​​​
         script {​​​​​
                sh 'docker run -d -p 8096:5000 --rm --name mypythonContainer ${​​​​​registryUrl}​​​​​/${​​​​​registryName}​​​​​'
            }​​​​​
      }​​​​​
    }​​​​​
    }​​​​​


    }​​​​​
 




