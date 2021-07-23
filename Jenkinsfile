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
                sh 'sudo gem update --system'
                sh "sudo bundle config set deployment 'true'"
                sh "sudo bundle config path vendor/bundle"
                sh "sudo bundle install --jobs 4 --retry 3"
                sh "sudo bundle exec middleman build"
            }
        }
    }
}
