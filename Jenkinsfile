def bob = './bob/bob -r ruleset.yaml'

pipeline {
    agent {
        node {
            label SLAVE
        }
    }
    options {
        timestamps()
        timeout(time: 1, unit: 'HOURS')
    }
    stages {
        stage('Prep bob') {
            steps {
                sh 'git clean -xdff --exclude=.m2 --exclude=settings.xml'
                sh 'git submodule sync'
                sh 'git submodule update --init --recursive'
            }
        }
        stage('Refresh Token - Update Credential') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'eadphub-psw', usernameVariable: 'JENKINS_USERNAME', passwordVariable: 'JENKINS_PASSWORD'), string(credentialsId: 'munin_token', variable: 'MUNIN_TOKEN')]) {
                    script {
                        sh "${bob} jenkins-munin-credential-update"
                    }
                }
            }
        }
    }
}