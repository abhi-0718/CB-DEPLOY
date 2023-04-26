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
                sh 'docker tag maven-hello-world:latest 795361990663.dkr.ecr.us-east-1.amazonaws.com/maven-hello-world:latest'
                // sh 'docker tag maven-hello-world:latest public.ecr.aws/w5g9n1i7/maven-hello-world:latest'
            }
        }
       stage('Push image to ECR') {
            steps{
                script{
                    echo '---------------- Deploying Image----------------------------------------'
                    docker.withRegistry('https://795361990663.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:aws-credentials') {
                        sh 'docker push 795361990663.dkr.ecr.us-east-1.amazonaws.com/maven-hello-world:latest'
                        // sh 'docker push public.ecr.aws/w5g9n1i7/maven-hello-world:latest'
                        echo '-------------------------Image Successfully pushed---------------------------------'
                    }
                } 
            }
        }

        // stage('Installing neccessory things for K8s Deployment on machine'){
        //     steps{
        //         script{
        //             echo'----------------------Checking AWS CLI version-------------------'
        //             sh'aws --version'

        //             echo '---------------------Installing KubeCtl------------------'
        //             sh 'curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.16.8/2020-04-16/bin/linux/amd64/kubectl'
        //             sh 'chmod +x ./kubectl'
        //             sh 'mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin'
        //             sh 'kubectl version --short --client'
                
        //             echo'-----------------------Installing EksCtl------------------'
        //             sh 'curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp'
        //             sh 'sudo mv /tmp/eksctl /usr/bin'
        //             sh 'eksctl version'
        //         }
        //     }
        // }

        // stage('Creating Cluster'){
        //     steps{
        //         script{
        //             echo '----------------Creating 3 cluster in us-east-1 region with (cluster name=dev)--------------'
        //             sh 'eksctl create cluster --name dev --region us-east-1 --nodegroup-name standard-workers --node-type t3.medium --nodes 3 --nodes-min 1 --nodes-max 4 --managed'
        //         }
        //     }
        // }


        stage('Deploy'){
            steps{
                sh 'kubectl apply -f hello_world-svc.yml'
                sh 'kubectl get svc'
            }
        }
    }
}



