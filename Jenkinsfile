pipeline {
  agent any
  
  environment {
        DOCKER_HOST = 'tcp://10.128.0.2:2425'
        DOCKER_COMPOSE_FILE = 'ci_cd/docker-compose.yml'
        DOCKER_REGISTRY = 'asia.gcr.io/trq1-161205'
        DOCKER_IMAGE = 'hello-springboot-app'
        VERSION = "1.0.${BUILD_NUMBER}"
        DOCKER_STACK = 'hello-springboot'
  }

  stages {
    stage("Ready") {
      steps {
        sh "cd hello-springboot-master && ./gradlew clean build"
      }
    }
    stage("Unit") {
      steps {
        echo "Unit testing phase."
      }
    }
    stage("Integ") {
      steps {
        echo "Integration testing phase."
        sh "cd hello-springboot-master && ./gradlew test"
      }
    }
    stage("Publish") {
      steps {
        sh "/usr/local/bin/docker-compose -f ${DOCKER_COMPOSE_FILE} build app"
        sh "docker tag ${DOCKER_IMAGE} ${DOCKER_REGISTRY}/${DOCKER_IMAGE}"
        sh "docker tag ${DOCKER_IMAGE} ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${VERSION}"
      }
    }
    stage("Prod-like") {
      steps {
        echo "A production-like cluster is yet to be created"
      }
    }
    stage("Production") {
      steps {
        echo "Producton phase."
        sh "sudo docker service update --image ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${VERSION} ${DOCKER_STACK}_app"
      }
    }
  }
  
  post {
    always {
        echo "This pipeline has finished."
        deleteDir()
    }
  }
}
