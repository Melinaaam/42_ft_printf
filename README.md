# Ft_printf
_Because ft_putnbr() and ft_putstr() aren’t enough_

**Objectif :** Créer une version simplifiée de la fonction `printf()` de la bibliothèque standard C.

## 1. Comprendre le sujet

Le but de ce projet est de coder une fonction `ft_printf()` capable d’afficher du texte formaté, en gérant plusieurs **spécificateurs de conversion** :

| Spécificateur | Signification              |
| ------------- | -------------------------- |
| `%c`          | Caractère                  |
| `%s`          | Chaîne de caractères       |
| `%p`          | Pointeur                   |
| `%d` ou `%i`  | Nombre entier (base 10)    |
| `%u`          | Nombre non signé (base 10) |
| `%x`          | Hexadécimal minuscule      |
| `%X`          | Hexadécimal majuscule      |
| `%%`          | Affichage du caractère `%` |

En plus de réaliser l’affichage, la fonction doit **retourner un entier** correspondant au nombre de caractères effectivement imprimés.  
Le défi réside principalement dans la gestion dynamique des arguments et dans le traitement approprié de chaque spécificateur.

---
## 2. Gestion des arguments

### 2.1 Initialisation

Pour gérer les arguments variables passés à `ft_printf()`, on utilise `va_list` :

va_list args;
va_start(args, format);
`format` est la chaîne de format (ex. "Hello %s").

### 2.2 Extraction et affichage
On parcourt la chaîne de format et, à chaque fois qu’on rencontre %, on identifie le type de conversion correspondant. 
Ensuite :
* On récupère l’argument à l’aide de va_arg(args, type).
* On l’affiche selon le spécificateur (par exemple, %c, %s, etc.).
* On compte le nombre de caractères écrits.

Une fois la lecture de tous les arguments terminée, on libère la liste :
va_end(args);

---
## 3. Implémentation des conversions

Chaque conversion doit être gérée séparément. Une bonne approche est de créer une fonction pour chaque type de conversion :

- `int	ft_putchar(char c)`
- `int	ft_putstr(char *str)`
- `int	ft_print_ptr(char *base, void *ptr, size_t len)`
- `int	ft_putnbr(int nbr)`
- `int	ft_putnbr_base(char *base, int nbr, int len)`
- etc.


L’important est de toujours retourner le nombre de caractères écrits pour maintenir un comptage précis dans ft_printf().

## 4. Mise en forme du code

Globalement, une fois ces fonctions centrales implementees, on ptu se retrouver avec quelque chose comme :

| Spécificateur | Corresponding function     |
| ------------- | -------------------------- |
| `%c`          | `ft_putchar(va_arg(args, int)` |
| `%s`          | `ft_putstr(va_arg(args, char *)` |
| `%p`          | `ft_print_ptr(HEX_BASE_MIN, va_arg(args, void *), 16)` |
| `%d` ou `%i`  | `ft_putnbr(va_arg(args, int)` |
| `%u`          | `ft_putnbr_base(DEC_BASE, va_arg(args, int), 10)` |
| `%x`          | `ft_putnbr_base(HEX_BASE_MIN, va_arg(args, int), 16)` |
| `%X`          | `ft_putnbr_base(HEX_BASE_MAJ, va_arg(args, int), 16)` |
| `%%`          | `ft_putchar('%')` |
