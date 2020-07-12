<h1> DevOps-Task4 </h1>
<h2>Objective : -</h2>
  
<h3>To deploy webserver on Kubernetes using a Dynamic Jenkins Cluster.</h3>

<h3>Steps :-</h3>

1. Create a Dockerfile which has kubectl and ssh configured.
2. Go to docker service and configure it, so that the jenkins docker plugin can connect with it. 
3. The above created Dockerfile is then use as a Dynamic Node.
4. Now to create Kubernetes pods and deployments, create another Dockerfile which will acts as the webserver image.
5. Then the rolling update of deployments is done using the Dynamic Slave.

   i) If launching for the first time, create a deployment of the pod using the image created in the previous job. If the deployment already exists then just roll-out so that    there is zero downtime for the user.
   
    ii) After the application is created, just expose it. If the pods are already exposed then don't expose.
    
 <h3>Requirements :-</h3>
 
 1. Knowledge of Docker.
 
 2. Knowledge of Kubernetes.
 
 3. Knowledge of Jenkins.
 
 <h3>Solution :-</h3>
 
 <b>Dockerfile for kubectl and ssh:<b>
 
 ![1](https://github.com/gauravsjc02/DevOps-Task4/blob/master/dockerfile1.png)

 Build the image using the command:- <b>docker build -t kudoc:v1<b> 
  
 Here kudoc:v1 is the image name. We can give any name according to our prefrences.
  
 <b>Kubectl configuration file:<b>
  
  ![2](https://github.com/gauravsjc02/DevOps-Task4/blob/master/config.png)
 
 <b>Dynamic slave node configuration:<b>
  
  ![3](https://github.com/gauravsjc02/DevOps-Task4/blob/master/doconfig.png)
  
  To allow jenkins to communicate with docker, we configure the docker service. We edit the <b>/usr/lib/systemd/system/docker.service<b> and add <strong>-H tcp://0.0.0.0:4243</strong> to the ExecStart field.
 
 After editing, restart the docker service using:-
 
 <b>systemctl daemon reload<b>
  
 <b>systemctl restart docker.service<b>
  
 After this, we create a dynamic slave node. To create <b>Go to Manage Jenkins > Manage Nodes and Clouds > Configure Clouds > Add A New Cloud > Select Docker.</b>
 
 ![4](https://github.com/gauravsjc02/DevOps-Task4/blob/master/cloud.png)
 ![5](https://github.com/gauravsjc02/DevOps-Task4/blob/master/cloud1.png)
 
 <h4>JOB 1 :</h4> : Create a job in Jenkins, which will pull the code from the Github having the Dockerfile of a webserver, which builds and uploads the image to Docker Hub.<br>
 
 ![6](https://github.com/gauravsjc02/DevOps-Task4/blob/master/job1.png)
 ![7](https://github.com/gauravsjc02/DevOps-Task4/blob/master/job1.1.png)
 
 We use the restrict option so that the jobs are aware of where to go for its execution.
 
 <h4>JOB 2 :</h4> This job will be executed on the dynamic slave node which will be used to Create Deployment/Rolling Updates for Webserver on Kubernetes. This is an downstream   job for job1.
<br>

![8](https://github.com/gauravsjc02/DevOps-Task4/blob/master/job2.png)

<h3>Before Rolling Update:</h3>

![12](https://github.com/gauravsjc02/DevOps-Task4/blob/master/h.png)

<h3>After Rolling Update:</h3>

![13](https://github.com/gauravsjc02/DevOps-Task4/blob/master/h1.png)

 <h3>Pipeline views for Job1 and Job2 :</h3>
 
 ![9](https://github.com/gauravsjc02/DevOps-Task4/blob/master/pipeline.png)
