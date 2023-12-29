Table des Matières ▼ SuperColliderBrowseSearchIndexes\
SuperColliderBrowseSearchIndexes ▼\
Tutoriels/Démarrage \| Tutoriels \> Démarrage\
\
04. Fonctions et autres fonctionnalités\
Démarrer avec SuperCollider\
\
Voir aussi : 00. Premiers pas avec SC\
AVERTISSEMENT : Si vous utilisez un casque ou des haut-parleurs
externes, il est recommandé de baisser le volume avant de lire
l\'exemple suivant, puis de l\'augmenter jusqu\'à un niveau confortable.
Les sons synthétiques bruts peuvent parfois sembler plus forts que la
musique et les autres formes d\'audio que nous écoutons habituellement.\
La façon la plus simple d\'obtenir du son à partir de SC est d\'utiliser
la fonction play. Après vous être assuré que le serveur est démarré,
exécutez l\'exemple simple ci-dessous.\
\
Lorsque vous en avez assez, arrêtez le son en appuyant sur Ctrl-. (Cmd-.
sur macOS). Cela arrêtera toujours tous les processus en cours et le son
dans SC. Vous l\'utiliserez souvent, alors apprenez-la par cœur.\
{ \[SinOsc.ar(440, 0, 0.2), SinOsc.ar(442, 0, 0.2)\] }.play ;\
Pas trop inspirant ? Ne vous inquiétez pas, nous ne faisons que
commencer, et ce n\'est qu\'un simple exemple pour démontrer les
fonctions et le son. Nous le décomposerons un peu plus loin.\
\
Mais avant cela, nous allons en apprendre un peu plus sur les fonctions
en général.\
\
Une fonction est un bout de code réutilisable. Vous définissez une
fonction en enfermant le code dans des crochets : { }. Voici un exemple
:\
f = { \"Fonction évaluée\".postln } ;\
Ce qui se trouve entre les crochets est ce qui sera exécuté chaque fois
que vous réutiliserez, ou \"appellerez\", ou \"évaluerez\" la fonction.
Notez que ceci est écrit comme une équation, c\'est-à-dire f = {\...}.
Il ne s\'agit pas d\'une équation au sens mathématique du terme, mais de
ce que l\'on appelle une \"affectation\". En fait, cela me permet de
nommer la fonction que j\'ai créée, en la stockant dans une variable
appelée f. Une variable est un nom représentant un emplacement dans
lequel on peut stocker des choses, comme une fonction, un nombre, une
liste, etc. Exécutez les lignes suivantes une à la fois et observez la
fenêtre d\'affichage :\
f = { \"Fonction évaluée\".postln } ;\
f ;\
Les deux fois, il doit être indiqué \"une fonction\". Maintenant, chaque
fois que nous voulons faire référence à notre fonction, nous pouvons
simplement utiliser la lettre f. C\'est en fait ce qui la rend
réutilisable ! Sinon, nous devrions taper la fonction à chaque fois.\
\
Les fonctions peuvent également s\'étendre sur plusieurs lignes.
Double-cliquez sur la première ligne pour vous assurer que tout ce qui
se trouve entre les parenthèses est sélectionné (tout le code à
l\'intérieur des crochets, mais aussi le f =) :\
(\
f = {\
\"Démarrage de l\'évaluation de la fonction\".postln ;\
\"Fin de l\'évaluation de la fonction\".postln\
} ;\
)\
f ;\
Comment le réutiliser ? Exécutez les lignes suivantes une à la fois et
observez la fenêtre d\'affichage :\
f = { \"Fonction évaluée\".postln } ;\
f.value ;\
f.value ;\
f.value ;\
Notre fonction est un objet (c\'est-à-dire une chose qui fait quelque
chose ou représente quelque chose), que nous avons défini et stocké dans
la variable f. Le bout de code qui dit \'.value\' indique qu\'il faut
évaluer cette fonction maintenant. Voici un exemple d\'envoi d\'un
message à un objet. La syntaxe est la suivante : someObject.someMessage.
Le point doit être placé entre les deux.\
\
La suite est un peu plus délicate. Dans un objet donné, chaque message
appelle (appelle signifie exécute) une méthode particulière. Différents
types d\'objets peuvent avoir des méthodes portant le même nom, et donc
répondre au même message de différentes manières. Relisez lentement, car
c\'est très important :\
\
Différents types d\'objets peuvent avoir des méthodes portant le même
nom, et donc répondre au même message de différentes manières.\
\
Ce qui est intéressant, c\'est que les méthodes réelles peuvent différer
dans ce qu\'elles font, mais tant qu\'elles implémentent une méthode
portant ce nom, elles deviennent interchangeables dans votre code.\
\
Un bon exemple est celui de la \"valeur\". Tous les objets de SC
répondent au message \"value\". Lorsque vous \"appelez\" une méthode,
elle \"renvoie\" toujours quelque chose. Lorsque vous appelez la méthode
\"value\" d\'une fonction, elle évalue et renvoie le résultat de sa
dernière ligne de code. L\'exemple de fonction ci-dessous renvoie le
résultat de la dernière ligne (5) :\
(\
f = {\
\"Evaluating\...\".postln ;\
2 + 3\
} ;\
f.value ;\
)\
Souvent, les méthodes renvoient simplement l\'objet lui-même. C\'est le
cas de la plupart des objets et du message \"value\". L\'exemple
ci-dessous le démontre. (Tout ce qui se trouve à droite du // est un
\"commentaire\", ce qui signifie que SC l\'ignore. Les commentaires sont
souvent utilisés pour laisser des notes sur des morceaux de code
compliqués ou déroutants pour les lecteurs futurs)\
f = 3 ; // Créer une variable et lui assigner un nombre comme valeur\
f.value ; // La fenêtre d\'affichage indique : 3, c\'est-à-dire que le
nombre se retourne lui-même\
f.value ; // f n\'a pas changé, donc la fenêtre d\'affichage indique à
nouveau 3\
\
f = { 3.0.rand } ; // Attribue à la variable une fonction comme valeur\
f.value ; // 3.0.rand renvoie une valeur aléatoire comprise entre 0,0 et
3,0 (exclusif).\
f.value ; // Une autre valeur aléatoire\
f.value ; // Encore une autre valeur aléatoire\
En utilisant la méthode \"value\", les fonctions et les autres objets
peuvent être interchangeables dans votre code. Il s\'agit d\'un exemple
de polymorphisme, qui est l\'une des puissantes caractéristiques de ce
que l\'on appelle la programmation orientée objet (POO). Le
polymorphisme signifie que différents objets sont interchangeables
s\'ils répondent au même message. Nous y reviendrons plus tard.\
\
Les fonctions peuvent également avoir ce que l\'on appelle des
arguments. Il s\'agit de valeurs qui sont transmises à la fonction
lorsqu\'elle est évaluée. L\'exemple ci-dessous montre comment cela
fonctionne. Essayez de deviner le résultat avant de l\'exécuter.\
(\
f = { arg a, b ;\
a - b\
} ;\
f.value(5, 3) ;\
)\
Les arguments sont déclarés au début de la fonction, à l\'aide du
mot-clé \"arg\". Vous pouvez ensuite y faire référence comme à des
variables. Lorsque vous appelez value sur une fonction, vous pouvez
passer des arguments, dans l\'ordre, en les mettant entre parenthèses :
someFunc.value(arg1, arg2). Il en va de même pour toutes les méthodes
qui prennent des arguments, et pas seulement pour value.\
\
Vous pouvez spécifier différents ordres en utilisant ce que l\'on
appelle des arguments par mot-clé :\
(\
f = { arg a, b ;\
a / b // \'/\' signifie diviser\
} ;\
f.value(10, 2) ; // style normal\
f.value(b : 2, a : 10) ; // style mot-clé\
)\
Vous pouvez mélanger le style normal et le style mot-clé si vous le
souhaitez, mais les arguments normaux doivent être placés en premier :\
(\
f = { arg a, b, c, d ;\
(a + b) \* c - d\
} ;\
f.value(2, c:3, b:4, d : 1) ; // (2 + 4) \* 3 - 1\
)\
(Notez que SC n\'a pas de priorité d\'opérateur, c\'est-à-dire que les
opérations mathématiques sont effectuées dans l\'ordre de gauche à
droite, et que la division et la multiplication ne sont pas effectuées
en premier. Pour forcer un ordre, utilisez des parenthèses, par exemple
4 + (2\* 8) )\
\
Il est parfois utile de définir des valeurs par défaut pour les
arguments. Vous pouvez le faire comme suit :\
(\
f = { arg a, b = 2 ;\
a + b\
} ;\
f.value(2) ; // 2 + 2\
)\
Les valeurs par défaut doivent être ce que l\'on appelle des littéraux.
Les valeurs littérales sont essentiellement des nombres, des chaînes de
caractères, des symboles (nous y reviendrons plus tard) ou des
collections de ces éléments. Ne vous inquiétez pas si cela n\'a pas tout
à fait de sens, cela deviendra plus clair au fur et à mesure que nous
avancerons.\
\
Il existe une autre façon de spécifier les args, qui consiste à les
entourer de deux lignes verticales. (Sur la plupart des claviers, le
symbole de la ligne verticale est Shift-\\) Les deux fonctions suivantes
sont équivalentes :\
(\
f = { arg a, b ;\
a + b\
} ;\
g = { \|a, b\|\
a + b\
} ;\
f.value(2, 2) ;\
g.value(2, 2) ;\
)\
Pourquoi deux méthodes différentes ? Certaines personnes préfèrent la
seconde et la considèrent comme un raccourci. SC dispose d\'un certain
nombre de raccourcis syntaxiques de ce type, qui peuvent rendre
l\'écriture du code un peu plus rapide. Dans tous les cas, vous
rencontrerez les deux formes, vous devez donc en être conscient.\
\
Vous pouvez également avoir des variables dans une fonction. Vous devez
les déclarer au début de la fonction, juste après les args, en utilisant
le mot-clé \"var\".\
(\
f = { arg a, b ;\
var firstResult, finalResult ;\
premierRésultat = a + b ;\
finalResult = firstResult \* 2 ;\
résultat final\
} ;\
f.value(2, 3) ; // le résultat sera (2 + 3) \* 2 = 10\
)\
Les noms de variables et d\'arguments peuvent être composés de lettres
et de chiffres, mais doivent commencer par une lettre minuscule et ne
peuvent pas contenir d\'espaces.\
\
Les variables ne sont valables que pour ce que l\'on appelle leur
portée. La portée d\'une variable déclarée dans une fonction est cette
fonction, c\'est-à-dire la zone située entre les deux crochets.
Exécutez-les un par un :\
(\
f = {\
var foo ;\
foo = 3 ;\
foo\
} ;\
f.value ;\
foo ; // cela provoquera une erreur car \"foo\" n\'est valide qu\'à
l\'intérieur de f.\
)\
Vous pouvez également déclarer des variables au début de n\'importe quel
bloc de code que vous exécutez en entier (c\'est-à-dire en le
sélectionnant entièrement). Dans ce cas, ce bloc de code constitue la
portée de la variable. Exécutez le bloc (entre parenthèses), puis la
dernière ligne.\
(\
var myFunc ;\
myFunc = { \|input\| input.postln } ;\
myFunc.value(\"foo\") ; // arg est une chaîne de caractères\
myFunc.value(\"bar\") ;\
)\
\
myFunc ; // jette une erreur\
Vous vous demandez peut-être pourquoi nous n\'avons pas eu besoin de
déclarer des variables comme f, et pourquoi elles conservent leur valeur
même lorsque le code est exécuté une ligne à la fois (c\'est-à-dire
qu\'elles ont une portée globale). Les lettres a à z sont ce qu\'on
appelle des variables d\'interprète. Elles sont prédéclarées au
démarrage de SC et ont une portée illimitée, ou \"globale\". Elles sont
donc utiles pour des tests ou des exemples rapides. Vous avez déjà
rencontré l\'une d\'entre elles, la variable \"s\", qui, vous vous en
souviendrez, fait référence par défaut au serveur localhost.\
\
Avec des arguments, nous pouvons voir un exemple du fonctionnement du
polymorphisme :\
(\
f = { arg a ;\
a.value + 3 // appelle \"value\" sur a ; le polymorphisme attend !\
} ;\
)\
f.value(3) ; // a.value est 3, ce qui renvoie 3 + 3 = 6\
g = { 3.0.rand } ;\
f.value(g) ; // ici l\'argument est une Fonction. a.value évalue
3.0.rand\
f.value(g) ; // essayez à nouveau, le résultat est différent\
Vous commencez à voir comment cela pourrait être utile ?\
\
Pour plus d\'informations, voir :\
\
Fonctions, fonctions, instructions d\'affectation, introduction aux
objets, littéraux, portée et fermeture.\
\
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\
\
Ce document fait partie du tutoriel Premiers pas avec SuperCollider.\
\
Cliquez ici pour passer à la section suivante : 05. Fonctions et son\
\
Cliquez ici pour revenir à la table des matières : 00. Démarrer avec SC\
\
