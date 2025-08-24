def branchName     = params.BranchName ?: "main"
def gitUrl         = "git@github.com:ghadeerhamdan-cmd/slacktecthtml.git"
def gitUrlCode     = "git@github.com:ghadeerhamdan-cmd/slacktecthtml.git"
def serviceName    = "ghadeerecr"
def EnvName        = "dev"
def registryId     = "727245885999"
def awsRegion      = "ap-south-1"
def ecrUrl         = "727245885999.dkr.ecr.ap-south-1.amazonaws.com"
def dockerfile     = "Dockerfile"
def imageTag       = "${EnvName}-${BUILD_NUMBER}"
def ARGOCD_URL     = "https://127.0.0.1:8080/applications"
def slashtecDir    = "slacktecthtml"
// AppConfig Params
def applicationName = "Task Test App"
def envName = "dev"
def configName = "dev"
// Fix: Use string concatenation, not arithmetic
def clientId = "${applicationName}-${envName}"
def latestTagValue = params.Tag
def namespace = "dev"
def helmDir = "/helm"

node {
  withCredentials([string(credentialsId: 'foodics-slack-online-deployments', variable: 'SLACK_WEBHOOK')]) {
    try {
      notifyBuild('STARTED')
      stage('cleanup') {
        cleanWs()
      }
      stage ("Get the app code") {
        checkout([$class: 'GitSCM', branches: [[name: "${branchName}"]] , extensions: [], userRemoteConfigs: [[ url: "${gitUrlCode}"]]])
        sh "rm -rf ~/workspace/\"${JOB_NAME}\"/slashtec"
        sh "mkdir ~/workspace/\"${JOB_NAME}\"/slashtec  ; cd slashtec ; git clone -b main ${gitUrl} "
        sh("cp ${slashtecDir}/docker/Dockerfile ${dockerfile}")
        sh("cp -r  ${slashtecDir}/docker/* .")
        sh("cp -r  ${slashtecDir}/files/* .")
      }
      stage("Get the env variables from App") {
        sh "aws appconfig get-configuration --application ${applicationName} --environment ${envName} --configuration ${configName} --client-id ${clientId} .env --region ${awsRegion}"
      }
      stage('login to ecr') {
        sh("aws ecr get-login-password --region ${awsRegion}  | docker login --username AWS --password-stdin ${ecrUrl}")
      }
      stage('Build Docker Image') {
        sh("docker build -t ${ecrUrl}/${serviceName}:${imageTag} -f ${dockerfile} .")
      }
      stage('Push Docker Image To ECR') {
        sh("docker push ${ecrUrl}/${serviceName}:${imageTag}")
      }
      stage('Clean docker images') {
        sh("docker rmi -f ${ecrUrl}/${serviceName}:${imageTag} || :")
      }
      stage ("Deploy ${serviceName} to ${EnvName} Environment") {
        sh ("cd slashtec/${helmDir}; pathEnv=\".deployment.image.tag\" valueEnv=\"${imageTag}\" yq 'eval(strenv(pathEnv)) = strenv(valueEnv)' -i values.yaml ; cat values.yaml")
        sh ("cd slashtec/${helmDir}; git pull ; git add values.yaml; git commit -m 'update image tag' ;git push ${gitUrl}")
      }
    } catch (e) {
      currentBuild.result = "FAILED"
      notifyBuild(currentBuild.result)
      throw e
    }
  }
}

def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus = buildStatus ?: 'SUCCESSFUL'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}