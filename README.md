# git_abstract
brief about Git

- Configurer l'identité global :
	$ git config --global user.name "your name"
	$ git config --global user. email youremail@mail.com
	-> Remarque :
	Si vous souhaitez par contre, pour un projet spécifique, changer votre nom d’utilisateur, vous devrez repasser cette ligne mais sans le --global.

- Vérifier les paramètres entrès :
	$ git config --list
- Activer les couleurs afin d’améliorer la lisibilité des différentes branches :
	$ git config --global color.diff auto
	$ git config --global color.status auto
	$ git config --global color.branch auto
- Par défaut, Git utilise Vim comme éditeur et Vimdiff comme outil de merge. Vous pouvez les modifier en utilisant :
	$ git config --global core.editor notepad++
	$ git config --global merge.tool vimdiff
- Créer un dépôt local. Pour ce faire, deux solutions possibles :
	-> créer un dépôt local vide pour accueillir un nouveau projet ; $git init
	-> cloner un dépôt distant, c’est-à-dire rapatrier tout l’historique d’un dépôt distant en local, afin de pouvoir travailler sur dessus. 
	     $ git remote add OC https://github.com/M-JARIDI/super-adventure.git
	     (OC représente le nom court que vous utiliserez ensuite pour appeler votre dépôt).
    	     Cette ligne ne permet pas de cloner le dépôt distant, mais permet de dire au dépôt local que l’on pointe vers le dépôt distant.
	     $ git clone https://github.com/M-JARIDI/super-adventure.git
- Pour connaître les branches présentes dans notre projet, il faut taper la ligne de commande :
	$git branch
- Cette commande va créer la branche Cagnotte en local (elle ne va pas être dupliquée sur le dépôt distant) (La branche va fonctionner comme un dossier virtuel) : 
	$ git branch cagnotte
- Pour basculer de branche, on utilise utilise :
	$ git checkout cagnotte
- Définition de "commit" : Un commit est un enregistrement de votre travail à un instant T sur la branche courante où vous êtes. 
	-> Utilisation : $ git commit -m “Réalisation de la partie cagnotte côté front end”
- La commande "$git push" permet d'envoyer les modifications que l'on a réalisées en local sur le dépôt à distance.
- La commande "git pull" permet de récupérer en local le projet distant.
- Git gère les versions de vos travaux locaux à travers 3 zones locales majeures :
	-> le répertoire de travail (working directory/WD) ;
	-> l’index, ou stage (on préférere le second terme) : désigne tous les fichiers modifiés que vous souhaitez voir apparaître dans votre prochain commit.
		Pour les ajouter au stage : $git add
	-> le dépôt local (Git directory/repository).
"$ git reflog" : affichera toutes vos actions et leurs SHA. Le SHA, c'est ce grand code qui vous permettra de revenir à un commit exact. C'est l’identifiant de votre action ! 

- Dans un nouveau projet et après un "git init" dedons, pour créer la branche master vous devez simplement ajouter un fichier et le commiter.
	$ touch premierFichier.txt
	$ git add premierFichier.txt // ajouter au stage
	$ git commit -m "le message de modification"
- Pour créer une branche alors :
	$ git branch nomBranche
- Pour supprimer une branche :
	$ git branch -d nomBranche
- Pour forcer la suppression d'une branche :
	$ git branch -D nomBranche

- Après une modification non souhaitable sur la branche Master (et après un "$ git add nomFichier"), vous pouvez revenir en arrière :
	-> $ git status // affiche l'état des fichiers
	-> $ git stash // créer une remise
	-> $ git branch nom_nouvelle_branche //créer une nouvelle branche
	-> $ git checkout nom_nouvelle_branche // accèder à cette branche
	-> $ git stash apply // remetre les modifications dans la nouvelle branche ( ou $ git stash pop)
- Si vous avez fait plusieurs remises :
	-> $ git stash list // renvoie un tableau des remises avec des identifiants du style : "stash@{i} avec i=0,1,...

- Après un commit non souhaitable dans la branche Master :
	-> $ git log // affiche les identifiants des commits "hash"
	-> $ git reset --hard HEAD^ // supprimer votre dernier commit de la branche master.  Le Head^ indique que c'est bien le dernier commit que nous voulons supprimer.
	-> créer un autre branche et faire un checkout sur elle.
	-> git reset --hard 046442ff // 046442ff : 8 premiers caractères de l'identifiant.
- Pour modifier le dernier commit :
	$ git commit --amend
- Pour changer le message d'un commit (Cette commande va fonctionner pour le dernier commit réalisé et lorsqu'aucun élément n'est encore modifié !) :
	-> $ git commit --amend -m "Votre nouveau message de commit"
	    (L'option -m permet de transmettre le nouveau message).
- Oublier un fichier dans le dernier commit (par exemple ajouter un nouveau fichier après avoir fait un commit) :
	$ git add fichierOublier.txt
	$ git commit --amend --no-edit
	(Votre fichier a été ajouté à votre commit et grâce à la commande --no-edit que nous avons ajoutée, nous n'avons pas modifié le message du commit).
- Corriger vos erreurs à disntance (après un push) :
	$ git revert HEAD^ // L'opération Revert annule un commit en créant un nouveau commit. (Cette commande n'a donc aucun impact sur l'historique.)
	Remarque : -> git revert  pour annuler des changements apportés à une branche publique.
		  -> git reset  pour faire de même, mais sur une branche privée.
		  -> Gardez à l'esprit que Git revert sert à annuler des changements commités, tandis que Git reset HEAD permet d'annuler des changements non commités.
		  -> Toutefois, attention, Git revert peut écraser vos fichiers dans votre répertoire de travail, il vous sera donc demandé de commiter vos modifications ou de les remiser.
- La commande  "git reset"  est un outil complexe et polyvalent pour annuler les changements.
- Les trois types de réinitialisation de Git :
	-> --soft : $ git reset --Soft . Il permet juste de se placer sur un commit spécifique afin de voir le code à un instant donné ou créer une branche partant d'un ancien commit.
	      Il ne supprime aucun fichier, aucun commit, et ne crée pas de HEAD détaché.
	-> --mixed : $ git reset --mixed . Cette commande va permettre de revenir juste après votre dernier commit ou le commit spécifié, sans supprimer vos modifications en cours. Il va par contre créer un HEAD détaché.
	     Il permet aussi, dans le cas de fichiers indexés mais pas encore commités, de désindexer les fichiers.
		$ git reset HEAD~
		Si rien n'est spécifié après  git reset, par défaut il exécutera un :
		$ git reset --mixed HEAD~
	-> -- hard : $ git reset notreCommitCible  --hard .Attention, si vous voulez exécuter cette commande, vérifiez 5 fois avant de la lancer et soyez sûr de vous à 200 % .
	     Cette commande va permettre de revenir à n'importe quel commit mais en oubliant absolument tout ce qu'il s'est passé après !
- Le HEAD, si vous n'êtes pas sûr d'avoir bien compris , est un pointeur, une référence sur notre position actuelle dans notre répertoire de travail Git. 
   C’est un peu comme notre ombre : elle nous suit où qu’on aille ! Par défaut, HEAD pointe sur la branche courante, master, et peut être déplacé vers une autre branche ou un autre commit.
- la différence entre Git revert et Git reset : Git revert va créer un nouveau commit alors que Git reset, non.
- Remonter le temps (vous avez fait un commit mais un fichier s'est glissé par "erreur") : Elle va réaliser une sorte de Undo, mais en faisant un deuxième commit.
   Elle ne va pas revenir en arrière et supprimer votre commit, mais va inverser vos actions dans le commit et réaliser un second commit.
	$ git revert HEAD
- énumèrer en ordre chronologique inversé les commits réalisés :
	$ git log
   Cette commande affiche chaque commit avec son identifiant SHA (Secure Hash Algorithm), l'auteur du commit, la date et le message du commit.
- La commande Git reflog va enregistrer vos commits, vos modifications de messages, vos merges, vos resets, et il va afficher un identifiant SHA-1 pour chaque action.
	$ git reflog
	$ git checkout e789e7c // 8 premiers caractères d SHA pour revenir à l'action
- Pour savoir qui a touché à une ligne en particulier dans le fichier test.html on la commande ci-dessous qui permet d’examiner le contenu d’un fichier ligne par ligne et affiche pour chaque ligne modifiée, son ID, l'auteur, l'horodatage, le numéro de la ligne et le contenu de la ligne.
	$ git blame
- Il me faut ce commit ! Vite Git cherry-pick :
  Parfois, vous ne voulez pas fusionner une branche entière dans une autre et vous n'avez besoin que de choisir un ou deux commits spécifiques. Ce processus s'appelle cherry-pick ! Attention, Git cherry-pick n'est pas très   apprécié dans la communauté des développeurs ! En effet, cette commande va dupliquer des commits existants. Il sera préférable, si possible, de réaliser un merge.
	$ git cherry-pick d356940 de966d4
- Pour générer votre duo de clés ssh :
	$ git ssh-keygen -t rsa -b 4096 -C "your_email@host.com"
- Découvrez l'arbre de Git et sa structure :
  il existe trois types d'objets principaux et un plus secondaire :
	-> le "tree" : est une forme de répertoire, il réference un liste de trees et de blobs (sous-réps et fichiers);
	-> le "commit : il pointe vers un arbre spécifique et la marquer afin de représenter son état à un instant donné;
	-> le "blob" (Binary Large Object) : il représente un fichier;
	-> le "Tag" : il repésente un commit d'une version spécifique.
	Remarque :
		Git n'utilise pas les noms des fichiers et des répertoires pour classer et stocker vos données, mais il utilise leur empreinte ou identifiant SHA-1.

- Représentation cryptographique d'un commit :
  Toutes les informations nécessaires pour décrire l’historique d’un projet sont stockées dans des fichiers référencés par un identifiant de 40 caractères qui ressemble à quelque chose      comme ça : 8gh96c4636981e4759825791c8ea3bcf5f2278t9.
  Remarque :
	Pour chacun des objets dans Git, vous trouverez cette chaîne de 40 caractères que nous appelons le hash SHA-1. Celui-ci représente le contenu de l'objet.
- la fonction Merge réalise une fusion en combinant deux branches. Donc git merge va créer un nouveau commit de merge
	$ git merge Nouvelle Fonctionnalité // situons dans la branche Master, Votre branche Nouvelle fonctionnalité va être fusionnée sur la branche master en créant un nouveau commit.
	Remarque :
		Un  git merge  ne devrait être utilisé que pour la récupération fonctionnelle, intégrale et finale d’une branche dans une autre

- La commande Git pull permet de télécharger les modifications qui ont eu lieu sur le dépôt distant, dans le but de les rapatrier sur le dépôt local. Git pull est en réalité la fusion de deux commandes Git :  git merge  que nous venons de voir et  git fetch  que nous verrons juste après.  git pull va créer un nouveau commit de fusion comme le fait  git merge. La commande  git pull  exécute d'abord  git fetch  qui télécharge le contenu du référentiel distant spécifié. Ensuite, un git merge  est exécuté pour fusionner les modifications du dépôt distant et créer un nouveau commit de merge en local. 
	$ git pull <remote>
- À l'inverse, la commande Git push permet d'envoyer des modifications que l'on a réalisées en local sur le dépôt à distance.
	$ git push <remote>
- Git fetch, contrairement à Git pull, va aller chercher les modifications sur le dépôt distant mais ne va pas les fusionner avec nos modifications locales. Git isole le contenu récupéré en tant que contenu local existant, cela n'a absolument aucun effet sur votre travail de développement local. La commande Git fetch va récupérer toutes les données des commits effectués sur la branche courante qui n'existent pas encore dans votre version en local. Ces données seront stockées dans le répertoire de travail local, mais ne seront pas fusionnées avec votre branche locale. Si vous souhaitez fusionner ces données pour que votre branche soit à jour, vous devez utiliser ensuite la commande Git merge.

- Git rebase a le même objectif que Git merge. Ces deux commandes permettent de transférer les changements d'une branche à une autre.
  Il existe deux types de rebase : manuel et interactif.
 Git va prendre vos modifications d'une branche et les transposer sur une autre branche.
 Rebaser est une méthode courante pour intégrer les changements en amont dans votre répertoire local.
 La commande  "$ git rebase"  standard appliquera les commits à votre branche courante puis à la pointe de la branche transférée. 
	Remarque :
		Attention ! Vous ne devez jamais rebaser des commits pushés sur le dépôt public !
- Le rebasage interactif "$ git rebase -i" vous donne un contrôle complet sur l'historique de votre projet. Il sert à nettoyer son historique.


$ git blame nomdufichier.extension
$ git show 05b1233
