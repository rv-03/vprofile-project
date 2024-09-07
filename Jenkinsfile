pipeline {
    
	agent any
/*	
	tools {
        maven "maven3"
    }
*/	
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "13.235.91.193:8081"
        NEXUS_REPOSITORY = "vprofile-release"
	    NEXUS_GRP_REPO    = "vprofile-grp-repo"
        NEXUS_LOGIN = "nexuslogin"
        ARTVERSION = "${env.BUILD_ID}"
        NEXUSPORT = "8081"
        NEXUSIP = "13.235.91.193"
        SNAP_REPO = "vprofile-snapshot"
        CENTRAL_REPO = "vprofile-maven-central"
        NEXUS_USER = "admin"
        NEUXS_PASS = "admin123"
    }
	
    stages{
        
        stage('BUILD'){
            steps {
                sh 'mvn clean install -DskipTests'
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