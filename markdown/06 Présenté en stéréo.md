Table des Matières ▼ SuperColliderBrowseSearchIndexes

SuperColliderBrowseSearchIndexes ▼

Tutoriels/Démarrage \| Tutoriels \> Démarrage

06\. Présenté en stéréo

Démarrer avec SuperCollider

Voir aussi : 00. Débuter avec SC

D\'accord, mais qu\'en est-il de notre premier exemple non simplifié ?
Rappelez-vous :

{ \[SinOsc.ar(440, 0, 0.2), SinOsc.ar(442, 0, 0.2)\] }.play ;

Il s\'agit également de deux SinOsc, mais dans une disposition
différente, entre deux crochets \[\], et avec une virgule entre les
deux. Tout comme les crochets frisés indiquent une fonction, les
crochets carrés définissent ce que l\'on appelle un tableau. Un tableau
est un type de collection, qui est (vous l\'avez deviné) une collection
d\'objets. Les collections elles-mêmes sont des objets, et la plupart
des types de collections peuvent contenir n\'importe quel type
d\'objets, mélangés les uns aux autres, y compris d\'autres collections
! Il existe de nombreux types de collections dans SC, et vous apprendrez
qu\'elles constituent l\'une des fonctionnalités les plus puissantes de
SC.

Un tableau est un type particulier de collection : Il s\'agit d\'une
collection ordonnée dont la taille maximale est limitée. Vous pouvez en
créer un comme nous l\'avons fait ci-dessus, en plaçant des objets entre
deux crochets, avec des virgules entre les deux. Vous pouvez obtenir les
différents éléments d\'un tableau en utilisant la méthode \"at\", qui
prend un index comme argument. Les indices correspondent à l\'ordre des
objets dans le tableau et commencent à 0.

a = \[\"foo\", \"bar\"\] ; // \"foo\" est à l\'indice 0 ; \"bar\" est à
l\'indice 1

a.at(0) ;

a.at(1) ;

a.at(2) ; // renvoie \"nil\", car il n\'y a pas d\'objet à l\'index 2

// il y a une abréviation pour at que vous verrez parfois :

a\[0\] ; // identique à a.at(0) ;

En plus d\'être utilisés pour contenir des collections d\'objets, les
tableaux ont également une utilisation spéciale dans SC : ils sont
utilisés pour implémenter l\'audio multicanal ! Si votre fonction
renvoie un tableau d\'UGens (rappelez-vous que les fonctions renvoient
le résultat de leur dernière ligne de code), le résultat sera un certain
nombre de canaux. Le nombre dépend de la taille du tableau, et chaque
canal correspond à un élément du tableau. Ainsi, dans notre exemple :

{ \[SinOsc.ar(440, 0, 0.2), SinOsc.ar(442, 0, 0.2)\] }.play ;

Nous obtenons une sortie stéréo, avec un SinOsc à 440 Hz dans le canal
gauche et un SinOsc à 442 Hz dans le canal droit. Nous pourrions avoir
encore plus de canaux de sortie en utilisant un tableau plus grand.

Maintenant, regardez attentivement, parce que la partie suivante
implique un petit tour de passe-passe, mais montre une autre façon dont
SC rend les choses très interchangeables. Parce que les arguments pour
la phase et le mul sont les mêmes pour les deux SinOsc, nous pouvons
réécrire le code de notre exemple comme suit :

{ SinOsc.ar(\[440, 442\], 0, 0.2) }.play ;

Nous avons remplacé l\'argument de fréquence par un tableau. Cela
provoque ce que l\'on appelle une \"expansion multicanal\", ce qui
signifie que si vous insérez un tableau dans l\'un des arguments d\'un
UGen, vous obtenez un tableau de cet UGen au lieu d\'un seul.
Considérons maintenant ceci :

(

{ var freq ;

freq = \[\[660, 880\], \[440, 660\], 1320, 880\].choose ;

SinOsc.ar(freq, 0, 0.2) ;

}.play ;

)

Essayez de l\'exécuter plusieurs fois et vous obtiendrez des résultats
différents. choose\" est simplement une méthode qui sélectionne au
hasard l\'un des éléments du tableau. Dans ce cas, le résultat peut être
un simple nombre ou un autre tableau. Dans ce dernier cas, vous
obtiendrez une sortie stéréo, dans le premier cas, une sortie
monophonique. Ce genre de choses peut rendre votre code très flexible.

Mais que se passe-t-il si vous voulez faire un panoramique, un fondu
enchaîné entre les canaux ? SC dispose d\'un certain nombre d\'UGens qui
font cela de différentes manières, mais pour l\'instant je vais juste
vous en présenter un : Pan2. Pan2 prend une entrée et une position comme
arguments et retourne un tableau de deux éléments, le canal gauche et le
canal droit ou le premier et le second canal. L\'argument position est
compris entre -1 (gauche) et 1 (droite). Voici un exemple :

{ Pan2.ar(PinkNoise.ar(0.2), SinOsc.kr(0.5)) }.play ;

Ceci utilise un SinOsc pour contrôler la position (rappelez-vous qu\'il
fournit des valeurs de -1 à 1, ou de gauche à droite), mais utilise un
UGen différent comme entrée du Pan2, quelque chose appelé PinkNoise.
C\'est juste une sorte de générateur de bruit, et il a un seul argument
: mul. Vous pouvez bien sûr utiliser des valeurs fixes pour l\'argument
position.

{ Pan2.ar(PinkNoise.ar(0.2), -0.3) }.play ; // légèrement vers la gauche

Pour plus d\'informations, voir :

Expansion multicanal, Collections, Pan2

Exercice suggéré

Essayez de modifier les fonctions dans le texte ci-dessus. Par exemple,
essayez de changer les fréquences du SinOsc, ou de créer des versions
multicanaux.

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Ce document fait partie du didacticiel Premiers pas avec SuperCollider.

Cliquez ici pour passer à la section suivante : 07. Mélanger les choses

Cliquez ici pour revenir à la table des matières : 00. Démarrer avec SC

Source du fichier d\'aide : C:\\NProgram
Files\\NSuperCollider-3.13.0\\NHelpSource\\NTutorials\\NGetting-Started\\06-Presented-in-Living-Stereo.schelp

link::Tutorials/Getting-Started/06-Presented-in-Living-Stereo: :
