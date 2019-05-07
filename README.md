# jenkins_ansible
Jenkins is free and open source continues integration tool and it’s code is written in Java. It provides the feature of continues build and deployment or in other words we can say it is a automation server. 
Jenkins are used where continues build and integration is going on for software development.

# Jenkins Installation

Make sure you are logged in the system as admin (with sudo permission). 

1- Jenkins is a Java application, so the first step is to install Java. I supposed that there is jenkins already installed.
   If not available, then install using the command.
   
   sudo yum install java-1.8.0-openjdk-devel
   
2- Ansible firstly downloading the Jenkins Repo at location /etc/yum.repos.d/jenkins.repo.
   
    - name: Download Jenkins Repo
      get_url:
        url: http://pkg.jenkins-ci.org/redhat/jenkins.repo
        dest: /etc/yum.repos.d/jenkins.repo
       
3 - Next step is to download repo rpm key. Using the Ansible rpm_key module. 

    - name: Download Key
      rpm_key:
        key: https://jenkins-ci.org/redhat/jenkins-ci.org.key
        state: present
        
4- Installing the jenkins in the system. As we have all the required file to install the jenkins.

    - name: Install Jenkins
      yum:
        name: jenkins
        state: present
        
5- After installtion of the jenkins we need to start the jenkins service. 

    - name: Start jenkins service
     service:
       name: jenkins
       state: started
       
6- After installation and service start just copy the ip of the machine and paste it in the browser. As we know that jenkins default port
   is 8080, put the port number as 8080 (i.e 10.20.0.30:8080). User can change the default port as per the requirement.
   
7- Jenkins provide default password for first time login into the jenkins. We can fetch that password using the cat command. But here we
   we will not do anything manually.
   
     - name: Run command to retrive the default password
       command: cat /var/lib/jenkins/secrets/initialAdminPassword
       register: initialAdminPassword
       
8- To see the default password we are using debug module of the ansible. The above code stored the initialAdminPassword in a variable
    register. Using the stdout we dispaly the initial password.
    
      - name: See the retrived password using debug
        debug:
          var: initialAdminPassword.stdout
     
