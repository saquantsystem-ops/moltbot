# Moltbot - रन करने के निर्देश (Run Instructions)

हमने Moltbot को सफलतापूर्वक सेट कर दिया है। इसे चलाने के लिए अब आप नीचे दिए गए कमांड्स का उपयोग कर सकते हैं।

## 1. ऐप को स्टार्ट करना (Start the App)

चूँकि हमने पहली बार का सेटअप कर लिया है, आप सीधे ऑनबोर्डिंग विजार्ड चला सकते हैं:

```bash
pnpm moltbot onboard --install-daemon
```

यह कमांड आपसे ये चीज़ें पूछेगा:
1. **Model Provider:** (जैसे OpenAI, Anthropic, आदि) - आपको अपनी API Key डालनी होगी।
2. **Gateway Config:** Default सेटिंग्स के लिए बस Enter दबाएं।

## 2. गेटवे को चलाना (Run Gateway)

अगर आप ऑनबोर्डिंग पूरी कर चुके हैं, तो आप सीधे गेटवे चला सकते हैं:

```bash
pnpm gateway:watch
```

## नोट (Note)
- **Windows** पर कुछ "bash" स्क्रिप्ट्स में समस्या आ सकती है (जैसे `canvas:a2ui:bundle`), लेकिन मुख्य ऐप इसके बिना भी चल जाएगा।
- अगर `Authentication` के लिए पूछा जाए, तो आप `Synthetic` चुन सकते हैं अगर आपके पास अभी API Keys नहीं हैं।

---
**Status:** App built and verified. Ready for configuration.
