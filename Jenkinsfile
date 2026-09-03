pipeline {
    agent any

    environment {
        DEPLOY_SERVER = 'DevOps-Deploy'
        COMPONENT     = 'zOS-Test-Comp'
        ZOS_VERSION   = 'v1.4'
        SHIPLIST_FILE = '/zbuzagent/shiplist/batchshiplist.xml'
    }

    stages {
        stage('Trigger zOS Version Import') {
            steps {
                echo 'Starting zOS component version import'
                echo "Deploy Server : ${DEPLOY_SERVER}"
                echo "Component     : ${COMPONENT}"
                echo "Version       : ${ZOS_VERSION}"
                echo "Shiplist      : ${SHIPLIST_FILE}"

                step([
                    $class: 'UCDeployPublisher',

                    siteName: "${DEPLOY_SERVER}",

                    component: [
                        $class: 'com.urbancode.jenkins.plugins.ucdeploy.VersionHelper$VersionBlock',

                        componentName: "${COMPONENT}",

                        delivery: [
                            $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeliveryHelper$Pull',

                            pullSourceType: 'zOS File',

                            pullProperties: """version=${ZOS_VERSION}
shiplistContent=
shiplitFilePath=${SHIPLIST_FILE}
packageAfterTimestamp=""",

                            pullIncremental: false
                        ]
                    ]
                ])
            }
        }
    }

    post {
        success {
            echo 'Jenkins successfully sent the import request to HCL Deploy.'
            echo "Requested component version: ${ZOS_VERSION}"
            echo 'Check the HCL Deploy Version Import History for the actual import result.'
        }

        failure {
            echo "Failed to trigger zOS version ${ZOS_VERSION} import."
            echo 'Check the Jenkins Console Output.'
        }
    }
}
