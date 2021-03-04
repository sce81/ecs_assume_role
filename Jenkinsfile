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
		stage('Get Session Credentials') {
				CREDS   = sh(returnStdout: true, script:"aws sts assume-role --role-arn arn:aws:iam::${env.TARGETACCOUNT}:role/${env.ROLE} --role-session-name DeployKubenetes${env.BUILD_NUMBER}-2 --output text | awk '/^CREDENTIALS/ { print \$2, \$4, \$5 }'")
				ACCESS  = sh(returnStdout: true, script:"set +x && echo \$'${CREDS}' | awk '{ print \$1 }' -")
            	SECRET  = sh(returnStdout: true, script:"set +x && echo \$'${CREDS}' | awk '{ print \$2 }' -")
            	SESSION  = sh(returnStdout: true, script:"set +x && echo \$'${CREDS}' | awk '{ print \$3 }' -")
			
		}

		stage('Run AWS Commands') {
			steps {
				sh "aws s3 ls"
					sh """
            		set +x 
            		export AWS_ACCESS_KEY_ID=${ACCESS} 
            		export AWS_SECRET_ACCESS_KEY=${SECRET} 
            		export AWS_SESSION_TOKEN=${SESSION} 
            		set -x
            		aws s3 ls
            	"""
			}
		}
	
		stage('Assume Role') {
			steps {
                   sh """
					#aws sts assume-role --role-arn arn:aws:iam::${env.TARGETACCOUNT}:role/${env.ROLE} --role-session-name AssumeRole-${env.BUILD_NUMBER}
					aws sts get-caller-identity
					aws s3 ls
					"""
				
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