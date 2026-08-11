# मोड्युल १२ — Cloud Monitoring (CloudWatch, CloudTrail, X-Ray, Health Dashboard)

> ⭐ **परीक्षामा धेरै महत्त्वपूर्ण — विशेषगरी CloudWatch vs CloudTrail vs X-Ray को फरक।**

---

## १. Amazon CloudWatch Metrics ⭐

- **CloudWatch ले AWS का हरेक सेवाको metric दिन्छ**
- **Metric = निगरानी गर्ने चर** (CPUUtilization, NetworkIn…)
- Metric मा **timestamp** हुन्छ
- Metric का **CloudWatch dashboard** बनाउन सकिन्छ

### जान्नैपर्ने Metrics ⭐⭐

| संसाधन | Metrics |
|---|---|
| **EC2 instances** | **CPU Utilization, Status Checks, Network** — ⚠️ **RAM होइन!** |
| | पूर्वनिर्धारित metric **हरेक ५ मिनेट** |
| | **Detailed Monitoring** ($$$): **हरेक १ मिनेट** |
| **EBS volumes** | Disk Read/Writes |
| **S3 buckets** | BucketSizeBytes, NumberOfObjects, AllRequests |
| **Billing** | **Total Estimated Charge** — ⚠️ **`us-east-1` मा मात्र!** |
| **Service Limits** | कति सेवाको API प्रयोग गरियो |
| **Custom metrics** | आफ्नै metric पठाउन सकिने |

> ⭐⭐ **परीक्षामा धेरै सोधिने दुई तथ्य:**
> १. **CloudWatch ले EC2 को RAM (memory) metric पूर्वनिर्धारित रूपमा दिँदैन** — त्यसको लागि custom metric चाहिन्छ
> २. **Billing metric केवल `us-east-1` मा हुन्छ**

---

## २. Amazon CloudWatch Alarms ⭐

- जुनसुकै metric मा **notification trigger गर्न** alarm प्रयोग हुन्छ

### Alarm ले गर्न सक्ने काम ⭐

| Action | विवरण |
|---|---|
| **Auto Scaling** | EC2 instance को "desired" सङ्ख्या बढाउने वा घटाउने |
| **EC2 Actions** | instance **stop, terminate, reboot वा recover** गर्ने |
| **SNS notifications** | SNS topic मा notification पठाउने |

- विभिन्न विकल्प: sampling, %, max, min आदि
- Alarm मूल्याङ्कन गर्ने अवधि छान्न सकिने
- **उदाहरण:** CloudWatch Billing metric मा **billing alarm** बनाउने

### Alarm का ३ अवस्था ⭐
- **OK** — सबै ठीक
- **INSUFFICIENT_DATA** — पर्याप्त डाटा छैन
- **ALARM** — सीमा नाघ्यो

---

## ३. Amazon CloudWatch Logs ⭐

CloudWatch Logs ले यी ठाउँबाट log सङ्कलन गर्न सक्छ:

| स्रोत | के सङ्कलन गर्छ |
|---|---|
| **Elastic Beanstalk** | एप्लिकेसनको log |
| **ECS** | Container बाट |
| **AWS Lambda** | Function को log |
| **CloudTrail** | Filter अनुसार |
| **CloudWatch log agent** | EC2 मेसिन वा **on-premises server** मा |
| **Route 53** | DNS query को log |

- **Real-time मा log निगरानी** गर्न सकिने
- **CloudWatch Logs को retention मिलाउन सकिने**

### EC2 को लागि CloudWatch Logs ⭐⭐

- ⚠️ **पूर्वनिर्धारित रूपमा EC2 instance बाट कुनै log CloudWatch मा जाँदैन!**
- चाहिएको log file पठाउन EC2 मा **CloudWatch agent चलाउनुपर्छ**
- **IAM अनुमति सही छ कि छैन** सुनिश्चित गर्नुहोस्
- CloudWatch log agent **on-premises मा पनि** सेटअप गर्न सकिन्छ

```
EC2 Instance (CloudWatch Logs Agent) ──> CloudWatch Logs
On Premise Server (CloudWatch Logs Agent) ──> CloudWatch Logs
```

---

## ४. Amazon EventBridge (पहिले CloudWatch Events) ⭐

**EventBridge ले २ किसिमले काम गर्छ:**

### १️⃣ Schedule — Cron job (तालिका अनुसार script)
```
हरेक घण्टा तालिका → Lambda function मा script चलाउने
```

### २️⃣ Event Pattern — कुनै सेवाले केही गर्दा प्रतिक्रिया दिने
```
IAM Root User Sign in Event → Email notification सहितको SNS Topic
```

**Lambda function trigger गर्ने, SQS/SNS message पठाउने** आदि गर्न सक्छ।

### EventBridge Rules — स्रोत र गन्तव्य

| **उदाहरण स्रोत (Source)** | **उदाहरण गन्तव्य (Destination)** |
|---|---|
| EC2 Instance (जस्तै: Start Instance) | **Compute:** Lambda, AWS Batch, ECS Task |
| CodeBuild (जस्तै: failed build) | **Integration:** SQS, SNS, Kinesis Data Streams |
| S3 Event (जस्तै: object upload) | **Orchestration:** Step Functions, CodePipeline, CodeBuild |
| Trusted Advisor (जस्तै: नयाँ Finding) | **Maintenance:** SSM, EC2 Actions |
| CloudTrail (कुनै पनि API call) | |
| Schedule वा Cron (जस्तै: हरेक ४ घण्टा) | |

### EventBridge का थप सुविधा
- **Schema Registry** — event schema को model
- Event bus मा पठाइएका event **archive** गर्न सकिने (सबै/filter — अनिश्चितकाल वा तोकिएको अवधि)
- **Archive गरिएका event replay गर्ने क्षमता**

**Event Bus का प्रकार:** Default Event Bus (AWS सेवा), Partner Event Bus (AWS SaaS Partner), Custom Event Bus (Custom App)

---

## ५. AWS CloudTrail ⭐⭐

**CloudTrail = तपाईंको AWS Account को governance, compliance र audit**

| तथ्य | विवरण |
|---|---|
| ⭐ **पूर्वनिर्धारित रूपमा सक्षम!** | अलग्गै अन गर्नुपर्दैन |
| के दिन्छ | तपाईंको AWS Account भित्र भएका **event / API call को इतिहास** |
| कहाँबाट भएका | **Console, SDK, CLI, AWS Services** |
| Log कहाँ राख्न मिल्छ | **CloudWatch Logs** वा **S3** |
| Trail को दायरा | **All Regions** (पूर्वनिर्धारित) वा एउटा Region |

> ⭐⭐ **परीक्षाको सुनौलो नियम:**
> **"AWS मा कुनै संसाधन मेटियो, कसले मेट्यो थाहा पाउनुपर्‍यो?" → पहिले CloudTrail हेर्नुहोस्!**

```
Console ┐
SDK     ├──> CloudTrail ──┬──> CloudTrail Console (Inspect र Audit)
CLI     ┘                 ├──> CloudWatch Logs
IAM Users & Roles         └──> S3 Bucket
```

### CloudTrail Insights
- CloudTrail Event को **स्वचालित विश्लेषण** — असामान्य गतिविधि पत्ता लगाउने

---

## ६. ⭐⭐ CloudWatch vs CloudTrail vs Config — सबैभन्दा महत्त्वपूर्ण फरक

| | **CloudWatch** | **CloudTrail** | **Config** |
|---|---|---|---|
| के गर्छ | **प्रदर्शन** निगरानी (metrics, logs, alarms) | **कसले के गर्‍यो** (API call को audit) | **Configuration** र compliance को रेकर्ड |
| प्रश्न | "CPU कति छ?" | "यो bucket कसले मेट्यो?" | "यो security group कहिले खुला भयो?" |
| सम्झने शब्द | **Performance** | **Who did what** | **What changed** |

---

## ७. AWS X-Ray ⭐

### पुरानो तरिकाले production मा debug गर्ने समस्या
- Local मा test गर्ने
- जताततै log statement थप्ने
- Production मा फेरि deploy गर्ने
- ⚠️ Log format फरक-फरक हुन्छ, log विश्लेषण गाह्रो हुन्छ
- एउटा ठूलो monolith debug गर्न "सजिलो", **distributed service debug गर्न "गाह्रो"**
- पूरै आर्किटेक्चरको साझा दृश्य छैन

**👉 समाधान: AWS X-Ray!**

### X-Ray = एप्लिकेसनको दृश्य विश्लेषण (visual analysis)

**फाइदाहरू ⭐**
- **प्रदर्शनको समस्या (bottleneck) पत्ता लगाउने**
- Microservice आर्किटेक्चरमा **निर्भरता बुझ्ने**
- सेवाको समस्या ठ्याक्कै पहिचान गर्ने
- Request को व्यवहार समीक्षा गर्ने
- Error र exception भेट्टाउने
- SLA पूरा भइरहेको छ कि छैन?
- कहाँ throttle भइरहेको छु?
- कुन प्रयोगकर्ता प्रभावित छन् पहिचान गर्ने

> **परीक्षा टिप:** प्रश्नमा **"distributed application", "microservices", "trace requests", "bottleneck"** आयो → **X-Ray**

---

## ८. AWS Health Dashboard ⭐

### १️⃣ AWS Health Dashboard — Service History
- **सबै region, सबै सेवाको स्वास्थ्य** देखाउने
- **हरेक दिनको ऐतिहासिक जानकारी**
- **RSS feed** छ, subscribe गर्न सकिने
- *पहिलेको नाम: AWS Service Health Dashboard*

### २️⃣ AWS Health Dashboard — Your Account ⭐
*पहिलेको नाम: AWS Personal Health Dashboard (PHD)*

- AWS मा तपाईंलाई असर पार्न सक्ने घटना हुँदा **alert र समाधानको मार्गदर्शन** दिन्छ
- ⭐ **फरक:**
  - **Service Health Dashboard** = AWS सेवाहरूको **सामान्य अवस्था**
  - **Account Health Dashboard** = तपाईंकै AWS संसाधनको **व्यक्तिगत दृश्य**
- चलिरहेको घटना व्यवस्थापन गर्न सान्दर्भिक र समयमै जानकारी
- **तालिकाबद्ध गतिविधिको लागि पूर्व-सूचना** दिन्छ
- ⭐ **पूरै AWS Organization को डाटा एकीकृत गर्न सक्ने**
- **Global सेवा**
- AWS को outage ले **तपाईंलाई र तपाईंका संसाधनलाई कसरी असर गर्छ** देखाउने
- Alert, remediation, proactive, scheduled activities

> **परीक्षा टिप:**
> - "AWS सेवाको सामान्य अवस्था" → **Service History Dashboard**
> - "मेरा संसाधनमा कस्तो असर?" → **Account Health Dashboard**

---

## 📝 अभ्यास प्रश्नहरू

**प्रश्न १.** कसैले production S3 bucket मेटायो। को ले मेट्यो पत्ता लगाउन कुन सेवा?

A) Amazon CloudWatch
B) AWS CloudTrail
C) AWS X-Ray
D) AWS Config

<details><summary>उत्तर</summary>

**B) AWS CloudTrail** — API call को audit। "संसाधन मेटियो → पहिले CloudTrail" भन्ने नियम याद गर्नुहोस्।
</details>

---

**प्रश्न २.** CloudWatch ले EC2 को कुन metric पूर्वनिर्धारित रूपमा **दिँदैन**?

A) CPU Utilization
B) Network In/Out
C) Memory (RAM) Utilization
D) Status Checks

<details><summary>उत्तर</summary>

**C) Memory (RAM)** — RAM को metric पूर्वनिर्धारित रूपमा हुँदैन। Custom metric पठाउनुपर्छ। यो परीक्षामा धेरै सोधिन्छ।
</details>

---

**प्रश्न ३.** Billing alarm बनाउन CloudWatch मा कुन region मा जानुपर्छ?

A) जुनसुकै region
B) us-east-1
C) eu-west-1
D) प्रयोगकर्ताको नजिकको region

<details><summary>उत्तर</summary>

**B) us-east-1** — Billing metric केवल `us-east-1` मा राखिन्छ।
</details>

---

**प्रश्न ४.** माइक्रोसर्भिस आर्किटेक्चरमा कुन सेवाले ढिलो प्रतिक्रिया दिइरहेको छ पत्ता लगाउन कुन सेवा?

A) Amazon CloudWatch Logs
B) AWS CloudTrail
C) AWS X-Ray
D) AWS Config

<details><summary>उत्तर</summary>

**C) AWS X-Ray** — distributed application मा request trace गरेर bottleneck पहिचान गर्छ।
</details>

---

**प्रश्न ५.** EC2 instance को application log CloudWatch मा पठाउन के गर्नुपर्छ?

A) केही गर्नुपर्दैन, आफैं जान्छ
B) EC2 मा CloudWatch agent install गर्नुपर्छ
C) CloudTrail सक्षम गर्नुपर्छ
D) Security Group मा rule थप्नुपर्छ

<details><summary>उत्तर</summary>

**B)** — पूर्वनिर्धारित रूपमा EC2 बाट log जाँदैन। CloudWatch agent चाहिन्छ, र IAM अनुमति पनि सही हुनुपर्छ।
</details>

---

**प्रश्न ६.** हरेक ४ घण्टामा एउटा Lambda function चलाउनुपर्‍यो। कुन सेवा प्रयोग गर्ने?

A) AWS CloudTrail
B) Amazon EventBridge (CloudWatch Events)
C) AWS X-Ray
D) Amazon SQS

<details><summary>उत्तर</summary>

**B) Amazon EventBridge** — schedule/cron मार्फत Lambda trigger गर्न।
</details>

---

**प्रश्न ७.** AWS को outage ले तपाईंकै संसाधनमा कस्तो असर परेको छ हेर्न कुन dashboard?

A) AWS Health Dashboard – Service History
B) AWS Health Dashboard – Your Account
C) Amazon CloudWatch Dashboard
D) AWS Trusted Advisor

<details><summary>उत्तर</summary>

**B) Your Account (पहिलेको Personal Health Dashboard)** — तपाईंका संसाधनको व्यक्तिगत दृश्य।
</details>

---

**प्रश्न ८.** CloudTrail बारे कुन सही हो?

A) प्रयोग गर्न पहिले सक्षम गर्नुपर्छ
B) पूर्वनिर्धारित रूपमा सक्षम हुन्छ
C) केवल Console बाट भएका action मात्र रेकर्ड गर्छ
D) एउटा region मा मात्र काम गर्छ

<details><summary>उत्तर</summary>

**B)** — CloudTrail पूर्वनिर्धारित रूपमा सक्षम हुन्छ, र Console/CLI/SDK/AWS सेवा सबैका API call रेकर्ड गर्छ। Trail सबै region मा लागू गर्न सकिन्छ।
</details>

---

## ✅ यो मोड्युलको सार

| सेवा | काम |
|---|---|
| **CloudWatch Metrics** | AWS सेवा र billing को **प्रदर्शन** निगरानी |
| **CloudWatch Alarms** | Metric अनुसार notification, EC2 action वा SNS notify स्वचालित गर्ने |
| **CloudWatch Logs** | EC2 instance, server, Lambda function आदिबाट log सङ्कलन |
| **EventBridge (Events)** | AWS मा भएका घटनामा प्रतिक्रिया, वा तालिकामा rule चलाउने |
| **CloudTrail** | AWS account भित्र भएका **API call को audit** |
| **CloudTrail Insights** | CloudTrail Event को स्वचालित विश्लेषण |
| **X-Ray** | **Distributed एप्लिकेसन** मा request trace गर्ने |
| **AWS Health Dashboard** | सबै region का सबै AWS सेवाको अवस्था |
| **AWS Account Health Dashboard** | तपाईंकै infrastructure लाई असर गर्ने AWS event |

---

⬅️ [मोड्युल ११](11-cloud-integration.md) | ➡️ [मोड्युल १३ — VPC](13-vpc.md)
