// Jenkinsfile - Scripted Pipeline (React)
node {
  sh 'rm -f pipeline.log || true; echo "Start: $(date -u)" | tee -a pipeline.log'

  stage('Checkout'){ 
    checkout scm 
    sh 'echo "Checked out $(git rev-parse --abbrev-ref HEAD) @ $(git rev-parse --short HEAD)" | tee -a pipeline.log'
   }

  stage('Build'){
    docker.image('node:lts-buster-slim').inside {
      sh '''
        node -v | tee -a pipeline.log
        npm -v  | tee -a pipeline.log
        npm ci  2>&1 | tee -a pipeline.log
        npm run build 2>&1 | tee -a pipeline.log
      '''
    }
  }

  stage('Test'){
    docker.image('node:lts-buster-slim').inside {
      sh 'npm test -- --watchAll=false 2>&1 | tee -a pipeline.log'
    }
  }

  try { } finally {
    archiveArtifacts artifacts: 'pipeline.log', fingerprint: true, onlyIfSuccessful: false
  }
}