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

  ) {
    node('mypod') {
        stage('Get latest version of code') {
          git url: 'https://github.com/Ibbuu/testsuj.git', credentialsId: '2546f0a6-fdd0-4aaa-9c94-9abe963d2f9c'
        }
        
        container('helm') { 
                
                withCredentials([file(credentialsId: 'gcp', variable: 'GC_KEY')]) {
                sh 'gcloud auth login --cred-file ${GC_KEY}'
                sh 'export USE_GKE_GCLOUD_AUTH_PLUGIN=True'
                sh 'gcloud components install gke-gcloud-auth-plugin'
                sh 'gcloud container clusters get-credentials ${CLUSTER_NAME} --region ${REGION} --project ${PROJECT_ID}'
                sh 'helm upgrade --install --namespace ${NAMESPACE} --create-namespace ${RELEASENAME} authapi-cleverstack/'
        }
                
            }
        }         
    }
