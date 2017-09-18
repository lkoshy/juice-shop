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
    stage('E2E Test') {         
        wrap([$class: 'Xvfb', autoDisplayName: true, 'timeout': 15]) {
            sh 'npm run protractor'
            sh 'echo "e2e test completed"'
        }    
    }
    stage('Build Image') {
        app = docker.build("lkoshy/juice-shop")        
        sh 'echo "Docker Image completed"'
    }
    stage('Push Image') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
        sh 'echo "Docker push completed"'
    }
   /* stage('Deploy to Test Env'){
        docker.run("lkoshy/juice-shop")
    }
    stage('Application Security Test'){
         zap.run(localhost:8080/juice-shop)
    } */
         
}
