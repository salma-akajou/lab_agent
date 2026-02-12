# Capacité : Ajouter Interactivité avec Alpine.js

## 🎯 Contexte
Alpine.js permet d'ajouter des interactions réactives sans frameworks lourds. Les données et événements sont déclarés directement dans le HTML.

---

## 📋 Décider Where Alpine est Nécessaire

| Besoin | Alpine | Raison |
|--------|--------|--------|
| Dropdown, toggle, modal | ✅ **OUI** | Légère, parfaite |
| Validation en temps réel | ✅ **OUI** | Rapide, réactive |
| Animations complexes | ❌ **NON** | Utiliser Tailwind transitions |
| SPA, Router côté client | ❌ **NON** | Utiliser Vue/React |
| Form interactif simple | ✅ **OUI** | Parfait |

---

## 🔧 Étapes Pratiques

### Étape 1 : Initialiser les Données
```blade
<div x-data="{ open: false, count: 0 }">
    <!-- Contenu -->
</div>
```

### Étape 2 : Ajouter les Événements
```blade
<button @click="open = !open">
    Toggle Menu
</button>
```

### Étape 3 : Afficher/Masquer Conditionnellement
```blade
<div x-show="open" class="menu">
    <!-- Menu content -->
</div>
```

### Étape 4 : Boucler sur des Collections
```blade
<ul>
    <template x-for="item in items" :key="item.id">
        <li x-text="item.name"></li>
    </template>
</ul>
```

### Étape 5 : Two-way Binding
```blade
<input type="text" x-model="search" 
       placeholder="Rechercher...">

<p x-text="`Résultats: ${search}`"></p>
```

---

## 📚 Directives Essentielles

| Directive | Usage | Exemple |
|-----------|-------|---------|
| `x-data` | Initialiser | `x-data="{ open: false }"` |
| `@click`, `@submit` | Événements | `@click="doSomething()"` |
| `x-show` | Afficher/Masquer (display:none) | `x-show="isVisible"` |
| `x-if` | Monter/démonter du DOM | `x-if="condition"` |
| `x-model` | Two-way binding | `x-model="email"` |
| `x-for` | Boucler | `x-for="item in items"` |
| `x-text` | Mettre à jour le texte | `x-text="message"` |
| `x-html` | Mettre à jour l'HTML | `x-html="htmlContent"` |
| `:class` | Classes dynamiques | `:class="{ active: isActive }"` |
| `:style` | Styles dynamiques | `:style="{ color: activeColor }"` |

---

## 💡 Exemples Courants

**Dropdown Menu**
```blade
<div x-data="{ open: false }">
    <button @click="open = !open">Menu</button>
    <div x-show="open" @click.away="open = false">
        Options...
    </div>
</div>
```

**Compteur**
```blade
<div x-data="{ count: 0 }">
    <button @click="count++">+</button>
    <p x-text="count"></p>
</div>
```

**Validation Champ Email**
```blade
<input x-data x-model="email" type="email">
<span x-show="!email.includes('@')" class="error">
    Email invalide
</span>
```

---

## ⚠️ Pièges Courants

- ❌ Logique trop complexe → Déléguer au serveur
- ❌ États partagés entre composants → Utiliser un store
- ❌ Oublier de fermer les modals → Ajouter `@click.away`
- ❌ Effets de performance → Utiliser `x-cloak` pour éviter le flash

---

## 🎭 Bonne Utilisation

✅ Simplicité avant tout (si c'est complexe, repenser)  
✅ Nécessaire seulement → Pas de HTML vide avec Alpine  
✅ Performant → Peu de re-renders  
✅ Accessible → Garder les attributs ARIA

---

## ✅ Validation Finale

- [ ] Interactions testées dans le navigateur
- [ ] États réactifs fonctionnels
- [ ] Aucun lag ou performance issue
- [ ] Accessibilité OK (clavier, ARIA)
- [ ] Code court et lisible
- [ ] Compatible navigateurs modernes
