pipeline {
  agent { label 'podman' }
  environment {
    REGISTRY = 'registry.assign.miqbal37.300.ops:5000'
    IMAGE = 'bind-secondary'
  }
  stages {
    stage('Build') {
      steps {
        sh 'podman build -t $REGISTRY/$IMAGE:$BUILD_NUMBER .'
      }
    }
    stage('Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'registry-login', usernameVariable: 'REG_USER', passwordVariable: 'REG_PASS')]) {
          sh '''
            echo "$REG_PASS" | podman login "$REGISTRY" -u "$REG_USER" --password-stdin
            podman push "$REGISTRY/$IMAGE:$BUILD_NUMBER"
            podman logout "$REGISTRY"
          '''
        }
      }
    }
  }
}
