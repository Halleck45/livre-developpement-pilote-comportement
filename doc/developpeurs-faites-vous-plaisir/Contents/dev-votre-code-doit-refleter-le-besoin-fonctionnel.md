# Votre code doit refléter le Besoin fonctionnel

Vous le savez, il arrive très souvent qu'un besoin fonctionnel évolue au cours de 
la vie d'un projet. Et, parfois, une évolution fonctionnelle, même mineure, peut 
entraîner une refonte majeure d'un code source.

C'est l'effet boule de neige : modifier un bloc de code va avoir un impact sur une 
grande partie de code source, qui elle même va concerner des domaines fonctionnels très variés, 
entraînant des régression fonctionnelle nombreuses et imprévisibles.

Si vous l'avez vécu, vous savez que c'est quelque chose qui est totalement 
incompréhensible pour une personne qui n'est pas développeur. Ce n'est pas de la mauvaise 
volonté, non ; c'est simplement que, s'il est déjà difficile de saisir pleinement qu'écrire 
du code source permet au final d'avoir une application qui marche, comprendre qu'il existe 
dans le code source des interactions fines, complexes et non liées aux domaines fonctionnels est 
une chose quasiment impossible pour un fonctionnel.

Il faut donc trouver une solution pour répondre à ceux deux questions fondamentales :

+ comment assurer la cohérence des changements techniques face aux changements fonctionnels ?
+ comment permettre aux fonctionnels d'anticiper le coût technique d'un changement fonctionnel ?

## Le Domain Driven Design

C'est justement l'enjeu du Domain Driven Design de répondre à ces questions. L'objet de 
cet ouvrage n'est pas de faire de vous des spécialistes du Domain Driven Design (ou DDD) en 
quelques pages. Non, il existe des livres très bien conçus, que je ne peux que vous inviter à 
lire *(cf. Domain Driven Design Vite Fait + DDD de Eric Envans) - Liens à mettre*

Non, l'objectif ici est de comprendre qu'il existe des démarches de travail et de conception 
logicielle qui permette de faciliter la gestion du besoin fonctionnel. Et c'est là, justement, l'objet du DDD.

Le DDD est une démarche d'échange, de conception et de développement, dont l'objectif est de calquer 
le code source (code, base de données, ressources...) sur le besoin métier.

Impossible ? Si, et même très profitable. Sitôt que vous aurez commencé à travailler de cette manière, 
vous ne pourrez plus vous en passer.

L'idée générale est d'élaborer une Langue commune (cette Langue dont nous avons déjà parlé) avec 
le fonctionnel, puis de concevoir une cartographie fonctionnelle de l'application, afin 
de n'utiliser, dans le code source, uniquement des concepts présents dans le domaine fonctionnel.

Dès lors, si cette démarche a été accomplie, il devient facile pour le développeur 
d'identifier quelle proportion de code et quels modules techniques seront impactés par une 
demande de changement fonctionnel : il lui "suffit" de superposer le nouveau besoin fonctionnel 
sur l'ancien code source.

De cette manière, toute modification technique devient représentative des modifications fonctionnelles : 
une modification fonctionnelle mineure ne peut plus entraîner une refonte technique longue et coûteuse. 
Et a contrario, il est totalement légitime qu'une évolution fonctionnelle lourde nécessite une refonte 
tout autant importante du code source.

De manière générale, il est important de prendre conscience que le travail du développeur est de répondre à 
un besoin fonctionnel. Il est donc important de structurer son code, de quelque manière que ce soit, de 
façon à répondre au mieux et le plus facilement à ce besoin.

> Les impacts techniques doivent être à la mesure des changements fonctionnels

## Les tests unitaires sont indispensables

En informatique on adore les sigles ; connaissez-vous le VDD ? C'est un sigle à la mode qui désigne
le "var_dump driven development", ou autrement dit, le développement piloté par la fonction var_dump() de PHP.

Pourquoi est-ce une expression à la mode ? Tout simplement parce qu'elle désigne une pratique de développement 
vouée à l'extinction, qui consiste pour le développeur à afficher à l'écran les valeurs de certaines variables pour 
en vérifier le contenu manuellement. 

La raison pour laquelle cette pratique est vouée à disparaître est simple : lorsque l'on vérifie à la main 
une valeur d'une variable, c'est long, ennuyant, et on risque de faire des erreurs. 

En réalité, faire un var_dump() pour afficher une information à l'écran n'est ni plus ni moins qu'un contrôle sur 
l'état d'une variable. Or il est très facile d'imaginer des robots dont la seule fonction est de s'assurer en permanence 
de l'état d'une variable. Et ces robots, infatiguables et extrêmement rapides, ne se tromperont jamais.

Ces robots, ce sont les tests unitaires automatisés. Ecrire des tests unitaires est, à ce jour, l'un des seuls moyens vraiment efficaces 
pour faciliter la vie d'un développeur, fiabiliser son code et réduire les coûts de maintenance d'une application.

Il existe dans tous les langages de programmation des outils qui facilitent la rédaction de tests unitaires : JUnit en Java, QUnit en JavaScript...
En PHP, deux outils se démarquent par leur fiabilité et leur grande communauté: [PHPUnit](http://phpunit.de), développé par Sebastian Bergmann, 
et [atoum][http://docs.atoum.org], développé par Frédéric Hardy. Ces deux outils proposent une approche légèrement différente des tests unitaires, 
mais sont tous les deux simples, efficaces et complets.

De manière générale, rédiger un test unitaire consiste à dérouler un processus simple :

+ mettre en condition un code que l'on souhaite tester : ce sont les conditions du test
+ exécuter une commande : il s'agit de ce que l'on souhaite tester
+ décrire quel doit être l'effet de la commande : il s'agit alors du résultat attendu

Lors que l'on affiche une variable à l'écran pour vérifier sa valeur, finalement ce que l'on fait c'est exiger que 
cette variable ait telle ou telle valeur. En d'autres termes, on fait une assertion sur la valeur de la variable.

On peut imaginer, par exemple, créer un test unitaire automatisé pour une fonction d'addition, qui utiliserait la 
fonction PHP native `assert()` :

    [php]
    $resultat = addition(5,5);
    assert($resultat === 10);

Ce code PHP génerera une erreur si la fonction addition() ne renvoie pas "10".

L'avantage d'utiliser un outil de tests automatisés sera de pouvoir organiser ses tests facilement et efficacement. Chaque 
assertion valide sera afficée en vert, les erreurs seront remontées aux développeur en un instant. 

> Les tests unitaires sont le seul moyen efficaces de fiabiliser un code source

## Tester le besoin métier est indispensable

Tester son code source est bien. C'est même déjà pas mal du tout ! Mais une application web est plus qu'un code source : c'est 
un assemblage de comportements (code source) vis-à-vis d'informations (données, souvent issues d'une base de données), pour 
délivrer un message (par exemple une page web), le tout exécuté dans un environnement spécifique (serveur), le tout 
vis-à-vis d'un utilisateur (navigateur).

On comprend vite du coup que ce n'est pas parce qu'un code source fonctionne de la manière attendue que l'application, elle aussi, 
aura le comportement désiré.

Bien plus, il est en réalité difficile de tester unitairement l'ensemble d'un code source : c'est long et coûteux, et cela 
exige des changements importants dans les pratiques de développement (Développement piloté par les tests, génération de 
tests aléatoires, etc.).

C'est pour ces raisons qu'il existe différents niveaux de tests automatisés : 

+ les *tests d'intégration*, qui s'assurent de l'environnement donné (version de PHP, présence d'un module apache...)
+ les *tests de charge*, qui s'assurent de la performance d'une application
+ les *tests unitaires*, qui vérifient le comportement du code source
+ les *tests d'interface*, qui vérifient qu'une application a bien l'aspect attendu
+ les *tests fonctionnels*, qui s'assurent que l'application délivre bien un service métier à l'utilisateur

Il existe bien entendu d'autres niveaux de tests (sécurité, cohérence...), mais ceux-ci sont les plus 
répandus.

Le dernier niveau de test (le test fonctionnel) est sans doute parmi les plus importants. Finalement, tout votre travail 
consiste justement à faire en sorte que l'utilisateur tire un bénéfice fonctionnel d'une application. S'assurer que 
ce bénéfice fonctionnel est bien présent est indispensable pour votre client.

Mais justement, ce contrôle, autrement dit ce travail de Recette client, est long et laborieux. Il est source 
d'erreurs, et surtout il est impossible de contrôler à la main, lors de chaque évolution fonctionnelle, que 
l'ensemble de l'application fonctionne toujours de la manière attendue.

C'est pour cela qu'il existe désormais des outils de tests fonctionnels automatisés. C'est outils sont 
infatiguables: ils traquent en permanence votre application à la recherche d'anomalies fonctionnelles.