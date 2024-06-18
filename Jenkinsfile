pipeline {
    agent any
    stages {
        stage('zip the file') {
            steps {
                sh 'rm -rf *.zip || echo " "'
                sh 'zip -r ansible-${BUILD_ID}.zip * --exclude Jenkinsfile'
            }
        }
        stage('upload my artifact to jfrog') {
            steps {
                sh '''
                    curl -u Admin:AP8gcgmmset5jeYChTJYDN6XmDd -T \
                    ansible-${BUILD_ID}.zip \
                    "http://ec2-54-83-138-116.compute-1.amazonaws.com:8081/artifactory/ansible/ansible-${BUILD_ID}.zip"
                '''
            }
        }
        stage('publish to ansible server') {
            steps {
                sshPublisher(publishers: [
                    sshPublisherDesc(
                        configName: 'Ansibleserver', 
                        transfers: [
                            sshTransfer(
                                cleanRemote: false, 
                                excludes: '', 
                                execCommand: 'unzip ansible-${BUILD_ID}.zip; rm -rf ansible-${BUILD_ID}.zip', 
                                execTimeout: 120000, 
                                flatten: false, 
                                makeEmptyDirs: false, 
                                noDefaultExcludes: false, 
                                patternSeparator: '[, ]+', 
                                remoteDirectory: '.', 
                                remoteDirectorySDF: false, 
                                removePrefix: '', 
                                sourceFiles: 'ansible-${BUILD_ID}.zip'
                            )
                        ], 
                        usePromotionTimestamp: false, 
                        useWorkspaceInPromotion: false, 
                        verbose: false
                    )
                ])
            }
        }
    }
}
