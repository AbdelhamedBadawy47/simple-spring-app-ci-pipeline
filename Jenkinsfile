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
        stage("Commit the Version updates"){
            steps{

                script{

                    withCredentials([usernamePassword(credentialsId: 'gitlabcredentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')])
                    
                    sh 'git config username.email "abdelhamed47@gmail.com" '
                    sh 'git config user.name "AbdelhamedBadawy47"'
                    sh 'git status'
                    sh 'git branch'
                    sh "git remote set-url origin https://${USERNAME}:${PASSWORD}@https://github.com/AbdelhamedBadawy47/simple-spring-app-ci-pipeline.git"
                    sh 'git add .'
                    sh 'git commit -m "ci ,Version release completed" '
                    sh 'git push origin HEAD:main'
                    sh 'git'
                     
                }
            }

        }
    }
}
