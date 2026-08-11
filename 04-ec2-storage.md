# मोड्युल ०४ — EC2 Instance Storage

> **यस मोड्युलमा:** EBS, Snapshot, AMI, EC2 Image Builder, Instance Store, EFS, EFS-IA, र FSx।

---

## १. EBS Volume (Elastic Block Store) ⭐

> **EBS Volume = चलिरहेको instance मा जोड्न मिल्ने network drive**

**मुख्य विशेषताहरू:**
- Instance terminate भए पनि **डाटा बाँकी रहन्छ** (persist)
- **एक पटकमा एउटै instance मा मात्र जोड्न मिल्छ** (Cloud Practitioner तहमा)
- **एउटा Availability Zone मा बाँधिएको** हुन्छ

> 🔌 **उपमा:** EBS भनेको "**network USB stick**" जस्तै हो। निकालेर अर्को कम्प्युटरमा हाल्न मिल्छ — तर उही कोठा (AZ) भित्रको मात्र।

### गहिरो कुरा
- यो **network drive** हो — भौतिक drive होइन
- नेटवर्क प्रयोग गर्ने भएकोले **अलिकति latency** हुन सक्छ
- एउटा EC2 बाट **छिटै निकालेर अर्को EC2 मा जोड्न** सकिन्छ
- **AZ मा lock** — `us-east-1a` को EBS `us-east-1b` मा जोड्न मिल्दैन
  - सार्न पहिले **snapshot** लिनुपर्छ
- **Provisioned capacity** हुन्छ (GB मा आकार, र IOPS)
- **provision गरेको सबै क्षमताको पैसा लाग्छ** (प्रयोग गरे पनि नगरे पनि)
- समयसँगै drive को क्षमता **बढाउन** सकिन्छ

### Delete on Termination attribute ⭐

EC2 terminate हुँदा EBS को के हुने भन्ने नियन्त्रण गर्छ:

| Volume | पूर्वनिर्धारित व्यवहार |
|---|---|
| **Root EBS volume** | **मेटिन्छ** (attribute सक्षम) |
| **अन्य जोडिएका EBS volume** | **मेटिँदैन** (attribute असक्षम) |

Console वा CLI बाट बदल्न सकिन्छ।
**प्रयोग:** instance terminate हुँदा पनि root volume जोगाउन।

---

## २. EBS Snapshots ⭐

**Snapshot = कुनै समयको EBS volume को backup**

- Snapshot लिन volume **detach गर्नै पर्दैन**, तर गर्नु राम्रो
- **AZ वा Region बीच copy गर्न मिल्छ** — यसैले EBS सार्ने तरिका यही हो

```
us-east-1a: EBS (50 GB) ──snapshot──> EBS Snapshot ──restore──> us-east-1b: EBS (50 GB)
```

### Snapshot का दुई सुविधा

| सुविधा | विवरण |
|---|---|
| **EBS Snapshot Archive** | Snapshot लाई "archive tier" मा सार्ने — **७५% सस्तो**। तर फिर्ता ल्याउन **२४ देखि ७२ घण्टा** लाग्छ |
| **Recycle Bin for EBS Snapshots** | गल्तीले मेटिएको snapshot फिर्ता ल्याउन नियम बनाउने। **Retention: १ दिनदेखि १ वर्षसम्म** |

---

## ३. AMI (Amazon Machine Image) ⭐

**AMI = EC2 instance को customization**

- आफ्नै software, configuration, OS, monitoring थप्न सकिने
- सबै software पहिल्यै प्याक भएकोले **boot र configuration छिटो**
- **AMI निश्चित region को लागि बन्छ** (तर अर्को region मा copy गर्न मिल्छ)

### AMI का ३ स्रोत

| प्रकार | विवरण |
|---|---|
| **Public AMI** | AWS ले दिएको |
| **Your own AMI** | तपाईंले आफैं बनाउने र मर्मत गर्ने |
| **AWS Marketplace AMI** | अरूले बनाएको (बेच्न पनि सक्छन्) |

### AMI बनाउने प्रक्रिया
1. EC2 instance सुरु गर्ने र customize गर्ने
2. Instance **stop** गर्ने (डाटाको अखण्डताको लागि)
3. **AMI बनाउने** — यसले EBS snapshot पनि बनाउँछ
4. त्यो AMI बाट अन्य instance launch गर्ने

---

## ४. EC2 Image Builder

- **Virtual Machine वा container image** बनाउने काम स्वचालित गर्ने
- AMI को **निर्माण, मर्मत, प्रमाणीकरण र परीक्षण** स्वचालित
- **तालिका अनुसार** चलाउन सकिने (साप्ताहिक, package update हुँदा…)
- **निःशुल्क सेवा** — तल्लो संसाधनको मात्र पैसा

```
EC2 Image Builder → Builder EC2 Instance बनाउँछ
                  → Build Components लागू (software customize)
                  → नयाँ AMI बनाउँछ
                  → Test EC2 Instance मा परीक्षण (AMI ठीक छ? सुरक्षित छ?)
                  → AMI वितरण (धेरै region मा हुन सक्छ)
```

---

## ५. EC2 Instance Store ⭐

EBS network drive हो — प्रदर्शन राम्रो तर "सीमित"। धेरै छिटो चाहिने भए **EC2 Instance Store** प्रयोग गर्नुहोस्।

| पक्ष | विवरण |
|---|---|
| ✅ फाइदा | **धेरै राम्रो I/O प्रदर्शन**, अत्यधिक उच्च IOPS — भौतिक disk नै हो |
| ❌ बेफाइदा | **Instance stop भए storage हराउँछ (ephemeral)** |
| उपयोग | Buffer, cache, scratch data, अस्थायी सामग्री |
| जोखिम | Hardware बिग्रिए डाटा हराउने |
| जिम्मेवारी | **Backup र Replication तपाईंको जिम्मा** |

> **परीक्षा टिप:** "उच्च प्रदर्शन चाहिने तर अस्थायी डाटा" → **EC2 Instance Store**। "डाटा जोगिनुपर्ने" → **EBS**।

---

## ६. EFS — Elastic File System ⭐

**EFS = Managed NFS (Network File System)**

| विशेषता | विवरण |
|---|---|
| कति instance मा जोड्न मिल्छ | **सयौं EC2 मा एकैचोटि** |
| कुन OS | **Linux EC2 मात्र** |
| AZ | **Multi-AZ** मा काम गर्छ |
| उपलब्धता | Highly available, scalable |
| मूल्य | **महँगो — gp2 भन्दा ३ गुणा**, pay per use |
| Capacity planning | **चाहिँदैन** — आफैं बढ्छ/घट्छ |

### EBS vs EFS ⭐⭐

| | **EBS** | **EFS** |
|---|---|---|
| प्रकार | Block storage | File storage (NFS) |
| कति instance | **एक पटकमा एउटा** | **सयौं एकैचोटि** |
| AZ | **एउटै AZ मा बाँधिएको** | **Multi-AZ** |
| OS | Linux र Windows | **Linux मात्र** |
| सार्ने तरिका | Snapshot लिनुपर्छ | आवश्यक छैन |
| मूल्य | सस्तो | महँगो (३x) |
| Capacity | पहिल्यै provision गर्नुपर्ने | आफैं बढ्ने |

### EFS-IA (Infrequent Access) ⭐
- दैनिक पहुँच नहुने फाइलको लागि किफायती storage class
- EFS Standard भन्दा **९२% सम्म सस्तो**
- **अन्तिम पहुँचको समय** हेरेर EFS ले आफैं फाइल EFS-IA मा सार्छ
- **Lifecycle Policy** ले सक्षम गर्ने
  - उदाहरण: ६० दिनसम्म पहुँच नभएका फाइल EFS-IA मा सार्ने
- एप्लिकेसनको लागि **पारदर्शी** — कुनै कोड बदल्नुपर्दैन

---

## ७. AWS Storage को वर्गीकरण ⭐

```
BLOCK (ब्लक)            FILE (फाइल)         OBJECT (अब्जेक्ट)
─────────────           ────────────         ─────────────
Amazon EBS              Amazon EFS           Amazon S3
EC2 Instance Store                           S3 Glacier
```

---

## ८. Amazon FSx ⭐

**FSx = AWS मा तेस्रो पक्षको उच्च-प्रदर्शन file system चलाउने। पूर्ण managed सेवा।**

### ३ प्रकार

| FSx प्रकार | विवरण |
|---|---|
| **FSx for Windows File Server** | Windows को native shared file system |
| **FSx for Lustre** | High Performance Computing (HPC) को लागि |
| **FSx for NetApp ONTAP** | NetApp को file system |

### FSx for Windows File Server
- पूर्ण managed, अत्यन्त भरपर्दो, scalable **Windows native shared file system**
- Windows File Server मा बनेको
- **SMB प्रोटोकल र Windows NTFS** समर्थन
- **Microsoft Active Directory** सँग एकीकृत
- AWS बाट वा आफ्नै on-premise infrastructure बाट पहुँच गर्न मिल्ने

### FSx for Lustre
- पूर्ण managed, उच्च-प्रदर्शन, scalable file storage — **HPC** को लागि
- नाम कहाँबाट: **"Linux" + "cluster" = Lustre**
- प्रयोग: Machine Learning, Analytics, Video Processing, Financial Modeling
- **सयौं GB/s, लाखौं IOPS, sub-millisecond latency** सम्म scale हुने
- Amazon S3 सँग जोड्न मिल्छ

> **सम्झने:** **Windows → FSx for Windows**, **Linux + HPC → FSx for Lustre**

---

## ९. EC2 Storage को Shared Responsibility Model

| **AWS को जिम्मा** | **तपाईंको जिम्मा** |
|---|---|
| Infrastructure | Backup / snapshot प्रक्रिया सेटअप गर्ने |
| EBS volume र EFS drive को डाटा replication | Data encryption सेटअप गर्ने |
| बिग्रिएको hardware फेर्ने | Drive मा भएको डाटाको जिम्मेवारी |
| AWS कर्मचारीले तपाईंको डाटा पहुँच गर्न नसक्ने सुनिश्चित गर्ने | EC2 Instance Store प्रयोगको जोखिम बुझ्ने |

---

## 📝 अभ्यास प्रश्नहरू

**प्रश्न १.** एउटा EBS volume `us-east-1a` मा छ। यसलाई `us-east-1b` मा सार्न के गर्नुपर्छ?

A) सिधै detach गरेर attach गर्ने
B) Snapshot लिएर अर्को AZ मा restore गर्ने
C) EFS मा convert गर्ने
D) EBS volume AZ बीच सार्न मिल्दैन

<details><summary>उत्तर</summary>

**B)** — EBS AZ मा बाँधिएको हुन्छ। सार्न snapshot लिएर अर्को AZ मा restore गर्नुपर्छ।
</details>

---

**प्रश्न २.** १०० वटा Linux EC2 instance ले एउटै file system साझा गर्नुपर्‍यो। कुन सेवा प्रयोग गर्ने?

A) Amazon EBS
B) EC2 Instance Store
C) Amazon EFS
D) Amazon S3 Glacier

<details><summary>उत्तर</summary>

**C) Amazon EFS** — सयौं Linux EC2 मा एकैचोटि mount गर्न मिल्छ, multi-AZ।
</details>

---

**प्रश्न ३.** EC2 Instance Store बारे कुन सही हो?

A) Instance stop भए पनि डाटा रहन्छ
B) उच्च IOPS दिन्छ तर instance stop भए डाटा हराउँछ
C) धेरै instance मा एकैचोटि जोड्न मिल्छ
D) AWS ले automatic backup लिन्छ

<details><summary>उत्तर</summary>

**B)** — Instance Store ephemeral हो। उच्च प्रदर्शन दिन्छ तर डाटा स्थायी हुँदैन।
</details>

---

**प्रश्न ४.** पूर्वनिर्धारित रूपमा EC2 terminate हुँदा के हुन्छ?

A) सबै EBS volume मेटिन्छन्
B) कुनै EBS volume मेटिँदैन
C) Root volume मेटिन्छ, अन्य जोडिएका volume मेटिँदैनन्
D) Root volume रहन्छ, अन्य मेटिन्छन्

<details><summary>उत्तर</summary>

**C)** — Root EBS volume को "Delete on Termination" पूर्वनिर्धारित रूपमा सक्षम हुन्छ, अन्य volume को असक्षम।
</details>

---

**प्रश्न ५.** High Performance Computing (HPC) को लागि Linux मा file system चाहियो। कुन सेवा?

A) FSx for Windows File Server
B) FSx for Lustre
C) Amazon EFS
D) Amazon EBS

<details><summary>उत्तर</summary>

**B) FSx for Lustre** — नाम नै "Linux + cluster" बाट आएको, HPC को लागि बनेको।
</details>

---

**प्रश्न ६.** AMI बारे कुन सही हो?

A) AMI global हुन्छ, सबै region मा उपलब्ध
B) AMI निश्चित region को लागि बन्छ, तर अर्को region मा copy गर्न मिल्छ
C) AMI बनाउन instance चलिरहेकै हुनुपर्छ
D) AMI ले EBS snapshot बनाउँदैन

<details><summary>उत्तर</summary>

**B)** — AMI region-specific हो, तर copy गर्न मिल्छ। बनाउँदा instance stop गर्नु राम्रो, र AMI ले EBS snapshot पनि बनाउँछ।
</details>

---

## ✅ यो मोड्युलको सार

- **EBS volume:** एक पटकमा एउटा EC2 मा जोडिने network drive, **AZ मा बाँधिएको**। Backup र AZ बीच सार्न **EBS Snapshot**
- **AMI:** आफ्नो customization सहितको तयार EC2 image
- **EC2 Image Builder:** AMI आफैं बनाउने, परीक्षण गर्ने र वितरण गर्ने
- **EC2 Instance Store:** उच्च प्रदर्शनको hardware disk, तर **stop/terminate भए हराउँछ**
- **EFS:** network file system, एउटै region मा सयौं instance मा जोडिने (Linux मात्र)
- **EFS-IA:** कम प्रयोग हुने फाइलको किफायती storage class
- **FSx for Windows:** Windows server को लागि Network File System
- **FSx for Lustre:** HPC को लागि उच्च-प्रदर्शन Linux file system

---

⬅️ [मोड्युल ०३](03-ec2.md) | ➡️ [मोड्युल ०५ — ELB र ASG](05-elb-asg.md)
