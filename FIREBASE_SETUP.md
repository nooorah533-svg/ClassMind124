# 🔥 دليل إعداد Firebase لمبادرة تمكين الاستراتيجيات النوعية

## 🎯 نظرة عامة

Firebase هي منصة مجانية من Google توفر:
- ✅ قاعدة بيانات في الوقت الفعلي
- ✅ نظام تسجيل دخول آمن
- ✅ تخزين ورفع ملفات
- ✅ استضافة مجانية
- ✅ مجاني 100% للاستخدام التعليمي

---

## 📝 خطوات الإعداد الكاملة

### المرحلة 1: إنشاء حساب Firebase

#### الخطوة 1: التسجيل في Firebase
1. اذهبي إلى [Firebase Console](https://console.firebase.google.com)
2. سجلي دخول باستخدام حساب Google الخاص بك
3. اضغطي على **"إضافة مشروع"** (Add Project)

#### الخطوة 2: إنشاء مشروع جديد
1. **اسم المشروع:** `tamkeen-strategies` (أو أي اسم تفضلينه)
2. اضغطي **"متابعة"** (Continue)
3. **تفعيل Google Analytics:** يمكنك اختيار "لا" (غير مطلوب)
4. اضغطي **"إنشاء المشروع"** (Create Project)
5. انتظري حتى يتم إنشاء المشروع (10-30 ثانية)
6. اضغطي **"متابعة"**

---

### المرحلة 2: تفعيل خدمات Firebase

#### الخطوة 1: تفعيل Authentication (نظام الدخول)

1. من القائمة الجانبية، اضغطي على **"Authentication"**
2. اضغطي على **"البدء"** (Get Started)
3. اختاري **"Sign-in method"** من الأعلى
4. اضغطي على **"Email/Password"**
5. ✅ فعّلي **"Email/Password"** (الخيار الأول)
6. اضغطي **"حفظ"** (Save)

#### الخطوة 2: إنشاء Firestore Database (قاعدة البيانات)

1. من القائمة الجانبية، اضغطي على **"Firestore Database"**
2. اضغطي على **"إنشاء قاعدة بيانات"** (Create Database)
3. اختاري **"Start in production mode"**
4. اضغطي **"التالي"** (Next)
5. اختاري المنطقة: **"europe-west"** (الأقرب للسعودية)
6. اضغطي **"تمكين"** (Enable)

#### الخطوة 3: تكوين قواعد الأمان لـ Firestore

1. بعد إنشاء قاعدة البيانات، اضغطي على تبويب **"Rules"**
2. استبدلي القواعد الموجودة بما يلي:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // قواعد المستخدمين
    match /users/{userId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
    
    // قواعد الاستراتيجيات
    match /strategies/{strategyId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null;
      allow update, delete: if request.auth != null && 
        resource.data.userId == request.auth.uid;
    }
  }
}
```

3. اضغطي **"نشر"** (Publish)

#### الخطوة 4: تفعيل Storage (تخزين الملفات)

1. من القائمة الجانبية، اضغطي على **"Storage"**
2. اضغطي على **"البدء"** (Get Started)
3. اضغطي **"التالي"** (Next) - اقبلي القواعد الافتراضية
4. اختاري نفس المنطقة: **"europe-west"**
5. اضغطي **"تم"** (Done)

#### الخطوة 5: تكوين قواعد الأمان لـ Storage

1. اضغطي على تبويب **"Rules"**
2. استبدلي القواعد بما يلي:

```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /strategies/{userId}/{allPaths=**} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

3. اضغطي **"نشر"** (Publish)

---

### المرحلة 3: الحصول على إعدادات التكامل

#### الخطوة 1: إضافة تطبيق ويب

1. من صفحة Overview الرئيسية، اضغطي على أيقونة **</>** (Web)
2. **اسم التطبيق:** `Tamkeen Strategies Web`
3. ✅ ضعي علامة على **"Also set up Firebase Hosting"** (اختياري)
4. اضغطي **"تسجيل التطبيق"** (Register App)

#### الخطوة 2: نسخ إعدادات Firebase

سيظهر لك كود مشابه لهذا:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyB1234567890abcdefghijklmnop",
  authDomain: "tamkeen-strategies.firebaseapp.com",
  projectId: "tamkeen-strategies",
  storageBucket: "tamkeen-strategies.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef1234567890"
};
```

**⚠️ احتفظي بهذا الكود - ستحتاجينه!**

---

### المرحلة 4: ربط الموقع بـ Firebase

#### الخطوة 1: تحديث ملف index.html

1. افتحي ملف `index.html` في محرر نصوص
2. ابحثي عن السطور التالية (حوالي السطر 800):

```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT_ID.appspot.com",
    messagingSenderId: "YOUR_SENDER_ID",
    appId: "YOUR_APP_ID"
};
```

3. استبدليها بالإعدادات التي نسختها من Firebase
4. احفظي الملف

#### مثال بعد التحديث:

```javascript
const firebaseConfig = {
    apiKey: "AIzaSyB1234567890abcdefghijklmnop",
    authDomain: "tamkeen-strategies.firebaseapp.com",
    projectId: "tamkeen-strategies",
    storageBucket: "tamkeen-strategies.appspot.com",
    messagingSenderId: "123456789012",
    appId: "1:123456789012:web:abcdef1234567890"
};
```

---

### المرحلة 5: رفع الموقع على Firebase Hosting (اختياري)

بدلاً من GitHub Pages، يمكنك استخدام Firebase Hosting:

#### التثبيت والرفع:

```bash
# 1. تثبيت Firebase CLI
npm install -g firebase-tools

# 2. تسجيل الدخول
firebase login

# 3. في مجلد الموقع
firebase init hosting

# اختاري:
# - المشروع الذي أنشأته
# - المجلد العام: .
# - تطبيق صفحة واحدة: نعم
# - لا تكتبي فوق index.html

# 4. رفع الموقع
firebase deploy
```

سيعطيك رابط مثل:
`https://tamkeen-strategies.web.app`

---

## ✅ اختبار الموقع

### 1. إنشاء حساب جديد
1. افتحي الموقع
2. اضغطي **"إنشاء حساب جديد"**
3. املئي البيانات:
   - الاسم: أ. اختبار
   - البريد: test@example.com
   - المادة: رياضيات
   - كلمة المرور: 123456
4. اضغطي **"إنشاء الحساب"**

### 2. رفع استراتيجية تجريبية
1. بعد تسجيل الدخول، اضغطي **"رفع استراتيجية جديدة"**
2. املئي النموذج
3. ارفعي ملف PDF تجريبي
4. اضغطي **"رفع الاستراتيجية"**

### 3. التحقق من قاعدة البيانات
1. ارجعي إلى Firebase Console
2. افتحي **Firestore Database**
3. يجب أن تري:
   - مجموعة `users` فيها حساب المستخدم
   - مجموعة `strategies` فيها الاستراتيجية المرفوعة

### 4. التحقق من الملفات
1. افتحي **Storage**
2. يجب أن تري مجلد `strategies` فيه الملف المرفوع

---

## 📊 ميزات الموقع الجديد

### ✨ للمعلمات:
- 🔐 تسجيل دخول آمن بالبريد الإلكتروني
- 📤 رفع الاستراتيجيات مباشرة من الموقع
- 📚 عرض جميع الاستراتيجيات المرفوعة
- 📊 إحصائيات مباشرة
- 📥 تحميل ملفات الاستراتيجيات

### 🛡️ للأمان:
- كل معلمة لها حساب خاص
- الملفات محمية ويمكن الوصول إليها فقط للمسجلين
- كل معلمة يمكنها رؤية جميع الاستراتيجيات ولكن لا يمكنها حذف إلا ما رفعته

### 📈 للإدارة:
- مراقبة جميع الأنشطة من Firebase Console
- تصدير البيانات إلى Excel
- إحصائيات مباشرة
- إمكانية حذف أو تعديل أي محتوى

---

## 🔧 الإدارة المتقدمة

### إضافة مستخدم من لوحة التحكم

1. افتحي **Authentication** في Firebase
2. اضغطي **"Add User"**
3. أدخلي البريد الإلكتروني وكلمة المرور
4. ثم اذهبي إلى **Firestore Database**
5. أنشئي document جديد في مجموعة `users`:
```
Document ID: نفس UID من Authentication
Fields:
  - name: "اسم المعلمة"
  - email: "email@example.com"
  - subject: "المادة"
  - createdAt: (timestamp)
```

### تصدير البيانات

#### من Firestore:
1. افتحي **Firestore Database**
2. اضغطي على النقاط الثلاث **⋮**
3. اختاري **"Export"**

#### من Authentication:
1. افتحي **Authentication**
2. اضغطي **"Users"**
3. اضغطي **"Download CSV"**

### حذف استراتيجية

1. افتحي **Firestore Database**
2. اذهبي إلى مجموعة `strategies`
3. اختاري الوثيقة المراد حذفها
4. اضغطي على أيقونة الحذف 🗑️

**ملاحظة:** احذفي الملف أيضاً من **Storage**

---

## 📱 مميزات إضافية يمكن إضافتها

### 1. إشعارات البريد الإلكتروني
عند رفع استراتيجية جديدة، إرسال إيميل للإدارة:
- استخدمي Firebase Cloud Functions

### 2. نظام التقييم
إضافة إمكانية تقييم الاستراتيجيات بالنجوم

### 3. التعليقات
السماح للمعلمات بالتعليق على استراتيجيات بعضهن

### 4. البحث المتقدم
إضافة محرك بحث للاستراتيجيات

### 5. تطبيق جوال
تحويل الموقع إلى تطبيق أندرويد/iOS

---

## ❗ حل المشاكل الشائعة

### المشكلة 1: "Firebase is not defined"
**الحل:** تأكدي من وضع إعدادات Firebase الصحيحة

### المشكلة 2: "Permission denied"
**الحل:** تحققي من قواعد Firestore و Storage

### المشكلة 3: الملفات لا ترفع
**الحل:** تأكدي من:
- حجم الملف أقل من 10 MB
- صيغة الملف مسموحة (PDF, Word, PowerPoint)
- قواعد Storage صحيحة

### المشكلة 4: لا يمكن إنشاء حساب
**الحل:** تأكدي من تفعيل Email/Password في Authentication

---

## 🎓 موارد إضافية

- [وثائق Firebase الرسمية](https://firebase.google.com/docs)
- [دروس Firebase بالعربية](https://www.youtube.com/results?search_query=firebase+arabic+tutorial)
- [مجتمع Firebase العربي](https://stackoverflow.com/questions/tagged/firebase)

---

## 📞 الدعم الفني

إذا واجهتك أي مشكلة:

1. **تحققي من Console في المتصفح:**
   - اضغطي F12
   - افتحي تبويب "Console"
   - ابحثي عن رسائل الخطأ باللون الأحمر

2. **تحققي من Firebase Console:**
   - افتحي قسم "Usage" للتأكد من عدم تجاوز الحد المجاني

3. **اسألي في المجتمع:**
   - Stack Overflow
   - Firebase Slack Community

---

## 🎉 تهانينا!

الآن لديك موقع تعليمي متكامل مع:
- ✅ قاعدة بيانات حقيقية
- ✅ نظام دخول آمن
- ✅ رفع وتخزين ملفات
- ✅ إحصائيات مباشرة
- ✅ مجاني 100%

**بالتوفيق لمبادرة تمكين الاستراتيجيات النوعية! 🌟**

---

## 📊 الحدود المجانية لـ Firebase

**Firestore:**
- 50,000 قراءة/يوم
- 20,000 كتابة/يوم
- 1 GB تخزين

**Storage:**
- 5 GB تخزين مجاني
- 1 GB نقل/يوم

**Authentication:**
- غير محدود (مجاني)

**هذا يكفي لآلاف المستخدمين!**