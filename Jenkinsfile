pipeline {
    agent any
    stages {
        stage('Example') {
            steps {
                echo 'Building Docker image'
                sh './build'
            }
        }
    }
    post {
        always {
            echo 'Now What'
        }
    }
}
