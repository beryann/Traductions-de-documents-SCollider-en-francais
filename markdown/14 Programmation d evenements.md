Table des Matières ▼ SuperColliderBrowseSearchIndexes\
SuperColliderBrowseSearchIndexes ▼\
Tutoriels / Mise en route \| Tutoriels \> Mise en route\
14. Programmation d\'événements\
Démarrer avec SuperCollider\
Voir aussi : 00. Premiers pas avec SC\
La musique se produit au fil du temps, et pour faire de la musique
efficace, il est nécessaire de contrôler le moment où les choses se
produisent. Dans SuperCollider, cela se fait en programmant les choses
sur des horloges.\
\
Horloges\
Une horloge dans SuperCollider a deux fonctions principales. Elle sait
quelle heure il est, et elle sait à quelle heure les choses sont censées
se produire, afin de les réveiller au bon moment.\
\
Le séquençage musical utilise généralement TempoClock, car il est
possible de modifier le tempo et l\'horloge est également consciente des
changements de mesure. Il existe deux autres types d\'horloge :
SystemClock, qui s\'exécute toujours en secondes, et AppClock, qui
s\'exécute également en secondes mais dont la priorité système est moins
élevée (elle est donc préférable pour les mises à jour graphiques et les
autres activités qui ne sont pas critiques en termes de temps).\
\
Planification\
Programmer signifie dire à l\'horloge d\'exécuter quelque chose à un
moment donné dans le futur. Vous devez donc disposer de la chose à
programmer et d\'un nombre indiquant l\'heure.\
\
Faisons en sorte que SuperCollider dise bonjour dans 5 secondes.\
SystemClock.sched(5, { \"hello\".postln }) ;\
Remarquez que lorsque vous faites cela, \'SystemClock\' s\'imprime
immédiatement. Chaque fois que vous exécutez quelque chose dans
SuperCollider, cette méthode doit renvoyer une valeur immédiatement ; la
valeur de retour de la méthode est l\'horloge. Cependant, avant de
retourner la valeur, l\'horloge se \"souvient\" de la fonction et du
fait que vous vouliez qu\'elle s\'exécute 5 secondes plus tard. Et en
effet, \"hello\" apparaît dans la fenêtre de postage, juste au bon
moment. {\"hello\".postln } est une action asynchrone : elle s\'exécute
après que son bloc de code est déjà revenu.\
\
sched fait de l\'ordonnancement relatif. Le moment où la fonction
s\'exécute est postérieur de x secondes (ou battements, pour TempoClock)
au moment où l\'appel à .sched s\'est produit. Il est également possible
d\'ordonnancer à un moment précis, à condition de connaître l\'heure sur
l\'horloge. schedAbs gère l\'ordonnancement absolu.

\
(\
var timeNow = TempoClock.default.beats ;\
\"L\'heure est maintenant : \".post ; timeNow.postln ;\
\"Programmation pour : \".post ; (timeNow + 5).postln ;\
TempoClock.default.schedAbs(timeNow + 5,\
{\"L\'heure est plus tardive : \".post ; thisThread.clock.beats.postln ;
nil }) ;\
)

\
Notez que nous sommes passés à TempoClock, car c\'est le plus couramment
utilisé. Alors qu\'il n\'y a qu\'une seule SystemClock, il peut y avoir
plusieurs TempoClocks fonctionnant tous à des vitesses différentes, si
nécessaire. Un TempoClock est celui par défaut, accessible par
TempoClock.default \-- nous l\'utiliserons tout au long de ce document.
(Pour faciliter la saisie, vous pouvez assigner un TempoClock à une
variable, par exemple, t = TempoClock.default).\
\
Pour le plaisir, changez le tempo et exécutez à nouveau le dernier
exemple :\
(\
var timeNow ;\
TempoClock.default.tempo = 2 ; // 2 battements/sec, ou 120 BPM\
timeNow = TempoClock.default.beats ;\
\"L\'heure est maintenant : \".post ; timeNow.postln ;\
\"Programmation pour : \".post ; (timeNow + 5).postln ;\
TempoClock.default.schedAbs(timeNow + 5,\
{\"L\'heure est plus tardive : \".post ; thisThread.clock.beats.postln ;
nil }) ;\
)\
Remarquez que le message \"L\'heure est plus tardive\" apparaît après un
délai plus court, mais que la différence entre les deux heures est
toujours de 5.\
\
Quelle heure est-il ?\
A l\'intérieur d\'une fonction programmée, vous pouvez vouloir savoir
quelle horloge exécute la fonction. thisThread.clock vous le dit \-- ne
vous préoccupez pas pour l\'instant de savoir comment il le sait, sachez
simplement que vous pouvez l\'utiliser pour le savoir.\
\
Une fois que vous connaissez l\'horloge, vous pouvez savoir quelle heure
il est en utilisant beats :\
SystemClock.beats ;\
TempoClock.default.beats ;\
AppClock.beats ;\
thisThread.clock.beats ;\
Que pouvez-vous programmer ?\
Supposons que nous programmions \"hello\" tout seul.\
TempoClock.default.sched(5, \"hello\") ;\
Rien ne se passe. C\'est parce que \"hello\" est juste une valeur - elle
ne fait rien. La leçon à en tirer est qu\'il est judicieux de programmer
des objets qui effectueront une action.\
Fonction\
Routine\
Tâche\
Les routines et les tâches seront abordées dans la section suivante, et
nous avons déjà vu les fonctions. Il en existe d\'autres, mais celles-ci
constituent le meilleur point de départ.\
\
Attention\
Si vous programmez une fonction qui renvoie un nombre, l\'horloge
considérera ce nombre comme le temps qui s\'écoulera avant que la
fonction ne soit exécutée à nouveau.\
// se déclenche plusieurs fois (mais on dirait qu\'il ne devrait se
déclencher qu\'une seule fois)\
TempoClock.default.sched(1, { rrand(1, 3).postln ; }) ;\
Cette fonction s\'exécutera indéfiniment, jusqu\'à ce que vous
l\'arrêtiez à l\'aide de cmd-.\
\
Si vous voulez que la fonction ne s\'exécute qu\'une seule fois,
assurez-vous de terminer la fonction par \'nil\' :

\
// ne s\'exécute qu\'une seule fois\
TempoClock.default.sched(1, { rrand(1, 3).postln ; nil }) ;

\
Il est facile de renvoyer un nombre par erreur et d\'obtenir une
activité continue alors que vous vouliez une action ponctuelle.\
\
Si ce nombre est 0 ou négatif, il se passe quelque chose de pire. La
fonction s\'exécutera à nouveau immédiatement. Et si le nombre est
toujours 0, cela crée une boucle infinie qui peut bloquer
SuperCollider.\
\
Cela ne devrait pas vous effrayer et vous empêcher d\'utiliser la
programmation - ce problème est moins susceptible de se produire avec
les Routines et les Tâches, que vous utiliserez plus souvent. Mais vous
devez en être conscient.\
\
Pour en savoir plus : SystemClock, TempoClock, AppClock, Function\
\
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\
\
Ce document fait partie du tutoriel Getting Started With SuperCollider.\
\
Cliquez ici pour passer à la section suivante : 15. Séquencement avec
les routines et les tâches\
\
Cliquez ici pour revenir à la table des matières : 00. Démarrer avec SC\
\
source du fichier d\'aide : C:\\NProgram
Files\\NSuperCollider-3.13.0\\NHelpSource\\NTutorials\\NGetting-Started\\N14-Scheduling-Events.schelp\
link::Tutorials/Getting-Started/14-Scheduling-Events: :
