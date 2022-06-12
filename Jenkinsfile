pipeline{
          agent {
              docker {
              image 'maven'
              args '-v $HOME/.m2:/root/.m2'
              }
          }

        stage{
            stage('Quality Gate Status Check'){
            steps{
              script{
                withSonarQubeEnv(credentialsId: 'Sonar-jenkins'){
                sh 'mvn sonar:sonar'
                }
                timeout(time: 1, unit: 'HOURS'){}
                def qg = waitForQualityGate()
                    if (qg.status !='ok'){
                      error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
                    sh "mvn clean install"
              }
            }
            }
        }
        }
