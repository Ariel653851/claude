# Générateur de site web local business

Tu génères un site web **single-file HTML** complet et moderne pour un commerce local (restaurant, coiffeur, barber, café, boulangerie…) qui n'a pas de site internet.

## Ce que tu reçois en entrée (via $ARGUMENTS)

L'utilisateur te donne les infos du commerce : nom, type, adresse, téléphone, horaires, photos disponibles (noms de fichiers), couleurs souhaitées ou ambiance, tarifs/menu si applicable.

## Ce que tu produis

Un fichier `[nom-du-commerce].html` **unique**, autonome, sans framework, sans dépendances NPM. Tout en HTML/CSS/JS vanilla dans un seul fichier.

## Style obligatoire

- **Moderne et premium** — pas de templates génériques
- **Typographie** : Google Fonts — combine une serif élégante (Playfair Display, Cormorant Garamond, DM Serif Display) avec une sans-serif propre (DM Sans, Inter, Outfit)
- **Hero avec scroll-expansion** : l'image principale démarre petite et centrée, s'agrandit progressivement au scroll jusqu'à remplir l'écran, puis le reste du site défile normalement
- **Palette de couleurs** adaptée au type de commerce et à l'ambiance demandée — jamais générique
- **Sections** adaptées au commerce :
  - Restaurant/café : hero, stats (note, horaires, prix moyen), ambiance avec photos, carte/menu tabulation, galerie mosaïque, réservation, carte OpenStreetMap, footer
  - Coiffeur/barber : hero, services + tarifs, galerie avant/après ou ambiance, booking/contact, localisation, footer
  - Autres : adapter selon le contexte
- **Animations subtiles** : fade-in au scroll (IntersectionObserver), parallax léger, hover effects
- **Nav fixe** qui devient opaque au scroll
- **100% responsive** mobile

## Règles techniques

- Images : chemins relatifs simples (`facade.jpg`, `interieur.jpg`…) — jamais de chemins absolus ou complexes
- Pas de `<script src="">` externe sauf Google Fonts en `<link>`
- Police Google Fonts via `<link>` dans le `<head>`
- Map : iframe OpenStreetMap (pas Google Maps — pas de clé API)
- Formulaire de réservation/contact avec `alert()` de confirmation
- Scroll-expansion hero en vanilla JS pur (wheel + touch events)

## Structure du scroll-expansion hero

```js
// Logique de base à adapter
let progress = 0;
let expanded = false;
document.body.style.overflow = 'hidden';

window.addEventListener('wheel', (e) => {
  if (!expanded) {
    e.preventDefault();
    progress = Math.min(1, Math.max(0, progress + e.deltaY * 0.001));
    applyProgress(progress);
    if (progress >= 1) { expanded = true; document.body.style.overflow = ''; }
  }
}, { passive: false });
```

## Ce que tu NE fais PAS

- Pas de React, Vue, Angular, Tailwind, Bootstrap
- Pas de images en base64 (trop lourd)
- Pas de placeholders génériques — si pas de photo dispo, utiliser un dégradé élégant de la palette
- Pas de lorem ipsum — inventer un vrai texte cohérent avec le commerce
- Pas de commentaires inutiles dans le code

## Output

Génère directement le fichier HTML complet. Après génération, envoie-le à l'utilisateur avec SendUserFile et explique en 2-3 lignes ce qui a été créé et comment placer les photos.
