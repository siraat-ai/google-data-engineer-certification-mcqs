
# Designing Exactly-Once Streaming Pipelines on Google Cloud

## 🎭 Learning Dialogue: Mr. X vs. Mr. Artificial King

**Mr. X:**
Sir, humari organization ko ek streaming pipeline design karni hai jo financial transactions process kare. Requirements hain:

* High resilience
* Fault tolerance
* Low latency
* Exactly-once delivery semantics

Kaunsa architecture best hoga Google Cloud par?

---

**Mr. Artificial King:**
Beta, jab financial transactions ki baat hoti hai, to sabse critical cheez hoti hai **data duplication avoid karna** aur **loss prevent karna**. Yahan humein ek strong streaming stack chahiye.

Chalo step by step samajhte hain.

---

## 🔄 Streaming Data Kya Hota Hai?

Streaming data woh hota hai jo continuously aata rehta hai:

* Payment transactions
* ATM withdrawals
* Stock trades
* Real-time app events

Is type ka data batch mein process nahi kiya ja sakta. Humein real-time ya near real-time processing chahiye hoti hai.

---

## 📥 Ingestion Layer – Cloud Pub/Sub

**Cloud Pub/Sub** ek fully managed messaging service hai.

### Iska role kya hota hai?

* Producers messages publish karte hain.
* Subscribers un messages ko consume karte hain.
* Decoupling create hoti hai producers aur consumers ke beech.

### Financial Scenario:

Payment service ek transaction publish karegi Pub/Sub topic par.
Streaming pipeline us topic se data read karegi.

---

## 🔢 Exactly-Once Delivery Semantics

Exactly-once delivery ka matlab:

> Har message ek hi baar process ho — na duplicate, na loss.

Financial systems mein agar ek transaction do baar process ho jaye to:

* Double charge ho sakta hai
* Accounting mismatch ho sakta hai

Cloud Pub/Sub mein:

* Message acknowledgment mechanism hota hai.
* Ordering keys use kar sakte ho.
* Dataflow ke sath mil kar duplicate processing avoid hoti hai.

---

## ⚙️ Processing Layer – Cloud Dataflow

**Cloud Dataflow** ek fully managed stream aur batch processing service hai jo Apache Beam model use karta hai.

### Kyun Dataflow?

* Auto-scaling
* Fault tolerance
* Checkpointing
* Exactly-once processing support

### Fault Tolerance Kaise Achieve Hoti Hai?

* Worker crash ho jaye to state recover ho jati hai.
* Checkpoints maintain hote hain.
* Duplicate records automatically handle hote hain (depending on pipeline design).

---

## 📦 Apache Beam (Programming Model)

**Apache Beam** ek unified programming model hai jo streaming aur batch dono support karta hai.

Dataflow isko execute karta hai.

### Important Concepts:

* PCollection
* Windowing
* Triggers
* State & Timers

Financial use case mein:

* Windowing use hota hai time-based aggregation ke liye.
* State use hoti hai duplicate detection ke liye.

---

## 🔁 Pub/Sub Ordering

**Pub/Sub ordering** allow karta hai messages ko ek specific ordering key ke andar sequence maintain karne ka.

### Kab Useful Hai?

* Agar same account ke transactions sequence-sensitive hain.
* Ledger update karte waqt order important hai.

Yeh feature ensure karta hai ke ek hi ordering key ke messages correct sequence mein process hon.

---

## 🛡️ Fault Tolerance

Fault tolerance ka matlab:

> System failure ke baad bhi processing continue ho.

Dataflow mein:

* Automatic retry
* Checkpointing
* Distributed processing

Financial pipeline mein downtime unacceptable hota hai — isliye yeh critical feature hai.

---

## ⚡ Low Latency Processing

Low latency ka matlab:

> Data receive hone ke baad jaldi process ho jaye.

Dataflow streaming mode:

* Milliseconds to seconds level processing
* Real-time dashboards
* Instant fraud detection

---

## ❌ Kyun Dusre Options Ideal Nahi Hote?

### Dataprep + Cloud Storage

* Batch-oriented
* Real-time streaming ke liye optimized nahi

### Dataproc + Spark Streaming

* Cluster management required
* Operational overhead zyada
* Fully managed serverless nahi

### Composer + At-Least-Once Semantics

* Orchestration tool hai
* Exactly-once guarantee nahi deta

Financial use case mein at-least-once dangerous ho sakta hai (duplicate transactions).

---

## ✅ Recommended Architecture Pattern

**Cloud Pub/Sub → Cloud Dataflow (Apache Beam)**

With:

* Exactly-once processing
* Pub/Sub ordering (if required)
* Automatic scaling
* Fault tolerance

Yeh combination streaming financial data ke liye enterprise-grade solution hai.

---

## 🧠 Conceptual Lesson

### Key Principle:

Roman Urdu:
"Critical streaming systems mein exactly-once processing aur fault tolerance design ka core hissa hona chahiye."

English Terms:
Use **Cloud Pub/Sub + Cloud Dataflow + Exactly-Once Semantics + Ordering Keys** for resilient streaming pipelines.

---

### Real-World Application:

Yeh pattern use hota hai:

* Banking systems
* Payment gateways
* Fraud detection engines
* Real-time trading platforms

Agar scenario mein keywords aaye:

* Streaming data
* Financial transactions
* Low latency
* Exactly-once delivery
* Fault tolerance

Toh seedha socho:

👉 **Pub/Sub as ingestion + Dataflow as processing**

Yeh scalable, resilient aur production-ready architecture hai.
