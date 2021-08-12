pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5') )
    }
    parameters {
        booleanParam name:'RUN_TESTS', defaultValue:true, description:'Run Tests?'
    }

    stages {
        stage ('Clone repository') {
            steps {
                echo "Clone the GIT repository ..."
                git credentialsId: 'git_https', url: 'https://github.com/markchron/jenkins-cmake.git'
            }
        }
        stage("Configure") {
            steps {
                echo "Configuring the application..."
                dir('build') {
                    bat 'cmake -G "Visual Studio 15 2017 Win64" ..'
                }
            }
        }
        stage("Build") {
            steps {
                echo "Building the application..."
                dir('build') {
                    bat 'cmake --build .  --config Release'
                }
            }
        }
        stage("Test") {
            when {
                environment name: 'RUN_TESTS', value: 'true'
            }
    
            steps {
                echo "Testing the application..."

                dir('build') {
                    bat 'ctest --no-compress-output -T Test'
                }
                
                // Archive the CTest xml output
                archiveArtifacts (
                    artifacts: 'build/Testing/**/*.xml',
                    fingerprint: true
                )

                // Process the CTest xml output with the xUnit plugin
                xunit (
                    testTimeMargin: '3000',
                    thresholdMode: 1,
                    thresholds: [
                        skipped(failureThreshold: '0'),
                        failed(failureThreshold: '0')
                        ],
                    tools: [CTest(
                        pattern: 'build/Testing/**/*.xml',
                        deleteOutputFiles: true,
                        failIfNotNew: false,
                        skipNoTestFiles: true,
                        stopProcessingIfError: true
                        )]
                    )
                
            }
        }
    }

    post {
        always {
            // Clear the source and build dirs before next run
            deleteDir()
        }
    }
}

