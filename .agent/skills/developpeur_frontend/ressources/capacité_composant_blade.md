# Capacité : Créer Composants Blade

## 🎯 Contexte
Les composants Blade encapsulent des portions réutilisables d'interface, avec props typées, styles Tailwind intégrés, et accessibilité garantie.

---

## 📋 Avant de Commencer

- [ ] Le besoin d'interface identifié
- [ ] Props et variantes définies
- [ ] Accessible sans dépendances externes
- [ ] Réutilisable dans plusieurs contextes

---

## 🔧 Création Pas à Pas

### Étape 1 : Créer le Fichier
Chemin : `resources/views/components/button.blade.php`

Nommage : kebab-case (ex: `user-card`, `alert-box`)

### Étape 2 : Déclarer les Props
```blade
@props(['label', 'type' => 'primary', 'size' => 'md'])

<button class="btn btn-{{ $type }} btn-{{ $size }}">
    {{ $label }}
</button>
```

### Étape 3 : Ajouter Styles Tailwind
```blade
@props(['isActive' => false])

<div class="px-4 py-2 rounded-lg transition-colors
    {{ $isActive ? 'bg-blue-500 text-white' : 'bg-gray-100 text-gray-900' }}">
    {{ $slot }}
</div>
```

### Étape 4 : Gérer les Attributs Dynamiques
```blade
@props(['class' => ''])

<div class="base-class {{ $class }}" {{ $attributes }}>
    {{ $slot }}
</div>
```

### Étape 5 : Utiliser dans une Vue
```blade
<x-button label="Créer" type="success" />

<x-card class="shadow-lg">
    Contenu du card
</x-card>
```

---

## 🎨 Bonnes Pratiques

### Accessibilité (A11y)
✅ Attributs `aria-*` pour lecteurs d'écran  
✅ Labels associés aux inputs (`for` et `id`)  
✅ Contrast de couleurs WCAG AA minimum  
✅ Focus visible et styles `:focus-within`

### Réutilisabilité
✅ Pas de logique métier dans le composant  
✅ Props flexibles, comportement simple  
✅ Classes CSS réutilisables  
✅ Noms clairs et documentés

### Performance
✅ Légèreté (pas de JS lourd)  
✅ Lazy loading pour images  
✅ CSS scoped au besoin  
❌ Jamais de requêtes HTTP directes

---

## 📐 Patterns Courants

**Bouton avec Variante**
```blade
@props(['variant' => 'primary'])
<button class="btn-{{ $variant }}">{{ $slot }}</button>
```

**Card with Header**
```blade
@props(['title'])
<div class="card">
    <h3>{{ $title }}</h3>
    {{ $slot }}
</div>
```

**Alert Box**
```blade
@props(['type' => 'info', 'dismissible' => false])
<div class="alert alert-{{ $type }}">
    {{ $slot }}
</div>
```

---

## ⚠️ Erreurs Courantes

- ❌ Logique complex dans le composant → Déléguer au contrôleur
- ❌ Props déjà tous optionnels → Prévoir les defaults
- ❌ Pas de documentation → Rendre les props claires
- ❌ Styles hardcodés → Utiliser Tailwind classes

---

## ✅ Validation Avant Livraison

- [ ] Composant créé dans `resources/views/components/`
- [ ] Props documentées et testables
- [ ] Accessible (ARIA, focus, contrast)
- [ ] Responsive (mobile, tablet, desktop)
- [ ] Réutilisé dans au moins 2 contextes
- [ ] Sans logique métier
