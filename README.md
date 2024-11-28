# LetsDefend Lab: SOC168 - OS Command Injection Investigation

## **Objective**  
The objective of this lab is to detect and respond to an **OS Command Injection attack** by:  
1. Analyzing logs to identify malicious requests and payloads.  
2. Assessing the attack’s success and determining its impact.  
3. Implementing containment measures and documenting IOCs for future prevention.


### **Skills Learned**  
- Analyzing HTTP POST requests and identifying OS Command Injection payloads.  
- Validating IP reputation using threat intelligence tools.  
- Containing compromised systems and documenting Indicators of Compromise (IOCs).  
- Applying incident response techniques to mitigate risks.  


### **Tools Used**  
- **Log Management Systems** for analyzing suspicious traffic.  
- **Threat Intelligence Platforms** (e.g., VirusTotal, AbuseIPDB) for validating IP reputation.  
- **HTTP and Network Monitoring Tools** for tracing malicious requests and actions.

## Steps
### Intro to Alert

Lets grab the alert details

	EventID: 118
	Event Time: Feb, 28, 2022, 04:12 AM
	Rule: SOC168 - Whoami Command Detected in Request Body
	Level: Security Analyst
	Hostname: Webserver1004
	Destination IP Address: 172.16.17.16
	Source IP Address: 61.177.172.87
	HTTP Request Method: POST
	Requested URL: https://172.16.17.16/video/
	User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1)
	Alert Trigger Reason: Request Body Contains whoami string
	Device Action: Allowed


![Image 1](https://i.imgur.com/6J4kdgf.png)

### Case Creation

Create Case

Start Playbook

### Understand the Alert


![Image 2](https://i.imgur.com/ConHLOz.png)

	Q) Why was it triggered?

Check Alert details for this

	A) Request body contains whoami string

	Q) Which two devices is the traffic occuring? What protocol is used between devices, etc?

Lets look at the source and destination IPs

For the source IP we'll use VirusTotal

Looks like its a malicious actor coming from China

Community notes also confirms this

![Image 3](https://i.imgur.com/rXowlnd.png)

![Image 4](https://i.imgur.com/riMVgRP.png)

Now lets look at our log files

Here's the query in question, although the threat actor made several other attempts with various payloads as well

![Image 5](https://i.imgur.com/teekg2e.png)

![Image 6](https://i.imgur.com/z0GF0xQ.png)

From the alert details, we know that the destination IP address is our WebServer1004

So for the answer to the prompt is:

	A) Threat actor located in China (61.177.172.87) -> Webserver1004 (172.16.17.16), over port 443 (HTTPS), POST request method 

### Collect Data

![Image 7](https://i.imgur.com/BErmHqU.png)

We've already done most of this, lets see what we stil need

We can review VirusTotal for this

	Q) Ownership of IP addresses and Devices
	A) ChinaNet Jiangsu Province Network. 

	Q) Coming from internet
	A) Yes

	Q) Static or pooled?
	A) Pooled

	Q) Reputation?
	A) Malicious

### Examine HTTP Traffic

![Image 8](https://i.imgur.com/B84XURN.png)

We briefly looked at this in the logs earlier

![Image 9](https://i.imgur.com/Uxj8NdL.png)

	Q) Malicious?
	A) Yes

![Image 10](https://i.imgur.com/449uRgm.png)

	Q) What type of attack?
	A) Command Injection

![Image 11](https://i.imgur.com/15ZTw54.png)

	Q) Is it a planned test?

Since we already determined the traffic is malicious and coming from outside our network we know it is not planned

	A) Not planned

![Image 12](https://i.imgur.com/6SXzwyk.png)

	Q) Direction of Traffic?
	A) Internet -> Company network

![Image 13](https://i.imgur.com/qVNld30.png)

	Q) Was the attack successful? 

Lets take a look at the endpoint to determine wether or not the commands were executed

![Image 14](https://i.imgur.com/aVbtCXU.png)

Unfortunately, it looks like the attack was successful

![Image 15](https://i.imgur.com/ZgJzgji.png)

	A) Yes 

### Containment

![Image 16](https://i.imgur.com/nxN3hV1.png)

Contain the machine

![Image 17](https://i.imgur.com/V5M2Q8o.png)

Add Artifacts

![Image 18](https://i.imgur.com/LCSYl3H.png)

![Image 19](https://i.imgur.com/utStp4r.png)

	Q) Tier 2 Escalation?

Since the attack was successful, yes

	A) Yes

Create you analyst note

![Image 20](https://i.imgur.com/tKLB1aU.png)

Finish playbook

Close alert

![Image 21](https://i.imgur.com/AQuoXlB.png)

	Q) Postive or negative?
	A) True positive

### Scorecard

Looks like we got a perfect score! 

![GIF](https://media.giphy.com/media/AoHEeIi9AzzwLlEmfb/giphy.gif?cid=ecf05e47nkrtn87ykxp9r58a19xz8kakaizh5j4iq24vtz1x&ep=v1_gifs_search&rid=giphy.gif&ct=g)


![Image 22](https://i.imgur.com/4m7E8ah.png)

### Conclusion

In conclusion, we successfully solved the alert and identified it as a true positive. We contained the machine to further mitigate our risk and we escalated the issue to Tier 2 support.

#### Feedback

Well that was fun, if you have any feedback feel free to comment or reach out to me via Linkedin! Cheers, and safe hacking. 
