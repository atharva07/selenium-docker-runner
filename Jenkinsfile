pipeline {
    agent any

    parameters {
        choice choices: ['chrome', 'firefox'], description: 'Select the browser', name: 'BROWSER'
    }

    stages {
        stage('Start grid') {
            steps {
                bat "docker-compose -f grid.yaml up --scale ${params.BROWSER}=2 -d"
            }
        }

        stage('Run Test') {
            steps {
                bat "docker-compose -f test-suites.yaml up"
                script {
                    if(fileExists('output/flight-reservation/testng-failed.xml')) {
                        error('failed test found')
                    }
                }
            }
        }
    }

    post {
        always {
            bat "docker-compose -f grid.yaml down"
            bat "docker-compose -f test-suites.yaml down"
            archiveArtifacts artifacts: 'output/flight-reservation/emailable-report.html', followSymlinks: false
        }
    }
}
