# subsync-gui
Un simple script shell pour utiliser subsync avec zenity

Ce script repose sur le logiciel **subsync** de [Stephen Macke](https://github.com/smacke).

Vous devez donc télécharger et installer son logiciel sur son dépôt : https://github.com/smacke/subsync
Il propose une intégration à VLC à l'aide d'un patch du code source de VLC. C'est très coputeux en temps et compiler VLC demande de très nombreuses dépendances. N'y étant pas parvenu personnellement, j'ai créé ce script qui répond à mon besoin.

Il est bon de noter que l'utilisation de son scipt est triviale : `subsync ma-video.mpg -i mon-fichier-sous-titre.srt -o fichier-sous-titre-synchronisé`

## Pré-requis

Le script a donc besoin que `subsync` et que `zenity` soient installés.

Zenity est très probablement déjà installé sur votre machine ou alors se trouve dans les dépôts officiels de votre distibution.

Pour l'installation de subsync je vous oriente vers le dépôt de Stephen Macke : https://github.com/smacke/subsync

Prenez soin de d'inclure l'exécutable `subsync` dans votre $PATH.

(Facultatif) : Le script `subsync-gui` propose le lancement de la vidéo avec les sous-titres dans VLC à la ifn de la procédure, donc pensez à l'installer si vous désirez profiter de cette fonctionnalité.

## Fonctionnement

Le script ne fait pas grand chose de magique, il fait appel à zenity pour avoir une interaction avec l'utilisateur (choisir le fichier vidéo & sous-titres) puis il fait tourner `subsync` sur les valeurs retournées.

1. Choisir le fichier vidéo.
2. Le script tente de trouver le fichier de sous-titre automatiquement en se basant sur le nom du fichier vidéo.
    * Fichier vidéo : `/home/elchusco/myvideo.mkv`
    * Le script tente de trouver le fichier `/home/elchusco/myvideo.srt`
3. S'il trouve le fichier sous-titre, alors il lance automatiquement l'autosync des sous-titres.
    1. Il propose ensuite de lancer la vidéo dans VLC (s'il est présent)
    2. Si VLC est absent du système alors il propose d'ouvrir le répertoire contenant le fichier et les sous-titres
4. Sinon, il propose à l'utilisateur de le sélectionner le fichier de sous-titre manuellement.
    1. L'autosync se lance et le fichier de sous-titre synchronisé est placé dans le même répertoire que la vidéo avec le même nom (à l'extension près). Afin de faciliter l'intégration dans VLC
    2. Il propose de lancer la vidéo dans VLC
    3. Sinon d'ouvrir le répertoire contenant la vidéo et les sous-titres

## Intégration à VLC (si on veut)

### Préambule VLsub

On sait que VLC pour trouver la piste de sous-titres à lire avec le fichier vidéo se base sur les noms de fichiers, par exemple :
```
myvideo.mkv
myvideo.srt
```
Dans ce cas, si on lance `myvideo.mkv` alors VLC lancera automatiquement la vidéo avec le sous-titre `myvideo.srt`.

Il est un plugin installé par défaut méconnue de VLC qui s'appelle **VLsub**. Il se trouve dans l'onglet `Vue -> VLsub`. Ce plugin permet de télécharger automatiquement des sous-titres dans de nombreuses langues pour le film ou l'épisode de la série que vous regardez en se basant sur des sites tels qu'opensubtitles.org ou addic7ed.com.

Il fonctionne sur 2 modes, l'un qui calcul le hash de la vidéo et permet d'obtenir des sous-titres généralement parfaitement synchronisé avec votre fichier. Mais ce n'est pas très souvent qu'on en trouve en français. Et si on veut commencer à regarder notre film alors que le téléchargement de celui-ci n'est pas terminé cela ne fonctionne pas non plus.

Le second mode se base sur le nom du film ou le nom de la série avec les numéros de saison et d'épisode. Souvent plus riche en résultats, on trouve son bonheur avec ce mode là. Seulement, très souvent les sous-titres ne sont pas synchonisés.

Alors on peut ajuster à la mano avec les touches `G` et `H` du clavier pour synchroniser mais c'est pénible.

D'où l'intérêt du projet `subsync` de Stephen Macke. On notera qu'il est probablement possible d'utiliser son script avec des outils comme Plex ou Kodi.

### Mon Workflow

Il faut noter que **VLsub** télécharge les sous-titres en les plaçants à côté de la vidéo avec le nom de fichier `myvideo.srt`. Donc il n'y a plus qu'à faire tourner `subsync-gui` par dessus et le tour est joué.

1. Lancer le film / épisode dans VLC
2. Ouvrir le plugin VLsub et télécharger le sous-titre qui vous plaît
3. Ouvrir `subsync-gui` et sélectionnez votre vidéo (pas besoin de sélectionner le sous-titre puisque formatté ainsi par VLsub il sera automatiquement détecté).
4. À la fin de la synchro le script propose de lancer VLC avec la vidéo et sous-titres synchro
5. You're good to go ! ;)

## Problèmes possibles

Stephen Macke indique qu'il peut y avoir de léger soucis de synchro dans certaines situations, par exemple un léger décalage des sous-titres. Voir son repo pour plus d'informations.

Les tests que j'ai réalisés sur mon script montrent qu'il fonctionne bien, cependant il n'est qu'à sa première version et peut susceptiblement ne pas marcher pour vous.
