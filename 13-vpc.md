# मोड्युल १३ — Amazon VPC (Virtual Private Cloud)

> ℹ️ **महत्त्वपूर्ण नोट:** VPC लाई **AWS Solutions Architect Associate** र **SysOps Administrator** परीक्षामा गहिरो जान्नुपर्छ। **Cloud Practitioner मा यसबाट १–२ प्रश्न मात्र आउँछन्।**
>
> त्यसैले यहाँ **उच्च-स्तरीय बुझाइ** मात्र चाहिन्छ। सबै बुझिएन भने चिन्ता नगर्नुहोस् — अन्त्यको सारांश तालिका कण्ठ पार्नुहोस्।

---

## १. AWS मा IP Address ⭐

### IPv4 — Internet Protocol version 4 (४.३ अर्ब ठेगाना)

| प्रकार | विवरण |
|---|---|
| **Public IPv4** | इन्टरनेटमा प्रयोग हुने। ⚠️ **EC2 instance stop गरेर start गर्दा नयाँ public IP आउँछ** (पूर्वनिर्धारित) |
| **Private IPv4** | निजी network (LAN) मा प्रयोग हुने — जस्तै `192.168.1.1`। ✅ **EC2 start/stop गर्दा पनि नबदलिने** |
| **Elastic IP** | EC2 मा **स्थिर (fixed) public IPv4** जोड्ने |

> ⚠️ **मूल्य:** AWS मा **सबै public IPv4 को $0.005 प्रति घण्टा** शुल्क लाग्छ — Elastic IP सहित।
> **त्यसैले नचाहिने Elastic IP release गर्नुहोस्!**

### IPv6 — Internet Protocol version 6 (३.४ × १०³⁸ ठेगाना)
- AWS मा **हरेक IPv6 ठेगाना public हुन्छ** (private range छैन)
- उदाहरण: `2001:db8:3333:4444:cccc:dddd:eeee:ffff`
- ✅ **निःशुल्क**

---

## २. VPC र Subnet ⭐⭐

| शब्द | अर्थ | दायरा |
|---|---|---|
| **VPC** (Virtual Private Cloud) | आफ्ना संसाधन deploy गर्ने **निजी network** | **Region-level संसाधन** |
| **Subnet** | VPC भित्रको network विभाजन | **Availability Zone-level संसाधन** |

- **Public subnet** = इन्टरनेटबाट पहुँच गर्न मिल्ने subnet
- **Private subnet** = इन्टरनेटबाट पहुँच गर्न **नमिल्ने** subnet
- इन्टरनेट र subnet बीचको पहुँच परिभाषित गर्न **Route Table** प्रयोग हुन्छ

```
Region
└── VPC (CIDR Range: 10.0.0.0/16)
    ├── Availability Zone 1
    │   ├── Public subnet
    │   └── Private subnet
    └── Availability Zone 2
        ├── Public subnet
        └── Private subnet
```

> **सम्झने:** **VPC = Region मा। Subnet = AZ मा।**

---

## ३. Internet Gateway र NAT Gateway ⭐

| सेवा | काम |
|---|---|
| **Internet Gateway (IGW)** | VPC का instance लाई **इन्टरनेटसँग जोड्ने**। **Public subnet** को IGW मा route हुन्छ |
| **NAT Gateway** (AWS-managed) / **NAT Instance** (आफैं manage गर्ने) | **Private subnet** का instance लाई **private रहँदै इन्टरनेट पहुँच** दिने |

```
Private Subnet ──> NAT ──> Public Subnet ──> IGW ──> www
```

👉 यसो गर्दा private subnet को server ले software update डाउनलोड गर्न सक्छ, तर बाहिरबाट कसैले त्यो server सम्म पुग्न सक्दैन।

---

## ४. ⭐⭐ Network ACL (NACL) vs Security Group

**यो परीक्षामा पक्कै सोधिन्छ।**

| | **NACL (Network ACL)** | **Security Group** |
|---|---|---|
| कहाँ लाग्छ | **Subnet level** | **EC2 Instance level** (वा ENI) |
| के नियन्त्रण गर्छ | Subnet भित्र/बाहिरको traffic | EC2 instance भित्र/बाहिरको traffic |
| नियम | ✅ **ALLOW र DENY दुवै** | ⚠️ **ALLOW मात्र** |
| नियममा के राख्न मिल्छ | **IP address मात्र** | **IP address र अरू Security Group** |
| State | **Stateless** | **Stateful** |

> **सम्झने:**
> - **NACL = Subnet को गार्ड** — Stateless, DENY गर्न सक्ने, IP मात्र
> - **Security Group = Instance को गार्ड** — Stateful, ALLOW मात्र, SG लाई पनि सन्दर्भ गर्न सक्ने

---

## ५. VPC Flow Logs ⭐

तपाईंको interface मा जाने IP traffic को जानकारी कैद गर्ने:
- **VPC Flow Logs**
- **Subnet Flow Logs**
- **Elastic Network Interface Flow Logs**

**काम:** कनेक्टिभिटीको समस्या निगरानी र समाधान गर्न सहयोग। उदाहरण:
- Subnet देखि इन्टरनेट
- Subnet देखि subnet
- इन्टरनेट देखि subnet

⭐ **AWS-managed interface बाट पनि network जानकारी कैद गर्छ:** Elastic Load Balancer, ElastiCache, RDS, Aurora आदि।

**VPC Flow log डाटा कहाँ जान्छ:** **S3, CloudWatch Logs, र Amazon Data Firehose**

---

## ६. VPC Peering ⭐

- **दुई VPC लाई AWS को network मार्फत निजी रूपमा जोड्ने**
- तिनलाई **एउटै network मा भए जस्तो** बनाउने
- ⚠️ **CIDR (IP address range) एकआपसमा overlap हुनु हुँदैन**
- ⚠️ **VPC Peering transitive हुँदैन!** — जुन-जुन VPC ले कुरा गर्नुपर्छ, तिनको लागि **अलग-अलग peering बनाउनुपर्छ**

```
VPC A ⟷ VPC B  (peering छ)
VPC B ⟷ VPC C  (peering छ)
VPC A ⟷ VPC C  ← यसको लागि छुट्टै peering चाहिन्छ! (transitive होइन)
```

---

## ७. VPC Endpoints ⭐

- **सार्वजनिक www network को सट्टा निजी network प्रयोग गरेर AWS सेवामा जोड्ने**
- यसले **बढी सुरक्षा** र **कम latency** दिन्छ

| प्रकार | कुन सेवाको लागि |
|---|---|
| **VPC Endpoint Gateway** | **S3 र DynamoDB** |
| **VPC Endpoint Interface** | अधिकांश सेवा (S3 र DynamoDB सहित) |

> **परीक्षा टिप:** **"Gateway Endpoint = S3 र DynamoDB"** — यो जोडी याद गर्नुहोस्।

---

## ८. AWS PrivateLink (VPC Endpoint Services)

- **हजारौं VPC मा सेवा उपलब्ध गराउने सबैभन्दा सुरक्षित र scalable तरिका**
- ✅ **VPC peering, internet gateway, NAT, route table केही चाहिँदैन**
- ⚠️ **Network Load Balancer (Service VPC) र ENI (Customer VPC) चाहिन्छ**

```
3rd party VPC (Application service → NLB) ──AWS PrivateLink──> Your VPC (ENI → Consumer App)
```

---

## ९. Site-to-Site VPN र Direct Connect ⭐⭐

| | **Site-to-Site VPN** | **Direct Connect (DX)** |
|---|---|---|
| के गर्छ | On-premises VPN लाई AWS सँग जोड्ने | On-premises र AWS बीच **भौतिक कनेक्सन** बनाउने |
| Encryption | ✅ **स्वचालित रूपमा encrypted** | Private र सुरक्षित |
| बाटो | **सार्वजनिक इन्टरनेट** मार्फत | **निजी network** मार्फत |
| गति | इन्टरनेट अनुसार | **छिटो** |
| सेटअप समय | छिटो | ⚠️ **कम्तीमा १ महिना लाग्छ** |

> ⭐ **परीक्षा टिप:**
> - "**छिटो सेटअप चाहियो**, encrypted" → **Site-to-Site VPN**
> - "**निरन्तर उच्च bandwidth**, निजी, स्थिर" → **Direct Connect** (तर १ महिना लाग्छ!)

### Site-to-Site VPN का दुई भाग ⭐
- **On-premises तर्फ:** **Customer Gateway (CGW)** चाहिन्छ
- **AWS तर्फ:** **Virtual Private Gateway (VGW)** चाहिन्छ

---

## १०. AWS Client VPN

- आफ्नो कम्प्युटरबाट **OpenVPN** प्रयोग गरेर AWS र on-premises को निजी network मा जोडिने
- EC2 instance मा **private IP** मार्फत जोडिन दिने — मानौं तपाईं VPC कै निजी network भित्र हुनुहुन्छ
- **सार्वजनिक इन्टरनेट** मार्फत जान्छ

> **फरक:** **Site-to-Site VPN = पूरै data center जोड्ने। Client VPN = एउटा कम्प्युटर जोड्ने।**

---

## ११. AWS Transit Gateway ⭐

Network topology जटिल हुँदै जान्छ। हजारौं VPC र on-premises बीच **transitive peering** चाहिन्छ।

- **Hub-and-spoke (star) कनेक्सन**
- **एउटै Gateway** ले यो सुविधा दिने
- **Direct Connect Gateway र VPN connection** सँग काम गर्ने

```
        VPC A    VPC B    VPC C
           \       |       /
            \      |      /
         [ Transit Gateway ]
            /      |      \
     On-prem 1  On-prem 2  VPN
```

> **परीक्षा टिप:** प्रश्नमा **"हजारौं VPC जोड्ने"**, **"transitive"**, **"hub-and-spoke"** आयो → **Transit Gateway**

---

## 📝 अभ्यास प्रश्नहरू

**प्रश्न १.** NACL र Security Group बीचको मुख्य फरक के हो?

A) NACL मा ALLOW मात्र, SG मा ALLOW र DENY दुवै
B) NACL subnet level मा, SG instance level मा
C) NACL stateful, SG stateless
D) NACL मा SG लाई सन्दर्भ गर्न मिल्छ

<details><summary>उत्तर</summary>

**B)** — NACL subnet level (stateless, ALLOW+DENY, IP मात्र), SG instance level (stateful, ALLOW मात्र, SG सन्दर्भ गर्न मिल्ने)।
</details>

---

**प्रश्न २.** On-premises data center र AWS बीच निरन्तर उच्च-bandwidth को निजी कनेक्सन चाहियो। कुन सेवा?

A) Site-to-Site VPN
B) AWS Direct Connect
C) AWS Client VPN
D) VPC Peering

<details><summary>उत्तर</summary>

**B) AWS Direct Connect** — निजी, सुरक्षित र छिटो भौतिक कनेक्सन। तर सेटअपमा कम्तीमा १ महिना लाग्छ।
</details>

---

**प्रश्न ३.** Private subnet का EC2 instance ले इन्टरनेटबाट software update डाउनलोड गर्नुपर्‍यो, तर बाहिरबाट कसैले तिनमा पहुँच गर्न नपाओस्। के प्रयोग गर्ने?

A) Internet Gateway
B) NAT Gateway
C) VPC Peering
D) Transit Gateway

<details><summary>उत्तर</summary>

**B) NAT Gateway** — private subnet लाई outbound इन्टरनेट पहुँच दिन्छ तर inbound रोक्छ।
</details>

---

**प्रश्न ४.** VPC Peering बारे कुन सही हो?

A) Transitive हो — A-B र B-C भए A-C पनि जोडिन्छ
B) CIDR range overlap गर्न मिल्छ
C) Transitive होइन — प्रत्येक जोडीको लागि छुट्टै peering चाहिन्छ
D) एउटै region मा मात्र काम गर्छ

<details><summary>उत्तर</summary>

**C)** — VPC Peering transitive हुँदैन, र CIDR overlap गर्नु हुँदैन।
</details>

---

**प्रश्न ५.** सार्वजनिक इन्टरनेट प्रयोग नगरी VPC बाट S3 मा पहुँच गर्न के प्रयोग गर्ने?

A) NAT Gateway
B) VPC Endpoint Gateway
C) Internet Gateway
D) Transit Gateway

<details><summary>उत्तर</summary>

**B) VPC Endpoint Gateway** — S3 र DynamoDB को लागि Gateway endpoint।
</details>

---

**प्रश्न ६.** EC2 instance को public IP stop/start गर्दा नबदलियोस् भन्ने चाहनुहुन्छ। के प्रयोग गर्ने?

A) Private IP
B) Elastic IP
C) IPv6
D) NAT Gateway

<details><summary>उत्तर</summary>

**B) Elastic IP** — स्थिर public IPv4। तर प्रयोगमा नभए पनि पैसा लाग्छ, त्यसैले नचाहिए release गर्नुहोस्।
</details>

---

## ✅ यो मोड्युलको सार — VPC चिट-सिट ⭐⭐

**यो तालिका मात्र कण्ठ पार्नुभयो भने VPC को प्रश्न सजिलै आउँछ:**

| शब्द | एक-लाइन अर्थ |
|---|---|
| **VPC** | Virtual Private Cloud — **Region-level** निजी network |
| **Subnet** | **AZ मा बाँधिएको**, VPC को network विभाजन |
| **Internet Gateway** | **VPC level** मा — इन्टरनेट पहुँच दिने |
| **NAT Gateway / Instance** | **Private subnet** लाई इन्टरनेट पहुँच दिने |
| **NACL** | **Stateless**, **Subnet** को inbound/outbound नियम, ALLOW र DENY |
| **Security Group** | **Stateful**, **EC2 instance वा ENI** मा, ALLOW मात्र |
| **VPC Peering** | दुई VPC जोड्ने (overlap नभएको IP range), **transitive होइन** |
| **Elastic IP** | स्थिर public IPv4 — **प्रयोगमा नभए पनि खर्च लाग्ने** |
| **VPC Endpoints** | VPC भित्रबाट **AWS सेवामा निजी पहुँच** (Gateway = S3, DynamoDB) |
| **PrivateLink** | तेस्रो पक्षको VPC मा भएको सेवामा निजी रूपमा जोडिने |
| **VPC Flow Logs** | Network traffic को log |
| **Site to Site VPN** | On-premises DC र AWS बीच **इन्टरनेट मार्फत** VPN |
| **Client VPN** | आफ्नो कम्प्युटरबाट VPC मा **OpenVPN** कनेक्सन |
| **Direct Connect** | AWS सँग **सीधा निजी कनेक्सन** (सेटअपमा १ महिना) |
| **Transit Gateway** | **हजारौं VPC र on-premises network** एकसाथ जोड्ने |

---

⬅️ [मोड्युल १२](12-monitoring.md) | ➡️ [मोड्युल १४ — Security](14-security.md)
