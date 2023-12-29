Table des Matières ▼ SuperColliderBrowseSearchIndexes\
SuperColliderBrowseSearchIndexes ▼\
Tutoriels / Mise en route \| Tutoriels \> Mise en route\
11. Bus\
Démarrer avec SuperCollider\
Voir aussi : 00. Démarrer avec SC\
Maintenant, un peu plus sur les bus sur le serveur. Les bus sont nommés
d\'après les bus ou les envois dans les tables de mixage analogiques, et
ils ont un but similaire : acheminer les signaux d\'un endroit à un
autre. Dans SC, cela signifie vers ou depuis le matériel audio, ou entre
différents synthés. Il en existe deux types : le taux audio et le taux
de contrôle. Comme vous l\'avez probablement deviné, le premier achemine
les signaux de taux audio et le second les signaux de taux de contrôle.\
\
Les bus de taux de contrôle sont assez simples à comprendre. Chacun
d\'entre eux possède un numéro d\'index, commençant à 0.\
\
Les bus de taux audio sont similaires, mais nécessitent un peu plus
d\'explications. Une application serveur dispose d\'un certain nombre de
canaux de sortie et d\'entrée. Ceux-ci correspondent aux premiers bus
audio, les sorties étant placées avant les entrées.\
\
Par exemple, si nous imaginons un serveur avec deux canaux de sortie et
deux canaux d\'entrée (c\'est-à-dire une entrée et une sortie stéréo),
les deux premiers bus audio (index 0 et index 1) seront les sorties, et
les deux suivants (index 2 et index 3) seront les entrées. L\'écriture
d\'un signal audio sur l\'un des bus de sortie se traduira par
l\'émission d\'un son sur vos haut-parleurs, et la lecture d\'un signal
audio sur les bus d\'entrée permettra d\'obtenir un son dans le SC à des
fins d\'enregistrement et de traitement (à condition que vous disposiez
d\'une source telle qu\'un microphone connecté à l\'entrée de votre
ordinateur ou d\'une interface audio).\
\
Les autres bus audio sont \"privés\". Ils sont utilisés simplement pour
envoyer des signaux audio et de contrôle entre différents synthés.
L\'envoi d\'audio à un bus privé ne produira pas de son sur vos
haut-parleurs, à moins que vous ne le réacheminiez plus tard vers l\'un
des bus de sortie. Ces bus \"privés\" sont souvent utilisés pour des
choses telles qu\'un \"envoi d\'effets\", c\'est-à-dire quelque chose
qui nécessite un traitement supplémentaire avant d\'atteindre les
haut-parleurs.\
\
Le nombre de bus de contrôle et de bus audio disponibles, ainsi que le
nombre de canaux d\'entrée et de sortie, est défini au moment du
démarrage de l\'application serveur. (Voir ServerOptions pour plus
d\'informations sur la manière de définir le nombre de canaux d\'entrée
et de sortie, ainsi que les bus).\
\
Ecrire ou lire dans les bus\
Nous avons déjà vu Out.ar, qui vous permet d\'écrire (c\'est-à-dire de
lire) de l\'audio sur un bus. Rappelons qu\'il a deux arguments - un
index, et une sortie - qui peut être un tableau d\'UGens (i.e. une
sortie multicanal) ou un seul UGen.\
\
Pour lire l\'audio d\'un bus, il faut utiliser un autre UGen : In. La
méthode \'ar\' de In prend également deux arguments : un index et le
nombre de canaux à lire. Si le nombre de canaux est supérieur à un, la
sortie de In sera un tableau. Exécutez les exemples suivants et observez
la fenêtre d\'affichage :\
In.ar(0, 1) ; // cela renverra \"un OutputProxy\".\
In.ar(0, 4) ; // ceci renverra un tableau de 4 OutputProxies\
Un OutputProxy est un type spécial d\'UGen qui agit comme un substitut
pour un signal qui sera présent lorsque le synthé fonctionnera. Vous
n\'aurez probablement jamais besoin d\'en utiliser un directement, donc
ne vous en préoccupez pas. Comprenez simplement ce qu\'ils sont afin de
les reconnaître lorsque vous les verrez dans la fenêtre d\'affichage et
ailleurs.\
\
In et Out ont également des méthodes \'kr\', qui lisent et écrivent des
signaux de taux de contrôle depuis et vers les bus de taux de contrôle.
Notez que Out.kr convertira un signal de débit audio en débit de
contrôle (c\'est ce qu\'on appelle le \"downsampling\"), mais que
l\'inverse n\'est pas vrai : Out.ar a besoin d\'un signal audio comme
second argument.\
// Cela provoque une erreur. Impossible d\'écrire un signal de débit de
contrôle dans un bus de débit audio\
{ \|out\| Out.ar(out, SinOsc.kr) }.play ;\
\
// Cela fonctionnera car le signal audio est sous-échantillonné en taux
de contrôle.\
Le signal audio est sous-échantillonné à la fréquence de contrôle {out\|
Out.kr(out, SinOsc.ar) }.scope ;\
(Cette limitation n\'est pas universelle parmi les UGens de taux audio,
cependant, et la plupart accepteront des signaux de taux de contrôle
pour une partie ou la totalité de leurs arguments. Certains
convertissent même les entrées en taux de contrôle en taux audio si
nécessaire, en remplissant les valeurs supplémentaires par un processus
appelé interpolation).\
\
Vous noterez que lorsque plusieurs Synths écrivent sur le même bus, leur
sortie est additionnée (c\'est-à-dire mélangée).\
(\
SynthDef(\"tutorial-args\", { arg freq = 440, out = 0 ;\
Out.ar(out, SinOsc.ar(freq, 0, 0.2)) ;\
).add ;\
)\
// les deux écrivent sur le bus 1, et leur sortie est mélangée\
x = Synth(\"tutorial-args\", \[\"out\", 1, \"freq\", 660\]) ;\
y = Synth(\"tutorial-args\", \[\"out\", 1, \"freq\", 770\]) ;\
Création d\'un objet Bus\
Il existe un objet pratique côté client pour représenter les bus de
serveur : Bus. Etant donné qu\'il suffit d\'un UGen In ou Out et d\'un
index pour écrire dans un bus, on peut se demander pourquoi on aurait
besoin d\'un objet Bus à part entière. La plupart du temps, ce n\'est
pas le cas, en particulier si vous ne faites que lire de l\'audio en
entrée et en sortie. Mais l\'objet Bus offre certaines fonctionnalités
utiles. Nous y reviendrons dans une seconde, mais voyons d\'abord
comment en créer un.\
\
Tout comme de nombreux UGens ont des méthodes ar et kr, Bus a deux
méthodes de création couramment utilisées : Bus-audio et Bus-control.
Elles prennent chacune deux arguments : un objet Serveur et le nombre de
canaux.\
b = Bus.control(s, 2) ; // Obtention d\'un Bus de contrôle à deux
canaux\
c = Bus.audio(s) ; // Obtention d\'un bus audio privé à un canal (un est
la valeur par défaut)\
Vous vous demandez peut-être ce qu\'est un bus à deux canaux, puisque
nous n\'en avons pas parlé auparavant. Vous devez vous rappeler que
lorsque Out a un tableau comme second argument, il écrira les canaux du
tableau dans des bus consécutifs. Rappelez-vous l\'exemple de 10.
SynthDefs et Synths :\
(\
SynthDef.new(\"tutorial-SinOsc-stereo\", { \|out\|\
var outArray ;\
outArray = \[SinOsc.ar(440, 0, 0.2), SinOsc.ar(442, 0, 0.2)\] ;\
Out.ar(out, outArray) ; // écrit dans les bus 0 et 1\
}).play ;\
)\
En réalité, il n\'existe pas de bus multicanaux à proprement parler,
mais les objets Bus sont capables de représenter une série de bus avec
des indices consécutifs. Ils encapsulent plusieurs bus adjacents côté
serveur dans un seul objet Bus, ce qui permet de les traiter comme un
groupe. Cela s\'avère très pratique.\
\
Lorsque vous travaillez avec des bus dits \"privés\" (c\'est-à-dire tout
ce qui n\'est pas les canaux d\'entrée et de sortie ; tous les bus de
contrôle sont \"privés\"), vous voulez généralement vous assurer que ce
bus n\'est utilisé que pour ce que vous voulez. Après tout, il s\'agit
de séparer les choses. Vous pourriez le faire en réfléchissant
soigneusement aux indices à utiliser, mais Bus permet de le faire
automatiquement. Chaque objet Server possède un allocateur de bus, et
lorsque vous créez un objet Bus, il réserve ces index privés, et ne les
redistribuera pas tant qu\'ils n\'auront pas été libérés. Vous pouvez
connaître l\'index d\'un bus en utilisant sa méthode \"index\".
Normalement, vous n\'aurez pas besoin de stocker cette valeur, car les
instances de Bus peuvent être passées directement en tant qu\'entrées
UGen ou args Synth.\
s.reboot ; // ceci va redémarrer le serveur et donc réinitialiser les
allocateurs de bus\
\
b = Bus.control(s, 2) ; // un bus de contrôle à 2 canaux\
b.index ; // doit être égal à zéro\
b.numChannels // Le bus possède également une méthode numChannels\
c = Bus.control(s) ;\
c.numChannels ; // le nombre de canaux par défaut est 1\
c.index ; // note que c\'est 2 ; b utilise 0 et 1\
Ainsi, en utilisant des objets Bus pour représenter des bus adjacents,
vous pouvez garantir qu\'il n\'y aura pas de conflit. Comme les indices
sont alloués dynamiquement, vous pouvez modifier le nombre de canaux
d\'un bus dans votre code (par exemple parce que vous devez maintenant
acheminer un signal multicanal), et vous avez toujours la garantie
d\'être en sécurité. Si vous allouiez simplement les bus en utilisant
des numéros d\'index, vous pourriez avoir à les ajuster tous pour faire
de la place à un canal adjacent supplémentaire, puisque les index
doivent être consécutifs ! C\'est un bon exemple de la puissance des
objets : en encapsulant des choses comme l\'allocation d\'index et en
fournissant une couche d\'abstraction, ils peuvent rendre votre code
plus flexible.\
\
Vous pouvez libérer les index utilisés par un bus en appelant sa méthode
\"free\". Cela permet de les réallouer.\
b = Bus.control(s, 2) ;\
b.free ; // libère les index. Vous ne pouvez plus utiliser cet objet Bus
après cela\
Notez que cela ne fait pas disparaître le bus sur le serveur, il est
toujours là. Le mot \'free\' indique simplement à l\'allocateur que vous
n\'utilisez plus ce bus pour le moment, et qu\'il peut réallouer
librement son index.\
\
Voici un autre avantage lorsque l\'on travaille avec des bus audio
privés. Comme nous l\'avons dit plus haut, les premiers bus sont les
canaux de sortie et d\'entrée. Donc si nous voulons utiliser le premier
bus privé, tout ce que nous avons à faire est de les additionner,
n\'est-ce pas ? Considérons notre application serveur avec 2 canaux de
sortie et 2 canaux d\'entrée. Le premier bus audio privé est d\'index 4
(0, 1, 2, 3 \... 4 !) Nous écrivons donc notre code et donnons à l\'UGen
Out 4 l\'index arg approprié.\
\
Mais que se passe-t-il si nous décidons plus tard de changer le nombre
de canaux de sortie à 6 ? Maintenant, tout ce qui a été écrit sur notre
bus privé sort par l\'un des canaux de sortie ! L\'allocateur de bus
audio d\'un serveur n\'attribue que des index privés, donc si vous
changez le nombre de canaux d\'entrée ou de sortie, il en tiendra compte
lors de l\'exécution de votre code. Une fois de plus, cela rend votre
code plus flexible.\
\
Les bus en action\
Voici donc deux exemples utilisant des bus. Le premier concerne un bus
de taux de contrôle.\
(\
SynthDef(\"tutorial-Infreq\", { arg bus, freqOffset = 0, out ;\
// ceci ajoutera freqOffset à tout ce qui est lu sur le bus\
Out.ar(out, SinOsc.ar(In.kr(bus) + freqOffset, 0, 0.5)) ;\
}).add ;\
\
SynthDef(\"tutorial-Outfreq\", { arg freq = 400, bus ;\
Out.kr(bus, SinOsc.kr(1, 0, freq/40, freq)) ;\
}).add ;\
\
b = Bus.control(s,1) ;\
)\
\
(\
x = Synth.new(\"tutorial-Outfreq\", \[\\bus, b\]) ;\
y = Synth.after(x, \"tutorial-Infreq\", \[\\bus, b\]) ;\
z = Synth.after(x, \"tutorial-Infreq\", \[\\bus, b, \\freqOffset, 200\])
;\
)\
x.free ; y.free ; z.free ; b.free ;\
y et z lisent tous deux sur le même bus. Le dernier modifie simplement
le signal de contrôle de la fréquence en y ajoutant une valeur constante
de 200. C\'est plus efficace que d\'avoir deux oscillateurs de contrôle
séparés pour contrôler la fréquence. Ce type de stratégie consistant à
connecter ensemble des synthés, dont chacun fait des choses différentes
dans un processus plus large, peut être très efficace en SC.\
\
Prenons maintenant l\'exemple d\'un bus audio. C\'est l\'exemple le plus
compliqué que nous ayons vu jusqu\'à présent, mais il devrait vous
donner une idée de la façon de commencer à mettre en place toutes les
choses que nous avons apprises. Le code ci-dessous utilise deux Synths
comme sources, l\'un créant des impulsions de PinkNoise (une sorte de
bruit qui a moins d\'énergie aux hautes fréquences qu\'aux basses), et
l\'autre créant des impulsions d\'ondes sinusoïdales. Les impulsions
sont créées à l\'aide des UGens Impulse et Decay2. Elles sont ensuite
réverbérées à l\'aide d\'une chaîne de AllpassC, qui est une sorte de
délai.\
\
Notez la construction 16.do({ \... }), ci-dessous. Celle-ci crée la
chaîne en évaluant la fonction 16 fois. C\'est une technique très
puissante et flexible, car en changeant simplement le nombre, je peux
changer le nombre d\'évaluations. Voir Integer pour plus d\'informations
sur Integer-do.\
(\
// l\'arg direct contrôlera la proportion du signal direct par rapport
au signal traité\
SynthDef(\"tutorial-DecayPink\", { arg outBus = 0, effectBus, direct =
0.5 ;\
var source ;\
// Impulsions décroissantes d\'un bruit rose. Nous ajouterons la
réverbération plus tard.\
source = Decay2.ar(Impulse.ar(1, 0.25), 0.01, 0.2, PinkNoise.ar) ;\
// ce sera notre sortie principale\
Out.ar(outBus, source \* direct) ;\
// ce sera notre sortie d\'effets\
Out.ar(effectBus, source \* (1 - direct)) ;\
}).add ;\
\
SynthDef(\"tutorial-DecaySin\", { arg outBus = 0, effectBus, direct =
0.5 ;\
var source ;\
// Impulsions décroissantes d\'une onde sinusoïdale modulante. Nous
ajouterons la réverbération plus tard.\
source = Decay2.ar(Impulse.ar(0.3, 0.25), 0.3, 1,
SinOsc.ar(SinOsc.kr(0.2, 0, 110, 440))) ;\
// ce sera notre sortie principale\
Out.ar(outBus, source \* direct) ;\
// ce sera notre sortie d\'effets\
Out.ar(effectBus, source \* (1 - direct)) ;\
}).add ;\
\
SynthDef(\"tutorial-Reverb\", { arg outBus = 0, inBus ;\
var input ;\
input = In.ar(inBus, 1) ;\
\
// une réverbération bas de gamme\
// aNumber.do évaluera son argument de fonction un nombre correspondant
de fois\
// {}.dup(n) évalue la fonction n fois et renvoie un tableau des
résultats.\
// La valeur par défaut de n est 2, ce qui donne une réverbération
stéréo.\
16.do({ input = AllpassC.ar(input, 0.04, { Rand(0.001,0.04) }.dup, 3)})
;\
\
Out.ar(outBus, input) ;\
}).add ;\
\
b = Bus.audio(s,1) ; // ce sera notre bus d\'effets\
)\
\
(\
x = Synth.new(\"tutorial-Reverb\", \[\\inBus, b\]) ;\
y = Synth.before(x, \"tutorial-DecayPink\", \[\\effectBus, b\]) ;\
z = Synth.before(x, \"tutorial-DecaySin\", \[\\effectBus, b, \\outBus,
1\]) ;\
)\
\
// Modifier l\'équilibre entre l\'humide et le sec\
y.set(\\direct, 1) ; // seulement PinkNoise direct\
z.set(\\direct, 1) ; // uniquement l\'onde sinusoïdale directe\
y.set(\\direct, 0) ; // uniquement le bruit rose réverbéré\
z.set(\\direct, 0) ; // uniquement onde sinusoïdale réverbérée\
x.free ; y.free ; z.free ; b.free ;\
Notez que nous pourrions facilement avoir beaucoup plus de synthés
sources traités par le seul synthé de réverbération. Si nous avions
intégré la réverbération dans les synthés sources, nous aurions dupliqué
l\'effort. Mais l\'utilisation d\'un bus privé nous permet d\'être plus
efficaces.\
\
Plus de plaisir avec les bus de contrôle\
Il y a d\'autres choses puissantes que vous pouvez faire avec les bus de
taux de contrôle. Par exemple, vous pouvez mapper n\'importe quel arg
dans un synthé en cours d\'exécution pour lire un bus de contrôle. Vous
pouvez également écrire des valeurs constantes dans les bus de contrôle
en utilisant la méthode \"set\" du bus, et interroger les valeurs en
utilisant sa méthode \"get\".\
(\
// créez deux bus de taux de contrôle et définissez leurs valeurs à 880
et 884.\
b = Bus.control(s, 1) ; b.set(880) ;\
c = Bus.control(s, 1) ; c.set(884) ;\
// et créer un synthé avec deux arguments de fréquence\
x = SynthDef(\"tutorial-map\", { arg freq1 = 440, freq2 = 440, out ;\
Out.ar(out, SinOsc.ar(\[freq1, freq2\], 0, 0.1)) ;\
}).play(s) ;\
)\
// Maintenant, mappez freq1 et freq2 pour les lire sur les deux bus\
x.map(\\freq1, b, \\freq2, c) ;\
\
// Créez maintenant un Synth pour écrire dans l\'un des bus\
y = {Out.kr(b, SinOsc.kr(1, 0, 50, 880))}.play(addAction : \\addToHead)
;\
\
// libère y, et b conserve sa dernière valeur\
y.free ;\
\
// utiliser Bus-get pour voir quelle est la valeur. Observez la fenêtre
d\'affichage\
b.get({ arg val ; val.postln ; f = val ; }) ;\
\
// définir le freq2, ce qui permet de le \"démapper\" de c\
x.set(\\freq2, f / 2) ;\
\
// la fréquence 2 n\'est plus mappée, de sorte que la modification de la
valeur de c n\'a pas d\'effet\
c.set(200) ;\
\
x.free ; b.free ; c.free ;\
Notez que contrairement aux bus de taux audio, les bus de taux de
contrôle conservent leur dernière valeur jusqu\'à ce que quelque chose
de nouveau soit écrit.\
\
Notez également que Bus-get prend une fonction (appelée fonction
d\'action) comme argument. Cela s\'explique par le fait qu\'il faut un
peu de temps au serveur pour obtenir la réponse et la renvoyer. La
fonction, qui reçoit la valeur (ou un tableau de valeurs, dans le cas
d\'un bus multicanal) comme argument, vous permet de faire quelque chose
avec la valeur une fois qu\'elle est revenue.\
\
Il est très important de comprendre ce concept de temps de réponse
(généralement appelé latence). Il existe un certain nombre d\'autres
méthodes dans SC qui fonctionnent de cette manière, et cela peut vous
causer des problèmes si vous ne faites pas attention. Pour illustrer
cela, considérons l\'exemple ci-dessous.\
// créer un objet Bus et définir ses valeurs\
b = Bus.control(s, 1) ; b.set(880) ;\
\
// exécuter tout simplement\
(\
f = nil ; // juste pour être sûr\
b.get({ arg val ; f = val ; }) ;\
f.postln ;\
)\
\
// f est égal à nil, mais essayez à nouveau et c\'est ce à quoi nous
nous attendions !\
f.postln ;\
Pourquoi f était-il nul la première fois, mais pas la seconde ? La
partie de l\'application du langage qui exécute votre code (appelée
interprète) fait ce que vous lui dites, aussi vite que possible, quand
vous le lui demandez. Ainsi, dans le bloc de code entre les parenthèses
ci-dessus, il envoie le message \"get\" au serveur, programme la
fonction pour qu\'elle s\'exécute lorsqu\'une réponse est reçue, puis
passe à l\'affichage de f. Comme il n\'a pas encore reçu la réponse, f
est toujours nul lorsqu\'il est affiché la première fois.\
\
Le serveur ne met qu\'un temps infime à envoyer une réponse, de sorte
que lorsque nous parvenons à exécuter la dernière ligne de code, f a
pris la valeur 880, comme nous l\'avions prévu. Dans l\'exemple
précédent, cela ne posait pas de problème, car nous n\'exécutions
qu\'une ligne à la fois. Mais dans certains cas, vous aurez besoin
d\'exécuter les choses en bloc, et la technique de la fonction d\'action
est très utile à cet effet.\
\
Mettre les choses dans le bon ordre\
Dans les exemples ci-dessus, vous vous êtes peut-être interrogé sur des
éléments tels que Synth.after, et addAction : \\AddToHead. Pendant
chaque cycle (la période pendant laquelle un bloc d\'échantillons est
calculé), le serveur calcule les choses dans un ordre particulier, en
fonction de sa liste de synthés en cours d\'exécution.\
\
Il commence par le premier synthé de sa liste et calcule un bloc
d\'échantillons pour son premier UGen. Il le prend ensuite et calcule un
bloc d\'échantillons pour chacun de ses UGens restants à tour de rôle
(chacun d\'entre eux pouvant prendre la sortie d\'un UGen précédent
comme entrée). La sortie de ce synthé est écrite sur un ou plusieurs
bus, selon le cas. Le serveur passe alors au synthé suivant dans sa
liste, et le processus se répète, jusqu\'à ce que tous les synthés en
cours d\'exécution aient calculé un bloc d\'échantillons. Le serveur
peut alors passer au cycle suivant.\
\
La chose importante à comprendre est qu\'en règle générale, lorsque vous
connectez des synthés ensemble en utilisant des bus, il est important
que les synthés qui écrivent des signaux sur les bus soient plus tôt
dans l\'ordre du serveur que les synthés qui lisent ces signaux à partir
de ces bus. Par exemple, dans l\'exemple du bus audio ci-dessus, il
était important que le synthé \"reverb\" soit calculé après les synthés
de bruit et d\'onde sinusoïdale qu\'il traite.\
\
Il s\'agit d\'un sujet complexe et il existe quelques exceptions, mais
vous devez savoir que l\'ordre est crucial lors de l\'interconnexion des
synthés. Le fichier Ordre d\'exécution couvre ce sujet plus en détail.\
\
Synth-new a deux arguments qui vous permettent de spécifier où dans
l\'ordre un synthé est ajouté. Le premier est une cible, et le second
est une addAction. Cette dernière spécifie la position du nouveau synthé
par rapport à la cible.\
x = Synth(\"default\", \[\\freq, 300\]) ;\
// ajoute un deuxième synthé immédiatement après x\
y = Synth(\"default\", \[\\freq, 450\], x, \\addAfter) ;\
x.free ; y.free ;\
Une cible peut être un autre Synth (ou d\'autres choses ; nous en
reparlerons bientôt), et une action d\'ajout est un symbole. Voir Synth
pour une liste complète des addActions possibles.\
\
Les méthodes comme Synth-after sont simplement des moyens pratiques de
faire la même chose, la différence étant qu\'elles prennent une cible
comme premier argument.\
// Ces deux lignes de code sont équivalentes\
y = Synth.new(\"default\", \[\\freq, 450\], x, \\addAfter) ;\
y = Synth.after(x, \"default\", \[\\freq, 450\]) ;\
Pour plus d\'informations, voir :\
\
Bus, In, OutputProxy, Ordre d\'exécution, Synth\
\
Exercice suggéré\
Expérimentez l\'interconnexion de différents synthés en utilisant les
bus audio et de contrôle. Ce faisant, veillez à respecter leur ordre
d\'exécution sur le serveur.\
\
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\
\
Ce document fait partie du tutoriel Premiers pas avec SuperCollider.\
\
Cliquez ici pour passer à la section suivante : 12. Les groupes\
\
Cliquez ici pour revenir à la table des matières : 00. Démarrer avec SC\
\
Source du fichier d\'aide : C:\\NProgram
Files\\NSuperCollider-3.13.0\\NHelpSource\\NTutorials\\NPrise en
main\\N11-Busses.schelp\
link::Tutorials/Getting-Started/11-Busses: :
