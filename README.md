# Impulse Landing Page — Netlify Deployment Guide

## Структура файлів

```
impulse-landing/
├── index.html       # Головна сторінка
├── style.css        # Стилі
├── main.js          # JavaScript (анімації, scroll reveal, лічильники)
├── netlify.toml     # Конфіг Netlify (headers, redirects)
├── robots.txt       # SEO
└── README.md        # Цей файл
```

---

## Деплой на Netlify — 3 способи

### Спосіб 1: Drag & Drop (найшвидший)

1. Зайди на [app.netlify.com](https://app.netlify.com)
2. Залогінься (GitHub / Google / email)
3. На головному екрані знайди **"Deploy manually"** або просто перетягни папку `impulse-landing/` у зону дропу
4. Сайт буде живий за ~10 секунд на `https://random-name.netlify.app`

### Спосіб 2: GitHub + Auto-Deploy (рекомендовано)

1. Створи репо на GitHub і залий туди папку `impulse-landing/`
   ```bash
   git init
   git add .
   git commit -m "Initial landing page"
   git remote add origin https://github.com/YOUR_USER/impulse-landing.git
   git push -u origin main
   ```
2. Зайди на [app.netlify.com](https://app.netlify.com) → **"Add new site"** → **"Import an existing project"**
3. Вибери GitHub → знайди репо `impulse-landing`
4. Налаштування білду:
   - **Build command**: _(залиш порожнім — статичний сайт)_
   - **Publish directory**: `.` (крапка — корінь репо)
5. Натисни **"Deploy site"**

✅ Тепер кожен `git push` автоматично деплоїть нову версію.

### Спосіб 3: Netlify CLI

```bash
# Встанови CLI
npm install -g netlify-cli

# Залогінься
netlify login

# З папки проекту
cd impulse-landing
netlify deploy --prod
```

---

## Підключення власного домену

1. В Netlify Dashboard → **"Domain settings"** → **"Add custom domain"**
2. Введи свій домен, наприклад `impulse.app`
3. Netlify покаже DNS записи які треба додати:

   | Type | Name | Value |
   |------|------|-------|
   | A | @ | 75.2.60.5 |
   | CNAME | www | your-site.netlify.app |

4. Додай ці записи в панелі свого реєстратора домену (Namecheap, GoDaddy, Cloudflare тощо)
5. Зачекай 10-60 хвилин на propagation

### SSL (HTTPS)
Netlify автоматично видає SSL сертифікат через Let's Encrypt — нічого робити не треба.

---

## Що налаштувати після деплою

### 1. Змінити мета-теги в index.html
```html
<!-- Рядок 5 -->
<meta name="description" content="Твій опис..." />

<!-- Рядок 6-7 -->
<meta property="og:title" content="Impulse — Brain Training" />
<meta property="og:description" content="Твій OG опис..." />
```

### 2. Додати favicon
```html
<!-- В <head> після рядка 4 -->
<link rel="icon" type="image/png" href="/favicon.png" />
<link rel="apple-touch-icon" href="/apple-touch-icon.png" />
```

### 3. Змінити посилання кнопок "Download"
Знайди в `index.html` кнопки `href="#"` і заміни на реальні посилання на App Store / Google Play.

### 4. Google Analytics (опціонально)
```html
<!-- Перед </head> -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

### 5. robots.txt
Заміни `https://your-domain.com` на реальний домен.

---

## Перфоманс

Сайт вже оптимізований:
- ✅ Google Fonts через `preconnect` (швидке завантаження)
- ✅ CSS animations — GPU-accelerated (`transform`, `opacity`)
- ✅ IntersectionObserver для scroll reveal (не блокує main thread)
- ✅ `passive: true` на scroll listener
- ✅ Cache headers в `netlify.toml` (CSS/JS кешуються на рік)

Очікуваний Lighthouse score: **90+ / 90+ / 95+ / 100** (Performance / Accessibility / Best Practices / SEO)
