@Library("shared") _
pipeline{
    
    agent { label  "sonika" }
    
    stages{
        stage("Hello"){
            steps{
                script{
                    hello()
                }
            }
        }
        stage("Code"){
            steps{
                script{
                clone("https://github.com/sonikasharma01/django-notes-app.git" , "main")
                }
            }
        }
        stage("Build"){
            steps{
                script{
                    docker_build("notes-app","latest","sonikasharma")
                }
            }
        }
        stage("Push to DockerHub"){
            steps{
                script{
                    docker_push("notes-app","latest","sonikasharma")
                }
            }
        }
        stage("Test"){
            steps{
                echo "This is testing the code"
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d"
            }
        }

        post{
        always{
            emailext(
                subject: "Jenkins Build '${env.JOB_NAME}' #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
                body: """
                    <p>Build Status: ${currentBuild.currentResult}</p>
                    <p>Job: ${env.JOB_NAME}</p>
                    <p>Build Number: ${env.BUILD_NUMBER}</p>
                    <p>Check console output at: ${env.BUILD_URL}</p>
                """,
                to: "your-receiver-email@example.com",
                mimeType: "text/html"
            )
            }
        }
    }
}
