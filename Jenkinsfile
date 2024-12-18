// Initial Approval
input message: 'Approve the pipeline execution?', ok: 'Start Pipeline', parameters: [
    string(name: 'Approver', description: 'Enter your name')
]

node {
    stage('Checkout') {
        git branch: 'jhsqa2', 
            credentialsId: 'localgit', 
            url: 'https://assetgit.com/jpl/jhs/code/webapplications/jhs-web-app.git'
    }

    stage('Install Dependencies') {
        nodejs(nodeJSInstallationName: 'NodeJS14.20.1') {
            sh "npm install"
        }
    }

    stage('Approval Email') {
        mail to: 'mailto:raghavendra.kondapalli@assettl.com',
             subject: 'Approval Required for Build Deployment',
             body: '''
             Hi Team,

             The build for the project 'JHS Web App' is ready for deployment.
             
             *Details:*
             - Branch: jhsqa2
             - Environment: Production
             - Action Required: Please approve the build deployment.

             Click the link below to approve:
             http://192.168.0.150:8081/job/email/
             
             Regards,
             Jenkins
             '''
        
        // Wait for manual approval
        input message: 'Approve the build deployment?', ok: 'Proceed'
    }

    stage('Build') {
        nodejs(nodeJSInstallationName: 'NodeJS16.14.0') {
            sh "ng config -g cli.warnings.versionMismatch false"
            sh "node --max_old_space_size=8096 ./node_modules/@angular/cli/bin/ng build --configuration production --build-optimizer=true"
        }
    }
}
