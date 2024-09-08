pipeline {
    agent any

    tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }

    environment {
        SNAP_REPO = 'vprofile-snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin123'
        CENTRAL_REPO = 'vpro-maven-central'
        NEXUS_GRP_REPO = 'vpro-maven-group'
        NEXUSIP = '172.31.6.135'
        NEXUSPORT = '8081'
        RELEASE_REPO = 'vprofile-release'
        NEXUS_LOGIN = 'nexuslogin'
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'
        NEXUS_VERSION = 'nexus3'
        NEXUS_CREDENTIAL_ID = "${NEXUS_LOGIN}"

    
    }

    stages{
        stage ('BUILD') {
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post {
                success {
                    echo "Archiving....!!"
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage ('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage ('Checkstyle Analysis') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage ('SonarQube Analysis') {
            environment {
                scannerHome = tool "${SONARSCANNER}"
            }
            steps {
                withSonarQubeEnv("${SONARSERVER}") {
                    sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                    -Dsonar.projectName=vprofile-repo \
                    -Dsonar.projectVersion=1.0 \
                    -Dsonar.sources=src/ \
                    -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                    -Dsonar.junit.reportsPath=target/surefire-reports/ \
                    -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                    -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                }
            }
        }
        stage ('Quality Gates') {
            steps {
                timeout (time: 1, unit: 'MINUTES' ) {
                    waitForQualityGate abortPipeline: true 
                }
            }
        }
        stage ("UploadArtifact") {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${NEXUSIP}:${NEXUSPORT}",
                    groupId: 'QA',
                    version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                    repository: "${RELEASE_REPO}",
                    credentialsId: "${NEXUS_LOGIN}",
                        artifacts: [
                            [
                                 artifactId: 'vprofile-app', // Define your artifactId here
                                 classifier: '',
                                 file: 'target/vprofile-v2.war', // Ensure this path is correct and exists after the build
                                 type: 'war'
                            ]
                        ]
                )
            }
        }
    }
}