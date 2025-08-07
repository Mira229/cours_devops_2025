pipeline {
    agent any



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




}












/*
        stage('Code Quality Checks') {
            parallel {
                stage('Lint') {
                    steps {
                        sh '''
                            . venv/bin/activate
                            pip install flake8 black isort

                            # Run linting
                            flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics

                            # Check code formatting
                            black --check .

                            # Check import sorting
                            isort --check-only .
                        '''
                    }
                }

                stage('Security Scan') {
                    steps {
                        sh '''
                            . venv/bin/activate
                            pip install bandit safety

                            # Run security checks
                            bandit -r . -x tests/

                            # Check for known vulnerabilities
                            safety check
                        '''
                    }
                }
            }
        }
        */
/*z5WQHoxUsDTh3saibDbaS7Ug0EZbToIk*/

/*TODO */
/* Plugins to block pipeline triggers*/