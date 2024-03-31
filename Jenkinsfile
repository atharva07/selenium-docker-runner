pipeline {
    agent any

    stages {
        stage('Start grid') {
            steps {
                bat "docker-compose -f grid.yaml up -d"
            }
        }

        stage('Bring Grid Down') {
            steps {
                bat "docker-compose -f test-suites.yaml up"
            }
        }
    }

    post {
        always {
            bat "docker-compose -f grid.yaml down"
            bat "docker-compose -f test-suites.yaml down"
        }
    }
}
