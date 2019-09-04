def imageStatus = 'UNKNOWN'


pipeline {

    agent any
    parameters {
        string(defaultValue: "repo01", description: 'this is the docker image name', name: 'dockerimagename')
        string(defaultValue: "testuser770770", description: 'this is the user name of docker hub', name: 'user_docker_hub')
        string(defaultValue: "docker-docker", description: 'this is the credentials id', name: 'cred_id')
       string(defaultValue: "master", description: 'this is the baranch's name, name: 'branch_name')

    }


    stages {
        stage('Building image') {
            steps {
                script {
                    sh "cd  docker"
                    sh "ls -ltrh "
                    docker.build("docker.io/${user_docker_hub}/" + "${dockerimagename}" + ":$BUILD_NUMBER", " ./docker/")

                }

            }

        }


        stage('dockerrun') {
            steps {
                sh " docker run -dit --name my_app -p 8090:80 docker.io/${user_docker_hub}/${dockerimagename}:${BUILD_NUMBER}"
            }
        }

        stage('Testing') {
            steps {
                script {
                    imageStatus = sh(returnStdout: true, script: 'docker ps | grep my_app | wc -l')
                }
            }
        }


        stage('clean all dockers') {
            steps {
                echo "${imageStatus}"
                sh " docker  stop my_app ;docker  rm -f  my_app"
            }
        }

        stage('Push docker  image') {
            when {
                branch '${baranch_name}'
            }

            steps {
                withDockerRegistry(credentialsId: "${cred_id}", url: '') {
                    sh 'docker push ${user_docker_hub}/${dockerimagename}:${BUILD_NUMBER}'
                }
            }
        }


    }

}


