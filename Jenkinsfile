// @Library('atc-jenkins-shared-library')
// import com.bmw.atc.jenkins.steps.*

// def pipelineSteps = new PipelineSteps(this)
// def scmSteps = new SCMSteps(this)
// def emailSteps = new EmailSteps(this)
// def projectName = pipelineSteps.multiBranchDisplayName()

pipeline {
  agent any

  options {
    disableConcurrentBuilds()
    buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '5'))
  }

  environment {
    //AWS Environment Variables
    // AWS_ECR_ACCOUNT = 'XXXXXXXX'
    // AWS_ACCOUNT = 'XXXXXXXXXXX'
    //APP_NAME = dealerportal-traefik
    // AWS_EC2_REGION = ''
    // AWS_EC2_CRED = ''
    // AWS_ECR_REGION = 'ap-southeast-1'
    // ECR_BASE_URI = "${AWS_ECR_ACCOUNT}.dkr.ecr.${AWS_ECR_REGION}.amazonaws.com"
    // ECR_REPOSITORY = ''
    // ECR_CRED = ''
    //Application code specific Variables
    //APP_REPO_URL = ''
    NEXUS_RAW_URL = 'https://otd-ci.bmwgroup.net/nexus/repository/dealerport-site'
    //General Variables
    ENV = 'uat'
    //NEXUS_USER = credentials('132d17e5-db0e-47a2-826b-43caea4a3b67')
    PROXY_USER = credentials('43a41202-4326-4f6a-8a64-88071d1005ba')
    PROXY_URL = "http://$PROXY_USER@proxy.muc:8080"
    WORKDIR =''
  }
  stages {
        stage('Build and tag Docker Images') {
          steps {
            script {
              log.outputBanner('Downloading the artifcat from Nexus and dockerizing it')
                  echo 'Downloading zip from Nexus'
                  bat 'del "*.zip"'
                  WORKDIR = '$PWD'
                  //bat 'powershell -command "$Username = "qxz2d1w"'
                  //bat 'powershell -command "$Password = Get-Content “$WORKDIR\securestring\password.txt” | ConvertTo-SecureString"'
                  //bat 'powershell -command "$Cred = New-Object System.Management.Automation.PSCredential $Username,$Password"'
                  //bat "powershell -command 'Invoke-RestMethod -uri ${NEXUS_RAW_URL}/traefik/traefik.zip -Credential $Cred '"
                  bat 'powershell -command "Expand-Archive traefik.zip"'
                  echo 'Building traefik image for deployment'
                  bat "docker build \
                    --build-arg https_proxy=${PROXY_URL} \
                    -f build/${DOCKER_FILE} \
                    -t traefik ."
                  DOCKER_IMAGE_TAG = ""
                  echo 'Tagging images for AWS ECR'
                  //need to provide ecr repo
                  //bat "powershell -command "docker tag traefik ${ECR_BASE_URI}/${ECR_REPOSITORY}/:latest""
                  //bat "docker tag traefik ${ECR_BASE_URI}/${ECR_REPOSITORY}/:${DOCKER_IMAGE_TAG}"
                  //bat 'docker images'
                  bat 'del "*.zip"'
            }           
          }
        }
  }
}
