// pipeline {
//   agent any

//   environment {
//     FRONTEND_IMAGE = "mern-frontend:jenkins"
//     BACKEND_IMAGE  = "mern-backend:jenkins"
//     PORT = "5000"
//     MONGO_URI = "mongodb://mongo:27017/taskdb"
//   }

//   stages {
//     stage('Checkout Code') {
//       steps {
//         git url: 'https://github.com/sangammukherjee/devops-youtube-course-2025.git', branch: 'main'
//       }
//     }

//     stage('Prepare .env') {
//       steps {
//         sh '''
//           mkdir -p server
//           cat > server/.env <<EOF
// PORT=$PORT
// MONGO_URI=$MONGO_URI
// EOF
//         '''
//       }
//     }

//     stage('Build Docker Images') {
//       steps {
//         sh '''
//           echo "Building backend image..."
//           docker build -t $BACKEND_IMAGE ./server

//           echo "Building frontend image..."
//           docker build -t $FRONTEND_IMAGE ./client --build-arg VITE_API_URL=http://localhost:5000/api
//         '''
//       }
//     }

//     stage('Run with Docker Compose') {
//       steps {
//         sh '''
//           echo "Starting MERN stack with Docker Compose..."
//           docker compose up -d

//           echo "Showing running containers..."
//           docker ps

//           echo "===== Backend Logs ====="
//           docker logs backend || true

//           echo "===== Frontend Logs ====="
//           docker logs frontend || true
//         '''
//       }
//     }
//   }
// }



pipeline {
  agent any

  environment {
    FRONTEND_IMAGE = "frontend:ingress"
    BACKEND_IMAGE  = "backend:ingress"
    PORT = "5000"
    MONGO_URI = "mongodb://mongo.devops-part3.svc.cluster.local:27017/taskdb"  // K8s service name দিয়ে আপডেট করুন
    K8S_NAMESPACE = "devops-part3"
  }

  stages {
    stage('Checkout Code') {
      steps {
        git url: 'https://github.com/abuabddullah/mern-docker-kubernetes.git', branch: 'main'  // আপনার রেপো URL দিন
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
          docker build -t ${FRONTEND_IMAGE} ./client --build-arg VITE_API_URL=/api  // K8s-এ backend service-এ পয়েন্ট করুন
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
          kubectl apply -f k8s/02.3-backend-ingress.yaml  // আপনার YAML ফাইলগুলো অর্ডার অনুসারে
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
      sh 'docker compose down || true'  // যদি ডকার-কম্পোজ চালু থাকে, ক্লিনআপ
    }
  }
}