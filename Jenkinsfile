podTemplate(label: 'mypod', serviceAccount: 'default', containers: [ 
    containerTemplate(
      name: 'helm', 
      image: 'ibbuu/sujaytest:1', 
      resourceRequestCpu: '500m',
      resourceLimitCpu: '1Gi',
      resourceRequestMemory: '500Mi',
      resourceLimitMemory: '1Gi',
      ttyEnabled: true, 
      command: 'cat',
      envVars: [
    envVar(key: 'USE_GKE_GCLOUD_AUTH_PLUGIN', value: 'True')
 ]
    )
  ],

//   volumes: [
//     hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
//     hostPathVolume(mountPath: '/usr/local/bin/helm', hostPath: '/usr/local/bin/helm')
//   ]
  ) {
    node('mypod') {
        stage('Get latest version of code') {
          git url: 'https://github.com/Ibbuu/testsuj.git', credentialsId: '2546f0a6-fdd0-4aaa-9c94-9abe963d2f9c'
        }
        // stage('Check running containers') {
        //     container('docker') {  
        //         sh 'hostname'
        //         sh 'hostname -i' 
        //         // sh 'docker ps'
        //         sh 'ls'
                
        //     }
//             container('kubectl'){
//                 withKubeConfig([credentialsId: '40a5a2ed-d243-4719-b907-ecb01df5f4f2']) {
//     sh 'kubectl get pods'
// }
//             }
            container('helm') { 
                
                
                // sh 'apt-get install google-cloud-sdk-gke-gcloud-auth-plugin'
                // sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/v1.20.5/bin/linux/amd64/kubectl"'
                // sh 'chmod u+x ./kubectl'
                // sh 'kubectl get pods' 
                // sh 'helm init'
                // sh 'helm install testrun authapi-cleverstack/'
                // sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/v1.20.5/bin/linux/amd64/kubectl"'
                // sh 'chmod u+x ./kubectl'
                // sh 'yum install google-cloud-sdk-gke-gcloud-auth-plugin'
                sh 'kubectl config view'
                // sh 'curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3'
                // sh 'chmod 700 get_helm.sh'
                // sh './get_helm.sh'
                // sh 'gcloud components update'
                // sh 'gcloud components install gke-gcloud-auth-plugin'
                sh 'gcloud auth login --cred-file gcpCred.json'
                sh 'export USE_GKE_GCLOUD_AUTH_PLUGIN=True'
                sh 'echo $USE_GKE_GCLOUD_AUTH_PLUGIN'
                // sh 'source ~/.bashrc'
                sh 'gke-gcloud-auth-plugin --version'
                sh 'gcloud components install gke-gcloud-auth-plugin'
                sh 'gcloud container clusters get-credentials cleverstack-auth --region asia-south2 --project cleverstack-376306'
                sh 'kubectl get pods'
                sh 'helm upgrade --install --namespace test --create-namespace testrun authapi-cleverstack/'
                
                // sh 'helm  uninstall testrun'
                
            }
        }         
    }
