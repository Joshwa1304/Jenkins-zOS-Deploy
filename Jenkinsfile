pipeline {
    agent any

    environment {
        DEPLOY_SERVER      = 'DevOps-Deploy'
        APPLICATION        = 'zOS-Test-App'
        ENVIRONMENT_NAME   = 'zOS-Test-Env'
        APPLICATION_PROCESS = 'zOS-App-Processes'
        COMPONENT          = 'zOS-Test-Comp'
        ZOS_VERSION        = 'v1.4'
    }

    stages {
        stage('Display Deployment Details') {
            steps {
                echo 'Starting zOS deployment'
                echo "Application : ${APPLICATION}"
                echo "Environment : ${ENVIRONMENT_NAME}"
                echo "Process     : ${APPLICATION_PROCESS}"
                echo "Component   : ${COMPONENT}"
                echo "Version     : ${ZOS_VERSION}"
            }
        }

        stage('Deploy to zOS') {
            steps {
                step([
                    $class: 'UCDeployPublisher',

                    siteName: "${DEPLOY_SERVER}",

                    deploy: [
                        $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeployHelper$DeployBlock',

                        createSnapshot: [
                            deployWithSnapshot: false,
                            snapshotName: ''
                        ],

                        deployApp: "${APPLICATION}",

                        deployEnv: "${ENVIRONMENT_NAME}",

                        deployProc: "${APPLICATION_PROCESS}",

                        deployVersions: "${COMPONENT}:${ZOS_VERSION}",

                        deployOnlyChanged: false
                    ]
                ])
            }
        }
    }

    post {
        success {
            echo 'Jenkins successfully submitted the zOS deployment request.'
            echo "Requested deployment: ${COMPONENT}:${ZOS_VERSION}"
            echo "Target environment: ${ENVIRONMENT_NAME}"
            echo 'Check HCL Deploy Application History for the actual deployment result.'
        }

        failure {
            echo 'Failed to submit the zOS deployment request.'
            echo 'Check Jenkins Console Output.'
        }
    }
}