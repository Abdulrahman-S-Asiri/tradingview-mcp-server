# 🤖 دليل المتداول الآلي (Claude MCP Trading Routine)

هذا الملف عبارة عن دليل تشغيل (قائمة مهام يومية) لكيفية استخدام الذكاء الاصطناعي (Claude) عبر الـ MCP للتحكم في منصة TradingView وتطبيق استراتيجية **"قناص السيولة برافعة 20x"** خطوة بخطوة.

---

## ⏰ أولاً: أوقات التداول المسموحة (توقيت مكة المكرمة)
الاستراتيجية تعتمد على اصطياد السيولة، والسيولة توجد في أوقات محددة يعرّفها الـ Algorithms.
- 🟢 **الجلسة الأوروبية (لندن):** من `10:00 صباحاً` إلى `1:00 ظهراً`. (جلسة هادئة وحركتها واضحة).
- 🔥 **الجلسة الأمريكية (نيويورك):** من `4:30 عصراً` إلى `7:30 مساءً`. (أهم جلسة في اليوم وأعلى سيولة).
- ❌ **الجلسة الآسيوية:** `ممنوع التداول`. تذبذب عشوائي أو مسار عرضي ميت.
- ❌ **عطلة نهاية الأسبوع:** `ممنوع التداول`. سيولة ضعيفة جداً وتلاعب فج.

---

## 🔄 ثانياً: الروتين اليومي (من فتح الكمبيوتر إلى إغلاق الصفقة)

### 1️⃣ تهيئة ساحة المعركة (أول ما تفتح الكمبيوتر)
1. قم بفتح **PowerShell** وانسخ هذا الأمر لإغلاق وإعادة تهيئة منصة TradingView:
   ```powershell
   Stop-Process -Name TradingView -Force -ErrorAction SilentlyContinue; Start-Sleep 3;
   ```
   *(ثم قم بفتح وتشغيل تطبيق TradingView)*
2. انتظر **10 ثواني** ثم افتح جلسة الـ Terminal للذكاء الاصطناعي عبر الأمر:
   ```powershell
   cd "$HOME\tradingview-mcp"; claude
   ```
3. انسخ هذا الأمر وألصقه ليقوم كلود بالتهيئة:
   > *"Run `tv_health_check` to ensure the MCP connection is fully stable. Next, read our '20x Liquidity Sweep Sniper' strategy from the `CLAUDE.md` file. Critically evaluate the strength of these rules and explicitly verify your capability to monitor these specific conditions (Volume spikes, ATR, EMA 200, Fakeouts) using your MCP tools. Confirm when loaded."*

### 2️⃣ مسح السوق (تحديد الاتجاه الرئيسي)
بدل أن تحلل بنفسك، دع الذكاء الاصطناعي يفعل ذلك. انسخ له هذا الأمر:
> *"Analyze the `BTCUSDT.P` chart on the 1H timeframe. Where is the price positioned relative to the EMA 200, and what is our directional bias (Long or Short) today? Based on that, drop down to the 15m timeframe and identify the top 3 high-probability liquidity pools (prominent highs/lows) closest to the current price."*

### 3️⃣ ربط كلب الحراسة (المراقبة اللحظية)
بعد أن يحدد لك Claude أسعار القمم والقيعان المستهدفة، ضع تنبيهاتك اليدوية (Alerts) على TradingView في تلك الأسعار. 
عُد للمحاثة واعطِ كلود هذا الأمر للمراقبة:
> *"Excellent. Now monitor the 5m timeframe around those specific liquidity levels. Watch the OHLC data and Volume carefully. Once the price sweeps a level and prints a reversal candle (Fakeout) with a volume spike, notify me immediately. Ensure you calculate the ATR 14 to prepare for a dynamic Stop Loss."*

### 4️⃣ لحظة الرنين (التنفيذ العسكري)
عندما يرن التنبيه (تأكد من إغلاق شمعة الـ 5 دقائق)، اسأل كلود بسرعة للتحليل الدقيق:
> *"The candle just closed! Evaluate this close on the 5m timeframe. Is it a valid Fakeout/Sweep according to our strict rules? Send me the entry report specifying: 1. Entry price. 2. Stop Loss size (using ATR, but strictly capping at 0.8% max risk). 3. Take profit targets (TP1 at VWAP, TP2 at opposing structure)."*

### 5️⃣ فتح الصفقة على Binance وإدارتها
- اذهب إلى Binance وافتح الصفقة بناءً على الأرقام التي أعطاك إياها المساعد تماماً (استخدم 25% من المحفظة فقط رافعة 20x).
- ضع أمر **TP** (جني أرباح نصف الكمية) وأمر **SL** (وقف الخسارة).
- بمجرد تحقيق الصفقة لربح بنسبة **+0.5%**، حرك الستوب لوز فوراً إلى نقطة الدخول (Breakeven).
- أغلق الشاشة أو استمتع بيومك، الصفقة الآن آمنة 100%.

---

## 💡 نصائح ذهبية لمدير الـ MCP (أنت)
*   **لا تتداول المشاعر:** الميزة العظمى أنك لا تقرر الدخول، الآلة تقرر بناءً على الحسابات الرياضية الدقيقة للـ ATR والفوليوم.
*   **احذر الذيل الطويل:** إذا أغلق البيتكوين تاركاً ذيلاً هائلاً يستوجب ستوب لوز بحجم **1.2%** مثلاً، سيخبرك Claude بـ "إلغاء الصفقة" لأنها تجاوزت الحد الأقصى للمخاطرة المسموح بها (0.8%). اطعه فوراً!
*   **التحديث المستمر:** المنصات تتحرك بالثواني، عندما تسأل كلود في وقت التنفيذ، اطلب منه فحص الشموع الحية المحدثة وليست المخزنة لضمان دقة قرار الستوب لوز.

---

## 🧰 أوامر سرية متقدمة (Slash Commands)
هذه أوامر مبنية مسبقاً داخل المشروع يمكنك كتابتها مباشرة لـ Claude:

| الأمر | ماذا يفعل | متى تستخدمه |
|-------|----------|-------------|
| `/macro-dashboard` | لوحة ماكرو شاملة: VIX، الدولار، الذهب، البيتكوين، العوائد | قبل بداية كل جلسة لمعرفة حالة السوق العالمية |
| `/market-regime` | تحديد هل السوق Bull أو Bear أو Correction | قبل التداول لمعرفة مستوى المخاطرة العام |

### أوامر إنجليزية متقدمة (للتحليل العميق)
يمكنك إرسال هذه الأوامر لـ Claude مباشرة:
> *"Run `get_ta_summary` for `BINANCE:BTCUSDT.P` on timeframes 5, 15, 60. Then run `lookup_symbols` for the same symbol to get current price, volume, and daily change. Summarize the results."*

> *"Run `/macro-dashboard` and tell me: is Bitcoin in Risk-On or Risk-Off mode right now? Should I be cautious today?"*

## 📅 مواقع الأخبار الاقتصادية (تفقدها يومياً قبل التداول)
- 🔴 [Forex Factory Calendar](https://www.forexfactory.com/calendar) — الأخبار الحمراء = ممنوع التداول
- 🪙 [CoinMarketCal](https://coinmarketcal.com/en/) — أحداث خاصة بالكريبتو
- 📱 [Investing.com](https://www.investing.com/economic-calendar/) — تطبيق مع تنبيهات فورية على الجوال
