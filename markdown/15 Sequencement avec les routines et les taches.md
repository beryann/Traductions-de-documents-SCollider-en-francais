Table des Matières ▼ SuperColliderBrowseSearchIndexes\
SuperColliderBrowseSearchIndexes ▼\
Tutoriels / Mise en route \| Tutoriels \> Mise en route\
15. Séquencement avec les routines et les tâches\
Démarrer avec SuperCollider\
Voir aussi : 00. Premiers pas avec SC\
Lorsque vous planifiez une fonction (comme dans le tutoriel
Planification d\'événements), la fonction commence toujours au début et
s\'exécute jusqu\'à la fin. Pour le séquençage, il est plus utile
d\'avoir une structure de contrôle qui peut s\'exécuter en partie,
renvoyer une valeur, puis reprendre là où elle s\'est arrêtée la
prochaine fois qu\'on en a besoin. Dans SuperCollider, il s\'agit d\'une
routine.\
\
Les routines peuvent être utilisées pour le traitement des données, par
exemple\
(\
r = Routine({\
\"abcde\".yield ;\
\"fghij\".yield ;\
\"klmno\".yield ;\
\"pqrst\".yield ;\
\"uvwxy\".yield ;\
\"z{\|}\~\".yield ;\
}) ;\
)\
\
r.next ; // obtient la valeur suivante de la routine\
6.do({ r.next.postln }) ;\
La première fois que vous appelez next, la routine produit \"abcde\".
Cette valeur devient le résultat de r.next et est imprimée dans la
fenêtre post. Lors du deuxième appel à next, l\'exécution reprend juste
après le premier résultat et continue avec la deuxième chaîne, et ainsi
de suite. Lorsqu\'il n\'y a plus rien à produire, r.next renvoie nil.\
\
Nous reviendrons sur l\'utilisation des routines pour la génération de
données. Ce qui est plus important pour le séquençage, c\'est ce qui se
passe lorsque vous programmez une routine sur une horloge et que la
routine renvoie des valeurs temporelles.\
\
Programmation de routines\
Rappelez-vous que, lorsque vous programmez une fonction sur une horloge,
les nombres renvoyés par la fonction sont traités comme des valeurs
temporelles - en particulier, le temps qui s\'écoule avant que la
fonction ne s\'exécute à nouveau. Il en va de même pour les nombres
renvoyés par une routine.\
(\
r = Routine({\
var delta ;\
boucle {\
delta = rrand(1, 3) \* 0.5 ;\
\"attendra \".post ; delta.postln ;\
delta.yield ;\
}\
}) ;\
)\
\
r.next ;\
\
TempoClock.default.sched(0, r) ;\
\
r.stop ;\
Remplaçons maintenant les instructions d\'écriture par des instructions
permettant de jouer sur un synthétiseur. Préparation :\
(\
SynthDef(\\singrain, { \|freq = 440, amp = 0.2, sustain = 1, out\|\
var sig ;\
sig = SinOsc.ar(freq, 0, amp) \* EnvGen.kr(Env.perc(0.01, sustain),
doneAction : Done.freeSelf) ;\
Out.ar(out, sig ! 2) ; // sig ! 2 est identique à \[sig, sig\]\
}).add ;\
\
r = Routine({\
var delta ;\
boucle {\
delta = rrand(1, 3) \* 0.5 ;\
Synth(\\singrain, \[freq : exprand(200, 800), amp : rrand(0.1, 0.5),
sustain : delta \* 0.8\]) ;\
delta.yield ;\
}\
}) ;\
)\
La programmation d\'une routine a un certain sens, mais la lecture
d\'une routine semble plus intuitive.\
r.play ;\
\
r.stop ;\
Et voilà, notre première séquence.\
\
Pause et reprise : Tâche\
Les routines ont une petite caractéristique collante qui peut limiter
leur utilité en tant qu\'objets musicaux. Une fois que vous avez arrêté
une routine, vous ne pouvez la reprendre qu\'à partir du début. Il n\'y
a aucun moyen de rejouer la routine à partir du point où elle a été
arrêtée.\
\
La tâche est une variation qui peut être interrompue et reprise à
volonté. Par exemple, itérons sur une gamme de do majeur. Notez que
toutes les structures de contrôle de SuperCollider sont valables à
l\'intérieur d\'une routine ou d\'une tâche. Notez également que nous
pouvons utiliser \'wait\' comme synonyme de \'yield\'.\
(\
t = Task({\
boucle {\
\[60, 62, 64, 65, 67, 69, 71, 72\].do({ \|midi\|\
Synth(\\singrain, \[freq : midi.midicps, amp : 0.2, sustain : 0.1\]) ;\
0.125.wait ;\
}) ;\
}\
}).play ;\
)\
\
// s\'arrête probablement au milieu de la gamme\
t.stop ;\
\
t.play ; // devrait reprendre avec la note suivante\
\
t.stop ;\
La tâche sera utilisée pour le reste de ce tutoriel.\
\
Quand voulez-vous commencer ?\
Par défaut, la lecture appliquée à une tâche démarre cette dernière
immédiatement. La plupart du temps, plusieurs tâches s\'exécutent
simultanément et doivent être synchronisées. Bien qu\'il existe
peut-être un virtuose capable d\'appuyer sur la touche Entrée au bon
moment pour une synchronisation précise, la plupart d\'entre nous
préféreraient un mécanisme plus fiable.\
\
Play prend plusieurs arguments pour contrôler son comportement.\
aRoutine.play(clock, quant)\
aTask.play(argClock, doReset, quant)\
clock (Routine) ou argClock (Task)\
Quelle horloge doit gérer l\'ordonnancement de cette séquence\
doReset (tâche uniquement)\
Si vrai, réinitialise la séquence au début avant de la jouer ; si faux
(par défaut), la reprend\
quant\
Spécification de l\'heure exacte de début de la séquence\
L\'argument quant utilise un modèle de base de deux nombres, qui peut
être relié au concept occidental de mètre :\
\
quant : Correspond à peu près à la longueur de la mesure ; le temps
actuel est arrondi au multiple supérieur de ce nombre phase : Position
dans la mesure (0 = début de la mesure)\
\
Par commodité, si vous voulez simplement commencer au début de la
mesure, vous pouvez donner la longueur de la mesure sous la forme d\'un
nombre. Un tableau de deux nombres indique à SuperCollider la longueur
de la barre et la phase.\
\
Pour voir comment cela fonctionne en pratique, prenons la gamme de do
majeur ci-dessus et jouons-en deux copies légèrement décalées. Nous
ralentirons le rythme à la double croche (0,25) et commencerons la
seconde croche dans la mesure. Pour ce faire, nous aurons besoin de deux
tâches, qui seront fabriquées dans une fonction.\
(\
f = {\
Task({\
loop {\
\[60, 62, 64, 65, 67, 69, 71, 72\].do({ \|midi\|\
Synth(\\singrain, \[freq : midi.midicps, amp : 0.2, sustain : 0.1\]) ;\
0.25.wait ;\
}) ;\
}\
}) ;\
} ;\
)\
\
t = f.value.play(quant : 4) ; // démarre sur la prochaine frontière à 4
temps\
\
u = f.value.play(quant : \[4, 0.5\]) ; // frontière suivante à 4 temps +
un demi temps\
\
t.stop ; u.stop ;\
Utilisation de routines de données dans le séquençage des notes\
L\'exemple précédent contrôle la génération d\'un paramètre (hauteur) en
bouclant sur un tableau à l\'intérieur de la tâche. Que se passe-t-il si
vous souhaitez contrôler plusieurs paramètres ?\
\
N\'oubliez pas que les routines peuvent également générer des données,
en plus de leurs capacités de programmation. Vous pouvez faire référence
à autant de routines de données que vous le souhaitez dans votre
séquence.\
(\
var midi, dur ;\
midi = Routine({\
\[60, 72, 71, 67, 69, 71, 72, 60, 69, 67\].do({ \|midi\| midi.yield })
;\
}) ;\
dur = Routine({\
\[2, 2, 1, 0.5, 0.5, 1, 1, 2, 2, 3\].do({ \|dur\| dur.yield }) ;\
}) ;\
\
SynthDef(\\smooth, { \|freq = 440, sustain = 1, amp = 0.5, out\|\
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
Notez que les routines sont utilisées pour les données, mais que la
tâche est utilisée pour la lecture. De plus, contrairement aux séquences
infinies précédentes, celle-ci s\'arrête lorsqu\'il n\'y a plus de
données. C\'est l\'objectif de la boucle while - elle ne continue que
tant que le flux de données \"dur\" continue à fournir des valeurs.
(Voir le fichier d\'aide des structures de contrôle pour plus
d\'informations sur la boucle while).\
\
Il doit y avoir un moyen plus simple d\'écrire les flux de données -
écrire de façon répétée la même boucle do est certainement peu pratique.
En fait, il existe une telle méthode, abordée dans le prochain tutoriel
: le séquençage avec des motifs.\
\
(Ici, nous utilisons quant simplement pour retarder le début de la tâche
d\'un temps. C\'est parce qu\'il faut un certain temps pour que le
synthdef soit prêt à être utilisé sur le serveur. Sans cela, la première
note ne serait pas entendue).\
\
Note sur la messagerie et la synchronisation du serveur\
L\'utilisation de Synth comme dans les exemples précédents peut
entraîner des imprécisions de timing minimes mais parfois perceptibles.
En effet, la transmission des messages OSC de votre code au serveur
prend peu de temps et ce temps n\'est pas toujours constant.
SuperCollider résout ce problème en vous donnant la possibilité
d\'envoyer le message avec un horodatage indiquant au serveur le moment
exact où le message doit prendre effet. Une valeur de latence est
utilisée pour calculer l\'horodatage.\
\
Le temps de latence s\'ajoute à l\'heure actuelle de l\'horloge. Si tous
les messages sont envoyés avec la même valeur de latence, leur timing
sera précis les uns par rapport aux autres et par rapport à l\'horloge.
Le fichier d\'aide de l\'ordonnancement et de la synchronisation du
serveur explique plus en détail comment cela fonctionne, mais vous
n\'avez pas besoin de tout savoir pour l\'utiliser. L\'essentiel est
d\'utiliser une valeur de latence faible et cohérente pour une
synchronisation parfaite. (Un objet Serveur possède une variable de
latence que vous pouvez utiliser pour la cohérence).\
\
Voici un exemple illustrant les types d\'imprécisions que vous pourriez
entendre. L\'imprécision peut être plus ou moins visible sur différents
systèmes. Il utilise le SynthDef \\singrain ci-dessus et joue 10 notes
par seconde.\
(\
t = Task({\
loop {\
Synth(\\singrain, \[freq : exprand(400, 1200), sustain : 0.08\]) ;\
0.1.wait ;\
}\
}).play ;\
)\
\
t.stop ;\
La façon la plus simple d\'ajouter de la latence à vos Synths sortants
est d\'utiliser la méthode makeBundle du serveur. Ne vous préoccupez pas
de son fonctionnement pour l\'instant \-- l\'important est qu\'elle
utilise la première valeur pour la latence, et qu\'elle exécute les
messages produits par la fonction en fonction de cette latence.\
(\
t = Task({\
loop {\
s.makeBundle(s.latency, {\
Synth(\\singrain, \[freq : exprand(400, 1200), sustain : 0.08\]) ;\
}) ;\
0.1.wait ;\
}\
}).play ;\
)\
\
t.stop ;\
Voir aussi :\
\
Routine, Tâche, Quant, Ordonnancement et synchronisation du serveur,
Messages groupés du serveur\
\
Exercice suggéré\
Créer un SynthDef plus intéressant pour remplacer le SynthDef lisse.
Utilisez plus d\'arguments pour une plus grande variabilité. Modifiez
ensuite les flux de données dans l\'exemple \"Over the Rainbow\" et
ajoutez de nouveaux flux de données pour jouer un air différent, de
manière plus expressive.\
\
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\
\
Ce document fait partie du tutoriel \"Getting Started With
SuperCollider\".\
\
Cliquez ici pour passer à la section suivante : 16. Séquencer avec des
motifs\
\
Cliquez ici pour revenir à la table des matières : 00. Premiers pas avec
SC\
\
Source du fichier d\'aide : C:\\NProgram
Files\\NSuperCollider-3.13.0\\NHelpSource\\NTutorials\\NGetting-Started\\N15-Sequencing-with-Routines-and-Tasks.schelp\
link::Tutorials/Getting-Started/15-Sequencing-with-Routines-and-Tasks: :
