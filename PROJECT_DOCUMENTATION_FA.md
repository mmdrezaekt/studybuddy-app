# مستند جامع پروژه StudyBuddy (نسخه فارسی)

## معرفی کلی
StudyBuddy یک پلتفرم هوشمند برنامه‌ریزی مطالعه با قابلیت همکاری گروهی است که با React + TypeScript در فرانت‌اند و Firebase (Authentication, Firestore, Cloud Functions, Hosting, Messaging, Storage) در بک‌اند پیاده‌سازی شده است. رابط کاربری واکنش‌گرا، نوتیفیکیشن‌های هوشمند (ایمیل/پوش)، یادآورها، دعوت‌نامه‌ها و مدیریت اعضا از امکانات کلیدی پروژه هستند.

## معماری سیستم
- **Frontend (React 19 + TS)**: نقطه ورود `src/index.tsx` و اپلیکیشن اصلی در `src/App.tsx`.
- **Routing**: با `react-router-dom` در `App.tsx` مسیرهای `/`, `/study-plan/:id`, `/invite/:id` پیاده شده‌اند.
- **Firebase Config**: در `src/firebase/config.ts` سرویس‌های `auth`, `db`, `storage`, `functions`, `analytics` مقداردهی می‌شوند.
- **Cloud Functions**: در `functions/src/index.ts` شامل زمان‌بندی یادآورها، ارسال ایمیل/پوش و توابع callable.
- **Firestore Security Rules**: در `firestore.rules` سیاست‌های دسترسی سطح داکیومنت تعریف شده‌اند.
- **Services**: منطق نوتیفیکیشن (`src/services/notificationService.ts`) و ریمایندر (`src/services/reminderService.ts`).
- **Types**: مدل‌های داده در `src/types/index.ts`.

## جریان اجرای اپلیکیشن
1. `index.tsx` اپ را mount می‌کند.
2. `App.tsx` وضعیت احراز هویت را با `onAuthStateChanged` می‌شنود. تا زمان تعیین هویت، صفحه Loading نمایش داده می‌شود.
3. در حالت لاگین: `Dashboard` رندر می‌شود؛ در غیر این صورت `AuthWrapper` (لاگین/ثبت‌نام).
4. مسیر `study-plan/:id` جزئیات یک پلن را برای کاربران مجاز نشان می‌دهد. مسیر `invite/:id` برای پذیرش دعوت است.

## احراز هویت (Authentication)
- در `src/firebase/config.ts`، `auth` مقداردهی می‌شود.
- `App.tsx` از `onAuthStateChanged` برای ذخیره `User` استفاده می‌کند.
- ساختار `User` در `src/types/index.ts` تعریف شده است.

## مدل‌های داده اصلی (Types)
- `User`: شناسه، ایمیل، نام نمایشی، تصویر و زمان ایجاد.
- `StudyPlan`: عنوان، توضیح، موضوع، تاریخ سررسید، سازنده، گروهی/انفرادی، لیست شرکت‌کنندگان، پیشرفت، لیست تسک‌ها.
- `Task`: عنوان، توضیح اختیاری، وضعیت تکمیل، زمان‌های ایجاد/تکمیل، تخصیص.
- `Note`: نوت‌های مرتبط با پلن، با امکان فایل.
- `AppNotification`: پیام‌های درون‌برنامه‌ای با وضعیت خوانده‌شده و نوع.
- `Invitation` و `MemberRequest`: فرآیند دعوت و عضویت.

## داشبورد و مدیریت پلن‌ها
- `components/Dashboard/Dashboard.tsx`: بارگذاری پلن‌ها برای کاربر لاگین‌شده با `onSnapshot` و فیلتر `participants`.
- `CreateStudyPlan.tsx`: ایجاد پلن جدید با `addDoc(collection(db,'studyPlans'), data)`؛ به‌صورت گروهی/انفرادی، افزودن شرکت‌کنندگان و تنظیمات اولیه.
- `StudyPlanList.tsx` و `StudyPlanCard.tsx`: نمایش کارت‌های پلن با درصد پیشرفت (محاسبه بر اساس وضعیت تسک‌ها) و ناوبری به جزئیات.
- `ProgressOverview.tsx`: خلاصه پیشرفت تمام پلن‌ها.

## جزئیات پلن مطالعه
- `components/StudyPlan/StudyPlanDetail.tsx`: مدیریت تسک‌ها، نوت‌ها، اعضا، و پیشرفت. تعاملات CRUD با Firestore (اضافه/ویرایش/حذف تسک و نوت). محاسبه پیشرفت بر مبنای تسک‌های تکمیل‌شده.

## همکاری و دعوت‌نامه‌ها
- `InviteMembers.tsx` و `AddMember.tsx`: تولید دعوت‌نامه و اضافه کردن اعضا.
- `RedeemInvitation.tsx`: مصرف توکن دعوت و افزودن کاربر به `participants` پلن.
- Cloud Function `sendInvitationEmail`: ارسال ایمیل دعوت و ثبت نوتیفیکیشن برای دعوت‌کننده.

## نوتیفیکیشن‌ها و یادآورها
- فرانت‌اند:
  - `NotificationBell.tsx`: شمارش خوانده‌نشده‌ها با `onSnapshot` از کالکشن `notifications`.
  - `NotificationCenter.tsx`: لیست و `markAsRead`/`markAllAsRead`.
  - `NotificationSettings.tsx`: خواندن/ذخیره ترجیحات نوتیفیکیشن کاربر در داکیومنت `users/{userId}`.
- سرویس نوتیفیکیشن (`src/services/notificationService.ts`):
  - درخواست مجوز، دریافت FCM Token، ارسال به سرور با تابع callable `updateFCMToken`، شنود پیام‌های foreground، ساخت نوتیفیکیشن محلی و ایجاد رکورد درون‌برنامه‌ای.
- سرویس یادآور (`src/services/reminderService.ts`):
  - ساخت Reminder در کالکشن `reminders`، واکشی/شنود ریمایندرها، منطق تشخیص نیاز به ریمایندر بر اساس سررسید/پیشرفت، ساخت خودکار ریمایندر و ریمایندر تسک.
- Cloud Functions (`functions/src/index.ts`):
  - `checkUpcomingDeadlines` (روزانه ۹ صبح Asia/Tehran): ایمیل، پوش، و ایجاد رکورد نوتیفیکیشن برای پلن‌هایی با سررسید نزدیک.
  - `checkIncompletePlans` (روزانه ۶ عصر): ایمیل/پوش/نوتیف در مورد پلن‌های ناتمام.
  - `updateFCMToken` (callable): ذخیره توکن کاربر در `users/{uid}`.
  - `sendInvitationEmail` و `sendMemberAddedEmail`: ارسال ایمیل و ثبت نوتیفیکیشن.

## قوانین امنیتی Firestore
- `users/{userId}`: فقط صاحب داکیومنت می‌تواند بخواند/بنویسد.
- `studyPlans/{planId}`: خواندن برای مشارکت‌کنندگان؛ ایجاد توسط مالک که در `participants` هم باشد؛ بروزرسانی توسط مالک یا هر مشارکت‌کننده؛ حذف فقط توسط مالک.
- `notes/{noteId}` و `reminders/{reminderId}`: فقط مشارکت‌کنندگان پلن مربوطه.
- `notifications/{notificationId}`: فقط صاحب `userId`.
- `invitations/{invitationId}`: خواندن توسط دعوت‌کننده یا گیرنده دعوت (بر اساس ایمیل)، ایجاد/بروزرسانی توسط دعوت‌کننده یا گیرنده.

## پیکربندی Firebase
- فایل `src/firebase/config.ts` شامل `apiKey`, `authDomain`, `projectId`, ... و مقداردهی سرویس‌ها است.
- Messaging: نیازمند `vapidKey` در فرانت‌اند و Service Worker `public/firebase-messaging-sw.js` (موجود در پروژه) برای پوش نوتیفیکیشن.
- Functions: نیازمند تنظیم `EMAIL_USER` و `EMAIL_PASS` در محیط دیپلوی.

## تست‌ها
- راهنما: `TESTING_GUIDE.md` با پوشش Unit/Integration/Rules/E2E/Responsive و دستورات اجرا.
- Jest + React Testing Library برای یونیت و اینتگریشن؛ Cypress برای E2E.
- نمونه‌ها در `src/__tests__/` و `cypress/e2e/` موجود است.

## تجربه کاربری و UI
- TailwindCSS برای استایل و طراحی واکنش‌گرا.
- مؤلفه‌های کارت، لیست، مودال، و فرم‌ها با الگوهای دسترس‌پذیری.

## دیپلوی و اجرا
- توسعه: `npm start` در روت `studybuddy/`.
- بیلد: `npm run build` و میزبانی روی Firebase Hosting.
- دیپلوی فانکشن‌ها: داخل پوشه `functions/` → `npm i` → `firebase deploy --only functions`.

## پرسش‌های پرتکرار (FAQ)
- تفاوت نوتیفیکیشن‌های ایمیلی و پوش؟ ایمیل‌ها از Cloud Functions با `nodemailer` و SMTP ارسال می‌شوند؛ پوش با FCM و نیازمند مجوز مرورگر است.
- بروزرسانی درصد پیشرفت چگونه است؟ بر اساس نسبت تسک‌های `completed` به کل تسک‌ها در هر پلن.
- کنترل دسترسی چگونه تضمین می‌شود؟ هم در UI (فیلتر و شرط) و هم در Firestore Rules.
- اگر کاربر پوش نوتیف را رد کند؟ اپ کار می‌کند اما پوش ارسال نمی‌شود؛ می‌توان صرفاً نوتیف درون‌برنامه‌ای/ایمیل داشت.

## مسیرهای کلیدی کد برای پاسخ به سوالات
- ورود اپ: `src/index.tsx`, `src/App.tsx`
- احراز هویت و کاربر: `src/firebase/config.ts`, `src/types/index.ts`
- داشبورد و فیچرها: `src/components/Dashboard/*`
- جزئیات پلن: `src/components/StudyPlan/*`
- نوتیفیکیشن‌ها: `src/components/Notifications/*`, `src/services/notificationService.ts`
- یادآورها: `src/services/reminderService.ts`
- توابع سروری: `functions/src/index.ts`
- قوانین امنیتی: `firestore.rules`
- تست‌ها: `TESTING_GUIDE.md`, `src/__tests__`, `cypress/e2e`

## نکات نگهداری و توسعه
- تایپ‌اسکریپت را برای مدل‌های مشترک یکپارچه نگه دارید.
- قبل از تغییر در ساختار داده، قوانین امنیتی و تست‌ها را به‌روز کنید.
- خطاهای async را لاگ و در UI مدیریت کنید؛ از `onSnapshot` برای realtime استفاده کنید.

---

## ساختار دیتابیس (Firestore Collections)
- `users/{userId}`:
  - فیلدهای نمونه: `email`, `displayName`, `photoURL`, `major`, `fcmToken`, `notificationPreferences`, `updatedAt`.
  - استفاده: تنظیمات کاربر، توکن پوش، ترجیحات نوتیفیکیشن.
- `studyPlans/{planId}`:
  - فیلدهای نمونه: `title`, `description`, `subject`, `dueDate (Timestamp)`, `createdAt`, `updatedAt`, `ownerId`, `isGroup`, `participants (string[])`, `progress (0..100)`, `tasks (Task[])`.
  - ایندکس‌های پیشنهادی: روی `ownerId`, و در صورت کوئری‌های ترکیبی، ایندکس مرکب برای فیلترهای پرتکرار.
- `notes/{noteId}`:
  - فیلدها: `title`, `content`, `planId`, `uploadedBy`, `fileUrl`, `fileName`, `fileType`, `createdAt`.
  - استفاده: نوت‌های مرتبط با پلن.
- `notifications/{notificationId}`:
  - فیلدها: `userId`, `title`, `message`, `type`, `read`, `createdAt`, `studyPlanId?`, `invitationId?`.
  - استفاده: اینباکس نوتیفیکیشن درون‌برنامه‌ای و شمارش خوانده‌نشده‌ها.
- `reminders/{reminderId}`:
  - فیلدها: `studyPlanId`, `studyPlanTitle`, `type ('deadline'|'task'|'progress')`, `message`, `scheduledFor`, `isActive`, `createdAt`.
  - استفاده: زمان‌بندی و مشاهده یادآورها.
- `invitations/{invitationId}`:
  - فیلدها: `studyPlanId`, `studyPlanTitle`, `inviterId`, `inviterName`, `inviterEmail`, `inviteeEmail?`, `token`, `expiresAt`, `used`, `usedAt?`, `usedBy?`, `createdAt`.

نکته: در کلاینت، `Date` استفاده می‌شود اما در Firestore بهتر است `Timestamp` ذخیره شود و هنگام خواندن به `Date` تبدیل گردد.

---

## فلودیاگرام سناریوهای کلیدی (توصیفی)
- ایجاد پلن:
  1) کاربر فرم `CreateStudyPlan` را پر می‌کند → 2) اعتبارسنجی ساده سمت کلاینت → 3) `addDoc('studyPlans')` → 4) نوتیف موفقیت/خطا → 5) نمایش در `Dashboard` با `onSnapshot`.
- به‌روزرسانی پیشرفت:
  1) کاربر تسک را Complete می‌کند → 2) محاسبه درصد کامل‌شده‌ها → 3) `updateDoc` پلن → 4) UI و ProgressOverview به‌روز می‌شود.
- دعوت عضو جدید:
  1) دعوت‌کننده ایمیل دعوت را وارد می‌کند → 2) فراخوانی Cloud Function `sendInvitationEmail` → 3) ایمیل ارسال و رکورد نوتیف برای دعوت‌کننده ثبت می‌شود → 4) گیرنده با لینک `/invite/:id` وارد می‌شود → 5) تأیید و افزودن به `participants`.
- یادآورهای زمان‌بندی‌شده:
  1) `checkUpcomingDeadlines` هر روز ساعت ۹ → 2) پلن‌های با سررسید ۱ تا ۲ روز آینده → 3) ایمیل/پوش/رکورد نوتیف برای شرکت‌کنندگان → 4) کاربر در NotificationCenter می‌بیند.

---

## جزئیات Cloud Functions
- Runtime: Node.js 18 (`firebase.json`).
- Initialization: `admin.initializeApp()` و SMTP از طریق `nodemailer`.
- توابع زمان‌بندی‌شده (Pub/Sub):
  - `checkUpcomingDeadlines` (روزانه ۰۹:۰۰ Asia/Tehran): جستجوی `studyPlans` با بازه سررسید، ارسال ایمیل/پوش، ثبت نوتیف.
  - `checkIncompletePlans` (روزانه ۱۸:۰۰): بررسی `progress < 100` و اطلاع‌رسانی.
- توابع Callable:
  - `updateFCMToken({ fcmToken })`: ذخیره توکن در `users/{uid}`.
  - `sendInvitationEmail({ inviteeEmail, studyPlanTitle, inviterName, invitationUrl })`.
  - `sendMemberAddedEmail({ memberEmail, studyPlanTitle, inviterName, studyPlanUrl })`.
- ملاحظات محیطی: نیازمند `EMAIL_USER`, `EMAIL_PASS` در env پروژه Functions.

نمونه دیپلوی:
```
cd functions
npm i
firebase deploy --only functions
```

---

## راه‌اندازی پوش‌نوتیفیکیشن (FCM + SW)
- Service Worker: فایل `public/firebase-messaging-sw.js` حضور دارد. اطمینان حاصل کنید کانفیگ FCM صحیح باشد و دامنه میزبانی HTTPS داشته باشد.
- کلاینت:
  - `requestNotificationPermission()` → گرفتن مجوز.
  - `getFCMToken(vapidKey)` → دریافت توکن.
  - `updateFCMToken` (callable) → ذخیره توکن سمت سرور.
  - `onForegroundMessage` → نمایش نوتیف محلی در حالت فورگراند.
- پیش‌نیاز: تنظیم `vapidKey` معتبر و فعال‌سازی Messaging در کنسول Firebase.

---

## PWA و تجربه موبایل
- Manifest در `public/manifest.json` و آیکون‌ها (`logo192.png`, `logo512.png`).
- SW اختصاصی FCM برای پوش نوتیف. در صورت نیاز به PWA کامل، SW سفارشی برای کش دارایی‌ها اضافه کنید.
- نکات UX: لمس‌پذیری مناسب، فونت خوانا، اندازه هدف‌های لمسی.

---

## متغیرهای محیطی پیشنهادی
Frontend (`.env`):
```
REACT_APP_FIREBASE_API_KEY=...
REACT_APP_FIREBASE_AUTH_DOMAIN=...
REACT_APP_FIREBASE_PROJECT_ID=...
REACT_APP_FIREBASE_STORAGE_BUCKET=...
REACT_APP_FIREBASE_MESSAGING_SENDER_ID=...
REACT_APP_FIREBASE_APP_ID=...
REACT_APP_FIREBASE_MEASUREMENT_ID=...
REACT_APP_FCM_VAPID_KEY=...
```
Cloud Functions (`.env` یا config):
```
EMAIL_USER=your-email@gmail.com
EMAIL_PASS=your-app-password
```

---

## امنیت و بهترین‌عمل‌ها
- اعمال منطق دسترسی هم در UI و هم در `firestore.rules`.
- اعتبارسنجی ورودی‌ها در فرم‌ها (طول، فرمت ایمیل، تاریخ معتبر).
- ذخیره تاریخ‌ها به‌صورت `Timestamp` برای سازگاری با کوئری‌ها/منطقه زمانی.
- اجتناب از قرار دادن کلیدها در سورس‌کد پابلیک؛ استفاده از `.env` و محیط دیپلوی.
- مدیریت خطاها: پیام کاربرپسند در UI، لاگ کافی در کنسول/سرور.

---

## کارایی و مقیاس‌پذیری
- استفاده از `onSnapshot` فقط در صفحات لازم؛ هنگام خروج از صفحه unsubscribe کنید.
- صفحه‌بندی/محدودسازی کوئری‌ها در لیست‌های بزرگ (`limit`, `orderBy`).
- ساخت ایندکس‌های ترکیبی برای کوئری‌های مرکب پرتکرار.
- کاهش اندازه باندل با lazy-loading صفحات سنگین.

---

## استقرار و میزبانی
- Hosting پیکربندی: `firebase.json` → سرو مسیرها به `index.html` برای SPA.
- بیلد: `npm run build` → فولدر `build/`.
- دیپلوی سراسری:
```
firebase deploy
```

---

## عیب‌یابی متداول
- پوش نوتیف نمی‌رسد:
  - HTTPS، مجوز مرورگر، `vapidKey` معتبر، درست بودن SW و دسترسی به FCM را بررسی کنید.
- خطای دسترسی Firestore:
  - `firestore.rules` و حضور کاربر در `participants` پلن را بررسی کنید.
- ایمیل‌ها ارسال نمی‌شوند:
  - `EMAIL_USER/PASS`، محدودیت‌های SMTP، لاگ Cloud Functions.
- کوئری کند است:
  - ایندکس‌ها، فیلترها، و تبدیل `Timestamp`/`Date` را بررسی کنید.

---

## سوالات پرتکرار توسعه‌دهندگان
- چطور نوتیف خوانده‌نشده را بشمارم؟
  - کوئری روی `notifications` با `where('userId','==',uid)` و `where('read','==',false)` و `onSnapshot`.
- چطور ریمایندر خودکار بسازم؟
  - از `createAutomaticReminders(studyPlan)` در سرویس ریمایندر بعد از ایجاد/به‌روزرسانی پلن استفاده کنید.
- چطور توکن FCM را همگام کنم؟
  - پس از `getFCMToken`، تابع callable `updateFCMToken` را صدا بزنید تا در `users/{uid}` ذخیره شود.
- اعتبارسنجی دعوت‌نامه کجاست؟
  - سمت کلاینت در `RedeemInvitation` (توکن/انقضا) و دسترسی نهایی با Rules کنترل می‌شود. ایمیل دعوت با Cloud Function ارسال می‌شود.

---

## منابع تکمیلی
- `README.md` برای شروع سریع و دستورات مرسوم.
- `TESTING_GUIDE.md` برای استراتژی کامل تست.
- فایل‌های کلیدی ارجاع‌شده:
  - `src/App.tsx`, `src/index.tsx`
  - `src/firebase/config.ts`
  - `src/components/**`
  - `src/services/notificationService.ts`, `src/services/reminderService.ts`
  - `functions/src/index.ts`
  - `firestore.rules`

---

## راهنمای جزئی کد برای هر فایل کلیدی

در این بخش، نقش هر فایل، وابستگی‌ها، جریان داده و توابع/هوک‌های مهم توضیح داده می‌شود تا هنگام ارائه بتوانید به سؤالات دقیق پاسخ دهید.

### src/index.tsx
- نقش: نقطه ورود React. ریشه DOM را با `ReactDOM.createRoot` می‌سازد و `App` را داخل `StrictMode` رندر می‌کند. در انتها `reportWebVitals` را فراخوانی می‌کند (اختیاری).
- وابستگی‌ها: `./App`, `./index.css`, `reportWebVitals`.
- نکته ارائه: هر تغییری در Contextهای سراسری/Providers باید این‌جا دور `App` تزریق شود.

### src/App.tsx
- نقش: مدیریت احراز هویت سطح اپ، روتینگ و اسکلت صفحات.
- وابستگی‌ها: `react-router-dom` برای `Router/Routes/Route/Navigate`، `firebase/auth` برای `onAuthStateChanged`، کانفیگ `auth`، تایپ `User`، صفحات `AuthWrapper`, `Dashboard`, `StudyPlanDetail`, `RedeemInvitation`.
- وضعیت‌ها: `user: User|null`, `loading: boolean`.
- اثر اصلی: `useEffect` برای subscribe به تغییرات auth و ساخت مدل `User` (با `displayName`, `photoURL`, `createdAt`). در cleanup unsubscribe می‌کند.
- UI:
  - هنگام `loading=true`: صفحه Loading با spinner.
  - روت `/`: اگر `user` موجود → `Dashboard`، وگرنه `AuthWrapper`.
  - روت `/study-plan/:id`: فقط در حالت لاگین؛ در غیر این صورت `Navigate` به `/`.
  - روت `/invite/:id`: رندر `RedeemInvitationWrapper`.
  - روت fallback: `Navigate` به `/`.
- نکته ارائه: جداسازی مسئولیت auth از صفحات، جلوگیری از نمایش صفحات حساس بدون احراز.

### src/firebase/config.ts
- نقش: مقداردهی Firebase App و سرویس‌ها (`auth`, `db`, `storage`, `functions`, `analytics`).
- نکات امنیتی: مقادیر کانفیگ کلاینتی public هستند؛ با این حال، دسترسی واقعی با Rules محدود می‌شود. برای کلیدهای حساس مثل SMTP از سرور/Functions استفاده کنید.
- خروجی‌ها: `auth`, `db`, `storage`, `functions`, و `analytics`.

### src/types/index.ts
- نقش: مرجع تایپ‌های سراسری شامل `User`, `StudyPlan`, `Task`, `Note`, `Badge`, `AppNotification`, `Invitation`, `MemberRequest`.
- نکته اجرایی: هم‌ترازی فیلدها با ساختار مستندات Firestore و اطمینان از تبدیل `Timestamp` ⇄ `Date` در لایه سرویس/UI.

### components/Auth/AuthWrapper.tsx
- نقش: سوئیچ بین فرم‌های ورود (`Login`) و ثبت‌نام (`Register`) با وضعیت محلی `isLogin`.
- نکته ارائه: Wrapper سبک برای ساده‌سازی روت `/` در حالتی که کاربر لاگین نیست.

### components/Dashboard/Dashboard.tsx
- نقش: صفحه اصلی پس از لاگین؛ تب‌ها: لیست پلن‌ها، ساخت پلن، پروفایل کاربر، آیکن زنگ نوتیف، تنظیمات نوتیف.
- وابستگی‌ها: `auth`, `db`، شنود auth با `onAuthStateChanged`، شنود پلن‌ها با `onSnapshot` و کوئری روی `studyPlans` جایی که `participants` شامل `uid` است.
- وضعیت‌ها: `user`, `studyPlans[]`, `loading`, `activeTab`, `showNotificationSettings`.
- UI: در تب `plans`، `ProgressOverview` و سپس `StudyPlanList`; در تب `create`, `CreateStudyPlan`; در تب `profile`, `UserProfile`.
- نکته ارائه: استفاده از کوئری realtime برای همگام‌سازی لحظه‌ای کارت‌ها.

### components/Dashboard/CreateStudyPlan.tsx
- نقش: فرم ساخت پلن جدید با فیلدهای `title/description/subject/dueDate/isGroup/participants`.
- تابع `handleSubmit`: می‌سازد `studyPlanData` (بدون `id`) و `addDoc(collection(db,'studyPlans'), data)` را فراخوانی می‌کند. پس از موفقیت فرم ریست و پیام موفقیت.
- نکته داده‌ای: `dueDate` به `Date` تبدیل می‌شود؛ در لایه ذخیره‌سازی Firestore بهتر است `Timestamp` شود (Firestore خودش تبدیل می‌کند اگر از SDK تاریخ داده شود).

### components/Dashboard/StudyPlanList.tsx و StudyPlanCard.tsx
- List: در نبود داده پیام خالی، در حضور داده grid کارت‌ها.
- Card: ناوبری با `useNavigate` به `/study-plan/:id`، نمایش `title/subject/description`، برچسب `Group` در حالت گروهی، محاسبه درصد پیشرفت بر اساس تعداد تسک‌های Completed نسبت به کل.

### components/Dashboard/ProgressOverview.tsx
- نقش: خلاصه آماری پیشرفت چندپلنی؛ ورودی `studyPlans[]`. می‌تواند میانگین پیشرفت و شمار تکمیل‌ها را نشان دهد.

### components/StudyPlan/StudyPlanDetail.tsx
- نقش: صفحه جزئیات یک پلن (تسک‌ها، نوت‌ها، اعضا، درصد پیشرفت). CRUD تسک/نوت، محاسبه پیشرفت و بروزرسانی.
- نکته ارائه: هر تغییر تسک باید به‌روزرسانی پیشرفت پلن را نیز فعال کند تا UI همگام بماند.

### components/Notifications/NotificationBell.tsx
- نقش: نمایش شمارش نوتیف‌های خوانده‌نشده با کوئری `where('userId','==',uid)` و `where('read','==',false)` و `onSnapshot`.
- تعامل: کلیک → باز کردن `NotificationCenter` در مودال.

### components/Notifications/NotificationCenter.tsx
- نقش: لیست نوتیف‌های کاربر با ترتیب `createdAt desc`، عملیات `markAsRead` برای یک آیتم و `markAllAsRead` برای همه با `updateDoc`.
- نکته ارائه: در سمت کلاینت تنها صاحب `userId` قادر به خواندن/نوشتن است و Rules نیز enforce می‌کند.

### components/Notifications/NotificationSettings.tsx
- نقش: بارگذاری/ذخیره ترجیحات نوتیفیکیشن از/به داکیومنت `users/{userId}`.
- وضعیت‌ها: `preferences`, `loading`, `saving`, `pushPermissionGranted`.
- توابع: `loadPreferences`, `checkPushPermission`, `handlePreferenceChange`, `handleSavePreferences`.

### services/notificationService.ts
- نقش: انتزاع لایه FCM و نوتیفای محلی/درون‌برنامه‌ای.
- توابع:
  - `requestNotificationPermission()`: درخواست مجوز.
  - `getFCMToken()`: دریافت توکن با `vapidKey`.
  - `updateFCMToken(token)`: فراخوانی تابع callable سرور برای ذخیره توکن در `users/{uid}`.
  - `onForegroundMessage(cb)`: شنود پیام‌های فورگراند.
  - `showLocalNotification(title, body, icon?)`: نمایش نوتیف محلی با Web Notifications API.
  - `createInAppNotification({...})`: افزودن رکورد به کالکشن `notifications`.
- نکته امنیتی: شکست در `updateFCMToken` اپ را بلاک نمی‌کند؛ فقط لاگ می‌شود.

### services/reminderService.ts
- نقش: ساخت/واکشی/شنود یادآورها و منطق تشخیصی نیاز به ریمایندر بر اساس سررسید/پیشرفت.
- توابع کلیدی:
  - `createReminder(...)`: افزودن داکیومنت به `reminders`.
  - `getRemindersForStudyPlan(planId)`: کوئری بر اساس `studyPlanId` و `orderBy('scheduledFor','asc')`.
  - `listenToReminders(planId, cb)`: شنود realtime.
  - `checkStudyPlanReminders(plan)`: خروجی بولی برای نیاز به ریمایندر سررسید/پیشرفت.
  - `createAutomaticReminders(plan)`: ساخت یادآورهای لازم بر اساس منطق بالا.
  - `createTaskReminder(plan, task, date)`: یادآور مخصوص یک تسک.
- نکته فنی: در خواندن از Firestore، `Timestamp` به `Date` تبدیل می‌شود.

### functions/src/index.ts
- نقش: بک‌اند سروری بدون سرور برای ایمیل/پوش/زمان‌بندی.
- پیکربندی: `admin.initializeApp()` و `nodemailer.createTransport` با Gmail یا سرویس دلخواه.
- انواع داده در سرور: `StudyPlan`, `Task`, `User` (با `admin.firestore.Timestamp`).
- زمان‌بندی‌ها:
  - `checkUpcomingDeadlines`: بازه سررسید ۱ تا ۲ روز آینده؛ برای هر شرکت‌کننده: ایمیل، پوش (اگر `fcmToken`)، و رکورد `notifications`.
  - `checkIncompletePlans`: اگر `progress < 100` مشابه بالا پیام اطلاع‌رسانی.
- Callables:
  - `updateFCMToken`: ذخیره توکن در `users/{uid}`.
  - `sendInvitationEmail`: ارسال ایمیل دعوت و ثبت نوتیف `Invitation Sent` برای دعوت‌کننده.
  - `sendMemberAddedEmail`: ایمیل خوش‌آمدگویی به عضو جدید و ثبت نوتیف `Member Added`.
- توابع کمکی: `getParticipants`, `sendDeadlineReminderEmail`, `sendIncompletePlanEmail`, `sendPushNotification`, `createNotificationRecord`, `getDaysUntilDeadline`.
- نکته امنیتی: دسترسی به این توابع callable نیازمند `context.auth` است.

### firestore.rules
- نقش: کنترل دسترسی دقیق در سطح کالکشن/داکیومنت.
- قوانین مهم: مالک/مشارکت‌کنندگان برای `studyPlans`، محدودیت دسترسی `notifications` به صاحب `userId`, و تأیید عضویت در پلن برای `notes` و `reminders`. دعوت‌نامه‌ها بر اساس `inviterId` یا `inviteeEmail`.

### components/Auth/Login.tsx
- نقش: فرم ورود با ایمیل/رمز عبور و گزینه Google OAuth.
- وابستگی‌ها: `firebase/auth` برای `signInWithEmailAndPassword` و `signInWithPopup` با `GoogleAuthProvider`.
- وضعیت‌ها: `email`, `password`, `loading`, `error`.
- توابع: `handleEmailLogin` (فرم submit) و `handleGoogleLogin` (دکمه Google).
- UI: فرم با فیلدهای ایمیل/رمز، دکمه ورود، خطای قرمز در صورت شکست، و دکمه Google با آیکون SVG.
- نکته ارائه: مدیریت خطاها در try/catch و نمایش پیام مناسب به کاربر.

### components/Auth/Register.tsx
- نقش: فرم ثبت‌نام با فیلدهای نام، ایمیل، رمز، تأیید رمز، و رشته تحصیلی (اختیاری).
- وابستگی‌ها: `firebase/auth` برای `createUserWithEmailAndPassword` و `updateProfile`، `firebase/firestore` برای `setDoc`.
- وضعیت‌ها: `formData` (شامل تمام فیلدها)، `loading`, `error`.
- تابع `handleSubmit`: اعتبارسنجی تطبیق رمزها، ایجاد کاربر، به‌روزرسانی پروفایل، و ذخیره داکیومنت `users/{uid}` در Firestore.
- نکته ارائه: در صورت شکست، خطا نمایش داده می‌شود؛ در صورت موفقیت، کاربر به داشبورد منتقل می‌شود.

### components/StudyPlan/StudyPlanDetail.tsx
- نقش: صفحه جزئیات یک پلن با تب‌های Tasks، Notes، Members؛ مدیریت تسک‌ها، نوت‌ها، و اعضا.
- وابستگی‌ها: `useParams` برای `id`، `useNavigate`، `onSnapshot` برای شنود پلن، `onAuthStateChanged` برای کاربر، `updateDoc` برای بروزرسانی.
- وضعیت‌ها: `studyPlan`, `user`, `loading`, `activeTab`, `showAddTask/Note/Member`, `members[]`, `localProgress`.
- اثرات:
  - `useEffect` برای auth: ساخت `User` از `firebaseUser`.
  - `useEffect` برای پلن: `onSnapshot` روی `studyPlans/{id}` و تبدیل `Timestamp` به `Date`.
  - `useEffect` برای اعضا: واکشی جزئیات هر `participant` (ایمیل یا UID) از کالکشن `users`.
  - `useEffect` برای ریست `localProgress` هنگام به‌روزرسانی سرور.
- توابع:
  - `handleTaskToggle`: تغییر وضعیت تسک، محاسبه پیشرفت، بروزرسانی `tasks` و `progress` در Firestore، و نمایش فوری با `localProgress`.
  - `handleAddTask`: افزودن تسک جدید با `id` تولیدی و `createdAt`.
  - `handleDeleteTask`: حذف تسک و بازمحاسبه پیشرفت.
- UI: هدر با عنوان/موضوع، نوار پیشرفت، توضیحات، ناوبری تب‌ها، و مودال‌های افزودن.
- نکته ارائه: همگام‌سازی realtime و نمایش فوری تغییرات با `localProgress`.

### components/InviteMembers.tsx
- نقش: مودال دعوت اعضا با دو حالت: ارسال ایمیل یا تولید لینک دعوت.
- وابستگی‌ها: `firebase/firestore` برای `addDoc`، `firebase/functions` برای `httpsCallable`.
- وضعیت‌ها: `inviteeEmail`, `loading`, `error`, `success`, `invitationLink`, `invitationMode`.
- توابع:
  - `generateInvitationToken`: تولید توکن تصادفی.
  - `sendInvitationEmail`: فراخوانی تابع callable `sendInvitationEmail` برای ارسال ایمیل.
  - `handleEmailInvite`: ساخت `Invitation`، ذخیره در `invitations`، و ارسال ایمیل.
  - `handleGenerateLink`: تولید لینک دعوت و نمایش آن.
  - `copyToClipboard`: کپی لینک به کلیپ‌بورد.
- UI: سوئیچ بین حالت‌ها، فرم ایمیل، نمایش لینک، و دکمه‌های کپی/بستن.
- نکته ارائه: مدیریت خطا در ارسال ایمیل بدون بلاک کردن فرآیند دعوت.

### components/AddMember.tsx
- نقش: مودال افزودن عضو مستقیم یا با لینک دعوت.
- وابستگی‌ها: `firebase/firestore` برای `updateDoc` و `arrayUnion`، `firebase/functions` برای `httpsCallable`.
- وضعیت‌ها: `email`, `loading`, `error`, `success`, `showInviteModal`, `addMode`.
- تابع `handleSubmit`: بررسی تکراری نبودن، افزودن ایمیل به `participants` با `arrayUnion`، و ارسال ایمیل خوش‌آمدگویی با `sendMemberAddedEmail`.
- UI: سوئیچ بین حالت‌ها، فرم ایمیل، و دکمه باز کردن `InviteMembers`.
- نکته ارائه: عدم بلاک افزودن عضو در صورت شکست ایمیل.

### components/RedeemInvitation.tsx
- نقش: صفحه پذیرش دعوت با بررسی اعتبار، انقضا، و عضویت قبلی.
- وابستگی‌ها: `useAuthState` از `react-firebase-hooks/auth`، `firebase/firestore` برای `getDoc` و `updateDoc`.
- وضعیت‌ها: `user`, `invitation`, `studyPlan`, `loadingInvitation`, `joining`, `joinError`, `joinSuccess`.
- اثر `useEffect`: واکشی `invitations/{id}` و `studyPlans/{planId}` و تبدیل `Timestamp` به `Date`.
- تابع `handleJoinStudyPlan`: بررسی اعتبار/انقضا/عضویت، افزودن `uid` به `participants`، و علامت‌گذاری دعوت به‌عنوان استفاده‌شده.
- UI: نمایش جزئیات پلن، اطلاعات دعوت‌کننده، و دکمه‌های لغو/پیوستن.
- نکته ارائه: مدیریت حالات مختلف (خطا، یافت نشدن، موفقیت) و ریدایرکت خودکار.

### components/Reminders/ReminderList.tsx
- نقش: نمایش لیست یادآورهای آتی کاربر با آیکون و فرمت تاریخ.
- وابستگی‌ها: `getUpcomingReminders` از سرویس ریمایندر.
- وضعیت‌ها: `reminders[]`, `loading`.
- تابع `loadReminders`: فراخوانی `getUpcomingReminders(userId, 5)` و تنظیم `reminders`.
- توابع کمکی:
  - `formatDate`: نمایش زمان نسبی (دقیقه/ساعت/روز) یا تاریخ کامل.
  - `getReminderIcon`: آیکون رنگی بر اساس نوع (deadline/task/progress).
- UI: کارت‌های یادآور با آیکون، عنوان، پیام، و برچسب نوع.
- نکته ارائه: نمایش حالت خالی و دکمه refresh در صورت عدم وجود یادآور.

### components/StudyPlan/TaskList.tsx
- نقش: لیست تسک‌ها با چک‌باکس، حذف، و نمایش تاریخ‌ها.
- Props: `tasks[]`, `onTaskToggle`, `onDeleteTask`.
- تابع `handleCheckboxChange`: فراخوانی `onTaskToggle` با `taskId` و وضعیت جدید.
- UI: کارت‌های تسک با چک‌باکس، عنوان، توضیحات، تاریخ ایجاد/تکمیل، و دکمه حذف.
- نکته ارائه: نمایش خط خورده برای تسک‌های تکمیل‌شده و مدیریت رویداد تغییر چک‌باکس.

### components/StudyPlan/AddTask.tsx
- نقش: مودال افزودن تسک جدید با عنوان و توضیحات.
- Props: `onClose`, `onAddTask`.
- وضعیت‌ها: `formData` (title/description), `loading`.
- تابع `handleSubmit`: فراخوانی `onAddTask` با داده‌های فرم و بستن مودال.
- UI: فرم با فیلدهای عنوان (اجباری) و توضیحات (اختیاری)، و دکمه‌های لغو/افزودن.
- نکته ارائه: مدیریت loading state و نمایش spinner در دکمه.

### components/StudyPlan/AddNote.tsx
- نقش: مودال افزودن نوت با عنوان، محتوا، و فایل ضمیمه.
- وابستگی‌ها: `firebase/firestore` برای `addDoc`، `firebase/storage` برای `uploadBytes` و `getDownloadURL`.
- وضعیت‌ها: `formData` (title/content/file), `loading`, `error`.
- تابع `handleSubmit`: آپلود فایل (در صورت وجود) به `notes/{planId}/{timestamp}_{filename}`، و ذخیره نوت در `notes` با `fileUrl/fileName/fileType`.
- UI: فرم با فیلدهای عنوان/محتوا (اجباری) و فایل (اختیاری)، و دکمه‌های لغو/افزودن.
- نکته ارائه: مدیریت آپلود فایل و ذخیره URL در Firestore.

### سایر فایل‌های پشتیبانی
- `public/firebase-messaging-sw.js`: Service Worker برای FCM (دریافت پوش در بک‌گراند).
- `public/manifest.json`: تنظیمات PWA و آیکون‌ها.
- `cypress/e2e/studybuddy.cy.js`: سناریوهای E2E اصلی برای اطمینان از سلامت گردش کار.
- `TESTING_GUIDE.md`: دستورالعمل کامل راه‌اندازی و اجرای تست‌ها.
