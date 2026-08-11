# मोड्युल ०२ — IAM (Identity and Access Management)

> **यस मोड्युलमा:** User, Group, Policy, Role, MFA, Access Key, CLI/SDK, र IAM का सुरक्षा उपकरण।

**IAM = Identity and Access Management** — यो **Global सेवा** हो (region छैन)।

---

## १. User र Group

- AWS account खोल्दा **Root account** आफैं बन्छ। ⚠️ **यो कहिल्यै दैनिक काममा प्रयोग नगर्नुहोस्, कसैसँग नबाँड्नुहोस्।**
- **User** = तपाईंको संस्थाका मानिस। एक व्यक्ति = एक user।
- **Group** = user हरूको समूह।

### Group का नियम ⭐
- Group मा **user मात्र** राख्न सकिन्छ — **अर्को group राख्न मिल्दैन** (nested group हुँदैन)
- User कुनै पनि group मा नहुन पनि सक्छ
- एउटा user **धेरै group** मा हुन सक्छ

```
Group: Developers → Alice, Bob, Charles
Group: Operations → Charles, David      ← Charles दुई group मा छ
Group: Audit Team → Charles, David, Edward
Fred → कुनै group मा छैन (inline policy मात्र)
```

---

## २. Permissions र Policy

User वा Group लाई **JSON document** (policy) दिइन्छ। Policy ले के गर्न पाइन्छ भन्ने तोक्छ।

> **Least Privilege Principle** ⭐ — प्रयोगकर्तालाई **आवश्यक भन्दा बढी अनुमति नदिने**। यो AWS को मूल सुरक्षा सिद्धान्त हो।

### Policy को उदाहरण

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:Describe*",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:ListMetrics",
                "cloudwatch:GetMetricStatistics"
            ],
            "Resource": "*"
        }
    ]
}
```

### Policy को संरचना ⭐

| भाग | अनिवार्य? | अर्थ |
|---|---|---|
| **Version** | हो | Policy भाषाको संस्करण — सधैं `"2012-10-17"` |
| **Id** | होइन | Policy को पहिचायक |
| **Statement** | **हो** | एक वा बढी कथन |

Statement भित्र:

| भाग | अर्थ |
|---|---|
| **Sid** | Statement को पहिचायक (वैकल्पिक) |
| **Effect** | `Allow` वा `Deny` |
| **Principal** | यो policy कुन account/user/role मा लागू हुने |
| **Action** | कुन-कुन काम अनुमति/निषेध गर्ने |
| **Resource** | कुन संसाधनमा लागू हुने |
| **Condition** | कहिले लागू हुने (वैकल्पिक) |

> **परीक्षा टिप:** `Version` सधैं `2012-10-17` हुन्छ — यो मिति नबदलिने स्थिर मान हो।

---

## ३. Password Policy

बलियो पासवर्ड = बढी सुरक्षा। AWS मा password policy सेट गर्न सकिन्छ:

- न्यूनतम **लम्बाइ** तोक्ने
- विशेष **अक्षर प्रकार** अनिवार्य गर्ने — ठूला अक्षर, साना अक्षर, अङ्क, विशेष चिह्न
- IAM user लाई आफ्नै पासवर्ड बदल्न दिने
- केही समयपछि पासवर्ड बदल्न बाध्य पार्ने (**password expiration**)
- **पुरानै पासवर्ड फेरि प्रयोग गर्न रोक्ने**

---

## ४. MFA — Multi Factor Authentication ⭐

**MFA = तपाईंलाई थाहा भएको पासवर्ड + तपाईंसँग भएको सुरक्षा यन्त्र**

> **मुख्य फाइदा:** पासवर्ड चोरी भयो वा ह्याक भयो भने पनि **account सुरक्षित रहन्छ**।

⚠️ **Root Account र IAM user दुवैमा MFA अनिवार्य लगाउनुहोस्।**

### MFA का यन्त्रहरू

| यन्त्र | विवरण |
|---|---|
| **Virtual MFA device** | Google Authenticator (फोन मात्र), Authy (फोन मात्र) — एउटै यन्त्रमा धेरै token राख्न मिल्ने |
| **U2F Security Key** | YubiKey (Yubico — तेस्रो पक्ष) — एउटै key ले धेरै root र IAM user सम्हाल्न सक्ने |
| **Hardware Key Fob** | Gemalto (तेस्रो पक्ष) |
| **Hardware Key Fob (GovCloud)** | SurePassID (तेस्रो पक्ष) — AWS GovCloud (US) को लागि |

---

## ५. AWS मा पहुँचका ३ तरिका ⭐

| तरिका | कसरी सुरक्षित |
|---|---|
| **AWS Management Console** | पासवर्ड + MFA |
| **AWS CLI** (Command Line Interface) | **Access Keys** |
| **AWS SDK** (कोडको लागि) | **Access Keys** |

### Access Keys बारे

- AWS Console बाट बन्छ
- User ले आफ्नै key व्यवस्थापन गर्छ
- **पासवर्ड जत्तिकै गोप्य — कसैसँग नबाँड्नुहोस्**
- **Access Key ID ≈ username**
- **Secret Access Key ≈ password**

उदाहरण (नक्कली):
```
Access key ID:      AKIASK4E37PV4983d6C
Secret Access Key:  AZPN3zojWozWCndIjhB0Unh8239a1bzbzO5fqqkZq
```

### AWS CLI के हो?
- Command line बाट AWS सेवा चलाउने उपकरण
- AWS सेवाका public API मा सीधा पहुँच
- संसाधन व्यवस्थापन गर्न **script** लेख्न सकिने
- Open-source: https://github.com/aws/aws-cli
- Console को विकल्प

### AWS SDK के हो?
- **भाषा-विशेष API** (library को सेट)
- तपाईंको एप्लिकेसनभित्रै embed हुन्छ
- समर्थित: JavaScript, Python, PHP, .NET, Ruby, Java, Go, Node.js, C++
- Mobile SDK: Android, iOS
- IoT Device SDK: Embedded C, Arduino
- **रोचक कुरा:** AWS CLI आफैं Python को AWS SDK मा बनेको हो

---

## ६. IAM Role ⭐

केही AWS सेवाहरूले तपाईंको तर्फबाट काम गर्नुपर्छ। तिनलाई अनुमति दिन **IAM Role** प्रयोग गरिन्छ।

**सामान्य Role हरू:**
- **EC2 Instance Role** — EC2 ले S3 बाट फाइल पढ्नुपर्‍यो भने
- **Lambda Function Role**
- **CloudFormation Role**

```
EC2 Instance (भर्चुअल सर्भर) ──[IAM Role]──> AWS सेवामा पहुँच
```

> ⚠️ **कहिल्यै EC2 भित्र Access Key नराख्नुहोस् — त्यसको सट्टा IAM Role प्रयोग गर्नुहोस्।** यो परीक्षामा सोधिन्छ।

---

## ७. IAM का २ सुरक्षा उपकरण ⭐

| उपकरण | तह | काम |
|---|---|---|
| **IAM Credentials Report** | **Account-level** | account का सबै user र तिनका credential को अवस्था देखाउने रिपोर्ट |
| **IAM Access Advisor** | **User-level** | कुन user लाई कुन सेवाको अनुमति छ र त्यो सेवा **अन्तिम पटक कहिले प्रयोग भयो** भन्ने देखाउने |

> **Access Advisor को उपयोग:** कुनै user ले ६ महिनादेखि एउटा सेवा नै प्रयोग गरेको छैन भने त्यो अनुमति हटाउन सकिन्छ — least privilege लागू गर्न।

**सम्झने तरिका:**
- **Credentials Report** = पूरै account को **रिपोर्ट कार्ड**
- **Access Advisor** = एउटा user को **सल्लाहकार**

---

## ८. IAM का उत्कृष्ट अभ्यास (Best Practices) ⭐⭐

यो सूची परीक्षामा सिधै सोधिन्छ:

1. **Root account** AWS account सेटअप बाहेक कहिल्यै प्रयोग नगर्ने
2. **एक भौतिक व्यक्ति = एक AWS user**
3. User लाई **group** मा राख्ने, अनुमति **group** लाई दिने
4. बलियो **password policy** बनाउने
5. **MFA** प्रयोग गर्ने र अनिवार्य बनाउने
6. AWS सेवालाई अनुमति दिन **Role** बनाउने र प्रयोग गर्ने
7. Programmatic पहुँच (CLI/SDK) को लागि **Access Key** प्रयोग गर्ने
8. **IAM Credentials Report र Access Advisor** ले अनुमतिको audit गर्ने
9. **IAM user र Access Key कहिल्यै नबाँड्ने**

---

## ९. IAM को Shared Responsibility Model

| **AWS को जिम्मा** | **तपाईंको जिम्मा** |
|---|---|
| Infrastructure (global network security) | User, Group, Role, Policy को व्यवस्थापन र निगरानी |
| Configuration र vulnerability विश्लेषण | सबै account मा MFA सक्षम गर्ने |
| Compliance validation | Key नियमित रूपमा rotate गर्ने |
| | IAM उपकरणले उपयुक्त अनुमति लागू गर्ने |
| | पहुँचको ढाँचा विश्लेषण गर्ने र अनुमति पुनरावलोकन गर्ने |

---

## 📝 अभ्यास प्रश्नहरू

**प्रश्न १.** IAM Group बारे कुन कथन सही हो?

A) Group भित्र अर्को group राख्न सकिन्छ
B) एउटा user एउटै group मा मात्र हुन सक्छ
C) Group मा user मात्र राख्न सकिन्छ, अर्को group राख्न मिल्दैन
D) हरेक user कुनै न कुनै group मा हुनैपर्छ

<details><summary>उत्तर</summary>

**C)** — Group मा user मात्र राखिन्छ। Nested group हुँदैन। User धेरै group मा हुन सक्छ र कुनै group मा नहुन पनि सक्छ।
</details>

---

**प्रश्न २.** EC2 instance ले S3 bucket बाट फाइल पढ्नुपर्‍यो। सबैभन्दा सुरक्षित तरिका कुन हो?

A) EC2 भित्र Access Key र Secret Key फाइलमा राख्ने
B) EC2 लाई IAM Role assign गर्ने
C) S3 bucket लाई public बनाउने
D) Root account को credential प्रयोग गर्ने

<details><summary>उत्तर</summary>

**B) IAM Role** — AWS सेवालाई अनुमति दिने सही तरिका Role हो। Access Key कहिल्यै instance भित्र नराख्नुहोस्।
</details>

---

**प्रश्न ३.** एउटा user ले कुन सेवाहरू कहिले अन्तिम पटक प्रयोग गर्‍यो भन्ने हेर्न कुन उपकरण प्रयोग गर्ने?

A) IAM Credentials Report
B) AWS CloudTrail
C) IAM Access Advisor
D) AWS Config

<details><summary>उत्तर</summary>

**C) IAM Access Advisor** — user-level मा, कुन सेवाको अनुमति छ र अन्तिम पहुँच कहिले भयो देखाउँछ।
</details>

---

**प्रश्न ४.** MFA को मुख्य फाइदा के हो?

A) पासवर्ड याद राख्नुपर्दैन
B) पासवर्ड चोरी भए पनि account सुरक्षित रहन्छ
C) लगइन छिटो हुन्छ
D) धेरै account एउटै पासवर्डले चल्छ

<details><summary>उत्तर</summary>

**B)** — MFA = "थाहा भएको कुरा (पासवर्ड) + भएको कुरा (यन्त्र)"। पासवर्ड मात्र चोरिए पुग्दैन।
</details>

---

**प्रश्न ५.** IAM Policy मा `Version` फिल्डको मान के हुन्छ?

A) चालु मिति
B) 1.0
C) 2012-10-17
D) प्रयोगकर्ताले छान्ने

<details><summary>उत्तर</summary>

**C) 2012-10-17** — यो policy भाषाको संस्करण हो, सधैं यही राखिन्छ।
</details>

---

**प्रश्न ६.** AWS मा पहुँच गर्ने ३ तरिका कुन-कुन हुन्?

A) Console, CLI, SDK
B) Console, SSH, RDP
C) CLI, SSH, API Gateway
D) SDK, VPN, Console

<details><summary>उत्तर</summary>

**A)** — Management Console (पासवर्ड + MFA), CLI (Access Key), SDK (Access Key)।
</details>

---

## ✅ यो मोड्युलको सार

- **User**: भौतिक व्यक्ति, Console पासवर्ड हुन्छ
- **Group**: user मात्र राख्ने, nested हुँदैन
- **Policy**: JSON document — कुन काम गर्न पाइन्छ तोक्ने। Version सधैं `2012-10-17`
- **Role**: EC2 वा AWS सेवाको लागि अनुमति
- **सुरक्षा**: MFA + Password Policy
- **AWS CLI**: command line बाट सेवा चलाउने
- **AWS SDK**: प्रोग्रामिङ भाषाबाट सेवा चलाउने
- **Access Keys**: CLI/SDK को लागि
- **Audit**: IAM Credentials Report (account-level) र IAM Access Advisor (user-level)
- **मूल सिद्धान्त:** Least Privilege

---

⬅️ [मोड्युल ०१](01-cloud-computing.md) | ➡️ [मोड्युल ०३ — EC2](03-ec2.md)
