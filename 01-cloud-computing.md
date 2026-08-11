# मोड्युल ०१ — क्लाउड कम्प्युटिङ के हो?

> **यस मोड्युलमा:** सर्भर कसरी काम गर्छ, क्लाउड किन आयो, क्लाउडका ३ प्रकार, ६ फाइदा, ५ विशेषता, IaaS/PaaS/SaaS, AWS Global Infrastructure, र Shared Responsibility Model।

---

## १. वेबसाइट कसरी चल्छ?

इन्टरनेटमा दुई पक्ष हुन्छन्:

- **Client** — तपाईंको ल्यापटप वा मोबाइल। यसको आफ्नै **IP address** हुन्छ।
- **Server** — वेबसाइट राखिएको कम्प्युटर। यसको पनि IP address हुन्छ।

Client ले server लाई अनुरोध (request) पठाउँछ, server ले जवाफ (response) पठाउँछ।

> **हुलाकको उदाहरण:** तपाईंले चिठी पठाउँदा ठेगाना चाहिन्छ नि? IP address भनेको इन्टरनेटको ठेगाना हो। बिना ठेगाना चिठी पुग्दैन, बिना IP डाटा पुग्दैन।

### सर्भर केले बनेको हुन्छ?

| भाग | अङ्ग्रेजी | काम |
|---|---|---|
| प्रोसेसर | **Compute (CPU)** | गणना गर्ने |
| मेमोरी | **Memory (RAM)** | चलिरहेको डाटा राख्ने |
| भण्डारण | **Storage** | डाटा स्थायी रूपमा राख्ने |
| डाटाबेस | **Database** | डाटालाई संरचित तरिकाले राख्ने |
| नेटवर्क | **Network** | Router, Switch, DNS server |

### IT का आधारभूत शब्दहरू

- **Network** — केबल, router र server एकआपसमा जोडिएको जाल।
- **Router** — कुन डाटा प्याकेट कुन नेटवर्कमा पठाउने भनेर निर्णय गर्ने यन्त्र। इन्टरनेटमा बाटो देखाउने।
- **Switch** — प्याकेटलाई आफ्नै नेटवर्कभित्रको सही server/client सम्म पुर्‍याउने।

---

## २. परम्परागत IT को समस्या

पहिले कम्पनीहरूले आफ्नै data center (वा अफिसको कोठा, वा घरको गराज) मा सर्भर राख्थे। समस्याहरू:

- Data center को **भाडा** तिर्नुपर्ने
- **बिजुली, कुलिङ, मर्मत** को खर्च
- नयाँ hardware थप्न/फेर्न **धेरै समय** लाग्ने
- **Scaling को सीमा** — एउटा कोठामा जति अट्छ त्यति मात्र
- २४/७ निगरानी गर्ने **टिम राख्नुपर्ने**
- **प्रकोप** (भूकम्प, आगलागी, बिजुली जाने) आए के गर्ने?

👉 **यी सबै अरूलाई जिम्मा दिन सकिन्छ?** — यही प्रश्नबाट क्लाउड जन्मियो।

---

## ३. क्लाउड कम्प्युटिङ भनेको के हो?

> **परिभाषा:** क्लाउड कम्प्युटिङ भनेको **compute power, database storage, applications र अन्य IT संसाधनहरूको on-demand delivery** हो, जुन **pay-as-you-go** मूल्यमा क्लाउड प्लेटफर्मबाट पाइन्छ।

मुख्य बुँदाहरू:
- तपाईंलाई **जति चाहिन्छ ठ्याक्कै त्यति** provision गर्न सक्नुहुन्छ
- **तुरुन्तै** जति संसाधन चाहिए पनि पाउन सक्नुहुन्छ
- Server, storage, database सजिलै पहुँच
- **AWS ले hardware किन्छ र मर्मत गर्छ**, तपाईंले वेब एप्लिकेसनबाट चाहिएको मात्र लिनुहुन्छ

### तपाईंले पहिल्यै क्लाउड प्रयोग गरिसक्नुभएको छ

| सेवा | के हो |
|---|---|
| **Gmail** | इमेल क्लाउड सेवा — जति इमेल राख्नुभयो त्यतिको मात्र मूल्य |
| **Dropbox** | क्लाउड स्टोरेज — सुरुमा AWS मै बनेको थियो |
| **Netflix** | AWS मा बनेको — Video on Demand |

---

## ४. क्लाउडका ३ Deployment Model

### 🔒 Private Cloud
- एउटै संस्थाले मात्र प्रयोग गर्ने, जनतालाई खुला नगरिने
- **पूर्ण नियन्त्रण**
- संवेदनशील एप्लिकेसनको लागि सुरक्षा
- विशेष व्यावसायिक आवश्यकता पूरा गर्ने

### 🌐 Public Cloud
- तेस्रो पक्ष (AWS, Azure, GCP) को स्वामित्वमा, इन्टरनेटबाट डेलिभर हुने
- क्लाउडका ६ वटै फाइदा यहीँ पाइन्छ

### 🔗 Hybrid Cloud
- केही server आफ्नै अफिसमा (on-premises), केही क्षमता क्लाउडमा
- संवेदनशील डाटा आफ्नै infrastructure मा नियन्त्रण
- साथै public cloud को लचकता र किफायत

> **परीक्षा टिप:** प्रश्नमा "compliance को कारण केही डाटा आफ्नै data center मै राख्नुपर्ने" भन्ने आयो भने उत्तर **Hybrid Cloud** हो।

---

## ५. क्लाउडका ५ विशेषता (Five Characteristics)

यी ५ वटा कण्ठै पार्नुहोस् — परीक्षामा सोधिन्छ।

| # | विशेषता | अर्थ |
|---|---|---|
| १ | **On-demand self service** | प्रयोगकर्ताले सेवा प्रदायकसँग कुरै नगरी आफैं संसाधन लिन सक्ने |
| २ | **Broad network access** | नेटवर्कबाट, जुनसुकै किसिमको यन्त्रबाट पहुँच |
| ३ | **Multi-tenancy र resource pooling** | धेरै ग्राहकले एउटै भौतिक infrastructure सुरक्षा र गोपनीयतासहित बाँड्ने |
| ४ | **Rapid elasticity र scalability** | चाहिँदा तुरुन्तै थप्ने, नचाहिँदा तुरुन्तै घटाउने — स्वचालित रूपमा |
| ५ | **Measured service** | प्रयोग नापिन्छ, जति प्रयोग गर्‍यो त्यति मात्र तिर्ने |

---

## ६. क्लाउडका ६ फाइदा (Six Advantages) ⭐

**यो परीक्षामा सबैभन्दा धेरै सोधिने सूची हो।**

| # | फाइदा | व्याख्या |
|---|---|---|
| १ | **CAPEX लाई OPEX मा बदल्ने** | पहिल्यै hardware किन्नुपर्दैन (Capital Expense), प्रयोग गरेको जति तिर्ने (Operational Expense)। TCO घट्छ। |
| २ | **ठूलो economies of scale को फाइदा** | AWS ठूलो भएकोले सस्तोमा किन्छ, त्यो बचत ग्राहकलाई दिन्छ — मूल्य घट्दै जान्छ |
| ३ | **क्षमताको अनुमान लगाउन छोड्ने** | "कति server चाहिएला?" भनेर अड्कल गर्नुपर्दैन — वास्तविक प्रयोग अनुसार scale गर्ने |
| ४ | **गति र चुस्तता (agility) बढ्ने** | नयाँ संसाधन मिनेटमै तयार |
| ५ | **Data center चलाउने खर्च रोक्ने** | बिजुली, कुलिङ, सुरक्षा गार्ड — कुनै झन्झट छैन |
| ६ | **मिनेटमै विश्वव्यापी जाने** | AWS को global infrastructure प्रयोग गरेर संसारभर फैलिने |

> **सम्झने तरिका:** *"पैसा-मात्रा-अनुमान-गति-खर्च-विश्व"*

---

## ७. क्लाउडले समाधान गर्ने समस्याहरू

| शब्द | अर्थ |
|---|---|
| **Flexibility** | चाहिँदा संसाधनको प्रकार बदल्न सकिने |
| **Cost-Effectiveness** | जति प्रयोग गर्‍यो त्यति मात्र तिर्ने |
| **Scalability** | ठूलो लोड सम्हाल्न hardware बलियो बनाउने वा थप node थप्ने |
| **Elasticity** | आवश्यकता अनुसार आफैं बढ्ने/घट्ने (scale out र scale in) |
| **High-availability र Fault-tolerance** | धेरै data center मा फैलाएर बनाउने |
| **Agility** | छिटो develop, test र launch गर्न सकिने |

⚠️ **Scalability vs Elasticity को फरक ध्यान दिनुहोस्** — यो परीक्षामा झुक्याउने प्रश्न बन्छ:
- **Scalability** = ठूलो लोड सम्हाल्ने *क्षमता* (म्यानुअल हुन सक्छ)
- **Elasticity** = त्यो क्षमता **स्वचालित रूपमा** प्रयोग हुनु (auto-scaling)
- **Agility** = scaling सँग सम्बन्धित छैन! यो त नयाँ IT संसाधन एक क्लिकमै पाइनु हो — हप्ता लाग्ने काम मिनेटमै

---

## ८. क्लाउड कम्प्युटिङका ३ प्रकार — IaaS, PaaS, SaaS ⭐

### 🏗️ Infrastructure as a Service (IaaS)
- क्लाउड IT का **आधारभूत ब्लक** दिन्छ
- Networking, कम्प्युटर, storage space
- **सबैभन्दा बढी लचकता**
- परम्परागत on-premises IT सँग सजिलै तुलना गर्न मिल्ने
- **उदाहरण:** Amazon EC2, GCP, Azure, Rackspace, Digital Ocean, Linode

### ⚙️ Platform as a Service (PaaS)
- तल्लो infrastructure व्यवस्थापन गर्नुपर्दैन
- तपाईंले **एप्लिकेसन deploy र manage** गर्ने मात्र
- **उदाहरण:** AWS Elastic Beanstalk, Heroku, Google App Engine, Windows Azure

### 📦 Software as a Service (SaaS)
- **पूरै तयार उत्पादन**, सेवा प्रदायकले नै चलाउने
- **उदाहरण:** Amazon Rekognition, Gmail, Dropbox, Zoom

### जिम्मेवारीको तह — यो चित्र सम्झनुहोस्

```
On-premises        IaaS              PaaS              SaaS
─────────────      ─────────────     ─────────────     ─────────────
Applications  ⬅तपाईं  Applications ⬅तपाईं  Applications ⬅तपाईं  Applications ⬅AWS
Data          ⬅तपाईं  Data         ⬅तपाईं  Data         ⬅तपाईं  Data         ⬅AWS
Runtime       ⬅तपाईं  Runtime      ⬅तपाईं  Runtime      ⬅AWS    Runtime      ⬅AWS
Middleware    ⬅तपाईं  Middleware   ⬅तपाईं  Middleware   ⬅AWS    Middleware   ⬅AWS
O/S           ⬅तपाईं  O/S          ⬅तपाईं  O/S          ⬅AWS    O/S          ⬅AWS
Virtualization⬅तपाईं  Virtualization⬅AWS   Virtualization⬅AWS   Virtualization⬅AWS
Servers       ⬅तपाईं  Servers      ⬅AWS    Servers      ⬅AWS    Servers      ⬅AWS
Storage       ⬅तपाईं  Storage      ⬅AWS    Storage      ⬅AWS    Storage      ⬅AWS
Networking    ⬅तपाईं  Networking   ⬅AWS    Networking   ⬅AWS    Networking   ⬅AWS
```

> **सम्झने तरिका (पिज्जाको उदाहरण):**
> - **On-premises** = घरमै पिज्जा बनाउने — पिठो, चीज, ओभन सबै आफ्नै
> - **IaaS** = पिज्जा बनाउने किट किन्ने — ओभन र सामग्री तयार, बनाउने तपाईं
> - **PaaS** = पिज्जा डेलिभरी — पिज्जा तयार आउँछ, टेबल र प्लेट तपाईंको
> - **SaaS** = रेस्टुरेन्टमा गएर खाने — केही गर्नुपर्दैन

---

## ९. क्लाउडको मूल्य निर्धारण — ३ आधार

AWS मा **pay-as-you-go** मोडेल अनुसार ३ वटा आधार छन्:

| # | आधार | विवरण |
|---|---|---|
| १ | **Compute** | कति समय compute चलायो, त्यति |
| २ | **Storage** | क्लाउडमा कति डाटा राख्यो, त्यति |
| ३ | **Data transfer OUT** | क्लाउडबाट **बाहिर** जाने डाटा |

⚠️ **महत्त्वपूर्ण:** **Data transfer IN निःशुल्क छ।** क्लाउडभित्र डाटा हाल्न पैसा लाग्दैन, बाहिर निकाल्न लाग्छ। यो परीक्षामा सोधिन्छ।

---

## १०. AWS को इतिहास र तथ्य

| वर्ष | घटना |
|---|---|
| २००२ | आन्तरिक रूपमा सुरु |
| २००३ | Amazon को infrastructure नै मुख्य शक्ति हो भन्ने बुझाइ — बजारमा ल्याउने विचार |
| २००४ | **SQS** सँग सार्वजनिक रूपमा सुरु |
| २००६ | SQS, **S3** र **EC2** सँग पुनः सार्वजनिक |
| २००७ | युरोपमा विस्तार |

**तथ्यहरू:**
- २०२३ मा AWS को वार्षिक आम्दानी **USD ९० अर्ब**
- २०२४ को Q1 मा बजारको **३१%** हिस्सा (Microsoft दोस्रो, २५%)
- लगातार **१३ औं वर्ष** क्लाउड बजारको अगुवा
- **१० लाख भन्दा बढी** सक्रिय प्रयोगकर्ता

### AWS को प्रयोग कहाँ हुन्छ?
Enterprise IT, Backup र Storage, Big Data analytics, Website hosting, Mobile र Social Apps, Gaming।

---

## ११. AWS Global Infrastructure ⭐

यो परीक्षाको जग हो। ४ तह छन्:

### 🌍 AWS Region
- संसारभर फैलिएका **data center का समूह**
- नाम: `us-east-1`, `eu-west-3`, `ap-south-1`, `ap-southeast-2`…
- **अधिकांश AWS सेवा region-scoped हुन्छन्** — अर्थात् एउटा region मा बनाएको अर्को region मा देखिँदैन

### Region कसरी छान्ने? (४ आधार) ⭐

| आधार | व्याख्या |
|---|---|
| **Compliance** | कानुनी/डाटा शासन आवश्यकता — तपाईंको अनुमतिबिना डाटा region बाहिर जाँदैन |
| **ग्राहकको नजिक (Proximity)** | latency घट्छ — नेपालका प्रयोगकर्ताको लागि Mumbai (`ap-south-1`) नजिक |
| **उपलब्ध सेवा** | सबै नयाँ सेवा सबै region मा उपलब्ध हुँदैनन् |
| **मूल्य (Pricing)** | region अनुसार मूल्य फरक हुन्छ |

### 🏢 Availability Zone (AZ)
- हरेक region मा धेरै AZ हुन्छन् — **सामान्यतया ३, न्यूनतम ३, अधिकतम ६**
- उदाहरण: `ap-southeast-2a`, `ap-southeast-2b`, `ap-southeast-2c`
- हरेक AZ मा एक वा बढी छुट्टै data center हुन्छन् — आफ्नै बिजुली, नेटवर्क, कनेक्टिभिटीसहित
- **एकअर्काबाट अलग** — त्यसैले एउटामा प्रकोप आए अर्को सुरक्षित
- तर आपसमा **उच्च bandwidth, अति कम latency** को नेटवर्कले जोडिएका

> **सम्झने तरिका:** Region = काठमाडौं शहर। AZ = शहरका फरक-फरक वडा, जहाँ फरक बिजुली लाइन र पानीको स्रोत छ। एउटा वडामा समस्या आए अर्को वडा चल्छ।

### ⚡ Edge Location / Points of Presence
- Amazon सँग **४००+ Points of Presence** (४००+ Edge Location र १०+ Regional Cache)
- **४० भन्दा बढी देशका ९० भन्दा बढी शहर** मा
- उद्देश्य: **कम latency** मा प्रयोगकर्तालाई सामग्री पुर्‍याउने

### Global सेवा vs Region-scoped सेवा ⭐

| **Global सेवा** (region छैन) | **Region-scoped सेवा** |
|---|---|
| IAM (Identity and Access Management) | Amazon EC2 (IaaS) |
| Route 53 (DNS) | Elastic Beanstalk (PaaS) |
| CloudFront (CDN) | Lambda (FaaS) |
| WAF (Web Application Firewall) | Rekognition (SaaS) |

> **परीक्षा टिप:** Global सेवा ४ वटा कण्ठै पार्नुहोस् — **IAM, Route 53, CloudFront, WAF**।

---

## १२. Shared Responsibility Model (साझा जिम्मेवारी मोडेल) ⭐⭐

यो परीक्षाको **सबैभन्दा महत्त्वपूर्ण अवधारणा** हो। हरेक सेक्सनमा फेरि आउँछ।

```
┌─────────────────────────────────────────────────┐
│  ग्राहक = क्लाउड "मा" (IN) को सुरक्षा           │
│  CUSTOMER = SECURITY IN THE CLOUD               │
│  → तपाईंको डाटा, OS प्याच, firewall सेटिङ,     │
│    IAM, encryption                              │
├─────────────────────────────────────────────────┤
│  AWS = क्लाउड "को" (OF) सुरक्षा                 │
│  AWS = SECURITY OF THE CLOUD                    │
│  → Hardware, data center, global network,       │
│    भौतिक सुरक्षा, virtualization                │
└─────────────────────────────────────────────────┘
```

**सम्झने सजिलो तरिका:**
- **OF the cloud → AWS** (क्लाउड *आफैं* बनाउने र सुरक्षित राख्ने)
- **IN the cloud → तपाईं** (क्लाउड *भित्र* तपाईंले जे राख्नुभयो त्यो)

---

## १३. AWS Acceptable Use Policy

AWS मा यी काम **निषेधित** छन् (https://aws.amazon.com/aup/):
- गैरकानुनी, हानिकारक वा आपत्तिजनक प्रयोग/सामग्री
- सुरक्षा उल्लङ्घन
- नेटवर्कको दुरुपयोग
- इमेल वा सन्देशको दुरुपयोग (spam)

---

## 📝 अभ्यास प्रश्नहरू

**प्रश्न १.** एउटा कम्पनीले संवेदनशील ग्राहक डाटा नियामकीय कारणले आफ्नै data center मै राख्नुपर्छ, तर वेबसाइट AWS मा चलाउन चाहन्छ। यो कस्तो deployment model हो?

A) Private Cloud
B) Public Cloud
C) Hybrid Cloud
D) Community Cloud

<details><summary>उत्तर हेर्नुहोस्</summary>

**C) Hybrid Cloud** — केही infrastructure on-premises, केही क्लाउडमा। संवेदनशील सम्पत्तिमा नियन्त्रण राख्दै public cloud को लचकता पाइन्छ।
</details>

---

**प्रश्न २.** कुन कथन क्लाउडको **elasticity** लाई सही रूपमा वर्णन गर्छ?

A) नयाँ IT संसाधन एक क्लिकमै उपलब्ध हुनु
B) लोड अनुसार सिस्टम आफैं बढ्ने र घट्ने
C) hardware बलियो बनाएर ठूलो लोड सम्हाल्नु
D) धेरै AZ मा एप्लिकेसन चलाउनु

<details><summary>उत्तर हेर्नुहोस्</summary>

**B)** — Elasticity भनेको **auto-scaling** हो: लोड अनुसार आफैं scale हुनु।
- A = Agility (यो distractor हो!)
- C = Vertical Scalability
- D = High Availability
</details>

---

**प्रश्न ३.** निम्नमध्ये कुन AWS सेवाहरू **global** हुन्? (दुई छान्नुहोस्)

A) Amazon EC2
B) AWS IAM
C) Amazon CloudFront
D) AWS Lambda
E) Elastic Beanstalk

<details><summary>उत्तर हेर्नुहोस्</summary>

**B) IAM** र **C) CloudFront** — Global सेवाहरू हुन्: IAM, Route 53, CloudFront, WAF। बाँकी सबै region-scoped हुन्।
</details>

---

**प्रश्न ४.** AWS मा डाटा transfer को मूल्यबारे कुन सही हो?

A) IN र OUT दुवैमा पैसा लाग्छ
B) IN मा पैसा लाग्छ, OUT निःशुल्क
C) IN निःशुल्क, OUT मा पैसा लाग्छ
D) दुवै निःशुल्क

<details><summary>उत्तर हेर्नुहोस्</summary>

**C)** — क्लाउडभित्र डाटा हाल्न (IN) निःशुल्क, बाहिर निकाल्न (OUT) पैसा लाग्छ।
</details>

---

**प्रश्न ५.** Amazon EC2 कुन प्रकारको क्लाउड सेवामा पर्छ?

A) IaaS
B) PaaS
C) SaaS
D) FaaS

<details><summary>उत्तर हेर्नुहोस्</summary>

**A) IaaS** — EC2 ले भर्चुअल मेसिन दिन्छ, OS र माथिको सबै तपाईंको जिम्मा। Elastic Beanstalk = PaaS, Rekognition = SaaS, Lambda = FaaS।
</details>

---

**प्रश्न ६.** एउटा region मा न्यूनतम कति Availability Zone हुन्छन्?

A) १
B) २
C) ३
D) ६

<details><summary>उत्तर हेर्नुहोस्</summary>

**C) ३** — सामान्यतया ३, न्यूनतम ३, अधिकतम ६।
</details>

---

**प्रश्न ७.** Shared Responsibility Model अनुसार data center को भौतिक सुरक्षा कसको जिम्मा हो?

A) ग्राहकको
B) AWS को
C) दुवैको साझा
D) तेस्रो पक्ष auditor को

<details><summary>उत्तर हेर्नुहोस्</summary>

**B) AWS को** — भौतिक infrastructure "security OF the cloud" मा पर्छ, जुन AWS को जिम्मेवारी हो।
</details>

---

## ✅ यो मोड्युलको सार

- क्लाउड = **on-demand, pay-as-you-go** IT संसाधन
- ३ deployment model: **Private, Public, Hybrid**
- ५ विशेषता: on-demand self service, broad network access, multi-tenancy, rapid elasticity, measured service
- ६ फाइदा: CAPEX→OPEX, economies of scale, क्षमता अनुमान नगर्ने, agility, data center खर्च रोक्ने, मिनेटमै global
- ३ प्रकार: **IaaS** (EC2), **PaaS** (Beanstalk), **SaaS** (Rekognition)
- मूल्यका ३ आधार: Compute, Storage, **Data transfer OUT** (IN निःशुल्क)
- Global Infrastructure: **Region → AZ → Data Center**, र **Edge Location**
- Global सेवा: **IAM, Route 53, CloudFront, WAF**
- **Shared Responsibility:** AWS = security OF the cloud, तपाईं = security IN the cloud

---

⬅️ [README मा फर्कने](README.md) | ➡️ [मोड्युल ०२ — IAM](02-iam.md)
