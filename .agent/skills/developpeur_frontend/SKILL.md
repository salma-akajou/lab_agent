---
name: agent-frontend
description: Développe l’interface utilisateur avec Blade, Tailwind et Alpine.js.
---

# Skill : Interface Utilisateur

## 🎯 Rôle Principal
**Objectif** : Construire des interfaces modernes, accessibles et interactives sans frameworks JavaScript lourds.

---

## ⛔ Contraintes UI
- 🚫 Pas de React, Vue ou Angular.
- 🎨 Tailwind CSS en priorité.
- ♿ Accessibilité obligatoire.
- 🔁 Réutilisation des composants existants.

---

## ⚡ Actions

### Action 1 : Créer Composants Blade
**Contexte** : Développer des blocs UI modulaires et accessibles  
**Capacité détaillée** : [ressources/capacité_composant_blade.md](ressources/capacité_composant_blade.md)

**Séquence d'actions :**
1. Identifier le besoin d'interface (bouton, card, modal, etc.)
2. Créer le fichier dans `resources/views/components/nom-kebab-case.blade.php`
3. Déclarer les props avec `@props()`
4. Styliser avec Tailwind CSS (responsive)
5. Ajouter les attributs ARIA pour l'accessibilité
6. Tester la réutilisabilité dans 2+ contextes

**Validation** : Composant accessible, responsive et réutilisable

---

### Action 2 : Ajouter Interactivité Alpine.js
**Contexte** : Implémenter l'interactivité sans frameworks lourds  
**Capacité détaillée** : [ressources/capacité_alpine_interactivite.md](ressources/capacité_alpine_interactivite.md)

**Séquence d'actions :**
1. Analyser le besoin d'interactivité (toggle, dropdown, validation, etc.)
2. Initialiser l'état avec `x-data="{ ... }"`
3. Ajouter les bindings: `x-model`, `x-show`, `@click`, etc.
4. Implémenter la logique des événements
5. Tester la réactivité et la performance
6. Vérifier l'accessibilité (clavier, ARIA)

**Validation** : Interactions fluides sans performance lag

---

### Action 3 : Implémenter Responsive Design
**Contexte** : Assurer l'affichage correct sur mobile, tablet, desktop  
**Capacité détaillée** : [ressources/capacité_design.md](ressources/capacité_design.md)

**Séquence d'actions :**
1. Concevoir d'abord pour mobile (par défaut)
2. Ajouter les variantes Tailwind (`sm:`, `md:`, `lg:`, `xl:`)
3. Tester sur 320px (mobile), 768px (tablet), 1024px+ (desktop)
4. Vérifier l'absence de débordements horizontaux
5. Valider la lisibilité du texte et des images
6. Tester sur vrais appareils (DevTools + émulateurs)

**Validation** : Interface fonctionnelle sur tous les breakpoints

---

## 🔄 Exemple de Création d'une Page
1. Layout principal.
2. Composants UI réutilisables.
3. Interactions Alpine.js.
4. Vérification responsive.

---

## 📐 Bonnes Pratiques
- Nommage clair des composants.
- Icônes standardisées.
- Code frontend simple et maintenable.
