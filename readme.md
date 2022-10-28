# Le jargon de la programmation fonctionnelle

> Ce document est une traduction française de [hemanth/functional-programming-jargon](https://github.com/hemanth/functional-programming-jargon).

La programmation fonctionnelle (PF) offre de nombreux avantages et sa popularité n'a cessé d'augmenter en conséquence. Cependant, chaque paradigme de programmation défini son propre jargon et la PF ne fait pas exception. En fournissant un glossaire, nous espérons faciliter l'apprentissage de la PF.

Les exemples sont écrits en JavaScript (ES2015). [Pourquoi JavaScript ?](https://github.com/hemanth/functional-programming-jargon/wiki/Why-JavaScript%3F)

Lorsque cela est possible, ce document utilise les termes définis dans la [spécification Fantasy Land](https://github.com/fantasyland/fantasy-land).

__Traductions__
* [Portugaise](https://github.com/alexmoreno/jargoes-programacao-funcional)
* [Espagnole](https://github.com/idcmardelplata/functional-programming-jargon/tree/master)
* [Chinoise](https://github.com/shfshanyue/fp-jargon-zh)
* [Indonésienne](https://github.com/wisn/jargon-pemrograman-fungsional)
* [Anglaise (exemples en Python)](https://github.com/jmesyou/functional-programming-jargon.py)
* [Anglaise (exemples en Scala)](https://github.com/ikhoon/functional-programming-jargon.scala)
* [Anglaise (exemples en Rust)](https://github.com/JasonShin/functional-programming-jargon.rs)
* [Coréenne](https://github.com/sphilee/functional-programming-jargon)
* [Polonaise](https://github.com/Deloryn/functional-programming-jargon)
* [Turque (exemples en Haskell)](https://github.com/mrtkp9993/functional-programming-jargon)
* [Russe (exemples en Haskell)](https://github.com/epogrebnyak/functional-programming-jargon)
* [Anglaise (exemples en Julia)](https://github.com/Moelf/functional-programming-jargon.jl)

__Table des matières__
<!-- RM(noparent,notop) -->

* [Arité](#arité)
* [Fonction d'ordre supérieur](#fonction-dordre-supérieur)
* [Fermeture](#fermeture)
* [Application partielle](#application-partielle)
* [Curryfication](#curryfication)
* [Auto-curryfication](#auto-curryfication)
* [Composition de fonctions](#composition-de-fonctions)
* [Continuation](#continuation)
* [Fonction pure](#fonction-pure)
* [Effets de bord](#effets-de-bord)
* [Idempotence](#idempotence)
* [Programmation tacite (point-free style)](#programmation-tacite-point-free-style)
* [Prédicat](#prédicat)
* [Contrats](#contrats)
* [Catégorie](#catégorie)
* [Valeur](#valeur)
* [Constante](#constante)
  * [Fonction constante](#fonction-constante)
  * [Foncteur constant](#foncteur-constant)
  * [Monade constante](#monade-constante)
* [Foncteur](#foncteur)
* [Pointed Functor](#pointed-functor)
* [Lift](#lift)
* [Referential Transparency](#referential-transparency)
* [Equational Reasoning](#equational-reasoning)
* [Lambda](#lambda)
* [Lambda Calculus](#lambda-calculus)
* [Functional Combinator](#functional-combinator)
* [Lazy evaluation](#lazy-evaluation)
* [Monoid](#monoid)
* [Monad](#monad)
* [Comonad](#comonad)
* [Kleisi Composition](#kleisi-composition)
* [Applicative Functor](#applicative-functor)
* [Morphism](#morphism)
  * [Homomorphism](#homomorphism)
  * [Endomorphism](#endomorphism)
  * [Isomorphism](#isomorphism)
  * [Catamorphism](#catamorphism)
  * [Anamorphism](#anamorphism)
  * [Hylomorphism](#hylomorphism)
  * [Paramorphism](#paramorphism)
  * [Apomorphism](#apomorphism)
* [Setoid](#setoid)
* [Semigroup](#semigroup)
* [Foldable](#foldable)
* [Lens](#lens)
* [Type Signatures](#type-signatures)
* [Algebraic data type](#algebraic-data-type)
  * [Sum type](#sum-type)
  * [Product type](#product-type)
* [Option](#option)
* [Function](#function)
* [Partial function](#partial-function)
  * [Dealing with partial functions](#dealing-with-partial-functions)
* [Total Function](#total-function)
* [Functional Programming Libraries in JavaScript](#functional-programming-libraries-in-javascript)


<!-- /RM -->

## Arité

Le nombre d'arguments qu'une fonction requiert. On utilise aussi des mots comme unaire, binaire, ternaire, etc.

```js
const somme = (a, b) => a + b
// L'arité de somme est 2 (binaire)
const inc = a => a + 1
// L'arité de inc est 1 (unaire)
const zero = () => 0
// L'arité de zero est 0 (nullaires)
```

__Pour aller plus loin__

* [Arité](https://fr.wikipedia.org/wiki/Arit%C3%A9) sur Wikipédia

## Fonction d'ordre supérieur

Une fonction qui prend une fonction en argument et/ou qui renvoie une fonction.

```js
const filtre = (predicat, xs) => xs.filter(predicat)
```

```js
const est = (type) => (x) => Object(x) instanceof type
```

```js
filtre(est(Number), [0, '1', 2, null]) // [0, 2]
```

## Fermeture

Une fermeture (closure en anglais) est une fonction accompagnée de l'ensemble des variables locales qu'elle a capturées au moment où elle a été définie.
Ces variables restent alors accessibles même après que le programme soit sorti du bloc où elles ont été définies.

```js
const ajouterA = x => y => x + y
const ajouterACinq = ajouterA(5)
ajouterACinq(3) // => 8
```

Dans ce cas, `x` a été capturée par la fermeture `ajouterACinq` avec la valeur `5`. `ajouterACinq` peut alors être
appelée avec `y` pour retourner la somme.

__Pour aller plus loin / Sources__
* [Closures (Fermetures)](https://developer.mozilla.org/fr/docs/Web/JavaScript/Closures)
* [Lambda Vs Closure (en)](http://stackoverflow.com/questions/220658/what-is-the-difference-between-a-closure-and-a-lambda)
* [JavaScript Closures highly voted discussion (en)](http://stackoverflow.com/questions/111102/how-do-javascript-closures-work)

## Application partielle

Appliquer partiellement une fonction signifie créer une nouvelle fonction en préremplissant certains des arguments de la fonction originale.

```js
// Permet de créer des fonctions appliquées partiellement
// Prend en paramètre une fonction et des arguments
const appliquerPartiellement = (f, ...args) =>
  // retourne une function qui prend en paramètre le reste des arguments
  (...resteDesArgs) =>
    // et appelle la fonction originale avec tous les arguments
    f(...args, ...resteDesArgs)

// une fonction à appliquer partiellement
const ajouter3 = (a, b, c) => a + b + c

// Applique partiellement `2` et `3` à `ajouter3` produit une fonction à un argument
const cinqPlus = appliquerPartiellement(ajouter3, 2, 3) // (c) => 2 + 3 + c

cinqPlus(4) // 9
```

Vous pouvez aussi utiliser `Function.prototype.bind` pour appliquer partiellement une fonction en JavaScript :

```js
const cinqPlus = ajouter3.bind(null, 2, 3) // (c) => 2 + 3 + c
```

L'application partielle permet de créer des fonctions plus simples à partir de fonctions plus complexes en y intégrant les données au fur et à mesure où elles sont disponibles. Les fonctions [curryfiées](#curryfication) sont automatiquement partiellement appliquées.


## Curryfication

Processus de conversion d'une fonction qui prend plusieurs arguments en une fonction qui les prend un à la fois.

Chaque fois que la fonction est appelée, elle n'accepte qu'un seul argument et retourne une fonction qui prend un argument jusqu'à ce que tous les arguments soient passés.

```js
const somme = (a, b) => a + b

const sommeCurryfiee = (a) => (b) => a + b

sommeCurryfiee(40)(2) // 42.

const ajouter2 = sommeCurryfiee(2) // (b) => 2 + b

ajouter2(10) // 12
```

## Auto-curryfication

La transformation d'une fonction qui prend plusieurs arguments en une fonction qui, si on lui donne moins que son nombre correct d'arguments, renvoie une fonction qui prend le reste des arguments. Lorsque la fonction est appelée avec le nombre correct d'arguments, elle est ensuite évaluée. 

Lodash et Ramda ont une fonction `curry` qui marche comme suit.

```js
const somme = (x, y) => x + y

const sommeCurryfiee = _.curry(somme)
sommeCurryfiee(1, 2) // 3
sommeCurryfiee(1) // (y) => 1 + y
sommeCurryfiee(1)(2) // 3
```

__Pour aller plus loin__
* [Favoring Curry (en)](http://fr.umio.us/favoring-curry/)
* [Hey Underscore, You're Doing It Wrong! (en)](https://www.youtube.com/watch?v=m3svKOdZijA)

## Composition de fonctions

L'acte de mettre deux fonctions ensemble pour en former une troisième, où la sortie d'une fonction est l'entrée de l'autre. C'est l'une des idées les plus importantes de la programmation fonctionnelle.

```js
const composer = (f, g) => (a) => f(g(a)) // Définition
const floorEtToString = composer((val) => val.toString(), Math.floor) // Utilisation
floorEtToString(121.212121) // '121'
```

## Continuation

À tout moment d'un programme, la partie du code qui n'a pas encore été exécutée est appelée _continuation_.

```js
const afficher = (nombre) => console.log(`Nombre ${nombre}`)

const ajouterUnEtContinuer = (nombre, cc) => {
  const resultat = nombre + 1
  cc(resultat)
}

ajouterUnEtContinuer(2, afficher) // 'Nombre 3'
```

Les continuations sont souvent observées en programmation asynchrone lorsque le programme doit attendre de recevoir des données avant de pouvoir continuer à s'exécuter. La réponse est souvent transmise au reste du programme, qui est la continuation, une fois qu'elle a été reçue.

```js
const poursuivreLeProgrammeAvec = (donnees) => {
  // Poursuit le programme avec les données
}

lireFichierAsync('chemin/vers/fichier', (err, reponse) => {
  if (err) {
    // prend en charge l'erreur
    return
  }
  poursuivreLeProgrammeAvec(reponse)
})
```

## Fonction pure

Une fonction est pure si sa valeur de retour n'est déterminée que par ses valeurs d'entrée, et qu'elle ne produit pas d'effets secondaires. La fonction doit toujours renvoyer le même résultat lorsqu'elle reçoit les mêmes entrées.

```js
const saluer = (nom) => `Salut, ${nom}`

saluer('Brianne') // 'Salut, Brianne'
```

Contrairement à chacun des exemples suivants :

```js
window.name = 'Brianne'

const saluer = () => `Salut, ${window.name}`

saluer() // "Salut, Brianne"
```

La sortie de l'exemple ci-dessus dépend de données stockées en dehors de la fonction...

```js
let salutation

const saluer = (nom) => {
  salutation = `Salut, ${nom}`
}

saluer('Brianne')
salutation // "Salut, Brianne"
```

... et celui-ci modifie l'état en dehors de la fonction.

## Effets de bord

Une fonction ou une expression est dite à effet de bord si, en plus de renvoyer une valeur, elle interagit (lit ou écrit) avec son environnement externe et que ce dernier est mutable.

```js
const differentAChaqueFois = new Date()
```

```js
console.log('Les entrées-sorties sont un effet de bord !')
```

## Idempotence

Une fonction est idempotente si, quand on l'applique à nouveau à son résultat, elle ne produit pas un résultat différent.

```js
Math.abs(Math.abs(10))
```

```js
sort(sort(sort([2, 1])))
```

## Programmation tacite (point-free style)

La programmation tacite, aussi connu sous le nom de _point-free style_, est une manière d'écrire une fonction en n'identifiant pas explicitement les arguments utilisés. Ce style de programmation requiert généralement les notions de [curryfication](#curryfication) ou de [fonctions d'ordre supérieur](#fonction-dordre-supérieur).

```js
// Étant donné
const map = (fn) => (liste) => liste.map(fn)
const ajouter = (a) => (b) => a + b

// Alors

// Sans la programmation tacite - `nombres` est un argument explicite
const incrementerTout = (nombres) => map(ajouter(1))(nombres)

// Avec la programmation tacite - La liste est un argument implicite
const incrementerTout2 = map(ajouter(1))
```

Les définitions de fonctions _point-free_ ressemblent à des affectations normales sans `function` ou `=>`. Il convient de mentionner que les fonctions _point-free_ ne sont pas nécessairement meilleures que leurs homologues non-_point-free_, car elles peuvent être plus difficiles à comprendre lorsqu'elles sont complexes.

## Prédicat

Un prédicat est une fonction qui retourne vrai ou faux pour une valeur donnée. Une utilisation courante d'un prédicat est la fonction utilisée pour filtrer un tableau.

```js
const predicat = (a) => a > 2

[1, 2, 3, 4].filter(predicat) // [3, 4]
```

## Contrats

Un contrat spécifie les obligations et les garanties de comportement qu'une fonction ou une expression doit respecter lors de son exécution. C'est un ensemble de règles qui s'appliquent aux entrées et sorties d'une fonction ou d'une expression. Des erreurs sont levées quand le contrat n'est pas respecté.

```js
// Definie le contrat : int -> boolean
const contrat = (entree) => {
  if (typeof entree === 'number') return true
  throw new Error('Contrat non respecté : attendu int -> boolean')
}

const ajouterUn = (nombre) => contrat(nombre) && nombre + 1

ajouterUn(2) // 3
ajouterUn('une chaine') // Contrat non respecté : attendu int -> boolean
```

## Catégorie

Une catégorie, dans la théorie des catégories, est un ensemble composé d'objets et des morphismes entre eux.
En programmation, les types représentent généralement les objets et les fonctions représentent les morphismes.

Pour qu'une catégorie soit valide, trois règles doivent être respectées :

1. Un morphisme d'identité, qui mappe un objet à lui-même, doit exister.
    Si `a` est un objet d'une catégorie,
    alors il doit y avoir une fonction de `a -> a`.
2. Les morphismes doivent pouvoir être composés.
    Si `a`, `b`, and `c` sont des objets d'une catégorie,
    `f` est un morphisme de `a -> b` et `g` est un morphisme de `b -> c`,
    alors `g(f(x))` doit être équivalent à `(g • f)(x)`.
3. La composition doit être associative.
    `f • (g • h)` est équivalent à `(f • g) • h`

Puisque ces règles régissent la composition à un niveau très abstrait, la théorie des catégories est excellente pour découvrir de nouvelles façons de composer des choses.

À titre d'exemple, nous pouvons définir une catégorie Max en tant que classe
```js

class Max {
  constructor (a) {
    this.a = a
  }

  id () {
    return this
  }

  compose (b) {
    return this.a > b.a ? this : b
  }

  toString () {
    return `Max(${this.a})`
  }
}

new Max(2).compose(new Max(3)).compose(new Max(5)).id().id() // => Max(5)
```

__Pour aller plus loin__

* [Théorie des catégories](https://fr.wikipedia.org/wiki/Th%C3%A9orie_des_cat%C3%A9gories) sur Wikipédia
* [Category Theory for Programmers (en)](https://bartoszmilewski.com/2014/10/28/category-theory-for-programmers-the-preface/)

## Valeur

Tout ce qui peut être assigné à une variable.

```js
5
Object.freeze({ nom: 'John', age: 30 }) // La fonction `freeze` renforce l'immutabilité.
;(a) => a
;[1]
undefined
```

## Constante

Une variable qui ne peut pas être réaffectée une fois définie.

```js
const five = 5
const john = Object.freeze({ name: 'John', age: 30 })
```

Les constantes sont [référentiellement transparentes](#referential-transparency). Cela veut dire qu'une constante peut être remplacée par la valeur qu'elle représente sans que le résultat soit modifié.

Avec les deux constantes ci-dessus, l'expression suivante renverra toujours `true`.

```js
john.age + five === ({ name: 'John', age: 30 }).age + 5
```

### Fonction constante

Une fonction [curryfiée](#curryfication) qui ignore son deuxième argument :

```js
const constante = a => () => a

;[1, 2].map(constante(0)) // => [0, 0]
```

### Foncteur constant

Un object dont la fonction `map` ne transforme pas le contenu. Voir [Foncteur](#foncteur).

```js
Constante(1).map(n => n + 1) // => Constante(1)
```

### Monade constante

Un object dont la fonction `chain` ne transforme pas le contenu. Voir [Monade](#monad).

```js
Constante(1).chain(n => Constante(n + 1)) // => Constante(1)
```

## Foncteur

Un objet qui implémente une fonction `map` qui prend en paramètre une fonction qui sera exécutée sur son contenu. Un foncteur doit respecter deux règles :

__Préserver l'identité__

```js
objet.map(x => x)
``` 

équivaut à `objet`. 


__Être composable__

```js
objet.map(x => g(f(x)))
```

équivaut à

```js
objet.map(f).map(g)
```

(où `f`, `g` sont des fonctions composables arbitraires).

L'implémentation de référence de [Option](#option) est un foncteur car il satisfait ces deux règles :

```js
some(1).map(x => x) // = some(1)
```

and

```js
const f = x => x + 1
const g = x => x * 2

some(1).map(x => g(f(x))) // = some(3)
some(1).map(f).map(g) // = some(3)
```

## Pointed Functor
An object with an `of` function that puts _any_ single value into it.

ES2015 adds `Array.of` making arrays a pointed functor.

```js
Array.of(1) // [1]
```

## Lift

Lifting is when you take a value and put it into an object like a [functor](#pointed-functor). If you lift a function into an [Applicative Functor](#applicative-functor) then you can make it work on values that are also in that functor.

Some implementations have a function called `lift`, or `liftA2` to make it easier to run functions on functors.

```js
const liftA2 = (f) => (a, b) => a.map(f).ap(b) // note it's `ap` and not `map`.

const mult = a => b => a * b

const liftedMult = liftA2(mult) // this function now works on functors like array

liftedMult([1, 2], [3]) // [3, 6]
liftA2(a => b => a + b)([1, 2], [3, 4]) // [4, 5, 5, 6]
```

Lifting a one-argument function and applying it does the same thing as `map`.

```js
const increment = (x) => x + 1

lift(increment)([2]) // [3]
;[2].map(increment) // [3]
```

Lifting simple values can be simply creating the object.

```js
Array.of(1) // => [1]
```


## Referential Transparency

An expression that can be replaced with its value without changing the
behavior of the program is said to be referentially transparent.

Given the function greet:

```js
const greet = () => 'Hello World!'
```

Any invocation of `greet()` can be replaced with `Hello World!` hence greet is
referentially transparent. This would be broken if greet depended on external
state like configuration or a database call. See also [Pure Function](#fonction-pure) and
[Equational Reasoning](#equational-reasoning).

##  Equational Reasoning

When an application is composed of expressions and devoid of side effects,
truths about the system can be derived from the parts. You can also be confident
about details of your system without having to go through every function.

```js
const grainToDogs = compose(chickenIntoDogs, grainIntoChicken)
const grainToCats = compose(dogsIntoCats, grainToDogs)
```
In the example above, if you know that `chickenIntoDogs` and `grainIntoChicken`
are pure then you know that the composition is pure. This can be taken further
when more is known about the functions (associative, commutative, idempotent, etc...)

## Lambda

An anonymous function that can be treated like a value.

```js
;(function (a) {
  return a + 1
})

;(a) => a + 1
```
Lambdas are often passed as arguments to Higher-Order functions.

```js
;[1, 2].map((a) => a + 1) // [2, 3]
```

You can assign a lambda to a variable.

```js
const add1 = (a) => a + 1
```

## Lambda Calculus
A branch of mathematics that uses functions to create a [universal model of computation](https://en.wikipedia.org/wiki/Lambda_calculus).

## Functional Combinator
A higher-order function, usually curried, which returns a new function changed in some way. Functional combinators are often used in [Point-Free Style](#programmation-tacite-point-free-style) to write especially terse programs.

```js
// The "C" combinator takes a curried two-argument function and returns one which calls the original function with the arguments reversed.
const C = (f) => (a) => (b) => f(b)(a)

const divide = (a) => (b) => a / b

const divideBy = C(divide)

const divBy10 = divideBy(10)

divBy10(30) // => 3
```

See also [List of Functional Combinators in JavaScript](https://gist.github.com/Avaq/1f0636ec5c8d6aed2e45) which includes links to more references.

## Lazy evaluation

Lazy evaluation is a call-by-need evaluation mechanism that delays the evaluation of an expression until its value is needed. In functional languages, this allows for structures like infinite lists, which would not normally be available in an imperative language where the sequencing of commands is significant.

```js
const rand = function * () {
  while (1 < 2) {
    yield Math.random()
  }
}
```

```js
const randIter = rand()
randIter.next() // Each execution gives a random value, expression is evaluated on need.
```

## Monoid

An object with a function that "combines" that object with another of the same type (semigroup) which has an "identity" value.

One simple monoid is the addition of numbers:

```js
1 + 1 // 2
```
In this case number is the object and `+` is the function.

When any value is combined with the "identity" value the result must be the original value. The identity must also be commutative.

The identity value for addition is `0`.
```js
1 + 0 // 1
0 + 1 // 1
1 + 0 === 0 + 1
```

It's also required that the grouping of operations will not affect the result (associativity):

```js
1 + (2 + 3) === (1 + 2) + 3 // true
```

Array concatenation also forms a monoid:

```js
;[1, 2].concat([3, 4]) // [1, 2, 3, 4]
```

The identity value is empty array `[]`

```js
;[1, 2].concat([]) // [1, 2]
```

As a counterexample, subtraction does not form a monoid because there is no commutative identity value:

```js
0 - 4 === 4 - 0 // false
```

## Monad

A monad is an object with [`of`](#pointed-functor) and `chain` functions. `chain` is like [`map`](#foncteur) except it un-nests the resulting nested object.

```js
// Implementation
Array.prototype.chain = function (f) {
  return this.reduce((acc, it) => acc.concat(f(it)), [])
}

// Usage
Array.of('cat,dog', 'fish,bird').chain((a) => a.split(',')) // ['cat', 'dog', 'fish', 'bird']

// Contrast to map
Array.of('cat,dog', 'fish,bird').map((a) => a.split(',')) // [['cat', 'dog'], ['fish', 'bird']]
```

`of` is also known as `return` in other functional languages.
`chain` is also known as `flatmap` and `bind` in other languages.

## Comonad

An object that has `extract` and `extend` functions.

```js
const CoIdentity = (v) => ({
  val: v,
  extract () {
    return this.val
  },
  extend (f) {
    return CoIdentity(f(this))
  }
})
```

Extract takes a value out of a functor.

```js
CoIdentity(1).extract() // 1
```

Extend runs a function on the comonad. The function should return the same type as the comonad.

```js
CoIdentity(1).extend((co) => co.extract() + 1) // CoIdentity(2)
```

## Kleisi Composition

An operation for composing two [monad](#monad)-returning functions (Kleisli Arrows) where they have compatible types. In Haskell this is the `>=>` operator.

Using [Option](#option):

```js
// safeParseNum :: String -> Option Number
const safeParseNum = (b) => {
  const n = parseNumber(b)
  return isNaN(n) ? none() : some(n)
}

// validatePositive :: Number -> Option Number
const validatePositive = (a) => a > 0 ? some(a) : none()

// kleisliCompose :: Monad M => ((b -> M c), (a -> M b)) -> a -> M c
const kleisliCompose = (g, f) => (x) => f(x).chain(g)

// parseAndValidate :: String -> Option Number
const parseAndValidate = kleisliCompose(validatePositive, safeParseNum)

parseAndValidate('1') // => Some(1)
parseAndValidate('asdf') // => None
parseAndValidate('999') // => None
```

This works because:

 * [Option](#option) is a [monad](#monad)
 * Both `validatePositive` and `safeParseNum` return the same kind of monad (Option).
 * The type of `validatePositive`'s argument matches `safeParseNum`'s unwrapped return.
 

## Applicative Functor

An applicative functor is an object with an `ap` function. `ap` applies a function in the object to a value in another object of the same type.

```js
// Implementation
Array.prototype.ap = function (xs) {
  return this.reduce((acc, f) => acc.concat(xs.map(f)), [])
}

// Example usage
;[(a) => a + 1].ap([1]) // [2]
```

This is useful if you have two objects and you want to apply a binary function to their contents.

```js
// Arrays that you want to combine
const arg1 = [1, 3]
const arg2 = [4, 5]

// combining function - must be curried for this to work
const add = (x) => (y) => x + y

const partiallyAppliedAdds = [add].ap(arg1) // [(y) => 1 + y, (y) => 3 + y]
```

This gives you an array of functions that you can call `ap` on to get the result:

```js
partiallyAppliedAdds.ap(arg2) // [5, 6, 7, 8]
```

## Morphism

A relationship between objects within a [category](#catégorie). In the context of functional programming all functions are morphisms.

### Homomorphism

A function where there is a structural property that is the same in the input as well as the output.

For example, in a [Monoid](#monoid) homomorphism both the input and the output are monoids even if their types are different. 

```js
// toList :: [number] -> string
const toList = (a) => a.join(', ')
```

`toList` is a homomorphism because:
* array is a monoid - has a `concat` operation and an identity value (`[]`)
* string is a monoid - has a `concat` operation and an identity value (`''`)

In this way, a homomorphism relates to whatever property you care about in the input and output of a transformation.

[Endomorphisms](#endomorphism) and [Isomorphisms](#isomorphism) are examples of homomorphisms.

__Further Reading__ 
* [Homomorphism | Learning Functional Programming in Go](https://subscription.packtpub.com/book/application-development/9781787281394/11/ch11lvl1sec90/homomorphism#:~:text=A%20homomorphism%20is%20a%20correspondence,pointing%20to%20it%20from%20A.)



### Endomorphism

A function where the input type is the same as the output. Since the types are identical, endomorphisms are also [homomorphisms](#homomorphism).

```js
// uppercase :: String -> String
const uppercase = (str) => str.toUpperCase()

// decrement :: Number -> Number
const decrement = (x) => x - 1
```

### Isomorphism

A morphism made of a pair of transformations between 2 types of objects that is structural in nature and no data is lost.  

For example, 2D coordinates could be stored as an array `[2,3]` or object `{x: 2, y: 3}`.

```js
// Providing functions to convert in both directions makes the 2D coordinate structures isomorphic.
const pairToCoords = (pair) => ({ x: pair[0], y: pair[1] })

const coordsToPair = (coords) => [coords.x, coords.y]

coordsToPair(pairToCoords([1, 2])) // [1, 2]

pairToCoords(coordsToPair({ x: 1, y: 2 })) // {x: 1, y: 2}
```

Isomorphisms are an interesting example of [morphism](#morphism) because more than single function is necessary for it to be satisfied. Isomorphisms are also [homomorphisms](#homomorphism) since both input and output types share the property of being reversible.

### Catamorphism

A function which deconstructs a structure into a single value. `reduceRight` is an example of a catamorphism for array structures.

```js
// sum is a catamorphism from [Number] -> Number
const sum = xs => xs.reduceRight((acc, x) => acc + x, 0)

sum([1, 2, 3, 4, 5]) // 15
```

### Anamorphism

A function that builds up a structure by repeatedly applying a function to its argument. `unfold` is an example which generates an array by from a function and a seed value. This is the opposite of a [catamorphism](#catamorphism). You can think of this as an anamorphism builds up a structure and catamorphism breaks it down.

```js
const unfold = (f, seed) => {
  function go (f, seed, acc) {
    const res = f(seed)
    return res ? go(f, res[1], acc.concat([res[0]])) : acc
  }
  return go(f, seed, [])
}
```

```js
const countDown = n => unfold((n) => {
  return n <= 0 ? undefined : [n, n - 1]
}, n)

countDown(5) // [5, 4, 3, 2, 1]
```

### Hylomorphism

The function which composes an [anamorphism](#anamorphism) followed by a [catamorphism](#catamorphism).

```js
const sumUpToX = (x) => sum(countDown(x))
sumUpToX(5) // 15
```

### Paramorphism

A function just like `reduceRight`. However, there's a difference:

In paramorphism, your reducer's arguments are the current value, the reduction of all previous values, and the list of values that formed that reduction.

```js
// Obviously not safe for lists containing `undefined`,
// but good enough to make the point.
const para = (reducer, accumulator, elements) => {
  if (elements.length === 0) { return accumulator }

  const head = elements[0]
  const tail = elements.slice(1)

  return reducer(head, tail, para(reducer, accumulator, tail))
}

const suffixes = list => para(
  (x, xs, suffxs) => [xs, ...suffxs],
  [],
  list
)

suffixes([1, 2, 3, 4, 5]) // [[2, 3, 4, 5], [3, 4, 5], [4, 5], [5], []]
```

The third parameter in the reducer (in the above example, `[x, ... xs]`) is kind of like having a history of what got you to your current acc value.

### Apomorphism

The opposite of paramorphism, just as anamorphism is the opposite of catamorphism. With paramorphism, you retain access to the accumulator and what has been accumulated, apomorphism lets you `unfold` with the potential to return early.

## Setoid

An object that has an `equals` function which can be used to compare other objects of the same type.

Make array a setoid:

```js
Array.prototype.equals = function (arr) {
  const len = this.length
  if (len !== arr.length) {
    return false
  }
  for (let i = 0; i < len; i++) {
    if (this[i] !== arr[i]) {
      return false
    }
  }
  return true
}

;[1, 2].equals([1, 2]) // true
;[1, 2].equals([0]) // false
```

## Semigroup

An object that has a `concat` function that combines it with another object of the same type.

```js
;[1].concat([2]) // [1, 2]
```

## Foldable

An object that has a `reduce` function that applies a function against an accumulator and each element in the array (from left to right) to reduce it to a single value.

```js
const sum = (list) => list.reduce((acc, val) => acc + val, 0)
sum([1, 2, 3]) // 6
```

## Lens ##
A lens is a structure (often an object or function) that pairs a getter and a non-mutating setter for some other data
structure.

```js
// Using [Ramda's lens](http://ramdajs.com/docs/#lens)
const nameLens = R.lens(
  // getter for name property on an object
  (obj) => obj.name,
  // setter for name property
  (val, obj) => Object.assign({}, obj, { name: val })
)
```

Having the pair of get and set for a given data structure enables a few key features.

```js
const person = { name: 'Gertrude Blanch' }

// invoke the getter
R.view(nameLens, person) // 'Gertrude Blanch'

// invoke the setter
R.set(nameLens, 'Shafi Goldwasser', person) // {name: 'Shafi Goldwasser'}

// run a function on the value in the structure
R.over(nameLens, uppercase, person) // {name: 'GERTRUDE BLANCH'}
```

Lenses are also composable. This allows easy immutable updates to deeply nested data.

```js
// This lens focuses on the first item in a non-empty array
const firstLens = R.lens(
  // get first item in array
  xs => xs[0],
  // non-mutating setter for first item in array
  (val, [__, ...xs]) => [val, ...xs]
)

const people = [{ name: 'Gertrude Blanch' }, { name: 'Shafi Goldwasser' }]

// Despite what you may assume, lenses compose left-to-right.
R.over(compose(firstLens, nameLens), uppercase, people) // [{'name': 'GERTRUDE BLANCH'}, {'name': 'Shafi Goldwasser'}]
```

Other implementations:
* [partial.lenses](https://github.com/calmm-js/partial.lenses) - Tasty syntax sugar and a lot of powerful features
* [nanoscope](http://www.kovach.me/nanoscope/) - Fluent-interface

## Type Signatures

Often functions in JavaScript will include comments that indicate the types of their arguments and return values.

There's quite a bit of variance across the community, but they often follow the following patterns:

```js
// functionName :: firstArgType -> secondArgType -> returnType

// add :: Number -> Number -> Number
const add = (x) => (y) => x + y

// increment :: Number -> Number
const increment = (x) => x + 1
```

If a function accepts another function as an argument it is wrapped in parentheses.

```js
// call :: (a -> b) -> a -> b
const call = (f) => (x) => f(x)
```

The letters `a`, `b`, `c`, `d` are used to signify that the argument can be of any type. The following version of `map` takes a function that transforms a value of some type `a` into another type `b`, an array of values of type `a`, and returns an array of values of type `b`.

```js
// map :: (a -> b) -> [a] -> [b]
const map = (f) => (list) => list.map(f)
```

__Further reading__
* [Ramda's type signatures](https://github.com/ramda/ramda/wiki/Type-Signatures)
* [Mostly Adequate Guide](https://drboolean.gitbooks.io/mostly-adequate-guide/content/ch7.html#whats-your-type)
* [What is Hindley-Milner?](http://stackoverflow.com/a/399392/22425) on Stack Overflow

## Algebraic data type
A composite type made from putting other types together. Two common classes of algebraic types are [sum](#sum-type) and [product](#product-type).

### Sum type
A Sum type is the combination of two types together into another one. It is called sum because the number of possible values in the result type is the sum of the input types.

JavaScript doesn't have types like this but we can use `Set`s to pretend:
```js
// imagine that rather than sets here we have types that can only have these values
const bools = new Set([true, false])
const halfTrue = new Set(['half-true'])

// The weakLogic type contains the sum of the values from bools and halfTrue
const weakLogicValues = new Set([...bools, ...halfTrue])
```

Sum types are sometimes called union types, discriminated unions, or tagged unions.

There's a [couple](https://github.com/paldepind/union-type) [libraries](https://github.com/puffnfresh/daggy) in JS which help with defining and using union types.

Flow includes [union types](https://flow.org/en/docs/types/unions/) and TypeScript has [Enums](https://www.typescriptlang.org/docs/handbook/enums.html) to serve the same role.

### Product type

A **product** type combines types together in a way you're probably more familiar with:

```js
// point :: (Number, Number) -> {x: Number, y: Number}
const point = (x, y) => ({ x, y })
```
It's called a product because the total possible values of the data structure is the product of the different values. Many languages have a tuple type which is the simplest formulation of a product type.

See also [Set theory](https://en.wikipedia.org/wiki/Set_theory).

## Option
Option is a [sum type](#sum-type) with two cases often called `Some` and `None`.

Option is useful for composing functions that might not return a value.

```js
// Naive definition

const Some = (v) => ({
  val: v,
  map (f) {
    return Some(f(this.val))
  },
  chain (f) {
    return f(this.val)
  }
})

const None = () => ({
  map (f) {
    return this
  },
  chain (f) {
    return this
  }
})

// maybeProp :: (String, {a}) -> Option a
const maybeProp = (key, obj) => typeof obj[key] === 'undefined' ? None() : Some(obj[key])
```
Use `chain` to sequence functions that return `Option`s
```js

// getItem :: Cart -> Option CartItem
const getItem = (cart) => maybeProp('item', cart)

// getPrice :: Item -> Option Number
const getPrice = (item) => maybeProp('price', item)

// getNestedPrice :: cart -> Option a
const getNestedPrice = (cart) => getItem(cart).chain(getPrice)

getNestedPrice({}) // None()
getNestedPrice({ item: { foo: 1 } }) // None()
getNestedPrice({ item: { price: 9.99 } }) // Some(9.99)
```

`Option` is also known as `Maybe`. `Some` is sometimes called `Just`. `None` is sometimes called `Nothing`.

## Function
A **function** `f :: A => B` is an expression - often called arrow or lambda expression - with **exactly one (immutable)** parameter of type `A` and **exactly one** return value of type `B`. That value depends entirely on the argument, making functions context-independent, or [referentially transparent](#referential-transparency). What is implied here is that a function must not produce any hidden [side effects](#effets-de-bord) - a function is always [pure](#fonction-pure), by definition. These properties make functions pleasant to work with: they are entirely deterministic and therefore predictable. Functions enable working with code as data, abstracting over behaviour:

```js
// times2 :: Number -> Number
const times2 = n => n * 2

;[1, 2, 3].map(times2) // [2, 4, 6]
```

## Partial function
A partial function is a [function](#function) which is not defined for all arguments - it might return an unexpected result or may never terminate. Partial functions add cognitive overhead, they are harder to reason about and can lead to runtime errors. Some examples:
```js
// example 1: sum of the list
// sum :: [Number] -> Number
const sum = arr => arr.reduce((a, b) => a + b)
sum([1, 2, 3]) // 6
sum([]) // TypeError: Reduce of empty array with no initial value

// example 2: get the first item in list
// first :: [A] -> A
const first = a => a[0]
first([42]) // 42
first([]) // undefined
// or even worse:
first([[42]])[0] // 42
first([])[0] // Uncaught TypeError: Cannot read property '0' of undefined

// example 3: repeat function N times
// times :: Number -> (Number -> Number) -> Number
const times = n => fn => n && (fn(n), times(n - 1)(fn))
times(3)(console.log)
// 3
// 2
// 1
times(-1)(console.log)
// RangeError: Maximum call stack size exceeded
```

### Dealing with partial functions
Partial functions are dangerous as they need to be treated with great caution. You might get an unexpected (wrong) result or run into runtime errors. Sometimes a partial function might not return at all. Being aware of and treating all these edge cases accordingly can become very tedious.
Fortunately a partial function can be converted to a regular (or total) one. We can provide default values or use guards to deal with inputs for which the (previously) partial function is undefined. Utilizing the [`Option`](#Option) type, we can yield either `Some(value)` or `None` where we would otherwise have behaved unexpectedly:
```js
// example 1: sum of the list
// we can provide default value so it will always return result
// sum :: [Number] -> Number
const sum = arr => arr.reduce((a, b) => a + b, 0)
sum([1, 2, 3]) // 6
sum([]) // 0

// example 2: get the first item in list
// change result to Option
// first :: [A] -> Option A
const first = a => a.length ? Some(a[0]) : None()
first([42]).map(a => console.log(a)) // 42
first([]).map(a => console.log(a)) // console.log won't execute at all
// our previous worst case
first([[42]]).map(a => console.log(a[0])) // 42
first([]).map(a => console.log(a[0])) // won't execte, so we won't have error here
// more of that, you will know by function return type (Option)
// that you should use `.map` method to access the data and you will never forget
// to check your input because such check become built-in into the function

// example 3: repeat function N times
// we should make function always terminate by changing conditions:
// times :: Number -> (Number -> Number) -> Number
const times = n => fn => n > 0 && (fn(n), times(n - 1)(fn))
times(3)(console.log)
// 3
// 2
// 1
times(-1)(console.log)
// won't execute anything
```
Making your partial functions total ones, these kinds of runtime errors can be prevented. Always returning a value will also make for code that is both easier to maintain and to reason about.

## Total Function

A function which returns a valid result for all inputs defined in its type. This is as opposed to [Partial Functions](#partial-function) which may throw an error, return an unexpected result, or fail to terminate.

## Functional Programming Libraries in JavaScript

* [mori](https://github.com/swannodette/mori)
* [Immutable](https://github.com/facebook/immutable-js/)
* [Immer](https://github.com/mweststrate/immer)
* [Ramda](https://github.com/ramda/ramda)
* [ramda-adjunct](https://github.com/char0n/ramda-adjunct)
* [ramda-extension](https://github.com/tommmyy/ramda-extension)
* [Folktale](http://folktale.origamitower.com/)
* [monet.js](https://cwmyers.github.io/monet.js/)
* [lodash](https://github.com/lodash/lodash)
* [Underscore.js](https://github.com/jashkenas/underscore)
* [Lazy.js](https://github.com/dtao/lazy.js)
* [maryamyriameliamurphies.js](https://github.com/sjsyrek/maryamyriameliamurphies.js)
* [Haskell in ES6](https://github.com/casualjavascript/haskell-in-es6)
* [Sanctuary](https://github.com/sanctuary-js/sanctuary)
* [Crocks](https://github.com/evilsoft/crocks)
* [Fluture](https://github.com/fluture-js/Fluture)
* [fp-ts](https://github.com/gcanti/fp-ts)

---

__P.S:__ This repo is successful due to the wonderful [contributions](https://github.com/hemanth/functional-programming-jargon/graphs/contributors)!
