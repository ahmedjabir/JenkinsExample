pipeline {
    agent any
    stages {       
	stage('Checkout') {
	   steps {
		checkout scm
	   }
	} 	
	stage('Build') {
            steps {
                sh 'echo "Hello World"'
                sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                '''
                echo '====== Build Started ======'
		        sh 'xcodebuild -scheme "JenkinsExample" -configuration "Debug" build test -destination "platform=iOS,name=Ahmed Jabir’s iPhone"'
		        echo '====== Build Ended ======'
            	}
            	post {
                    failure {
                    script { env.FAILURE_STAGE = 'Build' }
                }
         }
        }
	stage('Test') {
	    steps {
	   	echo '===== Test Started ====='
		sh 'make check'
		echo '===== Test Ended ====='
	    }
	}
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
            
            
            mail subject: "\u2639 ${env.JOB_NAME} (${env.BUILD_NUMBER}) has failed",
                    body: """Build ${env.BUILD_URL} is failing in ${env.FAILURE_STAGE} stage!
                          |Somebody should do something about that""",
                      to: "ahmedjabir@gmail.com",
                 replyTo: "ahmedjabir@gmail.com",
                    from: 'ahmed.jabir@maqta.ae'
            
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}