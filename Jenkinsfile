node {
	gitrepo = checkout scm
	gitBranch = gitrepo.GIT_BRANCH

	stag('checkout'){
		checkout scm
	}

	if (gitBranch == 'origin/staging'){
		stage('updating staging server'){
			withCredential([
				sshUserPrivateKey(credentialsId: 'staging-ssh-cred', keyFileVariable: 'SSHPASS'),
				file(credentialsId: 'git-cred', variable: 'GITCRED')]){
				ansiblePlaybook colorized: true, credentialsId: 'staging-ssh-cred', disableHostKeyChecking: true, extras: '-e branch="staging" -e keyfile="${GITCRED}"', inventory: './ansible-code/inventory', limit: 'stag', playbook: './ansible-code/bitclone.yml'
			}
		}
	}

	if (gitBranch == 'origin/master'){
		stage('updating production server'){
			withCredential([
				sshUserPrivateKey(credentialsId: 'staging-ssh-cred', keyFileVariable: 'SSHPASS'),
				file(credentialsId: 'git-cred', variable: 'GITCRED')]){
				ansiblePlaybook colorized: true, credentialsId: 'staging-ssh-cred', disableHostKeyChecking: true, extras: '-e branch="master" -e keyfile="${GITCRED}"', inventory: './ansible-code/inventory', limit: 'prod', playbook: './ansible-code/bitclone.yml'
			}
		}
	}
}
	catch (e) {
	    // If there was an exception thrown, the build failed
	    currentBuild.result = "FAILED"
	    throw e
	  }
	    finally {
	          echo "Current Jenkins pipeline name: $env.JOB_NAME"
	          echo "Current branch: $gitrepo.GIT_BRANCH"
			   }

}
