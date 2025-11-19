# Jenkins CI/CD Pipeline Assignment

# Open Cloud Jenkins
https://jenkinsacademics.herovired.com/

# Install required Jenkins plugins
Manage Jenkins → Plugins → Available Plugins <br>
Install: <br>

. GitHub Integration <br>
. Git Plugin <br>
. Pipeline <br>
. Email Extension Plugin <br>

# Fork and Prepare the Application Repository
https://github.com/durganaresh83/flask_Practice.git

# Add a Jenkinsfile in Your Repository
Create a Jenkinsfile in the root directory of your project: <br>

pipeline { <br>
    agent any <br>

    stages { <br>

        stage('Build') { <br>
            steps { <br>
                echo "Installing dependencies..." <br>
                sh 'pip3 install -r requirements.txt' <br>
            } <br>
        } <br>

        stage('Test') { <br>
            steps { <br>
                echo "Running tests..." <br>
                sh 'pytest || exit 1' <br>
            } <br>
        } <br>

        stage('Deploy') { <br>
            when { <br>
                expression { <br>
                    currentBuild.result == null || currentBuild.result == 'SUCCESS' <br>
                } <br>
            } <br>
            steps { <br>
                echo "Deploying Flask app to staging environment..." <br>
                sh ''' <br>
                    # Example simple deployment <br>
                    nohup python3 app.py & <br>
                ''' <br>
            } <br>
        } <br>
    } <br>

    post { <br>
        success { <br>
            emailext subject: "Build Successful: ${env.JOB_NAME}", <br>
                     body: "The Jenkins build was successful.", <br>
                     recipientProviders: [developers()] <br>
        } <br>
        failure { <br>
            emailext subject: "Build Failed: ${env.JOB_NAME}", <br>
                     body: "The Jenkins build has failed.", <br>
                     recipientProviders: [developers()] <br>
        } <br>
    } <br>
} <br>

# Create a Jenkins Pipeline Job
**Steps:**

. Open Jenkins → New Item
. Choose Pipeline
. Name it: durga-flask-ci-cd
. Under Pipeline → Definition → choose Pipeline script from SCM
. SCM → Git

<img width="1592" height="887" alt="image" src="https://github.com/user-attachments/assets/217df100-9f80-4c2a-819b-ac271dab3b15" />

