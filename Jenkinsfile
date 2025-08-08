pipeline {
    agent any

    environment{
        PROJECT = "test_mira"
        REPOSITORY = "fastapi_postgres"
        IMAGE = "$PROJECT/$REPOSITORY"
        REGISTRY_HOST = "https://harbor.devgauss.com/"
    }


    stages {
        stage('Set up Python') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    python3 -m pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }
        stage('run_test'){
            steps{
                sh '''
                    # Run tests with coverage reporting
                    . venv/bin/activate
                    pytest \
                        --cov=. \
                        --cov-report=xml:coverage.xml \
                        --cov-report=html:htmlcov \
                        --cov-report=term \
                        --cov-fail-under=80 \
                        --junitxml=test-results.xml

                '''
            }
            post {
                always {
                    // Archive test artifacts and coverage reports
                    archiveArtifacts artifacts: 'coverage.xml,htmlcov/**/*,test-results.xml', allowEmptyArchive: true

                    // Publish JUnit test results
                    junit 'test-results.xml'

                    // Publish HTML coverage report
                    publishHTML(target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'htmlcov',
                        reportFiles: 'index.html',
                        reportName: 'Coverage Report'
                    ])
                }
            }
        }
        stage('sonarqube analysis'){
            steps{
                withSonarQubeEnv("MySonarqube_mira"){
                    script{
                        def scannerHome = tool "SonarScanner"
                        sh  """
                            ${scannerHome}/bin/sonar-scanner \
                        """
                    }
                }
            }
        }

        stage('build_push_docker_image'){
            steps{
                script{
                    def image = docker.build("$IMAGE:${env.BUILD_ID}")
                    docker.withRegistry("$REGISTRY_HOST", 'mira_credential_harbor'){
                        image.push()

                        image.push("latest")
                    }
                }
            }
        }
    }



}


