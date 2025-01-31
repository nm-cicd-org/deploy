pipeline {

	options{
		disableConcurrentBuilds()
		overrideIndexTriggers(false)
	}
	
	agent any
	
	parameters{
		string(name: "GITHUB_ORG")
		string(name: "IMAGE_NAME")
		string(name: "VERSION")
	}
	
	environment{
		GITHUB_DEPLOY_REPO_URL = "git@github.com:${GITHUB_ORG}/deploy.git"	
	}
		
	stages{
	
		stage("Deploy to Staging"){
			
			steps{
			
				git url: "${GITHUB_DEPLOY_REPO_URL}", credentialsId: 'nm-ssh-jen', branch: 'main'
				
				sh "git config user.email pipeline@linkedin.com"
				
				sh "git config user.name pipeline"
				
				sh "cd base && kustomize edit set image ${IMAGE_NAME}=${IMAGE_NAME}:${VERSION}"
				
				sh 'git commit -am "Updated image ${IMAGE_NAME} to ${VERSION}"'
				
				sshagent(["nm-ssh-jen"]){
				
					sh "git push origin main"
				
				}
			
			}		
		
		}
		
		stage("Deploy to Production"){
			
			steps{
			
				git url: "${GITHUB_DEPLOY_REPO_URL}", credentialsId: 'nm-ssh-jen', branch: 'release'
				
				sh "git config user.email pipeline@linkedin.com"
				
				sh "git config user.name pipeline"
								
				sh "git merge main"
				
				sshagent(["nm-ssh-jen"]){
				
					sh "git push origin release"
				
				}
			
			}		
		
		}							
	}

}
