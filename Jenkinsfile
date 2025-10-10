pipeline {
    agent any

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
                sh 'mvn clean package -DskipTests'
            }
        }

        stage("Build and Push Docker Image") {
            steps {
                script {
                    echo "Building and pushing Docker image"

                    // Extract Maven project version dynamically
                    def version = sh(script: "mvn help:evaluate -Dexpression=project.version -q -DforceStdout", returnStdout: true).trim()
                    echo "Detected application version: ${version}"

                    withCredentials([usernamePassword(credentialsId: 'dockerhubcredentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh '''
                            echo $PASSWORD | docker login -u $USERNAME --password-stdin
                        '''
                        sh "docker build -t abdelhamedelbadawy/jenkinsbuiltapplication:${version} ."
                        sh "docker push abdelhamedelbadawy/jenkinsbuiltapplication:${version}"
                    }
                }
            }
        }
    }
}
