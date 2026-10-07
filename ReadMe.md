# 📋 Tender Management System | نظام إدارة المناقصات

An Android app (Java + SQLite) that manages the full tender lifecycle: **create a tender → collect company bids → compare them with a weighted score → award → contract.**

تطبيق أندرويد يدير دورة حياة المناقصة كاملة: إنشاء المناقصة، استقبال عروض الشركات، مقارنتها بمعادلة وزنية، الترسية، ثم العقد.

![Platform](https://img.shields.io/badge/platform-Android-3DDC84?logo=android&logoColor=white)
![Language](https://img.shields.io/badge/language-Java-ED8B00?logo=openjdk&logoColor=white)
![Database](https://img.shields.io/badge/database-SQLite-003B57?logo=sqlite&logoColor=white)
![Status](https://img.shields.io/badge/status-in%20development-7C3AED)

---

## 🎯 Problem

A company that receives bids from several IT vendors usually compares them by hand. That is slow and subjective, and it is easy to overlook a better offer.

This app keeps tenders, companies and bids in one place and ranks the bids with a **transparent formula**, so the best offer is chosen on numbers and not on impressions.

## ✨ Features

| Feature | Status |
|---|---|
| Login screen | ✅ |
| Dashboard with navigation to all modules | ✅ |
| Create tenders (title, department, budget, dates, requirements) stored in **SQLite** | ✅ |
| Tenders list (RecyclerView cards) and tender details | ✅ |
| Add bids (company, amount, duration, notes) | ✅ |
| Bid details and **bid comparison with weighted score** | ✅ (sample data) |
| Companies: add, list, details | 🔄 Intent-based, database next |
| Evaluation → Award flow | ✅ UI and navigation |
| Contracts, Reports, Settings screens | 🔄 UI done, logic in progress |
| SQLite for companies, bids, evaluations, awards, contracts, users | 🔜 |

## 🧮 Bid scoring

Each bid gets a score from 0 to 100:

```
priceScore = (lowestPrice / bidAmount) × 100
total      = 0.5 × priceScore + 0.3 × quality + 0.2 × experience
```

Example with three bids (quality and experience are rated 0–100):

| Company | Amount | Quality | Experience | Total |
|---|---|---|---|---|
| Future Co. (شركة المستقبل) | 45,000 | 85 | 90 | **89.06** 🏆 |
| Tech Solutions | 41,000 | 70 | 75 | 86.31 |
| Horizon Computing | 48,000 | 92 | 80 | 86.00 |

The logic lives in its own class (`BidEvaluator`), separate from the UI, so it is easy to test and to change the weights.

## 🗺️ App flow

```
Login → Dashboard ─┬─ Tenders ─┬─ Create Tender
                   │           └─ Tender Details → Bids → Bid Details
                   │                                  └→ Compare Bids → Evaluation → Award → Contracts
                   ├─ Companies ─┬─ Add Company
                   │             └─ Company Details
                   ├─ Reports
                   └─ Settings
```

## 🛠️ Tech stack

- **Language:** Java 11
- **UI:** XML layouts, RTL Arabic interface, dark theme with a purple accent, vector drawable backgrounds and `LayoutAnimation`
- **Navigation:** `Intent` with `putExtra` / `getExtra`
- **Lists:** `RecyclerView` with a custom `Adapter` and `ViewHolder`
- **Storage:** SQLite via `SQLiteOpenHelper`
- **SDK:** minSdk 24, targetSdk 37

## 📁 Project structure

```
app/src/main/java/com/example/tendermanagementsystem/
├── Database/
│   ├── DatabaseHelper.java      # SQLite schema + CRUD
│   ├── TenderModel.java         # Tender data model
│   └── Database_Adapter.java    # RecyclerView adapter for tenders
├── activity_login.java
├── activity_dashboard.java
├── activity_tenders.java / activity_create_tender.java / activity_tender_details.java
├── activity_companies.java / AddCompany.java / activity_company_details.java
├── activity_bids.java / AddBid.java / activity_bid_details.java
├── activity_compare_bids.java / BidEvaluator.java / Bid.java
├── activity_evaluation.java / activity_award.java
└── activity_contracts.java / activity_reports.java / activity_settings.java
```

## 📸 Screenshots

<!-- Add screenshots to a /screenshots folder, then uncomment: -->
<!--
| Login | Dashboard | Tenders |
|---|---|---|
| <img src="screenshots/login.png" width="200"> | <img src="screenshots/dashboard.png" width="200"> | <img src="screenshots/tenders.png" width="200"> |
-->

## 🚀 Getting started

1. Clone the repository:
   ```bash
   git clone https://github.com/<your-username>/TenderManagementSystem.git
   ```
2. Open it in **Android Studio** and wait for Gradle sync.
3. Run on an emulator or a device with Android 7.0 (API 24) or higher.

No API keys or external services are needed. The database is created locally on first launch.

## 🧠 What I practised

- Designing a multi-screen app and passing data safely between screens
- SQLite CRUD, `Cursor` and `ContentValues`
- `RecyclerView`, custom adapters, click handling and refreshing in `onResume()`
- Input validation before saving or parsing numbers
- A weighted decision model separated from the UI
- Custom backgrounds and animations in XML only

## 🗓️ Roadmap

- [ ] SQLite tables and CRUD for companies, bids, evaluations, awards, contracts, users
- [ ] Persist bids and run the comparison on real stored data
- [ ] Contracts generated from an award
- [ ] Search and filter for tenders
- [ ] Reports with real statistics
- [ ] Real login (hashed passwords)
- [ ] Unit tests for `BidEvaluator`

## 👤 Author

**Your Name** · University student, Java and Android developer
GitHub: [@your-username](https://github.com/your-username)

---
---

## 🇸🇦 نبذة بالعربي

**نظام إدارة المناقصات** تطبيق أندرويد مكتوب بلغة Java مع SQLite، هدفه مساعدة صاحب الشركة على اختيار أفضل عرض من عروض الموردين التقنيين.

**الفكرة:** بدل ما تقارن العروض يدوياً، التطبيق يسجّل المناقصات والشركات والعروض، ثم يرتّب العروض بمعادلة وزنية واضحة (السعر 50%، الجودة 30%، الخبرة 20%) ويعرض الأفضل، ثم تنتقل بالترسية إلى العقد.

**الواجهة:** عربية RTL بتصميم داكن حديث ولون بنفسجي، وكل شاشة لها خلفية ورسمة خاصة بها.

**الحالة الحالية:** الشاشات والتنقل جاهزة، ومناقصات المشروع مخزّنة في SQLite، والمقارنة تعمل على بيانات تجريبية. بقية الجداول (الشركات، العروض، التقييم، الترسية، العقود) هي المرحلة القادمة.

**الهدف من المشروع:** تدريب عملي على Java و SQLite و Intent و RecyclerView والتعامل مع العلاقات بين الجداول، ومشروع متكامل للـ CV.
