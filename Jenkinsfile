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
         sh 'rm -rf node_modules'
    }
    stage('E2E Test') {         
        wrap([$class: 'Xvfb', autoDisplayName: true, 'timeout': 15]) {
            sh 'npm run protractor'
            sh 'echo "e2e test completed"'
        }
        sh 'echo "e2e test completed"'
    }
    stage('Application security testing') {
        environment {
            analyzer.bundle.audit.enabled=false
        }   
        sh 'echo "Trying to run depedency check"'
        sh 'echo $analyzer.bundle.audit.enabled'
        dependencyCheckAnalyzer datadir: 'dependency-check-data', isFailOnErrorDisabled: true, hintsFile: '', includeCsvReports: false, includeHtmlReports: false, includeJsonReports: false, isAutoupdateDisabled: false, outdir: '', scanpath: 'package.json', skipOnScmChange: false, skipOnUpstreamChange: false, suppressionFile: '', zipExtensions: ''
        dependencyCheckPublisher canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''        
        archiveArtifacts allowEmptyArchive: true, artifacts: '**/dependency-check-report.xml', onlyIfSuccessful: true
        
/*	sh '/home/ubuntu/dependency-check/bin/dependency-check.sh --project "Juice Shop" --scan . --format HTML --disableRubygems --disableBundleAudit --disablePyPkg --disablePyDist' */
        step([$class: 'DependencyCheckPublisher', unstableTotalAll: '0'])

    }
    stage('Docker Build') {
        app = docker.build("lkoshy/juice-shop")        
        sh 'echo "Docker Image completed"'
    }
    stage('Push to Docker Hub') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
        sh 'echo "Docker push completed"'
    }
   /* stage('Deploy to Test Env'){
        docker.run("lkoshy/juice-shop")
    } */
         
}
