# What is a Cloud Service Provider?

A cloud service provider, or CSP, is a company that offers some component of cloud computing; typically when you search the internet a cloud service is defined as, infrastructure as a service (IaaS), software as a service (SaaS) or platform as a service (PaaS) to other businesses or individuals.  Microsoft Azure, AWS and Google Cloud, Oracle Cloud, IBM Cloud ... 

A cloud data service is a remote version of a data center. It is more secure and less expensive than managing a data center. A **region** is a set of data centers deployed within a latency-defined perimeter and connected through a dedicated regional low-latency network. Azure is generally available in 52 regions around the world, with plans announced for 6 additional regions


<p align="center">
  <img  src="https://github.com/okansungur/drafts/blob/main/iot_images/geo.png"><br/>
  Geography
</p>

**Geography** is a discrete market, typically containing two or more regions, that preserves data residency and compliance boundaries.

<p align="center">
  <img  src="https://github.com/okansungur/drafts/blob/main/iot_images/regionazure.png"><br/>
  Regions
</p>

Each  **region** features datacenters deployed within a latency-defined perimeter. They're connected through a dedicated regional low-latency network. This design ensures that  services within any region offer the best possible performance and security.



**Availability Zones** is a high-availability offering that protects your applications and data from datacenter failures.

**Region pairs** with another region within the same geography, together making a regional pair.  a Region Pair have direct connections that bring additional benefits to use them together. And a pair is always located greater than 300 miles apart 


<p align="center">
  <img  src="https://github.com/okansungur/drafts/blob/main/iot_images/region1.png"><br/>
  Region Pairs
</p>

**Availability Set** is a logical grouping capability for isolating VM resources from each other when they’re deployed

<p align="center">
  <img  src="https://github.com/okansungur/drafts/blob/main/iot_images/azurearchitect.png"><br/>
  Azure Architect
</p>


A refined more adequate definition would be *“A Cloud Service is any system that provides on-demand availability of computer system resources, e.g; data storage and computing power, without direct active management by the user”*. While this may seem a bit broad that is because it should be. Cloud services come in many forms and sizes even to the point where it may not be exactly clear to the average user, if their vendor or supplier should technically be classified as a cloud service provider or not.
Let’s look at a simple use case example:
An X company has built their own private cloud. They wanted to take advantage of benefits of cloud computing like

-	Rapid and simple deployment
-	Less time to market for services
-	Cost efficiency
-	More utilization of server resources
-	Less capital and operational costs
-	This is managed by X company Cloud datacenter services
-	Better perceived security by managing and controlling it internally


However, what if due to some natural disaster or a fire accident they lose their datacenter? 
They can’t afford to lose their data. They wanted a Disaster recovery solution, which would simply replicate all their data and services somewhere else. So, they outsourced services in a public cloud using AWS or GCP infrastructure so now they have the best of both worlds.

*How do you define your supplier as a cloud service or not?*

The basic concept behind the cloud is that the location of the service, and associated processes and assets such as the hardware and operating system(s) and/or applications on which it is running, are largely immaterial to the user. They may have a separate business unit that is a private cloud that is dedicated to serving the entire internal organization, they may use a 3rd party service like AWS, GCP or Azure and in some cases may use both. In any event they are servicing you from the cloud and you should expect that they have cloud specific controls like the CSA Cloud Control Matrix (CCM) to address the applicable scope of service and to mitigate the associated risks.

