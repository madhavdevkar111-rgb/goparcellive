# GoParcel — Zero-budget launch pack

## या ZIP मध्ये काय आहे
- `index.html` — GoParcel चे मुख्य सार्वजनिक वेबसाईट / portal landing page
- `Customer/` — ग्राहक बुकिंग आणि ट्रॅकिंग पॅनल
- `Rider/` — रायडर पार्टनर पॅनल
- `Admin/` — ऑपरेशन्स / Admin पॅनल
- `shared/` — तिन्ही पॅनलसाठी सामायिक UI styles

## महत्त्वाचे: सध्याची टेक्नॉलॉजी
तिन्ही पॅनलमध्ये Firebase project `goparcel-f89b5` आणि Firestore वापरलेले आहेत. Customer, Rider आणि Admin मधील live updates हे Firestore listeners वर अवलंबून आहेत. सध्याचा server OTP / WhatsApp OTP आणि UPI UTR पडताळणीचा flow या पॅकमध्ये जाणीवपूर्वक बदललेला नाही.

## शून्य बजेटमध्ये वेबसाईट प्रकाशित करणे
### पर्याय A — GitHub Pages
1. GitHub वर नवीन repository तयार करा.
2. ZIP extract करून त्यातील सर्व फाइल्स repository च्या root मध्ये upload करा.
3. Repository → Settings → Pages उघडा.
4. Deploy from a branch निवडा; `main` आणि `/ (root)` निवडा.
5. Save करा. GitHub ने दिलेल्या URL वर वेबसाईट उघडा.

### पर्याय B — Firebase Hosting
Firebase CLI आणि Node.js असलेल्या संगणकावर:
1. `npm install -g firebase-tools`
2. `firebase login`
3. या project directory मध्ये `firebase init hosting` चालवा; public directory `.` निवडा.
4. SPA rewrite विचारल्यास `No` निवडा (आपले तीन स्वतंत्र HTML routes आहेत).
5. `firebase deploy --only hosting` चालवा.

Firebase Spark plan मध्ये उपलब्ध quota/feature मर्यादा लागू होतात. वापर वाढल्यास billing सुरू होऊ शकते; आधी खर्च तपासा.

## Live launch करण्याआधी आवश्यक सुरक्षा तपासणी
ही आवृत्ती static frontend आहे. Firebase web config browser मध्ये दिसणे सामान्य आहे; पण ते database security चा पर्याय नाही. Firestore Security Rules, Admin identity verification आणि OTP server endpoint ची स्वतंत्र तपासणी व deploy आवश्यक आहे.

**Admin पॅनल सार्वजनिक URL वर प्रकाशित करण्यापूर्वी Admin authentication व Firestore rules सुरक्षित आहेत याची खात्री करा.** फक्त UI मधील लपवलेले बटण किंवा client-side password ही सुरक्षा नाही.

## व्यावसायिक वापरापूर्वी चाचणी
- Test customer आणि test rider वापरून booking → accept → pickup → delivered flow पूर्ण करा.
- दोन वेगवेगळ्या फोनवर status sync तपासा.
- UTR चुकीचा/duplicate असल्यास admin verification नाकारतो का ते तपासा.
- Rider दुसऱ्याची ऑर्डर स्वीकारू शकत नाही याची Firestore transaction/rules ने खात्री करा.
- Pricing, commission, rider payout आणि refunds प्रत्यक्ष व्यवहारांशी जुळतात का ते तपासा.
- OTP expiry, resend limit, rate limiting आणि abuse protection तपासा.

## खर्चाबद्दल
GitHub Pages / Firebase Spark सारखे free tiers वापरून सुरुवात करता येऊ शकते; पण free tier अमर्यादित नाही. SMS, WhatsApp Business API, maps routing, storage, high traffic आणि backend functions यांना शुल्क लागू होऊ शकते. सध्याचा OTP/UPI flow paid SMS/payment gateway मध्ये migrate केलेला नाही.

## सध्याच्या पॅकबद्दल प्रामाणिक मर्यादा
मुख्य वेबसाईट landing page जोडले आहे. Existing Customer/Rider/Admin app code जतन केले आहे. આ ZIP स्वतःहून Firebase rules deploy करत नाही, Firebase project permissions बदलत नाही, domain खरेदी करत नाही किंवा production security audit पूर्ण झाल्याचा दावा करत नाही.
