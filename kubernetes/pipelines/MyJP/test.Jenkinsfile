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
        stage('Deploy') {
            steps {
                script {
                        jobName = sh(returnStdout: true, script: "echo $JOB_NAME").split('/')[-1].trim().toLowerCase()
                        currentWs = sh(returnStdout: true, script: 'pwd').trim()
                        env = sh(returnStdout: true, script: "echo $JOB_NAME").split('/')[-3].trim()
                        chart_path = "${currentWs}/kubernetes/helm_charts/MyJP/$jobName"
                        values_file = "${currentWs}/private_repo/ansible/inventory/$env/MyJP/private_values.yaml"
                        k8s= sh(returnStdout: true, script: "echo $kubeconfig_path").split('/')[-1].trim()
                        // sh 'test=$(cut -d'/' -f2 <<<"$kubeconfig_path")'
                        sh 'echo $test'
                        //sh "helm upgrade --install $jobName $chart_path --namespace  $djp_namespace --kubeconfig  $kubeconfig_path -f $values_file"
                }
            }
        }
    }

}
