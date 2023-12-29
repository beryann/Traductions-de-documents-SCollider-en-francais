Table des Matières ▼ SuperColliderBrowseSearchIndexes\
SuperColliderBrowseSearchIndexes ▼\
Tutoriels/Démarrage \| Tutoriels \> Démarrage\
03. Démarrez vos moteurs\
Démarrer avec SuperCollider\
Voir aussi : 00. Démarrer avec SC\
Avant de pouvoir faire du son, nous devons démarrer ou \"amorcer\" une
application serveur. La façon la plus simple de le faire est d\'utiliser
le raccourci Ctrl-B (Cmd-B sur macOS). Il existe également une entrée
\"Boot Server\" dans le menu Serveur.\
\
Remarquez que la police blanche sur la vue noire en bas de la fenêtre
est devenue verte. Cela indique que le serveur est en cours
d\'exécution. La vue vous fournit également des informations sur
l\'utilisation du CPU, et d\'autres choses qui ne sont probablement pas
encore très claires. Nous en reparlerons bientôt.\
\
Jetez également un coup d\'oeil à la fenêtre post, où SC vous a donné
quelques informations, et vous a fait savoir qu\'il avait bien démarré.
Par exemple :\
Démarrage du serveur \'localhost\' sur l\'adresse 127.0.0.1:57110.\
Nombre de périphériques : 2\
0 : \"Microphone intégré\"\
1 : \"Sortie intégrée\"\
\
\"Dispositif d\'entrée \"Microphone intégré\
Flux : 1\
0 canaux 2\
\
\"Dispositif de sortie \"Built-in Output\
Flux : 1\
0 canaux 2\
\
SC_AudioDriver : fréquence d\'échantillonnage = 44100.000000, taille du
bloc du pilote = 512\
Le serveur SuperCollider 3 est prêt.\
Messages de notification demandés au serveur \'localhost\'\
localhost : maxLogins (1) du processus serveur correspond à mes
options.\
localhost : maintien de l\'identifiant client (0) comme confirmé par le
processus serveur.\
Initialisation de l\'interface du serveur à mémoire partagée\
Si, pour une raison quelconque, le démarrage a échoué, des informations
sur l\'erreur qui s\'est produite devraient être imprimées. Si cela se
produit, veuillez contacter la communauté pour obtenir de l\'aide :
https://supercollider.github.io#community\
\
Par défaut, vous pouvez faire référence au serveur localhost dans votre
code en utilisant la lettre s. Vous pouvez envoyer des messages pour le
démarrer et l\'arrêter comme suit :\
s.quit ;\
s.boot ;\
Essayez ceci et laissez le serveur fonctionner. De nombreux exemples
dans la documentation ont s.boot au début, mais en général vous devriez
vous assurer que le serveur est en marche avant d\'utiliser des exemples
qui génèrent de l\'audio, ou qui accèdent au serveur d\'une manière ou
d\'une autre. En général, les exemples de ce tutoriel supposent que le
serveur est en cours d\'exécution.\
\
Vous pouvez également faire référence au serveur par défaut avec le
texte Server.default, par exemple :\
Serveur.default.boot ;\
Pour plus d\'informations, voir\
\
Serveur\
\
Sélection d\'un périphérique audio\
\
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\
\
Ce document fait partie du tutoriel \"Getting Started With
SuperCollider\".\
\
Cliquez ici pour passer à la section suivante : 04. Fonctions et autres
fonctionnalités\
\
Cliquez ici pour revenir à la table des matières : 00. Premiers pas avec
SC\
\
