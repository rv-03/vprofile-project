pipeline {
    
	agent any
/*	
	tools {
        maven "maven3"
    }
*/	
    environment {
        SNAP_REPO = 'vprofile-snapshot'
        NEXUS_VERSION = 'nexus3'
        NEXUS_PROTOCOL = 'http'
        NEXUS_URL = '172.31.6.135:8081'
        NEXUSIP = '172.31.6.135'
        CENTRAL_REPO = 'vpro-maven-central'
        NEXUSPORT = '8081'
        RELEASE_REPO = 'vprofile-release'
	    NEXUS_REPO_ID    = 'vprofile-release'
        NEXUS_CREDENTIAL_ID = 'nexuslogin'
        ARTVERSION = '${env.BUILD_ID}'
        NEXUS_GRP_REPO = 'vpro-maven-group'
    }
	
    stages{
        
        stage('BUILD'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }
}