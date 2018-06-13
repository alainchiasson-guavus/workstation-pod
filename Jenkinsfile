pipeline {
    agent any
    stages {
        stage('Example') {
            steps {
                echo 'Building Docker image'
                ./build
            }
        }
    }
    post { 
        always { 
            echo 'Now What'
        }
    }
}
