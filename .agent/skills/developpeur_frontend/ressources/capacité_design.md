# Capacité : Implémenter Responsive Design

## 🎯 Contexte
Le responsive design assure que l'interface s'adapte fluides à tous les appareils. La stratégie est de concevoir **mobile d'abord**, puis ajouter des améliorations progressives.

---

## 📊 Breakpoints Standards (Tailwind)

Concevoir dans cet ordre:
1. Mobile (par défaut, pas de préfixe)
2. `sm:` (640px) - Smartphones en paysage
3. `md:` (768px) - Tablettes
4. `lg:` (1024px) - Laptops
5. `xl:` (1280px+) - Écrans larges

---

## 🔧 Processus de Conception

### Étape 1 : Concevoir MOBILE D'ABORD
```blade
<div class="px-4 py-2 text-base font-semibold">
    <!-- Styles mobile par défaut -->
</div>
```

### Étape 2 : Améliorer pour Tablettes
```blade
<div class="px-4 py-2 text-base md:px-8 md:py-4 md:text-lg">
    <!-- px/py augmentent, font grandit -->
</div>
```

### Étape 3 : Optimiser pour Desktops
```blade
<div class="px-4 py-2 text-base md:px-8 md:py-4 md:text-lg lg:px-12 lg:py-6">
    <!-- Maximiser l'espace disponible -->
</div>
```

### Étape 4 : Tester sur Vrais Appareils
- Chrome DevTools (cmd+shift+i, puis toggle device)
- iPhone, iPad physiques
- Smartphones Android

---

## 🎨 Patterns Responsive Courants

**Grille Fluide**
```blade
<!-- 1 colonne mobile, 2 tablette, 3 desktop -->
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
    @foreach($items as $item)
        <div>{{ $item->name }}</div>
    @endforeach
</div>
```

**Navigation Responsive**
```blade
<nav class="flex flex-col md:flex-row gap-4">
    <a href="/">Home</a>
    <a href="/about">About</a>
</nav>
```

**Images Responsives**
```blade
<img src="/image.jpg" class="w-full h-auto" alt="...">
<!-- w-full = largeur 100%, h-auto = hauteur automatique -->
```

**Typographie Responsive**
```blade
<h1 class="text-2xl md:text-3xl lg:text-4xl font-bold">
    Titre Responsif
</h1>
```

---

## 📏 Éléments à Vérifier

### Espacements
```blade
<!-- Classes par défaut, puis breakpoints -->
<div class="px-2 py-1 md:px-4 md:py-2 lg:px-8 lg:py-4">
```

### Flexibilité Conteneurs
```blade
<!-- Container centré avec padding horizontal -->
<div class="max-w-6xl mx-auto px-4">
    <!-- max-w-6xl: limite largeur, mx-auto: centre -->
</div>
```

### Masquage Conditionnel
```blade
<!-- Masquer/afficher selon écran -->
<div class="hidden md:block">Visible sur tablette+</div>
<div class="md:hidden">Visible sur mobile uniquement</div>
```

---

## ⚠️ Erreurs Courantes

- ❌ Oublier `w-full h-auto` sur images → Débordements
- ❌ Utiliser des tailles fixes (px) → Inflexible
- ❌ Breakpoints disparates → Incohérent
- ❌ Négliger le mobile → Mauvaise UX principale

---

## ✅ Checklist de Validation

Tester sur ces résolutions:
- [ ] 320px (iPhone SE)
- [ ] 480px (Android phone)
- [ ] 768px (iPad)
- [ ] 1024px (iPad Pro)
- [ ] 1280px (Laptop)
- [ ] 1920px+ (Desktop)

Points à vérifier:
- [ ] Pas de débordement horizontal (overflow)
- [ ] Texte lisible (taille, contrast)
- [ ] Boutons cliquables (min 44x44px)
- [ ] Images correctement dimensionnées
- [ ] Navigation utilisable
- [ ] Pas de contenu caché involontairement
