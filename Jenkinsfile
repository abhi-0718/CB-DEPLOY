// pipeline {
//     environment{
//         dockerImage = ''
//     }
//     agent any
//     tools {
//         maven "Maven"
//         git "Git"
//         jdk "Jdk"
// 	dockerTool "Docker"
//     }

//     stages {
//         stage('1.Code Build and Analysis of Code') {
//             steps {
// 				echo '-----------------Building code-----------------------'
                
                
//                 withSonarQubeEnv('sonarserver'){
//                     withMaven(maven:'Maven'){
//                         bat 'mvn clean package sonar:sonar'
//                         echo '---------------Code Build and analysis of code is successfull---------------------'
//                     }
// 				}
//             }

           
//     	}


// 		stage('Quality Gate Check'){
// 			steps{
// 				timeout(time: 1, unit: 'HOURS') {
//                     waitForQualityGate abortPipeline: true
//                 }
// 			}
// 		}

//         stage('3.Building image') {
//             steps{
//                 script {
//                     echo '---------------------------Building Image----------------------------------'
//                     bat 'docker build . -t cloudbased-deployment'
//                     echo '---------------------------Image Successfully Build---------------------------------'
// 		            bat 'docker images'
//                 }
//             }
//         }

//         stage('4.Deploying image to ECR') {
//             steps{
//                 script{
//                     echo '-----------------------------Deploying Image----------------------------------------'
//                     docker.withRegistry('https://795361990663.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:aws-credentials') {
//                         bat 'docker push 795361990663.dkr.ecr.us-east-1.amazonaws.com/cloudbased-deployment:latest'
//                         echo '-------------------------Image Successfully pushed--------------------------------'
//                     }
//                 } 
//             }
//         }
// 	}

// }


pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        // stage('SonarQube Scan') {
        //     steps {
        //         withSonarQubeEnv('SonarQube') {
        //             sh 'mvn sonar:sonar'
        //         }
        //     }
        // }
        stage('Build Docker Image') {
            steps {
                echo "------------------BUILDING IMAGE USING DOCKERFILE ---"
                sh 'docker build -t maven-hello-world .'
                echo "------------------TAGGING THE IMAGE------------------"
                sh 'docker tag maven-hello-world:latest 795361990663.dkr.ecr.us-west-1.amazonaws.com/maven-hello-world:latest'
            }
        }
       stage('4.Push image to ECR') {
            steps{
                script{
                    echo '---------------- Deploying Image----------------------------------------'
                    docker.withRegistry('https://795361990663.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:aws-credentials') {
                        sh 'docker push 795361990663.dkr.ecr.us-west-1.amazonaws.com/maven-hello-world:latest'
                        echo '-------------------------Image Successfully pushed---------------------------------'
                    }
                } 
            }
        }
    }
}



