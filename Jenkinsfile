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
		        //sh 'xcodebuild -scheme "Testing" -configuration "Debug" build test -destination "platform=iOS Simulator,name=iPhone 6,OS=11.2" -destination "platform=iOS Simulator,name=iPhone 7,OS=11.2"'
		        sh 'xcodebuild clean -project JenkinsExample.xcodeproj -sdk iphoneos -configuration Release build'
		        echo '====== Build Ended ======'
		        
/*
		        echo "\u2639 ${env.JOB_NAME} (${env.BUILD_NUMBER}) has failed"
		        echo """Build ${env.BUILD_URL} is failing in ${env.FAILURE_STAGE} stage! |Somebody should do something about that"""
		        
		        
		        mail subject: "Test Subject",//"\u2639 ${env.JOB_NAME} (${env.BUILD_NUMBER}) has failed",
                    body: "Test Body.....",//"""Build ${env.BUILD_URL} is failing in ${env.FAILURE_STAGE} stage! |Somebody should do something about that""",
                      to: "ahmedjabir@gmail.com",
                 replyTo: "ahmedjabir@gmail.com",
                    from: 'ahmed.jabir@maqta.ae'
*/
		        
		        
		        
		        
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
		        //sh "xcodebuild -scheme 'Testing' -enableCodeCoverage YES -configuration Debug -destination 'name=iPhone 6,OS=11.2' build-for-testing | tee build/xcodebuild-test.log | xcpretty"
		        sh 'xcodebuild archive -project JenkinsExample.xcodeproj -scheme Testing -archivePath /Users/ahmedjabir/Desktop/JenkinsBuild/JenkinsExample.xcarchive'
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
		        sh 'xcodebuild -exportArchive -archivePath /Users/ahmedjabir/Desktop/JenkinsBuild/JenkinsExample.xcarchive -exportPath /Users/ahmedjabir/Desktop/JenkinsBuild -exportOptionsPlist exportOptions.plist -allowProvisioningUpdates'
		        echo '===== Deployment Compelted ====='
	        }
	        post {
                failure {
                    script { env.FAILURE_STAGE = Deploy }
                }
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