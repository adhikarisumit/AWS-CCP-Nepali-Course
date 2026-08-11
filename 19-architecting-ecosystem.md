# मोड्युल १९ — Well-Architected Framework, CAF र AWS Ecosystem

> ⭐⭐ **परीक्षामा धेरै महत्त्वपूर्ण — विशेषगरी Well-Architected Framework का ६ Pillar र AWS CAF का ६ Perspective।**

---

## १. Well-Architected Framework — सामान्य मार्गदर्शक सिद्धान्त ⭐

- **क्षमताको अनुमान लगाउन छोड्नुहोस्** (Stop guessing your capacity needs)
- **Production scale मै सिस्टम परीक्षण गर्नुहोस्**
- Architectural प्रयोग सजिलो बनाउन **automate गर्नुहोस्**
- **विकासशील (evolutionary) architecture** लाई ठाउँ दिनुहोस्
- **बदलिँदो आवश्यकता अनुसार डिजाइन** गर्नुहोस्
- **डाटाले architecture चलाउनुहोस्**
- **Game day** मार्फत सुधार गर्नुहोस्
- **Flash sale** जस्ता दिनको लागि एप्लिकेसन simulate गर्नुहोस्

---

## २. AWS Cloud का उत्तम अभ्यास — Design Principles ⭐⭐

| सिद्धान्त | विवरण |
|---|---|
| **Scalability** | Vertical र Horizontal |
| **Disposable Resources** | Server **फालिने (disposable)** र सजिलै configure गर्न मिल्ने हुनुपर्छ |
| **Automation** | Serverless, Infrastructure as Code, Auto Scaling… |
| **Loose Coupling** | **Monolith** = समयसँगै झन्-झन् ठूलो हुँदै जाने एप्लिकेसन। यसलाई **साना, loosely coupled component** मा टुक्र्याउनुहोस्। ⭐ **एउटा component मा भएको परिवर्तन वा असफलता अरूमा सर्नु हुँदैन** |
| **Services, not Servers** | ⭐ **केवल EC2 मात्र प्रयोग नगर्नुहोस्!** Managed सेवा, database, serverless प्रयोग गर्नुहोस् |

---

## ३. Well-Architected Framework — ६ Pillar ⭐⭐⭐

**यो परीक्षामा सबैभन्दा धेरै सोधिने सूची हो।**

| # | Pillar | नेपालीमा |
|---|---|---|
| १ | **Operational Excellence** | परिचालन उत्कृष्टता |
| २ | **Security** | सुरक्षा |
| ३ | **Reliability** | भरपर्दोपन |
| ४ | **Performance Efficiency** | प्रदर्शन दक्षता |
| ५ | **Cost Optimization** | लागत अनुकूलन |
| ६ | **Sustainability** | दिगोपन |

> ⚠️ ⭐ **महत्त्वपूर्ण:** यी **सन्तुलन गर्ने वा trade-off गर्ने कुरा होइनन् — यी एकआपसमा सहकार्य (synergy) गर्छन्।** यो कथन परीक्षामा सोधिन्छ।

> **सम्झने तरिका:** **O-S-R-P-C-S** → "**O**perational **S**ecurity **R**eliably **P**erforms **C**ost-**S**ustainably"

---

### 1️⃣ Operational Excellence ⭐

**परिभाषा:** व्यावसायिक मूल्य दिन सिस्टम चलाउने र निगरानी गर्ने, र सहयोगी प्रक्रिया निरन्तर सुधार गर्ने क्षमता।

**Design Principles:**
- **Perform operations as code** — Infrastructure as code
- **बारम्बार, साना, उल्ट्याउन मिल्ने परिवर्तन गर्नुहोस्** — असफल भए उल्ट्याउन सकियोस्
- **Operations procedure बारम्बार परिष्कृत गर्नुहोस्** — टिमका सदस्य त्यसमा अभ्यस्त छन् सुनिश्चित गर्नुहोस्
- **असफलताको पूर्वानुमान गर्नुहोस्**
- **सबै परिचालन असफलताबाट सिक्नुहोस्**
- **Managed सेवा प्रयोग गर्नुहोस्** — परिचालन भार घटाउन
- **कदम चाल्न मिल्ने अन्तर्दृष्टिको लागि observability लागू गर्नुहोस्** — प्रदर्शन, भरपर्दोपन, लागत…

**सम्बन्धित AWS सेवा:**
- **Prepare:** CloudFormation, Config
- **Operate:** CloudFormation, Config, CloudTrail, CloudWatch, X-Ray
- **Evolve:** CloudFormation, CodeBuild, CodeCommit, CodeDeploy, CodePipeline

---

### 2️⃣ Security ⭐

**परिभाषा:** जोखिम मूल्याङ्कन र न्यूनीकरण रणनीति मार्फत व्यावसायिक मूल्य दिँदै सूचना, सिस्टम र सम्पत्तिको सुरक्षा गर्ने क्षमता।

**Design Principles:**
- ⭐ **बलियो identity foundation लागू गर्नुहोस्** — विशेषाधिकार व्यवस्थापन केन्द्रीकृत गर्ने, दीर्घकालीन credential मा निर्भरता घटाउने (वा हटाउने), **least privilege को सिद्धान्त — IAM**
- **Traceability सक्षम गर्नुहोस्** — log र metric सिस्टमसँग एकीकृत गरेर स्वचालित प्रतिक्रिया
- ⭐ **सबै तहमा सुरक्षा लागू गर्नुहोस्** — edge network, VPC, subnet, load balancer, हरेक instance, OS र एप्लिकेसन
- **सुरक्षा उत्तम अभ्यास स्वचालित गर्नुहोस्**
- ⭐ **Transit र rest दुवैमा डाटा जोगाउनुहोस्** — encryption, tokenization, access control
- **मानिसलाई डाटाबाट टाढा राख्नुहोस्** — प्रत्यक्ष पहुँच वा manual प्रशोधनको आवश्यकता घटाउने वा हटाउने
- **Security event को तयारी गर्नुहोस्** — incident response simulation चलाउने र automation भएका उपकरण प्रयोग गरेर पत्ता लगाउने, अनुसन्धान गर्ने र सुधार गर्ने गति बढाउने
- ⭐ **Shared Responsibility Model**

**सम्बन्धित AWS सेवा:**
- **Identity and Access Management:** IAM, AWS-STS, MFA token, AWS Organizations
- **Detective Controls:** Config, CloudTrail, CloudWatch
- **Infrastructure Protection:** CloudFront, VPC, Shield, WAF, Inspector
- **Data Protection:** KMS, S3, ELB, EBS, RDS
- **Incident Response:** IAM, CloudFormation, CloudWatch Events

---

### 3️⃣ Reliability ⭐

**परिभाषा:** Infrastructure वा सेवाको बाधाबाट उठ्ने, माग पूरा गर्न गतिशील रूपमा computing संसाधन प्राप्त गर्ने, र गलत configuration वा क्षणिक network समस्या जस्ता बाधा कम गर्ने क्षमता।

**Design Principles:**
- ⭐ **Recovery procedure परीक्षण गर्नुहोस्** — Automation प्रयोग गरेर विभिन्न असफलता simulate गर्ने वा पहिले भएका असफलताको परिदृश्य पुनःसिर्जना गर्ने
- ⭐ **असफलताबाट स्वचालित रूपमा उठ्नुहोस्** — असफलता हुनुअघि नै पूर्वानुमान गरेर सुधार्ने
- ⭐ **समग्र सिस्टम उपलब्धता बढाउन horizontally scale गर्नुहोस्** — request धेरै साना संसाधनमा बाँड्ने ताकि साझा असफलताको बिन्दु नहोस्
- ⭐ **क्षमताको अनुमान लगाउन छोड्नुहोस्** — बढी वा कम provision नगरी माग पूरा गर्ने उत्तम तह कायम राख्ने — **Auto Scaling प्रयोग गर्नुहोस्**
- **Automation मा परिवर्तन व्यवस्थापन गर्नुहोस्** — infrastructure मा परिवर्तन गर्न automation प्रयोग गर्ने

**सम्बन्धित AWS सेवा:**
- **Foundations:** Service Quotas, IAM, VPC, Trusted Advisor
- **Change Management:** CloudWatch, CloudTrail, Config, Auto Scaling
- **Failure Management:** Backups, CloudFormation, S3, S3 Glacier, Route 53

---

### 4️⃣ Performance Efficiency ⭐

**परिभाषा:** सिस्टमको आवश्यकता पूरा गर्न computing संसाधन कुशलतापूर्वक प्रयोग गर्ने, र माग बदलिँदा र प्रविधि विकसित हुँदा त्यो दक्षता कायम राख्ने क्षमता।

**Design Principles:**
- ⭐ **उन्नत प्रविधिलाई लोकतान्त्रिक बनाउनुहोस्** — उन्नत प्रविधि सेवा बन्छन्, त्यसैले तपाईं उत्पादन विकासमा बढी ध्यान दिन सक्नुहुन्छ
- ⭐ **मिनेटमै global जानुहोस्** — धेरै region मा सजिलो deployment
- ⭐ **Serverless architecture प्रयोग गर्नुहोस्** — server व्यवस्थापनको भार टार्ने
- **बारम्बार प्रयोग गर्नुहोस्** — तुलनात्मक परीक्षण गर्न सजिलो
- **Mechanical sympathy** — सबै AWS सेवाबारे जानकार हुनुहोस्

**सम्बन्धित AWS सेवा:** Auto Scaling, EBS, S3, Lambda, RDS, CloudFormation, CloudWatch, ElastiCache, Snowball, CloudFront

---

### 5️⃣ Cost Optimization ⭐

**परिभाषा:** सबैभन्दा कम मूल्यमा व्यावसायिक मूल्य दिने गरी सिस्टम चलाउने क्षमता।

**Design Principles:**
- ⭐ **Consumption model अपनाउनुहोस्** — जति प्रयोग गर्नुहुन्छ त्यति मात्र तिर्नुहोस्
- **समग्र दक्षता नाप्नुहोस्** — CloudWatch प्रयोग गर्नुहोस्
- ⭐ **Data center operation मा पैसा खर्च गर्न छोड्नुहोस्** — AWS ले infrastructure सम्हाल्छ, तपाईं संस्थाका project मा ध्यान दिन सक्नुहुन्छ
- ⭐ **खर्चको विश्लेषण र श्रेय दिनुहोस्** — सिस्टमको प्रयोग र लागतको सही पहिचानले ROI नाप्न मद्दत गर्छ — **Tag प्रयोग गर्न नबिर्सनुहोस्**
- ⭐ **स्वामित्वको लागत घटाउन managed र application-level सेवा प्रयोग गर्नुहोस्** — managed सेवा cloud scale मा चल्ने भएकोले प्रति-transaction वा प्रति-सेवा कम लागत दिन सक्छन्

**सम्बन्धित AWS सेवा:**
- **Expenditure Awareness:** Budgets, Cost and Usage Report, Cost Explorer, Reserved Instance Reporting
- **Cost-Effective Resources:** Spot instance, Reserved instance, S3 Glacier
- **Matching supply and demand:** Auto Scaling, Lambda
- **Optimizing Over Time:** Trusted Advisor, Cost and Usage Report

---

### 6️⃣ Sustainability ⭐

**परिभाषा:** क्लाउड workload चलाउँदा हुने **वातावरणीय प्रभाव न्यूनीकरण** मा केन्द्रित।

**Design Principles:**
- ⭐ **आफ्नो प्रभाव बुझ्नुहोस्** — प्रदर्शन सूचक स्थापना गर्ने, सुधारको मूल्याङ्कन गर्ने
- ⭐ **दिगोपनका लक्ष्य तय गर्नुहोस्** — प्रत्येक workload को लागि दीर्घकालीन लक्ष्य, ROI को model बनाउने
- ⭐ **उपयोग अधिकतम बनाउनुहोस्** — प्रत्येक workload right-size गरेर तल्लो hardware को ऊर्जा दक्षता बढाउने र निष्क्रिय संसाधन घटाउने
- ⭐ **नयाँ, बढी कुशल hardware र software को पूर्वानुमान र अपनाउनुहोस्** — समयसँगै नयाँ प्रविधि अपनाउन लचिलो डिजाइन गर्ने
- ⭐ **Managed सेवा प्रयोग गर्नुहोस्** — साझा सेवाले infrastructure को मात्रा घटाउँछ; managed सेवाले कम प्रयोग हुने डाटा cold storage मा सार्ने र compute क्षमता मिलाउने जस्ता दिगो अभ्यास स्वचालित गर्छ
- ⭐ **आफ्नो क्लाउड workload को downstream प्रभाव घटाउनुहोस्** — सेवा प्रयोग गर्न लाग्ने ऊर्जा/संसाधन घटाउने र ग्राहकलाई यन्त्र upgrade गर्नुपर्ने आवश्यकता घटाउने

**सम्बन्धित AWS सेवा:**
- EC2 Auto Scaling, Serverless (**Lambda, Fargate**)
- Cost Explorer, **AWS Graviton 2**, EC2 T instances, **Spot Instances**
- **EFS-IA, S3 Glacier, EBS Cold HDD volumes**
- **S3 Lifecycle Configurations, S3 Intelligent Tiering**
- **Amazon Data Lifecycle Manager**
- **Read Local, Write Global:** RDS Read Replicas, Aurora Global DB, DynamoDB Global Table, CloudFront

---

## ४. AWS Well-Architected Tool ⭐

- ⭐ **निःशुल्क उपकरण** — Well-Architected Framework का ६ pillar विरुद्ध आफ्नो architecture समीक्षा गर्ने र architectural उत्तम अभ्यास अपनाउने

**कसरी काम गर्छ:**
1. आफ्नो workload छान्नुहोस् र प्रश्नको उत्तर दिनुहोस्
2. ६ pillar विरुद्ध आफ्ना उत्तर समीक्षा गर्नुहोस्
3. सल्लाह पाउनुहोस् — भिडियो र documentation, रिपोर्ट बनाउने, dashboard मा नतिजा हेर्ने

*Console: https://console.aws.amazon.com/wellarchitected*

---

## ५. AWS Customer Carbon Footprint Tool ⭐

- AWS को प्रयोगबाट उत्पन्न **कार्बन उत्सर्जन** ट्र्याक, माप, समीक्षा र forecast गर्ने
- ⭐ **आफ्नै दिगोपनका लक्ष्य पूरा गर्न सहयोग**

देखाउने कुरा: भूगोल र सेवा अनुसार कार्बन उत्सर्जन, समयसँगै उत्सर्जन, १००% नवीकरणीय ऊर्जातर्फको बाटो

---

## ६. AWS Cloud Adoption Framework (AWS CAF) ⭐⭐

**AWS CAF = AWS को नवीन प्रयोग मार्फत आफ्नो डिजिटल रूपान्तरणको लागि व्यापक योजना बनाउन र लागू गर्न सहयोग गर्ने framework।**

- AWS Professionals ले AWS उत्तम अभ्यास र हजारौं ग्राहकबाट सिकेका पाठ प्रयोग गरेर बनाएको
- ⭐ **सफल cloud रूपान्तरणको आधार बन्ने विशेष सांगठनिक क्षमताहरू पहिचान गर्छ**

### CAF का ६ Perspective ⭐⭐⭐

**यो परीक्षामा पक्कै सोधिन्छ:**

#### 💼 व्यावसायिक क्षमता (Business Capabilities)

| Perspective | विवरण |
|---|---|
| **१. Business** | तपाईंको क्लाउड लगानीले **डिजिटल रूपान्तरणको महत्त्वाकाङ्क्षा र व्यावसायिक नतिजा** छिटो पुर्‍याओस् भन्ने सुनिश्चित गर्ने |
| **२. People** | ⭐ **प्रविधि र व्यवसाय बीचको पुल** — संस्थाले निरन्तर वृद्धि, सिकाइ र परिवर्तनलाई सामान्य मान्ने संस्कृतिमा छिटो विकास होस् भन्ने। **संस्कृति, सांगठनिक संरचना, नेतृत्व र कर्मचारी** मा केन्द्रित |
| **३. Governance** | क्लाउड पहल **orchestrate** गर्न सहयोग — सांगठनिक फाइदा अधिकतम बनाउँदै रूपान्तरणसँग सम्बन्धित जोखिम न्यूनतम बनाउने |

#### 🔧 प्राविधिक क्षमता (Technical Capabilities)

| Perspective | विवरण |
|---|---|
| **४. Platform** | **Enterprise-grade, scalable, hybrid cloud platform** बनाउने; अवस्थित workload आधुनिकीकरण गर्ने; नयाँ cloud-native समाधान लागू गर्ने |
| **५. Security** | आफ्नो डाटा र cloud workload को **गोपनीयता, अखण्डता र उपलब्धता** प्राप्त गर्ने |
| **६. Operations** | आफ्ना cloud सेवा **व्यवसायको आवश्यकता पूरा हुने तहमा** डेलिभर होस् भन्ने सुनिश्चित गर्ने |

> **सम्झने तरिका:** **B-P-G** (व्यावसायिक) + **P-S-O** (प्राविधिक)
> *"**B**usiness **P**eople **G**overnance — **P**latform **S**ecurity **O**perations"*

---

## ७. AWS CAF — Transformation Domains ⭐

| Domain | विवरण |
|---|---|
| **Technology** | Legacy infrastructure, एप्लिकेसन, डाटा र analytics platform migrate र आधुनिकीकरण गर्न क्लाउड प्रयोग गर्ने |
| **Process** | व्यावसायिक सञ्चालन **डिजिटाइज, स्वचालित र अनुकूलित** गर्ने — नयाँ data र analytics platform ले कदम चाल्न मिल्ने अन्तर्दृष्टि, ML ले ग्राहक सेवा सुधार्ने |
| **Organization** | **Operating model पुनर्कल्पना गर्ने** — उत्पादन र value stream वरिपरि टिम व्यवस्थित गर्ने, agile विधि प्रयोग गरेर छिटो iterate गर्ने |
| **Product** | नयाँ **value proposition (उत्पादन र सेवा)** र revenue model बनाएर व्यावसायिक model पुनर्कल्पना गर्ने |

---

## ८. AWS CAF — Transformation Phases ⭐⭐

**यी ४ चरण क्रममा याद गर्नुहोस्:**

| # | Phase | विवरण |
|---|---|---|
| १ | **Envision** (परिकल्पना) | रूपान्तरणका अवसर पहिचान गरेर क्लाउडले व्यावसायिक नतिजा कसरी छिटो ल्याउँछ देखाउने, र डिजिटल रूपान्तरणको जग बसाल्ने |
| २ | **Align** (मिलान) | **६ AWS CAF Perspective मा क्षमताको खाडल पहिचान गर्ने** — जसबाट **Action Plan** बन्छ |
| ३ | **Launch** (सुरुवात) | Production मा pilot initiative बनाएर डेलिभर गर्ने र क्रमिक व्यावसायिक मूल्य देखाउने |
| ४ | **Scale** (विस्तार) | Pilot initiative लाई चाहिएको scale सम्म विस्तार गर्ने र चाहिएको व्यावसायिक फाइदा प्राप्त गर्ने |

> **सम्झने: E-A-L-S** → **E**nvision → **A**lign → **L**aunch → **S**cale

---

## ९. AWS Right Sizing ⭐⭐

- EC2 मा धेरै instance type छन्, तर **सबैभन्दा शक्तिशाली छान्नु उत्तम विकल्प होइन** — किनभने क्लाउड elastic छ
- ⭐ **Right sizing = instance type र size लाई workload को प्रदर्शन र क्षमताको आवश्यकतासँग सबैभन्दा कम मूल्यमा मिलाउने प्रक्रिया**
- ⭐ **Scale up गर्न सजिलो छ, त्यसैले सधैं सानोबाट सुरु गर्नुहोस्**
- यो deploy भइसकेका instance हेरेर **क्षमता वा अन्य आवश्यकतामा सम्झौता नगरी हटाउने वा घटाउने अवसर** पहिचान गर्ने प्रक्रिया पनि हो — जसले लागत घटाउँछ

### कहिले Right Size गर्ने? ⭐
- ⭐ **Cloud Migration अघि**
- ⭐ **Cloud onboarding पछि निरन्तर** (आवश्यकता समयसँगै बदलिन्छ)

**सहयोगी उपकरण:** **CloudWatch, Cost Explorer, Trusted Advisor**, तेस्रो पक्षका उपकरण
*(र मोड्युल १६ मा पढेको **AWS Compute Optimizer**)*

---

## १०. AWS Ecosystem — निःशुल्क स्रोतहरू ⭐

| स्रोत | लिङ्क |
|---|---|
| **AWS Blogs** | https://aws.amazon.com/blogs/aws/ |
| **AWS Forums** (community) | https://forums.aws.amazon.com/index.jspa |
| **AWS Whitepapers र Guides** | https://aws.amazon.com/whitepapers |
| **AWS Solutions Library** (पहिलेको Quick Starts) | https://aws.amazon.com/solutions/ — AWS Cloud को लागि जाँचिएका प्रविधि समाधान |

---

## ११. AWS Marketplace ⭐

- ⭐ **स्वतन्त्र software vendor (तेस्रो पक्ष) का हजारौं software listing भएको डिजिटल catalog**

**उदाहरण:**
- **Custom AMI** (custom OS, firewall, प्राविधिक समाधान…)
- **CloudFormation template**
- **Software as a Service**
- **Container**

- ⭐ **AWS Marketplace बाट किन्नुभयो भने त्यो तपाईंकै AWS bill मा जान्छ**
- ⭐ **तपाईंले आफ्नै समाधान AWS Marketplace मा बेच्न पनि सक्नुहुन्छ**

---

## १२. AWS Training

- **AWS Digital (online) र Classroom Training** (व्यक्तिगत वा virtual)
- **AWS Private Training** (तपाईंको संस्थाको लागि)
- अमेरिकी सरकारको लागि Training र Certification
- Enterprise को लागि Training र Certification
- ⭐ **AWS Academy** — विश्वविद्यालयलाई AWS पढाउन सहयोग गर्ने

---

## १३. AWS Professional Services र Partner Network ⭐

- **AWS Professional Services** = विज्ञहरूको global टिम — तपाईंको टिम र छानिएको APN सदस्यसँग मिलेर काम गर्ने
- ⭐ **APN = AWS Partner Network**

| APN प्रकार | काम |
|---|---|
| **APN Technology Partners** | Hardware, connectivity र software दिने |
| **APN Consulting Partners** | AWS मा बनाउन सहयोग गर्ने professional services firm |
| **APN Training Partners** | AWS सिकाउन सहयोग गर्ने |

- ⭐ **AWS Competency Program** — विशेष समाधान क्षेत्रमा प्राविधिक दक्षता र ग्राहक सफलता देखाएका APN Partner लाई दिइने
- **AWS Navigate Program** — Partner लाई अझ राम्रो Partner बन्न सहयोग

---

## १४. AWS re:Post ⭐

- ⭐ **AWS-managed Q&A सेवा** — AWS सम्बन्धी प्राविधिक प्रश्नको crowd-sourced, विज्ञ-समीक्षित उत्तर
- **मूल AWS Forums को ठाउँ लिने**
- Community सदस्यले स्वीकृत उत्तर दिएर र अरूको उत्तर समीक्षा गरेर **reputation point** कमाउन र community expert बन्न सक्छन्
- ⭐ **AWS Premium Support ग्राहकका प्रश्नमा community बाट जवाफ आएन भने AWS Support engineer कहाँ पुग्छन्**
- ⚠️ ⭐ **समय-संवेदनशील प्रश्न वा गोप्य जानकारी भएका प्रश्नको लागि होइन**

### AWS re:Post — Knowledge Center
- सबैभन्दा बारम्बार सोधिने प्रश्न र अनुरोध — https://repost.aws/knowledge-center

---

## १५. AWS Managed Services (AMS) ⭐

- **AWS मा infrastructure र application support दिने**
- ⭐ **AWS विज्ञको टिम जसले सुरक्षा, भरपर्दोपन र उपलब्धताको लागि तपाईंको infrastructure व्यवस्थापन र सञ्चालन गर्छ**
- संस्थालाई नियमित व्यवस्थापन कार्य **offload** गरेर व्यावसायिक उद्देश्यमा ध्यान दिन सहयोग
- ⭐ **पूर्ण managed सेवा** — AWS ले change request, monitoring, patch management, security र backup सेवा जस्ता सामान्य गतिविधि सम्हाल्छ
- उत्तम अभ्यास लागू गरेर तपाईंको AWS infrastructure मर्मत गर्ने — परिचालन भार र जोखिम घटाउने
- ⭐ **AMS को व्यावसायिक समय: २४/३६५**

---

## 📝 अभ्यास प्रश्नहरू

**प्रश्न १.** Well-Architected Framework का ६ pillar कुन-कुन हुन्? (तीन छान्नुहोस्)

A) Operational Excellence
B) Scalability
C) Reliability
D) Sustainability
E) Agility

<details><summary>उत्तर</summary>

**A), C), D)** — ६ pillar: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, Sustainability। Scalability र Agility pillar होइनन्।
</details>

---

**प्रश्न २.** AWS CAF का ६ perspective मध्ये कुन "प्रविधि र व्यवसाय बीचको पुल" हो?

A) Business Perspective
B) People Perspective
C) Governance Perspective
D) Platform Perspective

<details><summary>उत्तर</summary>

**B) People Perspective** — संस्कृति, सांगठनिक संरचना, नेतृत्व र कर्मचारीमा केन्द्रित।
</details>

---

**प्रश्न ३.** "Instance type र size लाई workload को आवश्यकतासँग सबैभन्दा कम मूल्यमा मिलाउने" प्रक्रियालाई के भनिन्छ?

A) Auto Scaling
B) Right Sizing
C) Load Balancing
D) Elasticity

<details><summary>उत्तर</summary>

**B) Right Sizing** — Cloud migration अघि र onboarding पछि निरन्तर गर्नुपर्ने।
</details>

---

**प्रश्न ४.** AWS CAF का transformation phase कुन क्रममा हुन्छन्?

A) Align → Envision → Launch → Scale
B) Envision → Align → Launch → Scale
C) Launch → Envision → Align → Scale
D) Envision → Launch → Align → Scale

<details><summary>उत्तर</summary>

**B) Envision → Align → Launch → Scale**
</details>

---

**प्रश्न ५.** तेस्रो पक्षको software AMI किन्न कहाँ जाने?

A) AWS Service Catalog
B) AWS Marketplace
C) AWS re:Post
D) AWS Artifact

<details><summary>उत्तर</summary>

**B) AWS Marketplace** — तेस्रो पक्षको software catalog। किनेको खर्च AWS bill मै आउँछ।
</details>

---

**प्रश्न ६.** Well-Architected Framework का ६ pillar बारे कुन कथन सही हो?

A) यी trade-off हुन् — एउटा बढाउँदा अर्को घट्छ
B) यी सन्तुलन गर्ने कुरा हुन्
C) यी एकआपसमा सहकार्य (synergy) गर्छन्
D) Cost Optimization सबैभन्दा महत्त्वपूर्ण हो

<details><summary>उत्तर</summary>

**C)** — स्लाइडमा स्पष्ट भनिएको छ: "They are not something to balance, or trade-offs, they're a synergy"।
</details>

---

**प्रश्न ७.** AWS मा प्राविधिक प्रश्नको crowd-sourced, विज्ञ-समीक्षित उत्तर कहाँ पाइन्छ?

A) AWS Support
B) AWS re:Post
C) AWS Artifact
D) AWS Trusted Advisor

<details><summary>उत्तर</summary>

**B) AWS re:Post** — पुरानो AWS Forums को ठाउँ लिने Q&A सेवा।
</details>

---

**प्रश्न ८.** "Monolithic एप्लिकेसनलाई साना, स्वतन्त्र component मा टुक्र्याउने" कुन design principle हो?

A) Automation
B) Loose Coupling
C) Disposable Resources
D) Services, not Servers

<details><summary>उत्तर</summary>

**B) Loose Coupling** — एउटा component मा भएको परिवर्तन वा असफलता अरूमा सर्नु हुँदैन।
</details>

---

## ✅ यो मोड्युलको सार

### Well-Architected Framework — ६ Pillar ⭐⭐⭐
**Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, Sustainability**
👉 यी **trade-off होइनन्, synergy हुन्**

### AWS CAF — ६ Perspective ⭐⭐⭐
- **व्यावसायिक:** Business, People, Governance
- **प्राविधिक:** Platform, Security, Operations

### CAF — ४ Transformation Phase
**Envision → Align → Launch → Scale**

### Design Principles
Scalability, Disposable Resources, Automation, **Loose Coupling**, **Services not Servers**

### Ecosystem
| स्रोत | काम |
|---|---|
| **Well-Architected Tool** | ६ pillar विरुद्ध architecture समीक्षा (निःशुल्क) |
| **Customer Carbon Footprint Tool** | AWS प्रयोगबाट कार्बन उत्सर्जन ट्र्याक |
| **Marketplace** | तेस्रो पक्षको software catalog |
| **re:Post** | Q&A community (पुरानो Forums) |
| **Professional Services / APN** | AWS विज्ञ र partner network |
| **AWS Managed Services (AMS)** | AWS विज्ञले तपाईंको infrastructure चलाइदिने (२४/३६५) |

---

⬅️ [मोड्युल १८](18-other-services.md) | ➡️ [मोड्युल २० — परीक्षाको तयारी](20-exam-prep.md)
