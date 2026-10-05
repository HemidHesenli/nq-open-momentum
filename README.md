# NQ Open Momentum

Nasdaq 100 fyuçersləri (NQ / MNQ) üçün yalnız vaxta əsaslanan mexaniki sistem. Sistem NY cash sessiyasının ilk 45 dəqiqəsindəki hərəkətin istiqamətində giriş edir, stop H1 ATR ilə hesablanır, hədəf 2R-dir.

*A time-based mechanical intraday system for Nasdaq 100 futures: trade in the direction of the first 45 minutes of the NY cash session, 1.5 × H1 ATR stop, 2R target, time exit at 15:59 NY.*

> [!WARNING]
> Bu, maliyyə məsləhəti deyil. Bütün nəticələr tarixi məlumat üzərində aparılmış backtest-dir. Gələcək nəticələrə zəmanət vermir. Üstünlük kiçikdir (trade başına təxminən 0.09–0.12R) və slippage-ə həssasdır.

## Qaydalar

| | |
|---|---|
| **Alət** | NQ / MNQ (Nasdaq 100 futures) |
| **Bias pəncərəsi** | 09:30 – 10:15 (New York vaxtı) |
| **İstiqamət** | 10:15-dəki qiymət 09:30 açılışından **yuxarıdırsa LONG**, **aşağıdadırsa SHORT** |
| **Giriş** | 10:15 NY, market order |
| **Stop** | **1.5 × ATR(14, SMA, 1H)**. Sonuncu bağlanmış saatlıq şamın (09:00–10:00) dəyəri |
| **Hədəf** | **2R** (= 3 × ATR) |
| **Vaxtla çıxış** | Stop və ya hədəf vurulmasa, 15:59 NY-da bağlanır |
| **Tezlik** | Gündə ən çox 1 trade |
| **Trade edilməyən günlər** | ABŞ fond bazarının bayramları və qısa günlər (NQ 13:00-da bağlanır, cash açılışı olmur) |

### Pozisiya ölçüsü
**Kontrakt sayı = risk $ / (stop nöqtəsi × nöqtə dəyəri)**. Nəticə aşağı yuvarlaqlaşdırılır. MNQ-də 1 nöqtə $2, NQ-də $20-dır.

Misal: ATR = 45 olsun. Stop = 67.5 nöqtə, 1 MNQ-nun riski = $135. $300 riskdə 2 MNQ düşür.

## Backtest nəticələri (2020 – 2026)

**Məlumat:** Dukascopy USATECH (Nasdaq 100 CFD), 1 dəqiqəlik, 02.01.2020 – 02.10.2026, 1685 trade. ABŞ bayramları çıxılıb.
**Fərziyyələr:** trade başına 0.75 nöqtə xərc. Eyni dəqiqədə həm stop, həm hədəf vurulubsa stop sayılıb (ehtiyatlı yanaşma).

| İl | Trade | Win rate | Trade başına | Cəmi |
|---|---|---|---|---|
| 2020 | 251 | 38.6% | −0.003R | −0.6R |
| 2021 | 251 | 43.0% | +0.078R | +19.6R |
| 2022 | 250 | 42.4% | +0.134R | +33.5R |
| 2023 | 248 | 38.3% | +0.046R | +11.4R |
| 2024 | 249 | 41.8% | +0.116R | +29.0R |
| 2025 | 247 | 43.7% | +0.131R | +32.3R |
| 2026 (9 ay) | 189 | 45.5% | +0.107R | +20.3R |

| Göstərici | Dəyər |
|---|---|
| Trade başına orta | **+0.086R** |
| Cəmi | +145.4R |
| Qazanclı aylar | 53 / 81 (65%) |
| Ayda orta | +1.8R |
| Maksimum drawdown | 31.1R |
| Ən uzun ardıcıl zərərli trade seriyası | 11 |
| Ən uzun ardıcıl zərərli ay seriyası | 4 |

### Aylar üzrə (R)
| İl | Yan | Fev | Mar | Apr | May | İyn | İyl | Avq | Sen | Okt | Noy | Dek | **Cəmi** |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 2020 | 4.9 | −1.6 | −5.6 | −2.3 | −3.0 | 6.5 | 5.5 | 2.8 | 0.3 | 2.6 | −1.7 | −9.0 | **−0.6** |
| 2021 | 0.6 | −6.9 | −7.5 | 5.5 | 5.1 | −4.0 | 4.8 | −0.9 | 2.8 | 11.5 | 1.9 | 6.5 | **+19.6** |
| 2022 | 0.1 | 3.4 | −1.8 | 13.7 | 5.4 | −1.5 | 5.4 | 3.2 | −0.2 | 3.2 | −0.5 | 3.1 | **+33.5** |
| 2023 | 0.5 | 5.2 | −0.4 | 3.3 | 2.9 | 0.6 | −5.0 | −0.5 | −7.1 | 3.3 | 8.9 | −0.5 | **+11.4** |
| 2024 | 1.9 | 5.3 | −6.0 | 8.3 | 11.8 | −2.8 | 10.1 | 4.8 | 2.4 | 1.7 | 2.7 | −11.3 | **+29.0** |
| 2025 | 1.6 | −1.4 | 3.8 | 12.6 | 3.5 | 0.3 | 1.6 | 12.1 | 3.5 | −4.4 | −3.2 | 2.4 | **+32.3** |
| 2026 | 6.4 | 12.4 | 1.9 | 6.0 | 2.0 | 6.0 | −5.2 | −3.4 | −3.9 | | | | **+22.3** |

## Metodologiya
- 3152 kombinasiya yoxlanılıb: bias pəncərəsinin bitmə vaxtı (02:15–15:00), pəncərə uzunluğu (15–240 dəqiqə), follow/fade və ATR əmsalı (0.5–1.5).
- Parametrlər **yalnız 2022–2024** üzrə seçilib. Tək təsadüfi pikləri çıxarmaq üçün qonşu parametrlərin də yaxşı olması tələb olunub.
- Seçilən variant heç görmədiyi dövrlərdə də müsbət qalıb:

| Dövr | Trade başına | Qeyd |
|---|---|---|
| 2020–2021 | +0.038R | Seçimdə istifadə olunmayıb |
| 2022–2024 | +0.099R | Seçim dövrü |
| 2025–2026 | +0.121R | Sınaq dövrü |

- NY açılışı zonasında 24 qonşu kombinasiyadan 21-i sınaq dövründə də müsbət qalıb.
- Eyni metod XAUUSD, EURUSD, GBPUSD və BTCUSD üzərində də yoxlanılıb. Heç birində dayanıqlı üstünlük tapılmayıb. Effekt NQ-yə xasdır.

## TradingView indikatoru
[`NQ_Open_Momentum.pine`](NQ_Open_Momentum.pine) (Pine Script v6)

1. TradingView-də **Pine Editor → New blank indicator** seç, kodu yapışdır, sonra **Save → Add to chart**.
2. Qrafik: **NQ1! / MNQ1!**, 1–5 dəqiqəlik interval.
3. Default ayarlar: bias pəncərəsi `0930-1015`, Follow, ATR × 1.5, 2R. "Trade başına risk ($)" sahəsinə öz risk məbləğini yaz.
4. Alert üçün: **Condition → NQ Open Momentum (ATR) → "Any alert() function call"**. Girişdə (istiqamət, SL, TP, MNQ sayı) və çıxışda bildiriş göndərir.

İndikatorun göstərdikləri: bias açılışı xətti, giriş, SL və TP xətləri, kontrakt sayı, trade nəticələri, qrafik tarixi üzrə statistika cədvəli.

> [!NOTE]
> İndikator ABŞ bayram günlərini tanımır. O günlərdə də siqnal verəcək, onları əl ilə buraxmaq lazımdır.

## Risk qeydləri
- Trade başına hesabın **0.5%-indən çox** risk etmə. 31R drawdown 0.5% riskdə təxminən −15.5% deməkdir.
- 11–14 ardıcıl zərərli trade və 3–4 zərərli ay sistem üçün normaldır.
- **5+ ardıcıl zərərli ay** və ya **35R-dən böyük drawdown** sistemin işləməyi dayandırdığının siqnalı sayılmalıdır.
- Real pulla başlamazdan əvvəl minimal riskdə (MNQ) forward-test et.
