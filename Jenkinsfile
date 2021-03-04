#!/usr/bin/groovy
//@Library('xm-pipeline-library') _
pipeline {

    agent { 
        node {
            label "${env.NODELABEL}"
        }
    }

	stages {
        stage ('Start') {
            steps {
                echo "Assuming Role ${env.ROLE}"
				sh "aws sts get-caller-identity"
            }
        }    

		
		stage('List_S3_Assumed') {
			steps {
                withAWS(roleAccount:'596834884942', role:"${env.ROLE}", useNode: true) {
					sh "aws sts get-caller-identity"
                    sh 'aws s3 ls'
                }
			}
		}
	}
}