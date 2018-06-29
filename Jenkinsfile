node {
  try {
    stage('Checkout') {
      checkout scm
    }
    stage('Environment') {
      sh 'git --version'
      echo "Branch: ${env.BRANCH_NAME}"
      sh 'docker -v'
      sh 'printenv'
    }
    stage('Build'){
      if(env.BRANCH_NAME == 'master'){
        sh 'docker build -t app --no-cache .'
        sh 'docker tag app localhost:5000/app'
        sh 'docker push localhost:5000/app'
      }
    }
    stage('Deploy'){
      if(env.BRANCH_NAME == 'master'){
        sh 'docker pull localhost:5000/app'
        sh 'docker run -d -p 8090:8090 --name app localhost:5000/app:latest'
        sh 'docker rmi -f app localhost:5000/app'
      }
    }
  }
  catch (err) {
    throw err
  }
}
