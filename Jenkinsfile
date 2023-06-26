def COLOR_MAP = [
    'SUCCESS': 'good', 
    'FAILURE': 'danger',
]
pipeline {
    agent any

    environment {
		NEXUS_USER = 'admin'
		NEXUSIP = '172.31.19.238'
		NEXUSPORT = '8081'
        NEXUSPASS = credentials('nexuspass') //stored password only in nexuspass
    }

    stages {
        stage('Setup parameters'){
            steps {
                script {
                    properties ([
                        parameters([
                            string(
                                defaultValue: '',
                                name: 'BUILD'
                            ),
                            string(
                                defaultValue: '',
                                name: 'TIME'
                            )
                        ])
                    ])
                }
            }
        }
        stage('Ansible Deploy to prod'){
            steps {
                ansiblePlaybook([
                inventory   : 'ansible/prod.inventory',
                playbook    : 'ansible/site.yml',
                installation: 'ansible',
                colorized   : true,
			    credentialsId: 'applogin-prod', //credentials we created in jenkins
			    disableHostKeyChecking: true, //otherwise execution will fail, 
                extraVars   : [ //need to specify the variables and values for ansible playbook content.
                   	USER: "${NEXUS_USER}",
                    PASS: "${NEXUSPASS}",
			        nexusip: "${NEXUSIP}",
			        reponame: "vprofile-release",
			        groupid: "QA",
			        time: "${env.TIME}", //BUILD_TIMESTAMP is the variable we have from jenkins plugins
			        build: "${env.BUILD}",
                    artifactid: "vproapp",
			        vprofile_version: "vproapp-${env.BUILD}-${env.TIME}.war"
                ]
             ])
            }
        }

    }
    post {
        always {
            echo 'Slack Notifications.'
            slackSend channel: '#jenkinscicd',
                color: COLOR_MAP[currentBuild.currentResult],
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
        }
    }
}