pipeline {

    parameters {
      	string(defaultValue: '0', description: 'This is a parameter', name: 'IMAGE_TAG')
    }

    agent any

    stages {
/*
//
//no need the clone as it had been define and set when the job being created using the jenkins UI
//If set something from Jenkins UI, you will not see it here so if you define and creating the clone stage again it will be duplicated
//One way to notice this is when running the job, watch the console and will notice it.
//So remember that if set in the UI to checkout from git already, then Jenkinsfile no need to add again.
//
*/
	stage('Cloning Repo') {
	    steps {
		script {
			git branch: 'main', url: 'https://github.com/chloongloong/hello-api-manifest.git'
		}
	    }
	}

	stage('CD - Update Manifest with new version number for the newly built image') {
 
		steps {
			script {
				sh """
				    echo ${params.IMAGE_TAG}
				    pwd
				    ls -lrt
				    cat deployment-hello-hapi.yaml
				    sed -i 's/hello-hapi:.*/hello-hapi:'"${params.IMAGE_TAG}"'/g' deployment-hello-hapi.yaml
				    grep hello-hapi:.* deployment-hello-hapi.yaml
				    git config --global user.name “chloongloong”
				    git config --global user.email "loongch@yahoo.com.sg"
				    git add deployment-hello-hapi.yaml
				    git commit -m "Updated deployment Manifest to image ver ${params.IMAGE_TAG}"
				   """
			       withCredentials([gitUsernamePassword(credentialsId: 'GITHUB-PAT-USERPASS', gitToolName: 'Default')]) {
				   //sh "git push https://github.com/chloongloong/hello-api-manifest main"
				   sh "git push origin main"
			       }
			}
		}
	}
    }
}
