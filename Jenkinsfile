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
		stage('List_S3') {
			steps {
				sh "aws sts get-caller-identity"
				sh "aws s3 ls"
			}
		}
		stage('Assume Role') {
			steps {
				script {
                    "aws sts assume-role --role-arn arn:aws:iam::${env.TARGETACCOUNT}:role/${env.ROLE} --role-session-name AssumeRole-${env.BUILD_NUMBER} "
					"aws sts get-caller-identity"
				}
                
			}
		}
		stage('List_S3_Assumed') {
			steps {
					sh "aws sts get-caller-identity" 
					sh "aws s3 ls"               
			}
		}
	}
}