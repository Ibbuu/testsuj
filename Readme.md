Ingress Controller Installation
1. First create a static ip address from google cloud platform
2. Run the following command to install ingress controller using helm
    1. helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
    2. helm repo update
    3. helm install nginx-ingress ingress-nginx/ingress-nginx --set controller.service.loadBalancerIP=<static ip address>

Cert Manager Installation
1. helm repo add jetstack https://charts.jetstack.io
2. helm repo update
3. helm install   cert-manager jetstack/cert-manager   --namespace cert-manager   --create-namespace   --version v1.11.0   --set installCRDs=true --set global.leaderElection.namespace=cert-manager

ClusterIssuer Installation
1. Create a file clusterissuer.yaml with following contents.
    apiVersion: cert-manager.io/v1
    kind: ClusterIssuer
    metadata:
      name: letsencrypt-prod
    spec:
      acme:
        server: https://acme-v02.api.letsencrypt.org/directory
        email: sujay@technodreams.in
        privateKeySecretRef:
          name: letsencrypt-prod
        solvers:
        - http01:
            ingress:
              class: nginx
2. kubectl apply -f clusterissuer.yaml
3. Run the cluster issuer installation only once after cert manager is installed

Certificate generation
1. Before deploying the application helm chart, make sure you generate the certificate for the domain.
2. First create an record in dns mapping the domain to static ip of the ingress controller.
3. Create a file called certificate.yaml with following content to generate a certificate.
    1. apiVersion: cert-manager.io/v1
       kind: Certificate
       metadata:
         name: authapidev-com-cert<Change the name accordingly>
       spec:
         secretName: authapidev-com-tls<Change the name accordingly>
         commonName: authapidev.cleverstack.in<Change the domain name accordingly>
         dnsNames:
         - authapidev.cleverstack.in<Change the domain name accordingly>
         issuerRef:
           name: letsencrypt-prod
           kind: ClusterIssuer
         acme:
           config:
           - http01:
               ingressClass: nginx
4. kubectl apply -f certificate.yaml

Application helm chart installation
1. Checkout the code from this repo.
2. Run the following command from the root of the checked out repository. In this case, run from where the Readme.md file is present
3. helm install <release name that you want to give> authapi-cleverstack/
4. In case of multiple values.yaml files, we can specify the file which we want to supply during installation by followinng command.
    1. helm install <release name that you want to give> authapi-cleverstack/ -f <your values.yaml file>
5. In case of upgrade, run the following command
    1. helm upgrade --install <release name that you want to give> authapi-cleverstack/