# मोड्युल ०९ — ठूलो स्केलमा Infrastructure Deploy र व्यवस्थापन

> **यस मोड्युलमा:** CloudFormation, CDK, Elastic Beanstalk, CodeCommit/Build/Deploy/Pipeline/Artifact, र Systems Manager (SSM)।

---

## १. AWS CloudFormation ⭐⭐

**CloudFormation = तपाईंको AWS Infrastructure लाई declarative तरिकाले लेख्ने माध्यम।**

CloudFormation template मा तपाईं यसो भन्नुहुन्छ:
- मलाई एउटा **security group** चाहियो
- मलाई त्यही security group प्रयोग गर्ने **२ वटा EC2 instance** चाहियो
- मलाई एउटा **S3 bucket** चाहियो
- मलाई ती मेसिनको अगाडि एउटा **load balancer (ELB)** चाहियो

अनि CloudFormation ले ती सबै **सही क्रममा**, तपाईंले तोकेकै configuration मा बनाइदिन्छ।

### CloudFormation का फाइदाहरू ⭐

**१. Infrastructure as Code (IaC)**
- कुनै संसाधन **हातैले बनाइँदैन** — नियन्त्रणको लागि उत्कृष्ट
- Infrastructure मा हुने परिवर्तन **कोड मार्फत समीक्षा** हुन्छ

**२. लागत (Cost)**
- Stack का हरेक संसाधनमा **identifier tag** लाग्छ — कुन stack ले कति खर्च गर्‍यो सजिलै देखिन्छ
- Template बाट **लागत अनुमान** गर्न सकिन्छ
- **बचत रणनीति:** Dev मा बेलुका ५ बजे template delete गर्ने र बिहान ८ बजे फेरि बनाउने — सुरक्षित तरिकाले automate गर्न सकिने

**३. उत्पादकता (Productivity)**
- क्लाउडमा infrastructure तुरुन्तै नष्ट गर्ने र फेरि बनाउने क्षमता
- Template बाट **diagram स्वचालित रूपमा बन्ने**
- **Declarative programming** — क्रम र orchestration आफैं मिलाउनुपर्दैन

**४. पाङ्ग्रा फेरि आविष्कार गर्नु पर्दैन**
- इन्टरनेटमा भएका template प्रयोग गर्ने
- Documentation प्रयोग गर्ने
- (लगभग) **सबै AWS संसाधन समर्थित** — समर्थित नभएको को लागि **"custom resources"**

### CloudFormation + Infrastructure Composer
- Stack का **सबै संसाधन** देख्न सकिने
- ती component बीचको **सम्बन्ध** देख्न सकिने

> **परीक्षा टिप:** प्रश्नमा **"Infrastructure as Code"**, **"धेरै region/account मा दोहोर्‍याउने"** भन्ने आयो → **CloudFormation**

---

## २. AWS CDK — Cloud Development Kit ⭐

- **परिचित प्रोग्रामिङ भाषा** प्रयोग गरेर आफ्नो cloud infrastructure परिभाषित गर्ने
  - **JavaScript/TypeScript, Python, Java, .NET**
- त्यो कोड **CloudFormation template (JSON/YAML) मा "compile"** हुन्छ
- त्यसैले **infrastructure र application runtime code सँगै deploy** गर्न सकिन्छ
- **Lambda function** को लागि उत्तम
- **ECS / EKS का Docker container** को लागि उत्तम

```
CDK Application (प्रोग्रामिङ भाषा) → CDK CLI → CloudFormation Template → CloudFormation
```

> **CloudFormation vs CDK:** CloudFormation = YAML/JSON लेख्ने। CDK = Python/TypeScript जस्तो भाषामा लेख्ने।

---

## ३. Developer को समस्या

AWS मा developer हरूलाई यी समस्या हुन्छन्:
- Infrastructure व्यवस्थापन गर्नु
- कोड deploy गर्नु
- सबै database, load balancer आदि configure गर्नु
- Scaling को चिन्ता

तर **अधिकांश वेब एपको आर्किटेक्चर उही हुन्छ (ALB + ASG)**। Developer ले चाहेको त **आफ्नो कोड चलोस्** भन्ने मात्र हो — त्यो पनि फरक-फरक एप र environment मा एकरूप तरिकाले।

**👉 समाधान: Elastic Beanstalk!**

---

## ४. AWS Elastic Beanstalk ⭐⭐

**Elastic Beanstalk = AWS मा एप deploy गर्ने developer-केन्द्रित दृष्टिकोण।**

- यसले हामीले सिकेका सबै component प्रयोग गर्छ: **EC2, ASG, ELB, RDS** आदि
- तर सबै **एउटै सजिलो view** मा!
- Configuration मा **पूर्ण नियन्त्रण** रहन्छ
- ⭐ **Beanstalk = Platform as a Service (PaaS)**
- ⭐ **Beanstalk आफैं निःशुल्क** — तर तल्लो instance को पैसा लाग्छ

### के AWS ले सम्हाल्छ?
- Instance configuration / OS
- Deployment strategy (configure गर्न मिल्ने, तर चलाउने Beanstalk ले)
- **Capacity provisioning**
- **Load balancing र auto-scaling**
- **Application health-monitoring** र प्रतिक्रिया

👉 **Developer को जिम्मा: एप्लिकेसन कोड मात्र!**

### ३ आर्किटेक्चर मोडेल ⭐

| मोडेल | कहिले प्रयोग गर्ने |
|---|---|
| **Single Instance deployment** | Dev को लागि राम्रो |
| **LB + ASG** | Production वा pre-production **वेब एप्लिकेसन** को लागि उत्तम |
| **ASG only** | Production मा **non-web एप** (worker आदि) को लागि उत्तम |

### समर्थित प्लेटफर्म
Go, Java SE, Java with Tomcat, .NET on Windows Server with IIS, Node.js, PHP, Python, Ruby, Packer Builder

**Docker:** Single Container Docker, Multi-Container Docker, Preconfigured Docker

### Health Monitoring
- **Health agent** ले CloudWatch मा metric पठाउँछ
- एपको स्वास्थ्य जाँच्छ, health event प्रकाशित गर्छ

---

## ५. AWS CodeDeploy ⭐

- एप्लिकेसन **स्वचालित रूपमा deploy** गर्ने
- **EC2 Instance** सँग काम गर्छ
- **On-Premises Server** सँग पनि काम गर्छ
- ⭐ **Hybrid सेवा** हो
- ⚠️ Server/Instance मा पहिल्यै **CodeDeploy Agent** install र configure गरिएको हुनुपर्छ

```
v1, v1, v1 → CodeDeploy → v2, v2, v2
(EC2 Instances र On-premises Servers upgrade हुँदै)
```

---

## ६. AWS CodeCommit

- Server मा कोड पठाउनुअघि कतै राख्नुपर्छ
- Developer हरूले सामान्यतया **Git** प्रविधि प्रयोग गरेर repository मा कोड राख्छन्
- लोकप्रिय सार्वजनिक विकल्प **GitHub** हो; AWS को प्रतिस्पर्धी उत्पादन **CodeCommit**

**CodeCommit:**
- **Git-based repository** होस्ट गर्ने source-control सेवा
- अरूसँग कोडमा सहकार्य सजिलो
- कोडको परिवर्तन **स्वचालित रूपमा version हुन्छ**

**फाइदा:** पूर्ण managed, scalable र highly available, निजी, सुरक्षित, AWS सँग एकीकृत

---

## ७. AWS CodeBuild

- क्लाउडमा **कोड build गर्ने सेवा**
- Source code **compile** गर्ने, **test चलाउने**, र deploy गर्न तयार **package** बनाउने

**फाइदा:**
- पूर्ण managed, **serverless**
- निरन्तर scalable र highly available
- सुरक्षित
- **Pay-as-you-go** — build समयको मात्र पैसा

```
CodeCommit → [कोड लिने] CodeBuild → [कोड build गर्ने] → deploy गर्न तयार artifact
```

---

## ८. AWS CodePipeline ⭐

- कोड स्वचालित रूपमा production सम्म पुर्‍याउने विभिन्न चरणलाई **orchestrate** गर्ने
- **Code ⇒ Build ⇒ Test ⇒ Provision ⇒ Deploy**
- ⭐ **CICD (Continuous Integration र Continuous Delivery) को आधार**

**फाइदा:**
- पूर्ण managed
- CodeCommit, CodeBuild, CodeDeploy, Elastic Beanstalk, CloudFormation, GitHub, तेस्रो पक्ष सेवा र custom plugin सँग मिल्ने
- छिटो delivery र द्रुत update

```
CodePipeline (orchestration layer)
    ↓         ↓          ↓            ↓
CodeCommit → CodeBuild → CodeDeploy → Elastic Beanstalk
```

---

## ९. AWS CodeArtifact

- Software package एकआपसमा निर्भर हुन्छन् (**code dependencies**), र नयाँ-नयाँ बन्दै जान्छन्
- यी dependency राख्ने र निकाल्ने कामलाई **artifact management** भनिन्छ
- परम्परागत रूपमा आफ्नै artifact management सिस्टम बनाउनुपर्थ्यो
- **CodeArtifact = सुरक्षित, scalable, किफायती artifact management**
- मिल्ने उपकरण: **Maven, Gradle, npm, yarn, twine, pip, NuGet**
- Developer र CodeBuild ले सिधै CodeArtifact बाट dependency लिन सक्छन्

---

## १०. AWS Systems Manager (SSM) ⭐⭐

**SSM = ठूलो स्केलमा EC2 र On-Premises सिस्टम व्यवस्थापन गर्ने।**

- ⭐ **यो पनि Hybrid AWS सेवा हो**
- Infrastructure को अवस्थाबारे परिचालन अन्तर्दृष्टि दिने
- **१०+ उत्पादनको सुइट**

### सबैभन्दा महत्त्वपूर्ण सुविधाहरू ⭐
- Compliance को लागि **Patching automation**
- पूरै server fleet मा **command चलाउने**
- **SSM Parameter Store** मा parameter configuration राख्ने

**समर्थित OS:** Linux, Windows, MacOS, र **Raspberry Pi OS (Raspbian)**

### SSM कसरी काम गर्छ? ⭐

- नियन्त्रण गर्ने सिस्टममा **SSM Agent install गर्नुपर्छ**
- **Amazon Linux AMI र केही Ubuntu AMI मा पूर्वनिर्धारित रूपमा install भएको हुन्छ**
- ⚠️ **कुनै instance SSM ले नियन्त्रण गर्न सकेन भने सायद SSM agent को समस्या हो!** (यो परीक्षामा सोधिन्छ)
- SSM agent कै कारण हामी command चलाउन, patch गर्न र server configure गर्न सक्छौं

```
SSM ←→ SSM Agent (EC2 Instance)
    ←→ SSM Agent (EC2 Instance)
    ←→ SSM Agent (On Premise VM)
```

### SSM Session Manager ⭐⭐

- EC2 र on-premises server मा **सुरक्षित shell** सुरु गर्ने
- ✅ **SSH पहुँच चाहिँदैन**
- ✅ **Bastion host चाहिँदैन**
- ✅ **SSH key चाहिँदैन**
- ✅ **Port 22 खोल्नुपर्दैन — बढी सुरक्षित!**
- Linux, macOS र Windows समर्थन
- Session log डाटा **S3 वा CloudWatch Logs** मा पठाउन सकिने

> **परीक्षा टिप:** "port 22 नखोली EC2 मा shell चाहियो" → **SSM Session Manager**

### SSM Parameter Store ⭐

- Configuration र secret को **सुरक्षित भण्डारण**
- API Key, password, configuration…
- **Serverless, scalable, durable, सजिलो SDK**
- **IAM** ले पहुँच अनुमति नियन्त्रण
- **Version tracking र encryption** (वैकल्पिक — AWS KMS मार्फत)

---

## 📝 अभ्यास प्रश्नहरू

**प्रश्न १.** एउटा कम्पनीले उही infrastructure ३ वटा region मा दोहोर्‍याएर deploy गर्न चाहन्छ, र सबै परिवर्तन code review मार्फत होस् भन्ने चाहन्छ। कुन सेवा?

A) AWS Elastic Beanstalk
B) AWS CloudFormation
C) AWS CodeDeploy
D) AWS Systems Manager

<details><summary>उत्तर</summary>

**B) AWS CloudFormation** — Infrastructure as Code, region र account मा दोहोर्‍याउन मिल्ने।
</details>

---

**प्रश्न २.** एउटा developer ले Python एप AWS मा deploy गर्न चाहन्छ तर infrastructure बारे केही जान्न चाहँदैन। कुन सेवा उपयुक्त?

A) Amazon EC2
B) AWS CloudFormation
C) AWS Elastic Beanstalk
D) AWS Batch

<details><summary>उत्तर</summary>

**C) AWS Elastic Beanstalk** — PaaS, developer ले कोड मात्र दिन्छ, बाँकी Beanstalk ले गर्छ।
</details>

---

**प्रश्न ३.** EC2 instance मा port 22 नखोलिकन secure shell चाहियो। कुन सेवा?

A) EC2 Instance Connect
B) SSM Session Manager
C) AWS Direct Connect
D) Bastion Host

<details><summary>उत्तर</summary>

**B) SSM Session Manager** — SSH key, bastion host वा port 22 केही चाहिँदैन। (EC2 Instance Connect मा port 22 चाहिन्छ!)
</details>

---

**प्रश्न ४.** निम्नमध्ये कुन **hybrid** सेवाहरू हुन् (on-premises server सँग पनि चल्ने)? (दुई छान्नुहोस्)

A) AWS CloudFormation
B) AWS Elastic Beanstalk
C) AWS CodeDeploy
D) AWS Systems Manager
E) Amazon EC2

<details><summary>उत्तर</summary>

**C) CodeDeploy** र **D) Systems Manager** — दुवैले on-premises server सँग काम गर्छन्। CloudFormation र Beanstalk AWS-मात्र हुन्।
</details>

---

**प्रश्न ५.** Database password सुरक्षित रूपमा राख्न र encryption सहित version track गर्न कुन सेवा प्रयोग गर्ने?

A) Amazon S3
B) SSM Parameter Store
C) Amazon DynamoDB
D) AWS CodeArtifact

<details><summary>उत्तर</summary>

**B) SSM Parameter Store** — configuration र secret को सुरक्षित, serverless भण्डारण, KMS encryption सहित।
</details>

---

**प्रश्न ६.** CI/CD pipeline को orchestration गर्न कुन सेवा प्रयोग गरिन्छ?

A) AWS CodeBuild
B) AWS CodeCommit
C) AWS CodePipeline
D) AWS CodeArtifact

<details><summary>उत्तर</summary>

**C) AWS CodePipeline** — Code → Build → Test → Provision → Deploy को orchestration layer।
</details>

---

**प्रश्न ७.** Python र TypeScript जस्ता भाषा प्रयोग गरेर AWS infrastructure परिभाषित गर्न कुन सेवा?

A) AWS CloudFormation
B) AWS CDK
C) AWS Elastic Beanstalk
D) AWS CodeBuild

<details><summary>उत्तर</summary>

**B) AWS CDK** — कोडले infrastructure लेख्ने, जुन पछि CloudFormation template मा compile हुन्छ।
</details>

---

## ✅ यो मोड्युलको सार

### Deployment सारांश
| सेवा | प्रकार | काम |
|---|---|---|
| **CloudFormation** | AWS only | **Infrastructure as Code** — लगभग सबै AWS संसाधनसँग काम गर्ने, Region र Account मा दोहोर्‍याउने |
| **Elastic Beanstalk** | AWS only | **Platform as a Service (PaaS)** — निश्चित भाषा वा Docker मा सीमित। ज्ञात आर्किटेक्चर (ALB + EC2 + RDS) मा एकरूप deploy |
| **CodeDeploy** | **Hybrid** | जुनसुकै server मा एप deploy र upgrade गर्ने |
| **Systems Manager** | **Hybrid** | ठूलो स्केलमा patch, configure र command चलाउने |

### Developer सेवा सारांश
| सेवा | काम |
|---|---|
| **CodeCommit** | निजी git repository मा कोड राख्ने (version controlled) |
| **CodeBuild** | AWS मा कोड build र test गर्ने |
| **CodeDeploy** | Server मा कोड deploy गर्ने |
| **CodePipeline** | Pipeline को orchestration (कोडदेखि build हुँदै deploy सम्म) |
| **CodeArtifact** | AWS मा software package / dependency राख्ने |
| **AWS CDK** | प्रोग्रामिङ भाषाले cloud infrastructure परिभाषित गर्ने |

---

⬅️ [मोड्युल ०८](08-other-compute.md) | ➡️ [मोड्युल १० — Global Infrastructure](10-global-infrastructure.md)
