Table des Matières ▼ SuperColliderBrowseSearchIndexes\
SuperColliderBrowseSearchIndexes ▼\
Tutoriels/Démarrage \| Tutoriels \> Démarrage

0**8. Scoping et traçage\
Démarrer avec SuperCollider\
Voir aussi : 00. Débuter avec SC**

Analyse de quelques tracés\
Function possède deux autres méthodes audio utiles. Vous avez déjà vu
quelques résultats de la première, Function-plot :\
{ PinkNoise.ar(0.2) + SinOsc.ar(440, 0, 0.2) + Saw.ar(660, 0.2) }.plot
;\
Cette fonction crée un graphique du signal produit par la sortie de la
fonction. Vous pouvez spécifier certains arguments, tels que la durée.
La valeur par défaut est de 0,01 seconde, mais vous pouvez la définir
comme vous le souhaitez.\
{ PinkNoise.ar(0.2) + SinOsc.ar(440, 0, 0.2) + Saw.ar(660, 0.2)
}.plot(1) ;\
Cela peut être utile pour vérifier ce qui se passe et si vous obtenez la
sortie que vous pensez obtenir.\
\
La seconde méthode, Function-scope, affiche la sortie de la fonction
sous la forme d\'un oscilloscope.\
\
Essayons donc d\'analyser de l\'audio :\
{ PinkNoise.ar(0.2) + SinOsc.ar(440, 0, 0.2) + Saw.ar(660, 0.2) }.scope
;\
Cela devrait ouvrir une fenêtre qui
![](Pictures/100000010000010C000001239C5805839EDA0006.png){width="5.909cm"
height="6.415cm"}ressemble à ceci :

\
\
\

Cela fonctionne également pour plusieurs canaux :\
{ \[SinOsc.ar(440, 0, 0.2), SinOsc.ar(442, 0, 0.2)\] }.scope ;\
Scope dispose également d\'un argument zoom. Valeurs supérieures \"zoom
out\".\
{ \[SinOsc.ar(440, 0, 0.2), SinOsc.ar(442, 0, 0.2)\] }.scope(zoom : 10)
;\
Comme Function-plot, Function-scope peut être utile à des fins de test,
et pour voir si vous obtenez réellement ce que vous pensez obtenir.\
\
Analyse à la demande\
Vous pouvez également définir la portée de la sortie du serveur à tout
moment, en appelant \'scope\' sur celui-ci.\
{ \[SinOsc.ar(440, 0, 0.2), SinOsc.ar(442, 0, 0.2)\] }.play ;\
s.scope ;\
Vous pouvez faire la même chose en cliquant sur la fenêtre du serveur et
en appuyant sur la touche \'s\'.\
\
Pour plus d\'informations, voir :\
\
Fonction, Serveur, Stéthoscope\
\
Exercice suggéré\
Expérimentez le cadrage et le tracé de certains des exemples de
fonctions présentés dans les sections précédentes, ou de fonctions que
vous avez créées vous-même. Essayez d\'expérimenter différentes valeurs
de durée ou de zoom.\
\
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\
\
Ce document fait partie du didacticiel Premiers pas avec SuperCollider.\
\
Cliquez ici pour passer à la section suivante : 09. Obtenir de l\'aide\
\
Cliquez ici pour revenir à la table des matières : 00. Démarrer avec SC\
\
Source du fichier d\'aide : C:\\NProgram
Files\\NSuperCollider-3.13.0\\NHelpSource\\NTutorials\\NGetting-Started\\N08-Scoping-and-Potting.schelp\
link::Tutorials/Getting-Started/08-Scoping-and-Plotting: :
