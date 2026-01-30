# Moltbot - तकनीकी स्टैक और ऐप संरचना (Tech Stack & App Structure)

यह दस्तावेज़ Moltbot एप्लिकेशन के तकनीकी निर्माण (Technology Stack) और इसकी फ़ाइल संरचना (File Structure) का विस्तृत विवरण प्रदान करता है।

## 1. टेक्नोलॉजी स्टैक (Technology Stack)

Moltbot को आधुनिक और शक्तिशाली तकनीकों का उपयोग करके बनाया गया है ताकि यह तेज़, सुरक्षित और स्केलेबल हो सके।

### **कोर (Core Runtime & Language)**
- **Runtime:** Node.js (Version 22+) - यह सर्वर-साइड जावास्क्रिप्ट रनटाइम है।
- **Language:** TypeScript - यह कोड को सुरक्षित और समझने में आसान बनाता है (Strongly typed)।
- **Package Manager:** pnpm - डिपेंडेंसी मैनेजमेंट के लिए तेज़ और कुशल टूल।
- **Module System:** ESM (ECMAScript Modules) - आधुनिक मॉड्युलर सिस्टम।

### **फ्रेमवर्क और सर्वर (Frameworks & Server)**
- **API/Server:** Hono और Express.js - HTTP रिक्वेस्ट हैंडलिंग के लिए।
- **Real-time Communication:** `ws` (WebSockets) - गेटवे और क्लाइंट्स के बीच रीयल-टाइम संचार के लिए।
- **Control Plane:** कस्टम **Gateway** आर्किटेक्चर जो WebSocket पर आधारित है।

### **AI और एजेंट्स (AI & Agents)**
- **Agent Runtime:** `@mariozechner/pi-agent-core` (Pi Agent) - AI एजेंट्स के लॉजिक को चलाने के लिए।
- **LLM Integration:** 
  - `@aws-sdk/client-bedrock` (AWS Bedrock)
  - `ollama` (लोकल मॉडल्स के लिए)
  - `node-llama-cpp`
- **अदर (Other):** `@sinclair/typebox` (स्कीमा वैलिडेशन के लिए)।

### **चैनल इंटीग्रेशन (Channel Libraries)**
विभिन्न चैटिंग प्लेटफॉर्म्स से जुड़ने के लिए विशेष लाइब्रेरीज़ का उपयोग किया गया है:
- **WhatsApp:** `@whiskeysockets/baileys`
- **Telegram:** `grammy` और `@grammyjs/runner`
- **Slack:** `@slack/bolt`
- **ब्राउज़र (Browser):** `playwright-core` और `chromium-bidi` (वेब ऑटोमेशन के लिए)।

### **डेवलपमेंट और टेस्टिंग (Dev Tools & Testing)**
- **Testing:** `vitest` - यूनिट और इंटीग्रेशन टेस्टिंग के लिए।
- **Linting & Formatting:** `oxlint` और `oxfmt` - कोड की गुणवत्ता बनाए रखने के लिए।
- **Build Tool:** `tsc` (TypeScript Compiler) और `rolldown`।

---

## 2. ऐप संरचना (Application Structure)

Moltbot का कोडबेस एक मॉड्यूलर और व्यवस्थित तरीके से संरचित है। मुख्य डायरेक्टरीज और उनका उद्देश्य नीचे दिया गया है:

### **मुख्य डायरेक्टरीज (Main Directories)**

#### `src/` (Source Code)
यह प्रोजेक्ट का मुख्य फोल्डर है जहाँ सारा सोर्स कोड रहता है।
- **`src/gateway/`**: यह एप्लिकेशन का "Control Plane" है। यह सभी कनेक्शन्स, सेशन्स और इवेंट्स को मैनेज करता है।
- **`src/cli/`**: कमांड लाइन इंटरफेस (CLI) के लिए कोड (जैसे `moltbot onboard`, `moltbot gateway`)।
- **`src/channels/`**: विभिन्न प्लेटफॉर्म्स (WhatsApp, Telegram, etc.) के लिए स्पेसिफिक कोड और लॉजिक यहाँ होता है।
- **`src/agents/`**: AI एजेंट्स का व्यवहार और लॉजिक।
- **`src/media/`**: ऑडियो, वीडियो और इमेज प्रोसेसिंग पाइपलाइन।
- **`src/database/`**: डेटा स्टोरेज और मैनेजमेंट (SQLite/Vector DB)।

#### `extensions/` (Plugins)
Moltbot को एक्सटेंडेबल बनाने के लिए प्लगइन्स यहाँ रखे जाते हैं। उदाहरण के लिए, विशेष चैनल इंटीग्रेशन या कस्टम टूल्स। ये मुख्य कोडबेस से अलग और स्वतंत्र हो सकते हैं।

#### `apps/` (Companion Apps)
Moltbot के मोबाइल और डेस्कटॉप ऐप्स का कोड यहाँ होता है:
- **`apps/ios/`**: iOS नोड ऐप (Swift)।
- **`apps/android/`**: Android नोड ऐप (Kotlin/Java)।
- **`apps/macos/`**: macOS मेन्यू बार ऐप।

#### `dist/` (Distribution)
जब कोड को "Build" किया जाता है (`npm run build`), तो कंपाइल किया हुआ जावास्क्रिप्ट कोड यहाँ सेव होता है। Node.js इसी फोल्डर से कोड को रन करता है।

#### `docs/` (Documentation)
प्रोजेक्ट की विस्तृत डॉक्यूमेंटेशन, गाइड्स और आर्किटेक्चर डायग्राम्स यहाँ मौजूद होते हैं।

#### `scripts/`
ऑटोमेशन और मेंटेनेंस के लिए हेल्पर स्क्रिप्ट्स (जैसे रिलीज़ चेक, डॉक जनरेशन, इंस्टॉलेशन स्क्रिप्ट्स)।

### **महत्वपूर्ण फाइलें (Key Files)**
- **`package.json`**: प्रोजेक्ट की डिपेंडेंसीज, स्क्रिप्ट्स और मेटाडेटा।
- **`moltbot.mjs`**: CLI का एंट्री पॉइंट।
- **`README.md`**: प्रोजेक्ट का परिचय और क्विक स्टार्ट गाइड।
- **`AGENTS.md`**: एजेंट्स और प्रोजेक्ट गाइडलाइन्स।

---

## 3. डेटा फ्लो (Data Flow Architectue)

संक्षेप में, Moltbot का डेटा फ्लो इस प्रकार काम करता है:
1. **मैसेज इनपुट:** यूज़र WhatsApp/Telegram आदि पर मैसेज भेजता है।
2. **चैनल लेयर:** `src/channels` में मौजूद लाइब्रेरी मैसेज को रिसीव करती है और उसे एक स्टैंडर्ड फॉर्मेट में बदलती है।
3. **गेटवे:** यह मैसेज **Gateway** (`src/gateway`) को भेजा जाता है।
4. **एजेंट प्रोसेसिंग:** गेटवे इसे सक्रिय **Session** और **Agent** (`src/agents`) को भेजता है।
5. **AI रिस्पॉन्स:** एजेंट AI मॉडल (Claude/GPT) से बात करता है, टूल्स का उपयोग करता है (जैसे वेब सर्च), और जवाब तैयार करता है।
6. **आउटपुट:** जवाब वापस गेटवे के जरिए चैनल को भेजा जाता है और यूज़र को डिलीवर हो जाता है।
