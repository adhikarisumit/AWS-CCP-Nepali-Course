# मोड्युल १५ — Machine Learning सेवाहरू

> ℹ️ **यो सजिलो मोड्युल हो।** हरेक सेवाको **एक-लाइन काम** मात्र याद गर्नुहोस् — परीक्षामा "यो काम गर्न कुन सेवा?" भन्ने प्रश्न आउँछ।

---

## १. Amazon Rekognition ⭐

**ML प्रयोग गरेर image र video मा वस्तु, मानिस, text र दृश्य पत्ता लगाउने।**

- Face analysis र face search — user verification, मानिस गन्ने
- "परिचित अनुहार" को database बनाउने वा celebrity सँग तुलना गर्ने

**प्रयोग:**
- **Labeling** (लेबल लगाउने)
- **Content Moderation** (आपत्तिजनक सामग्री छान्ने)
- **Text Detection**
- **Face Detection र Analysis** (लिङ्ग, उमेर समूह, भावना…)
- **Face Search र Verification**
- **Celebrity Recognition**
- **Pathing** (खेलकुदको विश्लेषणमा)

> **सम्झने: 📷 तस्बिर/भिडियो → Rekognition**

---

## २. Amazon Transcribe ⭐

**बोली (speech) लाई स्वचालित रूपमा text मा बदल्ने।**

- **Automatic Speech Recognition (ASR)** नामक deep learning प्रक्रिया प्रयोग गर्छ — छिटो र सही
- ⭐ **Redaction** मार्फत **PII (व्यक्तिगत पहिचान जानकारी) स्वचालित रूपमा हटाउने**
- **बहुभाषी अडियोको लागि Automatic Language Identification** समर्थन

**प्रयोग:**
- Customer service call को transcribe गर्ने
- **Closed captioning र subtitle** स्वचालित बनाउने
- Media asset को metadata बनाएर खोज्न मिल्ने archive बनाउने

> **सम्झने: 🎤 आवाज → लेख = Transcribe**

---

## ३. Amazon Polly ⭐

**Deep learning प्रयोग गरेर text लाई जीवन्त आवाजमा बदल्ने।**

बोल्न सक्ने एप्लिकेसन बनाउन दिन्छ।

> **सम्झने: 📝 लेख → आवाज = Polly** (Polly = तोता, बोल्छ 🦜)

---

## ४. Amazon Translate ⭐

**प्राकृतिक र सही भाषा अनुवाद।**

- अन्तर्राष्ट्रिय प्रयोगकर्ताको लागि वेबसाइट र एप्लिकेसन जस्ता सामग्री **localize** गर्न दिन्छ
- ठूलो मात्राको text सजिलै र कुशलतापूर्वक अनुवाद गर्न

> **सम्झने: 🌐 भाषा अनुवाद = Translate**

---

## ५. Amazon Lex र Amazon Connect ⭐

### Amazon Lex
- ⭐ **Alexa चलाउने उही प्रविधि!**
- **Automatic Speech Recognition (ASR)** — बोलीलाई text मा
- **Natural Language Understanding** — text/caller को **मनसाय (intent)** पहिचान गर्ने
- **Chatbot, call center bot** बनाउन सहयोग

### Amazon Connect
- **Call प्राप्त गर्ने, contact flow बनाउने — क्लाउड-आधारित virtual contact center**
- अन्य CRM सिस्टम वा AWS सँग एकीकृत गर्न सकिने
- ⭐ **कुनै अग्रिम भुक्तानी छैन, परम्परागत contact center समाधान भन्दा ८०% सस्तो**

```
Phone Call → Connect → [call stream] → Lex (intent पहिचान) → Lambda → CRM (appointment मिलाउने)
```

> **सम्झने: Lex = चेटबट/Alexa को दिमाग। Connect = क्लाउड कल सेन्टर।**

---

## ६. Amazon Comprehend ⭐

**Natural Language Processing (NLP) को लागि।**

- **पूर्ण managed र serverless सेवा**
- ML प्रयोग गरेर text मा **अन्तर्दृष्टि र सम्बन्ध** पत्ता लगाउने:
  - Text को **भाषा**
  - **मुख्य वाक्यांश, ठाउँ, मानिस, ब्रान्ड, घटना** निकाल्ने
  - Text कति **सकारात्मक वा नकारात्मक** छ बुझ्ने (sentiment)
  - Tokenization र parts of speech ले text विश्लेषण
  - Text file हरूलाई **विषय अनुसार स्वचालित रूपमा समूहबद्ध** गर्ने

**प्रयोग:**
- ग्राहक अन्तरक्रिया (इमेल) विश्लेषण गरेर कुन कुराले सकारात्मक/नकारात्मक अनुभव ल्याउँछ पत्ता लगाउने
- Comprehend ले पत्ता लगाएका विषय अनुसार लेख समूहबद्ध गर्ने

> **सम्झने: 🧠 Comprehend = बुझ्नु = NLP, sentiment analysis**

---

## ७. Amazon SageMaker AI ⭐

**Developer / data scientist ले ML model बनाउने पूर्ण managed सेवा।**

सामान्यतया यी सबै प्रक्रिया एकै ठाउँमा गर्न + server provision गर्न गाह्रो हुन्छ। SageMaker ले त्यो सजिलो बनाउँछ।

### ML प्रक्रिया (सरलीकृत) — परीक्षाको अङ्क अनुमान गर्ने उदाहरण

```
ऐतिहासिक डाटा:                    → Model बनाउने → Train र Tune गर्ने
- IT मा कति वर्षको अनुभव                              ↓
- AWS मा कति वर्षको अनुभव          नयाँ डाटा → Model लागू → भविष्यवाणी:
- कोर्समा कति समय बिताएको                              "९०६ अङ्कमा PASS"
   (label: 670, 890, 934)
```

> **सम्झने: 🔧 आफ्नै ML model बनाउने = SageMaker**
> (बाँकी सबै ML सेवा तयारी model हुन्; SageMaker मात्र "आफैं बनाउने")

---

## ८. Amazon Kendra ⭐

**ML ले चल्ने पूर्ण managed document search सेवा।**

- Document भित्रैबाट **उत्तर निकाल्ने** (text, PDF, HTML, PowerPoint, MS Word, FAQ…)
- **Natural language search** क्षमता
- ⭐ **प्रयोगकर्ताको अन्तरक्रिया/प्रतिक्रियाबाट सिक्ने** — मनपर्ने नतिजा माथि ल्याउने (**Incremental Learning**)
- Search result **हातैले मिलाउने** क्षमता (डाटाको महत्त्व, ताजापन, custom…)

**डाटा स्रोत:** Amazon S3, Amazon RDS, Google Drive, MS SharePoint, MS OneDrive, तेस्रो पक्ष, APN, Custom

```
User: "IT support desk कहाँ छ?" → Kendra → "पहिलो तला"
```

> **सम्झने: 🔍 कागजातभित्र खोज्ने = Kendra**

---

## ९. Amazon Personalize ⭐

**Real-time व्यक्तिगत सिफारिस सहितको एप बनाउने पूर्ण managed ML सेवा।**

- उदाहरण: व्यक्तिगत उत्पादन सिफारिस/re-ranking, customized direct marketing
- उदाहरण: प्रयोगकर्ताले बगैंचाका औजार किन्यो → अर्को के किन्ने सिफारिस
- ⭐ **Amazon.com ले प्रयोग गर्ने उही प्रविधि**
- अवस्थित वेबसाइट, एप्लिकेसन, SMS, इमेल marketing सिस्टममा एकीकृत
- ⭐ **महिनौं होइन, दिनमै लागू** (ML समाधान आफैं बनाउनु, train गर्नु, deploy गर्नु पर्दैन)

**प्रयोग:** खुद्रा पसल, मिडिया र मनोरञ्जन

> **सम्झने: 🛒 सिफारिस (recommendation) = Personalize**

---

## १०. Amazon Textract ⭐

**AI र ML प्रयोग गरेर scan गरिएको कुनै पनि कागजातबाट text, हस्तलेखन र डाटा स्वचालित रूपमा निकाल्ने।**

- **Form र table बाट डाटा निकाल्ने**
- जुनसुकै किसिमको कागजात पढ्ने र प्रशोधन गर्ने (PDF, image…)

**प्रयोग:**
- **वित्तीय सेवा** — invoice, financial report
- **स्वास्थ्य** — medical record, insurance claim
- **सार्वजनिक क्षेत्र** — tax form, ID document, passport

```
Scan गरिएको ID → Textract → { "Document ID": "123456789-005", "SEX": "F", "DOB": "23.05.1997", … }
```

> **सम्झने: 📄 कागजात/स्क्यान → डाटा = Textract**
> ⚠️ **Textract vs Transcribe को भ्रम नपार्नुहोस्:**
> - **Textract** = कागजात/तस्बिरबाट **text निकाल्ने** (OCR)
> - **Transcribe** = **आवाजबाट** text बनाउने

---

## 📝 अभ्यास प्रश्नहरू

**प्रश्न १.** ग्राहक सेवाको फोन रेकर्डिङलाई लिखित रूपमा बदल्नुपर्‍यो। कुन सेवा?

A) Amazon Polly
B) Amazon Transcribe
C) Amazon Textract
D) Amazon Comprehend

<details><summary>उत्तर</summary>

**B) Amazon Transcribe** — आवाज → text।
</details>

---

**प्रश्न २.** वेबसाइटले प्रयोगकर्तालाई text पढेर सुनाउने सुविधा दिनुपर्‍यो। कुन सेवा?

A) Amazon Transcribe
B) Amazon Polly
C) Amazon Lex
D) Amazon Translate

<details><summary>उत्तर</summary>

**B) Amazon Polly** — text → जीवन्त आवाज।
</details>

---

**प्रश्न ३.** Scan गरिएको invoice र फारमबाट डाटा निकाल्नुपर्‍यो। कुन सेवा?

A) Amazon Rekognition
B) Amazon Textract
C) Amazon Comprehend
D) Amazon Kendra

<details><summary>उत्तर</summary>

**B) Amazon Textract** — scan गरिएको कागजात, form र table बाट text र डाटा निकाल्ने।
</details>

---

**प्रश्न ४.** एउटा e-commerce साइटले प्रयोगकर्तालाई व्यक्तिगत उत्पादन सिफारिस दिन चाहन्छ। कुन सेवा?

A) Amazon SageMaker
B) Amazon Personalize
C) Amazon Comprehend
D) Amazon Kendra

<details><summary>उत्तर</summary>

**B) Amazon Personalize** — real-time व्यक्तिगत सिफारिस। Amazon.com कै प्रविधि।
</details>

---

**प्रश्न ५.** ग्राहकको इमेलमा सकारात्मक वा नकारात्मक भावना छ पत्ता लगाउनुपर्‍यो। कुन सेवा?

A) Amazon Comprehend
B) Amazon Translate
C) Amazon Lex
D) Amazon Transcribe

<details><summary>उत्तर</summary>

**A) Amazon Comprehend** — NLP, sentiment analysis।
</details>

---

**प्रश्न ६.** एउटा टिमले आफ्नै custom machine learning model बनाउन, train गर्न र deploy गर्न चाहन्छ। कुन सेवा?

A) Amazon Rekognition
B) Amazon SageMaker
C) Amazon Personalize
D) Amazon Kendra

<details><summary>उत्तर</summary>

**B) Amazon SageMaker** — आफ्नै ML model बनाउने पूर्ण managed सेवा।
</details>

---

**प्रश्न ७.** एउटा एपले प्रयोगकर्ताले अपलोड गरेको तस्बिरमा आपत्तिजनक सामग्री छ कि छैन जाँच्नुपर्‍यो। कुन सेवा?

A) Amazon Textract
B) Amazon Rekognition
C) Amazon Comprehend
D) Amazon Macie

<details><summary>उत्तर</summary>

**B) Amazon Rekognition** — Content Moderation यसको प्रमुख प्रयोगमध्ये एक हो।
</details>

---

**प्रश्न ८.** ग्राहकसँग कुरा गर्न सक्ने chatbot बनाउनुपर्‍यो। कुन सेवा?

A) Amazon Connect
B) Amazon Lex
C) Amazon Polly
D) Amazon Comprehend

<details><summary>उत्तर</summary>

**B) Amazon Lex** — Alexa कै प्रविधि, chatbot बनाउन। (Connect = क्लाउड contact center)
</details>

---

## ✅ यो मोड्युलको सार — एक-लाइन तालिका ⭐

| सेवा | एक-लाइन काम | सम्झने चिह्न |
|---|---|---|
| **Rekognition** | अनुहार पत्ता लगाउने, labeling, celebrity recognition | 📷 |
| **Transcribe** | **आवाज → text** (subtitle आदि) | 🎤 |
| **Polly** | **Text → आवाज** | 🦜 |
| **Translate** | अनुवाद | 🌐 |
| **Lex** | संवादमूलक bot बनाउने — **chatbot** | 💬 |
| **Connect** | **क्लाउड contact center** | ☎️ |
| **Comprehend** | **Natural language processing** — sentiment | 🧠 |
| **SageMaker** | हरेक developer र data scientist को लागि **machine learning** | 🔧 |
| **Kendra** | **ML ले चल्ने search engine** | 🔍 |
| **Personalize** | **Real-time व्यक्तिगत सिफारिस** | 🛒 |
| **Textract** | **कागजातबाट text र डाटा** निकाल्ने | 📄 |

---

⬅️ [मोड्युल १४](14-security.md) | ➡️ [मोड्युल १६ — Billing र Support](16-billing-support.md)
