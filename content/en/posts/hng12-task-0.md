+++
title = 'Hng12 Task 0'
date = 2025-02-01T00:38:08+01:00
author = 'c0d33ngr'
toc = true
draft = false
+++

# **Effortless Web Server Deployment: Setting Up Nginx on AWS EC2**

As someone with experience in DevOps and infrastructure basics, tasks like setting up **Nginx** on an **AWS EC2** instance feel second nature. However, revisiting such foundational exercises offers an opportunity to refine workflows, optimize configurations, and ensure smooth deployments. Here’s a quick recap of how I approached this task and its contribution to my ongoing growth.

## **My Approach to the Task**

1. **Provisioning the EC2 Instance:**  
   Launching the EC2 instance was straightforward. I chose **Ubuntu Linux**, a required choice for the task setups, and ensured the necessary security group rules were configured (allowing **HTTP** on port 80 and **SSH** on port 22).

2. **Update and Upgrade the Ubuntu server:**

   Immediately i ssh into the server, I ran the command below to update and upgrade the Ubuntu server
   ```
   sudo apt update -y && sudo apt upgrade -y
   ```

3. **Installing Nginx:**  
   The command I ran to install `nginx` web server:
   ```bash
   sudo apt install nginx -y
   ```
   I started and enabled the service to make Nginx operational.
   ```
   sudo systemctl start nginx
   sudo systemctl enable nginx
   ```
   
   After that, I checked the status to see it's status
   ```
   sudo systemctl status nginx
   ```

4. **Customizing the Default HTML Page:**  
   I edited the default page under `/usr/share/nginx/html/index.html` to serve custom content. With the appropriate permissions, this step was quick and seamless:
   ```bash
   sudo nano /usr/share/nginx/html/index.html
   ```

5. **Testing and Verification:**  
   Accessing the instance’s public IP confirmed the setup was successful and serving the updated content.

## **Reflections on the Process**

Given my familiarity with these tasks, there were no obstacles during the setup. However, this exercise wasn’t just about completing a routine task—it was about reaffirming efficient practices and ensuring readiness for production-level deployments. 

## **How This Task Aligns With My Goals**

Although fundamental, this task highlights my emphasis on operational excellence:
- **Automation Opportunities:**  
   Revisiting manual setups often inspires ideas for automation. For instance, scripting this process with tools like Terraform or Ansible could eliminate manual effort in larger-scale deployments.
- **Foundation for Advanced Use Cases:**  
   Customizing Nginx configurations or integrating it with load balancers and reverse proxies often starts with a basic setup. This task reinforced the importance of getting the groundwork right.
- **Focus on Reliability:**  
   Even the simplest tasks are a reminder that reliable infrastructure starts with attention to detail.

While setting up Nginx might feel like a basic task, its mastery contributes to streamlining workflows and enabling more advanced deployments.

## **Key Takeaways**

1. **Streamlining DevOps Workflows:**  
   Routine tasks are an excellent opportunity to identify processes worth automating or optimizing.
2. **Reinforcing Expertise:**  
   Revisiting the basics ensures preparedness for production-level challenges where foundational skills are critical.
3. **Scalable Deployments:**  
   Understanding every aspect of deployment, even for simple tasks, is vital when scaling to more complex environments.

This task wasn’t about learning something new—it was a chance to reinforce and refine skills I already possess. It’s a reminder that even basic tasks, when approached thoughtfully, play a critical role in maintaining a strong foundation for advanced projects.

## References

- _DevOps Engineers - https://hng.tech/hire/devops-engineers_
- _Cloud Engineers - https://hng.tech/hire/cloud-engineers_
- _Site Reliability Engineers - https://hng.tech/hire/site-reliability-engineers_
- _Platform Engineers - https://hng.tech/hire/platform-engineers_
