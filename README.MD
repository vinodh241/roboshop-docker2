# Docker:#  BY VINODH AVALA
---------------------------


* Terraform provisions infrastructure, Ansible configures systems, * Docker packages applications*, and Kubernetes manages application runtime. Terraform is not suitable for deployments because it is state-driven and not designed for continuous runtime management, scaling, or self-healing. Kubernetes is built exactly for that purpos

* for i in cart catalogue mongodb mysql shipping user payment frontend; do cd $i; docker build -t vinod9072/$i:v1 . ; docker push vinod9072/$i:v1 ; cd ..;done


* Hypervisor
--------------
Hypervisor oe Virtual Machine Monitor (VMM), is software that enables a single physical host machine to run 
manage and provision multiple isolated virtual machines (VMs).

* VM
-------------
Logically devides physical hardware into multiple servers 

* IMAGE:
----------------
IMAGE =  Base min OS + system packages + Application code and dependencies = ( ex:500MB)

    VM                      vs               container
----------------------------------------------------------------------------
1. cost                     |            less cost 
2. Moretime (boot time)     |            less time (boot time)
3. more size                |            small size
4. more resources           |            less resources 
5. block the resources      |            Dynamic resource allocation
6. Hypervisor               |            no need of any extra software
7. Not portable             |            portable
8. More secure              |            less secure (we can achive security)
--------------------------------------------------------------------------------

# Docker:
--------------

 * /var/lib/docker/ -- docker home dirctroy


 * install Docker commands:
    -----------------------------
_-----------------------------------------------------------------------------------------------------------

    sudo dnf -y install dnf-plugins-core

    sudo dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo

    sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

 -----------------------------------------------------------------------------------------------------------   

Add your normal user to docker
 * by default we cant get the direct access for docker engine, for getting access to docker we have to add our user to docker group

   * sudo usermod -aG docker ec2-user 

----------------------------------------------------------------------------------------------------------

Docker commands:
---------------

* docker ps -- Running containers 
* docker ps -a -- Running + stopped conatiners ( All)
* docker images -- images available on your system

Docker IMAGE:
-------------

docker image: 

docker pull <image-name>:<tag/version>  -- download the images form central repo 

* docker rmi <image_id/image_name> -- remove the image


Docker Container:
----------------

container is nothing but running instance of image

    * it is also a mini server 

* docker create <image-name>/nginx -- creates a container 

* docker start <container_id/containername> -- start the container

* docker stop <container_id/containername> -- stop the container

* docker rm <container_id/containername> -- remove the container

* docker run <image_name/image_id>:tag   -- it will pull the image form central repo + creates container + start container too 

* docker run -d <image_name/image_id>:tag -- detach mode run the container in background 

* docker ps -a -q -- to get the all container ID's

* docker rm -f `docker ps -a -q` -- it will remove all containers ( stopped+ running containers )

* docker run -d -p --name <name> <imagename>:tag  -- to specify the name for container 



# docker port mapping:
  ====================

* docker run -d -p    80 :    80            nginx
                      |       |
                 hot-port  container-port

 * here host port anything itr our wish but container port should be 80 only , bcz nginx listing port 80 only 

 * docker run -d -p 80:80 nginx:latest 


    * we can't use host port agian ( ex: port 80 allocated to nginx container-1 , but we cant use same port for container -2 )
    * when you use docker run command every time one container will create (container port 80 we can use , every time new container created (i;e 0-65568 ports). if you want create another container then use diff host port

      * docker run -d -p 81:80 naginx:v1.0 

* docker run -d -p --name <name> <imagename>:tag  -- to specify the name for container 

    * we cant use same name and same host for another container, if you want use diff name & host port for another container 

* docker exec -it <container_name/contianer_Id> bash -- to login to the inside container 

 * CTRL + P + Q -- exit from  running container with out stopping ( container ) 

 * docker inspect <containername/container_id>  -- to check the container IP address 


How can you create custom images to your applications?
-----------------------------------------------------------------------------------------

* using Dockerfile we can create custom images 

# Dockerfile:
   ----------

* A set of instructions to create customized images

   # FROM:
     -----

     * FROM  almalinux:tag  -- Dockerfile must should start from < FROM  > instrution
   
* docker build -t <imagename>:tag  . --> to build the image using docker file
                |                  |
              tag        <current-directroy has dockerfile>

    ex: docker build -f from:v1 . 

 * NOTE: you should run where the dockerfile is available 

    * FROM run:v1  ( here run:v1) --> docker will check the image with  (run:v1) locally frist and then checks docker central repo if exits pull from there otherwise it will give error ( unable to fetch the image)

   # RUN:
     -----

     * RUN instructions configure the image like installing packages , doing some configurations etc ..
      
     * RUN dnf install nginx -y 

     * we can have multiplr run commands in dockerfile 

-----------------------------------------------------------------------------------
      * RUN executes at the time of image creation 
-----------------------------------------------------------------------------------    
      
   # CMD:
    ------

     * CMD  executes at time of container creation (i;e at the time of docker run  )

     * CMD ["instructions"]

-----------------------------------------------------------------------------------
     * CMD  executes at the time of container creation 
-----------------------------------------------------------------------------------    
     * we can't have multiple CMD commands in a dockerfile , if you mentioned multiple CMD , docker will excute the last CMD of  dockerfile

  # COPY:
    -----

     * COPY the files from source to destination 

     * COPY  source dest  -- copy the code from local to container 

 # ADD
  -----
     * ADD and COPY both are copy the code from local to the container but ADD has two  more advantages  
       
       i) It can directly fetch the code from internet
       ii) it can directly untar or unzip into the container 

 # LABEL:
    ------

     * LABEL is nothing but tag 

     * Using LABEL we can filter the images , suppose we have 100 images, 

      ex: docker images --filter "label=APP=NGINX"

# How can you push images into docker hub?
=================================================================================================

  * before pushing image into docker hub login to the  docker hub

   * docker login 

   * Then re-tag  with username  form existing image 

* docker tag <image_name:v1> <username/image_name:v1>

    ex: docker tag label:v1 vinod9072/lable:V1 

 * docker push URL/username/image-name:version 

    ex: docker push <image_name>

==================================================================================================

# EXPOSE:
  ----------
  * EXPOSE is using for informing to the user for which port is using 

    * EXPOSE 8080 
# ENV
 -------

 * It is like variables we can use inside container or server 
 
    * ENV   COURSE="DEVOPS" \
            TARINER="VINODH" \
            DURATION="120HR" 

# ENTRYPOINT
 -------------

 * CMD instruction can be overridden 
 * ENTRYPOINT -- we cant override ENTRYPOINT , if we try to do it will override  what you pass the command during run 

# USER:
 ---------

 * USER instruction to use  which user should run the container after container creation 
   * before that create a user and add mention that user in dockerfile 
    Ex: useradd vinod

   * USER vinod 

# WORKDIR:
  ---------
 
* WORKDIR instruction to set the path where our source code should run inside the container 
 
  * WORKDIR /tmp/app/

# ARG:
 ------

 * ARG is build time variable , they cant be access inside container . ENV can be access build and inside container also 
  * ARG instruction variables can be overridden 

   * docker build -t arg:v1 --build-arg trainer=vinodhAvala . 

   useually FROM instruction is the frist argmunet in dockerfile but , in exceptional case we can pass ARG is the 
   frist instruction in docker file to supply version to base os 

   ARG veriosn 
   FROM almalinux:9 

   if you want to change the version during build time use ARG 

   * docker build -t arg:v1 --build-arg version:10  . 

 * ARG tarianer 

  ** HOW can you use ARG inside container?
  ------------------------------------------------------

   ex:  ARG version
        ENV version=$version 


# ONBUILD:
----------------
   * while developing the images you can put some  confitions while others are using the  your images 



=================================================================================================================

# Docker Networking
--------------------
 * When you install docker , docker its self installing network by defalut 
  * There are two types of networking 

   * whenever uses container docker uses bridge network
   * Docker default bridge network conn't communicate containers , so docker always suggest to create custom bridge network

     * docker network ls  -- list of network
     * docker network create <name>

     * Docker Bridge network -- Bridge means docker creates seperate network interface and assign the IP address to containers 
     * Docker host network  -- host means direclty host network

     * docker network disconnect bridge catalogue -- disconnect from default network 

     * docker network connect <networkname> microservices -- to connect customnetwork 


##########################################################################################################

# Docker Compose
-----------------
* docker compose is command line tool , where you can define all containers as services 

* docker compose is a commad line tool to manage mutli container applications, we can define all the containers as services , create dependencies between them , start them at once and  stop them at once 

 EX: 
* networks:
  default:
    driver: bridge
    name: roboshop

    external: true 
    
    * I created a network already using docekr network create roboshop , so external is ture . otjer wise mention the network which you want create same as above

    * services:
       mongodb:
        image: <image_name>:tag
        container_name: mongodb
         * if it depends on any other microservices mention the dependencies like below
        depends_on:      
            - user
   
      * If you want create any default user inside compose file mention like below 
        environment:
          RABBITMQ_DEFAULT_USER: roboshop
          RABBITMQ_DEFAULT_PASS: roboshop123

   * How to run docker compose file?
   -----------------------------------

   * docker compose up -d  
   * docker compose down

------------------------------------------------------------------------------------------------------------------------

# Docker optimization:
  -----------------------

  * 








    















