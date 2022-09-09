**Documentation for EKS**

Creating an Amazon EKS cluster

1. Go to the aws console and choose the region and search for eks on navigation bar

![Aspose Words 3536303c-37c2-4423-ade8-b6504f62d085 001](https://user-images.githubusercontent.com/95607370/168243700-0d53021a-335f-436e-bd1e-0b59aae0f01c.png)

2. Click on Elastic Kubernetes Service it will navigate to the page and then click on clusters and then click on create cluster

![Aspose Words 3536303c-37c2-4423-ade8-b6504f62d085 002](https://user-images.githubusercontent.com/95607370/168243802-059f6db2-4278-44d5-af80-d0f545f2c086.png)

3. After click on create cluster we need to select cluster template (EC2 Linux)

![Aspose Words 3536303c-37c2-4423-ade8-b6504f62d085 003](https://user-images.githubusercontent.com/95607370/168243891-e7af9362-6d32-4333-9c6d-c7a8bc38b2bc.png)

4. After selecting the cluster template (EC2 Linux) click on Next step

5. Now we need to configure the cluster by define cluster name and instance type and number of instances and VPC and instance IAM role etc . 

![Aspose Words 3536303c-37c2-4423-ade8-b6504f62d085 004](https://user-images.githubusercontent.com/95607370/168244007-65891f55-6542-46fd-8f17-4872a16fa00d.png)

![Aspose Words 3536303c-37c2-4423-ade8-b6504f62d085 005](https://user-images.githubusercontent.com/95607370/168244065-15b2f9e6-ca29-464f-81a0-3eb656e15c1b.png)

6. After completion of cluster configuration click on create it will create the eks cluster

**Deploy application to EKS**

**Prerequisites**

1. Existing kubernetes cluster
1. Install kubectl on computer  

```bash
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/ 1.21.2/2021-07-05/bin/linux/amd64/kubectl
```
To install kubectl for different kubernetes versions 
```bash
https:// docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html
```

3. We need to configure kubectl to communicate with the cluster for that we need to create kubeconfig for Amazon EKS, to create kubeconfig we need to    install AWS-CLI on our computer.

**Install aws-cli**

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86\_64.zip" -o "awscliv2.zip"

unzip awscliv2.zip

sudo ./aws/install

./aws/install -i /usr/local/aws-cli -b /usr/local/bin
```
```bash
aws --version
```

Refer this link for aws-cli installation - [https://docs.aws.amazon.com/eks/latest/](https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html)

[userguide/create-kubeconfig.html](https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html)

4. After completion of **aws-cli** installation we need to create kubeconfig file,  **To Create kubeconfig file automatically follow the steps**
- Check the aws-cli version by run the command 
```bash
aws --version
```
- Create or update a kubeconfig file for your cluster

```bash
aws eks update-kubeconfig --region region-code --name cluster-name
```
- Test your configuration 

```bash
kubectl get svc
```

Refer this link for creating kubeconfig - [https://docs.aws.amazon.com/eks/ latest/userguide/create-kubeconfig.html](https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html)

**Steps to deploy application to EKS**

1. To deploy the appilcation to eks we need to clone the code templates 

(https://gitlab.com/stackexpresso/devops/code-templates/-/tree/master/kubernetes/application-deployments/nginx-example/nginx-example-web)

2. Create a namespace

    For example 
    ```bash
    kubectl create namespace example
    ```

3. To Deploy the application to EKS first we need to apply the deployment.yaml file (nginx-example-web-deployment.yaml) which we have in code templates   

```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-example-web
  namespace: example
  labels:
    app: nginx-example-web
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx-example-web
  template:
    metadata:
      labels:
        app: nginx-example-web
    spec:
      containers:
      - name: nginx-example-web
        image: nginx
        ports:
        - containerPort: 80
        env:
          - name: NODE_ENV
            value: production
          - name: API_HOST
            value: https://test.example.com
          - name: REACT_APP_API_HOST
            value: https://test2.example.com
```
To apply the deployment manifest file we need to run the following command 
```bash
kubectl apply -f nginx-example-web-deployment.yaml
 ```
4. Instead of hard coding the values in the deployment file we can create secrets.yaml file (if we need any secrets in deployment file)

Example:
```bash
apiVersion: v1
kind: Secret
metadata:
  name: keys-appname-web
  namespace: example
type: Opaque
data:
  DB_HOST: cGctc2Vy=
  DB_PORT: NTQswD==
  DB_USERNAME: b21ub21u=
  DB_PASSWORD: M28yUM28yUM28yUM28yU=
  DB_NAME: b2b2b2==
  SECRET_KEY_BASE: NWFtNWFtNWFtNWFtNWFtNWFtNWFtNWFtNWFtNWFtNWFt=
  AWS_ACCESS_KEY_ID: MzRhMzRhMzRhMzRhMzRhMzRhMzRhMzRhMzRh==
  AWS_SECRET_ACCESS_KEY: VUdVUdVUdVUdVUd=
  AWS_REGION: c2Zvc2Zvc2Zvc2Zvc2Zvc2Zv
  AWS_ENDPOINT: TmVTmVTmVTmVTmVTmVTmV==
```

To apply secret.yml file run 
```bash
kubectl apply -f secret.yml
```
5. After apply the deployment file we need to apply service manifest file (nginx-example-web-service.yaml ) which we have in code templates

   then it will make our application accessible
```bash
apiVersion: v1
kind: Service
metadata:
  name: nginx-example-web
  namespace: example
spec:
  type: ClusterIP
  ports:
    - port: 80
      targetPort: 80
      protocol: TCP
  selector:
    app: nginx-example-web
```
To apply the service manifest file we need to run the following command   
```bash
kubectl apply -f nginx-example-web-service.yaml  
```    
it will create the service for our application.

6. Now we need to apply the ingress file (nginx-example-ingress.yaml) which we have in code templates, ingress will manage external access to our service
```bash
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: nginx
  namespace: example
  annotations:
    kubernetes.io/ingress.class: nginx
    cert-manager.io/cluster-issuer: letsencrypt-prod
    nginx.ingress.kubernetes.io/rewrite-target: /$1

spec:
  tls:
  - hosts:
    - example.com
    - example1.com
    secretName: nginx-example-tls
  rules:
  - host: example.com
    http:
      paths:
      - backend:
          serviceName: nginx-example-web
          servicePort: 80
        path: /(.*)
  - host: example1.com
    http:
      paths:
      - backend:
          serviceName: nginx-example-web
          servicePort: 80
        path: /(.*)
``` 


   
To apply the ingress we need to run the following command 
```bash
kubectl apply -f nginx-example-ingress.yaml
```
it will apply the ingress rules for our service

7. To see the all resources exist in the name space run 
```bash
kubectl get all -n example
```
8. Now we need to create ssl certificate for that follow the link - **https:// kubernetes.io/docs/tasks/administer-cluster/certificates/**
