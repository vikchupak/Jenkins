### **Jenkins Agents** 🧑‍💻🤖

In Jenkins, **agents** (also called **slaves** in older versions) are worker nodes that run tasks assigned by the **Jenkins master**. They allow you to offload the processing from the master to other machines, enabling Jenkins to scale more efficiently by distributing workloads.

### **How Jenkins Master-Agent Architecture Works**:

- **Jenkins Master**:
  - The **Jenkins master** is the main controller of your Jenkins environment. It is responsible for:
    - Managing jobs and pipelines.
    - Scheduling tasks.
    - Distributing work to agents.
    - Providing the Jenkins web interface.
  - It doesn't actually run most of the build tasks (unless it's explicitly configured to do so); instead, it delegates those to agents.
  
- **Jenkins Agent**:
  - **Jenkins agents** are machines or containers that run build jobs. They perform the actual work requested by the Jenkins master.
  - Agents can be **physical machines**, **virtual machines**, or **containers**, depending on your infrastructure.
  - Agents are connected to the Jenkins master via a communication channel (over TCP/IP or other protocols).
  - They execute jobs, run test scripts, build software, and provide results back to the Jenkins master.

### **Types of Jenkins Agents**:

1. **Static Agents**:
   - These agents are permanently set up and registered with Jenkins. They have a fixed configuration, including:
     - Operating system.
     - Installed tools and dependencies (e.g., Java, Node.js).
     - Dedicated resources.
   - They're typically used for long-term, predictable jobs or specific tasks that require a known environment.

2. **Dynamic Agents (Cloud Agents)**:
   - These agents are provisioned dynamically when a job requires them. For instance, Jenkins can automatically spin up new agents in cloud environments like AWS, Azure, or Google Cloud to meet the demands of the job queue.
   - Dynamic agents are often used for **scaling** Jenkins and are deleted when no longer needed.

3. **Docker Agents**:
   - Docker agents are containers that act as agents within Jenkins. Jenkins can spin up a new Docker container for each job, giving the job a fresh and isolated environment. This is useful for building, testing, or deploying in environments that may differ from the Jenkins master.

### **Agent Communication with Jenkins Master**:

Agents communicate with the Jenkins master using a protocol called **Jenkins Remoting**. The master delegates tasks to the agents, and the agents send back the results of the task once they are completed. This connection is usually established through a **TCP/IP connection**, but other communication protocols can be configured.

### **Key Features of Jenkins Agents**:

- **Distributed Build Processing**: By distributing builds to agents, Jenkins can handle multiple jobs simultaneously and more efficiently. This also prevents the master from becoming overloaded with tasks.
  
- **Flexibility**: Agents can be customized with different operating systems, software, or configurations to meet specific needs. For instance, a job might require a Linux environment with Docker, while another might need a Windows environment.

- **Scalability**: Jenkins agents allow your Jenkins infrastructure to scale as your build requirements increase. New agents can be added dynamically based on the current load, especially in cloud-based setups.

### **Setting Up Jenkins Agents**:

To set up Jenkins agents, you need to:

1. **Configure the Master**:
   - In the Jenkins web interface, go to **Manage Jenkins** > **Manage Nodes and Clouds** > **New Node** to add a new agent.
   - You can specify:
     - **Node name**: The agent's identifier.
     - **Remote root directory**: The directory on the agent where Jenkins stores files.
     - **Labels**: To specify which jobs can run on the agent.
     - **Launch method**: How the agent should connect to the master. This could be through:
       - **SSH**: For Unix-based agents.
       - **JNLP**: A Java Web Start connection.
       - **Windows agent**: For Windows-based agents.
       
2. **Set Up the Agent Machine**:
   - Install Java (or other required tools) on the agent machine, as Jenkins requires Java to run.
   - Ensure that the agent can communicate with the Jenkins master (through network configurations, firewall settings, etc.).
   
3. **Connect the Agent**:
   - Once configured, the agent can connect to the master using the defined connection method. If using JNLP, the agent needs to download a `agent.jar` file from the Jenkins master and connect to it.

4. **Run Jobs**:
   - Once connected, Jenkins can assign jobs to the agent based on available resources and job requirements.

---

### **Why Use Jenkins Agents?**:

1. **Offload Build Workload**: By using agents, Jenkins can offload work to other machines, making the system more efficient and responsive.

2. **Environment Specific Builds**: Some jobs may require specific environments (e.g., Node.js, Java, etc.), and agents can be set up with the needed configurations.

3. **Scalability and Flexibility**: You can scale the build infrastructure up or down as needed, with the ability to add agents dynamically to handle more jobs during peak times.

4. **Cost-Effective**: You can use low-cost or pre-existing machines as agents, especially in cloud environments. You only need to pay for resources when required.

---

### **Types of Agent Connections**:

1. **SSH**:
   - For Linux and macOS agents, Jenkins can connect via SSH to a remote server to manage the agent.
   
2. **JNLP (Java Web Start)**:
   - This method is more common when dealing with Windows-based agents or when SSH is not an option. The agent machine connects to the Jenkins master via a secure protocol.

3. **Docker Agents**:
   - Docker containers can act as Jenkins agents, providing isolated environments for each job. This is particularly useful for building Docker images or testing in different environments.

---

### **Example: Setting up a Jenkins Agent via SSH**:

1. On the **master**, go to **Manage Jenkins** > **Manage Nodes and Clouds** > **New Node**.
2. Choose **Permanent Agent**, give it a name, and select **Launch agent via SSH**.
3. Fill out the details (e.g., remote SSH credentials, remote root directory).
4. On the **agent machine**, ensure Jenkins can connect via SSH and that the required tools (like Java) are installed.
5. Save the configuration, and the agent should appear as **online** on the Jenkins master.

---

### **Conclusion**:

Jenkins agents are an essential part of Jenkins' **distributed build architecture**, enabling you to offload build tasks, scale your CI/CD pipelines, and provide flexibility in the environment where your jobs run. By using agents, you can maintain a high-performance Jenkins system even with complex or large-scale applications.
