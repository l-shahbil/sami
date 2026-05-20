# 🎯 SAMI QR Number System

نظام توزيع أرقام عشوائية فريدة عبر QR Code لفعاليات SAMI.

---

## 📁 هيكل المشروع

```
sami-qr/
├── images/
│   ├── logo.jpg          # شعار SAMI
│   └── favicon.ico       # أيقونة المتصفح
├── supabase/
│   ├── setup.sql         # إعداد قاعدة البيانات
│   ├── assign-number.ts  # Edge Function — تخصيص الأرقام
│   └── admin-reset.ts    # Edge Function — لوحة الإدارة
├── index.html            # صفحة المستخدم (QR Code)
├── admin.html            # لوحة تحكم الإدارة
└── README.md
```

---

## ⚙️ كيف يعمل النظام

```
المستخدم يمسح QR Code
        ↓
الصفحة تولّد UUID وتحفظه في localStorage
        ↓
ترسله إلى Supabase Edge Function
        ↓
Function تتحقق: هل هذا UUID أخذ رقم قبل؟
    ├── نعم → يعرض نفس الرقم ✅
    └── لا  → يخصص رقم جديد من 1-100 ✅
```

---

## 🚀 طريقة الإعداد

### 1. قاعدة البيانات
شغّل `supabase/setup.sql` في **Supabase → SQL Editor**

### 2. Edge Functions
ارفع الملفين في **Supabase → Edge Functions**:
- `assign-number.ts` ← يخصص رقم للمستخدم
- `admin-reset.ts` ← يتحكم في إعادة التعيين

### 3. الصفحة
في `index.html` غيّر:
```javascript
const FUNCTION_URL = 'YOUR_SUPABASE_URL/functions/v1/assign-number';
const ANON_KEY     = 'YOUR_ANON_KEY';
```

### 4. لوحة الإدارة
في `admin-reset.ts` غيّر:
```typescript
const ADMIN_PASSWORD = "YOUR_PASSWORD";
```

---

## 🔒 الأمان

| الطبقة | الوصف |
|---|---|
| RLS | يمنع الوصول المباشر للجدول |
| Edge Function | الطريق الوحيد للحصول على رقم |
| UUID | كل جهاز يحصل على نفس رقمه |
| Admin Password | لوحة الإدارة محمية بباسورد |

---

## 📊 لوحة الإدارة

افتح `admin.html` للوصول إلى:
- عدد الأرقام المستخدمة والمتبقية
- Progress bar للفعالية
- إعادة تعيين جميع الأرقام

---

## 🛠️ التقنيات المستخدمة

- **Frontend:** HTML, CSS, JavaScript
- **Backend:** Supabase Edge Functions (Deno)
- **Database:** Supabase PostgreSQL
- **Hosting:** Vercel

---

## 📌 ملاحظات

- الأرقام من **1 إلى 100**
- كل شخص يحصل على رقم **فريد وثابت** من نفس المتصفح
- لإعادة تعيين الأرقام استخدم لوحة الإدارة
