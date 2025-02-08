# Jenkins Master-Agent (Controller-Agent) Architecture

Jenkins cluster consists of multiple machines (nodes) working together.

- Controller (Master): Manages jobs, scheduling, and delegating tasks to agents
- Agents (Slaves/Workers): Execute build jobs on separate nodes

When Jenkins is configured with multiple agents, it essentially forms a Jenkins cluster, distributing workloads across different machines.

### **Jenkins as a Cluster and the Role of Port 50000**  

Jenkins runs as a **cluster** when it operates in a **Master-Agent (Controller-Agent) architecture** where multiple nodes (agents) execute build jobs distributed by the controller (formerly called master). The controller handles scheduling, job management, and plugin configurations, while agents handle actual build execution.

---

## **1. When Does Jenkins Run as a Cluster?**  
Jenkins forms a **cluster** in the following scenarios:

### **a) Static Agent-Based Cluster**  
- A **Jenkins controller** is installed on one machine.
- Multiple **static agents (dedicated build machines)** are connected.
- The controller assigns jobs to agents based on labels, workload, or job configurations.
- **Example Use Case**:  
  - A **Windows agent** for Windows-based builds.  
  - A **Linux agent** for Linux-based builds.  
  - A **Mac agent** for iOS builds.

### **b) Dynamic Agent-Based Cluster (Cloud & Kubernetes)**  
- Agents are **spun up on-demand** using **Docker, Kubernetes, or cloud VMs**.  
- Jenkins dynamically provisions and deprovisions agents based on the workload.  
- **Example Use Case**:  
  - Running short-lived agents inside **Docker containers**.  
  - Deploying ephemeral **Kubernetes pods** for each job.  
  - Using **AWS EC2, GCP, or Azure VMs** as auto-scaling agents.  

### **c) Multi-Master High Availability (HA) Cluster**  
- Multiple **Jenkins controllers** are set up with **load balancing** for fault tolerance.  
- Agents connect to different controllers to distribute the load.  
- **Example Use Case**:  
  - Large-scale enterprise setups with high traffic.  
  - Avoiding a single point of failure by running multiple controllers.  
  - Requires external **load balancers (NGINX, HAProxy, or Kubernetes Ingress)**.

---

## **2. What is Jenkins Using Port 50000 For?**  

### **a) Port 50000 = JNLP (Java Network Launch Protocol) for Agents**  
Jenkins uses **port 50000** for **JNLP-based agents (Java Web Start agents)** to connect to the controller. This is the default **TCP port** used for agent-to-controller communication.

#### **How JNLP Agents Work via Port 50000**
1. An agent requests a connection from the controller.
2. The agent **downloads the `agent.jar`** file.
3. The agent starts and **connects back via port 50000**.
4. The controller assigns jobs to the agent.

#### **Checking Port 50000 on the Controller**
To verify if Jenkins is listening on **port 50000**, run:
```bash
netstat -tulnp | grep 50000
```
or  
```bash
ss -tulnp | grep 50000
```
If Jenkins is running, it should show something like:
```
tcp   LISTEN   0   50   0.0.0.0:50000   0.0.0.0:*   users:(("java",pid=12345,fd=5))
```

---

## **3. How to Configure Port 50000 in Jenkins?**  

### **a) Change Port 50000 (Optional)**
- Go to **Manage Jenkins** → **Configure Global Security**.
- Scroll down to **Agent Protocols & Security**.
- Change **TCP port for JNLP agents**:
  - **Fixed Port (Default: 50000)** → Change to another port (e.g., `51000`).
  - **Random (Each Restart)** → Randomly assigns a new port each restart.
  - **Disable JNLP** → If not using JNLP agents.

### **b) Open Port 50000 in Firewall (If Needed)**
If the agent cannot connect, check firewall rules:
```bash
sudo ufw allow 50000/tcp  # Ubuntu
sudo firewall-cmd --add-port=50000/tcp --permanent && sudo firewall-cmd --reload  # CentOS/RHEL
```

---

## **4. Alternatives to JNLP (Port 50000)**
If you don't want to use JNLP-based agents, consider:

| **Method** | **Description** | **Best For** |
|------------|----------------|--------------|
| **SSH Agent** | Agents connect via SSH instead of JNLP. | Secure, Linux-based agents. |
| **Docker Agents** | Agents are launched inside Docker containers. | Ephemeral builds, isolation. |
| **Kubernetes Agents** | Agents run as Kubernetes pods dynamically. | Cloud-native environments. |
| **Cloud Agents** | Agents are auto-scaled in AWS, Azure, GCP, etc. | Cloud-based workloads. |

To switch to **SSH agents**, configure **"Launch agents via SSH"** in **Manage Nodes** instead of using JNLP.

---

## **5. Example: Running a Jenkins Cluster with Port 50000**
If you have a **Jenkins controller** and an **agent**, set up the agent manually:

### **Step 1: Start Jenkins Controller**
Ensure Jenkins is running:
```bash
sudo systemctl start jenkins
```
Check if port 50000 is open:
```bash
netstat -tulnp | grep 50000
```

### **Step 2: Start Agent Manually**
On the agent machine:
```bash
wget http://your-jenkins-url/computer/agent-node/slave-agent.jnlp
java -jar agent.jar -jnlpUrl http://your-jenkins-url/computer/agent-node/slave-agent.jnlp -secret YOUR_SECRET -workDir "/home/jenkins/agent"
```

### **Step 3: Verify Connection**
- Go to **Manage Nodes and Clouds** in Jenkins.
- The agent should appear as **"Connected"**.

---

## **6. Conclusion**
- **Jenkins runs as a cluster** when using multiple agents (static, dynamic, or cloud-based).
- **Port 50000 is used for JNLP-based agent communication** but can be changed or disabled.
- **Other connection methods (SSH, Kubernetes, Docker) are often preferred** over JNLP for security and flexibility.
