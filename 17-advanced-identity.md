# मोड्युल १७ — Advanced Identity (STS, Cognito, Directory Services, IAM Identity Center)

> ℹ️ **सजिलो मोड्युल — ४ वटा सेवाको काम मात्र चिन्नुपर्छ। परीक्षामा १–३ प्रश्न आउँछ।**

---

## १. AWS STS — Security Token Service ⭐

**STS = तपाईंका AWS संसाधनमा पहुँचको लागि अस्थायी, सीमित-अधिकारको credential बनाउने।**

- ⭐ **छोटो-अवधिको credential** — तपाईंले समाप्ति अवधि तोक्नुहुन्छ

### प्रयोग ⭐
| प्रयोग | विवरण |
|---|---|
| **Identity federation** | बाहिरी सिस्टममा user identity व्यवस्थापन गरेर, तिनलाई AWS संसाधनमा पहुँच दिन **STS token** प्रदान गर्ने |
| **IAM Roles** | Cross-account वा same-account पहुँचको लागि |
| **IAM Roles for Amazon EC2** | EC2 instance लाई AWS संसाधन पहुँच गर्न अस्थायी credential दिने |

```
User → [assume role] → IAM Role (same/cross account) → STS Service
                                                            ↓
                                      Temporary Security Credentials
                                                            ↓ access
                                                     AWS Resources
```

> **सम्झने: STS = अस्थायी credential**

---

## २. Amazon Cognito ⭐

**Cognito = तपाईंका Web र Mobile एप्लिकेसनका प्रयोगकर्ताको identity (सम्भवतः लाखौं)।**

- ⭐ **तिनको लागि IAM user बनाउनुको सट्टा, Cognito मा user बनाउनुहुन्छ**

```
Web Applications  ┐
Mobile Applications├──login──> Amazon Cognito (Database of users)
                   │
Social Identity Provider (Facebook, Google, Twitter…) ──> Cognito
```

> ⭐⭐ **IAM vs Cognito को मुख्य फरक (परीक्षामा सोधिन्छ):**
> - **IAM** = **तपाईंको कम्पनीका कर्मचारी** — जसलाई तपाईं विश्वास गर्नुहुन्छ
> - **Cognito** = **तपाईंको एपका बाहिरी प्रयोगकर्ता** (ग्राहक, लाखौं हुन सक्ने)

---

## ३. Microsoft Active Directory (AD) के हो? ⭐

- **AD Domain Services** भएको जुनसुकै Windows Server मा पाइने
- **Object को database:** User Account, Computer, Printer, File Share, Security Group
- **केन्द्रीकृत security व्यवस्थापन** — account बनाउने, अनुमति दिने

```
John + Password → Domain Controller → प्रमाणीकरण
```

---

## ४. AWS Directory Services ⭐⭐

**३ विकल्प — यो तालिका परीक्षामा सोधिन्छ:**

| सेवा | विवरण |
|---|---|
| **AWS Managed Microsoft AD** | ⭐ **AWS मै आफ्नै AD बनाउने**, user स्थानीय रूपमा व्यवस्थापन गर्ने, **MFA समर्थन**। आफ्नो on-premise AD सँग **"trust" कनेक्सन** स्थापना गर्न सकिने |
| **AD Connector** | ⭐ **Directory Gateway (proxy)** — on-premise AD मा redirect गर्ने, **MFA समर्थन**। ⚠️ **User on-premise AD मै व्यवस्थापन हुन्छन्** |
| **Simple AD** | ⭐ AWS मा **AD-compatible managed directory**। ⚠️ **On-premise AD सँग जोड्न मिल्दैन** |

```
AWS Managed AD ←──trust──→ On-prem AD        (दुवैतिर user, trust सम्बन्ध)
AD Connector   ──proxy───→ On-prem AD        (user on-prem मै मात्र)
Simple AD      (एक्लै)                        (on-prem सँग जोडिन्न)
```

> **सम्झने:**
> - **Managed Microsoft AD** = AWS मा आफ्नै AD + on-prem सँग trust
> - **AD Connector** = कुनै user नराख्ने, on-prem मा पठाइदिने **proxy**
> - **Simple AD** = सस्तो, एक्लै, on-prem सँग जोडिन नमिल्ने

---

## ५. AWS IAM Identity Center ⭐⭐

*(पहिलेको नाम: **AWS Single Sign-On / AWS SSO**)*

**एउटै लगइन (single sign-on) ले यी सबैमा पहुँच:**

- ⭐ **AWS Organizations का सबै AWS account**
- **व्यावसायिक क्लाउड एप्लिकेसन** (जस्तै Salesforce, Box, Microsoft 365…)
- **SAML2.0-सक्षम एप्लिकेसन**
- **EC2 Windows Instances**

### Identity Provider (पहिचान प्रदायक)
- **IAM Identity Center मै भएको built-in identity store**
- **तेस्रो पक्ष:** Active Directory (AD), OneLogin, Okta…

> **परीक्षा टिप:** प्रश्नमा **"single sign-on"**, **"धेरै AWS account मा एउटै लगइन"** आयो → **IAM Identity Center**

---

## 📝 अभ्यास प्रश्नहरू

**प्रश्न १.** एउटा mobile एपका १० लाख प्रयोगकर्ताको लागि login सुविधा चाहियो, Facebook र Google login सहित। कुन सेवा?

A) AWS IAM
B) Amazon Cognito
C) AWS Directory Service
D) AWS STS

<details><summary>उत्तर</summary>

**B) Amazon Cognito** — web र mobile एपका बाहिरी प्रयोगकर्ताको identity, social identity provider सहित।
</details>

---

**प्रश्न २.** एउटा कम्पनीले on-premises Active Directory मै user राख्न चाहन्छ तर AWS मा प्रमाणीकरण गराउन चाहन्छ। AWS मा user को copy राख्न चाहँदैन। कुन सेवा?

A) AWS Managed Microsoft AD
B) AD Connector
C) Simple AD
D) Amazon Cognito

<details><summary>उत्तर</summary>

**B) AD Connector** — proxy को रूपमा काम गर्छ, user on-premise AD मै रहन्छन्।
</details>

---

**प्रश्न ३.** धेरै AWS account र Salesforce जस्ता व्यावसायिक एपमा एउटै लगइनले पहुँच चाहियो। कुन सेवा?

A) Amazon Cognito
B) AWS IAM Identity Center
C) AWS STS
D) AWS Organizations

<details><summary>उत्तर</summary>

**B) AWS IAM Identity Center** (पहिलेको AWS SSO)।
</details>

---

**प्रश्न ४.** EC2 instance लाई अस्थायी, सीमित-अधिकारको credential दिन कुन सेवाले काम गर्छ?

A) Amazon Cognito
B) AWS STS
C) AWS Secrets Manager
D) AWS KMS

<details><summary>उत्तर</summary>

**B) AWS STS** — Security Token Service ले अस्थायी credential दिन्छ (IAM Role assume गर्दा पछाडि यही चल्छ)।
</details>

---

**प्रश्न ५.** IAM र Cognito बीच के फरक छ?

A) IAM सस्तो छ
B) IAM आन्तरिक (कम्पनीका) user को लागि, Cognito एपका बाहिरी user को लागि
C) Cognito global हो, IAM regional
D) कुनै फरक छैन

<details><summary>उत्तर</summary>

**B)** — IAM = तपाईंले विश्वास गर्ने कम्पनीका user। Cognito = web/mobile एपका ग्राहक (लाखौं हुन सक्ने)।
</details>

---

## ✅ यो मोड्युलको सार

| सेवा | एक-लाइन काम |
|---|---|
| **IAM** | तपाईंको AWS account भित्रको Identity and Access Management — **तपाईंले विश्वास गर्ने, कम्पनीका user** को लागि |
| **Organizations** | धेरै account व्यवस्थापन |
| **STS (Security Token Service)** | AWS संसाधन पहुँचको लागि **अस्थायी, सीमित-अधिकारको credential** |
| **Cognito** | तपाईंका **mobile र web एप्लिकेसनका user** को database बनाउने |
| **Directory Services** | AWS मा **Microsoft Active Directory** एकीकृत गर्ने |
| **IAM Identity Center** | धेरै AWS account र एप्लिकेसनको लागि **एउटै लगइन (SSO)** |

---

⬅️ [मोड्युल १६](16-billing-support.md) | ➡️ [मोड्युल १८ — अन्य AWS सेवा](18-other-services.md)
