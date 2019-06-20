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

Le script ne fait pas grand chose de magique, il fait appel à zenity pour avoir une interaction avec l'utilisateur.

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

## Problèmes possibles

Stephen Macke indique qu'il peut y avoir de léger soucis de synchro dans certaines situations, par exemple un léger décalage des sous-titres. Voir son repo pour plus d'informations.

Les tests que j'ai réalisés sur mon script montrent qu'il fonctionne bien, cependant il n'est qu'à sa première version et peut susceptiblement ne pas marcher pour vous.
