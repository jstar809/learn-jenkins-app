pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/playwright:v1.61.0-noble'
            reuseNode true
        }
    }

    stages {

        stage('Build') {
            steps {
                sh '''
                    ls -la
                    node -v
                    npm -v

                    npm ci
                    npm run build
                '''
            }
        }

        stage('Jest Test') {
            steps {
                echo 'Start Jest Test'

                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage('Playwright Test') {
            steps {
                echo 'Start Playwright Test'

                sh '''
                    npm install serve

                    # React 서버 실행
                    node_modules/.bin/serve -s build -l 3000 &

                    # 서버가 뜰 때까지 대기
                    sleep 5

                    # Playwright 실행
                    npx playwright test
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'playwright-report/**', allowEmptyArchive: true
            archiveArtifacts artifacts: 'test-results/**', allowEmptyArchive: true
        }
    }
}