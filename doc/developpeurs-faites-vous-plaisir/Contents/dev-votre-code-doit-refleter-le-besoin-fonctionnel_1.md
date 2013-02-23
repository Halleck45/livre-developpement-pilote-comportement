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

> Les impacts techniques doivent être à la mesure des changements fonctionnels

## Tout objet a une identité : le Pattern Entity

La première chose à faire si l'on souhaite concevoir une application souple et fonctionnellement 
évolutive, en plus, bien entendu, des démarches de communications et d'échange dont nous avons déjà parlé, 
sera d'identifier et de catégoriser les concepts que vous allez être amenés à manipuler.

Parmi ces concepts, il en existe un de très particulier : il s'agit des ressources qui, 
malgré leurs changements et évolutions réguliers, resteront en permanence identifiables et 
distinctes des autres ressources.

Prenez, par exemple, un compte bancaire. Dans une application financière, il existe de très 
nombreux comptes bancaires : ils sont créés, évoluent, et, pour certains, disparaissent. Pourtant, 
malgré leurs évolutions successives, chacun de ces comptes reste reconnaissable parmi tous. 
Il conservent le même propriétaire, et vous n'avez aucun mal à les identifier parmi l'ensemble 
des comptes existants.

C'est en réalité que ces comptes bancaires *ont une identité propre*. Ils possèdent, dans leur état, une 
propriété qui les rend uniques. Dans le cas d'un compte bancaire, cette propriété est simplement 
le numéro de compte. Pour une entreprise, c'est son Siret, pour une personne son numéro de 
sécurité sociale...

Traditionnellement, les développeurs identifient (au sens de rendre uniques) les ressources dans une base 
de données par un Identifiant (ID).

-> double emploi avec le fonctionnel [...]

## Coder une règle métier efficacement : le Pattern Specification
## Représenter une information : le Pattern Object-Value

## Manipulez les objets métiers : le Pattern Repository
## Superposez le besoin métier à votre code source : le pattern Service
