# Jenkins-Configuration-as-Code(CasC)
Demo to use Configuartion As Code Plugin for Jenkins

OS: Ubuntu 21.10,
Docker Version: 20.10.17,
Kubectl Server Version: 1.26,
Minikube Version: v1.29.0

This demo is divided into two parts:
1. Setting up Jenkins on Minikube Cluster using YAML files.
2. Installing Configuration As Code plugin and using it.

Setting up Jenkins on Minikube Cluster using YAML files.

Here, I have installed Docker on my Ubuntu OS, and running my Minikube Cluster on Docker. Now I will setting up Jenkins on Minikube Cluster using YAML files. All YAML files are uploaded to GitHub Repo.

Steps:

1. Start Minikube Cluster. Command: minikube start

This might take upto few minutes. Once the minikube cluster starts, it should look like this:

![Screenshot from 2023-02-15 13-20-09](https://user-images.githubusercontent.com/122020679/218965458-1a3454f6-51b5-4ccd-a320-339e66845e3f.png)

2. Check Cluster Status. Command: kubectl cluster-info

![Screenshot from 2023-02-15 13-23-57](https://user-images.githubusercontent.com/122020679/218966627-4defb15a-5dd3-400d-bb82-db2ec358d72b.png)

3. Check Minikube Status. Command: minikube status

![Screenshot from 2023-02-15 13-27-23](https://user-images.githubusercontent.com/122020679/218966994-348fe6c2-266a-42ce-963e-d432f424b8c4.png)

4. Create namespace for your setup. Command: kubectl create namespace jenkins

    check if namespace is created. Command: kubectl get ns or kubectl get anmespace
    
    ![Screenshot from 2023-02-15 13-30-34](https://user-images.githubusercontent.com/122020679/218967915-62ae5a5b-38d8-485c-a0bf-d0cf51329491.png)
    
5. Create manifest service-acconnt.yaml with name jenkins-admin giving permission and apply it.

   Commond to create manifest: kubectl create -f service-account.yaml -n jenkins
   
   ![Screenshot from 2023-02-15 13-43-48](https://user-images.githubusercontent.com/122020679/218972140-f31530b2-6f81-40e2-82e7-7665b8c345f2.png)

   
   NOTE: Don't forget to mention "-n jenkins". It is the namespace we are working in. All manifests should be created in the same namespace.
   
   

6. Once manifest is created, apply it. 

   Command: kubectl apply -f service-account.yaml -n jenkins

   Check if manifest is created. Command: kubectl get serviceaccount -n jenkins
   
   ![Screenshot from 2023-02-15 13-53-32](https://user-images.githubusercontent.com/122020679/218972566-916364e5-bf68-4fb6-bf49-3319bcca0564.png)

7. Create and apply manifest volume.yaml

   Command: kubectl create -f volume.yaml -n jenkins
   
   Check if volume.yaml is created.
   
   command: kubectl get persistentvolumeclaim -n jenkins
   
   Apply command: kubectl apply -f volume.yaml -n jenkins
   
   ![Screenshot from 2023-02-15 14-02-29](https://user-images.githubusercontent.com/122020679/218974548-c485ead3-4f1a-44af-b114-acd7cd7c26a6.png)
   
8. Create and apply manifest deployment.yaml

   Command: kubectl create -f deployment.yaml -n jenkins
   
   ![Screenshot from 2023-02-15 15-13-09](https://user-images.githubusercontent.com/122020679/218991994-2af7ca1d-0ebe-4c05-95f4-115337690302.png)

   
   Check if deployment.yaml is created.
   
   Command: kubectl get deployments -n jenkins
   
   ![Screenshot from 2023-02-15 15-16-04](https://user-images.githubusercontent.com/122020679/218992230-81c56631-1d62-4cc0-8e9d-552e533941f6.png)

  
   Apply command: kubectl apply -f deployment.yaml -n jenkins
   
   ![Screenshot from 2023-02-15 15-17-01](https://user-images.githubusercontent.com/122020679/218992494-1b06e096-447d-4922-95b8-c787df498a1e.png)

9. Create and apply manifest service.yaml

   Command: kubectl create -f service.yaml -n jenkins
   
   Check if service is created: kubectl get services -n jenkins
   
   Apply: kubectl apply -f service.yaml -n jenkins
   
   ![Screenshot from 2023-02-15 15-19-47](https://user-images.githubusercontent.com/122020679/218993338-d959822a-0c63-44da-b085-707a68faec67.png)


10. Now, we will access the Jenkins Dashboard from our browser.

    Enter Command: minikube ip

    We will use this ip to access Jenkins Dashboard exposed on port 32000

    ![Screenshot from 2023-02-15 15-23-27](https://user-images.githubusercontent.com/122020679/218994136-dea40d77-9024-4a35-9589-3cbae02c2a35.png)
    
11. To access Jenkins Dashboard, we will need an access key.
    
    Enter command: kubectl get pods -n jenkins
    
    Copy the pod name and enter the following command: kubectl logs <podname> -n jenkins
    
    ![Screenshot from 2023-02-15 15-26-52](https://user-images.githubusercontent.com/122020679/218995345-96c71824-11fd-415a-9a06-a22ae9697c53.png)
    
    Here, we get our access key:
    
    ![Screenshot from 2023-02-15 15-29-10](https://user-images.githubusercontent.com/122020679/218995672-022517a0-9df2-4600-873e-f7704671bc77.png)
    
    Copy this key and open your browser. In the URL bar enter: <minikube ip>:32000 and press enter. 
    
    ![Screenshot from 2023-02-15 15-30-52](https://user-images.githubusercontent.com/122020679/218996241-dcd57a8f-1437-4279-836e-c6558ef490af.png)
    
    After the page is loaded, we will be able to see Jenkins Dashboard:
    
    ![Screenshot from 2023-02-15 15-33-39](https://user-images.githubusercontent.com/122020679/218996751-3dba3ae8-f1ef-4b46-9cdc-1157a8a4b352.png)
    
    Paste the copid access key and press contine.
    
    Select "Installed Jenkins Plugin"
    
    ![Screenshot from 2023-02-15 15-35-08](https://user-images.githubusercontent.com/122020679/218997411-cfcfb723-b003-4eb7-8dc7-110feff3f1b7.png)
    
    Wait while Plugins are being installed.
    
12. Sign-up with Jenkins.Create User. Save and Continue.

    ![Screenshot from 2023-02-15 15-44-26](https://user-images.githubusercontent.com/122020679/218999587-9f67c72e-05a9-4e0c-8611-49d1ff13a6f0.png)
    
    
    ![Screenshot from 2023-02-15 15-47-56](https://user-images.githubusercontent.com/122020679/219000235-2502b953-a305-4759-9b30-3b0f89decd01.png)
    
    
    ![Screenshot from 2023-02-15 15-49-48](https://user-images.githubusercontent.com/122020679/219001728-289599ae-a874-41bc-8a52-13b5ac9ffe9a.png)
    
    We have successfully signed up with Jenkins
    
    ![Screenshot from 2023-02-15 15-56-22](https://user-images.githubusercontent.com/122020679/219002043-18a7ace3-9d63-4633-a81c-18b146fb6846.png)
    
    
 
Installing Configuration As Code plugin and using it

Steps:

1. Install "Configuration As Code" Plugin

   Go to: Dashboard -> Manage Jenkins -> Manage Plugins -> Available Plugins
   
   ![Screenshot from 2023-02-15 16-02-16](https://user-images.githubusercontent.com/122020679/219003321-d2651e84-91b8-4a24-80c7-36cca5d294bd.png)
   
   Search for: Configuration As Code and press Install without restart
   
   ![Screenshot from 2023-02-15 16-04-48](https://user-images.githubusercontent.com/122020679/219003910-7f8d473d-08a3-4d27-89b6-6e60999fb5d2.png)
   
   Wait while plugin is being installed
   
   ![Screenshot from 2023-02-15 16-05-02](https://user-images.githubusercontent.com/122020679/219004254-474f5ee7-f321-46a9-8b71-cbb712378fa4.png)

2. Plugin is now installed and can be seen on Manage Jenkins

   ![Screenshot from 2023-02-15 16-08-10](https://user-images.githubusercontent.com/122020679/219004719-2603432c-50b1-4a95-922c-394ce6db663f.png)
   
   
3. Before we see how to use Configuration As Code, we need to update few security settings. For this go to: Dashboard -> Manage Jenkins -> Configure Global Security

   ![Screenshot from 2023-02-15 16-15-07](https://user-images.githubusercontent.com/122020679/219006161-bab60b87-520b-4518-8583-01cd245f06f9.png)
   
   Check the "Allow users to sign up"
   
   ![Screenshot from 2023-02-15 16-16-36](https://user-images.githubusercontent.com/122020679/219006469-691e970a-4a75-45e8-b870-125e57bcd13e.png)
   
   Next, under Authorization, select "Matrix Based Security"
   
   ![Screenshot from 2023-02-15 16-17-41](https://user-images.githubusercontent.com/122020679/219006922-9f853341-5e5a-4b0f-a04c-9219271fec55.png)
   
   Check the "read" box under Authenticated Users then click on "add user"
   
   ![Screenshot from 2023-02-15 16-19-35](https://user-images.githubusercontent.com/122020679/219007433-8d251041-834c-4443-aa64-51a3720d00e7.png)
   
   Enter user and click "Ok"
   
   Check all boxex for user and save.
   
   ![Screenshot from 2023-02-15 16-21-47](https://user-images.githubusercontent.com/122020679/219007838-90a5e792-3269-4f28-b058-888a2eeca76f.png)
   
   
4. Now we see how to use Configuration As Code Plugin. We are going to add "git" credentials using CasC. We'll need a manifest jenkins-demo.yaml to add credentials.

   We don't have any credentials right now.
   
   ![Screenshot from 2023-02-15 17-34-31](https://user-images.githubusercontent.com/122020679/219022979-868f0fec-1d52-49e2-a817-f3cdd58fab26.png)


5. Create a manifest jenkins-demo.yaml and push it to your GitHub Repository.
    
   ![Screenshot from 2023-02-15 18-08-13](https://user-images.githubusercontent.com/122020679/219029582-fa653049-1a20-4c62-a673-bc9ff7f0d549.png)
   

6. Now go to your jenkins-demo.yaml file in your repo and click "raw". Copy the URL 

   ![Screenshot from 2023-02-15 18-10-05](https://user-images.githubusercontent.com/122020679/219030064-94c13608-9150-4ca4-bd6f-698b6acc70c3.png)

7. Go to Jenkins Dashboard -> Manage Jenkins -> Configuration As Code

   Paste the URL and click on "Apply new configuration"
   
   ![Screenshot from 2023-02-15 18-13-41](https://user-images.githubusercontent.com/122020679/219030608-34585694-d358-4d31-8e74-b4c2630aa12c.png)
   
   
8. Now go to Dashboard -> Manage Jenkins -> Manage Credentials
  
   And now we can see that new credentials have been added.
   
   ![Screenshot from 2023-02-15 18-15-54](https://user-images.githubusercontent.com/122020679/219031087-91cfc358-1ae9-4bd5-b52c-5b7e99447f85.png)
   
   
We have Successfully added credentials using Configuration As Code.
