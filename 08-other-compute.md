# मोड्युल ०८ — अन्य Compute सेवाहरू (Docker, Containers, Serverless, Lambda)

> ⭐ **परीक्षामा धेरै महत्त्वपूर्ण — विशेषगरी Lambda, Fargate र ECS/EKS को फरक।**

---

## १. Docker के हो?

**Docker = एप्लिकेसन deploy गर्ने software development प्लेटफर्म।**

एप्लिकेसनलाई **container** मा प्याक गरिन्छ, जुन **जुनसुकै OS** मा चल्न सक्छ।

### फाइदाहरू
- जहाँ चलाए पनि **एउटै व्यवहार**
- **जुनसुकै मेसिन** मा चल्ने
- **Compatibility को समस्या छैन**
- **अनुमानयोग्य व्यवहार**
- **कम मेहनत**
- मर्मत र deploy गर्न **सजिलो**
- जुनसुकै भाषा, जुनसुकै OS, जुनसुकै प्रविधिसँग चल्ने
- **सेकेन्डमै** container scale up/down गर्न सकिने

### Docker image कहाँ राखिन्छ?
**Docker Repository** मा:

| प्रकार | नाम |
|---|---|
| **Public** | **Docker Hub** (https://hub.docker.com/) — Ubuntu, MySQL, NodeJS, Java जस्ता base image |
| **Private** | **Amazon ECR** (Elastic Container Registry) |

### Docker vs Virtual Machine ⭐

Docker "एक किसिमको" virtualization हो, तर ठ्याक्कै त्यस्तो होइन। **संसाधन host सँग साझा हुन्छ** — त्यसैले एउटै server मा धेरै container अट्छन्।

```
Virtual Machine                    Docker
──────────────────                 ──────────────────
Apps | Apps | Apps                 Container × 9
Guest OS (VM) × 3                  ─────────────────
─────────────────                  Docker Daemon
Hypervisor                         Host OS (EC2 Instance)
Host OS                            Infrastructure
Infrastructure
```

👉 **मुख्य फरक:** VM मा हरेकको छुट्टै Guest OS हुन्छ (भारी), Docker मा एउटै Host OS साझा हुन्छ (हलुका)।

---

## २. ECS — Elastic Container Service ⭐

- AWS मा **Docker container चलाउने**
- ⚠️ **तपाईंले infrastructure (EC2 instance) provision र मर्मत गर्नुपर्छ**
- **AWS ले container सुरु/बन्द गर्ने काम गर्छ**
- **Application Load Balancer** सँग एकीकरण

```
ECS Service → [EC2 instance | EC2 instance | EC2 instance] मा नयाँ Docker Container राख्छ
```

---

## ३. AWS Fargate ⭐⭐

- AWS मा **Docker container चलाउने**
- ✅ **तपाईंले infrastructure provision गर्नुपर्दैन — कुनै EC2 instance व्यवस्थापन गर्नुपर्दैन!** (सजिलो)
- **Serverless offering**
- तपाईंलाई चाहिने CPU/RAM अनुसार AWS ले container चलाइदिन्छ

> ⭐⭐ **ECS vs Fargate को फरक परीक्षामा पक्कै आउँछ:**
> - **ECS** = तपाईंले EC2 instance व्यवस्थापन गर्नुपर्छ
> - **Fargate** = कुनै EC2 छैन, **serverless**

---

## ४. ECR — Elastic Container Registry

- AWS मा **निजी Docker Registry**
- यहाँ Docker image राख्नुहुन्छ, ताकि **ECS वा Fargate** ले चलाउन सकोस्

---

## ५. Amazon EKS — Elastic Kubernetes Service ⭐

- AWS मा **managed Kubernetes cluster** चलाउने
- **Kubernetes** = containerized app को व्यवस्थापन, deployment र scaling को लागि open-source सिस्टम
- Container होस्ट गर्न सकिने ठाउँ:
  - **EC2 instance**
  - **Fargate** (Serverless)
- ⭐ **Kubernetes cloud-agnostic हो** — Azure, GCP जुनसुकै क्लाउडमा प्रयोग गर्न मिल्छ

> **परीक्षा टिप:** प्रश्नमा **"Kubernetes"** आयो → **EKS**। **"cloud-agnostic container orchestration"** आयो → **Kubernetes/EKS**।

---

## ६. Serverless के हो? ⭐

> **Serverless = developer ले server व्यवस्थापन गर्नुनपर्ने नयाँ प्रतिमान। तिनले कोड मात्र deploy गर्छन् — function मात्र deploy गर्छन्!**

- सुरुमा: **Serverless == FaaS (Function as a Service)**
- **AWS Lambda** ले यसको सुरुवात गर्‍यो
- अहिले **जुनसुकै managed कुरा** (database, messaging, storage आदि) पनि serverless मानिन्छ

> ⚠️ **महत्त्वपूर्ण:** **Serverless को अर्थ server नहुनु होइन!** — अर्थ हो तपाईंले server **व्यवस्थापन / provision / देख्नु पर्दैन**।

### अहिलेसम्म सिकेका Serverless सेवाहरू
**Lambda, Fargate, Amazon S3, DynamoDB**

---

## ७. AWS Lambda ⭐⭐⭐

### EC2 vs Lambda

| **Amazon EC2** | **AWS Lambda** |
|---|---|
| क्लाउडमा भर्चुअल Server | **भर्चुअल Function — कुनै server व्यवस्थापन गर्नुपर्दैन!** |
| RAM र CPU ले सीमित | **समयले सीमित — छोटो execution** |
| **निरन्तर चलिरहने** | **आवश्यक पर्दा मात्र चल्ने (on-demand)** |
| Scaling को लागि हस्तक्षेप चाहिने | **Scaling स्वचालित!** |

### Lambda का फाइदाहरू ⭐

| फाइदा | विवरण |
|---|---|
| **सजिलो मूल्य** | प्रति request र compute समय अनुसार |
| **Free tier** | **१०,००,००० (१० लाख) Lambda request** र **४,००,००० GB-सेकेन्ड** compute समय |
| **एकीकरण** | पूरै AWS सेवाहरूसँग |
| **Event-Driven** | आवश्यक पर्दा AWS ले function invoke गर्छ |
| **धेरै भाषा** | तल हेर्नुहोस् |
| **Monitoring** | CloudWatch मार्फत सजिलो |
| **संसाधन** | प्रति function **१० GB सम्म RAM**! |
| **बोनस** | **RAM बढाउँदा CPU र network पनि सुधार हुन्छ** |

### Lambda ले समर्थन गर्ने भाषा ⭐

- **Node.js (JavaScript)**
- **Python**
- **Java**
- **C# (.NET Core) / Powershell**
- **Ruby**
- **Custom Runtime API** (community समर्थित — जस्तै Rust, Golang)
- **Lambda Container Image**
  - Container image ले **Lambda Runtime API लागू गर्नैपर्छ**
  - ⚠️ **जथाभावी Docker image चलाउन ECS / Fargate नै राम्रो**

> **परीक्षा टिप:** "Lambda ले सबै भाषा समर्थन गर्छ" **गलत** हो। **मनपरी Docker image** चलाउन Lambda होइन, **ECS/Fargate** चाहिन्छ।

### Lambda को प्रयोगका उदाहरण ⭐

**१. Serverless Thumbnail बनाउने**
```
S3 मा नयाँ image → [trigger] → AWS Lambda Function → Thumbnail बनाउँछ → S3 मा नयाँ thumbnail
                                                    → DynamoDB मा metadata (नाम, आकार, मिति)
```

**२. Serverless CRON Job**
```
CloudWatch Events / EventBridge → [हरेक १ घण्टा trigger] → AWS Lambda Function → काम गर्छ
```

### Lambda को मूल्य ⭐

**Pay per call (call अनुसार):**
- पहिलो **१०,००,००० request निःशुल्क**
- त्यसपछि **$0.20 प्रति १० लाख request** ($0.0000002 प्रति request)

**Pay per duration (अवधि अनुसार — १ ms को increment मा):**
- प्रति महिना **४,००,००० GB-सेकेन्ड निःशुल्क**
  - == ४,००,००० सेकेन्ड यदि function 1 GB RAM को छ भने
  - == ३२,००,००० सेकेन्ड यदि function 128 MB RAM को छ भने
- त्यसपछि **$1.00 प्रति ६,००,००० GB-सेकेन्ड**

👉 Lambda चलाउन सामान्यतया **धेरै सस्तो** पर्छ — त्यसैले यो धेरै लोकप्रिय छ।

---

## ८. Amazon API Gateway ⭐

- API सजिलै **बनाउने, प्रकाशित गर्ने, मर्मत गर्ने, निगरानी गर्ने र सुरक्षित गर्ने** पूर्ण managed सेवा
- **Serverless र scalable**
- **RESTful API र WebSocket API** समर्थन
- सुरक्षा, user authentication, **API throttling**, API key, monitoring समर्थन

### Serverless API को उदाहरण
```
Client → API Gateway (REST API) → Lambda → DynamoDB (CRUD)
```

> **परीक्षा टिप:** **"Lambda function लाई HTTP API को रूपमा देखाउने" → API Gateway**

---

## ९. AWS Batch ⭐

- जुनसुकै scale मा **पूर्ण managed batch processing**
- AWS मा **लाखौं batch computing job** कुशलतापूर्वक चलाउने
- **"Batch" job = सुरु र अन्त्य भएको job** (निरन्तर चल्ने होइन)
- Batch ले **EC2 instance वा Spot Instance गतिशील रूपमा launch** गर्छ
- **सही मात्राको compute/memory provision** गर्छ
- तपाईंले job पेश गर्नुहुन्छ वा तालिका बनाउनुहुन्छ — **बाँकी AWS Batch ले गर्छ!**
- Batch job **Docker image** को रूपमा परिभाषित हुन्छ, **ECS, EKS वा Fargate** मा चल्छ
- **लागत अनुकूलन** र infrastructure मा कम ध्यान दिन उपयोगी

### Batch vs Lambda ⭐⭐

| | **Lambda** | **Batch** |
|---|---|---|
| **समय सीमा** | ✋ **हुन्छ (अधिकतम १५ मिनेट)** | ✅ **छैन** |
| **Runtime** | सीमित (निश्चित भाषा मात्र) | **जुनसुकै — Docker image मा प्याक भए पुग्यो** |
| **Disk space** | सीमित अस्थायी disk | **EBS / instance store मा भर पर्ने** |
| **Server** | **Serverless** | **EC2 मा निर्भर** (AWS ले manage गर्न सक्छ) |

> **परीक्षा टिप:** "**१५ मिनेट भन्दा लामो** काम" वा "**जुनसुकै runtime**" चाहियो → **Batch**।

---

## १०. Amazon Lightsail ⭐

- **भर्चुअल server, storage, database र networking**
- **कम र अनुमानयोग्य मूल्य** ⭐
- EC2, RDS, ELB, EBS, Route 53 प्रयोग गर्नुको **सरल विकल्प**
- ⭐ **क्लाउडको कम अनुभव भएकाहरूको लागि उत्तम!**
- आफ्ना Lightsail संसाधनको notification र monitoring सेट गर्न सकिने

**प्रयोग:**
- सरल वेब एप्लिकेसन (LAMP, Nginx, MEAN, Node.js का template छन्)
- वेबसाइट (WordPress, Magento, Plesk, Joomla का template)
- Dev / Test environment

⚠️ **सीमा:** High availability छ तर **auto-scaling छैन**, र **AWS सँग सीमित एकीकरण**।

> **परीक्षा टिप:** "क्लाउडको अनुभव नभएको प्रयोगकर्ता + सरल एप + अनुमानयोग्य कम मूल्य" → **Lightsail**

---

## 📝 अभ्यास प्रश्नहरू

**प्रश्न १.** एउटा कम्पनीले AWS मा Docker container चलाउन चाहन्छ तर कुनै EC2 instance व्यवस्थापन गर्न चाहँदैन। कुन सेवा?

A) Amazon ECS with EC2 launch type
B) AWS Fargate
C) Amazon EC2
D) AWS Batch

<details><summary>उत्तर</summary>

**B) AWS Fargate** — serverless container, कुनै EC2 provision गर्नुपर्दैन।
</details>

---

**प्रश्न २.** S3 मा image अपलोड हुनासाथ thumbnail बनाउनुपर्‍यो। सबैभन्दा उपयुक्त सेवा?

A) Amazon EC2
B) AWS Lambda
C) AWS Batch
D) Amazon Lightsail

<details><summary>उत्तर</summary>

**B) AWS Lambda** — event-driven, S3 event ले trigger गर्छ, छोटो काम। यो Lambda को classic उदाहरण हो।
</details>

---

**प्रश्न ३.** Lambda function को अधिकतम execution समय कति हो?

A) ५ मिनेट
B) १५ मिनेट
C) ३० मिनेट
D) कुनै सीमा छैन

<details><summary>उत्तर</summary>

**B) १५ मिनेट** — यसभन्दा लामो काम चाहिए AWS Batch प्रयोग गर्नुहोस्।
</details>

---

**प्रश्न ४.** "Serverless" को सही अर्थ के हो?

A) कुनै server हुँदैन
B) Server छन् तर तपाईंले व्यवस्थापन/provision गर्नुपर्दैन
C) Server आफ्नै data center मा हुन्छ
D) Server निःशुल्क हुन्छ

<details><summary>उत्तर</summary>

**B)** — Server त हुन्छन्, तर तपाईंले तिनलाई देख्नु, provision गर्नु वा व्यवस्थापन गर्नु पर्दैन।
</details>

---

**प्रश्न ५.** एउटा टिमले Kubernetes प्रयोग गर्दैछ र AWS मा managed Kubernetes cluster चाहन्छ। कुन सेवा?

A) Amazon ECS
B) AWS Fargate
C) Amazon EKS
D) Amazon ECR

<details><summary>उत्तर</summary>

**C) Amazon EKS** — Elastic Kubernetes Service।
</details>

---

**प्रश्न ६.** क्लाउडको धेरै अनुभव नभएको प्रयोगकर्ताले WordPress वेबसाइट चलाउन चाहन्छ र मासिक खर्च अनुमानयोग्य होस् भन्ने चाहन्छ। कुन सेवा?

A) Amazon EC2
B) AWS Elastic Beanstalk
C) Amazon Lightsail
D) AWS Lambda

<details><summary>उत्तर</summary>

**C) Amazon Lightsail** — कम र अनुमानयोग्य मूल्य, WordPress template, नौसिखियाको लागि बनेको।
</details>

---

**प्रश्न ७.** Lambda function लाई HTTP endpoint को रूपमा उपलब्ध गराउन कुन सेवा?

A) Elastic Load Balancer
B) Amazon API Gateway
C) Amazon CloudFront
D) AWS Step Functions

<details><summary>उत्तर</summary>

**B) Amazon API Gateway** — REST र WebSocket API बनाउने serverless सेवा।
</details>

---

**प्रश्न ८.** Lambda को billing कसरी हुन्छ? (दुई छान्नुहोस्)

A) कति समय चल्यो × कति RAM provision गरियो
B) कति invocation भयो
C) कति EC2 instance चल्यो
D) कति storage प्रयोग भयो
E) प्रति महिना निश्चित शुल्क

<details><summary>उत्तर</summary>

**A)** र **B)** — Lambda को billing: (१) duration × RAM, र (२) invocation सङ्ख्या।
</details>

---

## ✅ यो मोड्युलको सार

### Container र अन्य compute
| सेवा | काम |
|---|---|
| **Docker** | एप्लिकेसन चलाउने container प्रविधि |
| **ECS** | EC2 instance मा Docker container चलाउने |
| **Fargate** | infrastructure provision नगरी Docker container चलाउने — **Serverless (कुनै EC2 छैन)** |
| **ECR** | निजी Docker Image Repository |
| **EKS** | Managed Kubernetes |
| **Batch** | Managed EC2 मा batch job चलाउने — **समय सीमा छैन** |
| **Lightsail** | सरल एप्लिकेसन र DB stack को लागि अनुमानयोग्य र कम मूल्य |

### Lambda सारांश
- **Serverless, Function as a Service, निर्बाध scaling, reactive**
- **Billing:** (चलेको समय × provision गरिएको RAM) + (invocation सङ्ख्या)
- **भाषा:** धेरै प्रोग्रामिङ भाषा — तर **जथाभावी Docker image होइन**
- **Invocation समय: अधिकतम १५ मिनेट**
- **प्रयोग:** S3 मा अपलोड भएको image को thumbnail बनाउने, serverless cron job
- **API Gateway:** Lambda function लाई HTTP API को रूपमा देखाउने

---

⬅️ [मोड्युल ०७](07-databases.md) | ➡️ [मोड्युल ०९ — Deployment](09-deployment.md)
