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

        stage('test') {
            steps {
                echo 'start test'
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage('Playwright Test') {
            steps {
                

                sh '''
                  
                    

                    # React 서버 실행
                    npm install serve 
                    node_modules/.bin/serve -s build & sleep 10
                    serve -s build 
                   

                    # Playwright 실행
                    npx playwright test
                '''
    }

    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}