# README – AdvancedCrackme Éducatif

## Description

c'est un crackme éducatif multi-couches conçu pour apprendre :

* Analyse statique et dynamique
* Anti-debugging et protections simples
* Gestion sécurisée des clés
* Reverse engineering en environnement Windows

L’objectif est de **trouver les clés de chaque phase** pour obtenir le **flag final**.

---

## Structure du challenge

Le crackme est composé de **5 phases**, chacune avec :

1. Validation de la clé via une fonction de vérification
2. Possibles protections anti-debugging et anti-VM
3. Messages et indices pédagogiques

### Menu principal

À chaque phase, le crackme propose trois options :

1. **Saisir la clé** : entrer la clé pour déverrouiller la phase
2. **Afficher les indices** : obtenir des informations techniques sur la phase
3. **Quitter** : fermer le programme

---

## Étapes pédagogiques pour « cracker » le challenge

> ⚠️ Cette section est purement éducative et destinée à l’apprentissage du reverse engineering.

### Phase 1 : Analyse statique simple

* Ouvrir l’exécutable dans un **disséqueur / décompilateur** (ex: IDA, Ghidra)
* Rechercher la chaîne correspondant à la clé :

  ```text
  "5fa2a2c6e30b53c3a3b0a6b2dbfa2a2c"
  ```
* Comprendre que c’est une **clé hash simulée** pour la pédagogie.

### Phase 2 : Validation par pattern

* La fonction de validation vérifie le **format de la clé**
* Vérifier la longueur et le pattern des caractères
* Utiliser un petit script pour générer un string compatible ou l’analyser statiquement

### Phase 3-5 : Validations avancées

* Ces phases utilisent des **chaînes accentuées et Unicode**
* Le but est de pratiquer le reverse engineering sur des **chaînes wstring / UTF-16**
* La clé peut être trouvée via :

  * Analyse statique des chaînes
  * Débogage (sans violer les protections)
  * Lecture attentive du code dans un éditeur hexadécimal ou un décompilateur

---

## Astuces pour l’analyse

1. **Désactiver le debugger de manière contrôlée** pour comprendre le flux :

   * Étudier `DetectDebugger()` et `CheckHardwareBreakpoints()`
2. **Observer les prints Unicode** pour comprendre les messages et les indices
3. **Suivre le flux des phases** :

   * `current_layer` indique la phase actuelle
   * `ValidateKey(layer, key)` est la fonction de vérification

---

## Récupération du flag

Après avoir correctement trouvé toutes les clés :

```text
=== FÉLICITATIONS! ===
FLAG: CTF{Éducatif_Crackme_Success_<PID>}
```

* `<PID>` est l’ID du processus courant (utile pour l’unicité)
* Cela marque la fin de l’exercice éducatif

---

## Bonnes pratiques

* **Analyse statique d’abord**, puis dynamique si nécessaire
* **Ne pas contourner les protections de manière malveillante**
* Étudier les fonctions anti-debugging et VM pour comprendre les techniques défensives
* Apprendre à gérer les chaînes Unicode et le wcout/wcin sous Windows

---

## Résumé

Ce crackme permet de :

* Découvrir les bases de l’anti-debugging
* Travailler avec des clés de validation
* Comprendre la gestion Unicode dans un binaire Windows
* Appliquer des techniques d’analyse statique et dynamique

> L’objectif pédagogique est la **compréhension du flux et des protections**, pas le piratage malveillant.
