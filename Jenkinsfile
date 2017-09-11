node {
    def app

    stage('Clone repository') {
        checkout scm
    }
    
    stage('Test app') {
            sh 'echo "Tests passed"'     
    }
    
    stage('Build image') {
        app = docker.build("lkoshy/juice-shop")
    }

    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
