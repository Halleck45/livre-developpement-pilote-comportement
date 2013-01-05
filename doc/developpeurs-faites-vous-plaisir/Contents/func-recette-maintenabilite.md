# Recette et maintenabilité


## Chaque recette fonctionnelle peut être automatisée

La démarche et la syntaxe du Développement piloté a l'avantage d'être
claire et de faciliter la communication.


J'ai une excellente nouvelle pour vous : **cette syntaxe va vous
permettre d'automatiser la recette de votre application**. Vraiment !


Et je nous vous parle pas d'outils, si vous en avez déjà utilisé, qui
vont enregistrer puis reproduire les click et saisies que vous faites
sur une page web (Selenium...). Non, surtout pas ! Je vous parle
d'outils pour lesquels vous n'avez rien à faire...


Après tout, **si vous avez fait l'effort de respecter une syntaxe
spécifique pour décrire vos Scénario, un programme informatique doit
être capable de les interpréter**.


Il existe aujourd'hui deux principaux outils sur le marché : Cucumber et
Behat. Le choix de l'outil dépendra du socle technique choisi par vos
développeurs pour votre Projet.


Concrètement, ces outils vont transformer vos Scénarios en code
informatique. Ce code informatique sera adapté par vos développeurs qui
lui donneront un sens et s'assureront qu'il signifie bien ce que vous
avez demandé.


Par la suite vous disposerez d'une interface où chaque fonctionnalité,
chaque scénario et chaque étape seront, soit verte (terminée), soit
jaune (pas encore développé), soit rouge (ne répond pas au besoin
initial).


![ Retrouvez toutes vos fonctionnalités au sein d'une même interface. ](behat-wizard-home.jpg)

Utiliser de tels outils a donc un coût puisqu'il exige un travail de
traduction par l'équipe technique. Ce coût peut être important, cela
peut demander du temps non négligeable, en plus d'une montée en
compétence des équipes techniques (très rapide en général),


Cependant, en utilisant ces outils :

+ Les équipes techniques savent si oui ou non elles ont répondu au besoin
+ Les équipes techniques sont rassurées dans leur travail
+ Vous connaissez exactement l'avancée de votre projet
+ Vous pouvez sans risque gérer votre besoin de changement : si une autre fonctionnalité est "cassée" par ce changement, vous le savez tout de suite et pouvez agir en conséquent
+ Vous augmentez le niveau de qualité de votre projet. C'est autant de gains lors de la vie du Produit et de sa maintenance, qui sans cela peut être très coûteuse
+ Vous-même savez en un instant si le livrable est conforme à vos attentes


Rien ne vous oblige à utiliser ces composants d'automatisation de
recette. Ne pas les utiliser peut certes être dommage, mais n'enlève
rien à la démarche générale.


>Facilitez le changement en automatisant la recette fonctionnel

## Assurez-vous que les équipes techniques automatisent leurs recettes techniques

Tester automatiquement qu'un livrable correspond à vos **attentes**
fonctionnelles c'est bien ; mais être certain de la **fiabilité** de
votre produit c'est encore mieux !


La pratique du développement piloté par le comportement, c'est-à-dire de
tout ce que l'on a présenté ici (les fonctionnalités, les scénarios...)
nécessite quelque chose de fondamental : que **vos équipes mobilisent
leur énergie sur votre besoin, pas sur leurs bugs**.


Pour cela il n'y a pas énormément de solutions : **vos équipes
techniques doivent créer des tests automatisés de leur code source**. 


C'est un impératif fort : c'est le seul moyen pour qu'elles ne vous
disent pas "nous ne pouvons pas développer cette fonctionnalité, c'est
impossible car on risque de casser tout l'existant et d'introduire des
bugs". Non, avec des tests automatisés, une équipe compétente ne peut
pas vous répondre ça. Elle vous dira peut-être que la nouvelle
fonctionnalité a un coût, mais aucune fonctionnalité ne doit être
impossible à implémenter parce qu'elle risque de casser l'existant.


Si quelque chose casse lorsque vous introduisez du changement, vous
perdrez beaucoup d'argent : il va falloir identifier (et ça peut être
très long!) et corriger ce qui a cassé, corriger la nouvelle
fonctionnalité, la tester, ajouter des ressources pour le support
applicatif... 


Tous ces coûts, très importants sur le long terme, peuvent être réduits
en laissant à court terme une plus grande marge de manœuvre aux équipes
techniques pour fiabiliser leur code source et en écrivant des tests
automatisés. Ne soyez pas un mauvais économe : laissez leur le temps
qu'il faut pour vous fournir un produit plus stable et plus fiable. Vous
y  gagnerez sur le long terme.


> Laissez le temps aux développeurs de fiabiliser leur code : aucune
fonctionnalité ne doit être impossible à implémenter sous prétexte
qu'elle risque de casser l'existant
