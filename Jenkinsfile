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
                sh "scp -o StrictHostKeyChecking=no  /var/lib/jenkins/workspace/Eval-Jenkins_Production/target/*.jar ubuntu@:/home/ubuntu/"
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