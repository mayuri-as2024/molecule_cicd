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
                    python -m pip install ansible docker molecule
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
                    docker --version
                    echo "===== Ansible ====="
                    ansible --version
                    echo "===== Molecule ====="
                    molecule --version
                '''
            }
        }

        stage('Stage 4: Molecule Tests (Temporarily Skipped)') {
            steps {
                echo 'Molecule tests temporarily skipped.'
            }
        }

        stage('Stage 5: Deploy to Test Environment') {
            steps {
                echo 'Mock deployment only; no actual deployment performed.'
            }
        }
    }
}