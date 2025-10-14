pipeline {
    agent any

    environment {
        DEPLOY_HOST = '13.218.96.124' // EC2 server IP/DNS
        DOCKER_IMAGE = 'abdelhamedelbadawy/jenkinsbuiltapplication'
        GIT_REPO = 'github.com/AbdelhamedBadawy47/simple-spring-app-ci-pipeline.git'

    }

    tools {
        maven 'maven 3.9.11'
    }

    stages {
        stage("Versioning the Application") {
            steps {
                script {
                    echo "Versioning the application"
                    sh '''
                        mvn build-helper:parse-version versions:set \
                        -DnewVersion=${parsedVersion.majorVersion}.${parsedVersion.minorVersion}.${parsedVersion.nextIncrementalVersion} \
                        versions:commit
                    '''
                }
            }
        }

        stage("Build JAR Artifact") {
            steps {
                echo "Building the JAR artifact using Maven"
                sh 'mvn clean package'
            }
        }

        stage("Build and Push Docker Image") {
            steps {
                script {
                    echo "Building and pushing Docker image"

                    // Extract Maven project version dynamically
                    def version = sh(script: "mvn help:evaluate -Dexpression=project.version -q -DforceStdout", returnStdout: true).trim()
                    echo "Detected application version: ${version}"

                    // Export to env so other stages can use it
                    env.APP_VERSION = version

                    withCredentials([usernamePassword(credentialsId: 'dockerhubcredentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh '''
                            echo $PASSWORD | docker login -u $USERNAME --password-stdin
                            docker build -t ${DOCKER_IMAGE}:${APP_VERSION} .
                            docker push ${DOCKER_IMAGE}:${APP_VERSION}
                        '''
                    }
                }
            }
        }

        stage("Commit the Version updates") {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'githubcredentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh '''
                            git config user.email "jenkinsadmin@jenkins.com"
                            git config user.name "jenkinsadmin"
                            git status
                            git branch
                            git remote set-url origin https://${USERNAME}:${PASSWORD}@${GIT_REPO}
                            git add .
                            git commit -m "ci: Version release completed" || echo "No changes to commit"
                            git push origin HEAD:main
                        '''
                    }
                }
            }
        }

        stage("Deploy to Docker environment on EC2 server") {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhubcredentials', usernameVariable: 'DOCKERHUB_USER', passwordVariable: 'DOCKERHUB_PASS')]) {
                        sshagent(['ec2-deployment-server-credentials']) {
                            sh """
                                ssh -o StrictHostKeyChecking=no Docker@${DEPLOY_HOST} '
                                    echo "Logging into Docker Hub"
                                    echo "${DOCKERHUB_PASS}" | docker login -u "${DOCKERHUB_USER}" --password-stdin

                                    echo "Pulling Docker image"
                                    docker pull ${DOCKER_IMAGE}:${APP_VERSION}

                                    echo "Stopping any  previous version existing container"
                                    docker stop Spring-java-app || true
                                    docker rm Spring-java-app || true

                                    echo "Running new container"
                                    docker run -d --name Spring-java-app -p 3080:3080 ${DOCKER_IMAGE}:${APP_VERSION}
                                '
                            """
                        }
                    }
                }
            }
        }
    }
}
