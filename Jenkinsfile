pipeline {
    agent {
        label 'node7'
    }
    stages {
        stage('Build') {
            steps {
                echo 'Clone repository'
                sh 'sudo bash'
                sh 'gem update --system'
                sh "bundle config set deployment 'true'"
                sh "bundle config path vendor/bundle"
                sh "bundle install --jobs 4 --retry 3"
                sh "bundle exec middleman build"
            }
        }
    }
}
