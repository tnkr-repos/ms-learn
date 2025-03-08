# DEVOPS FOUNDATIONS: THE CORE PRINCIPLES AND PRACTICES

## DISCOVER DEVOPS

## PLAN WITH DEVOPS

## DEVELOP WITH DEVOPS

## DELIVER WITH DEVOPS

## OPERATE WITH DEVOPS

### INTRODUCTION

### EXPLORE OPERATIONAL EXCELLENCE

### EXPLORE SHIFT-RIGHT TESTING

### EXPLORE OBSERVABILITY THROUGH PERFORMANCE MONITORING

- 

### EXPLORE RESILIENCE WITH SITE RELIABILITY ENGINEERING

- DevOps shares its goals with Site Reliability Engineering (SRE) in that their primary objective is to increase reliability and efficiency of software delivery through the integration between development and operations practices and through collaboration between development and operations teams. By comibining them, the organizations that develop and maintain software solutions will be able to increase their resiliency and availability
- SRE promotes the application of software engineering practices to operations tasks. These practices guide you through the process of building, deploying, and operating reliable, scalable, and highly efficient software systems. What distinguishes SRE from DevOps is its focus on reliability engineering. The engineering part of this term is meant to convey the use of specific, well-defined practices to accomplish its goals
- SRE relies on explicit definition and measurement of reliability through quantifiable Service Level Objectives (SLOs) that determine the acceptable level of service characteristics, such as availability, latency, or error rates. Service Level Indicators (SLIs) are used to measure different aspects of the service's behavior that affect reliability. Service Level Agreements (SLAs) reference SLIs to define the acceptable performance. The SLAs serve as the foundation for SLOs. SRE also introduces the concept of error budgets, which represents the acceptable degree of service degradation within a specified timeframe. The intention behind this concept is to optimize, rather than maximize reliability. This is important for two reasons:
	- Increased reliability translates into more effort and a higher cost.
	- High pace of changes tends to be disruptive and, as a result, might negatively affect reliability.
Using error budgets facilitates finding a balance between reliability, cost, and the pace of changes.
- The initial steps of your implementation should include defining SLOs, establishing SLIs and SLAs, and setting up error budgets. The remaining steps include:
	- Applying automation throughout software delivery process (although the focus of SRE is on automation of operational tasks, such as scaling, monitoring, and incident response).
	- Setting up comprehensive monitoring and observability processes.
	- Planning proactively for capacity needs.
	- Promoting continuous learning and continuous improvement through regular post-incident reviews and root cause analysis. Regularly review SLOs, error budgets, and operational practices.
	- Integrating SRE practices with development processes.

### IMPROVE DEVELOPER EXPERIENCE WITH PLATFORM ENGINEERING

- Platform engineering aims to enhance the developer experience by providing self-service capabilities, user-friendly environment, and ensuring security and compliance. By adopting this practice, development teams can improve their efficiency, reduce time-to-market, and deliver high-quality software
	- Self-service capabilities - Providing developers with the necessary tools and resources to streamline their development process. By implementing self-service capabilities developers can access the required infrastructure, services, and environments without relying on external teams or waiting for manual provisioning, thus improving efficiency and reducing time spent on nondevelopment tasks
	- User-friendly environment - By automating repetitive tasks so that developers can focus on coding and delivering value to business
	- Security and Compliance - Implementing a secure and governed framework so that developers can adhere to best practices and regulatory requirements without hindering their productivity (such as access controls, monitoring, and auditing to maintain a secure development environment)

### ENHANCE WORKLOAD RESILIENCY TRAFFIC MANAGER AND AZURE CHAOS STUDIO

- If facing frequent downtime and performance issues with an application:
	- Enhance workload using `Traffic Manager` (by creating resilient configurations that implements load balancing)
	- Test resiliency using `Azure Chaos Studio` (by triggering failures and testing load balancing capabilities for example)
- PENDING