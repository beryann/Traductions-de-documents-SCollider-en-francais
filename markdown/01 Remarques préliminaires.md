Tutoriels/Démarrage \| Tutoriels \> Démarrage

01\. Remarques préliminaires

Premiers pas avec SuperCollider

Voir aussi : 00. Premiers pas avec SC

Le texte suivant est destiné à servir d\'introduction à SuperCollider 3,
un langage orienté objet pour la synthèse sonore et le traitement
numérique du signal (DSP). Ce tutoriel ne présuppose pas de
connaissances en informatique, mais suppose une familiarité de base avec
votre ordinateur et son système d\'exploitation, ainsi qu\'une
connaissance de base de l\'acoustique et de l\'audio numérique. (Je
suppose ici que des mots comme fréquence et échantillon ne prêteront pas
à confusion).

Le tutoriel est écrit du point de vue de macOS, mais une grande partie
devrait s\'appliquer à Linux et Windows également. Les parties qui
diffèrent spécifiquement ont principalement trait aux aspects de
l\'interface utilisateur graphique (GUI).

Je dois reconnaître que ce tutoriel n\'a été rédigé par moi que dans un
sens limité. Pour le rédiger, je me suis librement inspiré de la
documentation générale, qui a été rédigée par un certain nombre de
personnes. Ce document n\'a pas pour but de remplacer ces sources
(souvent plus détaillées) et renvoie constamment le lecteur à celles-ci
pour de plus amples informations.

Une liste complète des personnes ayant contribué à SuperCollider et à sa
documentation peut être consultée à l\'adresse suivante :

https://supercollider.github.io

Combinaisons de touches

Dans ce texte, vous verrez des expressions telles que \"Ctrl-Enter\" et
\"Cmd-Enter\". Il s\'agit de combinaisons de touches. Elles signifient :
maintenez la première touche enfoncée, puis appuyez sur la deuxième
touche (tout en maintenant la première enfoncée).

Par exemple, \"Ctrl-Enter\" est une combinaison de touches :
\"Ctrl-Enter\" est une combinaison de la touche \"Ctrl\" et de la touche
\"Enter\". Cela signifie : appuyez sur la touche \"Ctrl\" et
maintenez-la enfoncée, puis, tout en maintenant la touche \"Ctrl\"
enfoncée, appuyez sur la touche \"Entrée\". Une fois les deux touches
enfoncées en même temps, vous pouvez les relâcher.

\"Ctrl\" est une touche que l\'on trouve sur la plupart des ordinateurs
Windows et Linux. Les ordinateurs Mac ont une touche \"Ctrl\", mais
lorsque plusieurs combinaisons de touches sont données pour la même
instruction, celle qui commence par \"Cmd\" est destinée aux
utilisateurs Mac. \"Cmd\" est l\'abréviation de \"command\" et
correspond à la touche portant le symbole \"⌘\".

Pour les combinaisons de touches comprenant 3 touches ou plus, maintenez
toutes les touches enfoncées sauf la dernière, puis appuyez sur la
dernière touche. Par exemple, \"Ctrl-Shift-P\" signifie qu\'il faut
d\'abord maintenir enfoncées les touches \"Ctrl\" et \"Shift\", puis
appuyer sur la touche \"P\" tout en maintenant \"Ctrl\" et \"Shift\"
enfoncées.

Liens

Dans le texte, et à la fin de chaque section, il peut y avoir une liste
de liens vers d\'autres documents, qui ressemblera à ceci :

Voir aussi : 01. Remarques introductives : Liens

La plupart de ces liens sont destinés à développer ce que vous venez de
lire, mais certains vous orientent simplement vers d\'autres
informations dont vous aurez probablement besoin à l\'avenir. Certains
des documents liés sont rédigés dans un langage assez technique et
peuvent reproduire des informations présentées dans ce didacticiel sous
une forme plus informelle. Ils sont souvent conçus comme des documents
de référence pour les personnes déjà familiarisées avec SC, ne vous
inquiétez donc pas si tout ce qu\'ils contiennent n\'est pas
immédiatement compréhensible. Il n\'est pas nécessaire de les avoir vus
et/ou compris pour continuer ce tutoriel.

Exemples de code

Les exemples de code dans le texte sont dans une police différente :

{ \[SinOsc.ar(440, 0, 0.2), SinOsc.ar(442, 0, 0.2)\] }.play ;

Il s\'agit d\'une convention courante dans la documentation des langages
informatiques, qui est suivie tout au long de la documentation de SC.
Les différentes couleurs que vous verrez dans le code ne servent qu\'à
rendre les choses plus claires et n\'ont aucun effet sur ce que fait le
code.

Nous vous encourageons à copier les exemples de code dans une autre
fenêtre et à vous amuser à les modifier. C\'est une façon traditionnelle
d\'apprendre un nouveau langage informatique ! SC vous permettra de
modifier les documents originaux du didacticiel, mais si vous le faites,
veillez à ne pas les sauvegarder (par exemple si on vous le demande
lorsque vous les fermez). Il est plus sûr de copier les choses dans un
nouveau document avant de les modifier.

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Ce document fait partie du tutoriel \"Getting Started With
SuperCollider\".

Cliquez ici pour passer à la section suivante : 02. Premiers pas
