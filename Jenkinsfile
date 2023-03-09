pipeline {
    agent any
    stages {
        
        stage('Linting') {
            steps {
                sh 'composer install --no-interaction --no-progress --prefer-dist'
                sh 'php vendor/bin/phpcs --standard=PSR2 apitest'
                sh 'php vendor/bin/phpmd apitest text phpunit.xml'
            }
        }
        stage('Test, Build and Deploy'){
            parallel{
                stage ('Test'){         
                    steps {
                        sh 'docker compose run --rm api php artisan test'
                    }
                }
                stage('Build') {
                    steps {
                        sh 'docker build -t build-api .'
                }
                }
                stage('Deploy') {
                    steps {
                        sh 'docker compose up -d'
                    }
                }
            }
        }
    }
}