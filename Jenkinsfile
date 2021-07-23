pipeline {
    agent {
        label 'node7'
    }
    environment {
        GIT_REPOSITORY = 'https://github.com/luandnh/slate.git'
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
