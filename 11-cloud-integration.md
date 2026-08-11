# मोड्युल ११ — Cloud Integration (SQS, SNS, Kinesis, Amazon MQ)

> **यस मोड्युलमा:** एप्लिकेसन बीचको सञ्चार, decoupling, र ४ वटा integration सेवा।

---

## १. किन Integration सेवा चाहिन्छ? ⭐

धेरै एप्लिकेसन deploy गर्न थालेपछि तिनले एकआपसमा कुरा गर्नैपर्छ। **सञ्चारका २ ढाँचा** छन्:

### १️⃣ Synchronous communication (एप → एप)
```
Buying Service ────सीधै────> Shipping Service
```

### २️⃣ Asynchronous / Event based (एप → queue → एप)
```
Buying Service ──> Queue ──> Shipping Service
```

### Synchronous को समस्या ⭐

Traffic अचानक बढ्दा synchronous सञ्चार समस्याग्रस्त हुन्छ।

> **उदाहरण:** सामान्यतया १० वटा भिडियो encode गर्नुपर्ने ठाउँमा अचानक १००० वटा आयो भने के गर्ने?

**👉 समाधान: एप्लिकेसन decouple गर्ने!**

| सेवा | मोडेल |
|---|---|
| **SQS** | **Queue** model |
| **SNS** | **Pub/Sub** model |
| **Kinesis** | **Real-time data streaming** model |

⭐ **यी सेवाहरू तपाईंको एप्लिकेसनबाट स्वतन्त्र रूपमा scale हुन सक्छन्!**

---

## २. Amazon SQS — Simple Queue Service ⭐⭐

**Queue के हो?**

```
Producer ┐
Producer ├──[messages पठाउँछ]──> SQS Queue ──[messages poll गर्छ]──> Consumer
Producer ┘                                                          Consumer
                                                                    Consumer
```

### SQS Standard Queue का विशेषता ⭐

| विशेषता | विवरण |
|---|---|
| उमेर | AWS को **सबैभन्दा पुरानो सेवा** (१० वर्ष भन्दा बढी) |
| प्रकार | पूर्ण managed (~serverless), **एप्लिकेसन decouple गर्न** |
| Scale | प्रति सेकेन्ड **१ message देखि हजारौं** सम्म |
| **पूर्वनिर्धारित retention** | **४ दिन**, **अधिकतम १४ दिन** ⭐ |
| Message सङ्ख्या | **कुनै सीमा छैन** |
| Message को अन्त्य | **Consumer ले पढेपछि मेटिन्छ** |
| Latency | कम (<१० ms publish र receive मा) |
| Consumer | काम बाँडेर पढ्छन्, **horizontally scale** हुन्छन् |

### SQS ले tier बीच decouple गर्ने उदाहरण

```
WEB SERVERS (ASG मा EC2)  →  [PUT]  →  SQS Queue  →  VIDEO PROCESSING (ASG मा EC2)
     ↑ request आउँछ                        ↑ queue को लम्बाइ अनुसार scaling
```

👉 Web server लाई video processing सकिने पर्खनुपर्दैन। Queue लामो भयो भने processing तर्फको ASG ले आफैं instance थप्छ।

### SQS FIFO Queue ⭐

**FIFO = First In First Out** (queue मा message को क्रम)

```
Producer → [1][2][3][4] → Consumer → [1][2][3][4]
```

- ⭐ **Consumer ले message ठ्याक्कै क्रममा प्रशोधन गर्छ**

> **परीक्षा टिप:** "message को क्रम महत्त्वपूर्ण छ" → **SQS FIFO Queue**

---

## ३. Amazon Kinesis ⭐

> **परीक्षाको लागि: Kinesis = real-time big data streaming**

- **Real-time streaming data** जुनसुकै scale मा सङ्कलन, प्रशोधन र विश्लेषण गर्ने managed सेवा

**Cloud Practitioner को लागि धेरै विस्तृत, तर जान्न राम्रो:**

| सेवा | काम |
|---|---|
| **Amazon Kinesis Data Streams** | लाखौं स्रोतबाट कम latency मा डाटा ingest गर्ने |
| **Amazon Data Firehose** | Kinesis Data Streams को डाटा **Amazon S3, Redshift, OpenSearch** आदिमा लोड गर्ने |

### उच्च-स्तरीय चित्र

```
Click Streams  ┐
IoT devices    ├──> Amazon Kinesis Data Streams ──> Amazon Data Firehose ──┬──> Amazon S3
Metrics & Logs ┘                                                          └──> Amazon Redshift
     (Sources)                                                                (Destinations)
```

> **परीक्षा टिप:** प्रश्नमा **"real-time"** र **"streaming"** आयो → **Kinesis**

---

## ४. Amazon SNS — Simple Notification Service ⭐⭐

**समस्या:** एउटै message धेरै जनालाई पठाउनुपर्‍यो भने?

### Pub/Sub (Publish / Subscribe) मोडेल

```
                        ┌──> Email notification
Buying Service ──> SNS Topic ──> Fraud Service
                        ├──> Shipping Service
                        └──> SQS Queue
```

### SNS का विशेषता ⭐

- **"Event publisher" ले एउटै SNS topic मा मात्र message पठाउँछ**
- जति चाहियो त्यति **"event subscriber"** ले त्यो topic सुन्न सक्छन्
- ⭐ **हरेक subscriber ले सबै message पाउँछ**
- **प्रति topic १,२५,००,००० subscription सम्म**, **१,००,००० topic को सीमा**

### Subscriber हुन सक्ने सेवाहरू ⭐

**SQS, Lambda, Amazon Data Firehose, HTTP(S) Endpoints, SMS र Mobile Notifications, Emails**

### ⭐⭐ SQS vs SNS को फरक (परीक्षामा पक्कै आउँछ)

| | **SQS** | **SNS** |
|---|---|---|
| मोडेल | **Queue** | **Pub/Sub** |
| Consumer | Consumer हरूले **काम बाँड्छन्** — एउटा message एउटैले पढ्छ | **सबै subscriber ले सबै message पाउँछन्** |
| Message retention | **४ दिन (अधिकतम १४ दिन)** | ❌ **कुनै retention छैन** |
| Consumer ले | Message **poll** गर्छ | Message **push** हुन्छ |
| उद्देश्य | एप decouple गर्ने | Notification पठाउने |

---

## ५. Amazon MQ ⭐

- **SQS र SNS "cloud-native" सेवा हुन्** — AWS का proprietary प्रोटोकल
- On-premises बाट आएका परम्परागत एप्लिकेसनले **खुला प्रोटोकल** प्रयोग गर्न सक्छन्: **MQTT, AMQP, STOMP, Openwire, WSS**
- क्लाउडमा migrate गर्दा एप्लिकेसनलाई SQS/SNS प्रयोग गर्न re-engineer गर्नुको सट्टा **Amazon MQ** प्रयोग गर्न सकिन्छ

**Amazon MQ = ActiveMQ र RabbitMQ को managed message broker सेवा**

### ध्यान दिनुपर्ने ⭐
- ⚠️ **Amazon MQ ले SQS/SNS जति "scale" गर्दैन**
- **Server मा चल्छ**, **Multi-AZ मा failover सहित** चल्न सक्ने
- **दुवै सुविधा छन्:** queue (~SQS) र topic (~SNS)

> **परीक्षा टिप:** प्रश्नमा **"MQTT", "AMQP", "RabbitMQ", "ActiveMQ"** वा **"एप्लिकेसन नबदली migrate गर्ने"** आयो → **Amazon MQ**

---

## 📝 अभ्यास प्रश्नहरू

**प्रश्न १.** एउटा वेब एपले प्रयोगकर्ताको request लिन्छ, त्यसपछि भिडियो प्रशोधन गर्नुपर्छ जुन ढिलो हुन्छ। दुई भागलाई decouple गर्न कुन सेवा?

A) Amazon SNS
B) Amazon SQS
C) Amazon Kinesis
D) AWS Lambda

<details><summary>उत्तर</summary>

**B) Amazon SQS** — queue मार्फत producer र consumer लाई decouple गर्ने classic उदाहरण।
</details>

---

**प्रश्न २.** एउटै order को सूचना एकैचोटि email, SMS र ३ वटा microservice मा पुर्‍याउनुपर्‍यो। कुन सेवा?

A) Amazon SQS
B) Amazon SNS
C) Amazon MQ
D) Amazon Kinesis

<details><summary>उत्तर</summary>

**B) Amazon SNS** — pub/sub मोडेल, सबै subscriber ले सबै message पाउँछन्।
</details>

---

**प्रश्न ३.** SQS मा message को पूर्वनिर्धारित retention अवधि कति हो?

A) १ दिन
B) ४ दिन
C) ७ दिन
D) १४ दिन

<details><summary>उत्तर</summary>

**B) ४ दिन** — पूर्वनिर्धारित ४ दिन, **अधिकतम १४ दिन** सम्म बढाउन मिल्छ।
</details>

---

**प्रश्न ४.** एउटा on-premises एप्लिकेसन MQTT प्रोटोकल प्रयोग गर्छ र AWS मा migrate गर्नुपर्‍यो, तर कोड बदल्न चाहँदैनन्। कुन सेवा?

A) Amazon SQS
B) Amazon SNS
C) Amazon MQ
D) Amazon Kinesis

<details><summary>उत्तर</summary>

**C) Amazon MQ** — MQTT, AMQP, STOMP जस्ता खुला प्रोटोकल समर्थन गर्छ, त्यसैले एप re-engineer गर्नुपर्दैन।
</details>

---

**प्रश्न ५.** IoT यन्त्रबाट आउने real-time streaming डाटा सङ्कलन र विश्लेषण गर्न कुन सेवा?

A) Amazon SQS
B) Amazon Kinesis
C) Amazon SNS
D) Amazon MQ

<details><summary>उत्तर</summary>

**B) Amazon Kinesis** — real-time big data streaming को लागि।
</details>

---

**प्रश्न ६.** SNS बारे कुन सही हो?

A) Message ४ दिनसम्म राखिन्छ
B) Message retention छैन
C) एउटा message एउटै consumer ले मात्र पाउँछ
D) FIFO क्रम ग्यारेन्टी गर्छ

<details><summary>उत्तर</summary>

**B)** — SNS मा message retention छैन। सबै subscriber ले सबै message पाउँछन्। Retention SQS को विशेषता हो।
</details>

---

## ✅ यो मोड्युलको सार

| सेवा | मुख्य कुरा |
|---|---|
| **SQS** | AWS मा **Queue सेवा**। धेरै Producer, message **१४ दिनसम्म** राखिने। धेरै Consumer ले काम बाँडेर पढ्ने र सकिएपछि मेटाउने। **AWS मा एप decouple गर्न प्रयोग हुने।** FIFO queue ले क्रम कायम राख्छ |
| **SNS** | AWS मा **Notification सेवा** (Pub/Sub)। Subscriber: Email, Lambda, SQS, HTTP, Mobile… **धेरै Subscriber, सबैलाई सबै message।** ⚠️ **Message retention छैन** |
| **Kinesis** | **Real-time data streaming**, persistence र विश्लेषण |
| **Amazon MQ** | क्लाउडमा **ActiveMQ र RabbitMQ** को managed message broker (MQTT, AMQP जस्ता प्रोटोकल) |

---

⬅️ [मोड्युल १०](10-global-infrastructure.md) | ➡️ [मोड्युल १२ — Monitoring](12-monitoring.md)
