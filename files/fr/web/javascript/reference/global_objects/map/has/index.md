---
title: Map.prototype.has()
slug: Web/JavaScript/Reference/Global_Objects/Map/has
---

{{JSRef}}

La méthode **`has()`** renvoie un booléen permettant de déterminer si l'objet `Map` en question contient la clé donnée.

{{EmbedInteractiveExample("pages/js/map-prototype-has.html")}}

## Syntaxe

```js
maMap.has(clé);
```

### Paramètres

- `clé`
  - : Ce paramètre obligatoire correspond à la clé dont on veut savoir si elle appartient à l'objet `Map`.

### Valeur de retour

Cette méthode renvoie un booléen : `true` si un élément avec cette clé existe au sein de l'objet `Map` et `false` sinon.

## Exemples

```js
var maMap = new Map();
maMap.set("truc", "toto");

maMap.has("truc"); // renvoie true
maMap.has("machin"); // renvoie false
```

## Spécifications

{{Specifications}}

## Compatibilité des navigateurs

{{Compat}}

## Voir aussi

- {{jsxref("Map")}}
- {{jsxref("Map.prototype.set()")}}
- {{jsxref("Map.prototype.get()")}}
