pipeline {
  agent any

  environment {
    FRONTEND_IMAGE = "frontend:ingress"
    BACKEND_IMAGE  = "backend:ingress"
    PORT = "5000"
    MONGO_URI = "mongodb://mongo.devops-part3.svc.cluster.local:27017/taskdb"
    K8S_NAMESPACE = "devops-part3"
  }

  stages {
    stage('Checkout Code') {
      steps {
        git url: 'https://github.com/abuabddullah/mern-docker-kubernetes.git', branch: 'main'
      }
    }

    stage('Prepare .env') {
      steps {
        sh '''
          mkdir -p server
          cat > server/.env <<EOF
PORT=${PORT}
MONGO_URI=${MONGO_URI}
EOF
        '''
      }
    }

    stage('Build Docker Images') {
      steps {
        sh '''
          echo "Building backend image..."
          docker build -t ${BACKEND_IMAGE} ./server

          echo "Building frontend image..."
          docker build -t ${FRONTEND_IMAGE} ./client --build-arg VITE_API_URL=/api
        '''
      }
    }

    stage('Load Images to Minikube') {
      steps {
        sh '''
          minikube image load ${BACKEND_IMAGE}
          minikube image load ${FRONTEND_IMAGE}
        '''
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh '''
          kubectl apply -f k8s/00-namespace.yaml
          kubectl apply -f k8s/mongo-pvc.yaml
          kubectl apply -f k8s/secret-mongo.yaml
          kubectl apply -f k8s/configmap.yaml
          kubectl apply -f k8s/01-mongo.yaml
          kubectl apply -f k8s/02.3-backend-ingress.yaml
          kubectl apply -f k8s/03.3-frontend-ingress.yaml
          kubectl apply -f k8s/04-ingress.yaml
        '''
      }
    }

    stage('Verify Deployment') {
      steps {
        sh '''
          kubectl get pods -n ${K8S_NAMESPACE}
          kubectl get services -n ${K8S_NAMESPACE}
        '''
      }
    }
  }

  post {
    always {
      sh 'docker compose down || true'
    }
  }
}