pipeline {
    agent any

    stages {
        stage('Stage 1: Get Latest Code') {
            steps {
                step([$class: 'WsCleanup'])
                checkout scm
            }
        }

        stage('Stage 2: Setup Python Virtual Environment') {
            steps {
                sh '''
                    set -eu

                    python3 --version
                    python3 -m venv virtenv
                    . virtenv/bin/activate

                    python -m pip install --upgrade pip
                    python -m pip install ansible docker "molecule"

                    python -m pip install "molecule-plugins[docker]" || true
                '''
            }
        }

        stage('Stage 3: Display Versions') {
            steps {
                sh '''
                    set -eu
                    . virtenv/bin/activate

                    echo "===== Python ====="
                    python --version

                    echo "===== pip ====="
                    python -m pip --version

                    echo "===== Docker CLI ====="
                    if command -v docker >/dev/null 2>&1; then
                        docker --version
                    else
                        echo "Docker CLI is not installed"
                        exit 1
                    fi

                    echo "===== Ansible ====="
                    ansible --version

                    echo "===== Molecule ====="
                    molecule --version
                '''
            }
        }

        stage('Stage 4: Molecule Tests') {
            stages {
                stage('Stage 4.1: Test Common Role') {
                    steps {
                        sh '''
                            set -eu
                            . virtenv/bin/activate

                            cd roles/common
                            molecule test
                        '''
                    }
                }
            }
        }

        stage('Stage 5: Deploy to Test Environment') {
            when {
                branch 'next'
            }
            steps {
                sh '''
                    echo "MOCK DEPLOYMENT (FOR NOW)"
                '''
            }
        }
    }
}