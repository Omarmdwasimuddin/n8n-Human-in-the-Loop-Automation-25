# 🤖 n8n Human-in-the-Loop Automation
### ড্রাফট রিভিউ ও অনুমোদন ওয়ার্কফ্লো

---

## 📋 ওয়ার্কফ্লো ওভারভিউ

```
Webhook → OpenAI (Draft তৈরি) → Wait/Form (Human Review) → IF Check → Gmail (Approve/Reject)
```

---

## 🔷 ধাপ ১ — Webhook নোড সেটআপ

> **কাজ:** ব্রাউজার থেকে ডেটা রিসিভ করা

### পদক্ষেপ:
1. **Nodes Panel** খুলুন
2. সার্চ করুন → **`Webhook`** ক্লিক করুন
3. নিচের মতো কনফিগার করুন:

| ফিল্ড | মান |
|---|---|
| HTTP Method | `POST` |
| Path | `draft` |

4. **Test URL** কপি করুন
5. `index.html` ফাইলে URL **paste** করুন
6. `index.html` ব্রাউজারে ওপেন করুন
7. Webhook নোডে **"Listen for Test Event"** ক্লিক করুন
8. ব্রাউজারে গিয়ে ডেটা পূরণ করে **"Generate Draft"** বাটন ক্লিক করুন

---

## 🔷 ধাপ ২ — OpenAI নোড সেটআপ

> **কাজ:** Webhook থেকে পাওয়া ডেটা দিয়ে AI ড্রাফট তৈরি করা

### পদক্ষেপ:
1. Webhook নোডের **`+`** আইকনে ক্লিক করুন
2. সার্চ করুন → **`OpenAI`** ক্লিক করুন
3. **"Message a Model"** সিলেক্ট করুন
4. নিচের মতো কনফিগার করুন:

| ফিল্ড | মান |
|---|---|
| Model | *(পছন্দমতো মডেল দিন, যেমন `gpt-4o`)* |
| Role | `System` |
| Prompt | `prompt.txt` থেকে কপি করে পেস্ট করুন |

---

## 🔷 ধাপ ৩ — Wait নোড সেটআপ

> **কাজ:** মানুষকে ড্রাফট রিভিউ করার সুযোগ দেওয়া (Human-in-the-Loop)

### পদক্ষেপ:
1. OpenAI নোডের **`+`** আইকনে ক্লিক করুন
2. সার্চ করুন → **`Wait`** ক্লিক করুন
3. নিচের মতো কনফিগার করুন:

| ফিল্ড | মান |
|---|---|
| Resume | `On Form Submitted` |
| Form Title | `Request for Meeting` *(বা আপনার প্রয়োজনমতো)* |
| Form Description | `Please review this draft.` *(বা আপনার প্রয়োজনমতো)* |

4. **"Add Form Element"** ক্লিক করুন:

| ফিল্ড | মান |
|---|---|
| Field Name | Webhook নোড থেকে **`topic`** drag & drop করুন |
| Element Type | `Radio Buttons` |
| Radio Options | `Approve` এবং `Reject` |

---

## 🔷 ধাপ ৪ — IF নোড সেটআপ

> **কাজ:** Approve বা Reject চেক করা

### পদক্ষেপ:
1. Wait নোডের **`+`** আইকনে ক্লিক করুন
2. সার্চ করুন → **`IF`** ক্লিক করুন
3. **Conditions** কনফিগার করুন:

| Condition | মান |
|---|---|
| Value 1 | Wait নোড থেকে **`CS Hackathon`** (বা রিভিউ ফিল্ড) drag & drop করুন |
| Value 2 | `Approve` |

---

## 🔷 ধাপ ৫ — Gmail Approve নোড

> **কাজ:** অনুমোদিত হলে Approval ইমেইল পাঠানো

### পদক্ষেপ:
1. IF নোডের **`True`** আউটপুটের **`+`** আইকনে ক্লিক করুন
2. সার্চ করুন → **`Gmail`** ক্লিক করুন
3. **"Send a Message"** সিলেক্ট করুন
4. প্রয়োজনীয় **To, Subject, Body** মান সেট করুন
5. নোডের নাম পরিবর্তন করে **`Approve`** রাখুন

---

## 🔷 ধাপ ৬ — Gmail Reject নোড

> **কাজ:** প্রত্যাখ্যাত হলে Rejection ইমেইল পাঠানো

### পদক্ষেপ:
1. IF নোডের **`False`** আউটপুটের **`+`** আইকনে ক্লিক করুন
2. সার্চ করুন → **`Gmail`** ক্লিক করুন
3. **"Send a Message"** সিলেক্ট করুন
4. প্রয়োজনীয় **To, Subject, Body** মান সেট করুন
5. নোডের নাম পরিবর্তন করে **`Reject`** রাখুন

---

## 🗺️ সম্পূর্ণ ফ্লো চার্ট

```
┌─────────────┐
│   Webhook   │  ◄── index.html থেকে POST request
│  (POST)     │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   OpenAI    │  ◄── prompt.txt দিয়ে draft তৈরি
│  (Draft)    │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│    Wait     │  ◄── Human রিভিউ করেন (Form Submit)
│  (Form)     │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│     IF      │  ◄── Approve চেক করা হয়
│  (Check)    │
└──────┬──────┘
       │
   ┌───┴───┐
   │       │
TRUE     FALSE
   │       │
   ▼       ▼
┌──────┐ ┌──────┐
│Gmail │ │Gmail │
│Appro-│ │Rejec-│
│ ve   │ │  t   │
└──────┘ └──────┘
```

---

## ✅ কুইক চেকলিস্ট

- [ ] Webhook নোড: HTTP Method = POST, Path = `draft`
- [ ] `index.html` এ Test URL পেস্ট করা হয়েছে
- [ ] OpenAI নোডে Model ও System Prompt সেট করা হয়েছে
- [ ] Wait নোডে Form Title, Description ও Radio Buttons সেট করা হয়েছে
- [ ] IF নোডে Approve Condition সেট করা হয়েছে
- [ ] Gmail Approve নোড কানেক্ট (True branch)
- [ ] Gmail Reject নোড কানেক্ট (False branch)
- [ ] ওয়ার্কফ্লো **Activate** করা হয়েছে

---

> 💡 **টিপস:** প্রতিটি নোড সেটআপের পর **"Execute Node"** দিয়ে টেস্ট করুন। সব ঠিকঠাক হলে উপরে **"Active"** টগল চালু করুন।
