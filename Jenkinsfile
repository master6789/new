apiVersion: extensions/v1beta1
kind: Deployment
metadata:
 name: jenkins-ci
spec:
 replicas: 1
 template:
  metadata:
   labels:
    name: jenkins-ci
  spec:
   imagePullSecrets:
    - name: regsecret
   containers:
   - name: jenkins-ci
     imagePullPolicy: Always
     image: some-docker-registry/jenkins-ci:latest
     ports:
     - containerPort: 8080
     - containerPort: 50000
     readinessProbe:
      tcpSocket:
        port: 8080
      initialDelaySeconds: 40
      periodSeconds: 20
     securityContext: 
       privileged: true 
     volumeMounts: 
       - mountPath: /var/run
         name: docker-sock 
       - mountPath: /var/jenkins_home
         name: jenkins-home 
   volumes: 
     - name: docker-sock
       hostPath: 
         path: /var/run
     - name: jenkins-home
       hostPath: 
         path: /var/jenkins_home

apiVersion: v1
kind: Service
metadata:
  name: jenkins-ci-lb
spec:
 type: LoadBalancer
 ports:
   - name: jenkins
     port: 8080
     targetPort: 8080
   - name: jenkins-agent
     port: 50000
     targetPort: 50000
 selector:
   name: jenkins-ci
