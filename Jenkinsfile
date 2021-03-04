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
				sh "aws s3 ls"
            }
        }    


		stage('List_S3_Assumed') {
			steps {
                withAWS(roleAccount:"${env.TARGETACCOUNT}", role:"${env.ROLE}", useNode: true) {
                    sh 'aws s3 ls'
                }
			}
		}
		stage('List_EC2_Assumed') {
			steps {
                withAWS(roleAccount:"${env.TARGETACCOUNT}", role:"${env.ROLE}", useNode: true) {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    sh "aws ec2 describe-instances --region ${env.REGION} --output=table"
                    }
                }
			}
		}
	}
}