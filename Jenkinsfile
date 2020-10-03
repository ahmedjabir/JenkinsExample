def Checkout = 'Checkout'
def Build = 'Build'
def Archive = 'Archive'
def Deploy = 'Deploy'
def Publish = 'Publish'


pipeline {
    agent any
    stages {       
	    stage(CheckOut) {
	        steps {
		        checkout scm
	        }
	        post {
                failure {
                    script { env.FAILURE_STAGE = CheckOut }
                }
            }
	    }
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
     
     
     stage(Publish) {
         steps {
            echo '===== Publish Started ====='
             appCenter apiToken: 'f51cd29ba6b2d34a84cd99bc37348db77624c614',
                     ownerName: 'maqta.gateway.mobile',
                     appName: 'Jenkins',
                     pathToApp: '~/Users/automation/Jenkins_Projects_Archives/JenkinsExample.ipa',
                     distributionGroups: 'jenkins_distribution'
            echo '===== Publish Ended ====='
         }
         post {
             failure {
                 script { env.FAILURE_STAGE = Publish }
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
