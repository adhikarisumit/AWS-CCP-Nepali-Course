# मोड्युल १४ — Security र Compliance

> ⭐⭐ **परीक्षाको सबैभन्दा भारी मोड्युलमध्ये एक। CLF-C02 मा Security र Compliance ३०% भार बोक्छ।**

---

## १. AWS Shared Responsibility Model ⭐⭐⭐

### AWS को जिम्मेवारी — **Security OF the Cloud**
- सबै AWS सेवा चलाउने **infrastructure** (hardware, software, भवन र networking) को सुरक्षा
- **Managed सेवा** जस्तै S3, DynamoDB, RDS आदि

### ग्राहकको जिम्मेवारी — **Security IN the Cloud**
- **EC2 instance** को लागि: guest OS को व्यवस्थापन (सुरक्षा patch र update सहित), firewall र network configuration, IAM
- **एप्लिकेसन डाटा encrypt** गर्ने

### Shared Controls (साझा नियन्त्रण) ⭐
- **Patch Management**
- **Configuration Management**
- **Awareness र Training**

---

### उदाहरण — RDS को लागि ⭐

| **AWS को जिम्मा** | **तपाईंको जिम्मा** |
|---|---|
| तल्लो EC2 instance व्यवस्थापन, SSH पहुँच बन्द गर्ने | DB को SG मा port / IP / inbound नियम जाँच्ने |
| **स्वचालित DB patching** | Database भित्र user बनाउने र अनुमति दिने |
| **स्वचालित OS patching** | Public पहुँच सहित/बिना database बनाउने |
| तल्लो instance र disk को audit र काम गर्ने ग्यारेन्टी | Parameter group वा DB लाई SSL कनेक्सन मात्र दिने configure गर्ने |
| | Database को **encryption setting** |

### उदाहरण — S3 को लागि ⭐

| **AWS को जिम्मा** | **तपाईंको जिम्मा** |
|---|---|
| असीमित storage दिने ग्यारेन्टी | **Bucket configuration** |
| Encryption उपलब्ध गराउने ग्यारेन्टी | **Bucket policy / public setting** |
| फरक ग्राहकको डाटा अलग राख्ने | **IAM user र role** |
| AWS कर्मचारीले तपाईंको डाटा पहुँच गर्न नसक्ने सुनिश्चित गर्ने | **Encryption सक्षम गर्ने** |

---

## २. DDoS Attack के हो? ⭐

**DDoS = Distributed Denial-of-Service**

```
attacker → masters → bots (हजारौं) ────> application server
                                          ↑
                     normal users → ❌ पहुँच नहुने, प्रतिक्रिया नआउने
```

आक्रमणकारीले हजारौं मेसिनबाट एकैचोटि request पठाएर server लाई ठप्प पार्छ।

### AWS मा DDoS सुरक्षा ⭐⭐

| सेवा | विवरण |
|---|---|
| **AWS Shield Standard** | वेबसाइट र एपलाई DDoS बाट सुरक्षा — **सबै ग्राहकलाई अतिरिक्त शुल्कबिना** |
| **AWS Shield Advanced** | **२४/७ प्रिमियम DDoS सुरक्षा** |
| **AWS WAF** | नियम अनुसार निश्चित request **filter** गर्ने |
| **CloudFront र Route 53** | Global edge network मार्फत उपलब्धता सुरक्षा। AWS Shield सँग मिलेर **edge मै आक्रमण रोक्ने** |
| **AWS Auto Scaling** | scale गर्न तयार रहने |

---

## ३. AWS Shield ⭐

### AWS Shield Standard
- ✅ **निःशुल्क सेवा — हरेक AWS ग्राहकको लागि सक्रिय**
- **SYN/UDP Flood, Reflection attack** र अन्य **layer 3/layer 4** आक्रमणबाट सुरक्षा

### AWS Shield Advanced ⭐
- **वैकल्पिक DDoS mitigation सेवा — प्रति संस्था प्रति महिना $3,000**
- यी सेवामा जटिल आक्रमणबाट सुरक्षा: **Amazon EC2, ELB, CloudFront, AWS Global Accelerator, Route 53**
- ⭐ **AWS DDoS response team (DRP) मा २४/७ पहुँच**
- ⭐ **DDoS को कारण प्रयोग बढेर आउने उच्च शुल्कबाट सुरक्षा**

---

## ४. AWS WAF — Web Application Firewall ⭐⭐

- वेब एप्लिकेसनलाई सामान्य **web exploit (Layer 7)** बाट जोगाउने
- **Layer 7 = HTTP** (Layer 4 = TCP)
- ⭐ **कहाँ deploy गर्न मिल्ने:** **Application Load Balancer, API Gateway, CloudFront**

### Web ACL (Web Access Control List) परिभाषित गर्ने
- नियममा राख्न सकिने: **IP address, HTTP header, HTTP body, वा URI string**
- ⭐ सामान्य आक्रमणबाट सुरक्षा: **SQL injection र Cross-Site Scripting (XSS)**
- Size constraints, **geo-match (देश ब्लक गर्ने)**
- **Rate-based rules** (घटना गन्ने) — **DDoS सुरक्षाको लागि**

> **परीक्षा टिप:** "SQL injection / XSS बाट जोगाउने" → **AWS WAF**

---

## ५. AWS Network Firewall

- **पूरै Amazon VPC लाई सुरक्षा**
- **Layer 3 देखि Layer 7 सम्मको सुरक्षा**
- जुनसुकै दिशामा inspect गर्न सकिने:
  - VPC देखि VPC को traffic
  - इन्टरनेटतर्फ जाने (Outbound)
  - इन्टरनेटबाट आउने (Inbound)
  - Direct Connect र Site-to-Site VPN तर्फ/बाट

---

## ६. AWS Firewall Manager ⭐

- **AWS Organization का सबै account मा security rule व्यवस्थापन गर्ने**
- **Security policy** = साझा security नियमको सेट:
  - EC2, ALB आदिको **VPC Security Group**
  - **WAF rules**
  - **AWS Shield Advanced**
  - **AWS Network Firewall**
- ⭐ **नयाँ संसाधन बन्नासाथ नियम लागू हुन्छ** (compliance को लागि राम्रो) — Organization का सबै र भविष्यका account मा

> **सम्झने:** **Firewall Manager = धेरै account मा security rule व्यवस्थापन**

---

## ७. AWS मा Penetration Testing ⭐

AWS ग्राहकहरूले **अग्रिम अनुमतिबिना** यी ८ सेवाविरुद्ध security assessment वा penetration test गर्न सक्छन्:

1. Amazon EC2 instance, NAT Gateway, र Elastic Load Balancer
2. Amazon RDS
3. Amazon CloudFront
4. Amazon Aurora
5. Amazon API Gateway
6. AWS Lambda र Lambda Edge function
7. Amazon Lightsail संसाधन
8. Amazon Elastic Beanstalk environment

*(सूची समयसँगै बढ्न सक्छ — परीक्षामा ठ्याक्कै सूची सोधिँदैन)*

### ⛔ निषेधित गतिविधि ⭐

- **Route 53 Hosted Zone मार्फत DNS zone walking**
- **DoS, DDoS, Simulated DoS, Simulated DDoS**
- **Port flooding**
- **Protocol flooding**
- **Request flooding** (login request flooding, API request flooding)

अन्य simulated event को लागि सम्पर्क: `aws-security-simulated-event@amazon.com`

> **परीक्षा टिप:** **"DDoS simulate गर्न पाइन्छ?" → पाइँदैन!**

---

## ८. Data at rest vs Data in transit ⭐

| प्रकार | अर्थ | उदाहरण |
|---|---|---|
| **At rest** | यन्त्रमा **राखिएको वा archive गरिएको** डाटा | Hard disk मा, RDS instance मा, S3 Glacier Deep Archive मा |
| **In transit (in motion)** | एक ठाउँबाट अर्को ठाउँ **सर्दै गरेको** डाटा | On-premises बाट AWS मा transfer, EC2 बाट DynamoDB मा |

👉 **दुवै अवस्थामा डाटा encrypt गर्नुपर्छ!** यसको लागि **encryption key** प्रयोग गरिन्छ।

---

## ९. AWS KMS — Key Management Service ⭐⭐

> ⭐ **AWS सेवामा "encryption" भन्ने शब्द सुन्नुभयो भने प्रायः KMS नै हो।**

**KMS = AWS ले हाम्रो लागि encryption key व्यवस्थापन गर्ने।**

### Encryption Opt-in (तपाईंले सक्षम गर्नुपर्ने)
- **EBS volumes** — volume encrypt गर्ने
- **S3 buckets** — object को server-side encryption (SSE-S3 पूर्वनिर्धारित रूपमा सक्षम, SSE-KMS opt in)
- **Redshift database** — डाटा encryption
- **RDS database** — डाटा encryption
- **EFS drives** — डाटा encryption

### Encryption स्वचालित रूपमा सक्षम ⭐
- **CloudTrail Logs**
- **S3 Glacier**
- **Storage Gateway**

---

## १०. CloudHSM ⭐

| | **KMS** | **CloudHSM** |
|---|---|---|
| AWS ले के manage गर्छ | **Encryption को software** | **Encryption को hardware** |
| Key कसले manage गर्छ | **AWS ले** | ⭐ **तपाईंले पूर्ण रूपमा (AWS ले होइन)** |
| Hardware | — | **HSM = Hardware Security Module**, tamper-resistant |
| Compliance | — | **FIPS 140-2 Level 3** compliance |

```
AWS ले Hardware manage गर्छ ←→ [SSL Connection] ←→ प्रयोगकर्ताले Key manage गर्छ
```

> **परीक्षा टिप:** "encryption key मै पूर्ण रूपमा नियन्त्रण गर्न चाहन्छु / dedicated hardware चाहियो" → **CloudHSM**

---

## ११. KMS Key का प्रकार ⭐

| प्रकार | विवरण |
|---|---|
| **Customer Managed Key** | ग्राहकले बनाउने, व्यवस्थापन गर्ने र प्रयोग गर्ने। सक्षम/असक्षम गर्न मिल्ने। **Rotation policy** (हरेक वर्ष नयाँ key, पुरानो सुरक्षित) र **bring-your-own-key** सम्भव |
| **AWS Managed Key** | AWS ले ग्राहकको तर्फबाट बनाउने र व्यवस्थापन गर्ने। AWS सेवाले प्रयोग गर्ने (`aws/s3`, `aws/ebs`, `aws/redshift`) |
| **AWS Owned Key** | AWS सेवाले धेरै account मा प्रयोग गर्न आफैं बनाएको र व्यवस्थापन गरेको CMK को सङ्ग्रह। AWS ले तपाईंको account मा संसाधन जोगाउन प्रयोग गर्न सक्ने (तर तपाईंले key हेर्न पाउनुहुन्न) |
| **CloudHSM Keys (custom keystore)** | आफ्नै CloudHSM hardware बाट बनेको key। Cryptographic operation CloudHSM cluster भित्रै हुन्छ |

---

## १२. AWS Certificate Manager (ACM) ⭐

- **SSL/TLS Certificate** सजिलै provision, व्यवस्थापन र deploy गर्ने
- वेबसाइटको लागि **in-flight encryption (HTTPS)** दिन प्रयोग
- **Public र Private TLS certificate** दुवै समर्थन
- ⭐ **Public TLS certificate निःशुल्क!**
- ⭐ **स्वचालित TLS certificate नवीकरण**
- एकीकरण (TLS certificate load गर्न मिल्ने ठाउँ):
  - **Elastic Load Balancer**
  - **CloudFront Distribution**
  - **API Gateway का API**

```
User ──HTTPS──> Application Load Balancer ──HTTP──> EC2 (ASG मा)
                        ↑
              AWS Certificate Manager (TLS cert provision र मर्मत)
```

---

## १३. AWS Secrets Manager ⭐

- **Secret राख्नको लागि बनेको नयाँ सेवा**
- ⭐ **हरेक X दिनमा secret rotate गर्न बाध्य पार्ने क्षमता**
- Rotation मा **secret स्वचालित रूपमा बनाउने** (Lambda प्रयोग गर्छ)
- ⭐ **Amazon RDS (MySQL, PostgreSQL, Aurora) सँग एकीकरण**
- Secret **KMS** ले encrypt गर्छ
- ⭐ **मुख्यतया RDS एकीकरणको लागि बनेको**

> ⭐ **Secrets Manager vs SSM Parameter Store:**
> - **Secrets Manager** = स्वचालित rotation चाहिने secret (विशेषगरी RDS password)
> - **Parameter Store** = साधारण configuration र secret, सस्तो

---

## १४. AWS Artifact ⭐

**(वास्तवमा यो सेवा होइन, portal हो)**

- ग्राहकलाई **AWS compliance documentation र AWS सम्झौताहरूमा on-demand पहुँच** दिने portal

| भाग | के दिन्छ |
|---|---|
| **Artifact Reports** | तेस्रो पक्ष auditor बाट AWS का security र compliance कागजात डाउनलोड — **AWS ISO certification, PCI (Payment Card Industry), SOC report** |
| **Artifact Agreements** | AWS सम्झौता समीक्षा, स्वीकार र स्थिति ट्र्याक — **BAA (Business Associate Addendum), HIPAA** आदि |

- **आन्तरिक audit वा compliance** लाई सहयोग गर्न प्रयोग गर्न सकिने

> **परीक्षा टिप:** **"compliance report / ISO / PCI / SOC चाहियो" → AWS Artifact**

---

## १५. Amazon GuardDuty ⭐⭐

**GuardDuty = तपाईंको AWS Account जोगाउने बुद्धिमान् threat discovery**

- **Machine Learning एल्गोरिदम, anomaly detection, तेस्रो पक्षको डाटा** प्रयोग गर्छ
- ⭐ **एक क्लिकमा सक्षम (३० दिनको trial), कुनै software install गर्नुपर्दैन**

### Input डाटा ⭐

| स्रोत | के हेर्छ |
|---|---|
| **CloudTrail Events Logs** | असामान्य API call, अनधिकृत deployment |
| **CloudTrail Management Events** | VPC subnet बनाउने, trail बनाउने… |
| **CloudTrail S3 Data Events** | get object, list objects, delete object… |
| **VPC Flow Logs** | असामान्य आन्तरिक traffic, असामान्य IP address |
| **DNS Logs** | DNS query भित्र encoded डाटा पठाइरहेका compromised EC2 instance |
| **वैकल्पिक सुविधा** | EKS Audit Logs, RDS र Aurora, EBS, Lambda, S3 Data Events… |

- Finding आउँदा सूचित हुन **EventBridge rule** सेट गर्न सकिने
- EventBridge rule ले **AWS Lambda वा SNS** लाई target गर्न सक्छ
- ⭐ **CryptoCurrency आक्रमणबाट जोगाउन सक्ने** (यसको छुट्टै "finding" छ)

> **परीक्षा टिप:** "ML ले खराब गतिविधि पत्ता लगाउने" → **GuardDuty**

---

## १६. Amazon Inspector ⭐⭐

**Inspector = स्वचालित Security Assessment**

⚠️ **केवल ३ कुराको लागि** (यो याद गर्नुहोस्):

| के को लागि | कसरी |
|---|---|
| **EC2 instances** | **AWS Systems Manager (SSM) agent** प्रयोग गरेर। अनपेक्षित network पहुँच विश्लेषण, चलिरहेको OS मा ज्ञात vulnerability विश्लेषण |
| **Amazon ECR मा push गरिएका Container Image** | Container Image push हुँदै assessment |
| **Lambda Functions** | Function code र package dependency मा software vulnerability पहिचान। Deploy हुँदै assessment |

- **AWS Security Hub** सँग reporting र एकीकरण
- Finding **Amazon EventBridge** मा पठाउने

### Inspector ले के मूल्याङ्कन गर्छ? ⭐
- **सम्झनुहोस्: केवल EC2 instance, Container Image र Lambda function**
- Infrastructure को **निरन्तर scanning** — आवश्यक हुँदा मात्र
- **Package vulnerabilities** (EC2, ECR र Lambda) — **CVE डाटाबेस**
- **Network reachability** (EC2)
- सबै vulnerability सँग **risk score** जोडिन्छ — प्राथमिकता तोक्न

---

## १७. AWS Config ⭐⭐

**Config = AWS संसाधनको audit र compliance रेकर्ड गर्ने**

- **समयसँगै configuration र परिवर्तन रेकर्ड** गर्न सहयोग
- Configuration डाटा **S3 मा राख्न सकिने** (Athena ले विश्लेषण गर्न सक्ने)

### Config ले समाधान गर्ने प्रश्नहरू ⭐
- मेरा security group मा **अनियन्त्रित SSH पहुँच** छ कि?
- मेरा bucket मा **public access** छ कि?
- मेरो **ALB को configuration समयसँगै कसरी बदलियो?**

- कुनै पनि परिवर्तनको लागि **alert (SNS notification)** पाउन सकिने
- ⭐ **AWS Config per-region सेवा हो**
- **Region र account मा aggregate गर्न सकिने**

### Config Resource मा के हेर्न सकिन्छ
- संसाधनको **compliance समयसँगै**
- संसाधनको **configuration समयसँगै**
- **CloudTrail API call** (सक्षम भए)

---

## १८. Amazon Macie ⭐

- **पूर्ण managed डाटा सुरक्षा र गोपनीयता सेवा**
- **Machine Learning र pattern matching** प्रयोग गरेर AWS मा भएको **संवेदनशील डाटा पत्ता लगाउने र जोगाउने**
- ⭐ **PII (Personally Identifiable Information)** जस्ता संवेदनशील डाटा पहिचान गरेर alert दिने

```
S3 Buckets → [analyze] Macie (Discover Sensitive Data - PII) → [notify] Amazon EventBridge
```

> **परीक्षा टिप:** **"S3 मा PII / संवेदनशील डाटा खोज्ने" → Macie**

---

## १९. AWS Security Hub ⭐

- **धेरै AWS account मा security व्यवस्थापन गर्ने र security जाँच स्वचालित गर्ने केन्द्रीय security उपकरण**
- वर्तमान security र compliance अवस्था देखाउने **एकीकृत dashboard** — तुरुन्तै कदम चाल्न
- विभिन्न AWS सेवा र AWS partner उपकरणबाट **alert स्वचालित रूपमा जम्मा** गर्ने:
  - **Config, GuardDuty, Inspector, Macie, IAM Access Analyzer, AWS Systems Manager, AWS Firewall Manager, AWS Health, AWS Partner Network Solutions**
- ⚠️ ⭐ **पहिले AWS Config Service सक्षम गर्नैपर्छ!** (यो परीक्षामा सोधिन्छ)

---

## २०. Amazon Detective ⭐

- GuardDuty, Macie र Security Hub ले सम्भावित security समस्या वा **finding पहिचान** गर्छन्
- कहिलेकाहीँ finding को **मूल कारण पत्ता लगाउन गहिरो विश्लेषण** चाहिन्छ — यो जटिल प्रक्रिया हो
- ⭐ **Amazon Detective ले ML र graph प्रयोग गरेर security समस्या वा शङ्कास्पद गतिविधिको मूल कारण छिटो पहिचान गर्छ**
- **VPC Flow Logs, CloudTrail, GuardDuty** बाट event स्वचालित रूपमा सङ्कलन गरी **एकीकृत दृश्य** बनाउँछ
- मूल कारणसम्म पुग्न विस्तृत र सन्दर्भसहित **visualization** बनाउँछ

> **सम्झने:** **GuardDuty ले भेट्छ → Detective ले किन भयो पत्ता लगाउँछ**

---

## २१. AWS Abuse ⭐

AWS संसाधन दुरुपयोग वा गैरकानुनी काममा प्रयोग भएको शङ्का भए रिपोर्ट गर्ने।

**दुरुपयोग र निषेधित व्यवहार:**
- **Spam** — AWS-स्वामित्वको IP बाट अनावश्यक इमेल, AWS संसाधनले वेबसाइट र forum मा spam
- **Port scanning** — असुरक्षित port पत्ता लगाउन packet पठाउने
- **DoS वा DDoS आक्रमण** — AWS-स्वामित्वको IP ले तपाईंको server/software ठप्प पार्न खोज्ने
- **Intrusion attempts** — तपाईंका संसाधनमा लगइन गर्न खोज्ने
- **आपत्तिजनक वा copyright सामग्री होस्ट गर्ने**
- **Malware वितरण**

**सम्पर्क:** AWS abuse form, वा `abuse@amazonaws.com`

---

## २२. Root User Privileges ⭐⭐

**Root user = Account Owner** (account बन्दा बन्ने)। सबै AWS सेवा र संसाधनमा पूर्ण पहुँच।

⚠️ **AWS account root user को access key ताल्चा लगाएर राख्नुहोस्!**
⚠️ **दैनिक काम — प्रशासनिक काम पनि — root account बाट नगर्नुहोस्।**

### केवल Root User ले मात्र गर्न सक्ने काम ⭐⭐

**यो सूची परीक्षामा सोधिन्छ:**

1. **Account setting बदल्ने** (account नाम, इमेल ठेगाना, root user password, root user access key)
2. केही **tax invoice** हेर्ने
3. **AWS account बन्द गर्ने**
4. **IAM user को अनुमति पुनर्स्थापना गर्ने**
5. **AWS Support plan बदल्ने वा रद्द गर्ने**
6. **Reserved Instance Marketplace मा बिक्रेताको रूपमा दर्ता हुने**
7. **S3 bucket मा MFA सक्षम गर्न configure गर्ने**
8. अवैध VPC ID वा VPC endpoint ID भएको **S3 bucket policy सम्पादन वा मेट्ने**
9. **GovCloud को लागि दर्ता हुने**

---

## २३. IAM Access Analyzer ⭐

- **कुन संसाधन बाहिरी पक्षसँग साझा भएका छन् पत्ता लगाउने:**
  - S3 Buckets
  - IAM Roles
  - KMS Keys
  - Lambda Functions र Layers
  - SQS queues
  - Secrets Manager Secrets
- **Zone of Trust परिभाषित गर्ने** = AWS Account वा AWS Organization
- ⭐ **Zone of trust बाहिरको पहुँच ⇒ findings**

---

## 📝 अभ्यास प्रश्नहरू

**प्रश्न १.** एउटा वेब एप्लिकेसनलाई SQL injection र XSS आक्रमणबाट जोगाउनुपर्‍यो। कुन सेवा?

A) AWS Shield
B) AWS WAF
C) Amazon GuardDuty
D) AWS Network Firewall

<details><summary>उत्तर</summary>

**B) AWS WAF** — Layer 7 web exploit (SQL injection, XSS) बाट सुरक्षा।
</details>

---

**प्रश्न २.** S3 bucket मा ग्राहकको व्यक्तिगत जानकारी (PII) छ कि छैन पत्ता लगाउन कुन सेवा?

A) Amazon GuardDuty
B) Amazon Inspector
C) Amazon Macie
D) AWS Config

<details><summary>उत्तर</summary>

**C) Amazon Macie** — ML र pattern matching ले S3 मा संवेदनशील डाटा (PII) पत्ता लगाउँछ।
</details>

---

**प्रश्न ३.** Amazon Inspector ले कुन-कुन संसाधन scan गर्छ? (तीन छान्नुहोस्)

A) EC2 instances
B) S3 buckets
C) ECR मा भएका Container Image
D) Lambda functions
E) RDS databases

<details><summary>उत्तर</summary>

**A), C), D)** — Inspector केवल EC2, ECR Container Image र Lambda function को लागि हो।
</details>

---

**प्रश्न ४.** Security Hub प्रयोग गर्नुअघि कुन सेवा सक्षम गर्नैपर्छ?

A) AWS CloudTrail
B) AWS Config
C) Amazon GuardDuty
D) Amazon Inspector

<details><summary>उत्तर</summary>

**B) AWS Config** — Security Hub प्रयोग गर्न पहिले Config सक्षम गर्नुपर्छ।
</details>

---

**प्रश्न ५.** एउटा कम्पनीलाई encryption key मै पूर्ण नियन्त्रण चाहियो र FIPS 140-2 Level 3 compliance चाहियो। कुन सेवा?

A) AWS KMS
B) AWS CloudHSM
C) AWS Secrets Manager
D) SSM Parameter Store

<details><summary>उत्तर</summary>

**B) AWS CloudHSM** — dedicated hardware, key तपाईंले नै manage गर्ने, FIPS 140-2 Level 3।
</details>

---

**प्रश्न ६.** Auditor ले AWS को PCI र SOC compliance report मागे। कहाँबाट पाइन्छ?

A) AWS Config
B) AWS Artifact
C) AWS Security Hub
D) AWS Trusted Advisor

<details><summary>उत्तर</summary>

**B) AWS Artifact** — compliance documentation र सम्झौताको on-demand portal।
</details>

---

**प्रश्न ७.** निम्नमध्ये कुन काम केवल Root user ले मात्र गर्न सक्छ? (दुई छान्नुहोस्)

A) EC2 instance launch गर्ने
B) AWS account बन्द गर्ने
C) S3 bucket बनाउने
D) AWS Support plan बदल्ने
E) IAM user बनाउने

<details><summary>उत्तर</summary>

**B)** र **D)** — Account बन्द गर्ने र Support plan बदल्ने root-only काम हुन्।
</details>

---

**प्रश्न ८.** AWS मा DDoS test (simulated DDoS) गर्न पाइन्छ?

A) पाइन्छ, EC2 मा मात्र
B) पाइन्छ, अनुमति लिएर
C) पाइँदैन — निषेधित छ
D) पाइन्छ, Shield Advanced भए

<details><summary>उत्तर</summary>

**C) पाइँदैन** — DoS, DDoS, simulated DoS/DDoS, port/protocol/request flooding सबै निषेधित छन्।
</details>

---

**प्रश्न ९.** Security group मा unrestricted SSH access छ कि छैन र समयसँगै configuration कसरी बदलियो हेर्न कुन सेवा?

A) AWS CloudTrail
B) AWS Config
C) Amazon Inspector
D) Amazon Detective

<details><summary>उत्तर</summary>

**B) AWS Config** — configuration र compliance को रेकर्ड समयसँगै।
</details>

---

**प्रश्न १०.** GuardDuty ले खतरा पत्ता लगायो। त्यसको मूल कारण गहिरो विश्लेषण गर्न कुन सेवा?

A) AWS Config
B) Amazon Detective
C) AWS Security Hub
D) Amazon Inspector

<details><summary>उत्तर</summary>

**B) Amazon Detective** — ML र graph प्रयोग गरेर मूल कारण पत्ता लगाउँछ।
</details>

---

## ✅ यो मोड्युलको सार ⭐⭐

| सेवा | एक-लाइन काम |
|---|---|
| **Shared Responsibility** | AWS = security OF cloud, तपाईं = security IN cloud |
| **Shield** | स्वचालित DDoS सुरक्षा (Standard निःशुल्क) + Advanced मा २४/७ सहयोग |
| **WAF** | नियम अनुसार आउने request filter गर्ने firewall (Layer 7, SQL injection, XSS) |
| **KMS** | AWS ले व्यवस्थापन गर्ने encryption key |
| **CloudHSM** | Hardware encryption — **key तपाईंले manage गर्ने** |
| **Certificate Manager (ACM)** | SSL/TLS Certificate provision, व्यवस्थापन र deploy — **public निःशुल्क** |
| **Artifact** | **PCI, ISO, SOC** जस्ता compliance report पाउने |
| **GuardDuty** | VPC, DNS र CloudTrail Logs बाट **खराब व्यवहार** पत्ता लगाउने |
| **Inspector** | **EC2, ECR Image र Lambda** मा software vulnerability खोज्ने |
| **Network Firewall** | VPC लाई network आक्रमणबाट जोगाउने |
| **Config** | Configuration परिवर्तन र नियम विरुद्धको compliance ट्र्याक गर्ने |
| **Macie** | **S3 मा संवेदनशील डाटा (PII)** खोज्ने |
| **CloudTrail** | Account भित्र user ले गरेका **API call** ट्र्याक गर्ने |
| **Security Hub** | धेरै AWS account का security finding जम्मा गर्ने (**Config चाहिन्छ**) |
| **Detective** | Security समस्याको **मूल कारण** पत्ता लगाउने |
| **AWS Abuse** | दुरुपयोग वा गैरकानुनी प्रयोग रिपोर्ट गर्ने |
| **IAM Access Analyzer** | कुन संसाधन **बाहिरी पक्षसँग साझा** भएको छ पहिचान गर्ने |
| **Firewall Manager** | Organization भर security rule व्यवस्थापन (WAF, Shield…) |
| **Secrets Manager** | Secret राख्ने र **स्वचालित rotate** गर्ने (मुख्यतया RDS) |

---

⬅️ [मोड्युल १३](13-vpc.md) | ➡️ [मोड्युल १५ — Machine Learning](15-machine-learning.md)
