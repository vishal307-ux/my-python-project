pipeline{
    agent any

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
                sh 'pip install -r path/to/requirements.txt'
            }
        }
    }

}