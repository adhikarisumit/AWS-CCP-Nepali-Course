# मोड्युल ०५ — Elastic Load Balancing र Auto Scaling Group

> **यस मोड्युलमा:** Scalability का प्रकार, High Availability, ४ किसिमका Load Balancer, र Auto Scaling रणनीतिहरू।

---

## १. Scalability र High Availability

**Scalability** = एप्लिकेसन/सिस्टमले **अनुकूलन गरेर ठूलो लोड सम्हाल्न सक्ने क्षमता**।

दुई किसिमका छन्:

### ⬆️ Vertical Scalability (माथितिर)
- **Instance को आकार बढाउने**
- उदाहरण: `t2.micro` मा चलिरहेको एप `t2.large` मा सार्ने
- **Distributed नभएको सिस्टम** (जस्तै database) मा धेरै प्रयोग हुन्छ
- ⚠️ **सीमा हुन्छ** — hardware को अधिकतम सीमा
- *उपमा:* जुनियर operator लाई सिनियर operator ले फेर्ने

### ➡️ Horizontal Scalability (छेउतिर) = **Elasticity**
- **Instance को सङ्ख्या बढाउने**
- **Distributed सिस्टम** चाहिन्छ
- आधुनिक वेब एप्लिकेसनमा धेरै प्रयोग
- EC2 जस्तो क्लाउड सेवाले यो सजिलो बनाएको छ
- *उपमा:* एउटा operator को सट्टा ६ जना operator राख्ने

### 🏢 High Availability
- सामान्यतया horizontal scaling सँगै जान्छ
- **कम्तीमा २ Availability Zone मा** एप्लिकेसन चलाउनु
- **उद्देश्य: एउटा data center गुम्दा (प्रकोप) पनि सेवा नरोकिनु**
- *उपमा:* पहिलो भवन New York मा, दोस्रो भवन San Francisco मा

---

## २. EC2 मा Scalability र High Availability

| अवधारणा | EC2 मा कसरी |
|---|---|
| **Vertical Scaling** | Instance आकार बढाउने (scale up/down)। `t2.nano` (0.5 GB RAM, 1 vCPU) → `u-12tb1.metal` (12.3 TB RAM, 448 vCPU) |
| **Horizontal Scaling** | Instance सङ्ख्या बढाउने (scale out/in) — **Auto Scaling Group + Load Balancer** |
| **High Availability** | एउटै एप्लिकेसनका instance **धेरै AZ** मा चलाउने — ASG multi-AZ + Load Balancer multi-AZ |

---

## ३. Scalability vs Elasticity vs Agility ⭐⭐

**यो परीक्षामा झुक्याउने प्रश्न बनाइन्छ। ध्यान दिनुहोस्:**

| शब्द | अर्थ |
|---|---|
| **Scalability** | ठूलो लोड सम्हाल्ने क्षमता — hardware बलियो बनाएर (scale up) वा node थपेर (scale out) |
| **Elasticity** | सिस्टम scalable भइसकेपछि, लोड अनुसार **"auto-scaling"** हुनु। यो "cloud-friendly" हो: pay-per-use, माग अनुसार, लागत अनुकूलन |
| **Agility** | ⚠️ **Scaling सँग सम्बन्धित छैन — यो distractor हो!** नयाँ IT संसाधन एक क्लिकमै — developer लाई हप्ता लाग्ने कुरा मिनेटमै पुर्‍याउनु |

---

## ४. Load Balancing के हो?

**Load Balancer** = इन्टरनेटको traffic लाई तल्लो धेरै server (EC2 instance) मा बाँड्ने server।

```
User 1 ┐
User 2 ├──> Load Balancer ──┬──> EC2 Instance 1
User 3 ┘                    ├──> EC2 Instance 2
                            └──> EC2 Instance 3
```

### किन प्रयोग गर्ने? ⭐
- धेरै तल्लो instance मा **लोड बाँड्ने**
- एप्लिकेसनको लागि **एउटै पहुँच बिन्दु (DNS)** देखाउने
- तल्लो instance बिग्रिँदा **सजिलै सम्हाल्ने**
- Instance को **नियमित health check** गर्ने
- वेबसाइटको लागि **SSL termination (HTTPS)** दिने
- **Zone बीच high availability**

---

## ५. Elastic Load Balancer (ELB) ⭐

**ELB = AWS ले managed गर्ने Load Balancer**

- **AWS ले काम गर्ने ग्यारेन्टी दिन्छ**
- Upgrade, maintenance, high availability सबै AWS को जिम्मा
- AWS ले केही configuration मात्र बदल्न दिन्छ
- आफ्नै load balancer बनाउँदा सस्तो पर्छ तर **धेरै मेहनत** लाग्छ

### ४ किसिमका Load Balancer ⭐⭐

| Load Balancer | प्रोटोकल | Layer | विशेषता |
|---|---|---|---|
| **Application Load Balancer (ALB)** | HTTP / HTTPS / gRPC | **Layer 7** | HTTP routing सुविधा, **Static DNS (URL)** |
| **Network Load Balancer (NLB)** | TCP / UDP | **Layer 4** | अति उच्च प्रदर्शन — प्रति सेकेन्ड लाखौं request, **Static IP (Elastic IP)** |
| **Gateway Load Balancer (GWLB)** | GENEVE (IP Packets) | **Layer 3** | EC2 मा चलेका firewall मा traffic पठाउने, intrusion detection |
| **Classic Load Balancer (CLB)** | — | Layer 4 र 7 | **२०२३ मा retire भइसक्यो** (पुरानो) |

> **सम्झने तरिका:**
> - **A**pplication → **A**pplication layer (Layer 7) → HTTP
> - **N**etwork → **N**etwork/Transport (Layer 4) → TCP/UDP → **Static IP**
> - **G**ateway → Layer 3 → firewall/security appliance

> **परीक्षा टिप:** "Static IP address चाहियो" → **NLB**। "HTTP routing चाहियो" → **ALB**।

---

## ६. Auto Scaling Group (ASG) ⭐⭐

वास्तविक जीवनमा वेबसाइटको लोड बदलिरहन्छ। क्लाउडमा server छिटो बनाउन र हटाउन सकिन्छ।

### ASG को उद्देश्य
- लोड बढ्दा **Scale Out** — EC2 instance थप्ने
- लोड घट्दा **Scale In** — EC2 instance हटाउने
- **न्यूनतम र अधिकतम** मेसिन सङ्ख्या सुनिश्चित गर्ने
- नयाँ instance लाई **आफैं load balancer मा दर्ता** गर्ने
- **अस्वस्थ (unhealthy) instance फेर्ने**
- **लागत बचत:** उपयुक्त क्षमतामा मात्र चलाउने

### ASG का ३ आकार

```
┌─────────── AUTO SCALING GROUP ───────────┐
│                                          │
│  Minimum size    ← कम्तीमा यति चल्छ      │
│  Desired Capacity ← अहिले यति चलिरहेको   │
│  Maximum size    ← बढीमा यति सम्म        │
│                                          │
└──────────────────────────────────────────┘
```

### ASG + Load Balancer

```
Web Traffic → Load Balancer → [ASG: EC2, EC2, EC2, EC2, EC2, EC2]
```

---

## ७. Auto Scaling रणनीतिहरू ⭐⭐

| रणनीति | विवरण | उदाहरण |
|---|---|---|
| **Manual Scaling** | ASG को आकार हातैले बदल्ने | Console बाट desired = 5 राख्ने |
| **Simple / Step Scaling** | CloudWatch alarm मा प्रतिक्रिया | CPU > 70% भए २ unit थप्ने; CPU < 30% भए १ हटाउने |
| **Target Tracking Scaling** | निश्चित लक्ष्य कायम राख्ने | "ASG को औसत CPU करिब ४०% मै रहोस्" |
| **Scheduled Scaling** | थाहा भएको ढाँचा अनुसार पहिल्यै | "हरेक शुक्रबार बेलुका ५ बजे min capacity १० बनाउने" |
| **Predictive Scaling** | **Machine Learning** ले भविष्यको traffic अनुमान गरेर पहिल्यै instance तयार पार्ने | समयमा आधारित अनुमानयोग्य ढाँचा भएको लोडमा उपयोगी |

> **परीक्षा टिप:** प्रश्नमा "**Machine Learning** ले traffic अनुमान गर्ने" भन्ने आयो भने उत्तर **Predictive Scaling** हो। "**थाहा भएको समय**" (जस्तै हरेक शुक्रबार) भन्ने आयो भने **Scheduled Scaling**।

---

## 📝 अभ्यास प्रश्नहरू

**प्रश्न १.** `t2.micro` बाट `t2.large` मा सार्नु कस्तो scaling हो?

A) Horizontal Scaling
B) Vertical Scaling
C) Elasticity
D) High Availability

<details><summary>उत्तर</summary>

**B) Vertical Scaling** — instance को आकार बढाउनु (scale up)।
</details>

---

**प्रश्न २.** एउटा एप्लिकेसनलाई **static IP address** भएको load balancer चाहियो। कुन प्रयोग गर्ने?

A) Application Load Balancer
B) Network Load Balancer
C) Gateway Load Balancer
D) Classic Load Balancer

<details><summary>उत्तर</summary>

**B) Network Load Balancer** — NLB ले Elastic IP मार्फत static IP दिन्छ। ALB ले static DNS मात्र दिन्छ।
</details>

---

**प्रश्न ३.** "नयाँ IT संसाधन एक क्लिकमै उपलब्ध हुनु" — यो के हो?

A) Elasticity
B) Scalability
C) Agility
D) High Availability

<details><summary>उत्तर</summary>

**C) Agility** — यो scaling सँग सम्बन्धित छैन। परीक्षामा distractor को रूपमा आउँछ।
</details>

---

**प्रश्न ४.** एउटा कम्पनीको traffic हरेक सोमबार बिहान ९ बजे बढ्छ भन्ने थाहा छ। कुन scaling रणनीति उपयुक्त?

A) Simple Scaling
B) Target Tracking Scaling
C) Scheduled Scaling
D) Manual Scaling

<details><summary>उत्तर</summary>

**C) Scheduled Scaling** — थाहा भएको समयको ढाँचामा पहिल्यै scale गर्ने।
</details>

---

**प्रश्न ५.** High Availability प्राप्त गर्न के गर्नुपर्छ?

A) एउटै AZ मा धेरै instance चलाउने
B) कम्तीमा २ AZ मा instance चलाउने
C) Instance को आकार बढाउने
D) Spot Instance प्रयोग गर्ने

<details><summary>उत्तर</summary>

**B)** — High Availability = कम्तीमा २ Availability Zone मा चलाउनु, ताकि एउटा data center गुम्दा पनि सेवा चलोस्।
</details>

---

**प्रश्न ६.** ASG ले के गर्छ? (दुई छान्नुहोस्)

A) अस्वस्थ instance फेर्छ
B) Instance को आकार आफैं बढाउँछ
C) लोड अनुसार instance सङ्ख्या घटाउँछ/बढाउँछ
D) Database backup लिन्छ
E) SSL certificate व्यवस्थापन गर्छ

<details><summary>उत्तर</summary>

**A)** र **C)** — ASG ले horizontal scaling र unhealthy instance replacement गर्छ। आकार बढाउनु vertical scaling हो, ASG ले गर्दैन।
</details>

---

## ✅ यो मोड्युलको सार

- **High Availability vs Scalability (vertical र horizontal) vs Elasticity vs Agility** — फरक बुझ्नुहोस्
- **Elastic Load Balancer (ELB):**
  - तल्लो EC2 instance मा traffic बाँड्ने, **Multi-AZ** हुन सक्ने
  - **Health check** समर्थन गर्ने
  - ४ प्रकार: **Classic** (पुरानो), **Application** (HTTP – L7), **Network** (TCP – L4, static IP), **Gateway** (L3)
- **Auto Scaling Group (ASG):**
  - एप्लिकेसनमा **Elasticity** ल्याउने, धेरै AZ मा
  - माग अनुसार EC2 instance scale गर्ने, अस्वस्थ instance फेर्ने
  - **ELB सँग एकीकृत**
  - रणनीति: Manual, Simple/Step, Target Tracking, Scheduled, **Predictive (ML)**

---

⬅️ [मोड्युल ०४](04-ec2-storage.md) | ➡️ [मोड्युल ०६ — Amazon S3](06-s3.md)
