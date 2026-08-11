# मोड्युल १८ — अन्य AWS सेवाहरू, Migration र Disaster Recovery

> ℹ️ **यी सेवा कहिलेकाहीँ मात्र परीक्षामा आउँछन्, तर 7R migration strategy र DR strategy भने राम्ररी जान्नुहोस् — ती धेरै सोधिन्छन्।**

---

## भाग १ — Desktop र App सेवाहरू

## १. Amazon WorkSpaces ⭐

**Managed Desktop as a Service (DaaS)** — Windows वा Linux desktop सजिलै provision गर्ने।

- ⭐ **On-premise VDI (Virtual Desktop Infrastructure) को व्यवस्थापन हटाउन उत्तम**
- हजारौं प्रयोगकर्तासम्म छिटो र सजिलै scale
- **सुरक्षित डाटा — KMS सँग एकीकृत**
- **Pay-as-you-go** सेवा — मासिक वा घण्टाको दर

---

## २. Amazon AppStream 2.0 ⭐

**Desktop Application Streaming Service**

- Infrastructure किन्नु/provision गर्नु नपरी **जुनसुकै कम्प्युटरमा** पुर्‍याउने
- ⭐ **एप्लिकेसन वेब ब्राउजरभित्रैबाट डेलिभर हुन्छ**

### AppStream 2.0 vs WorkSpaces ⭐⭐

| | **WorkSpaces** | **AppStream 2.0** |
|---|---|---|
| के दिन्छ | **पूर्ण managed VDI र desktop** | **एउटा desktop एप्लिकेसन** वेब ब्राउजरमा stream |
| प्रयोगकर्ताले | VDI मा जोडिन्छ, त्यहाँ native वा WAM एप खोल्छ | **VDI मा जोडिनु पर्दैन** — ब्राउजर भए पुग्यो |
| उपलब्धता | On-demand वा always on | **वेब ब्राउजर भएको जुनसुकै यन्त्रमा** |
| Configuration | — | **प्रति एप्लिकेसन प्रकार instance type (CPU, RAM, GPU) मिलाउन मिल्ने** |

> **सम्झने: WorkSpaces = पूरै desktop। AppStream = एउटा एप मात्र, ब्राउजरमा।**

---

## भाग २ — Developer र IoT सेवाहरू

## ३. AWS IoT Core ⭐

**IoT = "Internet of Things"** — इन्टरनेटसँग जोडिएका यन्त्रहरूको सञ्जाल जसले डाटा सङ्कलन र स्थानान्तरण गर्न सक्छन्।

- **IoT यन्त्रलाई AWS Cloud सँग सजिलै जोड्ने**
- **Serverless, सुरक्षित र अर्बौं यन्त्र र खर्बौं message सम्म scalable**
- ⭐ **यन्त्र जोडिएको नहुँदा पनि एप्लिकेसनले तिनसँग सञ्चार गर्न सक्ने**
- धेरै AWS सेवासँग एकीकृत (Lambda, S3, SageMaker आदि)
- डाटा जम्मा गर्ने, प्रशोधन गर्ने, विश्लेषण गर्ने र कदम चाल्ने IoT एप बनाउने

---

## ४. AWS AppSync

- **Mobile र web एपमा real-time मा डाटा राख्ने र sync गर्ने**
- ⭐ **GraphQL** प्रयोग गर्छ (Facebook को mobile प्रविधि)
- Client Code स्वचालित रूपमा बन्न सक्ने
- **DynamoDB / Lambda** सँग एकीकरण
- **Real-time subscription**
- **Offline data synchronization** (Cognito Sync को ठाउँ लिन्छ)
- सूक्ष्म-तहको सुरक्षा
- **AWS Amplify ले पछाडि AppSync प्रयोग गर्न सक्छ!**

---

## ५. AWS Amplify ⭐

- **Scalable full stack web र mobile एप्लिकेसन बनाउन र deploy गर्न सहयोग गर्ने उपकरण र सेवाको सेट**
- समावेश: **Authentication, Storage, API (REST, GraphQL), CI/CD, PubSub, Analytics, AI/ML Predictions, Monitoring**, AWS/GitHub बाट Source Code

---

## ६. AWS Infrastructure Composer

- **AWS मा serverless एप्लिकेसन दृश्य रूपमा डिजाइन र निर्माण गर्ने**
- ⭐ **AWS को विज्ञ नभई पनि AWS infrastructure code deploy गर्ने**
- संसाधनहरू एकआपसमा कसरी अन्तरक्रिया गर्ने configure गर्ने
- ⭐ **CloudFormation प्रयोग गरेर Infrastructure as Code (IaC) बनाउने**
- अवस्थित CloudFormation / SAM template import गरेर देख्ने क्षमता

---

## ७. AWS Device Farm ⭐

- **Web र mobile एपलाई desktop browser, वास्तविक mobile यन्त्र र tablet मा परीक्षण गर्ने पूर्ण managed सेवा**
- ⭐ **धेरै यन्त्रमा एकैचोटि test चलाउने** (छिटो हुने)
- यन्त्रको सेटिङ configure गर्न सकिने (GPS, भाषा, Wi-Fi, Bluetooth…)
- ⭐ **वास्तविक यन्त्र — emulator होइन!**
- रिपोर्ट, log, screenshot दिन्छ

---

## भाग ३ — Backup र Disaster Recovery

## ८. AWS Backup ⭐

**AWS सेवाहरूभर backup केन्द्रीय रूपमा व्यवस्थापन र स्वचालित गर्ने पूर्ण managed सेवा।**

- **On-demand र तालिकाबद्ध backup**
- ⭐ **PITR (Point-in-time Recovery)** समर्थन
- Retention Period, Lifecycle Management, Backup Policies
- ⭐ **Cross-Region Backup**
- ⭐ **Cross-Account Backup** (AWS Organizations प्रयोग गरेर)

**समर्थित संसाधन:** Amazon EC2, EBS, DynamoDB, RDS, EFS, Aurora, FSx, Storage Gateway, S3

---

## ९. Disaster Recovery रणनीतिहरू ⭐⭐

**यो परीक्षामा धेरै सोधिन्छ। लागत र RTO (recovery समय) को अनुसार क्रममा राख्नुहोस्:**

| रणनीति | विवरण | लागत | Recovery समय |
|---|---|---|---|
| **१. Backup and Restore** | डाटा S3 मा backup मात्र। प्रकोप आएपछि सबै बनाउने | **सबैभन्दा कम** 💰 | **सबैभन्दा ढिलो** 🐢 |
| **२. Pilot Light** | एपका **मुख्य function** मात्र चलिरहेको (जस्तै database)। Scale गर्न तयार, तर न्यूनतम सेटअप | कम 💰💰 | ढिलो 🐢🐢 |
| **३. Warm Standby** | एपको **पूरा संस्करण, तर न्यूनतम आकारमा** चलिरहेको | बढी 💰💰💰 | छिटो 🐇🐇 |
| **४. Multi-Site / Hot-Site** | एपको **पूरा संस्करण, पूरा आकारमा** चलिरहेको | **सबैभन्दा बढी** 💰💰💰💰 | **सबैभन्दा छिटो** 🐇🐇🐇 |

```
कम लागत ←────────────────────────────────────────────→ बढी लागत
ढिलो recovery ←──────────────────────────────────→ छिटो recovery

Backup & Restore → Pilot Light → Warm Standby → Multi-Site/Hot-Site
```

### सामान्य DR सेटअप
```
N. Virginia (us-east-1)          London (eu-west-2)
AZ-A: EC2  AZ-B: EC2   ──failover──>   AZ-A: EC2  AZ-B: EC2
```

---

## १०. AWS Elastic Disaster Recovery (DRS) ⭐

*(पहिलेको नाम: **CloudEndure Disaster Recovery**)*

- **भौतिक, virtual र cloud-आधारित server लाई छिटो र सजिलै AWS मा recover गर्ने**
- उदाहरण: सबैभन्दा महत्त्वपूर्ण database (Oracle, MySQL, SQL Server), enterprise app (SAP) जोगाउने, **ransomware आक्रमणबाट डाटा जोगाउने**
- ⭐ **Server को निरन्तर block-level replication**

```
Corporate DC / अरू cloud (Disks: OS, Apps, DB)
    ↓ AWS Replication Agent — निरन्तर replication (सेकेन्डमा)
Staging (सस्तो EC2 instance र EBS volume)
    ↓ failover (मिनेटमा)
Production (लक्षित EC2 instance र EBS volume)
    ↑ failback
```

---

## ११. AWS DataSync ⭐

- **On-premises बाट AWS मा ठूलो मात्राको डाटा सार्ने**
- Sync गर्न सकिने ठाउँ: **Amazon S3 (Glacier सहित जुनसुकै storage class), Amazon EFS, Amazon FSx for Windows**
- ⭐ **Replication task घण्टा, दैनिक वा साप्ताहिक तालिकामा राख्न सकिने**
- ⭐ **पहिलो पूर्ण load पछि replication task incremental हुन्छ**

> ⭐ **Snowball vs DataSync को फरक:**
> - **Snowball** = **भौतिक यन्त्र** मार्फत (नेटवर्क सुस्त वा नभएको बेला)
> - **DataSync** = **नेटवर्क मार्फत** (TLS encrypted), तालिकाबद्ध र incremental

---

## भाग ४ — Cloud Migration Strategies

## १२. The 7Rs — क्लाउड Migration रणनीति ⭐⭐

**यी ७ वटा परीक्षामा पक्कै सोधिन्छन्। हरेकको उदाहरणसहित याद गर्नुहोस्।**

### १️⃣ Retire (सेवानिवृत्त गर्ने)
- **नचाहिने कुरा बन्द गर्ने** (Re-architecting को परिणामस्वरूप हुन सक्छ)
- **आक्रमणको क्षेत्र घटाउन** (बढी सुरक्षा) सहयोगी
- **१०% देखि २०% सम्म लागत बचत**
- मर्मत गर्नैपर्ने संसाधनमा ध्यान केन्द्रित

### २️⃣ Retain (यथावत् राख्ने)
- **अहिलेलाई केही नगर्ने** (यो पनि Cloud Migration मा एउटा निर्णय हो!)
- कारण: सुरक्षा, डाटा compliance, प्रदर्शन, समाधान नभएका निर्भरता
- Migrate गर्दा **व्यावसायिक मूल्य नहुने**, mainframe वा mid-range र non-x86 Unix एप

### ३️⃣ Relocate (सार्ने)
- **एपलाई on-premises बाट यसकै Cloud संस्करणमा** सार्ने
- EC2 instance लाई फरक VPC, AWS account वा AWS Region मा सार्ने
- **उदाहरण:** VMware SDDC बाट **VMware Cloud on AWS** मा server सार्ने

### ४️⃣ Rehost — "lift and shift" ⭐
- AWS मा **फेरि host गरेर सरल migration** (एप्लिकेसन, database, डाटा…)
- मेसिन (भौतिक, virtual, अर्को Cloud) AWS Cloud मा migrate गर्ने
- ⚠️ **कुनै cloud optimization गरिँदैन — एप जस्ताको तस्तै migrate हुन्छ**
- **३०% सम्म लागत बचत**
- **उदाहरण:** **AWS Application Migration Service** प्रयोग गरेर migrate गर्ने

### ५️⃣ Replatform — "lift and reshape" ⭐
- **उदाहरण:** database लाई **RDS** मा migrate गर्ने
- **उदाहरण:** एप्लिकेसन **Elastic Beanstalk** मा migrate गर्ने
- ⭐ **मुख्य आर्किटेक्चर नबदल्ने, तर केही cloud optimization प्रयोग गर्ने**
- Fully managed सेवा वा Serverless मा गएर समय र पैसा बचाउने

### ६️⃣ Repurchase — "drop and shop" ⭐
- Cloud मा जाँदा **फरक उत्पादनमा सर्ने**
- प्रायः **SaaS प्लेटफर्म** मा जाने
- ⚠️ **छोटो अवधिमा महँगो, तर deploy छिटो**
- **उदाहरण:** CRM → Salesforce.com, HR → Workday, CMS → Drupal

### ७️⃣ Refactor / Re-architect ⭐
- **Cloud Native सुविधा प्रयोग गरेर एप्लिकेसनको आर्किटेक्चर पुनर्कल्पना गर्ने**
- सुविधा थप्न र scalability, प्रदर्शन, सुरक्षा र agility सुधार्ने व्यावसायिक आवश्यकताले चालित
- **Monolithic एप्लिकेसनबाट micro-services मा जाने**
- **उदाहरण:** एपलाई Serverless architecture मा सार्ने, AWS S3 प्रयोग गर्ने

### 🎯 7Rs को छिटो सम्झना

| R | एक शब्दमा | उदाहरण |
|---|---|---|
| **Retire** | बन्द गर्ने | नचाहिने server off |
| **Retain** | छोड्ने | अहिलेलाई on-prem मै |
| **Relocate** | सार्ने | VMware → VMware Cloud on AWS |
| **Rehost** | **lift and shift** | Server → EC2 (जस्ताको तस्तै) |
| **Replatform** | **lift and reshape** | DB → RDS, App → Beanstalk |
| **Repurchase** | **drop and shop** | CRM → Salesforce (SaaS) |
| **Refactor** | पुनर्निर्माण | Monolith → Serverless microservices |

---

## १३. Migration का सहायक सेवाहरू ⭐

### AWS Application Discovery Service
- **On-premises data center को जानकारी जम्मा गरेर migration project योजना बनाउने**
- Server को प्रयोग डाटा र **dependency mapping** migration को लागि महत्त्वपूर्ण
- **Agentless Discovery** (AWS Agentless Discovery Connector) — VM inventory, configuration, र CPU/memory/disk जस्ता प्रदर्शन इतिहास
- **Agent-based Discovery** (AWS Application Discovery Agent) — सिस्टम configuration, प्रदर्शन, चलिरहेका process, सिस्टम बीचको network कनेक्सनको विवरण
- ⭐ **नतिजा AWS Migration Hub मा हेर्न सकिने**

### AWS Application Migration Service (MGN) ⭐
- CloudEndure Migration को **"AWS विकास"** — AWS Server Migration Service (SMS) को ठाउँ लिने
- ⭐ **Lift-and-shift (rehost) समाधान** — AWS मा एप migration सजिलो बनाउने
- भौतिक, virtual र cloud-आधारित server लाई **AWS मा native रूपमा चल्ने बनाउने**
- धेरै प्लेटफर्म, OS र database समर्थन
- **न्यूनतम downtime, घटेको लागत**

### AWS Migration Evaluator
- **AWS मा migration को लागि डाटामा आधारित business case बनाउन सहयोग**
- तपाईंको संस्थाले अहिले के चलाइरहेको छ भन्ने **स्पष्ट baseline** दिने
- **Agentless Collector** install गरेर व्यापक discovery
- On-premises footprint र server निर्भरताको snapshot लिने
- वर्तमान अवस्था विश्लेषण → लक्षित अवस्था परिभाषित → migration plan विकास

### AWS Migration Hub ⭐
- ⭐ **AWS मा migration को assessment, planning र tracking को लागि server र application inventory डाटा जम्मा गर्ने केन्द्रीय स्थान**
- Migration छिटो बनाउने, lift-and-shift स्वचालित गर्ने
- **AWS Migration Hub Orchestrator** — enterprise app (जस्तै SAP, Microsoft SQL Server) migrate गर्न पूर्व-निर्मित template
- ⭐ **Application Migration Service (MGN) र Database Migration Service (DMS) बाट migration स्थिति update समर्थन**

---

## भाग ५ — अन्य सेवाहरू

## १४. AWS Fault Injection Simulator (FIS) ⭐

- **AWS workload मा fault injection experiment चलाउने पूर्ण managed सेवा**
- ⭐ **Chaos Engineering** मा आधारित — एप्लिकेसनमा जानाजान बाधा (जस्तै CPU वा memory अचानक बढाउने) सिर्जना गरेर सिस्टमले कस्तो प्रतिक्रिया दिन्छ हेर्ने र सुधार लागू गर्ने
- **लुकेका bug र प्रदर्शनका bottleneck** पत्ता लगाउन सहयोग
- समर्थित सेवा: **EC2, ECS, EKS, RDS**
- चाहिएको बाधा उत्पन्न गर्न **पूर्व-निर्मित template** प्रयोग गर्ने

> **परीक्षा टिप:** प्रश्नमा **"Chaos Engineering"** आयो → **AWS FIS**

---

## १५. AWS Step Functions ⭐

- **Lambda function orchestrate गर्न serverless दृश्य workflow बनाउने**
- **सुविधा:** sequence, parallel, condition, timeout, error handling…
- EC2, ECS, On-premises server, API Gateway, SQS queue आदिसँग एकीकृत गर्न सकिने
- ⭐ **Human approval सुविधा लागू गर्ने सम्भावना**
- **प्रयोग:** order fulfillment, data processing, वेब एप्लिकेसन, जुनसुकै workflow

---

## १६. AWS Ground Station

- **उपग्रह सञ्चार नियन्त्रण गर्ने, डाटा प्रशोधन गर्ने र satellite operation scale गर्ने पूर्ण managed सेवा**
- AWS region नजिकै **satellite ground station को global network**
- ⭐ **सेकेन्डमै satellite डाटा तपाईंको AWS VPC मा डाउनलोड** गर्न दिने
- Satellite डाटा **S3 वा EC2 instance** मा पठाउने
- **प्रयोग:** मौसम पूर्वानुमान, surface imaging, सञ्चार, video broadcast

---

## १७. Amazon Pinpoint ⭐

- **Scalable २-तर्फी (outbound/inbound) marketing communications सेवा**
- समर्थन: **email, SMS, push, voice, र in-app messaging**
- ग्राहकलाई सही सामग्री सहित message **segment र personalize** गर्ने क्षमता
- **जवाफ पाउने सम्भावना**
- **प्रति दिन अर्बौं message** सम्म scale

**प्रयोग:** marketing, bulk र transactional SMS message पठाएर campaign चलाउने

### ⭐ Pinpoint vs SNS/SES

| | **SNS र SES** | **Amazon Pinpoint** |
|---|---|---|
| तपाईंले के manage गर्नुपर्छ | **हरेक message को audience, सामग्री र delivery schedule** | **Message template, delivery schedule, अति-लक्षित segment, र पूरै campaign** बनाउने |

> **सम्झने:** SNS/SES = साधारण notification। Pinpoint = पूरै marketing campaign।

---

## 📝 अभ्यास प्रश्नहरू

**प्रश्न १.** एउटा कम्पनीले on-premises server AWS मा जस्ताको तस्तै सार्न चाहन्छ, कुनै परिवर्तन नगरी। यो कुन migration रणनीति हो?

A) Replatform
B) Rehost
C) Refactor
D) Repurchase

<details><summary>उत्तर</summary>

**B) Rehost** — "lift and shift", कुनै cloud optimization नगरी।
</details>

---

**प्रश्न २.** एउटा कम्पनीले आफ्नो MySQL database RDS मा सार्‍यो र एप Elastic Beanstalk मा राख्यो, तर मुख्य आर्किटेक्चर बदलेन। यो कुन रणनीति हो?

A) Rehost
B) Replatform
C) Refactor
D) Retain

<details><summary>उत्तर</summary>

**B) Replatform** — "lift and reshape", केही cloud optimization तर आर्किटेक्चर उही।
</details>

---

**प्रश्न ३.** सबैभन्दा सस्तो तर सबैभन्दा ढिलो recovery दिने DR रणनीति कुन हो?

A) Multi-Site
B) Warm Standby
C) Pilot Light
D) Backup and Restore

<details><summary>उत्तर</summary>

**D) Backup and Restore** — सबैभन्दा सस्तो, सबैभन्दा ढिलो।
</details>

---

**प्रश्न ४.** प्रयोगकर्ताले वेब ब्राउजरबाट एउटा desktop एप्लिकेसन चलाउन चाहन्छन्, पूरै desktop चाहिँदैन। कुन सेवा?

A) Amazon WorkSpaces
B) Amazon AppStream 2.0
C) Amazon EC2
D) AWS Device Farm

<details><summary>उत्तर</summary>

**B) Amazon AppStream 2.0** — एप्लिकेसन streaming, ब्राउजरमै।
</details>

---

**प्रश्न ५.** Mobile एपलाई वास्तविक यन्त्रहरूमा (emulator होइन) परीक्षण गर्न कुन सेवा?

A) AWS Fault Injection Simulator
B) AWS Device Farm
C) Amazon AppStream 2.0
D) AWS Amplify

<details><summary>उत्तर</summary>

**B) AWS Device Farm** — वास्तविक mobile यन्त्र र tablet मा परीक्षण।
</details>

---

**प्रश्न ६.** धेरै AWS सेवा (EC2, RDS, DynamoDB, EFS) को backup केन्द्रीय रूपमा व्यवस्थापन गर्न कुन सेवा?

A) Amazon S3
B) AWS Backup
C) AWS DataSync
D) AWS Elastic Disaster Recovery

<details><summary>उत्तर</summary>

**B) AWS Backup** — धेरै AWS सेवाको backup केन्द्रीय रूपमा, cross-region र cross-account सहित।
</details>

---

**प्रश्न ७.** On-premises बाट S3 मा हरेक दिन डाटा sync गर्नुपर्‍यो, नेटवर्क राम्रो छ। कुन सेवा?

A) AWS Snowball
B) AWS DataSync
C) AWS Storage Gateway
D) AWS Migration Hub

<details><summary>उत्तर</summary>

**B) AWS DataSync** — नेटवर्क मार्फत, तालिकाबद्ध (दैनिक), incremental replication।
</details>

---

**प्रश्न ८.** एउटा टिमले जानाजान CPU spike सिर्जना गरेर सिस्टमको resilience परीक्षण गर्न चाहन्छ। कुन सेवा?

A) AWS Device Farm
B) AWS Fault Injection Simulator
C) Amazon CloudWatch
D) AWS X-Ray

<details><summary>उत्तर</summary>

**B) AWS Fault Injection Simulator (FIS)** — Chaos Engineering मा आधारित।
</details>

---

**प्रश्न ९.** धेरै Lambda function लाई क्रम, condition र error handling सहितको workflow मा जोड्न कुन सेवा?

A) Amazon SQS
B) AWS Step Functions
C) Amazon EventBridge
D) AWS Batch

<details><summary>उत्तर</summary>

**B) AWS Step Functions** — serverless visual workflow orchestration।
</details>

---

## ✅ यो मोड्युलको सार

| सेवा | एक-लाइन काम |
|---|---|
| **WorkSpaces** | Managed Desktop as a Service (पूरै desktop) |
| **AppStream 2.0** | ब्राउजरमै एप्लिकेसन streaming |
| **IoT Core** | IoT यन्त्रलाई AWS सँग जोड्ने |
| **AppSync** | GraphQL ले mobile/web मा real-time डाटा sync |
| **Amplify** | Full stack web र mobile एप बनाउने उपकरणको सेट |
| **Infrastructure Composer** | Serverless एप दृश्य रूपमा डिजाइन गर्ने (CloudFormation बनाउँछ) |
| **Device Farm** | वास्तविक यन्त्रमा web/mobile एप परीक्षण |
| **AWS Backup** | धेरै सेवाको backup केन्द्रीय व्यवस्थापन |
| **Elastic Disaster Recovery (DRS)** | Server को निरन्तर replication र AWS मा recovery |
| **DataSync** | नेटवर्क मार्फत on-prem बाट AWS मा डाटा sync |
| **Application Discovery Service** | On-prem inventory र dependency mapping |
| **Application Migration Service (MGN)** | Lift-and-shift migration |
| **Migration Evaluator** | Migration को business case बनाउने |
| **Migration Hub** | Migration को केन्द्रीय planning र tracking |
| **Fault Injection Simulator (FIS)** | Chaos Engineering — resilience परीक्षण |
| **Step Functions** | Serverless workflow orchestration |
| **Ground Station** | Satellite सञ्चार व्यवस्थापन |
| **Pinpoint** | Marketing campaign (email, SMS, push) |

### DR रणनीति (सस्तो/ढिलो → महँगो/छिटो)
**Backup and Restore → Pilot Light → Warm Standby → Multi-Site/Hot-Site**

### 7Rs
**Retire, Retain, Relocate, Rehost, Replatform, Repurchase, Refactor**

---

⬅️ [मोड्युल १७](17-advanced-identity.md) | ➡️ [मोड्युल १९ — Well-Architected](19-architecting-ecosystem.md)
