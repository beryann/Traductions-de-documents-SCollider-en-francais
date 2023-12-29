Table des Matières ▼ SuperColliderBrowseSearchIndexes\
SuperColliderBrowseSearchIndexes ▼\
Tutoriels / Mise en route \| Tutoriels \> Mise en route\
12. Groupes\
Démarrer avec SuperCollider\
Voir aussi : 00. Démarrer avec SC\
Notre discussion sur l\'ordre des synthés sur le serveur nous amène au
sujet des groupes. Les synthés sur le serveur sont un type de ce que
l\'on appelle des noeuds. Il existe un autre type de nœud : les groupes.
Les groupes sont simplement des collections de nœuds, et peuvent
contenir des synthés, d\'autres groupes, ou des combinaisons des deux.
Ils sont principalement utiles de deux manières : D\'une part, ils sont
très utiles pour contrôler l\'ordre, d\'autre part, ils vous permettent
de regrouper facilement des nœuds et de leur envoyer des messages en une
seule fois. Comme vous l\'avez probablement deviné, il existe un objet
d\'abstraction Server très pratique pour représenter les nœuds de groupe
dans l\'application client : Group.\
\
Les groupes comme outils d\'ordonnancement\
Les groupes peuvent être très utiles pour contrôler l\'ordre. Comme les
synthétiseurs, ils prennent les cibles et les addActions comme
arguments, ce qui facilite leur mise en place.\
g = Group.new ;\
h = Group.before(g) ;\
g.free ; h.free ;\
Cela peut s\'avérer très utile pour séparer les effets ou les
traitements des sources sonores et les placer dans le bon ordre.
Reprenons notre exemple de réverbération de la section précédente.\
(\
// une version stéréo\
SynthDef(\\tutorial_DecaySin2, { arg outBus = 0, effectBus, direct =
0.5, freq = 440 ;\
var source ;\
// 1.0.rand2 renvoie un nombre aléatoire de -1 à 1, utilisé ici pour un
pan aléatoire\
source = Pan2.ar(Decay2.ar(Impulse.ar(Rand(0.3, 1), 0, 0.125), 0.3, 1,\
SinOsc.ar(SinOsc.kr(0.2, 0, 110, freq))), Rand(-1.0, 1.0)) ;\
Out.ar(outBus, source \* direct) ;\
Out.ar(effectBus, source \* (1 - direct)) ;\
}).add ;\
\
SynthDef(\\tutorial_Reverb2, { arg outBus = 0, inBus ;\
var input ;\
input = In.ar(inBus, 2) ;\
16.do({ input = AllpassC.ar(input, 0.04, Rand(0.001,0.04), 3)}) ;\
Out.ar(outBus, input) ;\
}).add ;\
)\
\
// nous créons maintenant des groupes pour les effets et les synthés\
(\
\~sources = Group.new ;\
\~effects = Group.after(\~sources) ; // assurez-vous que c\'est bien
après\
\~bus = Bus.audio(s, 2) ; // ce sera notre bus d\'effets stéréo\
)\
\
// maintenant les synthés dans les groupes. L\'action d\'ajout par
défaut est \\addToHead\
(\
x = Synth(\\tutorial_Reverb2, \[\\inBus, \~bus\], \~effects) ;\
y = Synth(\\tutorial_DecaySin2, \[\\effectBus, \~bus, \\outBus, 0\],
\~sources) ;\
z = Synth(\\tutorial_DecaySin2, \[\\effectBus, \~bus, \\outBus, 0,
\\freq, 660\], \~sources) ;\
)\
\
// nous pourrions ajouter d\'autres synthés de sources et d\'effets ici\
\
\~sources.free ; \~effects.free ; // cela libère également leur contenu
(x, y, z)\
\~bus.free ;\
\
// supprimer les références aux variables d\'environnement \~sources et
\~effects :\
currentEnvironment.clear ;\
Notez que nous ne nous soucions probablement pas de l\'ordre dans lequel
les sources et les effets sont placés dans les groupes, tout ce qui
compte est que tous les synthés d\'effets viennent après les synthés de
sources qu\'ils traitent.\
\
Si vous vous interrogez sur les noms \'\~sources\' et \'\~effects\', le
fait de placer un tilde (\~) devant un mot est une façon de créer une
variable d\'environnement. Pour l\'instant, tout ce que vous devez
savoir à leur sujet est qu\'elles peuvent être utilisées de la même
manière que les variables d\'interpréteur (vous n\'avez pas besoin de
les déclarer et elles sont persistantes) et qu\'elles permettent
d\'utiliser des noms plus descriptifs. Vous devriez envisager
d\'utiliser des définitions de variables et des fonctions chaque fois
qu\'un accès direct ultérieur n\'est pas nécessaire - un grand nombre de
variables d\'environnement peut provoquer des bogues difficiles à
trouver. N\'oubliez pas d\'effacer l\'environnement actuel (voir
ci-dessus) pour éviter toute interférence.\
// pour être sûr, créez un nouvel environnement :\
Environment.new.push ;\
\
// un peu de code\...\
\
// restaurer l\'ancien environnement\
currentEnvironment.pop ;\
Toutes les actions d\'ajout\
À ce stade, il est probablement bon de couvrir les autres actions
d\'ajout. En plus de \\addBefore et \\addAfter, il y a aussi le
(rarement) utilisé \\addReplace, et deux actions d\'ajout qui
s\'appliquent aux groupes : \\addToHead et \\addToTail. La première
ajoute le récepteur au début du groupe, de sorte qu\'il s\'exécute en
premier, la seconde à la fin du groupe, de sorte qu\'il s\'exécute en
dernier. Comme les autres addActions, \\addToHead et \\addToTail ont des
méthodes de commodité appelées \"head\" et \"tail\".\
g = Group.new ;\
h = Group.head(g) ; // ajoute h à la tête de g\
x = Synth.tail(h, \\default) ; // ajoute x à la queue de h\
s.queryAllNodes ; // ceci affichera une représentation de la hiérarchie
des noeuds\
x.free ; h.free ; g.free ;\
QueryAllNodes et ID des nœuds\
Le serveur dispose d\'une méthode appelée \"queryAllNodes\" qui affiche
une représentation de l\'arbre des nœuds du serveur. Lors de
l\'exécution de l\'exemple ci-dessus, vous avez dû voir quelque chose
comme ce qui suit dans la fenêtre d\'affichage :\
nodes on localhost :

\
a Serveur\
Groupe(0)\
Groupe(1)\
Groupe(1000)\
Groupe(1001)\
Synth 1002\
Lorsque vous voyez un groupe imprimé ici, tout ce qui se trouve en
dessous et qui est indenté à droite est contenu dans ce groupe. L\'ordre
des nœuds est de haut en bas. Les nombres que vous voyez sont ce qu\'on
appelle des ID de nœuds, qui sont la façon dont le serveur garde la
trace des nœuds. Normalement, lorsque vous travaillez avec des objets
d\'abstraction du serveur, vous n\'avez pas besoin de vous occuper des
ID de nœuds, car les objets en gardent la trace, en les assignant et en
les libérant le cas échéant.\
\
Vous vous êtes peut-être demandé pourquoi il y avait quatre groupes
affichés ci-dessus alors que nous n\'en avons créé que deux. Les deux
premiers, avec les ID 0 et 1, sont des groupes spéciaux, appelés le nœud
racine et le \"groupe par défaut\".\
\
Le nœud racine et le groupe par défaut\
Lorsqu\'une application serveur est démarrée, un groupe spécial est créé
avec un ID de nœud de 0. Il représente le sommet de l\'arborescence des
nœuds du serveur. Il existe un objet d\'abstraction serveur spécial pour
le représenter, appelé RootNode. En outre, un autre groupe est créé avec
un ID de 1, appelé groupe par défaut. Il s\'agit de la cible par défaut
pour tous les nœuds et c\'est ce que vous obtiendrez si vous fournissez
un serveur comme cible. Si vous ne spécifiez pas de cible ou si vous
passez nil, vous obtiendrez le groupe par défaut du serveur par défaut.\
s.boot ;\
a = Synth.new(\\default) ; // crée un synthé dans le groupe par défaut
du serveur par défaut\
a.group ; // Retourne un objet Group. Notez l\'ID de 1 (le groupe par
défaut) dans la fenêtre d\'affichage\
Le groupe par défaut a une fonction importante : il fournit un arbre de
nœuds de base prévisible de sorte que des méthodes telles que
Server-scope et Server-record (qui créent des nœuds qui doivent venir
après tout le reste) peuvent fonctionner sans rencontrer de problèmes
d\'ordre d\'exécution. Dans l\'exemple ci-dessous, le nœud de scoping
vient après le groupe par défaut.\
{ SinOsc.ar(mul : 0.2) }.scope(1) ;\
\
// regarder la fenêtre d\'affichage ;\
s.queryAllNodes ;\
\
// notre synthé SinOsc se trouve dans le groupe par défaut (ID 1)\
// le nœud scope (\'stethoscope\') vient après le groupe par défaut,
donc pas de problème\
En général, vous devez ajouter des nœuds au groupe par défaut ou aux
groupes qu\'il contient, et non pas avant ou après. Lorsque vous ajoutez
un synthétiseur d\'effets, par exemple, vous devez résister à la
tentation de l\'ajouter après le groupe par défaut, et plutôt créer un
groupe source distinct dans le groupe par défaut. Cela permet d\'éviter
les problèmes de cadrage ou d\'enregistrement.\
groupe par défaut \[\
groupe source \[\
source synth1\
source synth2\
\]\
effets synth\
\]\
synthé d\'enregistrement\
Les groupes comme, eh bien, les groupes\...\
L\'autre utilisation majeure des groupes est de vous permettre de
traiter facilement un certain nombre de synthés comme un tout. Si vous
envoyez un message \"set\" à un groupe, il appliquera ce message à tous
les noeuds qu\'il contient.\
g = Group.new ;\
\
// créer 4 synthés dans g\
// 1.0.rand2 renvoie un nombre aléatoire compris entre -1 et 1.\
4.do({ { arg amp = 0.1 ; Pan2.ar(SinOsc.ar(440 + 110.rand, 0, amp),
1.0.rand2) }.play(g) ; }) ;\
\
g.set(\\amp, 0.005) ; // les réduit tous\
\
g.free ;\
Les groupes, leur héritage, et plus encore sur la recherche de l\'aide\
Voici maintenant un peu plus de théorie de la POO. Group et Synth sont
des exemples de ce que l\'on appelle des sous-classes. Les sous-classes
sont les enfants d\'une classe mère, appelée superclasse. Toutes les
sous-classes héritent des méthodes de leur superclasse. Elles peuvent
remplacer certaines méthodes par leur propre implémentation (en tirant
parti du polymorphisme), mais en général, les sous-classes répondent à
toutes les méthodes de leur superclasse et à d\'autres méthodes qui leur
sont propres. Certaines classes sont des classes abstraites, ce qui
signifie que vous ne créez pas d\'instances de ces classes, mais
qu\'elles existent simplement pour fournir un ensemble commun de
méthodes et de variables à leurs sous-classes.\
\
Nous pouvons par exemple imaginer une classe abstraite appelée Chien,
qui possède un certain nombre de sous-classes, telles que Terrier,
BassetHound, etc. Ces sous-classes pourraient toutes avoir une méthode
\"courir\", mais toutes n\'auraient pas besoin d\'une méthode
\"troupeau\".\
\
Cette façon de travailler présente certains avantages : Si vous devez
modifier une méthode héritée, vous pouvez le faire en un seul endroit,
et toutes les sous-classes qui en héritent seront également modifiées.
De même, si vous souhaitez étendre une classe pour en faire une variante
personnelle ou une version améliorée, vous pouvez automatiquement
bénéficier de toutes les fonctionnalités de la superclasse.\
\
L\'héritage peut remonter à plusieurs niveaux, ce qui signifie que la
superclasse d\'une classe peut également avoir une superclasse. (Tous
les objets de SC héritent en fait d\'une classe appelée Object, qui
définit un certain ensemble de méthodes dont toutes ses sous-classes
héritent ou qu\'elles surchargent.\
\
Group et Synth sont des sous-classes de la classe abstraite Node. Pour
cette raison, certaines de leurs méthodes sont définies dans Node et (ce
qui est peut-être plus important d\'un point de vue pratique) sont
documentées dans le fichier d\'aide de Node.\
\
Ainsi, si vous consultez un fichier d\'aide et que vous ne trouvez pas
une méthode particulière à laquelle une classe répond, vous devrez
peut-être consulter le fichier d\'aide de la superclasse de cette
classe, ou plus haut dans la chaîne. La plupart des classes ont leur
superclasse listée en haut de leur fichier d\'aide. Vous pouvez
également utiliser les méthodes suivantes pour obtenir ce type
d\'informations et trouver de la documentation (voir la fenêtre du post)
:\
Group.superclass ; // ceci renverra \'Node\'\
Group.superclass.help ;\
Group.findRespondingMethodFor(\'set\') ; // Node-set\
Group.findRespondingMethodFor(\'postln\') ; // Objet-postln ;\
Group.helpFileForMethod(\'postln\') ; // ouvre le fichier d\'aide de la
classe Object\
Pour plus d\'informations, voir :\
\
Groupe, Nœud, Groupe par défaut, Nœud racine, Introduction aux objets,
Ordre d\'exécution, Synth, En savoir plus sur l\'obtention d\'aide,
Interrogation interne (Introspection)\
\
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\
\
Ce document fait partie du tutoriel Premiers pas avec SuperCollider.\
\
Cliquez ici pour passer à la section suivante : 13. Tampons\
\
Cliquez ici pour revenir à la table des matières : 00. Démarrer avec SC\
\
Source du fichier d\'aide : C:\\NProgram
Files\\NSuperCollider-3.13.0\\NHelpSource\\NTutorials\\NGetting-Started\\N12-Groups.schelp\
link::Tutorials/Getting-Started/12-Groups: :
