Table des Matières ▼ SuperColliderBrowseSearchIndexes\
SuperColliderBrowseSearchIndexes ▼\
Tutoriels / Mise en route \| Tutoriels \> Mise en route\
10. SynthDefs et Synths\
Démarrer avec SuperCollider\
Voir aussi : 00. Débuter avec SC\
Maintenant que nous avons couvert quelques informations de base, nous
allons commencer à regarder les abstractions du serveur, qui sont les
différentes classes du langage app qui représentent des choses sur le
serveur. Il est important de comprendre que ces objets ne sont que des
représentations côté client de certaines parties de l\'architecture du
serveur, et qu\'il ne faut pas les confondre avec ces parties
elles-mêmes. Les objets d\'abstraction du serveur sont simplement des
commodités.\
\
La distinction entre les deux peut être un peu déroutante, c\'est
pourquoi je me réfère généralement aux classes côté client avec des noms
en majuscules, et aux aspects correspondants de l\'architecture du
serveur avec des noms en minuscules, c\'est-à-dire Synth vs. synth.\
\
Vous avez déjà rencontré un type d\'abstraction de serveur, la classe
Server elle-même. Les objets référencés par Server.local et
Server.internal (et celui qui est stocké dans la variable \'s\' de
l\'interpréteur à un moment donné) sont des instances de Server.\
\
Il est maintenant temps de se familiariser avec les autres. La première
chose que nous allons examiner est la classe SynthDef, qui est
l\'abréviation de \"définition de synthé\".\
\
Présentation de la classe SynthDef\
Jusqu\'à présent, nous avons utilisé des fonctions pour générer de
l\'audio. Cette façon de travailler est très utile pour les tests
rapides et dans les cas où une flexibilité maximale est nécessaire. En
effet, à chaque fois que nous exécutons le code, la fonction est évaluée
à nouveau, ce qui signifie que les résultats peuvent varier
considérablement.\
\
Le serveur, cependant, ne comprend pas les fonctions, ni la POO, ni le
langage SC. Il veut des informations sur la manière de créer une sortie
audio sous une forme spéciale appelée définition de synthé. Une
définition de synthé est une donnée sur les UGens et la manière dont ils
sont interconnectés. Ces données sont envoyées sous une forme spéciale
optimisée, appelée \"code d\'octets\", que le serveur peut traiter très
efficacement.\
\
Une fois que le serveur dispose d\'une définition de synthé, il peut
l\'utiliser de manière très efficace pour créer un certain nombre de
synthés basés sur cette définition. Sur le serveur, les synthés sont
essentiellement des éléments qui produisent ou traitent des sons, ou qui
produisent des signaux de commande pour piloter d\'autres synthés.\
\
Cette relation entre les définitions de synthés et les synthés est un
peu comme celle qui existe entre les classes et les instances, en ce
sens que la première est un modèle pour la seconde. Mais n\'oubliez pas
que l\'application serveur ne connaît rien à la POO.\
\
Heureusement pour nous, il existe des classes dans le langage telles que
SynthDef, qui facilitent la création du code d\'octets nécessaire et son
envoi au serveur, ainsi que le traitement des définitions de synthés
d\'une manière orientée objet.\
\
Chaque fois que vous utilisez l\'une des méthodes de création audio de
Function, une instance correspondante de SynthDef est créée \"en
coulisse\", pour ainsi dire, et le code byte nécessaire est généré et
envoyé au serveur, où un synthé est créé pour jouer l\'audio désiré. Les
méthodes audio de Function sont donc une sorte de commodité pour vous,
afin que vous n\'ayez pas à vous en occuper.\
\
Comment créer un SynthDef soi-même ? Vous utilisez sa \"nouvelle\"
méthode. Comparons un exemple familier basé sur Function, et créons un
SynthDef équivalent. Comme Function, SynthDef a aussi une méthode play
pratique, donc nous pouvons facilement confirmer que ces deux sont
équivalents.\
//d\'abord la Fonction\
{ SinOsc.ar(440, 0, 0.2) }.play ;\
\
// voici maintenant un SynthDef équivalent\
SynthDef.new(\"tutorial-SinOsc\", { \|out\| Out.ar(out, SinOsc.ar(440,
0, 0.2)) }).play ;\
SynthDef-new prend un certain nombre d\'arguments. Le premier est un
nom, généralement sous la forme d\'une chaîne de caractères comme
ci-dessus. Le second est en fait une fonction. Cet argument est appelé
fonction de graphe UGen, car il indique au serveur comment connecter ses
différents UGen.\
\
NOTE : Dans les accolades de la fonction, l\'argument \|out\| définit
une entrée de contrôle SynthDef, qui est ensuite utilisée comme première
entrée de Out.ar. C\'est une bonne habitude de fournir un contrôle out
dans chaque SynthDef. Voir 04. Fonctions et autres fonctionnalités pour
plus d\'informations sur les arguments des fonctions.\
SynthDefs vs. fonctions\
Cette fonction UGen Graph que nous avons utilisée dans le second exemple
ci-dessus est similaire à la fonction que nous avons utilisée dans le
premier, mais avec une différence notable : Elle possède un UGen
supplémentaire appelé Out. Out écrit un signal ar ou kr sur l\'un des
bus du serveur, que l\'on peut considérer comme des canaux de mixage ou
des sorties. Nous aborderons les bus plus en détail ultérieurement, mais
pour l\'instant, sachez qu\'ils sont utilisés pour diffuser de l\'audio
à partir de l\'ordinateur et pour le lire à partir de sources telles que
des microphones.\
\
Out prend deux arguments : Le premier est le numéro d\'index du bus sur
lequel écrire. Ceux-ci commencent à 0, ce qui, dans une configuration
stéréo, correspond généralement au canal de sortie gauche. Le second est
soit un UGen, soit un tableau d\'UGens. Si vous fournissez un tableau
(c\'est-à-dire une sortie multicanal), le premier canal sera joué sur le
bus avec l\'index indiqué, le deuxième canal sur le bus avec l\'index
indiqué + 1, et ainsi de suite.\
\
Voici un exemple stéréo pour comprendre comment cela fonctionne. Le
SinOsc dont l\'argument de fréquence est 440 Hz sera joué sur le premier
bus de sortie (le canal gauche), et le SinOsc dont l\'argument de
fréquence est 442 Hz sera joué sur le deuxième bus (le canal droit). Par
défaut, out prend le bus 0 comme premier canal, les deux seront donc
joués sur les bus 0 et 1 respectivement.\
(\
SynthDef.new(\"tutorial-SinOsc-stereo\", { \|out\|\
var outArray ;\
outArray = \[SinOsc.ar(440, 0, 0.2), SinOsc.ar(442, 0, 0.2)\] ;\
Out.ar(out, outArray)\
}).play ;\
)\
Lorsque vous utilisez Function-play, un Out UGen est en fait créé pour
vous si vous n\'en créez pas un explicitement. L\'index de bus par
défaut pour cet UGen Out est 0.\
\
Function-play et SynthDef-play renvoient tous deux un autre type
d\'objet, un Synth, qui représente un synthé sur le serveur. Si vous
stockez cet objet en l\'assignant à une variable, vous pouvez contrôler
son comportement de différentes manières. Par exemple, la méthode
\"free\" provoque l\'arrêt de la lecture du synthé sur le serveur et la
libération de sa mémoire et de ses ressources CPU.\
x = { SinOsc.ar(660, 0, 0.2) }.play ;\
y = SynthDef.new(\"tutorial-SinOsc\", { \|out\| Out.ar(out,
SinOsc.ar(440, 0, 0,2))) }).play ;\
x.free ; // libre seulement x\
y.free ; // libérer seulement y\
Ceci est plus flexible que Cmd-. qui libère tous les synthés en même
temps.\
\
Plus souvent, vous voudrez envoyer le code d\'octets correspondant à
l\'application serveur sans créer immédiatement un synthé. Le grand
avantage de cette méthode est que vous pouvez jouer n\'importe quel
nombre de copies de la SynthDef sans avoir à compiler ou à envoyer un
réseau de générateurs d\'unités. Dans la plupart des cas, utilisez
\'add\', comme dans l\'exemple ci-dessous. Voir SynthDef : -add pour
plus de détails.\
// s\'exécute en premier, seul\
SynthDef.new(\"tutorial-PinkNoise\", { \|out\| Out.ar(out,
PinkNoise.ar(0.3)) }).add ;\
\
// ensuite :\
x = Synth.new(\"tutorial-PinkNoise\") ;\
y = Synth.new(\"tutorial-PinkNoise\") ;\
x.free ; y.free ;\
Cette méthode est plus efficace que l\'utilisation répétée de la même
fonction, car elle permet d\'éviter d\'évaluer la fonction, de compiler
le code d\'octets et de l\'envoyer plusieurs fois. Dans de nombreux cas,
cette économie d\'utilisation du CPU est si petite qu\'elle est
insignifiante, mais lorsqu\'il s\'agit de produire des synthés en masse,
cela peut être important.\
\
Une limitation correspondante à l\'utilisation directe des SynthDefs est
que la fonction UGen Graph dans un SynthDef est évaluée une et une seule
fois. (Rappelez-vous que le serveur ne sait rien du langage SC.) Cela
signifie qu\'il est un peu moins flexible. Comparez ces deux exemples :\
// d\'abord avec une fonction. Notez la fréquence aléatoire à chaque
fois que \"play\" est appelé.\
f = { SinOsc.ar(440 + 200.rand, 0, 0.2) } ;\
x = f.play ;\
y = f.play ;\
z = f.play ;\
x.free ; y.free ; z.free ;\
\
// Maintenant avec un SynthDef. Pas de hasard !\
SynthDef(\"tutorial-NoRand\", { \|out\| Out.ar(out, SinOsc.ar(440 +
200.rand, 0, 0.2)) }).add ;\
x = Synth(\"tutorial-NoRand\") ;\
y = Synth(\"tutorial-NoRand\") ;\
z = Synth(\"tutorial-NoRand\") ;\
x.free ; y.free ; z.free ;\
Chaque fois que vous créez un nouveau Synth basé sur la déf, la
fréquence est la même. Ceci est dû au fait que la fonction (et donc
200.rand) n\'est évaluée qu\'une seule fois, lorsque le SynthDef est
créé.\
\
Créer de la variété avec les SynthDefs\
Il existe de nombreuses façons d\'obtenir de la variété avec les
SynthDefs. Certaines choses, comme le hasard, peuvent être accomplies
avec différents UGens. Un exemple est Rand, qui calcule un nombre
aléatoire entre des valeurs basses et hautes lorsqu\'un synthé est créé
pour la première fois :\
// Avec Rand, ça marche !\
SynthDef(\"tutorial-Rand\", { \|out\| Out.ar(out, SinOsc.ar(Rand(440,
660), 0, 0.2))) }).add ;\
x = Synth(\"tutorial-Rand\") ;\
y = Synth(\"tutorial-Rand\") ;\
z = Synth(\"tutorial-Rand\") ;\
x.free ; y.free ; z.free ;\
Ce lien de la catégorie Browse : UGens répertorie un certain nombre
d\'UGens de ce type.\
\
La façon la plus courante de créer des variables est de placer des
arguments dans la fonction UGen Graph. Cela vous permet de définir
différentes valeurs lors de la création du synthé. Celles-ci sont
transmises dans un tableau en tant que deuxième argument de Synth-new.
Le tableau doit contenir des paires de noms d\'arguments et de valeurs.\
(\
SynthDef(\"tutorial-args\", { arg freq = 440, out = 0 ;\
Out.ar(out, SinOsc.ar(freq, 0, 0.2)) ;\
).add ;\
)\
x = Synth(\"tutorial-args\") ; // pas d\'argument, donc valeurs par
défaut\
y = Synth(\"tutorial-args\", \[\"freq\", 660\]) ; // modifie le freq\
z = Synth(\"tutorial-args\", \[\"freq\", 880, \"out\", 1\]) ; // modifie
la fréquence et le canal de sortie\
x.free ; y.free ; z.free ;\
Cette combinaison d\'args et d\'UGens signifie que vous pouvez tirer
beaucoup d\'avantages d\'une seule def, mais dans certains cas où une
flexibilité maximale est requise, vous pouvez avoir besoin d\'utiliser
des fonctions, ou de créer plusieurs defs.\
\
En savoir plus sur Synth\
Synth comprend quelques méthodes qui vous permettent de changer les
valeurs des args après la création d\'un synthé. Pour l\'instant, nous
n\'en verrons qu\'une, \'set\'. Synth-set prend des paires de noms
d\'arg et de valeurs.\
s.boot ;\
(\
SynthDef.new(\"tutorial-args\", { arg freq = 440, out = 0 ;\
Out.ar(out, SinOsc.ar(freq, 0, 0.2)) ;\
}).add ;\
)\
s.scope ; // scope pour que vous puissiez voir l\'effet\
x = Synth.new(\"tutorial-args\") ;\
x.set(\"freq\", 660) ;\
x.set(\"freq\", 880, \"out\", 1) ;\
x.free ;\
Quelques remarques sur les symboles, les chaînes de caractères, les noms
de SynthDef et d\'Arg\
Les noms de SynthDef et d\'arguments peuvent être soit des chaînes de
caractères, comme nous l\'avons vu plus haut, soit un autre type de
littéral appelé symbole. Les symboles s\'écrivent de deux manières, soit
entre guillemets simples : \'tutorial_SinOsc\' ou précédés d\'une barre
oblique inverse : \\tutorial_SinOsc. Comme les chaînes de caractères,
les symboles sont constitués de séquences alphanumériques. La différence
entre les chaînes et les symboles est que tous les symboles contenant le
même texte sont garantis identiques, c\'est-à-dire qu\'il s\'agit
exactement du même objet, alors que ce n\'est pas forcément le cas pour
les chaînes. Vous pouvez tester cela à l\'aide de \'===\'. Exécutez ce
qui suit et observez la fenêtre d\'affichage.\
\"a String\" === \"a String\" ; // l\'affichage sera faux\
\\aSymbol === \'aSymbol\' ; // l\'affichage sera vrai\
En général, dans les méthodes qui communiquent avec le serveur, on peut
utiliser indifféremment des chaînes et des symboles, mais il faut savoir
que ce n\'est pas nécessairement vrai dans le code général.\
\"this\" === \\this ; // l\'affichage sera faux\
Pour plus d\'informations, voir :\
\
SynthDef, Synth, String, Symbol, Literals, Randomness, Browse : UGens\
\
Exercice suggéré\
Essayez de convertir certains des exemples précédents basés sur des
fonctions, ou vos propres fonctions, en versions SynthDef, en ajoutant
Out UGens. Expérimentez l\'ajout et la modification d\'arguments à la
fois lors de la création des synthés et par la suite en utilisant
\'set\'.\
\
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\
\
Ce document fait partie du tutoriel \"Getting Started With
SuperCollider\".\
\
Cliquez ici pour passer à la section suivante : 11. Les bus\
\
Cliquez ici pour revenir à la table des matières : 00. Démarrer avec SC\
\
Source du fichier d\'aide : C:\\NProgram
Files\\NSuperCollider-3.13.0\\NHelpSource\\NTutorials\\NGetting-Started\\N10-SynthDefs-and-Synths.schelp\
link::Tutorials/Getting-Started/10-SynthDefs-and-Synths: :
