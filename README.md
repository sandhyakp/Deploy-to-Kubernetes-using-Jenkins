# Deploy-to-Kubernetes-using-Jenkins
Deploy-to-Kubernetes-using-Jenkins

1. Setting Up the CI Pipeline with Jenkins

 Step 1: Install Jenkins
Jenkins ko install karne ke liye pehle Jenkins ka installer download karein aur usse apne system par install karein. Jenkins ko apne local machine ya server par install kar sakte hain.

1. Download Jenkins
   - Visit [Jenkins website](https://www.jenkins.io/download/).
   - Select the appropriate installer for your operating system (Windows, macOS, Linux).

2. Install Jenkins
   - Follow the installation instructions for your operating system.
   - Start the Jenkins service.

 Step 2: Configure Jenkins
Jenkins install hone ke baad usse configure karein.

1. Initial Setup
   - Open a web browser and go to `http://localhost:8080` (default URL for Jenkins).
   - Enter the initial admin password from the `secrets` file created during installation.
   - Install suggested plugins or select plugins manually according to your requirements.

2. Create Admin User
   - Set up the first admin user with a username and password.

 Step 3: Install Necessary Plugins
Jenkins mein kuch zaroori plugins install karein jo CI pipeline ko build karne mein madad karte hain.

1. Go to Manage Jenkins
   - Navigate to `Manage Jenkins` > `Manage Plugins`.

2. Install Plugins
   - Search for and install the following plugins:
     - Git Plugin: For integrating with Git repositories.
     - Maven Integration Plugin: For running Maven builds.
     - Docker Plugin: For working with Docker containers.
     - Pipeline Plugin: For creating pipeline jobs.

 Step 4: Set Up a Jenkins Job
Jenkins job create karke CI pipeline setup karein.

1. Create a New Job
   - Go to `New Item`, enter a name for the job, and select `Pipeline`.

2. Configure the Pipeline
   - Under the `Pipeline` section, select `Pipeline script from SCM` if you want to store your pipeline script in a source control repository (e.g., Git).
   - Configure the SCM (Source Code Management) section with your repository details.
   - If you prefer to write the pipeline script directly in Jenkins, select `Pipeline script` and enter your script.

 Step 5: Define the Pipeline Script
Pipeline script likhein jo developer changes detect karega aur build aur test processes ko automate karega.

1. Basic Pipeline Script
   ```groovy
   pipeline {
       agent any
       stages {
           stage('Checkout') {
               steps {
                   git 'https://github.com/your-repo.git'
               }
           }
           stage('Build') {
               steps {
                   sh 'mvn clean install'
               }
           }
           stage('Test') {
               steps {
                   sh 'mvn test'
               }
           }
           stage('Build Docker Image') {
               steps {
                   script {
                       docker.build('your-image-name')
                   }
               }
           }
           stage('Push Docker Image') {
               steps {
                   script {
                       docker.withRegistry('https://index.docker.io/v1/', 'docker-credentials-id') {
                           docker.image('your-image-name').push('latest')
                       }
                   }
               }
           }
       }
   }
   ```

2. Explanation
   - `stage('Checkout')`: Git repository se code checkout karta hai.
   - `stage('Build')`: Maven ko use karke project build karta hai.
   - `stage('Test')`: Maven ko use karke unit tests run karta hai.
   - `stage('Build Docker Image')`: Docker image build karta hai.
   - `stage('Push Docker Image')`: Docker image ko Docker Hub par push karta hai.

 Step 6: Triggering Jenkins Jobs
Developer changes detect karne ke liye Jenkins jobs ko trigger karein.

1. Webhook Configuration
   - Go to your Git repository settings (e.g., GitHub).
   - Add a webhook with the URL `http://your-jenkins-url/github-webhook/`.
   - Select the events that should trigger the webhook (e.g., push events).

2. Poll SCM
   - Alternatively, you can configure Jenkins to poll the SCM at regular intervals.
   - Go to the `Build Triggers` section in your Jenkins job configuration and select `Poll SCM`.
   - Specify the schedule using cron syntax (e.g., `H/5 * * * *` for every 5 minutes).

 Step 7: Monitoring and Feedback
Build aur test results monitor karein aur developers ko feedback provide karein.

1. Email Notifications
   - Configure email notifications to alert developers about build status.
   - Go to `Manage Jenkins` > `Configure System` and set up the email server.

2. Build History and Console Output
   - Check the build history and console output in Jenkins to diagnose issues.
   - Navigate to the job page and click on the specific build number to view details.

Is tarah, aap Jenkins ke saath apni CI pipeline setup kar sakte hain jo developer changes ko detect karte hue automatically build aur test processes ko run karega, aur Docker images ko build aur push karega.



