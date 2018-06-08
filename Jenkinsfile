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
/*    stage('E2E Test') {         
        wrap([$class: 'Xvfb', autoDisplayName: true, 'timeout': 15]) {
            sh 'npm run protractor'
            sh 'echo "e2e test completed"'
        }
        sh 'echo "e2e test completed"'
    } */
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
}
    
    /*
    stage('Kiuwan analyzer') {
        withCredentials([usernamePassword(credentialsId: 'kiuwan_credential',
                        passwordVariable: 'PASSWORD',
                        usernameVariable: 'USERNAME')]) {
                            def returnCode = bat(script: "${AGENT_HOME}/bin/agent.cmd -s \"${WORKSPACE}\" -n \"appPipeline\" -cr \"CR001\" -l ${BUILD_NUMBER} -wr --user \"$USERNAME\" --pass \"$PASSWORD\"",
                            returnStatus: true)
                             
                         switch(returnCode){
                                case 0:
                                    break
                                case 14:
                                    currentBuild.result = 'UNSTABLE'
                                    break
                                case 1:
                                case 10:
                                    currentBuild.result = 'FAILURE'
                                    break
                                case 11:
                                case 12:
                                    currentBuild.result = 'NOT_BUILT'
                                    break
                                case 13:
                                    currentBuild.result = 'ABORTED'
                                    break
                                default:
                                    currentBuild.result = 'NOT_BUILT'
                                }
                        }
        echo "Kiuwan Analyzer- Completed"        
        archiveArtifacts allowEmptyArchive: false, artifacts: '** /analyzer.html', onlyIfSuccessful: true 
    }
/*
    stage ('Owasp ZAP scanning'){        
        // Security testing stage to run security tests.        
        // target URL is https://one.onp-dev1.nixu.fi        
        echo "Owasp ZAP scanning"        
        sh '''
        sudo docker ps        
        chmod 755 security_automation/owasp-zap-docker.sh        
        ./security_automation/owasp-zap-docker.sh http://localhost:3000        '''        
        echo "OWASP ZAP scanning - Completed"        
        archiveArtifacts allowEmptyArchive: false, artifacts: '** / zap-report.html', onlyIfSuccessful: true    
    } */
    /*
    stage('Push to Docker Hub') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
        sh 'echo "Docker push completed"'
    }*/
   /* stage('Deploy to Test Env'){
        docker.run("lkoshy/juice-shop")
    } */
         
}
