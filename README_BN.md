# ARX HTML TO APK — Render Deployment

এটি একটি Telegram Bot, যা HTML/HTM অথবা `index.html`-সহ ZIP এবং PNG/JPEG app icon গ্রহণ করে একটি বাইরের APK Build API-তে পাঠায়। API সফলভাবে APK ফেরত দিলে Bot সেটি Telegram-এ পাঠায়।

## গুরুত্বপূর্ণ সীমাবদ্ধতা

- এটি Android Studio-এর মতো Native APK Builder নয়; HTML-কে Android WebView APK-তে রূপান্তরের জন্য API ব্যবহার করে।
- `API_URL`-এ দেওয়া Build API সচল ও অনুমোদিত হতে হবে। API বন্ধ/অপ্রাপ্য হলে Bot APK তৈরি করতে পারবে না।
- বর্তমান API-এর বর্ণনা অনুযায়ী APK হলো test build; প্রতিবার temporary signing key ব্যবহৃত হয়।
- Telegram Bot polling ব্যবহার করে, তাই Render-এ **Background Worker** হিসেবে চালাতে হবে; Web Service হিসেবে নয়।
- একই Bot Token দিয়ে একসঙ্গে একাধিক worker চালাবেন না।

## Render-এ Deploy

1. এই ZIP Extract করুন।
2. পুরো `ARX_HTML_TO_APK_RENDER` ফোল্ডারটি GitHub repository-তে Upload/Push করুন।
3. Render-এ Login করে **New + → Blueprint** নির্বাচন করুন।
4. GitHub repository Connect করুন এবং `render.yaml` নির্বাচন করে Deploy করুন।
5. Render চাইলে `BOT_TOKEN` Secret দিন। Telegram-এর `@BotFather` থেকে পাওয়া Bot Token এখানে বসাবেন।
6. `API_URL`-এ আপনার বৈধ Build API endpoint দিন। ডিফল্ট endpointটি আগের প্রজেক্ট থেকে রাখা হয়েছে; এর uptime বা ভবিষ্যৎ কার্যকারিতা নিশ্চিত নয়।
7. Deploy Log-এ `APK Converter Studio starting…` দেখলে worker চালু হয়েছে।

## Environment Variables

| Key | Value |
|---|---|
| `BOT_TOKEN` | Telegram BotFather token — Secret হিসেবে দিন |
| `API_URL` | APK Build API endpoint |

**কখনো BOT_TOKEN GitHub-এ লিখবেন না।** শুধু Render-এর Environment/Secret settings-এ দিন। Token প্রকাশ হয়ে গেলে BotFather দিয়ে revoke করে নতুন token নিন।

## Bot ব্যবহার

- `/start` বা `/menu` — Main menu
- `Create APK` — App name → HTML/ZIP → PNG/JPEG icon → Review → Build
- `/help` — নির্দেশনা
- `/cancel` — চলমান ধাপ বাতিল

## নিজের Android App বানানোর পরবর্তী ধাপ

এই ZIP-টি Render-এ চালানোর জন্য Telegram backend। এটি নিজে থেকে Android APK Converter-এর Android app তৈরি করে না। নিজের Android app বানাতে হলে আলাদা Android Native (Kotlin/XML) client এবং নিরাপদ backend API তৈরি করতে হবে। Bot token কখনো Android app-এর ভিতরে রাখা যাবে না।

## Dependencies

`requirements.txt`-এ Python Telegram Bot, HTTP request এবং image processing-এর লাইব্রেরি রয়েছে।


## UI আপডেট: একটিমাত্র Bot status message

এখন App name, Website upload, Icon upload, Review এবং Build progress ধাপে Bot আগের status message-টিই edit করবে। ফলে প্রতিটি ধাপে নতুন নতুন Bot message জমবে না। ব্যবহারকারীর পাঠানো HTML/ZIP ও Icon ফাইলের message অবশ্য Telegram chat-এ আলাদা থাকবে; Bot সেগুলো মুছতে পারে না।
