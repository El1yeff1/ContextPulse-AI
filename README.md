#  ContextPulse AI

> **NeuroBridge Hackathon** · Komanda: **Code of War**

**ContextPulse AI** proqramçı fasilədən qayıdanda kontekst itkisini sıfıra endirən süni intellekt platformasıdır. Sistem yalnız IDE və Pull Request metadatası ilə işləyir: ekran çəkilmir, klaviatura izlənmir, kod məzmunu oxunmur.

---

## 👥 Komanda
- **Mikayıl Məvsumzadə**
- **Elgün Hüseynli**
- **Nuran Əliyev**

---

## 🎯 Problem və Həll

Proqramçı toplantıdan və ya qısa fasilədən sonra harada qaldığını xatırlamaq üçün dəqiqələr itirir. Komanda rəhbəri isə tıxanmaların (blocker) səbəbini görmür, çünki fərdi işçini izləmək etik və hüquqi problem yaradır.

**ContextPulse AI iki problemi birlikdə həll edir:**
1. **İşçi üçün:** Fasilədən qayıdan kimi AI son işi qısa xatırlatma ilə bərpa edir: *son dəyişdirilən funksiya, açıq xəta, uğursuz test və PR-dəki həll olunmamış şərhlər.*
2. **Menecer üçün:** Komanda səviyyəsində tıxanmalar **K-anonimlik** qaydası ilə göstərilir. Fərdi işçi məlumatı menecer panelində yoxdur.

---

## ✨ Əsas Xüsusiyyətlər

- **Zero Spyware:** Ekran qeydi və keylogger yoxdur. Sistem yalnız metadata telemetriyası qəbul edir. API `privacy` sahələrindən biri `true` olarsa və ya sxemdə olmayan əlavə sahə (məsələn, `source_code`) gələrsə, hadisəni `422` ilə rədd edir.
- **Saniyələr İçində Geri Dönüş:** İşçi boş vəziyyətdən qayıdanda AI konteksti bir neçə saniyəyə bərpa edir. Xatırlatma Gemini ilə yazılır, cavab proqramla doğrulanır: uydurma fayl, funksiya, xəta kodu və ya PR nömrəsi varsa, hazır mətnə (`fallback`) keçilir.
- **K-anonimlik:** Menecer panelində qrup yalnız ən azı $K = 5$ cavab toplayanda görünür. Daha az olan qruplar "gizlədilib" kimi göstərilir.
- **İki Dil:** Sayt Azərbaycan və İngilis dillərini dəstəkləyir. AI xatırlatması da seçilmiş dildə gəlir.
- **Canlı Yenilənmə:** İşçi cavab verəndə menecer paneli Server-Sent Events (SSE) ilə eyni anda yenilənir.
- **Oflayn Demo Rejimi:** Backend olmadan (məsələn, GitLab Pages-də) sayt eyni məntiqi brauzerdə işlədir və "Oflayn demo" nişanı göstərir.

---

## 🛠️ Texnologiyalar

| Qat | Texnologiya |
| :--- | :--- |
| **Backend** | Python, FastAPI, Uvicorn, Pydantic |
| **Verilənlər bazası** | SQLite |
| **AI** | Google Gemini (`google-genai`) |
| **Frontend** | React 18 (CDN + Babel), Tailwind CSS (CDN), sadə HTML/CSS/JS (build addımı yoxdur) |
| **Canlı axın** | Server-Sent Events (SSE) |
| **Test** | pytest, httpx |
| **Deploy** | Docker, GitLab CI/CD, GitLab Pages |

---

## 📂 Layihə Strukturu

```text
NeuroBridge/
├── cp4/                      # Əsas layihə (FastAPI + frontend)
│   ├── app/
│   │   ├── main.py           # API marşrutları, SSE axını, statik fayllar
│   │   ├── core.py           # SQLite, seed data, K-anonim aqreqasiya
│   │   ├── ai_engine.py      # Gemini prompt, cavabın doğrulanması, fallback
│   │   └── schemas.py        # Pydantic sxemləri (məxfilik yoxlaması daxil)
│   ├── static/               # index.html, app.js, styles.css
│   ├── tests/test_core.py    # Avtomatik testlər
│   ├── simulator.py          # IDE simulyatoru
│   ├── Dockerfile
│   ├── .gitlab-ci.yml
│   └── requirements.txt
└── contextpulse_ai/          # İlk prototip (Streamlit, mock data)
