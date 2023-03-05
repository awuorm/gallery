pipeline {
    agent any
    
    tools {nodejs "node"}
    
    stages {
        stage('Start') {
            steps {
                echo 'Build is starting'
            }
        }
        stage('Clone github repository') {
            steps {
                git url: 'https://github.com/awuorm/gallery', branch: 'master'
            }
        }
        stage('Get latest commit') {
            steps {
                bat '''
                   @echo off
				   git log -1 --pretty=%%B > %temp%/git.txt
                   for /f "Tokens=*" %%a in (%temp%/git.txt) do @set COMMIT=%%a

                   echo %COMMIT%
                   ::export %COMMIT%
                   '''
            }
        }
        stage('Install dependencies') {
            steps {
                bat '''
                   cd gallery
                   npm install
                   '''
            }
        }
        stage('Deploy') {
            steps {
                bat 'curl -X POST https://api.render.com/deploy/srv-cfvgls5269v0ptl5kct0?key=Nh6cvLwRu8M'
            }
        }
        stage('End') {
            steps {
                echo 'Build is finished'
            }
        }
        stage('Hello') {
            steps {
                echo "Hello world"
                    }
            }
            }
    post{
        failure{
            slackSend( channel: "#saf04-devops-g1", token: "slack_webhook token", color: "good", message: "${custom_msg()}")
        }
    }
        
    
}

def custom_msg()
{
  def JENKINS_URL= "localhost:8080"
  def JOB_NAME = env.JOB_NAME
  def BUILD_ID= env.BUILD_ID

  def JENKINS_LOG= " FAILED: Job [${env.JOB_NAME}] Logs path: ${JENKINS_URL}/job/${JOB_NAME}/${BUILD_ID}/consoleText"
  return JENKINS_LOG

}