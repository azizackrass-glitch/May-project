# wa-intake-agent — סיכום שיחת הבנייה ומסמך המשך

> מקור: שיחת Claude Code "בניית סוכן wa-intake-agent" (18–21/09/2026).
> סוכם ב-04/10/2026 כדי להמשיך מאיפה שהפסקנו.
> הערה: ערכים סודיים (טוקנים/מפתחות) **לא** נשמרים כאן בכוונה.

## מה הסוכן עושה
סוכן קליטת פניות ב-WhatsApp למשרד עורכי הדין "אחרס עזיז": מקבל הודעות מלקוחות
דרך WhatsApp Business Cloud API, מנתח אותן עם Claude ומנסח טיוטת מענה / סיכום
פנייה לעורך הדין. שרת Node.js בתיקייה `server/`.

## איפה הקבצים נמצאים
- כל קובצי הפרויקט נוצרו במקור על המחשב **AZIZOFFICE** (משתמש Windows: Dakwa),
  בנתיב `C:\Users\Dakwa\Desktop\wa-intake-agent`.
- נוצר מהם zip + מסמך `HANDOFF_2026-09-21.md`, שחולצו במחשב הבית
  **DESKTOP-4UIK6UU** (משתמש: aziz) בנתיב
  `C:\Users\aziz\Desktop\תקיה חדשה\wa-intake-agent`.
- הבעיה בפתיחת שיחה חדשה נבעה מעבודה ב-Claude **Web** (שם Code עובד רק עם
  GitHub). הפתרון: התקנת אפליקציית **Claude Desktop** במחשב ופתיחת שיחה שם.
- בתיקייה גם `RUN_LOG.md` עם כל הפירוט הכרונולוגי.

## מה כבר בוצע
- **קוד השרת**: משתמש ב-SDK הרשמי `@anthropic-ai/sdk`, פלט JSON מובנה.
- **USER_PROFILE.md** מלא: שם המשרד "אחרס עזיז", רח' השוק 13 מעלות-תרשיחא,
  שעות קבלה ב'–ו' 17:00–20:00, עלויות ייעוץ, דרך תיאום פגישה.
- **WhatsApp Business** (Coexistence עם האפליקציה בטלפון 972526840887):
  - WABA ID: `689788913941066`
  - Phone Number ID: `691057690763725`
  - Business Portfolio "אחרס עזיז - משרד עורכי דין" (Business ID `1447549436605389`)
  - אמצעי תשלום נוסף לחשבון ה-WABA.
- **Meta App** "wa-intake-agent" (App ID `1644793653670185`) משויך לתיק העסקי עם use case WhatsApp.
- **Claude API**: חשבון ב-console.anthropic.com, נטענו $20 (בלי טעינה אוטומטית).
  נוצר מפתח בשם `wa-intake-agent` (ללא תפוגה) — אבל **כנראה לא נשמר** ב-`.env`,
  כי הקובץ היה על המחשב השני.

## אירוע אבטחה שטופל
הוסר שותף עסקי לא מזוהה ("Hmax PX Nữ Víp DIE", Partner ID 8091369670897261)
מהתיק העסקי. מומלץ להחליף סיסמת פייסבוק ולבדוק Login Activity.

## מה חסום / נכשל
1. **חשבון המודעות האישי** (1476301773128953) מוגבל לצמיתות; הערעור נדחה סופית.
   לפרסום — להשתמש בחשבון המודעות העסקי (1033169395543006).
2. **System User + טוקן WhatsApp קבוע — טרם נוצר.** מנהלת חדשה (מארי) חייבת
   "להבשיל" 7 ימים. קיבלה Admin ב-21/09 → **אפשר לנסות מ-28/09 — כבר עבר.**

## צעדים הבאים (לפי הסדר)
1. **מפתח Claude API**: להיכנס ל-`console.anthropic.com/settings/keys`. אם קיים
   מפתח `wa-intake-agent` שהערך שלו לא נשמר — למחוק וליצור חדש, ולהדביק מיד
   ב-`server\.env` בשורה `ANTHROPIC_API_KEY=` (לא בצ'אט).
2. **System User** (מחוברים כמארי, לא כעזיז):
   `business.facebook.com/settings/system-users?business_id=1447549436605389`
   → Add → תפקיד Admin → הקצאת ה-WABA `689788913941066` →
   יצירת טוקן קבוע עם ההרשאות `whatsapp_business_messaging` +
   `whatsapp_business_management` → להדביק ב-`WHATSAPP_TOKEN=`.
   (שגיאת "invalid System User name" מטעה — לחפש הודעה ספציפית יותר.)
3. במחשב הבית: להתקין Node.js (אם חסר) ולהריץ `npm install` בתוך `server\`.
4. פריסה לשירות אירוח 24/7 (Railway / Render / Fly.io).
5. רישום Webhook ב-Meta App Dashboard (Callback URL + Verify Token).
6. מבחן קבלה מקצה לקצה: הודעה מטלפון אחר → טיוטה מגיעה לעורך הדין.

## server/.env — מבנה (בלי סודות)
```
WHATSAPP_TOKEN=            # ימולא בשלב 2
WHATSAPP_PHONE_ID=691057690763725
WHATSAPP_VERIFY_TOKEN=     # קיים בקובץ ה-.env המקורי
LAWYER_PHONE_NUMBER=972526840887
ANTHROPIC_API_KEY=         # ימולא בשלב 1
FIRM_NAME=אחרס עזיז
PORT=3000
```
