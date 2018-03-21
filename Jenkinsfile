#!/bin/sh

#  Jenkinsfile.sh
#  JenkinsExample
#
#  Created by AhmedJabir on 3/21/18.
#  Copyright © 2018 AhmedJabir. All rights reserved.

#Jenkinsfile (Declarative Pipeline)
pipeline {
    agent { docker { image 'maven:3.3.3' } }
    stages {
        stage('build') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}
