pipeline{
    agent any
    environment {
            // Docker image details
            DOCKER_REGISTRY = 'vishalcloud307'                    // Docker Hub registry username
            IMAGE_NAME = 'myapp'                           // Docker image name
            // K8S_CLUSTER = 'minikube'                               // Minikube context name
            // K8S_NAMESPACE = 'default'                              // Kubernetes namespace

            // GIT Configuration
            // GIT_REPO_NAME = "java-project"                         // Git repo name
            // GIT_USER_NAME = "vishal307-ux"                         // Git user name
            // GIT_EMAIL = "vishalguptav307@gmail.com"                // Git user email

            // Docker image versioning based on Jenkins build number
            DOCKER_IMAGE = "${DOCKER_REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
            REGISTRY_CREDENTIALS = credentials('docker-cred')      // Docker registry credentials
    }

    stages {
        stage('checkout') {
            steps {
                 git branch: 'main', url: 'https://github.com/vishal307-ux/my-python-project.git'
            }
        }
        stage('OWASP Dependency Check') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DC'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
                
            }
        }
        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'pip3 install -r requirements.txt'
            }
        }
        stage('Build and Push Docker Image') {
            steps {
                script {
                // Build Docker image from the app's Dockerfile
                    sh 'cd my-python-project && docker build -t ${DOCKER_IMAGE} .'

                    // Push the Docker image to the registry
                    def dockerImage = docker.image("${DOCKER_IMAGE}")
                        docker.withRegistry('https://index.docker.io/v1/', "docker-cred") {
                            dockerImage.push()
                     }
                }
            }
        }
    }

}