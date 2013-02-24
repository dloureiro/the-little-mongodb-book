# À propos de ce livre #

## Licence ##
Le petit livre de MongoDB est sous licence Attribution-NonCommercial 3.0 Unported license. **Ce livre devrait vous avoir été fourni gratuitement.**

Vous êtes libre de copier, distribuer, modifier ou afficher ce livre. Cependant, je vous prie de bien vouloir attribuer ce livre à son auteur, Karl Seguin, et de ne pas en faire un usage commercial.

Vous pouvez consulter le texte intégral de la licence à l'adresse suivante :

<http://creativecommons.org/licenses/by-nc/3.0/legalcode.fr>

## À propos de l'auteur ##
Karl Seguin est un développeur expérimenté dans divers domaines et technologies. Il est expert en .NET et en Ruby. Il contribue semi-activement à des logiciels open source, écrit des articles techniques et présente parfois des conférences. En ce qui concerne MongoDB, il était l'un des contributeurs principaux à NoRM, la bibliothèque MongoDB pour C#, et a écrit le tutoriel interactif [mongly](http://mongly.com) ainsi que [Mongo Web Admin](https://github.com/karlseguin/Mongo-Web-Admin). Son service gratuit pour les développeurs de jeux *casual*, [mogade.com](http://mogade.com/), utilise MongoDB.

Karla depuis écrit [The Little Redis Book (en anglais)](http://openmymind.net/2012/1/23/The-Little-Redis-Book/)/[Le petit livre de Redis (en français)](http://url.to.translation)

Son blog se trouve à <http://openmymind.net> et il twitte depuis [@karlseguin](http://twitter.com/karlseguin).

## Remerciements ##
Un merci tout particulier à [Perry Neal](http://twitter.com/perryneal) pour m'avoir prêté ses yeux, son esprit et sa passion. Son aide m'a été inestimable. Merci.

## Dernière version ##
La version la plus récente de ce livre est disponible à :

<http://github.com/karlseguin/the-little-mongodb-book> (en anglais).
<http://url.to.translation> (en français).

# Introduction #
 > Si les chapitres sont courts, c'est qu'il est vraiment simple d'apprendre MongoDB.

On dit souvent que la technologie avance à un rythme effréné, et il est vrai que des nouvelles technologies et techniques apparaissent constamment. Et pourtant, j'ai longtemps pensé que les technologies fondamentales utilisées par les développeurs avançaient en fait plutôt lentement. On peut arrêter de se former pendant des années sans pour autant être complètement dépassé. Mais le plus étonnant, c'est la vitesse à laquelle les technologies sont remplacées. Du jour au lendemain, des technologies bien établies se retrouvent menacées par un changement d'approche des développeurs.

Rien n'illustre mieux ce changement soudain que la progression des technologies NoSQL en regard des vénérables bases de données relationnelles. Hier encore, le web semblait tourner sur quelques RDBMS (systèmes de gestion de bases de données relationnelles ou SGBD), et voilà qu'aujourd'hui, une demi-douzaine de solutions NoSQL se sont imposées comme des alternatives crédibles.

Bien que ces transitions puissent paraître soudaines, il faut en fait plusieurs années pour qu'elles soient acceptées. L'enthousiasme initial est porté par un nombre relativement restreint de développeurs et d'entreprises. Les solutions sont affinées, des leçons sont tirées, et c'est seulement quand la technologie paraît fermement installée que d'autres l'intègrent progressivement. Là encore, c'est particulièrement vrai pour NoSQL, où plusieurs solutions ne sont pas seulement des alternatives aux solutions de stockage traditionnelles mais répondent en plus à un nouveau besoin spécifique.

Cela étant dit, la première chose à faire est de préciser ce que l'on entend par NoSQL, un terme assez vague utilisé par différentes personnes pour signifier différentes choses. Personnellement, je l'utilise au sens large pour un système qui joue un rôle dans la gestion de données. Autrement dit, NoSQL signifie, à mon sens, que la couche de persistence des données n'est pas forcément de la responsabilité d'un système unique. Alors qe les vendeurs de bases de données relationnelles les ont historiquement présentées comme des solutions miracles, NoSQL tend vers uen organisation en unités plus petites, dans lesquelles l'outil le plus adapté peut être utilisé au mieux. Ainsi, une pile NoSQL pourra toujours s'appuyer sur une base de données relationnelle (disons, MySQL), mais utilisera également Redis pour assurer la persistence de certaines parties du système et Hadoop pour le traitement intensif des données. Pour faire simple, l'approche NoSQL consiste à être ouvert à des outils alternatifs pour gérer ses données.

Vous vous demandez peut-être où se situe MongoDB, dans tout ça. En tant que base de données orientée *document*, Mongo est une solution NoSQL plus généralisée. Il faut la voir comme une alternative aux bases relationnelles. Comme elles, Mongo peut tirer parti de certaines des solutions NoSQL les plus spécialisées. MongoDB a ses avantages et ses inconvénients, comme nous allons le voir par la suite.

Vous l'aurez remarqué : nous utilisons indifféremment les termes *MongoDB* et *Mongo*.

# Pour débuter #
La majeure partie de ce livre sera consacrée aux fonctionnalités principales de MongoDB. Nous nous appuierons donc sur la ligne de commande de MongoDB. Bien qu'étant un outil utile à l'apprentissage comme à l'administration, votre code, lui, utilisera un pilote MongoDB.

Cela nous amène à parler d'un élément essentiel de MongoDB : ses pilotes. Mongo dispose d'[un certain nombre de pilotes officiels](http://www.mongodb.org/display/DOCS/Drivers) pour divers langages. Ces pilotes sont semblables aux pilotes de bases de données avec lesquels vous êtes coutumier. En plus de ces pilotes, la communauté a produit d'autres bibliothèques spécifiques à des langages ou des *frameworks*. Ainsi, [NoRM](https://github.com/atheken/NoRM) est une bibliothèque C# qui implémente LINQ, et [MongoMapper](https://github.com/jnunemaker/mongomapper) est une bibliothèque Ruby compatible avec ActiveRecord.

Vous avez le choix d'utiliser directement les pilotes MongoDB dans votre code ou de passer par des bibliothèques haut niveau. Il s'agit d'une source habituelle de confusion pour les débutants, qui se demandent pourquoi il existe des pilotes officiels et des bibliothèques créées par la communauté. C'est que les premiers sont consacrés aux fonctionnalités de base de communication et de connectivité avec MongoDB alors que les seconds concernent des implémentations spécifiques à des langages ou des *frameworks*.

Lors de votre lecture, je vous encourage à jouer avec MongoDB pour reproduire ce que je présente, mais aussi pour répondre aux questions qui pourraient vous venir. Il est très facile de débuter avec MongoDB. Prenons quelques instants pour tout installer.

1. Rendez vous sur la [page de téléchargement officiel](http://www.mongodb.org/downloads) et récupérez les binaires de la première ligne (la version stable recommandée) pour le système d'exploitation de votre choix. Pour le développement, vous pouvez utiliser indifféremment les versions 32 et 64 bits.

2. Décompressez l'archive (où vous le souhaitez) et naviguez jusqu'au sous-répertoire `bin`. Ne lancez rien pour l'instant, mais sachez que `mongod` est le processus serveur et `mongo` le *shell* client. Ce sont les deux exécutables avec lesquels vous allez passer le plus de temps.

3. Créez un fichier texte dans le sous-répertoire `bin` et nommez-le `mongodb.config`

4. Ajoutez une ligne dans mongodb.config : `dbpath=PATH_TO_WHERE_YOU_WANT_TO_STORE_YOUR_DATABASE_FILES`. Ainsi, pour Windows, cela donnera quelque chose comme `dbpath=c:\mongodb\data`, et pour Linux `dbpath=/var/lib/mongodb/data`.

5. Assurez-vous que le chemin `dbpath` indiqué existe bien.

6. Lancez mongod avec le paramètre `--config /path/to/your/mongodb.config`.

Pour les utilisateurs de Windows, si vous avez extrait le fichier téléchargé dans `c:\mongodb\` et que vous avez créé `c:\mongodb\data\`, vous devrez indiquer `dbpath=c:\mongodb\data\` dans `c:\mongodb\bin\mongodb.config`. Vous pourrez alors lancer `mongod` depuis l'invite de commandes en utilisant `c:\mongodb\bin\mongod --config c:\mongodb\bin\mongodb.config`.

N'hésitez pas à ajouter le répertoire `bin` à votre *PATH* pour rendre tout ceci moins verbeux. Les utilisateurs de MacOS X et de Linux pourront suivre les mêmes instructions, à part que les chemins seront différents.

Si tout s'est bien passé, MongoDB sera fonctionnel. Si vous obtenez une erreur, lisez attentivement les messages d'erreur : le programme explique généralement très bien ce qui ne va pas.

Vous pouvez désormais lancer `mongo` (sans le *d* !), ce qui connectera un *shell* à votre serveur. Essayez de taper `db.version()` pour vérifier que tout fonctionne bien. Vous devriez obtenir le numéro de la version que vous avez installée.

# Chapitre 1 - Les bases #
Commençons par nous familiariser avec les mécanismes de base de MongoDB. Bien sûr, c'est crucial pour comprendre MongoDB, mais cela nous aidera également à répondre aux questions de plus haut niveau sur le rôle de MongoDB.

Pour débuter, il nous faut comprendre six concepts simples.

1. MongoDB dispose du concept de `databases` (base de données) qui vous est certainement familier (pour les utilisateurs d'Oracle, il s'agit des `schemas`). Une instance de MongoDB peut contenir zéro et plus bases de données, chacune étant un conteneur de haut niveau pour tout le reste.

2. Une base peut contenir zéro et plus `collections`. Une collection a suffisamment en commun avec les `tables` classiques pour les considérer comme équivalentes.

3. Une collection se compose de zéro et plus `documents`. Là encore, un document est semblable à un `row` (*ligne*).

4. Un document contient un ou plusieurs `fields` (*champs*), qui ressemblent étrangement aux `columns` (*colonnes*).

5. Dans MongoDB, les `indexes` (*index*) fonctionnent comme leur équivalent des SGBD.

6. Les `cursors` (*curseurs*) sont différents des autres concepts mais suffisamment importants (et souvent oubliés !) pour mériter qu'on s'y attarde. Ce qu'il faut comprendre, c'est que lorsque l'on demande des données à MongoDB, on obtient un curseur ; on peut le manipuler, le compter, le déplacer, sans *réellement* récupérer les données.

Pour résumer, MongoDB est composé de `databases` qui contiennent des `collections`. Une `collection` rassemble des `documents` formé par des `fields`. Les `collections` peuvent être `indexées`, ce qui améliore les performances de recherche et de tri. Enfin, lorsque l'on demande des données à MongoDB, on récupère en fait un `cursor`, dont l'exécution n'est effectuée que quand elle est vraiment nécessaire.

Pourquoi cette nouvelle terminologie (collection/table, document/row et field/column) ? Pas simplement pour rendre les choses plus compliquées : même si ces concepts sont proches de leurs équivalents relationnels, ils ne sont pas identiques. La principale différence repose sur le fait que les BDD relationnelles définissent leurs `columns` au niveau d'une `table` alors qu'une BDD orientée document définit ses `fields` au niveau d'un document. Autrement dit, chaque `document` au sein d'une `collection` peut contenir son propre ensemble de `fields` uniques. Ainsi, une `collection` n'est qu'un `table` simplifiée alors qu'un `document` contient bien plus d'informations qu'un `row`.

Même si ces concepts sont essentiels, ne paniquez pas si tout n'est pas encore clair pour vous. Après quelques *inserts*, vous comprendrez ce que cela signifie. Pour faire simple, une collection n'impose pas un format strict (on parle de *schema-less*), mais les fields sont contrôlés au sein de chaque document. Les avantages et inconvénients de cette méthode seront présentés plus loin.

Mettons la main à la pâte. Si vous ne l'avez pas encore fait, lancez le serveur `mongod` et un *shell* mongo. Le *shell* accepte le JavaScript. Vous pouvez exécuter des commandes globales, comme `help` ou `exit`. Les commandes que vous exécutez sur la base de données courante se lancent avec l'objet `db`, comme ceci : `db.help()` ou `db.stats()`. Les commandes appliquées à une collection spécifique, ce que nous allons souvent faire par la suite, se basent sur l'objet `db.NOM_DE_LA_COLLECTION`, par exemple : `db.unicorns.help()` ou `db.unicorns.count()`.

Tapez `db.help()` : vous obtenez la liste des commandes que vous pouvez appliquer à l'objet `db`.

Une remarque : comme il s'agit d'un *shell* JavaScript, si vous lancez une méthode sans les parenthèses `()` à la fin, vous verrez le code de la méthode au lieu de l'exécuter. Ne soyez pas surpris quand vous oublierez de les mettre et que vous aurez comme résultat `function (...){`. Ainsi, si vous tapez `db.help` sans les parenthèses, vous verrez l'implémentation interne de la méthode `help`.

Tout d'abord, nous allons utiliser la méthode globale `use` pour changer de BDD : tapez `use learn`. Peu importe que la base n'existe pas encore : la première collection que nous allons ajouter créera effectivement la base `learn`. Maintenant que vous êtes dans une base, vous pouvez commencer à exécuter des commandes de base, comme `db.getCollectionNames()`. Si vous tapez cela, vous devriez obtenir un tableau vide (`[]`). Puisque les collections sont *schema-less*, nous n'avons pas besoin de les créer : il suffit d'insérer un document dans une nouvelle collection. Pour ce faire, on utilise la commande `insert` à laquelle on fournit le document à insérer :

	db.unicorns.insert({name: 'Aurora', gender: 'f', weight: 450})

La ligne ci-dessus exécute `insert` dans la collection `unicorns` en lui fournissant un unique argument. En interne, MongoDB utilise un format JSON binaire sérialisé. En externe, cela veut dire qu'on utilise beaucoup JSON, comme pour nos paramètres ci-dessus. Si l'on exécute maintenant `db.getCollectionNames()`, on verra deux collections : `unicorns` et `system.indexes`. La collection `system.indexes` est créée une fois par base et contient l'information sur l'index de notre BDD.

Vous pouvez maintenant utiliser la commande `find` dans `unicorns` pour obtenir une liste de documents :

	db.unicorns.find()

Vous remarquerez que, en plus des données que vous avez spécifiées, il y a un champ `_id`. Chaque document doit contenir un champ `_id` unique. Vous pouvez l'indiquer vous-même, mais la plupart du temps vous laisserez sûrement MongoDB générer un *ObjectId* pour vous. Le champ `_id` est indexé par défaut, ce qui explique que la collection `system.indexes` ait été créée. Vous pouvez étudier `system.indexes` :

	db.system.indexes.find()

Vous voyez là le nom de l'index, la BDD et la collection pour lesquelles il a été créé, et les champs inclus dans l'index.

Revenons à nos collections *schema-less*. Essayez d'insérer un document totalement différent dans `unicorns`, par exemple :

	db.unicorns.insert({name: 'Leto', gender: 'm', planet: 'Arrakis', worm: false})


Là encore, utilisez `find` pour lister les documents. Quand nous en saurons un peu plus, nous discuterons de ce comportement intéressant de MongoDB, mais vous devriez déjà commencer à comprendre en quoi les solutions classiques ne convenaient plus.

## Maîtriser les sélecteurs ##
En plus des six concepts que nous avons explorés, il y a un aspect pratique de MongoDB qu'il est important de bien comprendre, avant de pouvoir aborder des sujets plus complexes : il s'agit des *query selectors*, les sélecteurs de requête. Un *query selector* est l'équivalent pour MongoDB de la clause `where` d'une requête SQL. On l'utilise pour trouver, compter, mettre à jour et effacer des documents dans des sélections. Un sélecteur est un objet JSON ; le plus simple, `{}`, sélectionne tous les documents (`null` marche aussi). Si l'on voulait trouver toutes les unicorns femelles, on pourrait utiliser `{gender: 'f'}`.

Avant de se plonger plus avant dans les sélecteurs, nous allons créer des données avec lesquelles jouer. D'abord, nous allons supprimer ce que nous avons mis précédemment dans `unicorns` avec `db.unicorns.remove()` (nous ne précisons pas de sélecteur, donc tous les documents seront supprimés). Maintenant, saisissez les commandes suivantes pour obtenir des données (je vous recommande d'utiliser un coper-coller) :

	db.unicorns.insert({name: 'Horny', dob: new Date(1992,2,13,7,47), loves: ['carrot','papaya'], weight: 600, gender: 'm', vampires: 63});
	db.unicorns.insert({name: 'Aurora', dob: new Date(1991, 0, 24, 13, 0), loves: ['carrot', 'grape'], weight: 450, gender: 'f', vampires: 43});
	db.unicorns.insert({name: 'Unicrom', dob: new Date(1973, 1, 9, 22, 10), loves: ['energon', 'redbull'], weight: 984, gender: 'm', vampires: 182});
	db.unicorns.insert({name: 'Roooooodles', dob: new Date(1979, 7, 18, 18, 44), loves: ['apple'], weight: 575, gender: 'm', vampires: 99});
	db.unicorns.insert({name: 'Solnara', dob: new Date(1985, 6, 4, 2, 1), loves:['apple', 'carrot', 'chocolate'], weight:550, gender:'f', vampires:80});
	db.unicorns.insert({name: 'Ayna', dob: new Date(1998, 2, 7, 8, 30), loves: ['strawberry', 'lemon'], weight: 733, gender: 'f', vampires: 40});
	db.unicorns.insert({name: 'Kenny', dob: new Date(1997, 6, 1, 10, 42), loves: ['grape', 'lemon'], weight: 690,  gender: 'm', vampires: 39});
	db.unicorns.insert({name: 'Raleigh', dob: new Date(2005, 4, 3, 0, 57), loves: ['apple', 'sugar'], weight: 421, gender: 'm', vampires: 2});
	db.unicorns.insert({name: 'Leia', dob: new Date(2001, 9, 8, 14, 53), loves: ['apple', 'watermelon'], weight: 601, gender: 'f', vampires: 33});
	db.unicorns.insert({name: 'Pilot', dob: new Date(1997, 2, 1, 5, 3), loves: ['apple', 'watermelon'], weight: 650, gender: 'm', vampires: 54});
    db.unicorns.insert({name: 'Nimue', dob: new Date(1999, 11, 20, 16, 15), loves: ['grape', 'carrot'], weight: 540, gender: 'f'});
	db.unicorns.insert({name: 'Dunx', dob: new Date(1976, 6, 18, 18, 18), loves: ['grape', 'watermelon'], weight: 704, gender: 'm', vampires: 165});

Nous avons des données, nous allons pouvoir maîtriser les sélecteurs. `{champ: valeur}` est utilisé pour trouver des documents où le `champ` est égal à `valeur`. Pour obtenir un `et`, on utilise `{champ1: valeur1, champ2: valeur2}`. Pour effectuer les comparaisons inférieur, inférieur ou égal, supérieur, supérieur ou égal, et différent, on utilise `$lt`, `$lte`, `$gt`, `$gte`, et `$ne`. Par exemple, pour trouver les unicorns mâles qui pèsent plus de 700 livres, on pourrait utiliser :

	db.unicorns.find({gender: 'm', weight: {$gt: 700}})
	// ou encore (pas strictement identique, mais utilisé pour montrer les possibilités)
	db.unicorns.find({gender: {$ne: 'f'}, weight: {$gte: 701}})

L'opérateur `$exists` est utilisé pour valider la présence ou l'absence d'un champ. Ainsi, l'exemple :

	db.unicorns.find({vampires: {$exists: false}})

ne devrait retourner qu'un seul document. Pour appliquer l'opérateur `ou` plutôt que `et`, on utilise `$or` et on lui assigne la liste des valeurs auxquelles appliquer le `ou` :

	db.unicorns.find({gender: 'f', $or: [{loves: 'apple'}, {loves: 'orange'}, {weight: {$lt: 500}}]})

La commande ci-dessus va retourner toutes les unicorns femelles qui soit aiment les apples, soit aiment les oranges, soit pèsent moins de 500 livres.

Cet exemple est très intéressant : vous l'avez peut-être déjà remarqué, mais le champ `loves` est un tableau. MongoDB accepte les tableaux comme objets à part entière, ce qui est extrêmement pratique. Une fois que vous commencerez à l'utiliser, vous vous demanderez comment vous faisiez, avant. Plus intéressant encore, la facilité avec laquelle on peut faire une sélection sur la valeur d'un tableau : `{loves: 'watermelon'}` va retourner tous les documents dans lesquels `watermelon` est une valeur de `loves`.

Il y a d'autres opérateurs que ceux que nous avons vus pour l'instant. Le plus flexible est `$where` qui permet de passer du JavaScript à exécuter sur le serveur. Ils sont tous décrits dans la section [Advanced Queries](http://www.mongodb.org/display/DOCS/Advanced+Queries#AdvancedQueries) du site de MongoDB. Ceux que nous avons vus jusqu'à présents sont ceux dont vous avez besoin pour démarrer. Ce sont aussi ceux que vous utiliserez le plus souvent.

Nous avons vu que ces sélecteurs peuvent être utilisés avec la commande `find`. Ils peuvent aussi être utilisés avec la commande `remove` que nous avons abordée brièvement, la commande `count` dont vous devinez probablement l'utilité, et la commande `update` avec laquelle nous allons passer du temps plus tard.

Le `ObjectId` que MongoDB a généré pour notre champ `_id` peut être sélectionné comme ceci :

	db.unicorns.find({_id: ObjectId("monObjectId")})

## Résumé du chapitre ##
Nous n'avons pas encore abordé la commande `update` ou certaines utilisations avancées de `find`. Par contre, nous avons fait fonctionner MongoDB, survolé les commandes `insert` et `remove` (il n'y a pas grand chose de plus à savoir !), et présenté `find` et les `sélecteurs` MongoDB. Nous sommes partis sur des bases solides. Croyez-le ou pas, mais vous connaissez désormais l'essentiel de ce qu'il y a à savoir sur MongoDB : c'est conçuç pour être rapide à apprendre et facile à utiliser. Je recommande vivement de jouer encore un peu avec votre BDD locale avant de passer à la suite. Insérez des documents différents, éventuellement dans de nouvelles collections, et familiarisez-vous avec les sélecteurs. Utilisez `find`, `count`, et `remove`. Après quelques essais par vous-même, ce qui vous avait échappé au début devrait devenir plus clair.

# Chapitre 2 - Update #
Dans le premier chapitre, nous avons présenté trois des quatre opérations CRUD (*create*, *read*, *update*, et *delete* soit créer, lire, mettre à jour et effacer). Le présent chapitre est donc consacré à celle qui manque : `update`, et à son fonctionnement parfois un peu surprenant.

## Update : Replace ou $set ? ##
Dans sa forme la plus simple, `update` prend deux arguments : le sélecteur (*where*) à utiliser, et le champ à mettre à jour. Si Roooooodles a pris un peu de poids, on peut exécuter :

	db.unicorns.update({name: 'Roooooodles'}, {weight: 590})

(Si vous avez joué avec votre collection `unicorns` et qu'elle ne contient plus les données originales, repartez de zéro en faisant `remove` puis en ré-utilisant le code du chapitre 1.)

S'il s'agissait de vrai code, on utiliserait sûrement `_id` pour mettre à jour les données. Mais comme je ne sais pas quel `_id` MongoDB a généré pour vous, nous allons utiliser les `noms`. Jetons maintenant un oeil sur la mise à jour :

	db.unicorns.find({name: 'Roooooodles'})

Première surprise d'`update` : aucun document n'est trouvé. En effet, le deuxième paramètre fourni est utilise pour **remplacer** l'original. Autrement dit, `update` a trouvé un document basé sur son `nom` et a remplacé le document entier par le nouveau document (le second paramètre). Ce fonctionnement est différent de celui de la commande `update` en SQL. Dans certains cas, ce comportement est très utile pour des mises à jour vraiment dynamiques. Cependant, quand vous souhaitez simplement remplacer la valeur d'un ou plusieurs champs, il est mieux d'utiliser le modificateur `$set` de MongoDB :

    db.unicorns.update({weight: 590}, {$set: {name: 'Roooooodles', dob: new Date(1979, 7, 18, 18, 44), loves: ['apple'], gender: 'm', vampires: 99}})

Nous voilà de nouveau avec les champs du début. `poids` n'a pas été modifié puisque nous ne l'avons pas spécifié. Maintenant, si l'on exécute :

	db.unicorns.find({name: 'Roooooodles'})

on obtient le résultat attendu. La méthode qu'il aurait fallu utiliser pour mettre le poids à jour est donc :

	db.unicorns.update({name: 'Roooooodles'}, {$set: {weight: 590}})

## Modificateurs de update ##
En plus de `$set`, on peut utiliser d'autres modificateurs pour faire des trucs sympas. Tous ces modificateurs de update fonctionne sur les champs, donc vous n'allez pas effacer tout votre document. Ainsi, le modificateur `$inc` est utilisé pour incrémenter un champ d'une certaine quantité, positive ou négative. Par exemple, si on a attribué à Pilot trop de *kills* de vampires, on peut corriger l'erreur avec :

	db.unicorns.update({name: 'Pilot'}, {$inc: {vampires: -2}})

Si Aurora succombe à l'attrait des sugarries, on peut ajouter une valeur à son champ `loves` avec le modificateur `$push` :

	db.unicorns.update({noms: 'Aurora'}, {$push: {loves: 'sugar'}})

La section [Updating](http://www.mongodb.org/display/DOCS/Updating) du site de MongoDB dispose de plus d'information sur les autres modificateurs disponibles.

## Upserts ##
L'une des bonnes surprises de `update` est qu'on peut utiliser un `upsert`. Un `upsert` met à jour un document s'il existe ou le crée sinon. C'est très utile dans certaines situations : vous les reconnaîtrez quand vous les verrez. Pour utiliser `upsert`, il faut utiliser un troisième paramètre, `true`.

Exemple trivial : un compteur de visites pour un site. Si on voulait avoir un compte total en temps réel, il faudrait vérifier si une entrée existe déjà pour la page, et décider d'utiliser `update` ou `insert`. Avec un troisième paramètre omis (ou `false`), les commandes suivantes ne feraient rien :

	db.visites.update({page: 'unicorns'}, {$inc: {visites: 1}});
	db.visites.find();

Par contre, si on active les upserts, le résultat est différent :

	db.visites.update({page: 'unicorns'}, {$inc: {visites: 1}}, true);
	db.visites.find();

Comme aucun document n'existe avec un champ `page` avec la valeur `unicorns`, un nouveau document est créé. Si la commande est exécutée à nouveau, le document existant est mis à jour et `visites` est incrémenté à 2.

	db.visites.update({page: 'unicorns'}, {$inc: {visites: 1}}, true);
	db.visites.find();

## Updates multiples ##
La dernière surprise que nous réserve `update`, c'est que, par défaut, un seul document est mis à jour. Dans les exemples que nous avons vus, cela paraît logique. En revanche, si l'on exécute quelque chose comme ça :

	db.unicorns.update({}, {$set: {vacciné: true }});
	db.unicorns.find({vacciné: true});

on s'attendrait à récupérer toutes nos chères unicorns qui doivent être vaccinées. Pour obtenir le comportement désiré, il faut ajouter un quatrième paramètre et lui assigner `true` :

	db.unicorns.update({}, {$set: {vacciné: true }}, false, true);
	db.unicorns.find({vacciné: true});

## Résumé du chapitre ##
Ce chapitre a conclu notre présentation des opérations CRUD disponibles pour une collection. Nous nous sommes intéressés en particulier à `update` et avons observé trois comportements intéressants. D'abord, contrairement à SQL, un `update` MongoDB remplace le document trouvé. Le modificateur `$set` est donc très utile. Ensuite, `update` propose une variante `upsert` très utile, surtout combinée au modificateur `$inc`. Enfin, par défaut, `update` ne met à jour que le premier document trouvé.

Rappelez-vous que nous utilisons MongoDB depuis son *shell*. Le pilote ou la bibliothèque que vous utiliserez pourront modifier ces comportements par défaut ou exposer une API différente. Ainsi, le pilote Ruby fusionne les deux derniers paramètres en un seul élément : `{:upsert => false, :multi => false}`. De même, les pilote PHP les fusionne dans un tableau : `array('upsert' => false, 'multiple' => false)`.

# Chapter 3 - Maîtriser find #
Chapter 1 provided a superficial look at the `find` command. There's more to `find` than understanding `selectors` though. We already mentioned that the result from `find` is a `cursor`. We'll now look at exactly what this means in more detail.

## Field Selection ##
Before we jump into `cursors`, you should know that `find` takes a second optional parameter. This parameter is the list of fields we want to retrieve. For example, we can get all of the unicorns' names by executing:

	db.unicorns.find(null, {name: 1});

By default, the `_id` field is always returned. We can explicitly exclude it by specifying `{name:1, _id: 0}`.

Aside from the `_id` field, you cannot mix and match inclusion and exclusion. If you think about it, that actually makes sense. You either want to select or exclude one or more fields explicitly.

## Ordering ##
A few times now I've mentioned that `find` returns a cursor whose execution is delayed until needed. However, what you've no doubt observed from the shell is that `find` executes immediately. This is a behavior of the shell only. We can observe the true behavior of `cursors` by looking at one of the methods we can chain to `find`. The first that we'll look at is `sort`. `sort` works a lot like the field selection from the previous section. We specify the fields we want to sort on, using 1 for ascending and -1 for descending. For example:

	//heaviest unicorns first
	db.unicorns.find().sort({weight: -1})

	//by unicorn name then vampire kills:
	db.unicorns.find().sort({name: 1, vampires: -1})

As with a relational database, MongoDB can use an index for sorting. We'll look at indexes in more detail later on. However, you should know that MongoDB limits the size of your sort without an index. That is, if you try to sort a large result set which can't use an index, you'll get an error. Some people see this as a limitation. In truth, I wish more databases had the capability to refuse to run unoptimized queries. (I won't turn every MongoDB drawback into a positive, but I've seen enough poorly optimized databases that I sincerely wish they had a strict-mode.)

## Paging ##
Paging results can be accomplished via the `limit` and `skip` cursor methods. To get the second and third heaviest unicorn, we could do:

	db.unicorns.find().sort({weight: -1}).limit(2).skip(1)

Using `limit` in conjunction with `sort`, is a good way to avoid running into problems when sorting on non-indexed fields.

## Count ##
The shell makes it possible to execute a `count` directly on a collection, such as:

	db.unicorns.count({vampires: {$gt: 50}})

In reality, `count` is actually a `cursor` method, the shell simply provides a shortcut. Drivers which don't provide such a shortcut need to be executed like this (which will also work in the shell):

	db.unicorns.find({vampires: {$gt: 50}}).count()

## In This Chapter ##
Using `find` and `cursors` is a straightforward proposition. There are a few additional commands that we'll either cover in later chapters or which only serve edge cases, but, by now, you should be getting pretty comfortable working in the mongo shell and understanding the fundamentals of MongoDB.

# Chapter 4 - Data Modeling #
Let's shift gears and have a more abstract conversation about MongoDB. Explaining a few new terms and some new syntax is a trivial task. Having a conversation about modeling with a new paradigm isn't as easy. The truth is that most of us are still finding out what works and what doesn't when it comes to modeling with these new technologies. It's a conversation we can start having, but ultimately you'll have to practice and learn on real code.

Out of all NoSQL databases, document-oriented databases are probably the most similar to relational databases - at least when it comes to modeling. The differences which exist are subtle but that doesn't mean they aren't important.

## No Joins ##
The first and most fundamental difference that you'll need to get comfortable with is MongoDB's lack of joins. I don't know the specific reason why some type of join syntax isn't supported in MongoDB, but I do know that joins are generally seen as non-scalable. That is, once you start to split your data horizontally, you end up performing your joins on the client (the application server) anyways. Regardless of the reasons, the fact remains that data *is* relational, and MongoDB doesn't support joins.

Without knowing anything else, to live in a join-less world, we have to do joins ourselves within our application's code. Essentially we need to issue a second query to `find` the relevant data. Setting our data up isn't any different than declaring a foreign key in a relational database. Let's give a little less focus to our beautiful `unicorns` and a bit more time to our `employees`. The first thing we'll do is create an employee (I'm providing an explicit `_id` so that we can build coherent examples)

	db.employees.insert({_id: ObjectId("4d85c7039ab0fd70a117d730"), name: 'Leto'})

Now let's add a couple employees and set their manager as `Leto`:

	db.employees.insert({_id: ObjectId("4d85c7039ab0fd70a117d731"), name: 'Duncan', manager: ObjectId("4d85c7039ab0fd70a117d730")});
	db.employees.insert({_id: ObjectId("4d85c7039ab0fd70a117d732"), name: 'Moneo', manager: ObjectId("4d85c7039ab0fd70a117d730")});


(It's worth repeating that the `_id` can be any unique value. Since you'd likely use an `ObjectId` in real life, we'll use them here as well.)

Of course, to find all of Leto's employees, one simply executes:

	db.employees.find({manager: ObjectId("4d85c7039ab0fd70a117d730")})

There's nothing magical here. In the worst cases, most of the time, the lack of join will merely require an extra query (likely indexed).

## Arrays and Embedded Documents ##
Just because MongoDB doesn't have joins doesn't mean it doesn't have a few tricks up its sleeve. Remember when we quickly saw that MongoDB supports arrays as first class objects of a document? It turns out that this is incredibly handy when dealing with many-to-one or many-to-many relationships. As a simple example, if an employee could have two managers, we could simply store these in an array:

	db.employees.insert({_id: ObjectId("4d85c7039ab0fd70a117d733"), name: 'Siona', manager: [ObjectId("4d85c7039ab0fd70a117d730"), ObjectId("4d85c7039ab0fd70a117d732")] })

Of particular interest is that, for some documents, `manager` can be a scalar value, while for others it can be an array. Our original `find` query will work for both:

	db.employees.find({manager: ObjectId("4d85c7039ab0fd70a117d730")})

You'll quickly find that arrays of values are much more convenient to deal with than many-to-many join-tables.

Besides arrays, MongoDB also supports embedded documents. Go ahead and try inserting a document with a nested document, such as:

	db.employees.insert({_id: ObjectId("4d85c7039ab0fd70a117d734"), name: 'Ghanima', family: {mother: 'Chani', father: 'Paul', brother: ObjectId("4d85c7039ab0fd70a117d730")}})

In case you are wondering, embedded documents can be queried using a dot-notation:

	db.employees.find({'family.mother': 'Chani'})

We'll briefly talk about where embedded documents fit and how you should use them.

## DBRef ##
MongoDB supports something known as `DBRef` which is a convention many drivers support. When a driver encounters a `DBRef` it can automatically pull the referenced document. A `DBRef` includes the collection and id of the referenced document. It generally serves a pretty specific purpose: when documents from the same collection might reference documents from a different collection from each other. That is, the `DBRef` for document1 might point to a document in `managers` whereas the `DBRef` for document2 might point to a document in `employees`.


## Denormalization ##
Yet another alternative to using joins is to denormalize your data. Historically, denormalization was reserved for performance-sensitive code, or when data should be snapshotted (like in an audit log). However, with the ever-growing popularity of NoSQL, many of which don't have joins, denormalization as part of normal modeling is becoming increasingly common. This doesn't mean you should duplicate every piece of information in every document. However, rather than letting fear of duplicate data drive your design decisions, consider modeling your data based on what information belongs to what document.

For example, say you are writing a forum application. The traditional way to associate a specific `user` with a `post` is via a `userid` column within `posts`. With such a model, you can't display `posts` without retrieving (joining to) `users`. A possible alternative is simply to store the `name` as well as the `userid` with each `post`. You could even do so with an embedded document, like `user: {id: ObjectId('Something'), name: 'Leto'}`. Yes, if you let users change their name, you'll have to update each document (which is 1 extra query).

Adjusting to this kind of approach won't come easy to some. In a lot of cases it won't even make sense to do this. Don't be afraid to experiment with this approach though. It's not only suitable in some circumstances, but it can also be the right way to do it.

## Which Should You Choose? ##
Arrays of ids are always a useful strategy when dealing with one-to-many or many-to-many scenarios. It's probably safe to say that `DBRef`s aren't used very often, though you can certainly experiment and play with them. That generally leaves new developers unsure about using embedded documents versus doing manual referencing.

First, you should know that an individual document is currently limited to 16 megabytes in size. Knowing that documents have a size limit, though quite generous, gives you some idea of how they are intended to be used. At this point, it seems like most developers lean heavily on manual references for most of their relationships. Embedded documents are frequently leveraged, but mostly for small pieces of data which we want to always pull with the parent document. A real world example I've used is to store an `accounts` document with each user, something like:

	db.users.insert({name: 'leto', email: 'leto@dune.gov', account: {allowed_gholas: 5, spice_ration: 10}})

That doesn't mean you should underestimate the power of embedded documents or write them off as something of minor utility. Having your data model map directly to your objects makes things a lot simpler and often does remove the need to join. This is especially true when you consider that MongoDB lets you query and index fields of an embedded document.

## Few or Many Collections ##
Given that collections don't enforce any schema, it's entirely possible to build a system using a single collection with a mishmash of documents.  From what I've seen, most MongoDB systems are laid out similarly to what you'd find in a relational system. In other words, if it would be a table in a relational database, it'll likely be a collection in MongoDB (many-to-many join tables being an important exception).

The conversation gets even more interesting when you consider embedded documents. The example that frequently comes up is a blog. Should you have a `posts` collection and a `comments` collection, or should each `post` have an array of `comments` embedded within it? Setting aside the 16MB document size limit for the time being (all of *Hamlet* is less than 200KB, so just how popular is your blog?), most developers still prefer to separate things out. It's simply cleaner and more explicit.

There's no hard rule (well, aside from 16MB). Play with different approaches and you'll get a sense of what does and does not feel right.

## In This Chapter ##
Our goal in this chapter was to provide some helpful guidelines for modeling your data in MongoDB. A starting point, if you will. Modeling in a document-oriented system is different, but not too different, than in a relational world. You have a bit more flexibility and one constraint, but for a new system, things tend to fit quite nicely. The only way you can go wrong is by not trying.

# Chapter 5 - When To Use MongoDB #
By now you should have a feel for where and how it might fit into your existing system. There are enough new and competing storage technologies that it's easy to get overwhelmed by all of the choices.

For me, the most important lesson, which has nothing to do with MongoDB, is that you no longer have to rely on a single solution for dealing with your data. No doubt, a single solution has obvious advantages, and for a lot projects - possibly even most - a single solution is the sensible approach. The idea isn't that you *must* use different technologies, but rather that you *can*. Only you know whether the benefits of introducing a new solution outweigh the costs.

With that said, I'm hopeful that what you've seen so far has made you see MongoDB as a general solution. It's been mentioned a couple times that document-oriented databases share a lot in common with relational databases. Therefore, rather than tiptoeing around it, let's simply state that MongoDB should be seen as a direct alternative to relational databases. Where one might see Lucene as enhancing a relational database with full text indexing, or Redis as a persistent key-value store, MongoDB is a central repository for your data.

Notice that I didn't call MongoDB a *replacement* for relational databases, but rather an *alternative*. It's a tool that can do what a lot of other tools can do. Some of it MongoDB does better, some of it MongoDB does worse. Let's dissect things a little further.

## Schema-less ##
An oft-touted benefit of document-oriented database is that they are schema-less. This makes them much more flexible than traditional database tables. I agree that schema-less is a nice feature, but not for the main reason most people mention.

People talk about schema-less as though you'll suddenly start storing a crazy mishmash of data. There are domains and data sets which can really be a pain to model using relational databases, but I see those as edge cases. Schema-less is cool, but most of your data is going to be highly structured. It's true that having an occasional mismatch can be handy, especially when you introduce new features, but in reality it's nothing a nullable column probably wouldn't solve just as well.

For me, the real benefit of schema-less design is the lack of setup and the reduced friction with OOP. This is particularly true when you're working with a static language. I've worked with MongoDB in both C# and Ruby, and the difference is striking. Ruby's dynamism and its popular ActiveRecord implementations already reduce much of the object-relational impedance mismatch. That isn't to say MongoDB isn't a good match for Ruby, it really is. Rather, I think most Ruby developers would see MongoDB as an incremental improvement, whereas C# or Java developers would see a fundamental shift in how they interact with their data.

Think about it from the perspective of a driver developer. You want to save an object? Serialize it to JSON (technically BSON, but close enough) and send it to MongoDB. There is no property mapping or type mapping. This straightforwardness definitely flows to you, the end developer.

## Writes ##
One area where MongoDB can fit a specialized role is in logging. There are two aspects of MongoDB which make writes quite fast. First, you can send a write command and have it return immediately without waiting for it to actually write. Secondly, with the introduction of journaling in 1.8, and enhancements made in 2.0, you can control the write behavior with respect to data durability. These settings, in addition to specifying how many servers should get your data before being considered successful, are configurable per-write, giving you a great level of control over write performance and data durability.

In addition to these performance factors, log data is one of those data sets which can often take advantage of schema-less collections. Finally, MongoDB has something called a [capped collection](http://www.mongodb.org/display/DOCS/Capped+Collections). So far, all of the implicitly created collections we've created are just normal collections. We can create a capped collection by using the `db.createCollection` command and flagging it as capped:

	//limit our capped collection to 1 megabyte
	db.createCollection('logs', {capped: true, size: 1048576})

When our capped collection reaches its 1MB limit, old documents are automatically purged. A limit on the number of documents, rather than the size, can be set using `max`. Capped collections have some interesting properties. For example, you can update a document but it can't grow in size. Also, the insertion order is preserved, so you don't need to add an extra index to get proper time-based sorting.

This is a good place to point out that if you want to know whether your write encountered any errors (as opposed to the default fire-and-forget), you simply issue a follow-up command: `db.getLastError()`. Most drivers encapsulate this as a *safe write*, say by specifying `{:safe => true}` as a second parameter to `insert`.

## Durability ##
Prior to version 1.8, MongoDB didn't have single-server durability. That is, a server crash would likely result in lost data. The solution had always been to run MongoDB in a multi-server setup (MongoDB supports replication). One of the major features added to 1.8 was journaling. To enable it add a new line with `journal=true` to the `mongodb.config` file we created when we first setup MongoDB (and restart your server if you want it enabled right away). You probably want journaling enabled (it'll be a default in a future release). Although, in some circumstances the extra throughput you get from disabling journaling might be a risk you are willing to take. (It's worth pointing out that some types of applications can easily afford to lose data).

Durability is only mentioned here because a lot has been made around MongoDB's lack of single-server durability. This'll likely show up in Google searches for some time to come. Information you find about this missing feature is simply out of date.

## Full Text Search ##
True full text search capability is something that'll hopefully come to MongoDB in a future release. With its support for arrays, basic full text search is pretty easy to implement. For something more powerful, you'll need to rely on a solution such as Lucene/Solr. Of course, this is also true of many relational databases.

## Transactions ##
MongoDB doesn't have transactions. It has two alternatives, one which is great but with limited use, and the other that is cumbersome but flexible.

The first is its many atomic operations. These are great, so long as they actually address your problem. We already saw some of the simpler ones, like `$inc` and `$set`. There are also commands like `findAndModify` which can update or delete a document and return it atomically.

The second, when atomic operations aren't enough, is to fall back to a two-phase commit. A two-phase commit is to transactions what manual dereferencing is to joins. It's a storage-agnostic solution that you do in code. Two-phase commits are actually quite popular in the relational world as a way to implement transactions across multiple databases. The MongoDB website [has an example](http://www.mongodb.org/display/DOCS/two-phase+commit) illustrating the most common scenario (a transfer of funds). The general idea is that you store the state of the transaction within the actual document being updated and go through the init-pending-commit/rollback steps manually.

MongoDB's support for nested documents and schema-less design makes two-phase commits slightly less painful, but it still isn't a great process, especially when you are just getting started with it.

## Data Processing ##
MongoDB relies on MapReduce for most data processing jobs. It has some [basic aggregation](http://www.mongodb.org/display/DOCS/Aggregation) capabilities, but for anything serious, you'll want to use MapReduce. In the next chapter we'll look at MapReduce in detail. For now you can think of it as a very powerful and different way to `group by` (which is an understatement). One of MapReduce's strengths is that it can be parallelized for working with large sets of data. However, MongoDB's implementation relies on JavaScript, which is single-threaded. The point? For processing of large data, you'll likely need to rely on something else, such as Hadoop. Thankfully, since the two systems really do complement each other, there's a [MongoDB adapter for Hadoop](https://github.com/mongodb/mongo-hadoop).

Of course, parallelizing data processing isn't something relational databases excel at either. There are plans for future versions of MongoDB to be better at handling very large sets of data.

## Geospatial ##
A particularly powerful feature of MongoDB is its support for geospatial indexes. This allows you to store x and y coordinates within documents and then find documents that are `$near` a set of coordinates or `$within` a box or circle. This is a feature best explained via some visual aids, so I invite you to try the [5 minute geospatial interactive tutorial](http://tutorial.mongly.com/geo/index), if you want to learn more.

## Tools and Maturity ##
You probably already know the answer to this, but MongoDB is obviously younger than most relational database systems. This is absolutely something you should consider, though how much it matters depends on what you are doing and how you are doing it. Nevertheless, an honest assessment simply can't ignore the fact that MongoDB is younger and the available tooling around isn't great (although the tooling around a lot of very mature relational databases is pretty horrible too!). As an example, the lack of support for base-10 floating point numbers will obviously be a concern (though not necessarily a show-stopper) for systems dealing with money.

On the positive side, drivers exist for a great many languages, the protocol is modern and simple, and development is happening at blinding speeds. MongoDB is in production at enough companies that concerns about maturity, while valid, are quickly becoming a thing of the past.

## In This Chapter ##
The message from this chapter is that MongoDB, in most cases, can replace a relational database. It's much simpler and straightforward; it's faster and generally imposes fewer restrictions on application developers. The lack of transactions can be a legitimate and serious concern. However, when people ask *where does MongoDB sit with respect to the new data storage landscape?* the answer is simple: **right in the middle**.

# Chapter 6 - MapReduce #
MapReduce is an approach to data processing which has two significant benefits over more traditional solutions. The first, and main, reason it was developed is performance. In theory, MapReduce can be parallelized, allowing very large sets of data to be processed across many cores/CPUs/machines. As we just mentioned, this isn't something MongoDB is currently able to take advantage of. The second benefit of MapReduce is that you get to write real code to do your processing. Compared to what you'd be able to do with SQL, MapReduce code is infinitely richer and lets you push the envelope further before you need to use a more specialized solution.

MapReduce is a pattern that has grown in popularity, and you can make use of it almost anywhere; C#, Ruby, Java, Python and so on all have implementations. I want to warn you that at first this'll seem very different and complicated. Don't get frustrated, take your time and play with it yourself. This is worth understanding whether you are using MongoDB or not.

## A Mix of Theory and Practice ##
MapReduce is a two-step process. First you map, and then you reduce. The mapping step transforms the inputted documents and emits a key=>value pair (the key and/or value can be complex). Then, key/value pairs are grouped by key, such that values for the same key end up in an array. The reduce gets a key and this array of values emitted for that key, and produces the final result. We'll look at each step, and the output of each step.

The example that we'll be using is to generate a report of the number of hits, per day, we get on a resource (say a webpage). This is the *hello world* of MapReduce. For our purposes, we'll rely on a `hits` collection with two fields: `resource` and `date`. Our desired output is a breakdown by `resource`, `year`, `month`, `day` and `count`.

Given the following data in `hits`:

	resource     date
	index        Jan 20 2010 4:30
	index        Jan 20 2010 5:30
	about        Jan 20 2010 6:00
	index        Jan 20 2010 7:00
	about        Jan 21 2010 8:00
	about        Jan 21 2010 8:30
	index        Jan 21 2010 8:30
	about        Jan 21 2010 9:00
	index        Jan 21 2010 9:30
	index        Jan 22 2010 5:00

We'd expect the following output:

	resource  year   month   day   count
	index     2010   1       20    3
	about     2010   1       20    1
	about     2010   1       21    3
	index     2010   1       21    2
	index     2010   1       22    1

The nice thing about this type of approach to analytics is that by storing the output, reports are fast to generate and data growth is controlled (per resource that we track, we'll add at most 1 document per day.)

For the time being, focus on understanding the concept. At the end of this chapter, sample data and code will be given for you to try on your own.

The first thing to do is look at the map function. The goal of map is to make it emit a value which can be reduced. It's possible for map to emit 0 or more times. In our case, it'll always emit once (which is common). Imagine map as looping through each document in hits. For each document we want to emit a key with resource, year, month and day, and a simple value of 1:

	function() {
		var key = {
		    resource: this.resource,
		    year: this.date.getFullYear(),
		    month: this.date.getMonth(),
		    day: this.date.getDate()
		};
		emit(key, {count: 1});
	}

`this` refers to the current document being inspected. Hopefully what'll help make this clear for you is to see what the output of the mapping step is. Using our above data, the complete output would is below. The values from `emit` are grouped together, as arrays, by key:

	{resource: 'index', year: 2010, month: 0, day: 20} => [{count: 1}, {count: 1}, {count:1}]
	{resource: 'about', year: 2010, month: 0, day: 20} => [{count: 1}]
	{resource: 'about', year: 2010, month: 0, day: 21} => [{count: 1}, {count: 1}, {count:1}]
	{resource: 'index', year: 2010, month: 0, day: 21} => [{count: 1}, {count: 1}]
	{resource: 'index', year: 2010, month: 0, day: 22} => [{count: 1}]

Understanding this intermediary step is the key to understanding MapReduce. .NET and Java developers can think of it as being of type `IDictionary<object, IList<object>>` (.NET) or `HashMap<Object, ArrayList>` (Java).

Let's change our map function in some contrived way:

	function() {
		var key = {resource: this.resource, year: this.date.getFullYear(), month: this.date.getMonth(), day: this.date.getDate()};
		if (this.resource == 'index' && this.date.getHours() == 4) {
			emit(key, {count: 5});
		} else {
			emit(key, {count: 1});
		}
	}

The first intermediary output would change to:

	{resource: 'index', year: 2010, month: 0, day: 20} => [{count: 5}, {count: 1}, {count:1}]

Notice how each emit generates a new value which is grouped by our key.

The reduce function takes each of these intermediary results and outputs a final result. Here's what ours looks like:

	function(key, values) {
		var sum = 0;
		values.forEach(function(value) {
			sum += value['count'];
		});
		return {count: sum};
	};

Which would output:

	{resource: 'index', year: 2010, month: 0, day: 20} => {count: 3}
	{resource: 'about', year: 2010, month: 0, day: 20} => {count: 1}
	{resource: 'about', year: 2010, month: 0, day: 21} => {count: 3}
	{resource: 'index', year: 2010, month: 0, day: 21} => {count: 2}
	{resource: 'index', year: 2010, month: 0, day: 22} => {count: 1}

Technically, the output in MongoDB is:

	_id: {resource: 'index', year: 2010, month: 0, day: 20}, value: {count: 3}

Hopefully you've noticed that this is the final result we were after.

If you've really been paying attention, you might be asking yourself why we didn't use `sum = values.length`. This would seem like an efficient approach when you are summing an array of 1s. This however, is not guaranteed to work. Reduce can be called multiple times with only a portion of the overall values. The purpose of this is to allow the reduce function to be distributed across threads, processes or even computers. The result of two or more reduce functions are fed back into the same reduce function (many times over, for a large data set).

Going back to our example, reduce might be called once with the following input:

	{resource: 'index', year: 2010, month: 0, day: 20} => [{count: 1}, {count: 1}, {count:1}]

or it might be called twice with the output of *step 1* making up part of the input to *step 2*:

	//STEP 1
	{resource: 'index', year: 2010, month: 0, day: 20} => [{count: 1}, {count: 1}]
	//STEP 2
	{resource: 'index', year: 2010, month: 0, day: 20} => [{count: 2}, {count: 1}]

Using `sum = values.length` would incorrectly return `{count: 2}` from the second step.

This means that the structure of reduce's output must be the same as its input, and calling reduce multiple times should result in the same result (known as idempotency).

Finally, we aren't going to cover it here but it's common to chain reduce methods when performing more complex analysis.

## Pure Practical ##
With MongoDB we use the `mapReduce` command on a collection. `mapReduce` takes a map function, a reduce function and an output directive. In our shell we can create and pass a JavaScript function. From most libraries you supply a string of your functions (which is a bit ugly). First though, let's create our simple data set:

	db.hits.insert({resource: 'index', date: new Date(2010, 0, 20, 4, 30)});
	db.hits.insert({resource: 'index', date: new Date(2010, 0, 20, 5, 30)});
	db.hits.insert({resource: 'about', date: new Date(2010, 0, 20, 6, 0)});
	db.hits.insert({resource: 'index', date: new Date(2010, 0, 20, 7, 0)});
	db.hits.insert({resource: 'about', date: new Date(2010, 0, 21, 8, 0)});
	db.hits.insert({resource: 'about', date: new Date(2010, 0, 21, 8, 30)});
	db.hits.insert({resource: 'index', date: new Date(2010, 0, 21, 8, 30)});
	db.hits.insert({resource: 'about', date: new Date(2010, 0, 21, 9, 0)});
	db.hits.insert({resource: 'index', date: new Date(2010, 0, 21, 9, 30)});
	db.hits.insert({resource: 'index', date: new Date(2010, 0, 22, 5, 0)});

Now we can create our map and reduce functions (the MongoDB shell accepts multi-line statements, you'll see *...* after hitting enter to indicate more text is expected):

	var map = function() {
		var key = {resource: this.resource, year: this.date.getFullYear(), month: this.date.getMonth(), day: this.date.getDate()};
		emit(key, {count: 1});
	};

	var reduce = function(key, values) {
		var sum = 0;
		values.forEach(function(value) {
			sum += value['count'];
		});
		return {count: sum};
	};

We can pass our `map` and `reduce` functions to the `mapReduce` command by running:

	db.hits.mapReduce(map, reduce, {out: {inline:1}})

If you run the above, you should see the desired output. Setting `out` to `inline` means that the output from `mapReduce` is immediately streamed back to us. This is currently limited for results that are 16 megabytes or less. We could instead specify `{out: 'hit_stats'}` and have the results stored in the `hit_stats` collections:

	db.hits.mapReduce(map, reduce, {out: 'hit_stats'});
	db.hit_stats.find();

When you do this, any existing data in `hit_stats` is lost. If we did `{out: {merge: 'hit_stats'}}` existing keys would be replaced with the new values and new keys would be inserted as new documents. Finally, we can `out` using a `reduce` function to handle more advanced cases (such an doing an upsert).

The third parameter takes additional options, for example we could filter, sort and limit the documents that we want analyzed. We can also supply a `finalize` method to be applied to the results after the `reduce` step.

## In This Chapter ##
This is the first chapter where we covered something truly different. If it made you uncomfortable, remember that you can always use MongoDB's other [aggregation capabilities](http://www.mongodb.org/display/DOCS/Aggregation) for simpler scenarios. Ultimately though, MapReduce is one of MongoDB's most compelling features. The key to really understanding how to write your map and reduce functions is to visualize and understand the way your intermediary data will look coming out of `map` and heading into `reduce`.

# Chapter 7 - Performance and Tools #
In this last chapter, we look at a few performance topics as well as some of the tools available to MongoDB developers. We won't dive deeply into either topic, but we will examine the most important aspects of each.

## Indexes ##
At the very beginning we saw the special `system.indexes` collection which contains information on all the indexes in our database. Indexes in MongoDB work a lot like indexes in a relational database: they help improve query and sorting performance. Indexes are created via `ensureIndex`:

	// where "name" is the fieldname
	db.unicorns.ensureIndex({name: 1});

And dropped via `dropIndex`:

	db.unicorns.dropIndex({name: 1});

A unique index can be created by supplying a second parameter and setting `unique` to `true`:

	db.unicorns.ensureIndex({name: 1}, {unique: true});

Indexes can be created on embedded fields (again, using the dot-notation) and on array fields. We can also create compound indexes:

	db.unicorns.ensureIndex({name: 1, vampires: -1});

The order of your index (1 for ascending, -1 for descending) doesn't matter for a single key index, but it can have an impact for compound indexes when you are sorting or using a range condition.

The [indexes page](http://www.mongodb.org/display/DOCS/Indexes) has additional information on indexes.

## Explain ##
To see whether or not your queries are using an index, you can use the `explain` method on a cursor:

	db.unicorns.find().explain()

The output tells us that a `BasicCursor` was used (which means non-indexed), that 12 objects were scanned, how long it took, what index, if any, was used as well as a few other pieces of useful information.

If we change our query to use an index, we'll see that a `BtreeCursor` was used, as well as the index used to fulfill the request:

	db.unicorns.find({name: 'Pilot'}).explain()

## Fire And Forget Writes ##
We previously mentioned that, by default, writes in MongoDB are fire-and-forget. This can result in some nice performance gains at the risk of losing data during a crash. An interesting side effect of this type of write is that an error is not returned when an insert/update violates a unique constraint. In order to be notified about a failed write, one must call `db.getLastError()` after an insert. Many drivers abstract this detail away and provide a way to do a *safe* write - often via an extra parameter.

Unfortunately, the shell automatically does safe inserts, so we can't easily see this behavior in action.

## Sharding ##
MongoDB supports auto-sharding. Sharding is an approach to scalability which separates your data across multiple servers. A naive implementation might put all of the data for users with a name that starts with A-M on server 1 and the rest on server 2. Thankfully, MongoDB's sharding capabilities far exceed such a simple algorithm. Sharding is a topic well beyond the scope of this book, but you should know that it exists and that you should consider it, should your needs grow beyond a single server.

## Replication ##
MongoDB replication works similarly to how relational database replication works. Writes are sent to a single server, the master, which then synchronizes itself to one or more other servers, the slaves. You can control whether reads can happen on slaves or not, which can help distribute your load at the risk of reading slightly stale data. If the master goes down, a slave can be promoted to act as the new master. Again, MongoDB replication is outside the scope of this book.

While replication can improve performance (by distributing reads), its main purpose is to increase reliability. Combining replication with sharding is a common approach. For example, each shard could be made up of a master and a slave. (Technically you'll also need an arbiter to help break a tie should two slaves try to become masters. But an arbiter requires very few resources and can be used for multiple shards.)

## Stats ##
You can obtain statistics on a database by typing `db.stats()`. Most of the information deals with the size of your database. You can also get statistics on a collection, say `unicorns`, by typing `db.unicorns.stats()`. Again, most of this information relates to the size of your collection.

## Web Interface ##
Included in the information displayed on MongoDB's startup was a link to a web-based administrative tool (you might still be able to see it if you scroll your command/terminal window up to the point where you started `mongod`). You can access this by pointing your browser to <http://localhost:28017/>. To get the most out of it, you'll want to add `rest=true` to your config and restart the `mongod` process. The web interface gives you a lot of insight into the current state of your server.

## Profiler ##
You can enable the MongoDB profiler by executing:

	db.setProfilingLevel(2);

With it enabled, we can run a command:

	db.unicorns.find({weight: {$gt: 600}});

And then examine the profiler:

	db.system.profile.find()

The output tells us what was run and when, how many documents were scanned, and how much data was returned.

You can disable the profiler by calling `setProfileLevel` again but changing the argument to `0`. Another option is to specify `1` which will only profile queries that take more than 100 milliseconds. Or, you can specify the minimum time, in milliseconds, with a second parameter:

	//profile anything that takes more than 1 second
	db.setProfilingLevel(1, 1000);

## Backups and Restore ##
Within the MongoDB `bin` folder is a `mongodump` executable. Simply executing `mongodump` will connect to localhost and backup all of your databases to a `dump` subfolder. You can type `mongodump --help` to see additional options. Common options are `--db DBNAME` to back up a specific database and `--collection COLLECTIONNAME` to back up a specific collection. You can then use the `mongorestore` executable, located in the same `bin` folder, to restore a previously made backup. Again, the `--db` and `--collection` can be specified to restore a specific database and/or collection.

For example, to back up our `learn` database to a `backup` folder, we'd execute (this is its own executable which you run in a command/terminal window, not within the mongo shell itself):

	mongodump --db learn --out backup

To restore only the `unicorns` collection, we could then do:

	mongorestore --collection unicorns backup/learn/unicorns.bson

It's worth pointing out that `mongoexport` and `mongoimport` are two other executables which can be used to export and import data from JSON or CSV. For example, we can get a JSON output by doing:

	mongoexport --db learn -collection unicorns

And a CSV output by doing:

	mongoexport --db learn -collection unicorns --csv -fields name,weight,vampires

Note that `mongoexport` and `mongoimport` cannot always represent your data. Only `mongodump` and `mongorestore` should ever be used for actual backups.

## In This Chapter ##
In this chapter we looked at various commands, tools and performance details of using MongoDB. We haven't touched on everything, but we've looked at the most common ones. Indexing in MongoDB is similar to indexing with relational databases, as are many of the tools. However, with MongoDB, many of these are to the point and simple to use.

# Conclusion #
You should have enough information to start using MongoDB in a real project. There's more to MongoDB than what we've covered, but your next priority should be putting together what we've learned, and getting familiar with the driver you'll be using. The [MongoDB website](http://www.mongodb.com/) has a lot of useful information. The official [MongoDB user group](http://groups.google.com/group/mongodb-user) is a great place to ask questions.

NoSQL was born not only out of necessity, but also out of an interest in trying new approaches. It is an acknowledgement that our field is ever-advancing and that if we don't try, and sometimes fail, we can never succeed. This, I think, is a good way to lead our professional lives.
