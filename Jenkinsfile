pipeline {
    agent {
        Label "windows"
    }

    stages {
        stage('Build') {
            steps {
                write-host 'Building..'
            }
        }
        stage('Test') {
            steps {
                write-host 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                write-host 'Deploying....'
            }
        }
    }
}
