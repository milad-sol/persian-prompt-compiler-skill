# مثال ۴: دیباگ بدون حدس زدن علت

اسکیل پیام خطا و رفتار مورد انتظار را دقیق نگه می‌دارد و علت باگ را از خودش نمی‌سازد.

## ورودی

```
پرامپت کن: وقتی فرم ثبت‌نام رو تو اپ فلاسکم سابمیت می‌کنم این ارور رو میده:
sqlite3.IntegrityError: UNIQUE constraint failed: user.email
ولی ایمیلی که میزنم تکراری نیست. باید کاربر ثبت بشه.
```

## خروجی

**دسته:** debugging - خطای UNIQUE هنگام ثبت‌نام با ایمیل غیرتکراری در Flask

**پرامپت انگلیسی:**
```
Diagnose and fix a bug in my Flask app.

Actual behavior:
Submitting the registration form raises:
sqlite3.IntegrityError: UNIQUE constraint failed: user.email

Context:
The email I submit is not a duplicate.

Expected behavior:
The user is registered successfully.

Find the root cause based on the code and this evidence before changing anything, then apply a fix.
```

**ترجمه فارسی (برای بررسی):**
```
یک باگ را در اپ Flask من عیب‌یابی و رفع کن.

رفتار فعلی:
ثبت فرم ثبت‌نام این خطا را می‌دهد:
sqlite3.IntegrityError: UNIQUE constraint failed: user.email

زمینه:
ایمیلی که وارد می‌کنم تکراری نیست.

رفتار مورد انتظار:
کاربر با موفقیت ثبت شود.

قبل از هر تغییری، علت اصلی را بر اساس کد و همین شواهد پیدا کن، بعد اصلاحش کن.
```

- **اطلاعات ناقص**
  - کد مسیر ثبت‌نام و مدل `User` ضمیمه نشده؛ اگر ایجنت به کد پروژه دسترسی ندارد، باید اضافه شود.

اجراش کنم؟

## چه چیزی اضافه نشد

حدسی مثل «احتمالاً فرم دوبار ارسال می‌شود» یا «احتمالاً ایمیل خالی ذخیره می‌شود». این کار ایجنت است، نه کامپایلر.
