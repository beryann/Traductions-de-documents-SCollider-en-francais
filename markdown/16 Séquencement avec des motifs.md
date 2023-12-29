Table des Matières ▼ SuperColliderBrowseSearchIndexes\
SuperColliderBrowseSearchIndexes ▼\
Tutoriels / Mise en route \| Tutoriels \> Mise en route\
16. Séquencement avec des motifs\
Démarrer avec SuperCollider\
Voir aussi : 00. Premiers pas avec SC\
La section précédente a montré comment utiliser des routines de données
pour générer des séquences de paramètres de synthèse. Cependant, écrire
une routine avec des rendements explicites n\'est pas une syntaxe très
pratique. Puisqu\'il s\'agit d\'une partie essentielle de la création de
musique assistée par ordinateur, nous avons vraiment besoin d\'un moyen
plus facile.\
\
Les motifs simplifient grandement l\'utilisation des flux de données. Un
motif est essentiellement une usine pour un flux de données. Les objets
du motif comprennent les données que vous souhaitez voir sortir du flux,
et le type de motif détermine la manière dont les données seront
diffusées.\
\
Par exemple, nous avons utilisé cette routine pour produire des numéros
de notes MIDI afin de jouer quelques phrases de \"Over the Rainbow\".\
(\
r = Routine({\
\[60, 72, 71, 67, 69, 71, 72, 60, 69, 67\].do({ \|midi\| midi.yield })
;\
}) ;\
)\
\
while { (m = r.next).notNil } { m.postln } ;\
Avec les motifs, nous pouvons exprimer l\'idée qu\'un flux renvoie les
mêmes valeurs, mais de manière plus claire et plus concise. Comme il
n\'est pas nécessaire d\'écrire explicitement le rendement, rien dans le
motif ne détourne l\'attention des données (qui sont la véritable
préoccupation en matière de composition).\
\
Pseq (Pattern-sequence) signifie simplement recracher les valeurs du
tableau une par une, dans l\'ordre, autant de fois que le second
argument (ici, une seule fois).\
p = Pseq(\[60, 72, 71, 67, 69, 71, 72, 60, 69, 67\], 1) ;\
r = p.asStream ;\
while { (m = r.next).notNil } { m.postln } ;\
Notez que le Pseq n\'est pas streamable en lui-même, mais qu\'il crée un
stream (Routine) lorsque vous appelez asStream sur lui. Cette routine
peut alors être utilisée exactement comme n\'importe quelle autre
routine - la boucle while utilisée pour lire les valeurs du flux est
exactement la même pour les deux, même si elles sont écrites
différemment.\
\
Ainsi, l\'exemple \"Over the Rainbow\" pourrait être réécrit, avec moins
d\'encombrement :\
(\
var midi, dur ;\
midi = Pseq(\[60, 72, 71, 67, 69, 71, 72, 60, 69, 67\], 1).asStream ;\
dur = Pseq(\[2, 2, 1, 0.5, 0.5, 1, 1, 2, 2, 3\], 1).asStream ;\
\
SynthDef(\\smooth, { \|out, freq = 440, sustain = 1, amp = 0.5\|\
var sig ;\
sig = SinOsc.ar(freq, 0, amp) \* EnvGen.kr(Env.linen(0.05, sustain,
0.1), doneAction : Done.freeSelf) ;\
Out.ar(out, sig ! 2)\
}).add ;\
\
r = Task({\
var delta ;\
while {\
delta = dur.next ;\
delta.notNil\
} {\
Synth(\\smooth, \[freq : midi.next.midicps, sustain : delta\]) ;\
delta.yield ;\
}\
}).play(quant : TempoClock.default.beats + 1.0) ;\
)\
Que peuvent faire d\'autre les motifs ?\
La bibliothèque de patterns de SuperCollider est vaste (plus de 120
classes, sans compter les bibliothèques d\'extension), ce qui dépasse
évidemment le cadre d\'un tutoriel pour la couvrir en profondeur. Mais
vous reviendrez sans cesse sur certains motifs.\
\
De nombreux motifs prennent des listes de valeurs et les renvoient dans
un certain ordre.\
\
Pseq(liste, répétitions, décalage)\
renvoie les valeurs de la liste dans l\'ordre\
Pshuf(liste, répétitions)\
brouille la liste dans un ordre aléatoire\
Prand(liste, répétitions)\
choisir au hasard parmi les valeurs de la liste\
Pxrand(liste, répétitions)\
choisit au hasard, mais ne renvoie jamais le même élément de la liste
deux fois de suite\
Pwrand(liste, poids, répétitions)\
comme Prand, mais choisit les valeurs en fonction d\'une liste de
probabilités/poids\
D\'autres modèles génèrent des valeurs en fonction de divers paramètres.
En plus de ces modèles de base, il existe toute une série de générateurs
de nombres aléatoires qui produisent des distributions spécifiques,
ainsi que des fonctions chaotiques.\
\
Pseries(start, step, length)\
série arithmétique, par exemple 1, 2, 3, 4, 5\
Pgeom(start, grow, length)\
série géométrique, par exemple 1, 2, 4, 8, 16\
Pwhite(lo, hi, longueur)\
générateur de nombres aléatoires, utilise rrand(lo, hi) \-- distribution
égale\
Pexprand(lo, hi, longueur)\
générateur de nombres aléatoires, utilise exprand(lo, hi) \--
distribution exponentielle\
D\'autres motifs modifient la sortie des motifs de valeur. Ils sont
appelés FilterPatterns (motifs de filtrage).\
\
Pn(motif, répétitions)\
répète le motif autant de fois que le nombre de répétitions l\'indique\
Pdup(n, motif)\
répète n fois les valeurs individuelles d\'un motif. n peut être un
motif numérique lui-même.\
Vous pouvez utiliser des motifs à l\'intérieur d\'autres motifs. Ici,
nous générons des nombres aléatoires sur une plage qui augmente
progressivement. La limite supérieure du générateur de nombres
aléatoires est un flux qui commence à 0,01, puis passe à 0,02, 0,03 et
ainsi de suite, comme le montre clairement le graphique.\
p = Pwhite(0.0, Pseries(0.01, 0.01, inf), 100).asStream ;\
// toutes les extractions du flux jusqu\'à ce qu\'il renvoie nil\
// il est évident que vous ne voulez pas faire cela pour un flux de
longueur \'inf\' !\
p.all.plot ;\
Ou, pour un autre exemple, si vous voulez ordonner un ensemble de
nombres de manière aléatoire de sorte que tous les nombres sortent avant
qu\'un nouvel ordre ne soit choisi, utilisez Pn pour répéter un Pshuf.\
p = Pn(Pshuf(\[1, 2, 3, 4, 5\], 1), inf).asStream ;\
p.nextN(15) ; // obtient 15 valeurs du flux du motif\
Ce n\'est qu\'un avant-goût, destiné à illustrer le type de flexibilité
que vous pouvez obtenir avec les motifs. Comme pour toute structure
riche et adaptable, le mieux est de commencer par des cas simples et
d\'aller progressivement vers des configurations plus compliquées.\
\
Jouer des notes avec un motif : Pbind\
Non seulement les motifs peuvent produire des données pour les notes,
mais ils peuvent également jouer les notes elles-mêmes. Encore une fois,
\"Over the Rainbow\".\
(\
SynthDef(\\smooth, { \|out, freq = 440, sustain = 1, amp = 0.5\|\
var sig ;\
sig = SinOsc.ar(freq, 0, amp) \* EnvGen.kr(Env.linen(0.05, sustain,
0.1), doneAction : Done.freeSelf) ;\
Out.ar(out, sig ! 2)\
}).add ;\
)\
\
(\
p = Pbind(\
// le nom du SynthDef à utiliser pour chaque note\
\\instrument, \\smooth,\
// numéros de notes MIDI \-- convertis automatiquement en Hz\
\\midinote, Pseq(\[60, 72, 71, 67, 69, 71, 72, 60, 69, 67\], 1),\
// valeurs rythmiques\
\\dur, Pseq(\[2, 2, 1, 0.5, 0.5, 1, 1, 2, 2, 3\], 1)\
).play ;\
)\
La première chose à remarquer est la brièveté, la concision et la
propreté de la syntaxe. Rien n\'est en trop ; elle concentre toute votre
attention sur ce qui est censé être joué et minimise les distractions
liées à la logique du programme.\
\
La documentation de Streams explique comment tout cela fonctionne en
détail. La vue d\'ensemble de haut niveau est la suivante :\
\
- Le modèle Pbind génère des objets Event, qui contiennent des noms et
des valeurs décrivant comment la note est censée sonner.\
\
- Pour ce faire, il lit les paires \"name, pattern\", récupère les
valeurs de chaque flux de motifs à tour de rôle et les ajoute à
l\'événement résultant.\
\
- L\'événement est ensuite joué. Il interprète les valeurs conformément
à un ensemble de valeurs par défaut et de règles encodées dans le
prototype de l\'événement et exécute une action en réponse. L\'action
par défaut consiste à jouer un nouveau synthétiseur sur le serveur. Vous
pouvez choisir parmi plusieurs autres actions définies dans le prototype
d\'événement par défaut, qui sont documentées dans les fichiers d\'aide
de la série Streams.\
\
- Pour jouer le synthé, l\'événement doit savoir quelles valeurs
transmettre comme arguments au serveur. SuperCollider peut stocker des
informations sur un synthdef dans une bibliothèque de descriptions de
synthdef à l\'aide de la méthode add.\
\
- La valeur delta de l\'événement indique à SuperCollider combien de
temps il doit attendre avant de jouer l\'événement suivant.\
\
Un didacticiel d\'introduction ne peut pas couvrir toutes les
possibilités. Il est important d\'apprendre un ensemble de classes de
motifs de base ; la série de fichiers d\'aide Practical Guide to
Patterns constitue une introduction plus complète. Les manipulations de
motifs et les moyens de combiner ou d\'imbriquer des motifs ouvrent le
champ à presque tous les besoins de composition.\
\
Par exemple, nous pouvons générer une ligne de basse rythmique (mais pas
nécessairement métrique) en choisissant au hasard parmi un ensemble de
séquences Pbind. (Certaines de ces séquences utiliseront Pmono, qui est
une variante de Pbind conçue pour jouer des lignes de synthé
monophoniques). Bien qu\'il s\'agisse d\'un bloc de code plus important,
sa structure est assez simple et il rassemble plusieurs concepts
introduits dans les tutoriels de séquençage. Notez que l\'argument quant
to play est utilisé pour maintenir deux séquences distinctes ensemble
sur le rythme.\
\
Ne vous laissez pas intimider par le motif de la ligne de basse. A un
niveau plus élevé, il se réduit à Pxrand(\[a, b, c, d\], inf), qui
choisit simplement des éléments de manière aléatoire sans répéter aucun
d\'entre eux deux fois de suite. Il se peut que chaque élément soit un
modèle d\'événement qui joue une série de notes, mais cela n\'a pas
d\'importance pour Pxrand. Il choisit simplement un élément, le joue
jusqu\'à la fin, puis choisit le suivant, et ainsi de suite. Vu sous cet
angle, le motif est une expression élégante de l\'idée de sélection de
phrases. La représentation du code est facile à relier à une conception
musicale.\
(\
SynthDef(\\bass, { \|out, freq = 440, gate = 1, amp = 0.5, slideTime =
0.17, ffreq = 1100, width = 0.15,\
detune = 1.005, preamp = 4\|\
var sig, env ;\
env = Env.adsr(0.01, 0.3, 0.4, 0.1) ;\
freq = Lag.kr(freq, slideTime) ;\
sig = Mix(VarSaw.ar(\[freq, freq \* detune\], 0, width, preamp)).distort
\* amp\
\* EnvGen.kr(env, gate, doneAction : Done.freeSelf) ;\
sig = LPF.ar(sig, ffreq) ;\
Out.ar(out, sig ! 2)\
}).add ;\
\
TempoClock.default.tempo = 132/60 ;\
\
p = Pxrand(\[\
Pbind(\
\\Ninstrument, \\Nbasse,\
\\Nmidinote, 36,\
\\dur, Pseq(\[0.75, 0.25, 0.25, 0.25, 0.5\], 1),\
\\legato, Pseq(\[0.9, 0.3, 0.3, 0.3, 0.3\], 1),\
\\Namp, 0.5, \\Ndetune, 1.005\
),\
Pmono(\\bass,\
\\Nmidinote, Pseq(\[36, 48, 36\], 1),\
\\dur, Pseq(\[0.25, 0.25, 0.5\], 1),\
\\amp, 0.5, \\detune, 1.005\
),\
Pmono(\\bass,\
\\Nmidinote, Pseq(\[36, 42, 41, 33\], 1),\
\\dur, Pseq(\[0.25, 0.25, 0.25, 0.75\], 1),\
\\Namp, 0.5, \\Ndetune, 1.005\
),\
Pmono(\\bass,\
\\Nmidinote, Pseq(\[36, 39, 36, 42\], 1),\
\\dur, Pseq(\[0.25, 0.5, 0.25, 0.5\], 1),\
\\Namp, 0.5, \\Ndetune, 1.005\
)\
\], inf).play(quant : 1) ;\
)\
\
// totalement ringard, mais qui pourrait y résister ?\
(\
SynthDef(\\kik, { \|out, preamp = 1, amp = 1\|\
var freq = EnvGen.kr(Env(\[400, 66\], \[0.08\], -3)),\
sig = SinOsc.ar(freq, 0.5pi, preamp).distort \* amp\
\* EnvGen.kr(Env(\[0, 1, 0.8, 0\], \[0.01, 0.1, 0.2\]), doneAction :
Done.freeSelf) ;\
Out.ar(out, sig ! 2) ;\
}).add ;\
\
// avant de jouer :\
// que pensez-vous que \'\\delta, 1\' va faire ?\
k = Pbind(\\instrument, \\kik, \\delta, 1, \\preamp, 4.5, \\amp,
0.32).play(quant : 1) ;\
)\
\
p.stop ;\
k.stop ;\
Pour en savoir plus\
Streams, Streams-Patterns-Events, Guide pratique des patterns\
\
Exercices suggérés\
Choisissez un air familier et écrivez un Pbind pour lui, en utilisant la
synthdef de votre choix.\
Ajoutez autant de phrases que vous le souhaitez à la séquence de lignes
de basse de l\'exemple précédent.\
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\
\
Ce document fait partie du tutoriel \"Getting Started With
SuperCollider\".\
\
Cliquez ici pour revenir à la table des matières : 00. Premiers pas avec
SC\
\
source du fichier d\'aide : C:\\NProgram
Files\\NSuperCollider-3.13.0\\NHelpSource\\NTutoriels\\NPrise en
main\\N16-Séquençage-avec-modèles.schelp\
link::Tutorials/Getting-Started/16-Sequencing-with-Patterns: :
