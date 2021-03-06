 **Project: “Cloud Computing in European schools”**  
<img src="/img/cloud-computing-logoproject.jpg" height="100" width="170">

 Number: Project: 2017-1-ES01-KA202-038471

<img src="/img/cofinanciadoEN.png" height="50" width="200"> <img src="/img/logoIES-Modificado.png" height="75" width="200">  




### Exercise no. 2: Docker
&nbsp;&nbsp;&nbsp;After learning what is the scope of PaaS,  now it is time to start the study of the first level: the **containers**.  Docker is the most used solution nowaday.  We are going to structure the learning of Docker in the next sections:
   1. Docker installation: virtual machine managed by Vagrant.
   2. Docker applications lifecycle.
   3. Docker containers: not persistent.
   4. Docker commands summary.
   5. Docker applications example: Wordpress.
   
&nbsp;&nbsp;&nbsp;The sections will be explained by following a Spanish documentation, but you can also read an English version: [Docker - English version](https://iesgn.github.io/cloudandrelated/docker.html#/).
<br/><br/>

####  Section 1.- Docker installation: virtual machine managed by Vagrant
&nbsp;&nbsp;&nbsp;  We will practice Docker by using a virtual machine managed by **Vagrant**.
<br/><br/>
 &nbsp;&nbsp;&nbsp; Vagrant is a tool for building and managing virtual machine environments in a single workflow. With an easy-to-use workflow and focus on automation, Vagrant lowers development environment setup time, increases production parity, and makes the "works on my machine" excuse a relic of the past. Machines are provisioned on top of VirtualBox, VMware, AWS, or any other provider. Go to [Vagrant](https://www.vagrantup.com/intro/index.html) for more details.
<br/><br/>
 &nbsp;&nbsp;&nbsp; Vagrant is useful for developers, for operators, for designers, for everyone.
 <br/><br/>
 &nbsp;&nbsp;&nbsp; Instead of building a virtual machine from scratch, which would be a slow and tedious process, Vagrant uses a base image to quickly clone a virtual machine. These base images are known as "boxes" in Vagrant, and specifying the box to use for your Vagrant environment is always the first step after creating a new Vagrantfile. We can access to these boxes in [Vagrant Boxes](https://app.vagrantup.com/boxes/search).
 <br/><br/>
 &nbsp;&nbsp;&nbsp;  The next **commands** are the basic ones:
   1. **vagrant box add …** →  download image / box from Vagrant repository.  Example: vagrant box add ubuntu/bionic64 --provider virtualbox  --> download an image (box) of Ubuntu verson Bionic 64 bits for VirtualBox.
   2. **vagrant box list** →   list all downloaded images / boxes.
   3. **mkdir FOLDER**  → create folder where we will create the Vagrantfile file that will contain the definition of the MV.
   4. **cd FOLDER**  →  go in inside the folder.
   5. **vagrant init BOX**  →    The MV files are located in the directory where the hypervisor (VirtualBox, VMWare ...) stores its MVs. In FOLDER, the VagrantFile file, a log file and a hidden .vagrant folder will be saved.  Example: vagrant init ubuntu/bionic64 --> the box used is ubuntu bionic 64.
   7. **vagrant up**  →  start the MV  --> We can check the virtual machine runnning by open the VirtualBox app.
   5. **vagrant ssh** →  connect to the MV
   6. **vagrant halt** → stop the MV
 <br/><br/>
 &nbsp;&nbsp;&nbsp;  Now, we know the basic concepts in order to install a virtual machine by using a Vagrant box. The next steps are:
   1. Install vagrant    
   > $ sudo apt-get install vagrant  -->  We can verify the installation by *$ vagrant --version*<br/>  
   2. Install Virtualbox
   > $ sudo apt-get install virtualbox <br/>
   
 &nbsp;&nbsp;&nbsp;The article [How to install Vagrant on Ubuntu 18.04](https://linuxize.com/post/how-to-install-vagrant-on-ubuntu-18-04/) explains the steps.

<br/><br/>
 &nbsp;&nbsp;&nbsp; 
   Finally, we are going to **install a virtual machine with Docker**. Follow [Installation of Docker - Spanish version](https://github.com/iesgn/cloudandrelated/blob/master/paas/doc/docker.md) to do it. Note: We don't use *vagrant init BOX*; instead of this, we create a Vagrantfile with the content that appears in the previous link.
   <br/><br/>
 &nbsp;&nbsp;&nbsp;  Have you been able to connect to the virtual machine and check whether "Docker" is running? *Which Docker version do you have?*
<br><br>


#### Section 2.- Docker applications lifecycle
&nbsp;&nbsp;&nbsp;After installing an environment with Docker, we are going to develop Docker images and deploy containers to run our applications. The documentation to read is [Lifecycle of Docker based applications - Spanish version](https://iesgn.github.io/cloudandrelated/es_docker.html#/). 
<br/><br/>
&nbsp;&nbsp;&nbsp;The reading must have taught you (it is showed a summary):
   1. **Create the application**. We are going to create a web page *index.html* that will be served by a web server that will run in a Docker container. The web page  and it will be saved in */home/vagrant/public_html/*: <br/>
   > $ mkdir public_html <br/>
   > $ cd public_html<br/>
   > $ echo "&lt;h1&gt;Prueba&lt;/h1&gt;" > index.html <br/>

   
<br/><br/>
 
   2. **Create a Docker image**. <br/>
   2.1.- Using a **Dockerfile** we define how we are going to create our image:
    <pre>&nbsp;&nbsp;&nbsp;
    FROM -->       Which base image we are going to use
    RUN  -->       Which packages we are going to install
    COPY -->       Where we copy our source code (web page)
    ENTRYPOINT --> We indicate the service that will run the container (apache server) </pre>

 &nbsp;&nbsp;&nbsp; Example.  Define an image with debian, install Apache2, copy our webpage to the public directory of Apache and start Apache. Note: the image *debian* is downloaded from [dockerhub](https://hub.docker.com/)
   <pre>&nbsp;&nbsp;&nbsp; FROM debian
    RUN apt-get update -y && apt-get install -y \
        apache2 \
        && apt-get clean && rm -rf /var/lib/apt/lists/*
    COPY ./public_html /var/www/html/
    ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"] </pre>
    
 &nbsp;&nbsp;&nbsp;  2.2.- **Create the Docker image**
   > $ docker build -t jlr2/aplicacionesweb:v1 .  
   
   > $ docker image ls  --> list the docker images created  
   
   <br/>
   
  3. **Create/Run a container in the development environment**.
   > $ docker run --name aplweb -d -p 80:80   jlr2/aplicacionesweb:v1  
   
   > $ docker container ls --> list the containers created  
   
&nbsp;&nbsp;&nbsp; We can check our web page by a web browser. Connect locally by using, for example, the text web browser links (URL = http://127.0.0.1).
    <br/><br/><br/>
    
   4.- **Distribute/Share the Docker image; dockerhub**
   > Create a Docker account: https://hub.docker.com  
   
   > docker push jlr2/aplicacionesweb :v1  --> Upload the image  
   
   We can check it by 2 ways:
   > docker search jlr2/aplicacionesweb  
   
   > Look for in our Docker account (dockerhub).  
   
 <br/><br/><br/>
   5.- **Deploy the application in the production environment**
   >  docker pull jlr2/aplicacionesweb:v1  --> download the image from the dockerhub  
   
   >  docker run --name aplweb -d -p 80:80   jlr2/aplicacionesweb:v1   --> create/run the container  
   
<br/><br/><br/>
   6.- **Modify the application** In case we modify the application it is necessary to build a new image:
   push   -->  modify the application  
   
   >  docker build -t jlr2/aplicacionweb:v2  -->  create the new image (in the development environment)  
   
   >  docker push jlr2/aplicacionesweb:v2  --> upload the new image  
   
   >  docker pull  jlr2/aplicacionesweb:v2  --> (download the new image to the production environment)  
   
   >  docker container rm -f aplweb  -->  delete the current container  (in the production environment)  
   
   >  docker run --name aplweb2 -d -p 80:80   jlr2/aplicacionesweb:v2  --> run the new container (in the production environment)
   
<br/><br/><br/>

#### Section 3.- Docker containers: not persistent.
&nbsp;&nbsp;&nbsp;This section explains the need to use persistent volumes due to data in containers are lost. Read [Uso de volúmenes persistentes](https://iesgn.github.io/cloudandrelated/es_docker.html#/10).  
&nbsp;&nbsp;&nbsp;This is a summary about the previous reading:  
 - Data stored in a container is not persistent.
 - When data must be stored persistently, volumes must be used.
 - The scenario is: the **application is decoupled from the data**, that is, the application will run in a container and the data in a persistent medium external to the container. Advantages:
   + If the container fails, information is not lost, you only need to create a new container.
   + If the data is updated it is not necessary to build a new image.
 - **Example: database in a persistent volume**:
    * We are going to create a container with a MySQL server; the data is stored in a persistent volume:
        > $ docker run --name some-mysql -v /opt/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=asdasd -d mysql
    * Create a database called *dbtest*
        > $ docker exec -it some-mysql bash  
        
        > root@75544a024f9b:/# mysql -u root -p -h localhost  
        
        > ...  
        
        > create database dbtest;
    * Delete the container  
        > $ docker container rm -f some-mysql
    * Create a new container:
        > $ docker run --name some-mysql2 -v /opt/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=asdasd -d mysql
    * Verify that the database is still created
        > $ docker exec -it some-mysql bash  
    
        > root@75544a024f9b:/# mysql -u root -p -h localhost  
    
        > ...  
    
       > show  databases;

<br><br>


#### Section 4.- Docker commands summary
  This section shows a summary of Docker commands. We can see examples in [Ejercicios con Docker](https://github.com/iesgn/cloudandrelated/blob/master/paas/doc/ejercicios_docker.md).
  
- List containers
    > $ docker ps  
    
    > $ docker container ls
- List images
    > $ docker image ls
- Create a container and establish an interactive session
    > $ docker run -it --name CONTAINER-NAME ubuntu /bin/bash
- Stop a container when we are inside it
    > exit
- Stop a container without being inside it
    > $ docker stop CONTAINER-NAME
- Start a stopped container
    > $ docker start CONTAINER-NAME
- Connect to a container
    > $ docker attach CONTAINER-NAME
- Create a daemon container  
    > $ docker run -d --name CONTAINER-NAME ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
- Check what a container is doing
    > docker logs CONTAINER-NAME
- Delete a container
    > $ docker rm  CONTAINER-NAME

<br><br>

#### Section 5.- Docker applications example:  Wordpress
&nbsp;&nbsp;&nbsp; Wordpress installation needs 2 containers: one for Wordpress and other for the database:
- Database server. Container: servidor_mariadb
> $ docker run --name servidor_mariadb -e MYSQL_ROOT_PASSWORD=asdasd -e MYSQL_DATABASE=wordpress -d mariadb

- Wordpress. Container: servidor_wp. This container links to the database container
> $ docker run --name servidor_wp -p 8000:80 --link servidor_mariadb:mariadb -d wordpress


&nbsp;&nbsp;&nbsp;Now, check the installation process is working:  
- We can use 'links' and connect to the URL "http://IP_servidor_wp" from the same virtual machine with docker (The IP of the servidor_wp can get by "$docker logs servidor_wp").  
- Or,from the host computer, by using the virtual machine IP address:8080 in the web browser.
<br><br>


---
This exercise  is part of the activity **Introduction to PaaS: develop, deploy, run and manage apps on the Cloud**



#### Disclaimer
&nbsp;&nbsp;&nbsp;  *"The European Commission support for the production of this publication does not constitute an endorsement of the contents which reflects the views only of the authors, and the Commission cannot be held responsible for any use which may be made of the information contained therein."*




#### Author

&nbsp;&nbsp;&nbsp;  This material has been produced by José Luis Rodríguez Rodríguez under the Creative Commons licence:  <img src="/img/Licencia-Tipo2.png" height="25" width="75">  




