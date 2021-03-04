#!/usr/bin/groovy
//@Library('xm-pipeline-library') _
pipeline {
	agent none

	stages {
		stage('List_S3') {
			agent {
				label "${env.agentLabel}"
			}
			steps {
				sh "aws s3 ls"
			}
		}
		stage('List_S3_Assumed') {
			agent {
				label "${env.agentLabel}"
			}
			steps {
                withAWS(roleAccount:'596834884942', role:"${env.ROLE}") {
                    sh 'aws s3 ls'
                }
			}
		}
	}
}