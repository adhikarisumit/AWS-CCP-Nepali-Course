# मोड्युल १० — Global Infrastructure र Global Applications

> **यस मोड्युलमा:** Route 53, CloudFront, S3 Transfer Acceleration, Global Accelerator, Outposts, WaveLength, Local Zones।

---

## १. Global एप्लिकेसन किन बनाउने? ⭐

**Global application = धेरै भौगोलिक क्षेत्रमा deploy गरिएको एप्लिकेसन।** AWS मा यो Region र/वा Edge Location मार्फत हुन्छ।

### ३ मुख्य कारण

**१. Latency घटाउन**
- **Latency** = network packet server सम्म पुग्न लाग्ने समय
- एसियाबाट अमेरिकाको server सम्म packet पुग्न समय लाग्छ
- प्रयोगकर्ताको नजिक एप deploy गर्दा latency घट्छ, अनुभव राम्रो हुन्छ

**२. Disaster Recovery (DR)**
- एउटा AWS region बिग्रियो भने (भूकम्प, आँधी, बिजुली बन्द, राजनीति…)
- अर्को region मा **failover** गरेर एप चलाइराख्न सकिन्छ
- **DR योजना एप्लिकेसनको उपलब्धता बढाउन महत्त्वपूर्ण छ**

**३. आक्रमणबाट सुरक्षा**
- विश्वव्यापी रूपमा फैलिएको infrastructure मा **आक्रमण गर्न गाह्रो**

### AWS Global Infrastructure सम्झना

| तह | उद्देश्य |
|---|---|
| **Regions** | एप्लिकेसन र infrastructure deploy गर्न |
| **Availability Zones** | धेरै data center मिलेर बनेको |
| **Edge Locations (PoP)** | प्रयोगकर्ताको सकेसम्म नजिक सामग्री पुर्‍याउन |

---

## २. AWS मा Global Application का ४ सेवा ⭐⭐

| सेवा | के गर्छ |
|---|---|
| **Route 53** (Global DNS) | प्रयोगकर्तालाई **सबैभन्दा कम latency** भएको deployment मा पठाउने। DR रणनीतिको लागि उत्तम |
| **CloudFront** (Global CDN) | एपको केही भाग **Edge Location** मा replicate गर्ने — latency घटाउने। सामान्य request **cache** गर्ने |
| **S3 Transfer Acceleration** | Amazon S3 मा **global upload/download छिटो** बनाउने |
| **AWS Global Accelerator** | AWS को global network प्रयोग गरेर **global उपलब्धता र प्रदर्शन** सुधार्ने |

---

## ३. Amazon Route 53 ⭐⭐

**Route 53 = Managed DNS (Domain Name System)**

**DNS** = URL मार्फत server सम्म कसरी पुग्ने भन्ने बुझाउने नियम र record को सङ्ग्रह।

### AWS मा सबैभन्दा सामान्य record ⭐

| Record | उदाहरण | अर्थ |
|---|---|---|
| **A** | `www.google.com => 12.34.56.78` | hostname → **IPv4** |
| **AAAA** | `www.google.com => 2001:0db8:...:7334` | hostname → **IPv6** |
| **CNAME** | `search.google.com => www.google.com` | hostname → **hostname** |
| **Alias** | `example.com => AWS resource` | AWS संसाधन (ELB, CloudFront, S3, RDS…) मा |

### A Record कसरी काम गर्छ

```
१. Web browser → Route 53: "myapp.mydomain.com कहाँ छ?"
२. Route 53 → Browser: "IP 32.45.67.85" (A record)
३. Browser → Application Server (32.45.67.85): HTTP Request
४. Server → Browser: HTTP Response
```

---

## ४. Route 53 Routing Policies ⭐⭐

**Cloud Practitioner मा उच्च-स्तरमा जान्नुपर्छ।**

| Policy | विवरण |
|---|---|
| **Simple Routing Policy** | एउटै record मा एक वा धेरै IP। ⚠️ **Health check हुँदैन** |
| **Weighted Routing Policy** | Traffic प्रतिशतमा बाँड्ने — जस्तै ७०% / २०% / १०% |
| **Latency Routing Policy** | प्रयोगकर्तालाई **सबैभन्दा कम latency** भएको server मा पठाउने |
| **Failover Routing Policy** | **Disaster Recovery** — Primary मा health check, बिग्रिए Failover मा पठाउने |

> **परीक्षा टिप:**
> - **"Blue-Green deployment / traffic को % बाँड्ने"** → **Weighted**
> - **"सबैभन्दा छिटो प्रतिक्रिया"** → **Latency**
> - **"Disaster Recovery / backup server"** → **Failover**

---

## ५. Amazon CloudFront ⭐⭐

**CloudFront = Content Delivery Network (CDN)**

| विशेषता | विवरण |
|---|---|
| प्रदर्शन | **पढ्ने प्रदर्शन सुधार** — सामग्री edge मा cache हुन्छ |
| अनुभव | प्रयोगकर्ताको अनुभव सुधार |
| फैलावट | संसारभर **सयौं Points of Presence** (edge location, cache) |
| सुरक्षा | **DDoS सुरक्षा** (विश्वव्यापी भएकोले), **AWS Shield** र **AWS WAF** सँग एकीकरण |

### CloudFront का Origin ⭐

| Origin प्रकार | विवरण |
|---|---|
| **S3 bucket** | फाइल वितरण र edge मा cache गर्न; CloudFront मार्फत S3 मा upload गर्न। **Origin Access Control (OAC)** ले सुरक्षित |
| **VPC Origin** | VPC को private subnet मा भएका एप — Private ALB / NLB / EC2 Instance |
| **Custom Origin (HTTP)** | S3 website (पहिले static S3 website सक्षम गर्नुपर्ने), वा जुनसुकै public HTTP backend (जस्तै Public ALB) |

### CloudFront कसरी काम गर्छ

```
Client → CloudFront Edge Location
             ├─ Local Cache मा छ? → तुरुन्तै दिन्छ
             └─ छैन? → Origin (S3 वा HTTP) मा request पठाउँछ → cache गर्छ → दिन्छ
```

### CloudFront vs S3 Cross Region Replication ⭐⭐

| | **CloudFront** | **S3 Cross Region Replication** |
|---|---|---|
| नेटवर्क | **Global Edge network** | प्रत्येक region को लागि छुट्टै सेटअप गर्नुपर्ने |
| डाटा ताजापन | फाइल **TTL** सम्म cache हुन्छ (सायद १ दिन) | फाइल **near real-time** मा update हुन्छ |
| पहुँच | Read | **Read only** |
| उपयुक्त | **Static सामग्री** जुन सबैतिर उपलब्ध हुनुपर्छ | **Dynamic सामग्री** जुन थोरै region मा कम latency मा चाहिन्छ |

---

## ६. S3 Transfer Acceleration ⭐

**फाइल पहिले AWS edge location मा पठाएर, त्यहाँबाट लक्षित region को S3 bucket सम्म पुर्‍याएर transfer speed बढाउने।**

```
File in USA → [छिटो, public www] Edge Location USA → [छिटो, private AWS] S3 Bucket Australia
```

👉 मुख्य कुरा: सार्वजनिक इन्टरनेटको दूरी घट्छ, AWS को छिटो private network बढी प्रयोग हुन्छ।

---

## ७. AWS Global Accelerator ⭐⭐

**AWS को global network प्रयोग गरेर global एप्लिकेसनको उपलब्धता र प्रदर्शन सुधार्ने।**

- AWS को **आन्तरिक network** प्रयोग गरेर एपसम्मको बाटो अनुकूलित (**६०% सम्म सुधार**)
- तपाईंको एपको लागि **२ वटा Anycast IP** बन्छन्
- Traffic **Edge Location** मार्फत पठाइन्छ
- Edge location ले traffic तपाईंको एपमा पठाउँछ

### Global Accelerator vs CloudFront ⭐⭐

**दुवैले** AWS global network र edge location प्रयोग गर्छन्। **दुवै** AWS Shield सँग DDoS सुरक्षाको लागि एकीकृत छन्।

| | **CloudFront** | **Global Accelerator** |
|---|---|---|
| प्रकार | **Content Delivery Network** | **Network layer accelerator** |
| Caching | ✅ **cache गर्छ** | ❌ **cache गर्दैन** — edge मा packet proxy मात्र गर्छ |
| के सुधार्छ | **Cacheable सामग्री** (image, video) को प्रदर्शन | **TCP वा UDP** मा चल्ने धेरै किसिमका एपको प्रदर्शन |
| सामग्री कहाँ | Edge मै serve हुन्छ | एक वा बढी AWS Region मा चलिरहेको एपमा जान्छ |
| Static IP | ❌ | ✅ **Static IP चाहिने HTTP प्रयोगको लागि राम्रो** |
| Failover | — | ✅ **छिटो, निश्चित regional failover चाहिने HTTP प्रयोगको लागि राम्रो** |

> **सम्झने:**
> - **CloudFront = cache गर्छ** (static सामग्री)
> - **Global Accelerator = cache गर्दैन, बाटो छिटो बनाउँछ** (TCP/UDP एप, static IP, failover)

---

## ८. AWS Outposts ⭐

**Hybrid Cloud** = व्यवसायले on-premises infrastructure र cloud infrastructure सँगै राख्नु।

**समस्या:** त्यसो हुँदा IT सिस्टम सम्हाल्ने **दुई फरक तरिका** हुन्छन्:
- AWS cloud को लागि (Console, CLI, AWS API)
- आफ्नै on-premises infrastructure को लागि

**समाधान: AWS Outposts**
- **"Server rack"** हरू जसले **उही AWS infrastructure, सेवा, API र उपकरण** on-premises मै दिन्छन्
- AWS ले तपाईंको on-premises infrastructure भित्र **"Outposts Rack" सेटअप र व्यवस्थापन** गर्छ
- अनि तपाईंले AWS सेवा on-premises मै प्रयोग गर्न सक्नुहुन्छ
- ⚠️ **Outposts Rack को भौतिक सुरक्षा तपाईंको जिम्मा हो** (यो परीक्षामा सोधिन्छ)

### फाइदाहरू
- On-premises सिस्टममा **कम latency** पहुँच
- **स्थानीय डाटा प्रशोधन**
- **Data residency** (डाटा देशभित्रै राख्नुपर्ने)
- On-premises बाट क्लाउडमा **सजिलो migration**
- **पूर्ण managed सेवा**

**Outposts मा चल्ने केही सेवा:** Amazon EC2, EBS, S3, EKS, ECS, RDS, EMR

---

## ९. AWS WaveLength ⭐

- **WaveLength Zone** = टेलिकम प्रदायकको data center भित्रै, **5G network को edge** मा राखिएको infrastructure
- **5G network को edge मा AWS सेवा** ल्याउँछ (जस्तै EC2, EBS, VPC)
- **5G network मार्फत अति कम latency** भएका एप
- ⭐ Traffic ले **Communication Service Provider (CSP) को network छाड्दैन**
- Parent AWS Region सँग **उच्च-bandwidth र सुरक्षित कनेक्सन**
- **कुनै अतिरिक्त शुल्क वा सेवा सम्झौता छैन**

**प्रयोग:** Smart City, ML-सहायित निदान, जोडिएका सवारी साधन, अन्तरक्रियात्मक Live Video Stream, AR/VR, Real-time Gaming

> **परीक्षा टिप:** प्रश्नमा **"5G"** आयो → **WaveLength**

---

## १०. AWS Local Zones ⭐

- AWS को **compute, storage, database र केही सेवा प्रयोगकर्ताको नजिक** राख्ने — latency-sensitive एप चलाउन
- ⭐ **तपाईंको VPC लाई थप स्थानमा विस्तार गर्ने** — "AWS Region को विस्तार"
- मिल्ने सेवा: **EC2, RDS, ECS, EBS, ElastiCache, Direct Connect**

**उदाहरण:**
- AWS Region: N. Virginia (`us-east-1`)
- AWS Local Zones: Boston, Chicago, Dallas, Houston, Miami…

> **सम्झने:** **Local Zone = Region को विस्तार** (उही देशभित्र, प्रयोगकर्ताको नजिक)

---

## ११. Global Application Architecture ⭐

| आर्किटेक्चर | High Availability | Global Latency | कठिनाइ |
|---|---|---|---|
| **Single Region, Single AZ** | ❌ | ❌ | सजिलो |
| **Single Region, Multi AZ** | ✅ | ❌ | मध्यम |
| **Multi Region, Active-Passive** | ✅ | Global **read** मा राम्रो, **write** मा नराम्रो | गाह्रो |
| **Multi Region, Active-Active** | ✅ | Global **read र write** दुवैमा राम्रो | सबैभन्दा गाह्रो |

```
Active-Passive:  Region A (Active) ⟷ Region B (Passive)
                 User: लेख्ने → A मात्र, पढ्ने → दुवै

Active-Active:   Region A (Active) ⟷ Region B (Active)
                 User: लेख्ने र पढ्ने → दुवैमा
```

---

## 📝 अभ्यास प्रश्नहरू

**प्रश्न १.** एउटा कम्पनीको वेबसाइटमा संसारभरका प्रयोगकर्ताले image र video हेर्छन्। Latency घटाउन कुन सेवा उत्तम?

A) AWS Global Accelerator
B) Amazon CloudFront
C) Amazon Route 53
D) AWS Direct Connect

<details><summary>उत्तर</summary>

**B) Amazon CloudFront** — cacheable static सामग्री (image, video) को लागि CDN उत्तम।
</details>

---

**प्रश्न २.** एउटा gaming एप UDP प्रयोग गर्छ र static IP चाहिन्छ, साथै छिटो regional failover चाहिन्छ। कुन सेवा?

A) Amazon CloudFront
B) AWS Global Accelerator
C) Amazon Route 53
D) Elastic Load Balancer

<details><summary>उत्तर</summary>

**B) AWS Global Accelerator** — TCP/UDP, static IP (Anycast), छिटो deterministic failover। CloudFront ले cache गर्छ, यहाँ cache चाहिँदैन।
</details>

---

**प्रश्न ३.** Data residency कानुनका कारण डाटा देशभित्रै आफ्नै data center मा राख्नुपर्छ तर AWS सेवा प्रयोग गर्न चाहिन्छ। कुन समाधान?

A) AWS Local Zones
B) AWS WaveLength
C) AWS Outposts
D) Amazon CloudFront

<details><summary>उत्तर</summary>

**C) AWS Outposts** — AWS ले तपाईंको आफ्नै data center मा server rack राख्छ, त्यहीँबाट AWS सेवा चल्छ।
</details>

---

**प्रश्न ४.** नयाँ version मा १०% traffic र पुरानो version मा ९०% traffic पठाउनुपर्‍यो। कुन Route 53 routing policy?

A) Simple
B) Weighted
C) Latency
D) Failover

<details><summary>उत्तर</summary>

**B) Weighted** — प्रतिशतमा traffic बाँड्ने।
</details>

---

**प्रश्न ५.** 5G network को edge मा अति कम latency भएको एप चलाउनुपर्‍यो। कुन सेवा?

A) AWS Local Zones
B) AWS WaveLength
C) AWS Outposts
D) Amazon CloudFront

<details><summary>उत्तर</summary>

**B) AWS WaveLength** — 5G network को edge मा AWS सेवा।
</details>

---

**प्रश्न ६.** Australia को S3 bucket मा USA बाट ठूलो फाइल छिटो upload गर्नुपर्‍यो। कुन सुविधा?

A) S3 Cross-Region Replication
B) S3 Transfer Acceleration
C) AWS Snowball
D) Amazon CloudFront

<details><summary>उत्तर</summary>

**B) S3 Transfer Acceleration** — नजिकको edge location मार्फत पठाएर AWS को private network प्रयोग गर्छ।
</details>

---

**प्रश्न ७.** Route 53 मा कुन record ले hostname लाई अर्को hostname मा जोड्छ?

A) A record
B) AAAA record
C) CNAME
D) MX record

<details><summary>उत्तर</summary>

**C) CNAME** — hostname to hostname। A = IPv4, AAAA = IPv6, Alias = AWS संसाधन।
</details>

---

## ✅ यो मोड्युलको सार

### Global Application सेवाहरू
| सेवा | काम |
|---|---|
| **Route 53** (Global DNS) | सबैभन्दा कम latency भएको deployment मा प्रयोगकर्ता पठाउने; DR रणनीति |
| **CloudFront** (Global CDN) | Edge Location मा सामग्री replicate र **cache** गर्ने — latency घटाउने |
| **S3 Transfer Acceleration** | S3 मा global upload/download छिटो बनाउने |
| **AWS Global Accelerator** | AWS global network प्रयोग गरेर उपलब्धता र प्रदर्शन सुधार्ने (**cache गर्दैन**) |

### Hybrid र Edge सेवाहरू
| सेवा | काम |
|---|---|
| **AWS Outposts** | आफ्नै Data Center मा Outposts Rack राखेर AWS सेवा विस्तार गर्ने |
| **AWS WaveLength** | **5G network** को edge मा AWS सेवा — अति कम latency |
| **AWS Local Zones** | AWS संसाधन (compute, database, storage) प्रयोगकर्ताको नजिक ल्याउने — latency-sensitive एपको लागि |

---

⬅️ [मोड्युल ०९](09-deployment.md) | ➡️ [मोड्युल ११ — Cloud Integration](11-cloud-integration.md)
