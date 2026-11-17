# Posts by Adil | Firebase Blog

এটি Posts by Adil-এর জন্য vanilla HTML, CSS ও JavaScript (ES modules) দিয়ে তৈরি responsive blog। Firebase Authentication, Firestore ও Storage ব্যবহার করে। কোনো frontend framework নেই।

## অন্তর্ভুক্ত
- Editorial-style premium homepage, pinned feature, search, category filter, sorting, load-more
- Single post ও category page
- Email/password admin login, post CRUD, image/video uploads, category CRUD
- Dark mode, mobile navigation, Bengali date format
- DOMPurify দিয়ে public post body HTML sanitize
- Firestore ও Storage security rules

## Firebase সেটআপ
1. Firebase Console-এ একটি project তৈরি করুন।
2. Authentication > Sign-in method থেকে Email/Password চালু করুন।
3. Authentication > Users থেকে নিজের অ্যাডমিন user তৈরি করুন। Email verification চালু করে ইমেইল verify করুন।
4. Firestore Database তৈরি করুন এবং Storage চালু করুন।
5. Project settings > Your apps > Web app থেকে config কপি করে `js/firebase-config.js`-এ বসান। Storage bucket-এর নাম কনসোল থেকে হুবহু কপি করুন। নতুন project-এ bucket `.firebasestorage.app` suffix-ও হতে পারে।
6. `firestore.rules` ও `storage.rules`-এ `YOUR_ADMIN_EMAIL@example.com`-এর জায়গায় verified admin email বসান। দুই জায়গায় একই email দিন।
7. Firebase Console-এ দুই rules ফাইলের বিষয়বস্তু আলাদা আলাদা Rules editor-এ paste করে Publish করুন।
8. Authentication > Settings > Authorized domains-এ আপনার GitHub Pages domain যোগ করুন।

## GitHub Pages
- সব ফাইল repository-র root-এ রাখুন।
- GitHub repository > Settings > Pages > Deploy from a branch নির্বাচন করুন।
- `main` branch এবং `/ (root)` নির্বাচন করে Save করুন।
- GitHub Pages URL HTTPS-এ খুলুন।

## প্রথমবার ব্যবহার
1. `admin.html` খুলে Firebase-এ তৈরি করা verified admin email/password দিয়ে login করুন।
2. প্রথমে “বিষয়সমূহ” ট্যাবে অন্তত একটি category তৈরি করুন।
3. “নতুন লেখা” ট্যাবে title, excerpt, category, body ও media দিয়ে save করুন।
4. Published নির্বাচন করলে public site-এ দেখা যাবে। Draft হলে শুধু admin পড়তে পারবেন।

## ডেটা মডেল
`posts`-এ title, slug, excerpt, body, coverImage, media, category, tags, pinned, order, createdAt, updatedAt, published রাখা হয়। আপলোড করা ফাইল পরে মুছতে `coverPath` ও `media[].storagePath` অতিরিক্ত ফিল্ড রাখা হয়।
`categories`-এ name, slug, description, color রাখা হয়।

## নিরাপত্তা ও সীমাবদ্ধতা
- Firebase config browser-এ প্রকাশিত থাকে; API key গোপন password নয়। আসল নিরাপত্তা Authentication ও Rules-এ।
- Public read শুধুমাত্র published posts-এ। Draft পড়া/লেখা/মুছতে verified admin email দরকার।
- Rules-এ verified email বাধ্যতামূলক। Firebase Auth-এ admin email verify করুন।
- `body` DOMPurify দিয়ে sanitize করা হয়। Admin-এ trusted HTML লিখুন; অপ্রয়োজনীয় embed/script যোগ করবেন না।
- Storage-এ সর্বোচ্চ 100 MB এবং image/video MIME type সীমা আছে। বাস্তবে আরও ছোট সীমা বেছে নেওয়া ভালো।
- Post delete করলে Storage file delete করার চেষ্টা হয়। Firebase permission বা network error হলে orphan file থেকে যেতে পারে।
- GitHub Pages static hosting। কোনো secret বা admin password HTML/JS-এ রাখবেন না।
- Firestore Rules deploy করার আগে Firebase Rules Playground-এ logged-out read, admin read/write এবং non-admin write পরীক্ষা করুন।
- Firebase Storage/Firestore usage quota ও billing settings নিজের প্রয়োজন অনুযায়ী নির্ধারণ করুন।

## ফাইল
`index.html`, `post.html`, `category.html`, `admin.html`, `css/style.css`, `js/firebase-config.js`, `js/app.js`, `js/admin.js`, `firestore.rules`, `storage.rules`।


## পোস্ট গুছিয়ে রাখার ব্যবস্থা
- ক্যাটাগরি ট্যাব থেকে নিজের প্রয়োজনমতো ক্যাটাগরি তৈরি, সম্পাদনা ও রঙ নির্ধারণ করুন।
- পোস্টে একাধিক ট্যাগ দিন। হোমপেজের সার্চ শিরোনাম, সারাংশ, ক্যাটাগরি ও ট্যাগে খোঁজে।
- পোস্টকে পিন করলে সেটি হোমপেজের নির্বাচিত অংশে এবং পোস্ট তালিকার শুরুতে থাকবে।
- `order` নম্বর দিয়ে ম্যানুয়াল অর্ডার ঠিক করুন। কম নম্বর আগে দেখাবে।
- Draft/Published দিয়ে প্রকাশনার অবস্থা নিয়ন্ত্রণ করুন।
- অ্যাডমিন পোস্ট তালিকায় শিরোনাম/ক্যাটাগরি/ট্যাগ সার্চ এবং প্রকাশিত/খসড়া/পিন করা ফিল্টার আছে।
- কোনো ক্যাটাগরিতে পোস্ট থাকলে সেটি মুছতে দেবে না। আগে পোস্টগুলো অন্য ক্যাটাগরিতে সরাতে হবে।
