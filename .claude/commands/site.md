---
name: site
description: >
  Génère un site web vitrine complet en un seul fichier HTML autonome pour des petits commerces locaux
  (restaurants, coiffeurs, barbers, salons de beauté, cafés, boulangeries, spas, tatoueurs, etc.)
  qui n'ont pas encore de présence en ligne. Le style est moderne, cinématique, de niveau Awwwards /
  21st.dev / Aceternity UI — avec hero scroll-to-expand, spring physics, typographies massives et
  animations intentionnelles. Utilise ce skill dès que l'utilisateur mentionne vouloir créer un site
  pour un commerce local, une boutique, un artisan, ou demande un "site vitrine", "site one-page",
  "page web" pour un établissement physique.
---

# Skill : Génération de site vitrine — style 21st.dev / Aceternity

## Objectif

Produire un fichier HTML unique, autonome, au niveau de qualité **Awwwards / 21st.dev**.
Le style de référence est Aceternity UI : spring physics, hero scroll-to-expand, typographies 8–10rem,
bordures `rgba(255,255,255,0.06)`, grain texture, minimalisme cinématique.

**Aucune dépendance externe** sauf Google Fonts + OpenStreetMap embed iframe.
Fichier directement ouvrable ou déployable sur Netlify Drop.

---

## Étape 1 — Collecter les informations

Demander **en une seule fois** (jamais question par question) :

```
Pour créer ton site, j'ai besoin de :
1. Nom du commerce
2. Type de commerce (restaurant, barber, coiffeur, spa, café…)
3. Slogan ou phrase d'accroche (optionnel)
4. Services / plats / prestations + prix si disponibles
5. Horaires d'ouverture
6. Adresse (pour la carte)
7. Téléphone / email / Instagram
8. Photos disponibles (noms de fichiers) ou liens d'images
9. Couleurs préférées ? (optionnel — sinon palette auto)
```

Inventer les infos manquantes de façon crédible, noter ce qui a été inventé à la fin.

---

## Étape 2 — Palette de couleurs automatique

| Type de commerce | `--bg` | `--accent` | `--accent2` | `--text` |
|---|---|---|---|---|
| Restaurant gastronomique | `#080808` | `#c9a84c` | `#2d6a4f` | `#f0ede6` |
| Restaurant asiatique | `#09090f` | `#e63946` | `#f4a261` | `#edf2f4` |
| Café / boulangerie | `#100c07` | `#d4a574` | `#8b5e3c` | `#faf3e0` |
| Barber shop | `#0a0a0a` | `#8b5cf6` | `#06b6d4` | `#f8fafc` |
| Coiffeur / salon beauté | `#08080f` | `#ec4899` | `#a78bfa` | `#fdf4ff` |
| Spa / bien-être | `#060e0e` | `#10b981` | `#7c3aed` | `#f0fdf4` |
| Tatoueur | `#060606` | `#dc2626` | `#475569` | `#f1f5f9` |
| Pizzeria / fast casual | `#0a0703` | `#ef4444` | `#f97316` | `#fff7ed` |
| Bar / cocktail | `#04040e` | `#6366f1` | `#22d3ee` | `#eef2ff` |
| Défaut | `#080810` | `#7c3aed` | `#06b6d4` | `#f8fafc` |

Dériver `--bg2` = version légèrement plus claire (+ ~8% luminosité) de `--bg`.

---

## Étape 3 — Architecture visuelle (style 21st.dev)

### RÈGLES D'OR — à respecter absolument

1. **Typographies MASSIVES** : titres hero `clamp(5rem, 12vw, 10rem)`, sections `clamp(2.5rem, 5vw, 4rem)`
2. **Bordures ultra-fines** : toujours `1px solid rgba(255,255,255,0.06)` — jamais de border colorée criarde
3. **Pas de glow criard** : effets d'accent subtils, max `0 0 40px rgba(ACCENT, 0.12)` au repos
4. **Grain texture** : bruit SVG inline sur le fond pour éviter le flat black mort
5. **Spring physics** : toutes les animations utilisent `cubic-bezier(0.16, 1, 0.3, 1)` ou `cubic-bezier(0.76, 0, 0.24, 1)`
6. **Espacement généreux** : sections `padding: 10rem 6%`, jamais étouffé
7. **Minimalisme** : chaque élément justifie sa présence — pas de déco gratuite

---

## Étape 4 — Structure des sections

### 1. HERO — Scroll-to-Expand (OBLIGATOIRE)

L'effet signature : une image centrale petite qui grandit pour couvrir tout l'écran au scroll,
pendant que les deux mots du titre s'écartent vers les côtés opposés.

**Structure HTML requise :**

```html
<section id="hero">
  <div class="hero-bg-blur"></div>
  <div class="hero-media-wrap">
    <img class="hero-media-img" src="URL_IMAGE" alt="">
    <div class="hero-media-overlay"></div>
  </div>
  <div class="hero-title-wrap">
    <h1 class="hero-word hero-word--left">MOT1</h1>
    <h1 class="hero-word hero-word--right">MOT2</h1>
  </div>
  <p class="hero-date">Type de commerce</p>
  <p class="hero-cta-hint">↓ Scroll to discover</p>
</section>
```

**JS de l'effet :**

```javascript
let progress = 0;
let expanded = false;
document.body.style.overflow = 'hidden';

window.addEventListener('wheel', e => {
  if (expanded) return;
  e.preventDefault();
  progress = Math.min(1, Math.max(0, progress + e.deltaY * 0.001));

  const W = 320 + progress * (window.innerWidth - 320);
  const H = 420 + progress * (window.innerHeight - 420);
  mediaWrap.style.width = W + 'px';
  mediaWrap.style.height = H + 'px';
  mediaWrap.style.borderRadius = (1 - progress) * 20 + 'px';

  heroBg.style.opacity = 1 - progress;

  const tx = progress * 35;
  wordLeft.style.transform = `translateX(-${tx}vw)`;
  wordRight.style.transform = `translateX(${tx}vw)`;
  heroDate.style.transform = `translateX(${tx * 0.6}vw)`;
  heroCta.style.transform = `translateX(-${tx * 0.6}vw)`;

  if (progress >= 1) {
    expanded = true;
    document.body.style.overflow = '';
    document.getElementById('content').style.opacity = '1';
    document.getElementById('content').style.pointerEvents = 'auto';
  }
}, { passive: false });

let touchStart = 0;
window.addEventListener('touchstart', e => { touchStart = e.touches[0].clientY; });
window.addEventListener('touchmove', e => {
  if (expanded) return;
  e.preventDefault();
  const delta = touchStart - e.touches[0].clientY;
  touchStart = e.touches[0].clientY;
  // Même logique que wheel, delta * 0.006
}, { passive: false });
```

**CSS hero :**

```css
#hero {
  position: fixed;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  z-index: 10;
}
.hero-media-wrap {
  position: absolute;
  width: 320px; height: 420px;
  border-radius: 20px;
  overflow: hidden;
  transition: none;
  z-index: 1;
  box-shadow: 0 30px 80px rgba(0,0,0,0.5);
}
.hero-media-img { width: 100%; height: 100%; object-fit: cover; }
.hero-title-wrap {
  position: absolute; inset: 0;
  display: flex; align-items: center; justify-content: center;
  gap: 0.15em; z-index: 2; pointer-events: none;
}
.hero-word {
  font-family: 'Playfair Display', serif;
  font-size: clamp(5rem, 12vw, 10rem);
  font-weight: 900;
  color: var(--text);
  mix-blend-mode: difference;
  will-change: transform;
}
.hero-date, .hero-cta-hint {
  position: absolute;
  font-size: 0.75rem; letter-spacing: 0.15em;
  text-transform: uppercase; color: rgba(255,255,255,0.5);
  z-index: 3; will-change: transform;
}
.hero-date { bottom: 15%; left: 6%; }
.hero-cta-hint { bottom: 15%; right: 6%; }
.hero-bg-blur {
  position: absolute; inset: 0;
  background-size: cover; background-position: center;
  filter: blur(60px) brightness(0.3);
  transform: scale(1.1); z-index: 0;
}
```

---

### 2. ABOUT — Split layout cinématique

- 2 colonnes : texte gauche, image grande droite
- Titre massif + paragraphe + stats en ligne
- Image : `border-radius: 2px`, légère rotation `-2deg` au hover
- Les 2 colonnes entrent depuis les côtés opposés

### 3. SERVICES / MENU — Liste éditoriale

**NE PAS faire** une grille de cartes carrées génériques.
**FAIRE** une liste numérotée façon magazine :

```
01 — Coupe Classique .............. 25€
02 — Rasage Traditionnel .......... 30€
```

- Numéro en accent color, très grand et léger (`font-weight: 200`)
- Ligne de points : `border-bottom: 1px dotted rgba(255,255,255,0.15)`
- Prix aligné à droite
- Hover : `translateX(12px)` + trait accent à gauche
- Stagger 80ms entre chaque ligne

### 4. GALERIE — Grille asymétrique

- 3 colonnes, hauteurs variées
- Col 1 : 1 grande image (60% hauteur)
- Col 2 : 2 images empilées
- Col 3 : 1 image décalée (`margin-top: 3rem`)
- Lightbox au clic, fermeture Escape

### 5. HORAIRES — Section minimaliste

- Format : `Lundi — Mardi · 10h00 — 20h00`
- Jour actuel en accent color + `●` animé (pulse)
- Badge ouvert/fermé fixe en bas à droite

### 6. CONTACT — Deux colonnes

- Gauche : infos (tel, email, adresse) + icônes SVG inline
- Droite : formulaire avec champs seulement `border-bottom` (pas de box)
- Bouton : `width: 100%`, texte "Envoyer →"

### 7. CARTE

```html
<iframe src="https://www.openstreetmap.org/export/embed.html?bbox=...&layer=mapnik&marker=LAT,LNG"
  style="filter: grayscale(100%) contrast(1.15) brightness(0.7); width:100%; height:450px; border:none;">
</iframe>
```

### 8. FOOTER — Ultra-minimaliste

- 1 ligne : logo gauche, liens centre, copyright droite
- `border-top: 1px solid rgba(255,255,255,0.06)`

---

## Étape 5 — Animations (spring physics + scroll)

```css
--spring: cubic-bezier(0.16, 1, 0.3, 1);
--snap: cubic-bezier(0.76, 0, 0.24, 1);
--smooth: cubic-bezier(0.4, 0, 0.2, 1);
```

```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const idx = [...entry.target.parentElement.children].indexOf(entry.target);
      entry.target.style.transitionDelay = (idx * 90) + 'ms';
      entry.target.classList.add('in');
    }
  });
}, { threshold: 0.12, rootMargin: '0px 0px -60px 0px' });

document.querySelectorAll('[data-animate]').forEach(el => observer.observe(el));
```

```css
[data-animate] { opacity: 0; transform: translateY(40px); transition: opacity 0.8s var(--spring), transform 0.8s var(--spring); }
[data-animate].in { opacity: 1; transform: none; }
[data-animate="left"] { transform: translateX(-40px); }
[data-animate="right"] { transform: translateX(40px); }
[data-animate="scale"] { transform: scale(0.95); opacity: 0; }
```

### Animations supplémentaires OBLIGATOIRES après le hero

**A. Clip-path reveal sur les titres de section :**
```css
.section-title[data-animate] {
  clip-path: inset(0 100% 0 0);
  transition: clip-path 0.9s var(--spring), opacity 0.5s var(--smooth);
}
.section-title[data-animate].in { clip-path: inset(0 0% 0 0); opacity: 1; }
/* Trait accent qui grandit sous chaque titre */
.section-title::after {
  content: ''; display: block; width: 0; height: 1px;
  background: var(--accent); margin-top: 0.9rem;
  transition: width 0.7s var(--spring) 0.45s;
}
.section-title.in::after { width: 40px; }
```

**B. Marquee / bandeau défilant** entre About et Services :
```html
<div class="marquee-wrap">
  <div class="marquee-track">
    <span>NOM · TYPE · VILLE · DEPUIS XXXX · &nbsp;&nbsp;&nbsp;</span>
    <!-- répété 4× -->
  </div>
</div>
```
```css
.marquee-wrap { overflow: hidden; padding: 1.2rem 0; border-top: 1px solid var(--border); border-bottom: 1px solid var(--border); background: rgba(ACCENT_RGB, 0.08); }
.marquee-track { display: flex; width: max-content; animation: marquee 24s linear infinite; font-size: 0.78rem; letter-spacing: 0.2em; text-transform: uppercase; color: var(--accent); }
@keyframes marquee { from { transform: translateX(0); } to { transform: translateX(-50%); } }
```

**C. Stats counter animation** :
```html
<span class="stat-val" data-counter="8" data-suffix="+">0+</span>
<span class="stat-val" data-counter="4.9" data-decimal="1">0</span>
<span class="stat-val" data-counter="500" data-suffix="+">0+</span>
```
```javascript
function animateCounter(el) {
  const target = parseFloat(el.dataset.counter);
  const suffix = el.dataset.suffix || '';
  const decimals = parseInt(el.dataset.decimal || '0');
  const start = performance.now();
  const tick = (now) => {
    const p = Math.min((now - start) / 1800, 1);
    const ease = 1 - Math.pow(1 - p, 3);
    el.textContent = (target * ease).toFixed(decimals) + suffix;
    if (p < 1) requestAnimationFrame(tick);
  };
  requestAnimationFrame(tick);
}
// Déclencher via IntersectionObserver sur .about-stats
```

**D. Service items stagger** (slide depuis la gauche, 100ms entre chaque) :
```css
.service-item[data-stagger] { opacity: 0; transform: translateX(-30px); transition: opacity 0.6s var(--spring), transform 0.6s var(--spring); }
.service-item[data-stagger].stagger-in { opacity: 1; transform: translateX(0); }
.service-item.stagger-done { transition: transform 0.3s var(--spring); }
```

**E. Galerie stagger** (scale depuis 0.92, 120ms entre chaque) :
```css
.gallery-item[data-gstagger] { opacity: 0; transform: scale(0.92); transition: opacity 0.6s var(--spring), transform 0.6s var(--spring); }
.gallery-item[data-gstagger].in { opacity: 1; transform: scale(1); }
```

**F. Formulaire stagger** (fields depuis le bas, 80ms entre chaque) :
```css
.fg[data-fstagger] { opacity: 0; transform: translateY(20px); transition: opacity 0.5s var(--spring), transform 0.5s var(--spring); }
.fg[data-fstagger].in { opacity: 1; transform: none; }
```

**G. Parallax image About** :
```javascript
window.addEventListener('scroll', () => {
  const wrap = document.querySelector('.about-img-wrap');
  if (!wrap) return;
  const rect = wrap.getBoundingClientRect();
  wrap.querySelector('img').style.transform = `translateY(${(rect.top / window.innerHeight) * 40}px)`;
});
```
```css
.about-img-wrap { overflow: hidden; }
.about-img-wrap img { will-change: transform; transition: none; }
```

---

## Étape 6 — Grain texture (obligatoire)

```css
body::before {
  content: '';
  position: fixed; inset: 0;
  z-index: 9999; pointer-events: none; opacity: 0.035;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  background-size: 200px 200px;
}
```

---

## Étape 7 — Typographie

```css
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;0,900;1,400&family=Inter:wght@300;400;500&display=swap');
/* Titres : Playfair Display */
/* Corps, labels, UI : Inter */
/* Labels section : Inter 300, letter-spacing: 0.2em, uppercase, 0.7rem */
/* Numéros déco : Playfair Display, font-weight: 200, 4–6rem, opacity: 0.2 */
```

---

## Étape 8 — CSS Variables complètes

```css
:root {
  --bg: ; --bg2: ; --accent: ; --accent2: ; --text: ;
  --text-muted: rgba(248,250,252,0.45);
  --border: rgba(255,255,255,0.06);
  --card-bg: rgba(255,255,255,0.03);
  --radius-sm: 4px; --radius: 8px; --radius-lg: 16px;
  --spring: cubic-bezier(0.16, 1, 0.3, 1);
  --snap: cubic-bezier(0.76, 0, 0.24, 1);
  --smooth: cubic-bezier(0.4, 0, 0.2, 1);
}
```

---

## Étape 9 — Navbar

- Transparente → `rgba(BG, 0.85)` + `backdrop-filter: blur(24px)` au scroll > 80px
- Logo : Playfair Display, texte simple
- Liens : Inter 300, `letter-spacing: 0.08em`, underline `scaleX(0→1)` au hover
- Mobile : burger → menu plein écran avec stagger

---

## Étape 10 — Images Unsplash fiables

Format : `https://images.unsplash.com/photo-[ID]?auto=format&fit=crop&w=1200&q=85`

- **Barber** : `1585747860715-2ba37e788b70`, `1622286342621-4bd786c2447c`, `1503951914875-452162b0f3f1`
- **Restaurant** : `1517248135467-4c7edcad34c4`, `1414235077428-338989a2e8c0`, `1555396273-367ea4eb4db5`
- **Café** : `1495474472287-4d71bcdd2085`, `1442512595331-8f33382cd603`, `1509042239860-f550ce710b93`
- **Coiffeur** : `1560066984-138daecaab5d`, `1522337360788-8b13dee7a37e`
- **Spa** : `1544161515-4ab6ce6db874`, `1540555700478-4be289fbecef`
- **Boulangerie** : `1509440159596-0249088772ff`, `1555507036-ab1f4038808a`
- **Bar** : `1572116469-282b4724ce63`

---

## Étape 11 — Output final

1. Générer le fichier HTML complet dans `/home/user/claude/[nom-commerce]/[nom-commerce].html`
2. Seules dépendances : Google Fonts CDN, OpenStreetMap embed, Unsplash URLs (ou chemins locaux fournis)
3. Envoyer le fichier avec `SendUserFile`
4. Mentionner : ce qui a été inventé, comment remplacer les images, déploiement Netlify Drop

---

## Checklist avant de livrer

- [ ] Hero scroll-to-expand fonctionnel (JS intercept + image grandit + mots s'écartent)
- [ ] Fond blur du hero qui s'efface au scroll
- [ ] `mix-blend-mode: difference` sur les mots du hero
- [ ] Grain texture `body::before` présent
- [ ] CSS variables complètes avec `--spring`, `--snap`, `--smooth`
- [ ] Playfair Display + Inter importées
- [ ] Services en liste numérotée éditoriale (pas de grille de cartes)
- [ ] Galerie asymétrique
- [ ] Horaires : lignes épurées, jour actuel en accent + badge fixe
- [ ] Contact : champs avec seulement bordure bottom
- [ ] Maps OpenStreetMap avec `grayscale(100%)`
- [ ] Navbar transparente → frosted glass au scroll
- [ ] Intersection Observer avec stagger sur `[data-animate]`
- [ ] Spring physics sur toutes les transitions
- [ ] Responsive mobile avec burger menu
