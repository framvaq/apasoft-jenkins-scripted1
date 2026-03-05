node('java&&dev') {
  stage('Get git repo') {
    // en las scripted se debe indicar para que se traiga el resto del repositorio
    git branch: 'main', url: 'https://github.com/framvaq/apasoft-jenkins-scripted1'
  }

  stage('Compile') {
    echo 'Compile'
    sh 'mvn compile'
  }

  stage('Test') {
    echo 'Compile'
    sh 'mvn test'
  }

  stage('Run') {
    echo 'Run'
    sh 'mvn exec:java -Dexec.mainClass="com.apasoft.CardProcessor" -Dexec args="4111111111111111"'
    // to send to another node
    stash includes: 'target/**', name: 'target-jar'
  }
}

node('java&&pro') {
  stage('Deploy') {
    // unpack artifact
    unstash 'target-jar'
    echo 'Deploying...'
    // copy artifact to ~/app on prod node
    sh 'rm -rf /home/jenkins/app/'
    sh 'mkdir -p /home/jenkins/app/'
    sh 'cp -r target/* /home/jenkins/app/'
    echo 'Deployment completed'
  }
}
