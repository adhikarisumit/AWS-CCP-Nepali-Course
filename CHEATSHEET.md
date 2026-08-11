# 📋 AWS Cloud Practitioner — अन्तिम रिभिजन चिट-सिट

> **परीक्षा भन्दा १–२ दिन अघि यो मात्र पढ्नुहोस्। सबै मोड्युलका मुख्य कुरा एकै ठाउँमा।**

---

## १. क्लाउडका आधारभूत कुरा

**६ फाइदा:** CAPEX→OPEX | Economies of scale | क्षमताको अनुमान नगर्ने | Agility बढ्ने | Data center खर्च रोक्ने | मिनेटमै global

**५ विशेषता:** On-demand self service | Broad network access | Multi-tenancy र resource pooling | Rapid elasticity | Measured service

**३ Deployment Model:** Private | Public | **Hybrid** (केही on-prem, केही cloud)

**३ प्रकार:**
| | उदाहरण |
|---|---|
| **IaaS** | EC2, GCP, Digital Ocean |
| **PaaS** | Elastic Beanstalk, Heroku |
| **SaaS** | Rekognition, Gmail, Dropbox, Zoom |

**मूल्यका ३ आधार:** Compute | Storage | **Data transfer OUT** (⚠️ **IN निःशुल्क**)

**शब्दहरूको फरक:**
- **Scalability** = ठूलो लोड सम्हाल्ने क्षमता (up वा out)
- **Elasticity** = आफैं scale हुनु (auto-scaling)
- **Agility** = ⚠️ **distractor!** नयाँ संसाधन एक क्लिकमै

---

## २. Global Infrastructure

```
Region (us-east-1) → Availability Zone (३–६ वटा) → Data Center
Edge Location (४००+ PoP, ९०+ शहर, ४०+ देश)
```

**Region छान्ने ४ आधार:** Compliance | Proximity (latency) | उपलब्ध सेवा | Pricing

**⭐ Global सेवा (४ वटा):** **IAM, Route 53, CloudFront, WAF**

**Shared Responsibility:**
- **AWS = security OF the cloud** (hardware, data center, network)
- **तपाईं = security IN the cloud** (डाटा, OS patch, IAM, firewall)

---

## ३. IAM

| कुरा | मुख्य तथ्य |
|---|---|
| **User** | एक व्यक्ति = एक user |
| **Group** | user मात्र, **nested group हुँदैन** |
| **Policy** | JSON, `Version` सधैं **2012-10-17** |
| **Role** | AWS सेवा (EC2, Lambda) लाई अनुमति |
| **MFA** | पासवर्ड + यन्त्र। Root मा अनिवार्य |
| **Access Key** | CLI/SDK को लागि। ⚠️ **कहिल्यै EC2 भित्र नराख्ने — Role प्रयोग गर्ने** |
| **Credentials Report** | **Account-level** |
| **Access Advisor** | **User-level** — कुन सेवा कहिले प्रयोग भयो |

**मूल सिद्धान्त:** **Least Privilege**

---

## ४. EC2

**७ Purchasing Options:**

| विकल्प | छुट | कहिले |
|---|---|---|
| **On-Demand** | ० | छोटो, अनुमान गर्न नसकिने |
| **Reserved (१/३ वर्ष)** | **७२%** | स्थिर, दीर्घकालीन (database) |
| **Convertible Reserved** | ६६% | दीर्घकालीन + लचिलो instance |
| **Savings Plans** | ७२% | $/घण्टा प्रतिबद्धता |
| **Spot** | **९०%** | बीचमा रोकिए हुने (batch, analysis) |
| **Dedicated Host** | सबैभन्दा महँगो | **BYOL, licensing, compliance** |
| **Dedicated Instance** | — | hardware नबाँड्ने |
| **Capacity Reservation** | ० | निश्चित AZ मा क्षमता |

**Security Group:**
- ⚠️ **Timeout = Security Group को समस्या**
- ⚠️ **Connection refused = Application को समस्या**
- **Inbound पूर्वनिर्धारित बन्द, Outbound पूर्वनिर्धारित खुला**
- ALLOW मात्र, अर्को SG सन्दर्भ गर्न मिल्ने

**Ports:** 22=SSH | 21=FTP | 80=HTTP | 443=HTTPS | 3389=RDP(Windows)

**User Data:** पहिलो boot मा **मात्र एक पटक**, **root** को रूपमा

---

## ५. Storage

| सेवा | मुख्य तथ्य |
|---|---|
| **EBS** | Network drive, **एक पटकमा १ EC2**, **AZ मा बाँधिएको**। सार्न snapshot चाहिन्छ |
| **EBS Snapshot** | Backup। AZ/Region मा copy गर्न मिल्ने। Archive = ७५% सस्तो (२४–७२ घण्टा restore) |
| **Instance Store** | **Ephemeral!** उच्च IOPS, तर stop भए **डाटा हराउँछ** |
| **EFS** | **सयौं EC2**, **Multi-AZ**, **Linux मात्र**, महँगो (३x) |
| **EFS-IA** | ९२% सस्तो, lifecycle policy ले सार्ने |
| **FSx for Windows** | Windows SMB/NTFS, Active Directory |
| **FSx for Lustre** | **HPC, Linux + cluster** |
| **AMI** | Region-specific, copy गर्न मिल्ने |

**वर्गीकरण:** BLOCK = EBS, Instance Store | FILE = EFS | OBJECT = S3, Glacier

---

## ६. ELB र ASG

| LB | Layer | प्रोटोकल | विशेषता |
|---|---|---|---|
| **ALB** | **7** | HTTP/HTTPS/gRPC | HTTP routing, **Static DNS** |
| **NLB** | **4** | TCP/UDP | अति उच्च प्रदर्शन, **Static IP** |
| **GWLB** | **3** | GENEVE | Firewall, intrusion detection |
| Classic | 4 र 7 | — | **२०२३ मा retire** |

**ASG Scaling:** Manual | Simple/Step (alarm) | Target Tracking (लक्ष्य ४०% CPU) | **Scheduled** (ज्ञात समय) | **Predictive (ML)**

**High Availability = कम्तीमा २ AZ**

---

## ७. Amazon S3

**Bucket:** नाम **विश्वव्यापी रूपमा अद्वितीय**, region मा बन्ने। ठूला अक्षर र `_` हुँदैन
**Object:** **५ GB भन्दा ठूलो → multi-part upload अनिवार्य**। Tag बढीमा **१०**। अधिकतम object आकार: स्लाइडमा **५० TB** लेखिएको छ (AWS को आधिकारिक सीमा **५ TB**)

**७ Storage Class:**

| Class | Availability | न्यूनतम अवधि | Retrieval |
|---|---|---|---|
| **Standard** | 99.99% | — | तुरुन्तै |
| **Intelligent-Tiering** | 99.9% | — | **कुनै retrieval fee छैन** |
| **Standard-IA** | 99.9% | ३० दिन | तुरुन्तै |
| **One Zone-IA** | **99.5%** | ३० दिन | तुरुन्तै (**१ AZ मात्र**) |
| **Glacier Instant** | 99.9% | ९० दिन | **मिलिसेकेन्ड** |
| **Glacier Flexible** | 99.99% | ९० दिन | १–५ मिनेट / ३–५ घण्टा / ५–१२ घण्टा |
| **Glacier Deep Archive** | 99.99% | **१८० दिन** | **१२ घण्टा / ४८ घण्टा** ← सबैभन्दा सस्तो |

⭐ **Durability सबैमा 99.999999999% (११ ९) — Availability मात्र फरक**

**पहुँच कसरी दिने:**
| परिस्थिति | समाधान |
|---|---|
| सार्वजनिक | **Bucket Policy** |
| उही account को IAM user | **IAM Policy** |
| EC2 | **IAM Role** |
| **अर्को AWS account** | **Bucket Policy** (cross-account) |

**Versioning:** bucket-level, गल्तीले मेटिनबाट बचाउने। Suspend गर्दा पुराना संस्करण मेटिँदैनन्
**Replication (CRR/SRR):** ⚠️ **दुवैतिर Versioning अनिवार्य**, asynchronous
**403 Forbidden** = bucket policy मा public read छैन

**Snowball:** नेटवर्कबाट **१ हप्ता भन्दा बढी** लाग्छ भने Snowball। Edge computing (EC2/Lambda जहाजमा/ट्रकमा)
**Storage Gateway:** on-prem ↔ S3 को hybrid पुल

---

## ८. Databases

| आवश्यकता | सेवा |
|---|---|
| Relational SQL (OLTP) | **RDS** (PostgreSQL, MySQL, MariaDB, Oracle, SQL Server, DB2, Aurora) |
| ५x MySQL, ३x Postgres, auto-grow २५६ TB | **Aurora** |
| Capacity planning नचाहिने, अनियमित load | **Aurora Serverless** |
| पढ्ने प्रदर्शन (१५ सम्म) | **Read Replica** |
| AZ failover, High Availability | **Multi-AZ** (१ मात्र) |
| Region-level DR | **Multi-Region** |
| In-memory cache | **ElastiCache** (Redis, Memcached) |
| Key/Value NoSQL, serverless | **DynamoDB** (३ AZ, single-digit ms) |
| DynamoDB को cache | **DAX** (microsecond) ⚠️ DynamoDB मात्र |
| DynamoDB multi-region active-active | **Global Tables** |
| Data warehouse (OLAP), columnar | **Redshift** |
| Hadoop / Spark cluster | **EMR** |
| **S3 मा serverless SQL** | **Athena** ($5/TB scan) |
| Dashboard / BI (serverless) | **QuickSight** |
| MongoDB को AWS संस्करण | **DocumentDB** |
| **Graph / social network** | **Neptune** |
| **Time series / IoT** | **Timestream** |
| Hyperledger, Ethereum | **Managed Blockchain** |
| **ETL + Data Catalog** | **Glue** (serverless) |
| **Database migration** | **DMS** (स्रोत DB चलिरहन्छ) |

⚠️ **RDS मा SSH गर्न मिल्दैन। OS patching AWS को जिम्मा।**

---

## ९. Compute र Serverless

| सेवा | मुख्य तथ्य |
|---|---|
| **ECS** | Docker on EC2 — **तपाईंले EC2 manage गर्ने** |
| **Fargate** | Docker **serverless — EC2 छैन** |
| **ECR** | निजी Docker registry |
| **EKS** | **Kubernetes** (cloud-agnostic) |
| **Lambda** | Serverless function, **अधिकतम १५ मिनेट**, **१० GB RAM सम्म**। Billing = duration × RAM + invocations। Free: १० लाख request + ४ लाख GB-sec |
| **API Gateway** | Lambda लाई **HTTP API** बनाउने। REST + WebSocket |
| **Batch** | **समय सीमा छैन**, **जुनसुकै runtime** (Docker), EC2/Spot मा |
| **Lightsail** | **नौसिखियाको लागि**, कम र **अनुमानयोग्य मूल्य**, auto-scaling छैन |
| **Step Functions** | Serverless workflow orchestration, human approval |

**Serverless सेवाहरू:** Lambda, Fargate, S3, DynamoDB, Athena, Aurora Serverless, API Gateway, SQS/SNS, Glue, QuickSight

---

## १०. Deployment

| सेवा | प्रकार | काम |
|---|---|---|
| **CloudFormation** | AWS only | **Infrastructure as Code** (YAML/JSON), region/account मा दोहोर्‍याउने |
| **CDK** | AWS only | प्रोग्रामिङ भाषाले IaC → CloudFormation मा compile |
| **Elastic Beanstalk** | AWS only | **PaaS** — कोड मात्र देऊ। **निःशुल्क** (instance को मात्र पैसा) |
| **CodeDeploy** | **Hybrid** | EC2 र on-prem मा deploy (agent चाहिन्छ) |
| **Systems Manager (SSM)** | **Hybrid** | Patch, command, Parameter Store। ⚠️ **SSM agent चाहिन्छ** |
| **SSM Session Manager** | | **Port 22, SSH key, bastion केही नचाहिने** shell |
| **SSM Parameter Store** | | Config र secret, KMS encryption |
| CodeCommit / CodeBuild / CodePipeline / CodeArtifact | | Git / build / **CI-CD orchestration** / dependency |

---

## ११. Global Applications

| सेवा | काम |
|---|---|
| **Route 53** | Managed DNS। Records: A (IPv4), AAAA (IPv6), CNAME (host→host), Alias (AWS संसाधन) |
| **Routing Policies** | Simple (health check छैन) \| **Weighted** (%) \| **Latency** \| **Failover** (DR) |
| **CloudFront** | CDN, **cache गर्छ**, DDoS सुरक्षा। Origin: S3 (OAC), VPC, Custom HTTP |
| **Global Accelerator** | **cache गर्दैन**, AWS network मा proxy, **Anycast static IP**, TCP/UDP, छिटो failover |
| **S3 Transfer Acceleration** | Edge मार्फत S3 upload/download छिटो |
| **Outposts** | AWS rack **तपाईंकै data center मा**। ⚠️ **भौतिक सुरक्षा तपाईंको जिम्मा** |
| **WaveLength** | **5G** network को edge |
| **Local Zones** | Region को विस्तार, प्रयोगकर्ताको नजिक |

**CloudFront vs CRR:** CloudFront = static, cache (TTL)। CRR = dynamic, near real-time, read-only

---

## १२. Integration

| सेवा | मोडेल | मुख्य तथ्य |
|---|---|---|
| **SQS** | **Queue** | Decouple। Retention **४ दिन (अधिकतम १४)**। Consumer ले काम बाँड्ने र पढेपछि मेट्ने। **FIFO** = क्रम कायम |
| **SNS** | **Pub/Sub** | **सबै subscriber ले सबै message**। ⚠️ **Retention छैन**। १.२५ करोड subscription/topic |
| **Kinesis** | Streaming | **Real-time big data** |
| **Amazon MQ** | Broker | **ActiveMQ, RabbitMQ** — MQTT, AMQP, STOMP। ⚠️ कम scale, server मा चल्ने |

---

## १३. Monitoring

| सेवा | प्रश्न जसको उत्तर हो |
|---|---|
| **CloudWatch** | "**प्रदर्शन** कस्तो छ?" — metrics, logs, alarms |
| **CloudTrail** | "**कसले के गर्‍यो?**" — API call audit। **पूर्वनिर्धारित रूपमा सक्षम** |
| **Config** | "**Configuration कहिले बदलियो?**" — compliance रेकर्ड, **per-region** |
| **X-Ray** | "**Distributed app** मा कहाँ ढिलो भयो?" — request trace |
| **EventBridge** | Schedule (cron) + Event pattern → Lambda/SNS/SQS |
| **Health Dashboard (Service)** | सबै AWS सेवाको सामान्य अवस्था |
| **Health Dashboard (Account)** | **मेरा संसाधनमा** कस्तो असर |

⚠️ **CloudWatch ले EC2 को RAM metric दिँदैन** (custom metric चाहिन्छ)
⚠️ **Billing metric केवल `us-east-1` मा**
⚠️ **EC2 log CloudWatch मा पठाउन agent चाहिन्छ**
**EC2 metric:** पूर्वनिर्धारित ५ मिनेट, Detailed Monitoring १ मिनेट ($$$)
**Alarm states:** OK, INSUFFICIENT_DATA, ALARM

---

## १४. VPC

| शब्द | तथ्य |
|---|---|
| **VPC** | **Region-level** |
| **Subnet** | **AZ-level** |
| **Internet Gateway** | VPC लाई इन्टरनेट |
| **NAT Gateway** | Private subnet लाई outbound इन्टरनेट |
| **NACL** | **Subnet**, **Stateless**, ALLOW+**DENY**, IP मात्र |
| **Security Group** | **Instance**, **Stateful**, **ALLOW मात्र**, SG सन्दर्भ गर्न मिल्ने |
| **VPC Peering** | ⚠️ **Transitive होइन**, CIDR overlap हुनु हुँदैन |
| **VPC Endpoint** | **Gateway = S3, DynamoDB**। Interface = अरू |
| **PrivateLink** | तेस्रो पक्षको VPC सँग (NLB + ENI चाहिन्छ) |
| **Flow Logs** | S3, CloudWatch Logs, Firehose मा जाने |
| **Site-to-Site VPN** | इन्टरनेट मार्फत, encrypted, **CGW + VGW**, छिटो सेटअप |
| **Direct Connect** | निजी, छिटो, ⚠️ **१ महिना लाग्ने** |
| **Client VPN** | OpenVPN, एउटा कम्प्युटरबाट |
| **Transit Gateway** | **हजारौं VPC**, hub-and-spoke, transitive |
| **Elastic IP** | स्थिर public IPv4, ⚠️ **नप्रयोग भए पनि खर्च** ($0.005/घण्टा) |

---

## १५. Security

| सेवा | एक-लाइन |
|---|---|
| **Shield Standard** | निःशुल्क DDoS (Layer 3/4) |
| **Shield Advanced** | **$3,000/महिना**, २४/७ DRP टिम, spike को शुल्कबाट सुरक्षा |
| **WAF** | **Layer 7** — SQL injection, XSS, geo-block, rate-based। **ALB, API Gateway, CloudFront** मा |
| **Network Firewall** | पूरै VPC, **Layer 3–7** |
| **Firewall Manager** | **Organization भर** security rule |
| **KMS** | AWS ले key manage। CloudTrail Logs, S3 Glacier, Storage Gateway मा **auto encrypted** |
| **CloudHSM** | **Hardware, key तपाईंले manage**, FIPS 140-2 Level 3 |
| **ACM** | SSL/TLS cert — **public निःशुल्क, auto-renew**। ELB, CloudFront, API Gateway |
| **Secrets Manager** | Secret + **auto rotation** (Lambda), **RDS एकीकरण** |
| **Artifact** | **Compliance report** — ISO, PCI, SOC, HIPAA/BAA |
| **GuardDuty** | **ML threat detection** — CloudTrail, VPC Flow, DNS Logs। एक क्लिकमा |
| **Inspector** | ⚠️ **केवल EC2, ECR Image, Lambda** — CVE vulnerability |
| **Config** | Configuration + compliance रेकर्ड |
| **Macie** | **S3 मा PII / संवेदनशील डाटा** |
| **Security Hub** | धेरै account का finding। ⚠️ **Config चाहिन्छ** |
| **Detective** | **मूल कारण** (ML + graph) |
| **IAM Access Analyzer** | **बाहिर साझा भएका संसाधन** पहिचान |

**Pen testing:** ८ सेवामा अनुमतिबिना ✅ | ⛔ **DoS/DDoS/flooding निषेधित**

**Root-only कामहरू:** Account setting बदल्ने | **Account बन्द गर्ने** | **Support plan बदल्ने** | RI Marketplace मा बिक्रेता दर्ता | IAM अनुमति पुनर्स्थापना | S3 मा MFA Delete | GovCloud दर्ता

---

## १६. Machine Learning

| सेवा | एक-लाइन |
|---|---|
| **Rekognition** | 📷 तस्बिर/भिडियो — अनुहार, label, content moderation |
| **Transcribe** | 🎤 **आवाज → text** (subtitle) |
| **Polly** | 🦜 **Text → आवाज** |
| **Translate** | 🌐 अनुवाद |
| **Lex** | 💬 Chatbot (Alexa को प्रविधि) |
| **Connect** | ☎️ क्लाउड contact center (८०% सस्तो) |
| **Comprehend** | 🧠 NLP, sentiment |
| **SageMaker** | 🔧 **आफ्नै ML model** बनाउने |
| **Kendra** | 🔍 कागजातभित्र ML search |
| **Personalize** | 🛒 Real-time सिफारिस |
| **Textract** | 📄 **कागजात/scan → डाटा** (OCR) |

---

## १७. Billing र Support

**Organizations:** Consolidated billing, volume discount, RI pooling। **SCP** = OU/Account मा IAM action सीमित (⚠️ **Master account मा लागू हुँदैन**, स्पष्ट Allow चाहिन्छ)
**Control Tower:** Organizations माथि — guardrail सहित multi-account सेटअप
**RAM:** संसाधन साझा (VPC Subnet, Transit Gateway…)
**Service Catalog:** Admin ले तयार गरेको stack मात्र user ले launch गर्ने

**४ Pricing Model:** Pay as you go | Save when you reserve | Pay less by using more | Pay less as AWS grows

**Savings Plans:**
- **EC2 Savings Plan** — ७२%, instance family + region मा बाँधिने
- **Compute Savings Plan** — ६६%, **EC2 + Fargate + Lambda**, जे भए पनि

**Billing उपकरण:**
| उपकरण | काम |
|---|---|
| **Pricing Calculator** | **अनुमान** (नयाँ architecture) |
| **Cost Explorer** | विश्लेषण + **१२ महिनाको forecast** |
| **Cost and Usage Report** | **सबैभन्दा व्यापक** dataset (Athena/Redshift/QuickSight) |
| **Cost Allocation Tags** | `aws:` = AWS generated, `user:` = user-defined |
| **Billing Alarms** | **us-east-1**, **actual cost मात्र** |
| **Budgets** | Usage/Cost/Reservation/Savings Plans, **५ SNS सम्म** |
| **Cost Anomaly Detection** | **ML**, threshold तोक्नु नपर्ने |
| **Compute Optimizer** | ML ले right-size सिफारिस, **२५% सम्म बचत** |
| **Service Quotas** | Quota सीमा नजिक हुँदा alert |

**Trusted Advisor — ६ वर्ग:** Cost optimization | Performance | Security | Fault tolerance | Service limits | Operational Excellence
⚠️ **Full checks + API = Business र Enterprise मा मात्र** (Basic मा ७ core check)

**Support Plans:**
| Plan | मुख्य |
|---|---|
| **Basic** | निःशुल्क, ७ core Trusted Advisor check |
| **Developer** | General guidance < २४ घण्टा, system impaired < १२ घण्टा |
| **Business** | २४/७ फोन+chat, **Full Trusted Advisor + API**, prod down **< १ घण्टा**, तेस्रो पक्ष OS support |
| **Enterprise** | ⭐ **TAM**, business-critical down **< १५ मिनेट**, Security Incident Response, Countdown |

---

## १८. Advanced Identity

| सेवा | एक-लाइन |
|---|---|
| **IAM** | कम्पनीका user |
| **STS** | **अस्थायी credential** |
| **Cognito** | **एपका बाहिरी user** (लाखौं), social login |
| **Managed Microsoft AD** | AWS मै AD + on-prem सँग **trust** |
| **AD Connector** | **Proxy** — user on-prem मै |
| **Simple AD** | एक्लै, on-prem सँग जोडिन्न |
| **IAM Identity Center** | **SSO** — धेरै account + Salesforce/M365 |

---

## १९. Migration र DR

**7Rs:** **Retire** (बन्द) | **Retain** (छोड्ने) | **Relocate** (VMware→VMware on AWS) | **Rehost** (lift & shift, MGN, ३०% बचत) | **Replatform** (lift & reshape — RDS, Beanstalk) | **Repurchase** (drop & shop — SaaS) | **Refactor** (microservices, serverless)

**DR (सस्तो/ढिलो → महँगो/छिटो):**
**Backup & Restore → Pilot Light → Warm Standby → Multi-Site/Hot-Site**

**Migration उपकरण:** Application Discovery Service (inventory) | **MGN** (lift & shift) | Migration Evaluator (business case) | **Migration Hub** (केन्द्रीय tracking) | **DMS** (database) | **DataSync** (नेटवर्क, तालिकाबद्ध) | **Snowball** (भौतिक यन्त्र) | **Elastic DR / DRS** (निरन्तर replication)

---

## २०. Well-Architected र CAF

**Well-Architected ६ Pillar:** Operational Excellence | Security | Reliability | Performance Efficiency | Cost Optimization | **Sustainability**
⚠️ **यी trade-off होइनन् — synergy हुन्**

**Design Principles:** Scalability | Disposable Resources | Automation | **Loose Coupling** | **Services, not Servers**

**AWS CAF ६ Perspective:**
- व्यावसायिक: **Business, People (प्रविधि↔व्यवसायको पुल), Governance**
- प्राविधिक: **Platform, Security, Operations**

**CAF ४ Phase:** **Envision → Align → Launch → Scale**

**CAF ४ Domain:** Technology, Process, Organization, Product

**Right Sizing:** Cloud migration अघि र onboarding पछि **निरन्तर**। सानोबाट सुरु गर्नुहोस्

**Ecosystem:** Well-Architected Tool (निःशुल्क) | Carbon Footprint Tool | **Marketplace** (तेस्रो पक्ष, bill मै आउने) | **re:Post** (Q&A) | APN (Technology/Consulting/Training Partners) | **AMS** (AWS ले infra चलाइदिने, २४/३६५)

---

## 🔥 परीक्षामा सबैभन्दा धेरै झुक्याउने १५ कुरा

1. **Availability फरक-फरक, Durability सधैं ११ ९** (S3)
2. **CloudWatch ले EC2 को RAM metric दिँदैन**
3. **Billing metric = us-east-1 मात्र**
4. **Timeout = SG, Connection refused = App**
5. **Agility ≠ Scalability** (distractor हो)
6. **SCP master account मा लागू हुँदैन**
7. **SNS मा message retention छैन**
8. **VPC Peering transitive होइन**
9. **Data transfer IN निःशुल्क, OUT मा पैसा**
10. **RDS मा SSH गर्न मिल्दैन**
11. **Inspector = EC2 + ECR Image + Lambda मात्र** (S3, RDS होइन)
12. **Security Hub को लागि Config चाहिन्छ**
13. **TAM = Enterprise Support मात्र**
14. **DAX = DynamoDB मात्र, ElastiCache = अरू DB सँग पनि**
15. **Lambda ले जथाभावी Docker image चलाउँदैन** (त्यसको लागि ECS/Fargate)

---

## ⏱️ परीक्षाको तथ्य

- **६५ प्रश्न, ९० मिनेट** (~१ मिनेट २० सेकेन्ड प्रति प्रश्न)
- **७००/१०००** मा पास
- **खाली नछोड्नुहोस् — गलत उत्तरमा दण्ड छैन**
- गाह्रो प्रश्न **flag** गरेर अगाडि बढ्नुहोस्
- जटिल देखिने समाधान सायद गलत हो

---

**शुभकामना! 🚀**

⬅️ [README](README.md) | 📝 [अभ्यास परीक्षा](PRACTICE-EXAM.md)
