Table des Matières ▼ SuperColliderBrowseSearchIndexes\
SuperColliderBrowseSearchIndexes ▼\
Tutoriels/Démarrage \| Tutoriels \> Démarrage\
\
**05. Fonctions et son**\
**Premiers pas avec SuperCollider\
Voir aussi : 00. Premiers pas avec SC**

\
Qu\'en est-il des fonctions et du son ?\
Je vous ai probablement assez ennuyé avec des détails techniques, alors
revenons à faire du bruit, et je suppose que c\'est la raison pour
laquelle vous lisez ceci après tout. Croyez-moi, tout ce travail portera
ses fruits plus tard, et croyez-le ou non, nous avons déjà couvert une
bonne partie des bases du langage, au moins en passant.\
\
Revenons à notre exemple sonore, ou plutôt à une version légèrement
simplifiée de celui-ci. Vérifiez que le serveur localhost fonctionne,
exécutez le code ci-dessous et appuyez sur Cmd-. lorsque vous en avez
assez.\
{ SinOsc.ar(440, 0, 0.2) }.play ;\
Dans ce cas, nous avons créé une fonction en insérant du code entre
crochets, puis nous avons appelé la méthode \"play\" sur cette fonction.
Pour les fonctions, \"play\" signifie s\'évaluer soi-même et jouer le
résultat sur un serveur. Si vous ne spécifiez pas de serveur, vous
obtiendrez le serveur par défaut, dont vous vous souviendrez qu\'il est
stocké dans la variable \"s\" et qu\'il est défini au démarrage comme
étant le serveur localhost.\
\
Nous n\'avons pas stocké la fonction dans une variable, elle ne peut
donc pas être réutilisée. (En fait, vous pourriez simplement exécuter à
nouveau la même ligne de code, mais vous voyez ce que je veux dire\...).
C\'est souvent le cas lorsqu\'on utilise Function-play, car c\'est un
moyen rapide d\'obtenir quelque chose qui fait du bruit, et c\'est
souvent utilisé à des fins de test. Il y a d\'autres façons de
réutiliser les fonctions pour les sons, qui sont souvent meilleures et
plus efficaces comme nous le verrons.\
\
Regardons ce qu\'il y a entre les crochets. Nous prenons quelque chose
appelé \'SinOsc\' et nous lui envoyons le message ar, avec quelques
arguments. Il s\'avère que SinOsc est un exemple de ce que l\'on appelle
une classe. Pour comprendre ce qu\'est une classe, nous devons en savoir
un peu plus sur la POO et les objets.\
\
En résumé, un objet est constitué de données, c\'est-à-dire
d\'informations, et d\'un ensemble d\'opérations que vous pouvez
effectuer sur ces données. Vous pouvez avoir plusieurs objets différents
du même type. On les appelle des instances. Le type lui-même est la
classe de l\'objet. Par exemple, nous pouvons avoir une classe appelée
Étudiant, et plusieurs instances de cette classe, Bob, Dave et Sue. Les
trois auront les mêmes types de données, par exemple une donnée appelée
gpa. La valeur de chaque donnée peut cependant être différente. Ils
disposent également des mêmes méthodes pour traiter les données. Par
exemple, ils pourraient avoir une méthode appelée calculateGPA, ou
quelque chose de similaire.\
\
La classe d\'un objet définit son ensemble de données (ou variables
d\'instance, comme on les appelle) et de méthodes. En outre, elle peut
définir d\'autres méthodes qui ne sont envoyées qu\'à la classe
elle-même, ainsi que certaines données qui seront utilisées par toutes
ses instances. Ces méthodes et variables sont appelées \"méthodes de
classe\" et \"variables de classe\".\
\
Toutes les classes commencent par des lettres majuscules, ce qui permet
de les identifier facilement dans le code.\
\
Les classes sont utilisées pour créer des objets. Elles sont comme un
modèle. Pour ce faire, vous utilisez des méthodes de classe telles que
\"new\" ou, dans le cas de notre classe SinOsc ci-dessus, \"ar\". Ces
méthodes renvoient un objet, une instance, et les arguments affectent
ses données et son comportement. Reprenons maintenant l\'exemple en
question :

\
SinOsc.ar(440, 0, 0.2)

\
Cela indique à la classe SinOsc de créer une instance d\'elle-même. Tous
les SinOsc sont un exemple de ce que l\'on appelle des générateurs
d\'unités, ou UGens. Il s\'agit d\'objets qui produisent des signaux
audio ou de contrôle. SinOsc est un oscillateur sinusoïdal. Cela
signifie qu\'il produit un signal composé d\'une seule fréquence. Un
graphique de sa forme d\'onde ressemblerait à ceci :

![](Pictures/10000001000000B3000000B1B2A4EB029670EDAF.png){width="3.948cm"
height="3.902cm"}

\
\
\
(ne vous préoccupez pas de l\'index et de la valeur, ce n\'est pas
important pour l\'instant).\
Cette forme d\'onde tourne en boucle, créant ainsi le signal de sortie.
ar\' signifie \'make the instance audio rate\' (faire le taux audio de
l\'instance). SuperCollider calcule l\'audio par groupes
d\'échantillons, appelés blocs. Le taux audio signifie que l\'UGen
calculera une valeur pour chaque échantillon du bloc. Il existe une
autre méthode, \'kr\', qui signifie \'control rate\' (taux de contrôle).
Cela signifie qu\'une seule valeur est calculée pour chaque bloc
d\'échantillons. Cette méthode permet d\'économiser beaucoup de
puissance de calcul et convient parfaitement aux signaux qui contrôlent
d\'autres UGen, mais elle n\'est pas assez détaillée pour synthétiser
des signaux audio.\
\
Les trois arguments de SinOsc-ar donnés dans l\'exemple déterminent un
certain nombre de choses sur l\'instance résultante. Il se trouve que je
sais que les arguments sont la fréquence, la phase et mul. (La fréquence
est simplement la fréquence de l\'oscillateur en Hertz (Hz), ou cycles
par seconde (cps). La phase fait référence à l\'endroit où
l\'oscillateur commence le cycle de sa forme d\'onde. Pour SinOsc (mais
pas pour tous les UGens), la phase est donnée en radians. Si vous ne
savez pas ce que sont les radians, ne vous inquiétez pas, comprenez
simplement qu\'il s\'agit d\'une valeur comprise entre 0 et 2 \* pi.
(Vous pouvez consulter un texte de trigonométrie si vous voulez vraiment
plus de détails.) Donc, si nous créons un SinOsc avec une phase de (pi
\* 0,5), ou un quart de son cycle, la forme d\'onde ressemblerait à ceci
:

![](Pictures/10000001000000C0000000B8818A23F31AA76CC2.png){width="4.233cm"
height="4.057cm"}

\
\
\

Vous comprenez ? Voici plusieurs cycles des deux côte à côte pour rendre
l\'idée plus claire :

![](Pictures/100000010000022B0000014D54E539343B799E2E.png){width="12.238cm"
height="7.341cm"}

\
\
\

Qu\'en est-il de \"mul\" ? Mul est un argument spécial que presque tous
les UGens possèdent. Il est tellement omniprésent qu\'il n\'est
généralement même pas expliqué dans la documentation. Il s\'agit
simplement d\'une valeur ou d\'un signal par lequel la sortie de l\'UGen
sera multipliée. Il s\'avère que dans le cas des signaux audio, cela
affecte l\'amplitude du signal, ou son niveau sonore. Le mul par défaut
de la plupart des UGen est 1, ce qui signifie que le signal oscillera
entre 1 et -1. Il s\'agit d\'une bonne valeur par défaut, car toute
valeur supérieure entraînerait un écrêtage et une distorsion. Un mul de
0 serait effectivement silencieux, comme si le bouton de volume était
tourné à fond.\
\
Pour rendre plus clair le fonctionnement du mul, voici un graphique de
deux SinOscs, l\'un avec le mul par défaut de 1, et l\'autre avec un mul
de 0,25 :

![](Pictures/100000010000022B0000014D301832657DF0EDD8.png){width="12.238cm"
height="7.341cm"}

\
\
\

Vous voyez ce que je veux dire ? Il y a aussi un autre arg similaire
appelé \'add\' (aussi généralement inexpliqué dans la doc), qui (vous
l\'avez deviné) est quelque chose qui est ajouté au signal de sortie.
Cela peut être très utile pour des choses comme les signaux de contrôle.
\'add\' a une valeur par défaut de 0, c\'est pourquoi nous n\'avons pas
besoin de spécifier quelque chose pour cela.\
\

Bon, avec tout cela à l\'esprit, revoyons notre exemple, avec des
commentaires :\
(\
{ // Ouvrir la fonction\
SinOsc.ar( // Créer un taux audio SinOsc\
440, // fréquence de 440 Hz, ou l\'accord A\
0, // phase initiale de 0, ou début du cycle\
0.2) // mul de 0.2\
}.play ; // fermer la fonction et appeler \'play\' dessus\
)\
Un peu plus de plaisir avec les fonctions et les UGens\
Voici un autre exemple de polymorphisme et de sa puissance. Lorsque vous
créez des fonctions d\'UGens, vous n\'êtes pas obligé d\'utiliser des
valeurs fixes pour de nombreux arguments, vous pouvez en fait utiliser
d\'autres UGens ! Voici un exemple qui le démontre :\
(\
{ var ampOsc ;\
ampOsc = SinOsc.kr(0,5, 1,5pi, 0,5, 0,5) ;\
SinOsc.ar(440, 0, ampOsc) ;\
}.play ;\
)\
Essayez ceci. (Encore une fois, utilisez Cmd-. pour arrêter le son).\
\
Ce que nous avons fait ici, c\'est brancher le premier SinOsc (un taux
de contrôle !) dans l\'arg mul du second. Ainsi, sa sortie est
multipliée par la sortie du second. Regardons maintenant les arguments
du premier SinOsc.\
\
La fréquence est fixée à 0,5 cps, ce qui, si l\'on y réfléchit un peu,
signifie qu\'il effectuera un cycle toutes les 2 secondes. (1 / 0.5 =
2)\
\
Mul et add sont tous deux réglés sur 0,5. Réfléchissez un instant à ce
que cela va donner. Si par défaut SinOsc est compris entre 1 et -1, un
mul de 0,5 le ramènera à une valeur comprise entre 0,5 et -0,5. Si l\'on
ajoute 0,5 à cette valeur, on obtient une valeur comprise entre 0 et 1,
ce qui est une bonne plage pour un mul !\
\
La phase de 1,5pi (ce qui signifie simplement 1,5 \* pi) correspond aux
3/4 du cycle, ce qui, si vous regardez le premier graphique ci-dessus,
correspond au point le plus bas, ou dans ce cas, à 0. La forme d\'onde
de l\'ampOsc SinOsc ressemblera donc à ceci :\
\
\
Au final, nous obtenons un SinOsc qui s\'estompe doucement. Le déphasage
signifie simplement que nous commençons doucement et que nous entrons en
fondu. Nous utilisons effectivement ampOsc comme ce que l\'on appelle
une enveloppe d\'amplitude. Il existe d\'autres façons de faire la même
chose, certaines plus simples, mais celle-ci démontre le principe.\
\
L\'assemblage d\'UGens de cette façon est la méthode de base pour créer
du son dans SC. Pour une vue d\'ensemble des différents types d\'UGens
disponibles dans SC, voir Browse : UGens ou Tour of UGens.\
\
Pour plus d\'informations, voir :\
\
Fonctions, Fonction, Parcourir : UGens Visite des UGens\
\
Exercice suggéré\
Essayez de modifier les fonctions dans le texte ci-dessus. Par exemple,
essayez de changer les fréquences du SinOsc, ou de créer des versions
multicanaux.\
\
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\
\
Ce document fait partie du tutoriel \"Getting Started With
SuperCollider\".\
\
Cliquez ici pour passer à la section suivante : 06. Présentation en
stéréo vivante\
\
Cliquez ici pour revenir à la table des matières : 00. Démarrer avec SC\
\
Source du fichier d\'aide : C:\\NProgram
Files\\NSuperCollider-3.13.0\\NHelpSource\\NTutorials\\NGetting-Started\\N05-Functions-and-Sound.schelp\
link::Tutorials/Getting-Started/05-Functions-and-Sound: :
