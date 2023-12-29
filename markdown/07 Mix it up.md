Table des Matières ▼ SuperColliderBrowseSearchIndexes\
SuperColliderBrowseSearchIndexes ▼\
Tutoriels/Démarrage \| Tutoriels \> Démarrage\
07. Mélangez-les\
Débuter avec SuperCollider\
Voir aussi : 00. Débuter avec SC\
Nous avons déjà vu que la multiplication modifie le niveau de quelque
chose, mais qu\'en est-il du mélange d\'UGens ? Cela s\'avère tout aussi
simple. Tout ce dont nous avons besoin, c\'est d\'une addition :\
{ PinkNoise.ar(0.2) + SinOsc.ar(440, 0, 0.2) + Saw.ar(660, 0.2) }.play
;\
Saw est un autre type d\'oscillateur, dont la forme d\'onde ressemble à
une dent de scie. Notez que nous utilisons une valeur faible pour mul,
ce qui garantit que la sortie finale sera comprise entre -1 et 1, et non
pas clipée.\
\
Il existe une autre classe pratique appelée Mix, qui permet de mixer un
tableau de canaux en un seul canal ou un tableau de tableaux de canaux
en un seul tableau de canaux. Regardez la fenêtre d\'affichage pour voir
les résultats de Mix.\
// un canal\
{ Mix.new(\[SinOsc.ar(440, 0, 0.2), Saw.ar(660, 0.2)\]).postln }.play ;\
\
// combinaison de deux tableaux stéréo\
(\
{\
var a, b ;\
a = \[SinOsc.ar(440, 0, 0.2), Saw.ar(662, 0.2)\] ;\
b = \[SinOsc.ar(442, 0, 0.2), Saw.ar(660, 0.2)\] ;\
Mix(\[a, b\]).postln ;\
}.play ;\
)\
Dans le premier cas, nous obtenons un \"BinaryOpUGen\" (dans ce cas, il
s\'agit des deux UGens additionnés), et dans le second, nous obtenons un
tableau de deux BinaryOpUGen.\
\
Notez que dans le premier exemple, nous utilisons Mix.new(\...), mais
dans le second, nous utilisons Mix(\...). Ce dernier est une abréviation
du premier. new\" est la méthode de classe la plus courante pour créer
un nouvel objet. Dans certains cas, les objets ont plus d\'une méthode
de classe pour créer des objets, comme les méthodes \'ar\' et \'kr\' de
UGens (Mix, cependant, n\'est en fait qu\'une classe de \'commodité\' et
ne crée pas réellement d\'objets Mix, elle renvoie simplement les
résultats de son addition, soit un BinaryOpUGen, soit un tableau de ces
objets).\
\
Mix possède également une autre méthode de classe appelée fill, qui
prend deux arguments. Le premier est un nombre, qui détermine combien de
fois le second argument, une fonction, sera évalué. Les résultats des
évaluations seront additionnés. Vous êtes confus ? Regardez l\'exemple
suivant :\
(\
var n = 8 ;\
{ Mix.fill(n, { SinOsc.ar(500 + 500.0.rand, 0, 1 / n) }) }.play ;\
)\
La fonction sera évaluée n fois, créant à chaque fois un SinOsc avec une
fréquence aléatoire de 500 à 1000 Hz (500 plus un nombre aléatoire entre
0 et 500). Le mul arg de chaque SinOsc est fixé à 1 / n, garantissant
ainsi que l\'amplitude totale ne sera pas comprise entre -1 et 1. En
changeant simplement la valeur de n, vous pouvez avoir des nombres de
SinOsc très différents ! (Essayez !) Ce type d\'approche rend ce code
extrêmement flexible et réutilisable.\
\
Chaque fois que la fonction est évaluée, le nombre de fois qu\'elle a
été évaluée jusqu\'à présent lui est transmis en tant qu\'argument.
Ainsi, si \"n\" est égal à 8, la fonction se verra passer les valeurs de
0 à 7, dans l\'ordre, en comptant à rebours. En déclarant un argument
dans notre fonction, nous pouvons utiliser cette valeur.\
// Regardez la fenêtre de poste pour les fréquences et les indices\
(\
var n = 8 ;\
{\
Mix.fill(n, { arg index ;\
var freq ;\
index.postln ;\
freq = 440 + index ;\
freq.postln ;\
SinOsc.ar(freq , 0, 1 / n)\
})\
}.play ;\
)\
En combinant l\'addition et la multiplication (ou toute autre procédure
mathématique imaginable) avec l\'utilisation de classes telles que Mix,
nous disposons des outils nécessaires pour combiner des sources sonores
multicanaux dans des mixages et des sous-mixages complexes.\
\
Pour plus d\'informations, voir\
\
Mix, BinaryOpUGen, Opérateurs, Raccourcis syntaxiques\
\
Exercice suggéré\
Essayez de modifier les fonctions du texte ci-dessus. Essayez par
exemple de modifier les fréquences du SinOsc ou de créer des versions
multicanaux.\
\
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\
\
Ce document fait partie du didacticiel Premiers pas avec SuperCollider.\
\
Cliquez ici pour passer à la section suivante : 08. Détermination du
champ d\'application et traçage\
\
Cliquez ici pour revenir à la table des matières : 00. Démarrer avec SC\
\
Source du fichier d\'aide : C:\\NProgram
Files\\NSuperCollider-3.13.0\\NHelpSource\\NTutorials\\NPrise en
main\\N07-Mix-it-Up.schelp\
link::Tutorials/Getting-Started/07-Mix-it-Up: :
