Table des Matières ▼ SuperColliderBrowseSearchIndexes\
SuperColliderBrowseSearchIndexes ▼\
Tutoriels / Mise en route \| Tutoriels \> Mise en route\
13. Tampons\
Démarrer avec SuperCollider\
Voir aussi : 00. Premiers pas avec SC\
Les tampons représentent les tampons du serveur, qui sont des tableaux
ordonnés de nombres flottants sur le serveur. Le terme \"float\" est
l\'abréviation de floating point number (nombre à virgule flottante), ce
qui signifie un nombre avec une virgule décimale, comme 1,3. Par
opposition aux nombres entiers, qui sont des nombres entiers positifs ou
négatifs (ou zéro), et qui sont écrits sans virgule. Ainsi, 1 est un
nombre entier, mais 1,0 est un nombre flottant.\
\
Les tampons du serveur peuvent être à un ou plusieurs canaux et
constituent le moyen habituel de stocker des données côté serveur. Leur
utilisation la plus courante est de contenir des fichiers audio en
mémoire, mais toute sorte de données pouvant être représentées par des
flottants peut être stockée dans un tampon.\
\
Comme pour les bus, le nombre de tampons est défini avant le démarrage
du serveur (à l\'aide de ServerOptions), mais avant que les tampons
puissent être utilisés, il faut leur allouer de la mémoire, ce qui est
une étape asynchrone. Tout comme les bus, les tampons sont numérotés, à
partir de 0. L\'utilisation de Buffer permet d\'allouer des numéros et
d\'éviter les conflits.\
\
Vous pouvez considérer les tampons comme l\'équivalent côté serveur
d\'un tableau, mais sans toutes les élégantes fonctionnalités de la POO.
Heureusement, avec les tampons et la possibilité de manipuler les
données dans l\'application cliente lorsque cela est nécessaire, vous
pouvez faire presque tout ce que vous voulez avec les données des
tampons. Les tampons d\'un serveur sont globaux, ce qui signifie qu\'ils
peuvent être accédés par n\'importe quel synthé, et par plus d\'un à la
fois. Il est possible d\'y écrire ou même d\'en modifier la taille
pendant qu\'on les lit.\
\
De nombreuses méthodes de Buffer ont de nombreux arguments. Il va sans
dire que pour obtenir des informations complètes, consultez le fichier
d\'aide de Buffer.\
\
Création d\'un objet Buffer et allocation de mémoire\
Créer un objet Buffer et allouer la mémoire nécessaire dans
l\'application serveur est assez facile. Vous pouvez le faire en une
seule étape avec la méthode alloc de Buffer :\
s.boot ;\
b = Buffer.alloc(s, 100, 2) ; // alloue 2 canaux et 100 images\
b.free ; // libère la mémoire (lorsque vous avez fini de l\'utiliser)\
L\'exemple ci-dessus alloue une mémoire tampon à 2 canaux avec 100
images. Le nombre réel de valeurs stockées est numChannels \* numFrames,
donc dans ce cas il y aura 200 flottants. Dans ce cas, chaque trame
correspond à une paire de valeurs.\
\
Si vous souhaitez allouer des données en termes de secondes, plutôt
qu\'en termes de trames, vous pouvez procéder comme suit :\
b = Buffer.alloc(s, s.sampleRate \* 8.0, 2) ; // un tampon stéréo de 8
secondes\
b.free ;\
La méthode \'free\' de Buffer libère la mémoire sur le serveur et
renvoie le numéro du Buffer pour réaffectation. Vous ne devez pas
utiliser un objet Buffer après avoir effectué cette opération.\
\
Utilisation de tampons avec des fichiers audio\
La classe Buffer possède une autre méthode appelée \"read\", qui lit un
fichier son du disque vers la mémoire et renvoie un objet Buffer. En
utilisant l\'objet UGen PlayBuf, nous pouvons lire le fichier.\
// lire un fichier son\
b = Buffer.read(s, Platform.resourceDir +/+ \"sounds/a11wlk01.wav\") ;\
\
// maintenant, jouons-le\
(\
x = SynthDef(\"tutorial-PlayBuf\",{ arg out = 0, bufnum ;\
Out.ar( out,\
PlayBuf.ar(1, bufnum, BufRateScale.kr(bufnum))\
)\
}).play(s,\[\\bufnum, b\]) ;\
)\
x.free ; b.free ;\
PlayBuf.ar dispose d\'un certain nombre d\'arguments qui vous permettent
de contrôler divers aspects de son fonctionnement. Jetez un coup d\'œil
au fichier d\'aide de PlayBuf pour plus de détails sur chacun d\'entre
eux, mais pour l\'instant, contentons-nous des trois premiers, utilisés
dans l\'exemple ci-dessus.\
PlayBuf.ar(\
1, // nombre de canaux\
bufnum, // nombre de tampons à jouer\
BufRateScale.kr(bufnum) // taux de lecture\
)\
Nombre de canaux : Lorsque vous travaillez avec PlayBuf, vous devez lui
indiquer le nombre de canaux du tampon qu\'il lira. Vous ne pouvez pas
en faire un argument dans la SynthDef et le changer plus tard. Pourquoi
? Rappelez-vous que les SynthDefs doivent avoir un nombre fixe de canaux
de sortie. Donc un PlayBuf à un canal est toujours un PlayBuf à un
canal. Si vous avez besoin de versions qui peuvent jouer un nombre
variable de canaux, créez plusieurs SynthDefs ou utilisez
Function-play.\
\
Numéro de Buffer : Comme indiqué ci-dessus, les Buffers sont numérotés,
en commençant par zéro. Vous pouvez obtenir le numéro d\'un Buffer en
utilisant sa méthode bufnum, mais vous n\'aurez normalement pas besoin
de le faire, puisque les objets Buffer peuvent être passés directement
en tant qu\'entrées UGen ou args Synth.\
\
Vitesse de lecture : Un taux de 1 correspondrait à une vitesse normale,
2 à une vitesse deux fois plus rapide, etc. Mais ici, nous voyons un
UGen appelé BufRateScale. Ce qu\'il fait, c\'est vérifier
l\'échantillonnage du tampon (qui est réglé pour correspondre à celui du
fichier son lorsqu\'il est chargé) et produire le taux qui
correspondrait à la vitesse normale. Ceci est utile car le fichier son
que nous avons chargé (a11wlk01.wav) a un taux d\'échantillonnage de
11025 Hz. Avec un taux de 1, PlayBuf le lirait en utilisant le taux
d\'échantillonnage du serveur, qui est généralement de 44100 Hz, soit
quatre fois plus vite ! BufRateScale ramène donc les choses à la
normale.\
\
Lecture en continu d\'un fichier à partir du disque\
Dans certains cas, par exemple lorsque vous travaillez avec de très gros
fichiers, vous ne voudrez peut-être pas charger complètement un son en
mémoire. Au lieu de cela, vous pouvez l\'introduire en continu à partir
du disque, petit à petit, en utilisant la méthode DiskIn d\'UGen et la
méthode \'cueSoundFile\' de Buffer :\
(\
SynthDef(\"tutorial-Buffer-cue\",{ arg out=0,bufnum ;\
Out.ar(out,\
DiskIn.ar( 1, bufnum )\
)\
).add ;\
)\
\
b = Buffer.cueSoundFile(s,Platform.resourceDir +/+
\"sounds/a11wlk01-44_1.aiff\", 0, 1) ;\
y = Synth.new(\"tutorial-Buffer-cue\", \[\\bufnum,b\], s) ;\
\
b.free ; y.free ;\
Cette méthode n\'est pas aussi souple que PlayBuf (pas de contrôle de la
vitesse), mais elle permet d\'économiser de la mémoire.1\
\
En savoir plus sur les variables d\'instance et les fonctions d\'action\
Maintenant, un peu plus de POO. Rappelez-vous que les objets individuels
stockent des données dans des variables d\'instance. Certaines variables
d\'instance possèdent ce que l\'on appelle des méthodes getter ou
setter, qui vous permettent d\'obtenir ou de définir leurs valeurs. Nous
avons déjà vu cela en action avec la méthode \"bufnum\" de Buffer, qui
est une méthode d\'obtention pour sa variable d\'instance numéro de
tampon.\
\
Buffer dispose d\'un certain nombre d\'autres variables d\'instance
dotées d\'entrées qui peuvent fournir des informations utiles. Celles
qui nous intéressent pour l\'instant sont numChannels, numFrames et
sampleRate. Celles-ci peuvent être particulièrement utiles lorsque l\'on
travaille avec des fichiers audio, car nous n\'avons pas forcément
toutes ces informations à portée de main avant de charger le fichier.\
// regarder la fenêtre d\'affichage\
b = Buffer.read(s, Platform.resourceDir +/+ \"sounds/a11wlk01.wav\") ;\
b.bufnum ;\
b.numFrames ;\
b.numChannels ;\
b.sampleRate ;\
b.free ;\
Maintenant (comme dans l\'exemple utilisant une fonction d\'action dans
notre exemple Bus-get ; voir 11. Busses), en raison de la faible latence
de messagerie entre le client et le serveur, les variables d\'instance
ne seront pas immédiatement mises à jour lorsque vous ferez quelque
chose comme lire un fichier dans un tampon. C\'est pourquoi de
nombreuses méthodes de Buffer prennent des fonctions d\'action comme
arguments. Rappelez-vous qu\'une fonction d\'action est simplement une
fonction qui sera évaluée une fois que le client aura reçu une réponse
et mis à jour les variables du tampon. L\'objet Buffer lui est passé en
argument.\
// avec une fonction d\'action\
// notez que les variables ne sont pas immédiatement mises à jour\
(\
b = Buffer.read(s, Platform.resourceDir +/+ \"sounds/a11wlk01.wav\",
action : { arg buffer ;\
(\"numFrames après mise à jour :\" + buffer.numFrames).postln ;\
x = { PlayBuf.ar(1, buffer, BufRateScale.kr(buffer)) }.play ;\
}) ;\
\
// Notez que la ligne suivante sera exécutée AVANT la fonction
d\'action\
(\"numFrames avant la mise à jour :\" + b.numFrames).postln ;\
)\
x.free ; b.free ;\
Dans l\'exemple ci-dessus, le client envoie la commande de lecture à
l\'application serveur, ainsi qu\'une demande d\'informations
nécessaires à la mise à jour des variables d\'instance du tampon. Il
demande ensuite à la fonction d\'action d\'être exécutée lorsqu\'il
reçoit la réponse, et poursuit l\'exécution du bloc de code. C\'est
pourquoi la ligne \"Before update\...\" s\'exécute en premier.\
\
Enregistrement dans des tampons\
Outre PlayBuf, il existe un UGen appelé RecordBuf, qui vous permet
d\'enregistrer dans un tampon.\
b = Buffer.alloc(s, s.sampleRate \* 5, 1) ; // un tampon de 5 secondes à
1 canal\
\
// enregistrement pendant quatre secondes\
(\
x = SynthDef(\"tutorial-RecordBuf\",{ arg out=0,bufnum=0 ;\
var noise ;\
noise = PinkNoise.ar(0.3) ; // enregistrer un peu de PinkNoise\
RecordBuf.ar(noise, bufnum) ; // par défaut, cela se boucle\
}).play(s,\[\\out, 0, \\bufnum, b\]) ;\
)\
\
// libérer le synthétiseur d\'enregistrement après quelques secondes\
x.free ;\
\
// lecture de l\'enregistrement\
(\
SynthDef(\"tutorial-playback\",{ arg out=0,bufnum=0 ;\
var playbuf ;\
playbuf = PlayBuf.ar(1,bufnum) ;\
FreeSelfWhenDone.kr(playbuf) ; // libère le synthé lorsque le PlayBuf a
joué une fois\
Out.ar(out, playbuf) ;\
}).play(s,\[\\out, 0, \\bufnum, b\]) ;\
)\
b.free ;\
Voir le fichier d\'aide de RecordBuf pour plus de détails sur toutes ses
options.\
\
Accès aux données\
Buffer dispose d\'un certain nombre de méthodes permettant d\'obtenir ou
de définir des valeurs dans un tampon. Buffer-get et Buffer-set sont
simples à utiliser et prennent un index en argument. Les tampons
multicanaux entrelacent leurs données, de sorte que pour un tampon à
deux canaux, l\'index 0 = frame1-chan1, l\'index 1 = frame1-chan2,
l\'index 2 = frame2-chan1, et ainsi de suite. La fonction \'get\' prend
une fonction d\'action.\
b = Buffer.alloc(s, 8, 1) ;\
b.set(7, 0.5) ; // fixe la valeur de 7 à 0.5\
b.get(7, {\|msg\| msg.postln}) ; // récupère la valeur à 7 et l\'affiche
lorsque la réponse est reçue\
b.free ;\
Les méthodes \"getn\" et \"setn\" permettent d\'obtenir et de définir
des plages de valeurs adjacentes. La méthode \"setn\" prend un indice de
départ et un tableau de valeurs à définir, tandis que la méthode
\"getn\" prend un indice de départ, le nombre de valeurs à obtenir et
une fonction d\'action.\
b = Buffer.alloc(s,16) ;\
b.setn(0, \[1, 2, 3\]) ; // définit les 3 premières valeurs\
b.getn(0, 3, {\|msg\| msg.postln}) ; // les récupérer\
b.setn(0, Array.fill(b.numFrames, {1.0.rand})) ; // remplir le tampon
avec des valeurs aléatoires\
b.getn(0, b.numFrames, {\|msg\| msg.postln}) ; // les obtient\
b.free ;\
Il existe une limite supérieure au nombre de valeurs que vous pouvez
obtenir ou définir à la fois (généralement 1633 lorsque vous utilisez
UDP, la valeur par défaut). Ceci est dû à une limite sur la taille des
paquets du réseau. Pour y remédier, Buffer dispose de deux méthodes,
\"loadCollection\" et \"loadToFloatArray\", qui permettent de définir ou
d\'obtenir de grandes quantités de données en les écrivant sur le
disque, puis en les chargeant sur le client ou le serveur, selon le
cas.\
(\
// faire du bruit blanc\
v = FloatArray.fill(44100, {1.0.rand2}) ;\
b = Buffer.alloc(s, 44100) ;\
)\
(\
// charger le FloatArray dans b, puis le lire\
b.loadCollection(v, action : {\|buf\|\
x = { PlayBuf.ar(buf.numChannels, buf, BufRateScale.kr(buf), loop : 1)\
\* 0.2 }.play ;\
}) ;\
)\
x.free ;\
\
// récupère maintenant le FloatArray, et le compare à v ; ceci affiche
\'true\'\
// les arguments 0, -1 signifient que l\'on commence au début et que
l\'on charge tout le tampon\
b.loadToFloatArray(0, -1, {\|floatArray\| (floatArray == v).postln }) ;\
b.free ;\
Un FloatArray est simplement une sous-classe de Array qui ne peut
contenir que des flottants.\
\
Tracer et jouer\
Buffer dispose de deux méthodes de commodité utiles : \"plot\" et
\"play\".\
// voir la forme d\'onde\
b = Buffer.read(s, Platform.resourceDir +/+ \"sounds/a11wlk01.wav\") ;\
b.plot ;\
\
// lire le contenu\
// ceci prend un argument : loop. Si false (par défaut), le synthé
résultant est\
// libéré automatiquement\
b.play ; // se libère lui-même\
x = b.play(true) ; // boucle donc ne se libère pas\
x.free ; b.free ;\
Autres utilisations des tampons\
En plus d\'être utilisés pour charger des fichiers audio, les tampons
sont également utiles dans toutes les situations où vous avez besoin
d\'ensembles de données volumineux et/ou accessibles globalement sur le
serveur. Un exemple d\'une autre utilisation est la création d\'une
table de recherche pour la mise en forme des ondes.\
b = Buffer.alloc(s, 512, 1) ;\
b.cheby(\[1,0,1,1,0,1\]);\
(\
x = play({\
Shaper.ar(\
b,\
SinOsc.ar(300, 0, Line.kr(0,1,6)),\
0.5\
)\
}) ;\
)\
x.free ; b.free ;\
L\'UGen Shaper effectue une mise en forme des ondes sur une source
d\'entrée. La méthode \'cheby\' remplit le tampon avec une série de
polynômes de Chebyshev, qui sont nécessaires pour cela. (Ne vous
inquiétez pas si vous ne comprenez pas tout cela.) Buffer dispose de
nombreuses méthodes similaires pour remplir un tampon avec différentes
formes d\'ondes.\
\
Il existe de nombreuses autres utilisations des tampons. Vous les
rencontrerez tout au long de la documentation.\
\
Pour plus d\'informations, voir :\
\
Buffer, PlayBuf, RecordBuf, SynthDef, BufRateScale, Shaper\
\
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\
\
Ce document fait partie du tutoriel \"Getting Started With
SuperCollider\".\
\
Cliquez ici pour passer à la section suivante : 14. Programmation
d\'événements\
\
Cliquez ici pour revenir à la table des matières : 00. Démarrer avec SC\
\
\[1\] - Pour une vitesse de lecture variable lors de la diffusion à
partir d\'un disque, consultez l\'UGen VDiskIn.\
helpfile source : C:\\NProgram
Files\\NSuperCollider-3.13.0\\NHelpSource\\NTutorials\\NPrise en
main\\N13-Buffers.schelp\
link::Tutorials/Getting-Started/13-Buffers: :
