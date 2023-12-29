Table des Matières ▼ SuperColliderBrowseSearchIndexes\
SuperColliderBrowseSearchIndexes ▼\
Tutoriels/Démarrage \| Tutoriels \> Démarrage

02\. Premiers pas

\
Débuter avec SuperCollider\
Voir aussi : 00. Premiers pas avec SC\
NOTE : Ce document est écrit de manière à pouvoir être utilisé sur tous
les systèmes supportés, en ayant à l\'esprit avant tout macOS, en
utilisant l\'application SuperCollider.app (SCapp). Certaines
fonctionnalités, par exemple les commandes de menu, sont spécifiques à
une plate-forme, mais les principes s\'appliquent à toutes les
plates-formes. Voir le fichier d\'aide Raccourcis clavier pour les
commandes clés lors de l\'utilisation d\'autres outils tels que les
éditeurs améliorés SC, par exemple sur les plates-formes Linux.\
Bonjour le monde, je suis SuperCollider\
Lors de l\'apprentissage d\'un nouveau langage de programmation, il est
de coutume de commencer par un programme simple appelé \"Hello World\".
Ce programme imprime simplement le texte \"Hello World\" à l\'endroit où
il imprime du texte. Dans SC, c\'est un endroit appelé la fenêtre
d\'affichage. La fenêtre post est celle qui s\'est ouverte lorsque vous
avez démarré SC pour la première fois, et un tas de choses y ont été
imprimées qui ressemblent à ceci :\
init_OSC\
compilation de la bibliothèque de classe..\
NumPrimitives = 587\
répertoire de compilation : \'/Applications/SC3/SCClassLibrary\'\
passe 1 fait\
Taille de la table des méthodes 3764776 octets\
Nombre de sélecteurs de méthodes 3184\
Nombre de classes 1814\
Nombre de symboles 7595\
Taille du code en octets 180973\
compilation de 296 fichiers en 1,34 secondes\
compilation terminée\
RESULTAT = 256\
Arbre de classe initié en 0,14 seconde\
Ne vous préoccupez pas trop de ce que tout cela signifie pour
l\'instant, gardez simplement à l\'esprit que c\'est là que SC vous
enverra des informations. C\'est également là que nous obtiendrons le
résultat de notre programme Hello World, que vous pouvez voir ci-dessous
:\
\"Hello World !\".postln ;\
Pour l\'exécuter, il suffit de cliquer pour placer le curseur quelque
part sur la même ligne que le code, puis d\'appuyer sur Shift-Enter
(Shift-Return sur macOS). Essayez maintenant.\
\
Si tout s\'est bien passé, vous devriez voir ceci dans la fenêtre de
post.\
Hello World !\
-\> Hello World !\
Vous pouvez également sélectionner le code que vous souhaitez exécuter
(en cliquant sur le texte et en le faisant glisser jusqu\'à ce qu\'il
soit mis en surbrillance), puis appuyer sur Ctrl-Entrée (Cmd-Retour sur
macOS). Essayez-le maintenant.\
\
Examinons maintenant le code de plus près. La première partie, \"Hello
World !\", est une sorte d\'objet, appelé chaîne de caractères. Un objet
est essentiellement une façon de représenter quelque chose dans
l\'ordinateur, par exemple un morceau de texte ou un oscillateur, qui
vous permet de le contrôler et de lui envoyer des messages. Nous y
reviendrons plus tard, mais pour l\'instant, il suffit de comprendre
qu\'une chaîne est une façon de représenter un morceau de texte.\
\
Le deuxième bit, .postln ;, dit \"imprimez-moi (ou une description
significative de moi) dans la fenêtre post\". N\'oubliez pas postln,
c\'est votre ami. Vous pouvez l\'appliquer à presque n\'importe quoi
dans SC et obtenir quelque chose de significatif en retour. Cela peut
s\'avérer très pratique lorsque vous recherchez des bogues dans votre
code.\
\
Pourquoi cela s\'est-il imprimé deux fois ? Eh bien, lorsque vous
exécutez du code dans SC, il affiche toujours le résultat du dernier
morceau de code (la dernière instruction). Il imprime donc d\'abord
parce que nous lui avons explicitement demandé d\'imprimer, puis il
imprime le résultat de cette opération, qui se trouve être la même dans
ce cas.\
\
Dans ce cas, nous n\'avons donc pas vraiment besoin du bit postln. Mais
dans l\'exemple suivant, nous en aurions besoin. Sélectionnez les deux
lignes de texte en cliquant dessus et en les faisant glisser, puis
exécutez, c\'est-à-dire appuyez sur Ctrl-Entrée (Cmd-Retour sur macOS).\
\"Bonjour, je suis SuperCollider !\".postln ;\
Bonjour, je suis SuperCollider !\".postln ; \"Hello World !\".postln ;\
La première ligne, \"Hello there, I\'m SuperCollider !\", ne se serait
pas imprimée si nous n\'avions pas eu le postln explicite.\
\
En général, lorsque vous devez exécuter plusieurs lignes de code en même
temps, elles sont entourées de parenthèses, comme dans l\'exemple
ci-dessous. Vous pouvez placer votre curseur n\'importe où dans cette
région (ou sur la ligne des parenthèses sous macOS), puis double-cliquer
et appuyer sur Ctrl-Entrée ou Shift-Entrée (Cmd-Return ou Shift-Return
sous macOS) - cela sélectionne toute la région et l\'exécute. Essayez-le
dans l\'exemple ci-dessous.\
(\
\"Appelez-moi\".postln ;\
\"Ishmael.\".postln ;\
)\
Un double clic à l\'intérieur d\'une paire de parenthèses peut ne pas
sélectionner toute la région sur tous les systèmes, notamment sur les
Mac modernes. Sur certains, il se peut qu\'il ne sélectionne que le mot
doublement cliqué. Il peut donc être utile de prendre l\'habitude de
s\'en tenir à une technique particulière. Par exemple, vérifiez toujours
que vous avez sélectionné tout le code voulu, quelle que soit la
technique de sélection, avant d\'appuyer sur Ctrl-Enter ou Shift-Enter
(Cmd-Return ou Shift-Enter sur macOS). Ensuite, adoptez la technique qui
vous convient le mieux. Faites des essais pour savoir comment votre
système se comporte.\
\
Lorsque le code n\'est pas entouré de parenthèses, il est généralement
destiné à être exécuté une ligne à la fois. Vous pouvez placer votre
curseur n\'importe où dans une ligne de code et appuyer sur Ctrl-Enter
ou Shift-Enter (Cmd-Return ou Shift-Return sur macOS) - cela sélectionne
la ligne entière et l\'exécute.\
\
Notez que chaque ligne du bloc de code se termine par un point-virgule.
C\'est très important lorsque l\'on exécute plusieurs lignes de code.
Essayez ce qui se passe lorsque vous exécutez la variante suivante du
code presque identique.\
(\
\"Call me ?\".postln\
\"Ishmael.\".postln ;\
)\
L\'exécution du code ci-dessus entraîne une \"erreur d\'analyse\". Avec
une erreur de ce type, le point dans le message d\'erreur vous indique
l\'endroit où SC a rencontré un problème. Ici, il apparaît juste après
\"Ishmael\".\
ERROR : syntax error, unexpected STRING, expecting DOTDOT or \':\' or
\',\' or \')\'\
dans le texte interprété\
ligne 3 char 10 :\
\
\"Ishmael.\".postln ;\
\^\^\^\^\^\^\^\^\^\
)\
En général, le problème survient un peu avant, c\'est donc là qu\'il
faut chercher. Dans ce cas, il s\'agit de l\'absence de point-virgule à
la fin de la ligne précédente. Notez que chaque ligne de code se termine
normalement par un point-virgule. C\'est ainsi que l\'on sépare les
lignes de code dans SC. Comme nous n\'avons pas de point-virgule entre
les deux lignes, nous avons obtenu une erreur.\
\
Notez également que le fait d\'avoir un point-virgule supplémentaire à
la toute fin du dernier morceau de code ne nuit pas et est toléré par SC
pour des raisons de commodité.\
\
Quelques remarques supplémentaires à propos de la fenêtre d\'affichage.
Il est très utile de pouvoir la voir, mais elle peut parfois être cachée
derrière d\'autres fenêtres. Vous pouvez la faire apparaître à tout
moment en appuyant sur Cmd-\\.\
\
Dans d\'autres cas, la fenêtre d\'affichage devient pleine de texte et
difficile à lire. Vous pouvez l\'effacer à tout moment en appuyant sur
Ctrl-Maj-P (Cmd-Maj-P sur macOS).\
\
Le monde selon SuperCollider\
SuperCollider est en fait constitué de trois programmes :\
\
L\'éditeur de texte que vous regardez (également appelé IDE ou
Environnement de Développement Intégré),\
le langage (sclang ou l\'application \"client\"),\
et le serveur, qui effectue la synthèse et le calcul de l\'audio.\
La partie sclang est un langage de programmation sophistiqué avec des
fonctionnalités intéressantes pour la construction d\'interfaces
utilisateur graphiques (GUI) ; et la partie serveur est une application
en ligne de commande UNIX mince, moyenne et efficace (ce qui signifie
qu\'elle s\'exécute sans aucune représentation GUI).\
\
Ils communiquent par le biais d\'un protocole appelé OSC (Open Sound
Control), via UDP (User Datagram Protocol) ou TCP (Transmission Control
Protocol), qui sont des protocoles de réseau également utilisés sur
l\'internet. Étant donné que le client et le serveur communiquent de
cette manière, les projets les plus avancés peuvent les faire
fonctionner sur des ordinateurs distincts pour des raisons de
performance. En fait, il est même possible qu\'ils fonctionnent dans
différentes parties du monde ! Cependant, ce n\'est pas parce que ces
deux applications communiquent à l\'aide de protocoles internet communs
qu\'elles doivent être connectées à l\'internet ou installées sur des
ordinateurs différents. La plupart du temps, elles fonctionneront sur le
même ordinateur et l\'aspect \"réseau\" sera relativement transparent
pour vous. Surtout lorsque vous commencez à travailler.\
\
Vous ne pouvez communiquer avec le serveur qu\'en utilisant des messages
OSC sur le réseau, mais heureusement, l\'application langage possède de
nombreux objets puissants qui représentent les choses sur le serveur et
vous permettent de les contrôler facilement et élégamment. Il est
essentiel de comprendre comment cela fonctionne exactement pour
travailler efficacement avec SC, c\'est pourquoi nous allons en parler
plus en détail.\
\
Mais d\'abord, amusons-nous un peu et faisons du son !\
\
Pour plus d\'informations, voir\
\
Comment utiliser l\'interpréteur, Littéraux, Chaînes, Client vs Serveur,
Architecture du serveur\
\
Exercice suggéré\
Ouvrez un nouvel onglet ou une nouvelle fenêtre en appuyant sur Ctrl-N
(Cmd-N sur macOS) ou en choisissant \"Nouveau\" dans le menu Fichier.
Enregistrez le document en lui donnant un nom tel que \"Mon premier code
SC.scd\". Notez que vous devez toujours utiliser l\'extension \'.scd\'
pour les fichiers contenant du code SC. Copiez certains des exemples de
code ci-dessus et collez-les dans le nouveau document en utilisant
Ctrl-C et Ctrl-V (Cmd-C et Cmd-V sur macOS) ou en utilisant les éléments
du menu Edition pour copier et coller.\
\
SC vous permettra d\'éditer les fichiers d\'aide et la documentation,
c\'est donc toujours une bonne idée de copier le texte avant de le
modifier pour éviter d\'enregistrer accidentellement des fichiers
d\'aide modifiés !\
\
Essayez de modifier le texte entre les guillemets pour imprimer
différentes choses dans la fenêtre d\'affichage. Faites-le avec des
blocs de texte entre parenthèses et des lignes simples.\
\
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\
\
Ce document fait partie du tutoriel Premiers pas avec SuperCollider.\
\
Cliquez ici pour passer à la section suivante : 03. Démarrez vos
moteurs\
\
Cliquez ici pour revenir à la table des matières : 00. Démarrer avec SC\
\
Source du fichier d\'aide : C:\\NProgram
Files\\NSuperCollider-3.13.0\\NHelpSource\\NTutorials\\
