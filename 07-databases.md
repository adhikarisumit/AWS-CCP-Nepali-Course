# मोड्युल ०७ — Databases र Analytics

> ⭐ **परीक्षामा धेरै सोधिने मोड्युल। "कुन डाटाबेस कहाँ प्रयोग गर्ने" भन्ने प्रश्न धेरै आउँछ।**

---

## १. डाटाबेस किन चाहिन्छ?

Disk (EFS, EBS, EC2 Instance Store, S3) मा डाटा राख्दा सीमा हुन्छ। डाटाबेसले दिन्छ:
- डाटालाई **संरचना** दिन
- छिटो खोज्न **index** बनाउन
- डाटासेटहरू बीच **सम्बन्ध** परिभाषित गर्न

डाटाबेसहरू **निश्चित उद्देश्यको लागि अनुकूलित** हुन्छन् — फरक-फरक विशेषता, आकार र बन्धन सहित।

---

## २. Relational vs NoSQL

### 📊 Relational Database
- Excel स्प्रेडसिट जस्तै — तर तालिकाहरू बीच सम्बन्ध हुन्छ
- **SQL भाषा** ले query गर्न सकिने

```
Students टेबल        Subjects टेबल      Departments टेबल
Student ID | Dept ID  Student ID|Subject  Dept ID | SPOC
    1      |  M01         1     |Physics    M01   | Kelly Jones
    2      |  B01         1     |Math       B01   | Satish Kumar
```

### 📁 NoSQL Database
**NoSQL = non-SQL = non-relational database**

निश्चित data model को लागि बनेको, **लचिलो schema** भएको।

**फाइदाहरू:**
- **Flexibility** — data model सजिलै विकास गर्न सकिने
- **Scalability** — distributed cluster मार्फत scale-out गर्न बनेको
- **High-performance** — निश्चित data model को लागि अनुकूलित
- **Highly functional** — data model अनुसार अनुकूलित प्रकार

**उदाहरण:** Key-value, document, graph, in-memory, search database

### JSON — NoSQL को सामान्य रूप

```json
{
    "name": "John",
    "age": 30,
    "cars": ["Ford", "BMW", "Fiat"],
    "address": {
        "type": "house",
        "number": 23,
        "street": "Dream Road"
    }
}
```

- डाटा **nested** हुन सक्छ
- Field समयसँगै बदलिन सक्छ
- नयाँ प्रकार (array आदि) समर्थन

---

## ३. AWS मा Managed Database — किन?

AWS ले डाटाबेस managed गरिदिँदा फाइदा:
- छिटो Provisioning, High Availability, Vertical र Horizontal Scaling
- **स्वचालित Backup र Restore**, Operations, Upgrades
- **OS Patching AWS ले गर्छ**
- Monitoring, alerting

> ⚠️ **धेरै डाटाबेस EC2 मा पनि चलाउन सकिन्छ — तर त्यसो गर्दा resiliency, backup, patching, high availability, fault tolerance र scaling सबै तपाईंले आफैं सम्हाल्नुपर्छ।**

---

## ४. Amazon RDS ⭐⭐

**RDS = Relational Database Service** — SQL query भाषा प्रयोग गर्ने डाटाबेसको managed सेवा।

### समर्थित engine (७ वटा) ⭐
1. **PostgreSQL**
2. **MySQL**
3. **MariaDB**
4. **Oracle**
5. **Microsoft SQL Server**
6. **IBM DB2**
7. **Aurora** (AWS को आफ्नै proprietary डाटाबेस)

### RDS vs EC2 मा DB चलाउने — RDS को फाइदा ⭐

| फाइदा | विवरण |
|---|---|
| स्वचालित provisioning, **OS patching** | AWS ले गर्छ |
| **निरन्तर backup** र निश्चित समयमा restore | **Point in Time Restore** |
| Monitoring dashboard | तयार |
| **Read Replica** | पढ्ने प्रदर्शन सुधार |
| **Multi-AZ setup** | Disaster Recovery |
| Maintenance window | Upgrade को लागि |
| Scaling क्षमता | Vertical र Horizontal |
| Storage | EBS ले समर्थित |

> ⚠️ **तर तपाईंले RDS instance मा SSH गर्न पाउनुहुन्न!** यो परीक्षामा सोधिन्छ।

### RDS को सामान्य आर्किटेक्चर

```
User → Elastic Load Balancer → EC2 Instances (ASG मा) → RDS (SQL Database)
                                                          read/write
```

---

## ५. Amazon Aurora ⭐

**Aurora = AWS को proprietary प्रविधि (open source होइन)**

| विशेषता | विवरण |
|---|---|
| समर्थित | **PostgreSQL र MySQL** |
| प्रदर्शन | RDS को MySQL भन्दा **५ गुणा**, RDS को Postgres भन्दा **३ गुणा** छिटो |
| Storage | **१० GB को increment मा आफैं बढ्छ, २५६ TB सम्म** |
| मूल्य | RDS भन्दा **२०% बढी** — तर बढी कुशल |

### Amazon Aurora Serverless ⭐
- **डाटाबेस स्वचालित रूपमा बन्ने र वास्तविक प्रयोग अनुसार auto-scale हुने**
- PostgreSQL र MySQL दुवै समर्थित
- **Capacity planning चाहिँदैन**
- **सबैभन्दा कम व्यवस्थापन भार**
- **प्रति सेकेन्ड भुक्तानी** — बढी किफायती हुन सक्ने
- **प्रयोग:** कम, रुक-रुक गरेर वा अनुमान गर्न नसकिने workload

> **परीक्षा टिप:** "अनुमान गर्न नसकिने / रुक-रुक गरेर हुने workload + relational DB + capacity planning नचाहिने" → **Aurora Serverless**

---

## ६. RDS Deployment: Read Replica, Multi-AZ, Multi-Region ⭐⭐

**यो तीनको फरक परीक्षामा पक्कै सोधिन्छ।**

| | **Read Replica** | **Multi-AZ** | **Multi-Region** |
|---|---|---|---|
| **उद्देश्य** | पढ्ने workload scale गर्ने | **AZ बिग्रिए failover** (High Availability) | **Region बिग्रिए Disaster Recovery** + global पाठकको लागि local प्रदर्शन |
| **कति बनाउन सकिने** | **१५ सम्म** | **१ अर्को AZ मात्र** (failover) | धेरै region |
| **लेख्ने** | मुख्य DB मा मात्र | मुख्य DB मा मात्र | प्रत्येक region मा आफ्नै |
| **पढ्ने** | Replica बाट पढ्न सकिने | Failover DB बाट पढ्न **मिल्दैन** | स्थानीय replica बाट |
| **लागत** | Replication लागत | Replication cross-AZ | **Replication लागत बढी** |

```
Read Replica:      Main ──replication──> Read Replica × 15 (पढ्न मिल्ने)
Multi-AZ:          Main ──replication──> Failover DB (पढ्न मिल्दैन, failover मात्र)
Multi-Region:      us-east-2 (Main) ──> eu-west-1 (Replica) ──> ap-southeast-2 (Replica)
```

> **सम्झने:**
> - **Read Replica → प्रदर्शन (performance)**
> - **Multi-AZ → उपलब्धता (availability / failover)**
> - **Multi-Region → प्रकोप पुनर्प्राप्ति (disaster recovery) + global latency**

---

## ७. Amazon ElastiCache ⭐

RDS ले managed relational database दिन्छ भने **ElastiCache ले managed Redis वा Memcached** दिन्छ।

- **Cache = in-memory database** — उच्च प्रदर्शन, कम latency
- **धेरै पढ्ने workload मा डाटाबेसको भार घटाउन** सहयोग गर्छ
- AWS ले OS maintenance/patching, optimization, setup, configuration, monitoring, failure recovery र backup सम्हाल्छ

```
ELB → EC2 (ASG) ──छिटो──> ElastiCache (in-memory) ← cache मा भेटिएन भने
                └──ढिलो──> RDS (SQL Database)
```

---

## ८. Amazon DynamoDB ⭐⭐

**DynamoDB = पूर्ण managed, serverless, NoSQL key/value database**

| विशेषता | विवरण |
|---|---|
| उपलब्धता | **३ AZ मा replication** सहित highly available |
| प्रकार | **NoSQL** — relational होइन |
| Scale | विशाल workload सम्हाल्ने, distributed "serverless" database |
| क्षमता | **प्रति सेकेन्ड लाखौं request, खरबौं row, सयौं TB storage** |
| Latency | **Single-digit millisecond** — अति कम |
| सुरक्षा | **IAM** सँग एकीकृत |
| लागत | कम लागत, **auto scaling** |
| Table Class | **Standard र Infrequent Access (IA)** |

### DynamoDB Accelerator — DAX ⭐

- **DynamoDB को लागि पूर्ण managed in-memory cache**
- **१० गुणा प्रदर्शन सुधार** — millisecond बाट **microsecond** latency मा
- सुरक्षित, अति scalable, अति उपलब्ध

> ⚠️ **DAX vs ElastiCache को फरक (परीक्षामा सोधिन्छ):**
> **DAX केवल DynamoDB को लागि मात्र** हो र त्यसैसँग एकीकृत छ।
> **ElastiCache अन्य डाटाबेससँग पनि** प्रयोग गर्न सकिन्छ।

### DynamoDB Global Tables ⭐
- **धेरै region मा कम latency** मा DynamoDB table पहुँचयोग्य बनाउने
- **Active-Active replication** — जुनसुकै AWS Region मा पढ्न/लेख्न मिल्ने

```
N. Virginia (us-east-1) ←──२-तर्फी replication──→ Paris (eu-west-3)
```

---

## ९. Amazon Redshift ⭐

**Redshift = Data Warehouse (OLAP)**

| विशेषता | विवरण |
|---|---|
| आधार | **PostgreSQL** मा आधारित — तर **OLTP को लागि होइन** |
| प्रकार | **OLAP** — Online Analytical Processing (analytics र data warehousing) |
| डाटा लोड | **हरेक घण्टा** एक पटक, हरेक सेकेन्ड होइन |
| प्रदर्शन | अन्य data warehouse भन्दा **१० गुणा राम्रो**, PB सम्म scale |
| Storage | **Columnar storage** (row-based होइन) |
| Query | **Massively Parallel Query Execution (MPP)**, highly available |
| मूल्य | provision गरेको instance अनुसार pay as you go |
| Interface | **SQL interface** |
| BI उपकरण | **AWS QuickSight** वा **Tableau** सँग एकीकृत |

> ⭐ **OLTP vs OLAP सम्झने:**
> - **OLTP** = **T**ransaction = दैनिक लेनदेन = **RDS / Aurora**
> - **OLAP** = **A**nalytics = विश्लेषण/रिपोर्ट = **Redshift**

### Redshift Serverless
- Data warehouse को तल्लो क्षमता **आफैं provision र scale** हुने
- Infrastructure व्यवस्थापन नगरी analytics चलाउने
- **जति प्रयोग गर्‍यो त्यति मात्र भुक्तानी**
- **प्रयोग:** Reporting, dashboarding application, real-time analytics

---

## १०. Amazon EMR

**EMR = Elastic MapReduce**

- **Hadoop cluster** बनाएर विशाल डाटा विश्लेषण र प्रशोधन गर्न सहयोग
- Cluster मा **सयौं EC2 instance** हुन सक्छन्
- समर्थन: **Apache Spark, HBase, Presto, Flink**
- EMR ले सबै provisioning र configuration सम्हाल्छ
- **Auto-scaling** र **Spot instance** सँग एकीकृत
- **प्रयोग:** data processing, machine learning, web indexing, big data

> **परीक्षा टिप:** प्रश्नमा **"Hadoop"** भन्ने शब्द आयो भने उत्तर **EMR** हो।

---

## ११. Amazon Athena ⭐

**Athena = Amazon S3 मा भएको डाटा विश्लेषण गर्ने Serverless query सेवा**

- **Standard SQL** ले फाइल query गर्ने
- ढाँचा: **CSV, JSON, ORC, Avro, Parquet** (Presto मा बनेको)
- **मूल्य: $5.00 प्रति TB स्क्यान गरिएको डाटा**
- लागत बचाउन **compressed वा columnar** डाटा प्रयोग गर्नुहोस् (कम scan हुन्छ)
- **प्रयोग:** Business intelligence, analytics, reporting; VPC Flow Logs, ELB Logs, CloudTrail trail विश्लेषण

> ⭐ **परीक्षा टिप (Stéphane को सुझाव):** **"S3 मा भएको डाटा serverless SQL ले विश्लेषण गर्ने" → Athena**

```
S3 Bucket → Amazon Athena (Query र विश्लेषण) → Amazon QuickSight (Report र Dashboard)
```

---

## १२. Amazon QuickSight ⭐

**QuickSight = Serverless, machine learning-powered Business Intelligence सेवा** — अन्तरक्रियात्मक dashboard बनाउन।

- छिटो, स्वचालित रूपमा scalable, embed गर्न मिल्ने
- **प्रति-session मूल्य**
- **प्रयोग:** Business analytics, visualization बनाउने, ad-hoc विश्लेषण, व्यावसायिक अन्तर्दृष्टि
- **एकीकृत:** RDS, Aurora, Athena, Redshift, S3

> **सम्झने:** **Dashboard चाहियो → QuickSight**

---

## १३. अन्य डाटाबेस सेवाहरू ⭐

### DocumentDB
- Aurora भनेको PostgreSQL/MySQL को "AWS संस्करण" हो
- **DocumentDB भनेको MongoDB को AWS संस्करण** (MongoDB एक NoSQL database हो)
- MongoDB **JSON डाटा** राख्न, query गर्न र index गर्न प्रयोग हुन्छ
- Aurora जस्तै deployment अवधारणा
- पूर्ण managed, **३ AZ मा replication** सहित highly available
- Storage **१० GB को increment मा आफैं बढ्छ**
- प्रति सेकेन्ड लाखौं request सम्म आफैं scale

### Amazon Neptune ⭐
- **पूर्ण managed graph database**
- लोकप्रिय graph dataset: **सामाजिक सञ्जाल**
  - User को साथी हुन्छन्, Post मा comment हुन्छन्, Comment मा like हुन्छन्…
- **३ AZ** मा highly available, **१५ read replica** सम्म
- अत्यन्त जोडिएका dataset सँग काम गर्ने एप्लिकेसन — जटिल query को लागि अनुकूलित
- **अर्बौं सम्बन्ध** राख्न र millisecond latency मा query गर्न सक्ने
- **उत्तम:** Knowledge graph (Wikipedia), **fraud detection**, recommendation engine, social networking

> **परीक्षा टिप:** प्रश्नमा **"graph"**, **"social network"**, **"relationships"** आयो भने → **Neptune**

### Amazon Timestream
- पूर्ण managed, छिटो, scalable, **serverless time series database**
- क्षमता समायोजन गर्न आफैं scale up/down
- **प्रति दिन खरबौं event** राख्ने र विश्लेषण गर्ने
- Relational database भन्दा **हजारौं गुणा छिटो र १/१० मूल्यमा**
- **Built-in time series analytics function** — near real-time मा ढाँचा पहिचान गर्न

> **परीक्षा टिप:** **"time series" / IoT sensor डाटा → Timestream**

### Amazon Managed Blockchain
- Blockchain ले **विश्वसनीय केन्द्रीय अख्तियारबिना धेरै पक्षले लेनदेन गर्न** दिन्छ
- Managed सेवाले:
  - सार्वजनिक blockchain network मा जोडिने
  - वा आफ्नै scalable निजी network बनाउने
- **Framework: Hyperledger Fabric र Ethereum** सँग मिल्ने

### AWS Glue ⭐
- **Managed ETL (Extract, Transform, Load) सेवा**
- Analytics को लागि डाटा तयार र रूपान्तरण गर्न उपयोगी
- **पूर्ण serverless सेवा**
- **Glue Data Catalog:** dataset को catalog — **Athena, Redshift, EMR** ले प्रयोग गर्न सक्ने

```
S3 Bucket / Amazon RDS → [Extract, Transform] Glue ETL → [Load] Redshift
```

> **परीक्षा टिप:** **"ETL" शब्द आयो → Glue**

### DMS — Database Migration Service ⭐
- **डाटाबेस छिटो र सुरक्षित रूपमा AWS मा migrate गर्ने** — resilient, self-healing
- ⭐ **Migration हुँदा स्रोत डाटाबेस उपलब्धै रहन्छ**
- समर्थन:
  - **Homogeneous migration:** Oracle → Oracle
  - **Heterogeneous migration:** Microsoft SQL Server → Aurora

```
Source DB → EC2 instance (DMS चलिरहेको) → Target DB
```

> **परीक्षा टिप:** **"database migrate गर्ने" → DMS**

---

## 📝 अभ्यास प्रश्नहरू

**प्रश्न १.** एउटा एप्लिकेसनको read traffic धेरै बढ्यो। RDS को पढ्ने प्रदर्शन सुधार्न के गर्ने?

A) Multi-AZ सक्षम गर्ने
B) Read Replica थप्ने
C) Multi-Region deployment गर्ने
D) Instance size घटाउने

<details><summary>उत्तर</summary>

**B) Read Replica** — पढ्ने workload scale गर्न। Multi-AZ availability को लागि हो, प्रदर्शनको लागि होइन।
</details>

---

**प्रश्न २.** S3 मा राखिएको CSV फाइललाई SQL ले विश्लेषण गर्नुपर्‍यो, तर कुनै server व्यवस्थापन गर्न चाहँदैनन्। कुन सेवा?

A) Amazon Redshift
B) Amazon RDS
C) Amazon Athena
D) Amazon EMR

<details><summary>उत्तर</summary>

**C) Amazon Athena** — S3 मा serverless SQL query। यो परीक्षाको classic प्रश्न हो।
</details>

---

**प्रश्न ३.** DAX र ElastiCache बीचको मुख्य फरक के हो?

A) DAX सस्तो छ
B) DAX केवल DynamoDB को लागि हो, ElastiCache अन्य DB सँग पनि चल्छ
C) ElastiCache मात्र in-memory हो
D) DAX serverless हो

<details><summary>उत्तर</summary>

**B)** — DAX केवल DynamoDB को cache हो। ElastiCache (Redis/Memcached) अन्य डाटाबेससँग पनि प्रयोग हुन्छ।
</details>

---

**प्रश्न ४.** सामाजिक सञ्जालको लागि प्रयोगकर्ता, साथी र post बीचको सम्बन्ध राख्न कुन डाटाबेस उपयुक्त?

A) Amazon DynamoDB
B) Amazon Redshift
C) Amazon Neptune
D) Amazon Timestream

<details><summary>उत्तर</summary>

**C) Amazon Neptune** — graph database, अत्यन्त जोडिएको dataset को लागि।
</details>

---

**प्रश्न ५.** एउटा कम्पनीले Oracle डाटाबेस AWS को Aurora मा सार्न चाहन्छ, तर migration हुँदा पनि पुरानो डाटाबेस चलिरहनुपर्छ। कुन सेवा?

A) AWS Glue
B) AWS DMS (Database Migration Service)
C) AWS DataSync
D) AWS Snowball

<details><summary>उत्तर</summary>

**B) AWS DMS** — heterogeneous migration समर्थन गर्छ, र स्रोत डाटाबेस migration को क्रममा उपलब्धै रहन्छ।
</details>

---

**प्रश्न ६.** व्यावसायिक रिपोर्ट र analytics को लागि data warehouse चाहियो। कुन सेवा?

A) Amazon RDS
B) Amazon DynamoDB
C) Amazon Redshift
D) Amazon ElastiCache

<details><summary>उत्तर</summary>

**C) Amazon Redshift** — OLAP / data warehousing को लागि।
</details>

---

**प्रश्न ७.** RDS बारे कुन कथन सही हो?

A) RDS instance मा SSH गरेर OS सम्म पहुँच गर्न मिल्छ
B) OS patching तपाईंले गर्नुपर्छ
C) RDS ले Point in Time Restore समर्थन गर्छ
D) RDS ले NoSQL डाटाबेस मात्र चलाउँछ

<details><summary>उत्तर</summary>

**C)** — RDS ले निरन्तर backup र Point in Time Restore दिन्छ। SSH गर्न मिल्दैन, OS patching AWS ले गर्छ, र RDS relational (SQL) को लागि हो।
</details>

---

**प्रश्न ८.** IoT sensor बाट आउने time-stamped डाटा प्रति दिन खरबौं event राख्नुपर्‍यो। कुन डाटाबेस?

A) Amazon Neptune
B) Amazon Timestream
C) Amazon DocumentDB
D) Amazon Redshift

<details><summary>उत्तर</summary>

**B) Amazon Timestream** — time series database, IoT र sensor डाटाको लागि बनेको।
</details>

---

## ✅ यो मोड्युलको सार — छिटो सन्दर्भ तालिका ⭐

| आवश्यकता | सेवा |
|---|---|
| Relational Database (OLTP), SQL | **RDS र Aurora** |
| पढ्ने प्रदर्शन बढाउने | **Read Replica** |
| AZ failover / High Availability | **Multi-AZ** |
| Region-level Disaster Recovery | **Multi-Region** |
| In-memory Database / Cache | **ElastiCache** (Redis, Memcached) |
| Key/Value NoSQL, serverless | **DynamoDB** |
| DynamoDB को cache | **DAX** |
| Data Warehouse (OLAP), SQL | **Redshift** |
| Hadoop / Spark cluster | **EMR** |
| S3 मा serverless SQL query | **Athena** |
| Dashboard र visualization (serverless) | **QuickSight** |
| MongoDB को AWS संस्करण (JSON, NoSQL) | **DocumentDB** |
| Hyperledger Fabric, Ethereum | **Managed Blockchain** |
| ETL र Data Catalog | **Glue** |
| Database migration | **DMS** |
| Graph database | **Neptune** |
| Time-series database | **Timestream** |

---

⬅️ [मोड्युल ०६](06-s3.md) | ➡️ [मोड्युल ०८ — अन्य Compute](08-other-compute.md)
