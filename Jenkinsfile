pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/playwright:v1.39.0-noble'
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
                    # serve 설치
                    npm install serve

                    # build 폴더를 웹서버로 실행
                    node_modules/.bin/serve -s build -l 3000 &

                    # 서버가 뜰 때까지 잠시 대기
                    sleep 5

                    # Playwright 실행
                    npx playwright test
                '''
            }
        }
    }

    post {
        always {
            junit 'jest-results/junit.xml'
        }
    }
}