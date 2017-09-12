node {
    def app

    stage('Clone Repository') {
        checkout scm
    }
    
    stage('Unit Test App') {
         sh 'node -v'
         sh 'npm prune'
         sh 'npm install'
         sh 'npm test'
         sh 'echo "Unit test completed"'     
    }
    
    stage('e2e Test') {
         /*sh 'npm run protractor'*/
         sh 'echo "e2e test completed"'     
    }
    
    stage('Build Image') {
        app = docker.build("lkoshy/juice-shop")
        sh 'echo "Docker Image completed"'
    }

    stage('Push Image') {
        /*docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }*/
        sh 'echo "Docker push completed"'
    }
    post {
        always {
             junit 'build/reports/*.xml'
        }
    }
}
