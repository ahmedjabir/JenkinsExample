def Checkout = 'Checkout'
def Build = 'Build'
def Archive = 'Archive'
def Deploy = 'Deploy'


pipeline {
    agent any
    stages {       
	    /*stage(CheckOut) {
	        steps {
		        checkout scm
	        }
	        post {
                failure {
                    script { env.FAILURE_STAGE = CheckOut }
                }
            }
	    }*/
	    stage(Build) {
            steps {
                sh 'echo "Hello World"'
                sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                '''
                echo '====== Build Started ======'
		        //sh 'xcodebuild -scheme "JenkinsExample" -configuration "Debug" build test -destination "platform=iOS Simulator,name=iPhone 6,OS=12.1" -destination "platform=iOS Simulator,name=iPhone 7,OS=12.1"'
		        sh 'xcodebuild clean -project JenkinsExample.xcodeproj -sdk iphoneos -configuration Release build -destination "platform=iOS Simulator,name=iPhone 6,OS=12.1" -destination "platform=iOS Simulator,name=iPhone 7,OS=12.1"'
		        echo '====== Build Ended ======'
            }
        	post {
                failure {
                    script { env.FAILURE_STAGE = Build }
                }
            }
	    }
	    stage(Archive) {
	        steps {
	   	        echo '===== Test Started ====='
		        //sh "xcodebuild -scheme 'JenkinsExample' -enableCodeCoverage YES -configuration Debug -destination 'name=iPhone 6,OS=12.1' build-for-testing | tee build/xcodebuild-test.log | xcpretty"
		        sh 'xcodebuild archive -project JenkinsExample.xcodeproj -scheme JenkinsExample -archivePath /Users/automation/Jenkins_Projects_Archives/JenkinsExample.xcarchive'
		        echo '===== Test Ended ====='
	        }
	        post {
                failure {
                    script { env.FAILURE_STAGE = Archive }
                }
            }
	    }
	    
	    stage(Deploy) {
	        steps {
	   	        echo '===== Deployment Started ====='
		        sh 'xcodebuild -exportArchive -archivePath /Users/automation/Jenkins_Projects_Archives/JenkinsExample.xcarchive -exportPath /Users/automation/Jenkins_Projects_Archives -exportOptionsPlist exportOptions.plist -allowProvisioningUpdates'
		        echo '===== Deployment Compelted ====='
	        }
	        post {
                failure {
                    script { env.FAILURE_STAGE = Deploy }
                }
            }
	    }
     
     
     stage('Publish') {
       environment {
         APPCENTER_API_TOKEN = credentials('30d8938de76409402011c7a9b5dd47bd68e113b0')
       }
       steps {
         appCenter apiToken: APPCENTER_API_TOKEN,
                 ownerName: 'maqta.gateway.mobile',
                 appName: 'Jenkins',
                 pathToApp: 'three/days/Jenkins.ipa',
                 distributionGroups: 'jenkins_distribution',
                 notifyTesters: true
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
