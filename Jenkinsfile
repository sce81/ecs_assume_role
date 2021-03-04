#!/usr/bin/groovy
//@Library('xm-pipeline-library') _
pipeline {
	agent none

	stages {
        stage ('Start') {
            steps {
                echo "Assuming Role ${env.ROLE}"
				sh "aws sts get-caller-identity"
            }
        }    
		stage('List_S3') {
			agent {
				label "${env.NODELABEL}"
			}
			steps {
				sh "aws sts get-caller-identity"
				sh "aws s3 ls"
			}
		}
		stage('List_S3_Assumed') {
			agent {
				label "${env.NODELABEL}"
			}
			steps {
                withAWS(roleAccount:'596834884942', role:"${env.ROLE}") {
					sh "aws sts get-caller-identity"
                    sh 'aws s3 ls'
                }
			}
		}
	}
}