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
		sh 'make'
            	}
        }
	stage('Test') {
	    steps {
	   	echo '=====Test Started====='
		sh 'make check'
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