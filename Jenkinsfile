pipeline{
    agent{
        label 'Master'
    }
    tools { 
        maven 'maven3'
    }
    stages
    {
        stage("Clean")
        {
            steps{
                sh "mvn clean"
            }
        }
        
        stage("Build")
        {
            steps{
                sh "mvn compile"
            }
            
        }
        
        stage("Test")
        {
            steps{
                sh "mvn test"
            }
        }
        
        stage("Package")
        {
            when
            {
                branch 'Production';   
            }
            steps{
                sh "mvn package"
            }
        }
        
        stage("Deploy")
        {
            when
            {
                branch 'Production';   
            }
            steps{
                sshagent(['shashwat']) {
                sh "scp -o StrictHostKeyChecking=no  /var/lib/jenkins/workspace/jenkinscapstone_Production/target/hello-0.0.1-SNAPSHOT.jar ubuntu@35.154.208.141:/home/ubuntu/"
                sh "ssh java -jar file,jar &"
                }
            }
        } 
    }
    post {
        always{
            mail to: 'shashwat.sst@gmail.com',
            subject: "Capstone Pipeline Status",
            body: "Your build was ${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}"
        }
        success {
            echo "Application build succesfully"
        }
        failure {
            echo "Application build failed"
        }
    }

}