pipeline {
    agent any
    
    stages {
        stage('Checkout helm chart') {
            steps {
                checkout scm
            }
        }
        stage('checkout private repo') {
            steps {
                    dir('private_repo') {
                    git branch: private_repo_branch, credentialsId: private_repo_credentials, url: private_repo_url
                }
             }
            }
        stage('copying keys to the helm chart') {
	  steps {
		script{
        jobName = sh(returnStdout: true, script: "echo $JOB_NAME").split('/')[-1].trim().toLowerCase()
        currentWs = sh(returnStdout: true, script: 'pwd').trim()
	keys_path = "${currentWs}/private_repo/ansible/inventory/$env/MyJP/keys"
        dest_path = "${currentWs}/kubernetes/helm_charts/MyJP/$jobName/"
	sh "cp -rf $keys_path $dest_path"		
	}
}
	
}
        stage('Deploy') {
            steps {
                script {
                        jobName = sh(returnStdout: true, script: "echo $JOB_NAME").split('/')[-1].trim().toLowerCase()
                        currentWs = sh(returnStdout: true, script: 'pwd').trim()
			env = sh(returnStdout: true, script: "echo $JOB_NAME").split('/')[-3].trim()
                        chart_path = "${currentWs}/kubernetes/helm_charts/MyJP/$jobName"
			values_file = "${currentWs}/private_repo/ansible/inventory/$env/MyJP/private_values.yaml"
                        sh "helm upgrade --install $jobName $chart_path --namespace  $djp_namespace --kubeconfig  $kubeconfig_path -f $values_file"
                }
            }
        }
    }

}
