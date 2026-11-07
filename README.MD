# CST8915 lab9 - Cloud Cost Estimation for Enterprise Application Migration



## 1. Assumptions: 

- **Regions**: There are 3 regions, namely North America, Europe and Asia-Pacific. For the **Web front-end** VMs, we will deploy 10 VMs in each region(not 30 VMs in one region), totaling 30 VMs distributed in 3 regions. For **API** VMs, they shall be distributed in 3 regions based on traffic, with VM Scale Sets auto-scaling up to 2X during peak hours. 
- **Hours**: 730 hours per month for always-on VMs. 
- **VM Configuration**: Front-end VMs are 2 vCPU general purpose, API VMs are 4-8 vCPUs.
- **Traffic**: a guess of peak hours vs off-peak hours. 
- **Data egress**: 



## 2. System Setups: 



| Web Front-End Cluster:                  |                                                              |      |
| --------------------------------------- | ------------------------------------------------------------ | ---- |
| 10 VMs NA, 10 VMs Eu, 10 VMs APAC       | 30 General Purpose VMs distributed in 3 regions              |      |
| Load Balancers/App Gateway              | Azure Front Door as a Global load balancer + <br /> 3 X Azure Application Gateway as Regional load balancers + 3 X Internal Standard load balancers. |      |
| CDN                                     | Azure Front Door(Standard or Premium)                        |      |
| **API Back-End Services:**              |                                                              |      |
| 50 microservices hosted across 20 VMs   | Distributed in 3 regions, with auto scaling up to 2X         |      |
| **Payment Processing:**                 |                                                              |      |
| Dedicate VMs                            | A normal VM is its own VNet, separated from everything else by firewalls and NSG. <br /> 2 VMs in each region for high availability - **6 VMs in total.** <br /> 2 VMs are distributed in different zones within one region for geo-redundancy. |      |
| Key Vault                               | Azure Key Vault is for storage of payment secrets, TLS certificates. |      |
| Azure Firewall                          | One firewall in each region.                                 |      |
| **Database Layer:**                     |                                                              |      |
| Primary 5TB SQL                         | Azure SQL Databases 5TB for transaction data. <br />Primary vCore + 8 TB storage in one region<br />2X Secondary vCore + 8 TB storage in other regions<br /> |      |
| Analytics NoSQL with 10TB               | Cosmos DB 10TB<br />Analytics NoSQL stores customer behavior data, which is huge in **write** volume, flexible in structure. |      |
| Data Warehouse with 15TB                | Synapse DB 15TB stores history and reporting. <br />Only work in Business hours(12 hours*22 workdays = 264 hours) |      |
| **Data Analytics and ML:**              |                                                              |      |
| GPU VMs for ML                          | For a recommendation model, a mid-range option is NC A100 v4 series. Use Azure Machine Learning compute clusters, which scales to zero nodes when no training job is running. <br />One region only, sitting close to Data Warehouse. <br />4 X NC A100 v4,  200 h/month. |      |
| Scheduled batch jobs                    | Run at night or off-peak and shut down VMs afterwards.<br />Nightly 4-hour jobs. |      |
| **Backup and Disaster Recovery:**       |                                                              |      |
| Redundant storage for disaster recovery |                                                              |      |
| Geo-redundant storage in 3 regions      |                                                              |      |
| **Management and Monitoring Tools:**    |                                                              |      |
|                                         |                                                              |      |
|                                         |                                                              |      |
|                                         |                                                              |      |
|                                         |                                                              |      |
|                                         |                                                              |      |
|                                         |                                                              |      |







