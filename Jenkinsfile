pipeline {
    environment {
      registry = "registry.cn-hangzhou.aliyuncs.com/sunnylua/learning"
      registryCredential = 'a8ca8156-0a2e-47a7-9ee5-cb169aab989a'
	  dockerImage = ''
	  tag = '1.0'
    }
	agent none
    stages {
        stage('Build Package') {
			agent {
				docker {
					image 'maven'
					args '-v /root/.m2:/root/.m2'
				}
			}
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
		stage('Build Image') {
			steps {
			    script {
				    dockerImage = docker.build registry + ":$tag"
			    }
			}
		}
        stage('Push image') { 
            steps { 
                script { 
                    docker.withRegistry(registry, registryCredential ) { 
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Clean image') { 
            steps { 
                sh "docker rmi $registry:$tag"
            }
        }
    }
}