### Project: **Deploying a Scalable Web Application Using Red Hat OpenShift**

#### **Objective:**

Deploy a scalable web application using Red Hat OpenShift for container orchestration, Red Hat Enterprise 
Linux (RHEL) for the operating system, and Red Hat Ansible Automation Platform for configuration 
management and automation.

#### **Project Overview:**

1.  **Setup Red Hat Enterprise Linux (RHEL) Servers**
2.  **Install and Configure Red Hat OpenShift**
3.  **Deploy Web Application**
4.  **Configure Automated Deployment with Ansible**

### **1. Setup Red Hat Enterprise Linux (RHEL) Servers**

-   **Provisioning:**
    
    -   Use Red Hat Cloud Infrastructure or any cloud provider to provision virtual machines running RHEL.
    -   Ensure each server has network connectivity and is properly configured with security groups and 
firewalls.
-   **Basic Configuration:**
    
    -   Install necessary updates:

		```bash
		sudo hostnamectl set-hostname openshift-master
		```

	- Configure networking and hostname:
		```bash
		sudo hostnamectl set-hostname openshift-master
		```

	- Set up firewall rules:
		```bash
		sudo firewall-cmd --permanent --add-port=6443/tcp
		sudo firewall-cmd --permanent --add-port=80/tcp
		sudo firewall-cmd --permanent --add-port=443/tcp
		sudo firewall-cmd --reload
		```

### **2. Install and Configure Red Hat OpenShift**

-   **Prerequisites:**
    
    -   Ensure that you have the OpenShift installation media or access to Red Hat OpenShift Installer.
-   **Installation Steps:**
    
    -   **Install OpenShift CLI:**

		```bash
		sudo dnf install -y origin-clients
		```
	- **Download and Extract OpenShift Installer:**
	```bash
	wget 
https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/openshift-client-linux-<version>.tar.gz
	tar -xvf openshift-client-linux-<version>.tar.gz
	sudo mv oc kubectl /usr/local/bin/
	```
	- **Create a Configuration File:** Create a configuration file for the OpenShift installation:

		```yaml
		apiVersion: v1
		baseDomain: example.com
		metadata:
		  name: mycluster
		platform:
		  baremetal:
		    hosts:
		      - role: master
		        address: <master-ip>
		      - role: worker
		        address: <worker-ip>
		```
	- Run OpenShift Installer:

		```bash
		openshift-install create cluster --dir=<dir> --log-level=info
		```

### **3. Deploy Web Application**

**Create a Project:**

```bash
oc new-project my-web-app
```
**Deploy a Sample Web Application:** Create a deployment configuration for a sample application. For 
example, using a simple Nginx container:

```bash
oc new-app nginx
```
**Expose the Application:**

```bash
oc expose svc/nginx
```
**Verify Deployment:** Access the application using the URL provided by the service.


### **4. Configure Automated Deployment with Ansible**

**Install Ansible:**

```bash
sudo yum install -y ansible
```

**Create Ansible Playbook:** 
Define an Ansible playbook to automate the deployment of the web application:

```yml
---
- hosts: localhost
  tasks:
    - name: Deploy web application
      shell: |
        oc new-app nginx
        oc expose svc/nginx
```

#### ** Run Ansible Playbook: **

```bash
ansible-playbook deploy-web-app.yml
```

### **Summary**

This project involves setting up a Red Hat Enterprise Linux environment, deploying Red Hat OpenShift for 
container orchestration, deploying a sample web application, and automating the deployment process with 
Ansible. This setup ensures scalability, automation, and efficient management of your web application 
infrastructure.

