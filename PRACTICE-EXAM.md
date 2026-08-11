# 📝 अभ्यास परीक्षा — ६५ प्रश्न

> **निर्देशन:** ९० मिनेटको टाइमर लगाउनुहोस्। सबै प्रश्नको उत्तर दिनुहोस् (खाली छोड्नु = गलत)। उत्तरहरू फाइलको अन्त्यमा छन् — पहिले नहेर्नुहोस्!
>
> **उत्तीर्ण लक्ष्य: ६५ मध्ये ४६+ (७०%)**

---

## प्रश्नहरू

**१.** क्लाउड कम्प्युटिङका ६ फाइदामध्ये कुन हो?
A) Capital expense लाई operational expense मा बदल्ने
B) सबै सेवा निःशुल्क हुने
C) Data center आफैं चलाउने
D) Hardware पहिल्यै किन्ने

---

**२.** कुन AWS सेवा **global** हो?
A) Amazon EC2
B) Amazon Route 53
C) Amazon RDS
D) AWS Lambda

---

**३.** Shared Responsibility Model अनुसार EC2 instance मा OS को security patch लगाउने जिम्मा कसको हो?
A) AWS को
B) ग्राहकको
C) दुवैको
D) तेस्रो पक्षको

---

**४.** एउटा कम्पनीले batch data processing चलाउँछ जुन बीचमा रोकिए फेरि सुरु गर्न सकिन्छ। सबैभन्दा सस्तो EC2 विकल्प?
A) On-Demand
B) Reserved Instance
C) Spot Instance
D) Dedicated Host

---

**५.** IAM Policy मा `Version` को मान के हुन्छ?
A) 1.0
B) 2012-10-17
C) चालु मिति
D) प्रयोगकर्ताले छान्ने

---

**६.** EC2 instance ले S3 पहुँच गर्नुपर्‍यो। उत्तम अभ्यास कुन हो?
A) Access key EC2 भित्र राख्ने
B) IAM Role assign गर्ने
C) Root credential प्रयोग गर्ने
D) Bucket public बनाउने

---

**७.** एउटा region मा न्यूनतम कति Availability Zone हुन्छन्?
A) १
B) २
C) ३
D) ६

---

**८.** प्रयोगकर्ताले वेब एप्लिकेसनमा जोडिन खोज्दा **timeout** आयो। सम्भावित कारण?
A) एप चलेकै छैन
B) Security Group को नियम
C) IAM अनुमति
D) DNS को समस्या

---

**९.** ७ वर्षसम्म राख्नुपर्ने, कहिल्यै जस्तो पहुँच नहुने, र चाहिँदा १२ घण्टा पर्खन सकिने डाटाको लागि सबैभन्दा सस्तो S3 storage class?
A) S3 Standard-IA
B) S3 One Zone-IA
C) S3 Glacier Flexible Retrieval
D) S3 Glacier Deep Archive

---

**१०.** S3 Cross-Region Replication को लागि के अनिवार्य छ?
A) दुवै bucket एउटै account मा हुनु
B) दुवै bucket मा Versioning सक्षम हुनु
C) Bucket public हुनु
D) उही storage class हुनु

---

**११.** S3 को durability कति हो?
A) 99.9%
B) 99.99%
C) 99.999999999%
D) Storage class अनुसार फरक

---

**१२.** EBS volume बारे कुन सही हो?
A) एकैचोटि धेरै EC2 मा जोड्न मिल्छ
B) एउटा AZ मा बाँधिएको हुन्छ
C) Instance stop भए डाटा हराउँछ
D) Windows मा मात्र चल्छ

---

**१३.** १०० वटा Linux EC2 ले एउटै file system साझा गर्नुपर्‍यो। कुन सेवा?
A) EBS
B) EFS
C) EC2 Instance Store
D) FSx for Windows

---

**१४.** Static IP address चाहिने load balancer कुन हो?
A) Application Load Balancer
B) Network Load Balancer
C) Gateway Load Balancer
D) Classic Load Balancer

---

**१५.** Machine Learning ले भविष्यको traffic अनुमान गरेर पहिल्यै instance तयार पार्ने ASG रणनीति?
A) Simple Scaling
B) Target Tracking
C) Scheduled Scaling
D) Predictive Scaling

---

**१६.** एउटा एप्लिकेसनको read traffic बढ्यो। RDS को read प्रदर्शन सुधार्न?
A) Multi-AZ सक्षम गर्ने
B) Read Replica थप्ने
C) Instance stop गर्ने
D) Backup बढाउने

---

**१७.** S3 मा राखिएको डाटा serverless SQL ले विश्लेषण गर्न कुन सेवा?
A) Amazon Redshift
B) Amazon Athena
C) Amazon RDS
D) Amazon EMR

---

**१८.** Data warehouse (OLAP) को लागि कुन सेवा?
A) Amazon RDS
B) Amazon DynamoDB
C) Amazon Redshift
D) Amazon Neptune

---

**१९.** Social network मा प्रयोगकर्ता, साथी र post बीचको सम्बन्ध राख्न?
A) DynamoDB
B) Neptune
C) Timestream
D) DocumentDB

---

**२०.** DAX र ElastiCache बीचको फरक?
A) DAX केवल DynamoDB को लागि हो
B) ElastiCache मात्र in-memory हो
C) DAX ले relational DB समर्थन गर्छ
D) कुनै फरक छैन

---

**२१.** AWS मा Docker container चलाउनुपर्‍यो तर EC2 instance व्यवस्थापन गर्न चाहँदैन। कुन सेवा?
A) Amazon ECS (EC2 launch type)
B) AWS Fargate
C) Amazon EC2
D) Amazon Lightsail

---

**२२.** Lambda function को अधिकतम execution समय?
A) ५ मिनेट
B) १५ मिनेट
C) ३० मिनेट
D) १ घण्टा

---

**२३.** १५ मिनेट भन्दा लामो चल्ने र जुनसुकै runtime चाहिने compute job को लागि?
A) AWS Lambda
B) AWS Batch
C) Amazon API Gateway
D) AWS Step Functions

---

**२४.** क्लाउडको अनुभव नभएकाले WordPress साइट चलाउन चाहन्छन्, मासिक खर्च अनुमानयोग्य होस्। कुन सेवा?
A) Amazon EC2
B) AWS Elastic Beanstalk
C) Amazon Lightsail
D) AWS Fargate

---

**२५.** Infrastructure as Code को लागि कुन सेवा?
A) AWS CloudFormation
B) AWS Elastic Beanstalk
C) AWS CodeDeploy
D) Amazon EC2

---

**२६.** Developer ले कोड मात्र दिन्छ, बाँकी AWS ले सम्हाल्छ। यो कुन सेवा हो र कुन प्रकारमा पर्छ?
A) EC2 — IaaS
B) Elastic Beanstalk — PaaS
C) Lambda — IaaS
D) S3 — SaaS

---

**२७.** Port 22 नखोली EC2 मा secure shell चाहियो। कुन सेवा?
A) EC2 Instance Connect
B) SSM Session Manager
C) AWS Direct Connect
D) AWS Client VPN

---

**२८.** संसारभरका प्रयोगकर्तालाई image र video छिटो पुर्‍याउन?
A) AWS Global Accelerator
B) Amazon CloudFront
C) AWS Direct Connect
D) Amazon Route 53

---

**२९.** UDP प्रयोग गर्ने gaming एपलाई static IP र छिटो regional failover चाहियो?
A) Amazon CloudFront
B) AWS Global Accelerator
C) Amazon Route 53
D) Application Load Balancer

---

**३०.** डाटा आफ्नै data center मै राख्नुपर्ने compliance छ, तर AWS सेवा प्रयोग गर्न चाहिन्छ?
A) AWS Local Zones
B) AWS WaveLength
C) AWS Outposts
D) Amazon CloudFront

---

**३१.** Route 53 मा traffic को ७०% एक ठाउँ र ३०% अर्को ठाउँ पठाउन?
A) Simple Routing
B) Weighted Routing
C) Latency Routing
D) Failover Routing

---

**३२.** वेब tier र video processing tier लाई decouple गर्न?
A) Amazon SNS
B) Amazon SQS
C) Amazon Kinesis
D) AWS Lambda

---

**३३.** एउटै message एकैचोटि email, SMS र ३ microservice मा पुर्‍याउन?
A) Amazon SQS
B) Amazon SNS
C) Amazon MQ
D) AWS Step Functions

---

**३४.** SQS मा message को पूर्वनिर्धारित retention?
A) १ दिन
B) ४ दिन
C) ७ दिन
D) १४ दिन

---

**३५.** कसैले production S3 bucket मेटायो। कसले मेट्यो पत्ता लगाउन?
A) Amazon CloudWatch
B) AWS CloudTrail
C) AWS X-Ray
D) Amazon Inspector

---

**३६.** CloudWatch ले EC2 को कुन metric पूर्वनिर्धारित रूपमा दिँदैन?
A) CPU Utilization
B) Network In/Out
C) Memory (RAM) Utilization
D) Status Checks

---

**३७.** Microservice आर्किटेक्चरमा कहाँ ढिलो भइरहेको छ पत्ता लगाउन?
A) CloudWatch Logs
B) AWS CloudTrail
C) AWS X-Ray
D) AWS Config

---

**३८.** NACL र Security Group बीचको फरक?
A) NACL instance level मा, SG subnet level मा
B) NACL stateless र DENY rule राख्न मिल्ने, SG stateful र ALLOW मात्र
C) NACL मा अर्को SG सन्दर्भ गर्न मिल्छ
D) SG मा DENY rule हुन्छ

---

**३९.** Private subnet का instance ले इन्टरनेटबाट update डाउनलोड गर्नुपर्‍यो तर बाहिरबाट पहुँच नहोस्। के प्रयोग गर्ने?
A) Internet Gateway
B) NAT Gateway
C) VPC Peering
D) Transit Gateway

---

**४०.** Site-to-Site VPN र Direct Connect बीचको मुख्य फरक?
A) VPN सार्वजनिक इन्टरनेट मार्फत, Direct Connect निजी network मार्फत
B) VPN encrypted हुँदैन
C) Direct Connect तुरुन्तै सेटअप हुन्छ
D) दुवै एउटै हुन्

---

**४१.** SQL injection र XSS आक्रमणबाट वेब एप जोगाउन?
A) AWS Shield
B) AWS WAF
C) Amazon GuardDuty
D) AWS Network Firewall

---

**४२.** S3 मा PII (व्यक्तिगत पहिचान जानकारी) छ कि छैन पत्ता लगाउन?
A) Amazon GuardDuty
B) Amazon Macie
C) Amazon Inspector
D) AWS Config

---

**४३.** Amazon Inspector ले कुन संसाधन scan गर्दैन?
A) EC2 instances
B) ECR मा भएका Container Image
C) Lambda functions
D) S3 buckets

---

**४४.** Encryption key मै पूर्ण नियन्त्रण र FIPS 140-2 Level 3 चाहियो?
A) AWS KMS
B) AWS CloudHSM
C) AWS Secrets Manager
D) SSM Parameter Store

---

**४५.** Auditor ले AWS को SOC र PCI report मागे। कहाँ पाइन्छ?
A) AWS Config
B) AWS Artifact
C) AWS Security Hub
D) AWS Trusted Advisor

---

**४६.** AWS Security Hub प्रयोग गर्नुअघि कुन सेवा सक्षम गर्नैपर्छ?
A) AWS CloudTrail
B) AWS Config
C) Amazon GuardDuty
D) Amazon Detective

---

**४७.** SSL/TLS certificate निःशुल्क provision र auto-renew गर्न?
A) AWS KMS
B) AWS Certificate Manager
C) AWS Secrets Manager
D) AWS CloudHSM

---

**४८.** आवाज रेकर्डिङलाई text मा बदल्न?
A) Amazon Polly
B) Amazon Transcribe
C) Amazon Textract
D) Amazon Comprehend

---

**४९.** Scan गरिएको invoice बाट डाटा निकाल्न?
A) Amazon Rekognition
B) Amazon Textract
C) Amazon Comprehend
D) Amazon Kendra

---

**५०.** ग्राहक इमेलको sentiment (सकारात्मक/नकारात्मक) विश्लेषण गर्न?
A) Amazon Comprehend
B) Amazon Translate
C) Amazon Lex
D) Amazon Polly

---

**५१.** ५ वटा AWS account को एउटै बिल र volume discount साझा गर्न?
A) AWS Control Tower
B) AWS Organizations
C) AWS Budgets
D) AWS Cost Explorer

---

**५२.** Technical Account Manager (TAM) कुन Support Plan मा पाइन्छ?
A) Basic
B) Developer
C) Business
D) Enterprise

---

**५३.** AWS Trusted Advisor ले कुन वर्गमा सिफारिस **दिँदैन**?
A) Cost optimization
B) Security
C) Data migration
D) Fault tolerance

---

**५४.** अर्को ६ महिनाको AWS खर्च forecast गर्न?
A) AWS Pricing Calculator
B) AWS Cost Explorer
C) AWS Budgets
D) CloudWatch Billing Alarm

---

**५५.** ML ले threshold तोक्नु नपरी असामान्य खर्च पत्ता लगाउने सेवा?
A) AWS Budgets
B) AWS Cost Anomaly Detection
C) AWS Trusted Advisor
D) AWS Compute Optimizer

---

**५६.** CloudWatch मा billing metric कुन region मा हुन्छ?
A) सबै region
B) us-east-1
C) eu-west-1
D) ap-south-1

---

**५७.** Mobile एपका १० लाख प्रयोगकर्ताको login व्यवस्थापन गर्न?
A) AWS IAM
B) Amazon Cognito
C) AWS Directory Service
D) AWS STS

---

**५८.** धेरै AWS account र Salesforce मा एउटै लगइनले पहुँच चाहियो?
A) Amazon Cognito
B) AWS IAM Identity Center
C) AWS STS
D) AWS Organizations

---

**५९.** On-premises server AWS मा जस्ताको तस्तै सार्ने रणनीति?
A) Replatform
B) Rehost
C) Refactor
D) Repurchase

---

**६०.** सबैभन्दा सस्तो तर सबैभन्दा ढिलो DR रणनीति?
A) Multi-Site
B) Warm Standby
C) Pilot Light
D) Backup and Restore

---

**६१.** Well-Architected Framework को pillar **होइन** कुन?
A) Operational Excellence
B) Sustainability
C) Scalability
D) Performance Efficiency

---

**६२.** AWS CAF का transformation phase को सही क्रम?
A) Align → Envision → Launch → Scale
B) Envision → Align → Launch → Scale
C) Launch → Align → Envision → Scale
D) Envision → Launch → Align → Scale

---

**६३.** तेस्रो पक्षको software AMI र SaaS कहाँबाट किन्ने?
A) AWS Service Catalog
B) AWS Marketplace
C) AWS re:Post
D) AWS Artifact

---

**६४.** १ PB डाटा AWS मा सार्नुपर्‍यो, इन्टरनेट 100 Mbps छ। के गर्ने?
A) S3 मा सीधै upload
B) AWS Snowball
C) S3 Transfer Acceleration
D) AWS DataSync

---

**६५.** AWS मा निम्नमध्ये कुन-कुन काम **केवल Root user** ले गर्न सक्छ? (दुई छान्नुहोस्)
A) EC2 instance launch गर्ने
B) AWS account बन्द गर्ने
C) S3 bucket बनाउने
D) AWS Support plan बदल्ने
E) IAM user बनाउने

---
---

# ✅ उत्तरहरू

| # | उत्तर | व्याख्या |
|---|---|---|
| **१** | **A** | CAPEX → OPEX। ६ फाइदामध्ये पहिलो |
| **२** | **B** | Global: IAM, Route 53, CloudFront, WAF |
| **३** | **B** | EC2 मा OS patch ग्राहकको जिम्मा ("security IN the cloud")। RDS मा भने AWS को |
| **४** | **C** | Spot — ९०% सम्म छुट, असफलता सहने workload को लागि |
| **५** | **B** | सधैं `2012-10-17` |
| **६** | **B** | IAM Role — कहिल्यै access key instance भित्र नराख्ने |
| **७** | **C** | सामान्यतया ३, न्यूनतम ३, अधिकतम ६ |
| **८** | **B** | Timeout = SG। "Connection refused" भए मात्र app को समस्या |
| **९** | **D** | Glacier Deep Archive — सबैभन्दा सस्तो, १२–४८ घण्टा retrieval |
| **१०** | **B** | स्रोत र गन्तव्य दुवैमा Versioning अनिवार्य |
| **११** | **C** | ११ वटा ९ — **सबै storage class मा उही**। Availability मात्र फरक |
| **१२** | **B** | EBS एउटा AZ मा बाँधिएको। सार्न snapshot चाहिन्छ |
| **१३** | **B** | EFS — सयौं Linux EC2, multi-AZ |
| **१४** | **B** | NLB ले Elastic IP मार्फत static IP दिन्छ |
| **१५** | **D** | Predictive Scaling — ML प्रयोग गर्छ |
| **१६** | **B** | Read Replica = प्रदर्शन। Multi-AZ = उपलब्धता |
| **१७** | **B** | Athena — S3 मा serverless SQL |
| **१८** | **C** | Redshift = OLAP, columnar, MPP |
| **१९** | **B** | Neptune = graph database |
| **२०** | **A** | DAX केवल DynamoDB को cache |
| **२१** | **B** | Fargate = serverless container |
| **२२** | **B** | १५ मिनेट |
| **२३** | **B** | Batch — समय सीमा छैन, Docker मा जुनसुकै runtime |
| **२४** | **C** | Lightsail — नौसिखिया, अनुमानयोग्य कम मूल्य, WordPress template |
| **२५** | **A** | CloudFormation |
| **२६** | **B** | Elastic Beanstalk = PaaS |
| **२७** | **B** | SSM Session Manager — port 22, SSH key, bastion केही नचाहिने। (EC2 Instance Connect मा port 22 चाहिन्छ) |
| **२८** | **B** | CloudFront = CDN, cacheable static सामग्री |
| **२९** | **B** | Global Accelerator — TCP/UDP, Anycast static IP, deterministic failover |
| **३०** | **C** | Outposts — AWS rack आफ्नै data center मा |
| **३१** | **B** | Weighted Routing Policy |
| **३२** | **B** | SQS = queue, decouple |
| **३३** | **B** | SNS = pub/sub, सबै subscriber ले सबै message |
| **३४** | **B** | ४ दिन (अधिकतम १४ दिन) |
| **३५** | **B** | CloudTrail = API call audit |
| **३६** | **C** | RAM metric पूर्वनिर्धारित रूपमा हुँदैन — custom metric चाहिन्छ |
| **३७** | **C** | X-Ray = distributed app मा request trace |
| **३८** | **B** | NACL: subnet, stateless, ALLOW+DENY, IP मात्र। SG: instance, stateful, ALLOW मात्र, SG सन्दर्भ गर्न मिल्ने |
| **३९** | **B** | NAT Gateway — outbound दिन्छ, inbound रोक्छ |
| **४०** | **A** | VPN = इन्टरनेट (encrypted, छिटो सेटअप)। DX = निजी network (छिटो, तर १ महिना लाग्ने) |
| **४१** | **B** | WAF = Layer 7, SQL injection र XSS |
| **४२** | **B** | Macie = S3 मा PII |
| **४३** | **D** | Inspector केवल EC2, ECR Image र Lambda को लागि |
| **४४** | **B** | CloudHSM — key तपाईंले manage, FIPS 140-2 Level 3 |
| **४५** | **B** | AWS Artifact = compliance report portal |
| **४६** | **B** | Security Hub को लागि Config अनिवार्य |
| **४७** | **B** | ACM — public certificate निःशुल्क, auto-renew |
| **४८** | **B** | Transcribe = आवाज → text |
| **४९** | **B** | Textract = कागजात/scan → डाटा |
| **५०** | **A** | Comprehend = NLP, sentiment analysis |
| **५१** | **B** | Organizations — Consolidated Billing |
| **५२** | **D** | TAM = Enterprise Support मा मात्र |
| **५३** | **C** | ६ वर्ग: Cost optimization, Performance, Security, Fault tolerance, Service limits, Operational Excellence |
| **५४** | **B** | Cost Explorer — १२ महिनासम्म forecast। (Pricing Calculator नयाँ architecture को अनुमान) |
| **५५** | **B** | Cost Anomaly Detection — ML, threshold तोक्नु नपर्ने |
| **५६** | **B** | us-east-1 |
| **५७** | **B** | Cognito = एपका बाहिरी प्रयोगकर्ता |
| **५८** | **B** | IAM Identity Center (पहिलेको AWS SSO) |
| **५९** | **B** | Rehost = "lift and shift" |
| **६०** | **D** | Backup and Restore — सबैभन्दा सस्तो, सबैभन्दा ढिलो |
| **६१** | **C** | Scalability pillar होइन। ६ pillar: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, Sustainability |
| **६२** | **B** | Envision → Align → Launch → Scale |
| **६३** | **B** | AWS Marketplace |
| **६४** | **B** | 100 Mbps मा 1 PB = ३ वर्ष! नियम: १ हप्ता भन्दा बढी लाग्छ भने Snowball |
| **६५** | **B, D** | Account बन्द गर्ने र Support plan बदल्ने root-only काम |

---

## 📊 आफ्नो स्कोर मूल्याङ्कन

| सही उत्तर | स्थिति | अर्को कदम |
|---|---|---|
| **५५–६५** | 🌟 उत्कृष्ट | परीक्षा दिन तयार! CHEATSHEET एक पटक दोहोर्‍याउनुहोस् |
| **४६–५४** | ✅ पास हुने तह | गल्ती गरेका विषयका मोड्युल दोहोर्‍याउनुहोस् |
| **३५–४५** | ⚠️ थोरै कमी | कमजोर ३–४ मोड्युल पूरै फेरि पढ्नुहोस् |
| **३५ भन्दा कम** | 📚 थप अध्ययन चाहिन्छ | सबै मोड्युलको "सार" भाग + CHEATSHEET फेरि पढ्नुहोस्, अनि यो test दोहोर्‍याउनुहोस् |

---

## 🎯 कुन विषय कमजोर छ पत्ता लगाउनुहोस्

| प्रश्न नम्बर | विषय | मोड्युल |
|---|---|---|
| १–३, ७ | क्लाउडका आधारभूत कुरा | [०१](01-cloud-computing.md) |
| ५, ६ | IAM | [०२](02-iam.md) |
| ४, ८ | EC2 | [०३](03-ec2.md) |
| १२, १३ | EC2 Storage | [०४](04-ec2-storage.md) |
| १४, १५ | ELB र ASG | [०५](05-elb-asg.md) |
| ९, १०, ११, ६४ | S3 | [०६](06-s3.md) |
| १६–२० | Databases | [०७](07-databases.md) |
| २१–२४ | Compute र Serverless | [०८](08-other-compute.md) |
| २५, २६, २७ | Deployment | [०९](09-deployment.md) |
| २८–३१ | Global Infrastructure | [१०](10-global-infrastructure.md) |
| ३२–३४ | Cloud Integration | [११](11-cloud-integration.md) |
| ३५–३७ | Monitoring | [१२](12-monitoring.md) |
| ३८–४० | VPC | [१३](13-vpc.md) |
| ४१–४७, ६५ | Security | [१४](14-security.md) |
| ४८–५० | Machine Learning | [१५](15-machine-learning.md) |
| ५१–५६ | Billing र Support | [१६](16-billing-support.md) |
| ५७, ५८ | Advanced Identity | [१७](17-advanced-identity.md) |
| ५९, ६० | Migration र DR | [१८](18-other-services.md) |
| ६१–६३ | Well-Architected र Ecosystem | [१९](19-architecting-ecosystem.md) |

---

⬅️ [README](README.md) | 📋 [चिट-सिट](CHEATSHEET.md)
