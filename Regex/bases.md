Les Bases Regex
-


    a — correspond à un caractère qui doit être un a (ni b, ni aa, etc.)
    abc — correspond à a, suivi de b, suivi de c.
    a* — correspond au caractère a, absent ou présent plusieurs fois (+ correspond à un caractère une ou plusieurs fois).
    [^a] — correspond à un caractère qui n'est pas un a.
    a|b — correspond à un caractère qui est a ou b.
    [abc] — correspond à un caractère qui est a, b ou c.
    [^abc] — correspond à un caractère qui n'est pas a, b ou c.
    [a-z] — correspond à tout caractère de la plage a–z, en minuscules seulement (utilisez [A-Za-z] pour minuscules et majuscules et [A-Z] pour les majuscules uniquement).
    a.c — correspond à a, suivi par n'importe quel caractère,suivi par c.
    a{5} — correspond à a, 5 fois.
    a{5,7} — correspond à  a, 5 à 7 fois, mais ni plus, ni moins.

Vous pouvez utiliser des nombres ou d'autres caractères dans ces expressions, comme :

    [ -] — correspond à une espace ou un tiret.
    [0-9] — correspond à tout nombre compris entre 0 et 9.

Vous pouvez combiner cela pratiquement comme vous l'entendez en précisant les différentes parties les unes après les autres :

    [Ll].*k — Un seul caractère L en majuscules ou minuscules, suivi de zéro ou plusieurs caractères de n'importe quel type, suivis par un k minuscules.
    [A-Z][A-Za-z' -]+ — Un seul caractère en majuscules suivi par un ou plusieurs caractères en majuscules ou minuscules, un tiret, une apostrophe ou une espace. Cette combinaison peut s'utiliser pour valider les nom de villes dans les pays anglo‑saxons ; ils débutent par une majuscule et ne contiennent pas d'autre caractère. Quelques exemples de ville de GB correspondant à ce schéma : Manchester, Ashton-under-lyne et Bishop's Stortford.
    [0-9]{3}[ -][0-9]{3}[ -][0-9]{4} — Un schéma pour un numéro de téléphone intérieur américain — trois chiffres, suivis par une espace ou un tiret, suivis par trois nombres, suivis par une espace ou un tiret, suivis par quatre nombres. Vous aurez peut-être à faire plus compliqué, car certains écrivent leur numéro de zone entre parenthèses, mais ici il s'agit simplement de faire une démonstration.
