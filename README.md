# جام‌آوران من — سورس آماده بیلد نسخه ۲.۷

## تغییرات این نسخه
- شناسه برنامه روی `com.app.myjamavaran` تنظیم شده تا با نسخه ۲.۶ یکسان باشد.
- نسخه روی `2.7` و `versionCode` روی `207` تنظیم شده است؛ نسخه قبلی ارائه‌شده `versionCode=206` داشت.
- تنظیمات Gradle اصلاح شده‌اند.
- کلید اشتباه موجود در مخزن GitHub حذف شده است.
- فایل کلید خصوصی داخل این ZIP قرار داده نشده تا تصادفاً در مخزن عمومی منتشر نشود.
- در سورس اندروید، SDK یا کد تبلیغاتی WebIntoApp پیدا نشد. اسپلش بومی فعلی فقط پس‌زمینه و لوگوی برنامه است.

> توجه: برنامه صفحه وب `https://my.fc-jamavaran.ir/` را داخل WebView باز می‌کند. اگر خود وب‌سایت تبلیغ نشان دهد، باید جداگانه در کد/محتوای سایت بررسی شود.

## کلید امضای صحیح نسخه قبلی
فایل ZIP قبلی WebIntoApp را باز کنید و این فایل را از مسیر زیر استخراج کنید:

`android/my-release-key.jks`

این کلید با گواهی امضای APK نسخه ۲.۶ تطبیق داده شد. از `keystore.jks` قدیمی موجود در مخزن GitHub استفاده نکنید؛ گواهی آن متفاوت است.

Alias کلید قبلی:

`myjamavaran`

رمز Keystore و Private Key را از فایل `android/certification.txt` در ZIP قبلی WebIntoApp بردارید. برای فایل PKCS12، در صورت یکسان بودن رمزهای فایل، همان رمز را در هر دو فیلد وارد کنید.

## بیلد محلی با Android Studio
1. فایل `android/my-release-key.jks` را از ZIP قبلی WebIntoApp استخراج کنید.
2. آن را در پوشه اصلی همین پروژه (کنار `settings.gradle`) کپی کنید و نامش را `my-release-key.jks` بگذارید.
3. فایل `keystore.properties.example` را به `keystore.properties` کپی کنید.
4. مقادیر رمز را در `keystore.properties` پر کنید. این فایل و فایل `.jks` نباید به GitHub پوش شوند.
5. در Android Studio از Gradle اجرا کنید: `:app:assembleRelease`.

## بیلد امن در GitHub Actions
برای آنکه کلید خصوصی وارد مخزن عمومی نشود، در GitHub بروید به:
`Settings → Secrets and variables → Actions → New repository secret`

این چهار Secret را بسازید:

- `RELEASE_KEYSTORE_BASE64`
- `RELEASE_STORE_PASSWORD`
- `RELEASE_KEY_ALIAS`
- `RELEASE_KEY_PASSWORD`

مقدارها:
- `RELEASE_KEYSTORE_BASE64`: محتوای Base64 فایل `my-release-key.jks`
- `RELEASE_STORE_PASSWORD`: رمز Keystore از `certification.txt`
- `RELEASE_KEY_ALIAS`: مقدار `myjamavaran`
- `RELEASE_KEY_PASSWORD`: رمز کلید خصوصی از `certification.txt`

در Windows PowerShell برای ساخت مقدار Base64:
```powershell
[Convert]::ToBase64String([IO.File]::ReadAllBytes("my-release-key.jks"))
```

در Linux:
```bash
base64 -w 0 my-release-key.jks
```

بعد از تنظیم Secretها، پروژه را به شاخه `main` یا `master` پوش کنید یا از مسیر `Actions`، ورک‌فلو را دستی اجرا کنید. خروجی APK و AAB در بخش Artifacts قرار می‌گیرد.

## نکته انتشار
پیش از انتشار، مطمئن شوید نسخه منتشرشده در مایکت هنوز `versionCode=206` است یا خیر. اگر مایکت نسخه‌ای با کد بالاتر دارد، `versionCode` باید از آن بیشتر شود. Package Name باید دقیقاً `com.app.myjamavaran` باقی بماند.
