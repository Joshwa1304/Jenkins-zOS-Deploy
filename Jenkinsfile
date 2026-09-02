pipeline {
    agent any

    environment {
        DEPLOY_SERVER = 'DevOps-Deploy'
        COMPONENT     = 'zOS-Test-Comp'
        ZOS_VERSION   = 'v1.3'
    }

    stages {
        stage('Trigger zOS Version Import') {
            steps {
                echo "Creating version ${ZOS_VERSION} in ${COMPONENT}"

                step([
                    $class: 'UCDeployPublisher',

                    siteName: "${DEPLOY_SERVER}",

                    component: [
                        $class: 'com.urbancode.jenkins.plugins.ucdeploy.VersionHelper$VersionBlock',

                        componentName: "${COMPONENT}",

                        delivery: [
                            $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeliveryHelper$Pull',

                            pullSourceType: 'zOS File',

                            pullProperties: "zOSFileImportProperties/versionName=${ZOS_VERSION}",

                            pullIncremental: false
                        ]
                    ]
                ])
            }
        }
    }

    post {
        success {
            echo "Version ${ZOS_VERSION} import triggered successfully."
        }

        failure {
            echo "Failed to create version ${ZOS_VERSION}."
        }
    }
}