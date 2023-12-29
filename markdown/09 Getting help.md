Table des Matières ▼ SuperColliderBrowseSearchIndexes\
SuperColliderBrowseSearchIndexes ▼\
Tutoriels/Démarrage \| Tutoriels \> Démarrage\
09. Obtenir de l\'aide\
Démarrer avec SuperCollider\
Voir aussi : 00. Débuter avec SC\
C\'est probablement le bon moment pour s\'arrêter et explorer quelques
méthodes pour trouver plus d\'informations. Vous êtes déjà familier avec
les liens cliquables qui ont été utilisés jusqu\'à présent dans ce
tutoriel. En voici un exemple :\
\
Aide\
\
En cliquant sur ce lien, vous ouvrirez la fenêtre d\'aide principale,
qui contient un certain nombre de liens vers d\'autres fichiers d\'aide.
Il serait bon, à un moment ou à un autre, de vous familiariser avec
certains de ces fichiers. Encore une fois, ne vous inquiétez pas si tout
n\'est pas immédiatement clair pour vous. Apprendre un langage
informatique, c\'est parfois un peu comme se concentrer lentement sur
quelque chose, plutôt que de l\'obtenir immédiatement, et certaines
informations peuvent être conservées pour référence ultérieure.\
\
Classes et méthodes\
Nous avons maintenant appris suffisamment de théorie de la POO pour
savoir que nous avons des classes, qui sont comme des modèles d\'objets,
et des instances, qui sont des objets créés à partir de ces modèles.
Nous avons également des méthodes de classe et d\'instance, qui peuvent
prendre des arguments. Les méthodes de classe permettent de créer des
instances (ainsi que certaines fonctions de commodité qui ne nécessitent
pas d\'instance réelle), tandis que les méthodes d\'instance contrôlent
et manipulent les instances. Il existe également des variables
d\'instance, qui sont des données spécifiques à chaque instance, et des
variables de classe, qui sont des données communes à toutes les
instances.\
\
Rappelez-vous que tout ce qui, dans le code, commence par une lettre
majuscule est une classe. La plupart des classes disposent de fichiers
d\'aide. Si vous sélectionnez une classe en double-cliquant dessus et
que vous appuyez sur Cmd - d (c\'est-à-dire que vous maintenez la touche
Cmd enfoncée et que vous appuyez sur la touche d), le fichier d\'aide de
cette classe s\'ouvrira s\'il existe. (Si ce n\'est pas le cas, vous
obtiendrez la fenêtre d\'aide principale.) Essayez-le avec l\'exemple
ci-dessous :\
SinOsc\
Vous devriez obtenir une fenêtre avec une brève description de la classe
et de ce qu\'elle fait, une liste de quelques méthodes et une
description de leurs arguments. (N\'oubliez pas que \'mul\' et \'add\'
ne sont généralement pas expliqués).\
\
En dessous se trouvent des exemples de la classe en action. Ces exemples
peuvent être très utiles pour clarifier ce que fait exactement la classe
et peuvent servir de point de départ à votre propre travail. Il est
conseillé de les copier et de les coller dans une nouvelle fenêtre, puis
de les modifier. (N\'oubliez pas que SC ne vous empêchera pas de
sauvegarder les fichiers modifiés, y compris ce tutoriel). C\'est une
excellente façon d\'apprendre.\
\
Vous vous demandez peut-être comment accéder aux fichiers d\'aide pour
Function et Array, puisqu\'ils apparaissent souvent dans le code sous la
forme {\...} et \[\...\]. Ce sont aussi des classes nommées, donc en
tapant ce qui suit, vous pouvez aussi les sélectionner et faire Cmd- ?
sur elles.\
Fonction\
Tableau\
Certaines méthodes ont également des fichiers d\'aide, et il en existe
un certain nombre sur des sujets généraux. La plupart d\'entre elles
sont répertoriées dans la fenêtre d\'aide principale.\
\
Raccourcis syntaxiques\
Vous vous souvenez de l\'exemple de Mix(\...) par rapport à
Mix.new(\...) ? SC dispose d\'un certain nombre de formes abrégées ou de
syntaxes alternatives. Un exemple courant est la distinction entre la
notation fonctionnelle et la notation du récepteur. Cela signifie que la
notation someObject.someMethod(anArg) est équivalente à
someMethod(someObject, anArg). Voici un exemple concret. Les deux font
exactement la même chose :\
{ SinOsc.ar(440, 0, 0.2) }.play ;\
\
play({ SinOsc.ar(440, 0, 0.2) }) ;\
Vous trouverez de nombreux autres exemples de raccourcis syntaxiques
dans la documentation de SC. Si vous voyez quelque chose que vous ne
reconnaissez pas, vous pouvez consulter la rubrique Raccourcis
syntaxiques, qui donne des exemples de la plupart de ces raccourcis.\
\
Surveillance, etc.\
SC dispose de nombreuses autres méthodes pour trouver des informations
sur les classes, les méthodes, etc. La plupart d\'entre elles ne vous
seront pas très utiles à ce stade, mais il est bon de les connaître pour
une utilisation future. Des informations sur ces méthodes peuvent être
trouvées dans les fichiers More on Getting Help et Internal Snooping
(Introspection).\
\
Pour plus d\'informations, voir\
\
En savoir plus sur l\'obtention d\'aide, Surveillance interne
(Introspection), Raccourcis syntaxiques\
\
Exercice suggéré\
Revenez sur les exemples des tutoriels précédents et essayez d\'ouvrir
les fichiers d\'aide des différentes classes utilisées. Essayez les
exemples, et si vous le souhaitez, ouvrez les fichiers d\'aide pour les
classes non familières utilisées dans ces exemples. Habituez-vous à
Cmd-d, vous l\'utiliserez souvent :-)\
\
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\
\
Ce document fait partie du tutoriel \"Getting Started With
SuperCollider\".\
\
Cliquez ici pour passer à la section suivante : 10. SynthDefs et Synths\
\
Cliquez ici pour revenir à la table des matières : 00. Démarrer avec SC\
\
Source du fichier d\'aide : C:\\NProgram
Files\\NSuperCollider-3.13.0\\NHelpSource\\NTutorials\\NGetting-Started\\N09-Getting-Help.schelp\
link::Tutorials/Getting-Started/09-Getting-Help: :
