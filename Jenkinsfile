pipeline {
    agent any
    stages {
        stage('Preparation') {
            steps {
                sh 'echo "TEST PREPARATION"'
            }
        }
        stage('Static Analysis') {
            parallel {
                stage('PHPStan') {
                    steps {
                        sh 'vendor/bin/phpstan analyse --error-format=checkstyle --no-progress -n . > logs/phpstan.checkstyle.xml'
                    }
                }
            }
        }


        stage('Build Simulation') {
            steps {
                sh 'ls -ll -a' // Example PHP build step
            }
        }
    }

    post {
        always {
            recordIssues([
                sourceCodeEncoding: 'UTF-8',
                enabledForFailure: true,
                aggregatingResults: true,
                blameDisabled: true,
                referenceJobName: "testjenkinsphp/main",
                tools: [
                    phpStan(id: 'phpstan', name: 'PHPStan', pattern: 'logs/phpstan.checkstyle.xml', reportEncoding: 'UTF-8')
                ]
            ])
        }
    }
}