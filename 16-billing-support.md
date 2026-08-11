# मोड्युल १६ — Account Management, Billing र Support

> ⭐⭐ **परीक्षामा धेरै भारी मोड्युल। CLF-C02 मा "Billing, Pricing and Support" १२% भार बोक्छ, तर Organizations र Trusted Advisor जस्ता विषय अन्य क्षेत्रमा पनि आउँछन्।**

---

## भाग १ — Account Management

## १. AWS Organizations ⭐⭐

**Organizations = धेरै AWS account व्यवस्थापन गर्ने Global सेवा।**

- मुख्य account लाई **master account** (वा management account) भनिन्छ

### लागतको फाइदा ⭐
- ⭐ **Consolidated Billing** — सबै account को एउटै भुक्तानी विधि
- ⭐ **जम्मा प्रयोगबाट मूल्यको फाइदा** (EC2, S3 आदिमा **volume discount**)
- ⭐ **Reserved EC2 instance को pooling** — उत्तम बचतको लागि

### अन्य फाइदा
- AWS account बनाउने काम automate गर्न **API** उपलब्ध
- **Service Control Policies (SCP)** ले account को अधिकार सीमित गर्ने

---

## २. Multi Account रणनीति ⭐

**कहिले धेरै account बनाउने?**
- प्रति विभाग, प्रति cost center
- प्रति dev / test / prod
- नियामकीय बन्धन अनुसार (SCP प्रयोग गरेर)
- **राम्रो resource isolation** को लागि (जस्तै VPC)
- **प्रति-account छुट्टै service limit** पाउन
- Logging को लागि छुट्टै account

**उत्तम अभ्यास:**
- Billing को लागि **tagging standard** प्रयोग गर्ने
- ⭐ **सबै account मा CloudTrail सक्षम गर्ने, log केन्द्रीय S3 account मा पठाउने**
- ⭐ **CloudWatch Logs केन्द्रीय logging account मा पठाउने**

### Organizational Unit (OU) — उदाहरण

OU बनाउने ३ तरिका: **Business Unit** अनुसार, **Environmental Lifecycle** अनुसार, वा **Project-based**

```
Root OU
├── Master Account
├── Prod OU
├── Dev OU
├── Finance OU
└── HR OU
```

---

## ३. Service Control Policies (SCP) ⭐⭐

- IAM action को **whitelist वा blacklist**
- ⭐ **OU वा Account level** मा लागू हुन्छ
- ⚠️ ⭐ **Master Account मा लागू हुँदैन!**
- SCP account का **सबै User र Role मा लागू हुन्छ — Root user सहित**
- ⭐ **SCP ले service-linked role लाई असर गर्दैन**
  - Service-linked role ले अन्य AWS सेवालाई AWS Organizations सँग एकीकृत हुन दिन्छ, SCP ले रोक्न सक्दैन
- ⚠️ ⭐ **SCP मा स्पष्ट Allow हुनैपर्छ** (पूर्वनिर्धारित रूपमा केही अनुमति दिँदैन)

**प्रयोग:**
- निश्चित सेवामा पहुँच रोक्ने (उदाहरण: EMR प्रयोग गर्न नदिने)
- सेवा स्पष्ट रूपमा असक्षम गरेर **PCI compliance लागू गर्ने**

---

## ४. AWS Organization — Consolidated Billing ⭐

सक्षम गर्दा पाइने:

| सुविधा | विवरण |
|---|---|
| **Combined Usage** | AWS Organization का सबै account को प्रयोग जोडेर **volume pricing, Reserved Instance र Savings Plans को छुट साझा** गर्ने |
| **One Bill** | Organization का सबै AWS Account को **एउटै बिल** |

⭐ **Management account ले Organization का कुनै पनि account (आफू सहित) को लागि Reserved Instance discount sharing बन्द गर्न सक्छ।**

**उदाहरण:**
```
AWS Organization
├── AWS Account A — Susan (५ Reserved EC2 Instances)
└── AWS Account B — Bob (कुनै Reserved EC2 Instance छैन)
    ↑ Bob ले Susan को नप्रयोग भएको Reserved Instance को छुट पाउन सक्छ
```

---

## ५. AWS Control Tower ⭐

- **उत्तम अभ्यासमा आधारित सुरक्षित र compliant multi-account AWS environment** सेटअप र शासन गर्ने सजिलो तरिका

**फाइदा:**
- केही क्लिकमै environment सेटअप स्वचालित गर्ने
- **Guardrail** प्रयोग गरेर निरन्तर policy व्यवस्थापन स्वचालित गर्ने
- **Policy उल्लङ्घन पत्ता लगाउने र सच्याउने**
- अन्तरक्रियात्मक dashboard बाट compliance निगरानी

⭐ **AWS Control Tower AWS Organizations माथि चल्छ** — यसले account व्यवस्थित गर्न र SCP लागू गर्न **AWS Organizations आफैं सेटअप गरिदिन्छ।**

> **सम्झने:** **Organizations = आधार। Control Tower = Organizations लाई उत्तम अभ्याससहित स्वचालित सेटअप गर्ने।**

---

## ६. AWS Resource Access Manager (AWS RAM) ⭐

- **तपाईंको स्वामित्वका AWS संसाधन अरू AWS account सँग साझा गर्ने**
- जुनसुकै account सँग वा आफ्नो Organization भित्र साझा गर्न मिल्ने
- ⭐ **संसाधन दोहोरिनबाट जोगिने!**

**समर्थित संसाधन:** Aurora, **VPC Subnets**, Transit Gateway, Route 53, EC2 Dedicated Hosts, License Manager Configurations

---

## ७. AWS Service Catalog ⭐

**समस्या:**
- AWS मा नयाँ प्रयोगकर्तासँग धेरै विकल्प हुन्छन्, र तिनले संस्थाको मापदण्ड नमिल्ने stack बनाउन सक्छन्
- कोही प्रयोगकर्ताले admin ले पहिल्यै तय गरेका अधिकृत उत्पादन launch गर्न **छिटो self-service portal** मात्र चाहन्छन्
- यसमा पर्ने: भर्चुअल मेसिन, डाटाबेस, storage विकल्प आदि

**👉 समाधान: AWS Service Catalog**

```
ADMIN का काम:                          USER का काम (IAM ले अधिकृत):
Product (CloudFormation Template)      Product List हेर्ने
    ↓                                       ↓ launch
Portfolio (Product को सङ्ग्रह)          Provisioned Products
    ↓                                   ✅ प्रयोग गर्न तयार
Portfolio पहुँचको IAM अनुमति            ✅ ठीकसँग Configure गरिएको
                                        ✅ ठीकसँग Tag गरिएको
```

> **परीक्षा टिप:** "प्रयोगकर्तालाई पूर्व-अनुमोदित stack मात्र launch गर्न दिने" → **Service Catalog**

---

## भाग २ — Pricing

## ८. AWS का ४ Pricing Model ⭐⭐

| # | Model | विवरण |
|---|---|---|
| १ | **Pay as you go** | जति प्रयोग गर्‍यो त्यति तिर्ने — चुस्त, प्रतिक्रियाशील, scale को माग पूरा गर्ने |
| २ | **Save when you reserve** | जोखिम कम, बजेट अनुमानयोग्य, दीर्घकालीन आवश्यकतासँग मिल्ने |
| ३ | **Pay less by using more** | **Volume-based discount** |
| ४ | **Pay less as AWS grows** | AWS बढ्दै जाँदा मूल्य घट्दै |

### Reservation उपलब्ध सेवाहरू ⭐
- **EC2 Reserved Instances**
- **DynamoDB Reserved Capacity**
- **ElastiCache Reserved Nodes**
- **RDS Reserved Instance**
- **Redshift Reserved Nodes**

---

## ९. AWS मा निःशुल्क सेवा र Free Plan ⭐

- नयाँ AWS account सँग **USD २०० सम्मको credit**
- **Free Plan वा Paid Plan** मध्ये छान्ने
  - **Free Plan** — **६ महिनामा** वा credit सकिँदा समाप्त (कुनै शुल्क छैन)
  - **Paid Plan** — credit सकिएपछि शुल्क लाग्ने
- दुवै Plan मा **Always Free Services** मा पहुँच

**मासिक निःशुल्क सीमाका उदाहरण:**
- **Lambda** — प्रति महिना १०,००,००० request र ४,००,००० GB-सेकेन्ड compute
- **DynamoDB** — २५ GB storage र प्रति महिना २० करोड request

*विवरण: https://aws.amazon.com/free/*

---

## १०. Compute Pricing ⭐

### EC2 को मूल्य केमा निर्भर?
- **Instance को सङ्ख्या**
- **Instance configuration:**
  - भौतिक क्षमता
  - **Region**
  - **OS र software**
  - Instance type
  - Instance size
- **ELB चलेको समय र प्रशोधन गरिएको डाटाको मात्रा**
- **Detailed monitoring**

### EC2 का purchasing option (मूल्यको दृष्टिकोणबाट)

| विकल्प | विवरण |
|---|---|
| **On-demand** | **न्यूनतम ६० सेकेन्ड**; प्रति सेकेन्ड (Linux/Windows) वा प्रति घण्टा (अन्य) |
| **Reserved** | On-demand भन्दा **७५% सम्म छुट**, १ वा ३ वर्ष प्रतिबद्धता; All/Partial/No upfront |
| **Spot** | **९०% सम्म छुट**, नप्रयोग भएको क्षमताको लागि बोलकबोल |
| **Dedicated Host** | On-demand वा १/३ वर्षको reservation |
| **Savings Plans** | निरन्तर प्रयोगमा बचतको विकल्प |

### Lambda र ECS को मूल्य ⭐

| सेवा | मूल्य |
|---|---|
| **Lambda** | **प्रति call** + **प्रति duration** |
| **ECS (EC2 Launch Type)** | कुनै अतिरिक्त शुल्क छैन — एप्लिकेसनमा बनाइएका/राखिएका **AWS संसाधनको मात्र** |
| **Fargate (Fargate Launch Type)** | Container मा **allocate गरिएको vCPU र memory** को |

---

## ११. Storage Pricing ⭐

### S3 को मूल्य केमा निर्भर?
- **Storage class** — S3 Standard, IA, One-Zone IA, Intelligent Tiering, Glacier, Glacier Deep Archive
- **Object को सङ्ख्या र आकार** (मात्रा अनुसार tiered हुन सक्ने)
- **Request को सङ्ख्या र प्रकार**
- **S3 region बाट बाहिर जाने डाटा (Data transfer OUT)**
- **S3 Transfer Acceleration**
- **Lifecycle transition**

*यस्तै सेवा: EFS (pay per use, infrequent access र lifecycle rule सहित)*

### EBS को मूल्य केमा निर्भर?
- **Volume type** (प्रदर्शन अनुसार)
- **प्रति महिना GB मा provision गरिएको storage**
- **IOPS:**
  - General Purpose SSD: **समावेश**
  - Provisioned IOPS SSD: provision गरिएको IOPS को
  - Magnetic: request को सङ्ख्या
- **Snapshot:** प्रति GB प्रति महिना थपिएको डाटाको मूल्य
- **Data transfer:** Outbound tiered (volume discount), **Inbound निःशुल्क**

---

## १२. Database Pricing — RDS ⭐

- **प्रति घण्टा billing**
- **Database का विशेषता:** Engine, Size, Memory class
- **Purchase type:** On-demand, Reserved instance (१ वा ३ वर्ष) वैकल्पिक up-front सहित
- ⭐ **Backup Storage: कुल database storage को १००% सम्म कुनै अतिरिक्त शुल्क छैन**
- अतिरिक्त storage (प्रति GB प्रति महिना)
- प्रति महिना input र output request को सङ्ख्या
- **Deployment type** (storage र I/O परिवर्तनशील): Single AZ / Multiple AZ
- **Data transfer:** Outbound tiered, **Inbound निःशुल्क**

---

## १३. CloudFront को मूल्य

- ⭐ **भौगोलिक क्षेत्र अनुसार मूल्य फरक**
- प्रत्येक edge location को लागि जम्मा गरेर बिलमा लागू
- **Data Transfer Out** (volume discount)
- **HTTP/HTTPS request को सङ्ख्या**

---

## १४. AWS मा Networking Cost ⭐⭐

**यो चित्र परीक्षामा उपयोगी छ:**

```
        Region A                           Region B
   ┌──────────────────┐               ┌──────────────┐
   │ AZ 1      AZ 2   │  $0.02/GB     │              │
   │  EC2 ←→ EC2      │ ──Inter-region──> │          │
   │  ↑ $0.01/GB      │               │              │
   │  (private IP)    │               └──────────────┘
   │                  │
   │  उही AZ भित्र:   │
   │  private IP → निःशुल्क │
   │  public/Elastic IP → $0.02/GB │
   └──────────────────┘

   भित्र आउने traffic (IN) → निःशुल्क
```

### ⭐ बचतका २ नियम
1. **Public IP को सट्टा Private IP प्रयोग गर्नुहोस्** — राम्रो बचत र राम्रो network प्रदर्शन
2. **अधिकतम बचतको लागि उही AZ प्रयोग गर्नुहोस्** — तर high availability गुम्छ

---

## १५. Savings Plan ⭐⭐

**१ वा ३ वर्षको लागि प्रति घण्टा निश्चित रकमको प्रतिबद्धता।**
⭐ **AWS मा दीर्घकालीन प्रतिबद्धता सेटअप गर्ने सबैभन्दा सजिलो तरिका।**

| प्रकार | छुट | दायरा |
|---|---|---|
| **EC2 Savings Plan** | **७२% सम्म** | एउटा region मा निश्चित **instance family** (जस्तै C5 वा M5) को प्रयोगमा प्रतिबद्धता। **AZ, size (m5.xl → m5.4xl), OS (Linux/Windows), tenancy जे भए पनि हुने** |
| **Compute Savings Plan** | **६६% सम्म** | **Family, Region, size, OS, tenancy, compute विकल्प जे भए पनि हुने।** Compute विकल्प: **EC2, Fargate, Lambda** |
| **Machine Learning Savings Plan** | — | SageMaker आदि |

- **AWS Cost Explorer console** बाट सेटअप गर्ने
- मूल्य अनुमान: https://aws.amazon.com/savingsplans/pricing/

> **सम्झने:** **EC2 Savings Plan = बढी छुट, कम लचकता। Compute Savings Plan = कम छुट, बढी लचकता (Fargate र Lambda सहित)।**

---

## भाग ३ — Cost Management र Monitoring

## १६. AWS Compute Optimizer ⭐

- Workload को लागि **उत्तम AWS संसाधन सिफारिस गरेर लागत घटाउने र प्रदर्शन सुधार्ने**
- ⭐ **Workload लाई right-size गर्न** मद्दत (over/under provisioned)
- **Machine Learning** ले संसाधनको configuration र CloudWatch metric को प्रयोग विश्लेषण गर्छ

**समर्थित संसाधन:**
- EC2 instances
- EC2 Auto Scaling Groups
- EBS volumes
- Lambda functions

- ⭐ **२५% सम्म लागत घटाउने**
- सिफारिस **S3 मा export** गर्न सकिने

---

## १७. Billing र Costing Tools — वर्गीकरण ⭐⭐

**यो वर्गीकरण परीक्षामा धेरै सहयोगी हुन्छ:**

| उद्देश्य | उपकरण |
|---|---|
| **क्लाउडमा लागत अनुमान गर्ने** | **Pricing Calculator** |
| **क्लाउडमा लागत ट्र्याक गर्ने** | **Billing Dashboard, Cost Allocation Tags, Cost and Usage Reports, Cost Explorer** |
| **लागत योजनाविरुद्ध निगरानी गर्ने** | **Billing Alarms, Budgets** |

---

## १८. AWS Pricing Calculator ⭐

- https://calculator.aws/
- ⭐ **आफ्नो solution architecture को लागत अनुमान गर्ने**
- ⚠️ **यो अनुमानको लागि हो — वास्तविक खर्च ट्र्याक गर्न होइन**

---

## १९. Cost Allocation Tags ⭐

**विस्तृत तहमा AWS लागत ट्र्याक गर्न tag प्रयोग गर्ने।**

| प्रकार | विवरण |
|---|---|
| **AWS generated tags** | तपाईंले बनाएको संसाधनमा **स्वचालित रूपमा** लाग्ने। उपसर्ग **`aws:`** ले सुरु (जस्तै `aws:createdBy`) |
| **User-defined tags** | प्रयोगकर्ताले परिभाषित गर्ने। उपसर्ग **`user:`** ले सुरु |

### Tagging र Resource Groups
- **Tag संसाधन व्यवस्थित गर्न** प्रयोग हुन्छ:
  - EC2: instance, image, load balancer, security group…
  - RDS, VPC संसाधन, Route 53, IAM user आदि
- **CloudFormation ले बनाएका संसाधन सबै एउटै तरिकाले tag हुन्छन्**
- नाम स्वतन्त्र — सामान्य tag: `Name`, `Environment`, `Team`
- ⭐ **Tag बाट Resource Group बनाउन सकिन्छ** — साझा tag भएका संसाधनको सङ्ग्रह बनाउने, मर्मत गर्ने र हेर्ने
- ती tag **Tag Editor** ले व्यवस्थापन गर्ने

---

## २०. Cost and Usage Reports ⭐

- AWS को लागत र प्रयोगमा **गहिरो जानकारी**
- ⭐ **उपलब्ध AWS लागत र प्रयोग डाटाको सबैभन्दा व्यापक सेट** — AWS सेवा, मूल्य र reservation (जस्तै EC2 RI) को थप metadata सहित
- Account र यसका IAM user ले प्रयोग गरेको प्रत्येक सेवा वर्गको AWS प्रयोग **घण्टा वा दैनिक line item** मा सूचीबद्ध गर्छ — cost allocation को लागि सक्रिय गरिएका tag सहित
- ⭐ **Athena, Redshift वा QuickSight सँग एकीकृत गर्न सकिने**

---

## २१. Cost Explorer ⭐⭐

- **समयसँगै AWS लागत र प्रयोग हेर्ने, बुझ्ने र व्यवस्थापन गर्ने**
- लागत र प्रयोग डाटा विश्लेषण गर्ने **custom रिपोर्ट** बनाउने
- उच्च तहमा विश्लेषण: सबै account को कुल लागत र प्रयोग
- वा **मासिक, घण्टा, संसाधन तहको granularity**
- ⭐ **उत्तम Savings Plan छान्ने** (बिल घटाउन)
- ⭐⭐ **पहिलेको प्रयोगमा आधारित रहेर १२ महिनासम्मको प्रयोग forecast गर्ने**

> **परीक्षा टिप:** **"भविष्यको खर्च forecast गर्ने" → Cost Explorer**

---

## २२. CloudWatch मा Billing Alarms ⭐

- ⭐ **Billing डाटा metric CloudWatch को `us-east-1` मा राखिन्छ**
- Billing डाटा **विश्वव्यापी कुल AWS लागत** को हो
- ⚠️ ⭐ **यो वास्तविक (actual) लागतको लागि हो, अनुमानित (projected) लागतको लागि होइन**
- साधारण alarm को लागि बनेको — **AWS Budgets जति शक्तिशाली होइन**

---

## २३. AWS Budgets ⭐⭐

- **Budget बनाएर लागत budget नाघ्दा alarm पठाउने**
- ⭐ **४ किसिमका budget: Usage, Cost, Reservation, Savings Plans**
- Reserved Instance (RI) को लागि:
  - **Utilization ट्र्याक** गर्ने
  - समर्थन: **EC2, ElastiCache, RDS, Redshift**
- ⭐ **प्रति budget ५ SNS notification सम्म**
- Filter गर्न सकिने: **Service, Linked Account, Tag, Purchase Option, Instance Type, Region, Availability Zone, API Operation** आदि
- ⭐ **AWS Cost Explorer कै विकल्पहरू!**

> **Billing Alarm vs Budgets:** Billing Alarm साधारण छ (actual cost मात्र)। **Budgets बढी शक्तिशाली** — usage, cost, reservation, savings plan चारै ट्र्याक गर्छ र forecast मा पनि alert दिन्छ।

---

## २४. AWS Cost Anomaly Detection ⭐

- ⭐ **Machine Learning प्रयोग गरेर असामान्य खर्च निरन्तर पत्ता लगाउने**
- तपाईंको अद्वितीय ऐतिहासिक खर्चको ढाँचा सिकेर **एक-पटकको खर्च उछाल र/वा निरन्तर लागत वृद्धि** पत्ता लगाउँछ
- ⭐ **तपाईंले कुनै threshold परिभाषित गर्नुपर्दैन!**
- निगरानी गर्न सकिने: AWS सेवा, member account, cost allocation tag, वा cost category
- **मूल-कारण विश्लेषण सहितको anomaly detection रिपोर्ट** पठाउँछ
- व्यक्तिगत alert वा दैनिक/साप्ताहिक सारांश (SNS मार्फत)

> **परीक्षा टिप:** **"ML ले असामान्य खर्च पत्ता लगाउने, threshold तोक्नु नपर्ने" → Cost Anomaly Detection**

---

## २५. AWS Service Quotas ⭐

- **Service quota को सीमा नजिक पुग्दा सूचित गर्ने**
- Service Quotas console बाट **CloudWatch Alarm बनाउने**
- उदाहरण: **Lambda concurrent executions**
- **AWS Service Quotas बाट quota वृद्धिको अनुरोध गर्ने**, वा सीमा पुग्नुअघि संसाधन बन्द गर्ने

---

## २६. AWS Trusted Advisor ⭐⭐

- ⭐ **केही install गर्नुपर्दैन** — उच्च-स्तरीय AWS account मूल्याङ्कन
- AWS account विश्लेषण गरेर **६ वर्गमा सिफारिस** दिने ⭐⭐:

| # | वर्ग |
|---|---|
| १ | **Cost optimization** |
| २ | **Performance** |
| ३ | **Security** |
| ४ | **Fault tolerance** |
| ५ | **Service limits** |
| ६ | **Operational Excellence** |

⭐ **Business र Enterprise Support plan मा:**
- **पूर्ण जाँचको सेट (Full Set of Checks)**
- **AWS Support API प्रयोग गरेर programmatic पहुँच**

⚠️ **Basic/Developer plan मा ७ core check मात्र।**

---

## भाग ४ — AWS Support Plans

## २७. AWS Support Plans ⭐⭐

**Basic Support: निःशुल्क**

### 🆓 AWS Basic Support Plan
- **Customer Service र Communities** — २४x७ customer service, documentation, whitepaper र support forum मा पहुँच
- **AWS Trusted Advisor** — ⭐ **७ core Trusted Advisor check** मा पहुँच, प्रदर्शन र सुरक्षा सुधार्न उत्तम अभ्यास अनुसार संसाधन provision गर्ने मार्गदर्शन
- **AWS Personal Health Dashboard** — AWS सेवाको स्वास्थ्यको व्यक्तिगत दृश्य र तपाईंका संसाधन प्रभावित हुँदा alert

### 💼 AWS Business Support+ Plan (२४/७) ⭐
- ⭐ **Production workload भएमा प्रयोग गर्ने**
- **Generative AI** मार्फत real-time र सन्दर्भगत जवाफ
- ⭐ **Trusted Advisor — पूर्ण जाँचको सेट + API पहुँच**
- **२४x७ फोन, वेब र chat बाट Cloud Support Engineer मा पहुँच**
- **असीमित case / असीमित सम्पर्क**
- ⭐ **Business-critical system down case मा अधिकतम ३० मिनेट पर्खनुपर्ने**
- ⭐ **तेस्रो पक्षको software support** (जस्तै EC2 को Ubuntu जस्ता OS)

### 🏢 AWS Enterprise Support Plan (२४/७) ⭐
- ⭐ **Production वा business critical workload भएमा**
- Business Support+ Plan को सबै सुविधा
- ⭐⭐ **तोकिएको Technical Account Manager (TAM) मा पहुँच**
- ⭐⭐ **Production-critical case मा १५ मिनेट भन्दा कम प्रतिक्रिया**
- **AWS Security Incident Response** मा पहुँच (security incident बाट उठ्न सहयोग)
- AWS विज्ञबाट **business review**
- **AWS Countdown event management** मा पहुँच (महत्त्वपूर्ण व्यावसायिक घटनामा TAM-नेतृत्वको विशेष सहयोग)

### 🎯 AWS Unified Operations Support Plan (२४/७)
- ⭐ **Mission critical workload भएमा**
- Business Support Plan को सबै +
- **Application Architecture Guidance** — तपाईंको प्रयोग र workload अनुसार architecture design गर्न सहयोग
- गहिरो बुझाइ, विश्लेषण र architectural मार्गदर्शनको लागि AWS Support सँग छोटो अवधिको engagement
- **तोकिएको पहुँच:**
  - Technical Account Manager (TAM), Domain Specialist Engineer (DSE)
  - Senior Billing and Account Specialist (SBAS), Incident Management Engineer (IME)
  - Migration Specialist (on-demand), Specialist Support Engineer (SSE)
- **AWS Countdown Premium र AWS Customer Incident Response Team (CIRT)** मा पहुँच
- Critical workload समीक्षा, operational procedure

### ⏱️ प्रतिक्रिया समय — सारांश ⭐⭐

| Plan | प्रतिक्रिया समय |
|---|---|
| **Developer** | General guidance: **< २४ business hours**; System impaired: **< १२ business hours** |
| **Business** | Production system impaired: **< ४ घण्टा**; Production system down: **< १ घण्टा** |
| **Enterprise** | Business-critical system down: **< १५ मिनेट** + **TAM** |

> ⭐ **परीक्षामा सबैभन्दा धेरै सोधिने:** **TAM (Technical Account Manager) = Enterprise Support Plan मा मात्र**

---

## 📝 अभ्यास प्रश्नहरू

**प्रश्न १.** एउटा कम्पनीसँग ५ वटा AWS account छन्। तिनले एउटै बिल पाउन र volume discount साझा गर्न चाहन्छन्। कुन सेवा?

A) AWS Control Tower
B) AWS Organizations (Consolidated Billing)
C) AWS Budgets
D) AWS Cost Explorer

<details><summary>उत्तर</summary>

**B) AWS Organizations** — Consolidated Billing ले एउटै बिल, volume discount र RI pooling दिन्छ।
</details>

---

**प्रश्न २.** अर्को ६ महिनामा AWS को खर्च कति होला भनेर forecast गर्न कुन उपकरण?

A) AWS Pricing Calculator
B) AWS Cost Explorer
C) AWS Budgets
D) CloudWatch Billing Alarm

<details><summary>उत्तर</summary>

**B) AWS Cost Explorer** — पहिलेको प्रयोगमा आधारित रहेर १२ महिनासम्म forecast गर्छ। (Pricing Calculator नयाँ architecture को अनुमान गर्न हो।)
</details>

---

**प्रश्न ३.** Technical Account Manager (TAM) कुन Support Plan मा पाइन्छ?

A) Basic
B) Developer
C) Business
D) Enterprise

<details><summary>उत्तर</summary>

**D) Enterprise** — TAM Enterprise (र Unified Operations) मा मात्र।
</details>

---

**प्रश्न ४.** Organization का सबै account मा कुनै पनि account ले EMR सेवा प्रयोग गर्न नपाओस् भन्ने चाहनुहुन्छ। के प्रयोग गर्ने?

A) IAM Policy
B) Service Control Policy (SCP)
C) Bucket Policy
D) Security Group

<details><summary>उत्तर</summary>

**B) SCP** — OU वा Account level मा IAM action को whitelist/blacklist। ⚠️ Master account मा लागू हुँदैन।
</details>

---

**प्रश्न ५.** Threshold तोक्नु नपरी ML ले असामान्य खर्च पत्ता लगाओस् भन्ने चाहनुहुन्छ। कुन सेवा?

A) AWS Budgets
B) CloudWatch Billing Alarm
C) AWS Cost Anomaly Detection
D) AWS Trusted Advisor

<details><summary>उत्तर</summary>

**C) AWS Cost Anomaly Detection** — ML ले ऐतिहासिक ढाँचा सिकेर threshold बिनै anomaly पत्ता लगाउँछ।
</details>

---

**प्रश्न ६.** Trusted Advisor ले कुन-कुन वर्गमा सिफारिस दिन्छ? (तीन छान्नुहोस्)

A) Cost optimization
B) Security
C) Data migration
D) Fault tolerance
E) Machine learning

<details><summary>उत्तर</summary>

**A), B), D)** — ६ वर्ग: Cost optimization, Performance, Security, Fault tolerance, Service limits, Operational Excellence।
</details>

---

**प्रश्न ७.** प्रयोगकर्ताहरूले admin ले पहिल्यै अनुमोदन गरेका पूर्व-configured stack मात्र launch गर्न पाऊन् भन्ने चाहनुहुन्छ। कुन सेवा?

A) AWS CloudFormation
B) AWS Service Catalog
C) AWS Control Tower
D) AWS Config

<details><summary>उत्तर</summary>

**B) AWS Service Catalog** — admin ले Portfolio मा Product तयार पार्ने, user ले self-service portal बाट launch गर्ने।
</details>

---

**प्रश्न ८.** CloudWatch मा billing metric कुन region मा उपलब्ध छ?

A) सबै region
B) us-east-1
C) us-west-2
D) प्रयोगकर्ताले छान्ने

<details><summary>उत्तर</summary>

**B) us-east-1**
</details>

---

**प्रश्न ९.** EC2, Fargate र Lambda तीनैमा छुट पाउने Savings Plan कुन हो?

A) EC2 Savings Plan
B) Compute Savings Plan
C) Machine Learning Savings Plan
D) Reserved Instance

<details><summary>उत्तर</summary>

**B) Compute Savings Plan** — ६६% सम्म छुट, EC2/Fargate/Lambda सबैमा, family र region जे भए पनि।
</details>

---

**प्रश्न १०.** नेटवर्क लागत घटाउन कुन उत्तम अभ्यास हो?

A) Public IP प्रयोग गर्ने
B) Private IP प्रयोग गर्ने र सम्भव भए उही AZ मा राख्ने
C) सधैं फरक region प्रयोग गर्ने
D) Elastic IP धेरै बनाउने

<details><summary>उत्तर</summary>

**B)** — Private IP मा traffic सस्तो/निःशुल्क हुन्छ र network प्रदर्शन पनि राम्रो हुन्छ। तर उही AZ मा राख्दा high availability गुम्छ भन्ने ध्यान राख्नुहोस्।
</details>

---

## ✅ यो मोड्युलको सार

### Account को उत्तम अभ्यास ⭐
- **Organizations** ले धेरै account सञ्चालन गर्ने
- **SCP** ले account को अधिकार सीमित गर्ने
- **AWS Control Tower** ले उत्तम अभ्याससहित धेरै account सजिलै सेटअप गर्ने
- **Tags र Cost Allocation Tags** ले व्यवस्थापन र billing सजिलो बनाउने
- **IAM मार्गदर्शन:** MFA, least-privilege, password policy, password rotation
- **Config** ले सबै संसाधनको configuration र compliance समयसँगै रेकर्ड गर्ने
- **CloudFormation** ले account र region मा stack deploy गर्ने
- **Trusted Advisor** ले अन्तर्दृष्टि दिने; आवश्यकता अनुसारको **Support Plan**
- Service Log र Access Log **S3 वा CloudWatch Logs** मा पठाउने
- **CloudTrail** ले account भित्रका API call रेकर्ड गर्ने
- ⚠️ **Account compromised भयो भने:** root password बदल्ने, सबै password/key मेटाउने र rotate गर्ने, AWS support लाई सम्पर्क गर्ने
- **AWS Service Catalog** ले admin-परिभाषित stack मात्र user लाई बनाउन दिने

### Billing र Costing उपकरण ⭐
| उपकरण | काम |
|---|---|
| **Compute Optimizer** | लागत घटाउन संसाधनको configuration सिफारिस |
| **Pricing Calculator** | AWS सेवाको लागत **अनुमान** |
| **Billing Dashboard** | उच्च-स्तरीय सिंहावलोकन |
| **Cost Allocation Tags** | Tag लगाएर विस्तृत रिपोर्ट बनाउने |
| **Cost and Usage Reports** | **सबैभन्दा व्यापक** billing dataset |
| **Cost Explorer** | वर्तमान प्रयोग (विस्तृत) र **forecast** |
| **Billing Alarms** | **us-east-1** मा — समग्र र प्रति-सेवा billing ट्र्याक |
| **Budgets** | बढी उन्नत — usage, cost, RI ट्र्याक र alert |
| **Savings Plans** | दीर्घकालीन प्रयोगमा बचतको सजिलो तरिका |
| **Cost Anomaly Detection** | **ML** ले असामान्य खर्च पत्ता लगाउने |
| **Service Quotas** | Service quota को सीमा नजिक पुग्दा सूचित गर्ने |

---

⬅️ [मोड्युल १५](15-machine-learning.md) | ➡️ [मोड्युल १७ — Advanced Identity](17-advanced-identity.md)
