# Comprenez (enfin!) ce que votre client vous demande


Vous l'avez compris : votre client ne parle pas la même langue que vous. Comment comprendre ce qu'il vous demande dans ce cas ?
Certains vous répondront : "utilisez UML". D'autres vous diront qu'il faut demander à
votre client de s'exprimer sous forme de "si...alors". J'imagine que cela doit être possible
dans certain cas. Mais dans la majorité des situations, exiger de vos clients qu'ils apprennent l'UML, 
ou leur demander de formaliser leur pensée sous forme de logique booléenne, est un contre-sens ! 
Ils n'y arriveront pas efficacement : ce n'est pas leur métier, et probablement pas leur mode de pensée.

C'est pour cela qu'il existe ce que l'on appelle le "Développement piloté par le Comportement" 
("Behavior Driven Development", ou "BDD" en anglais). Ce qui se cache derrière ce terme barbare est 
en réalité très simple : il s'agit d'une démarche pour faciliter la communication entre fonctionnels
 et techniques, démarche soutenue par l'utilisation d'une langue et d'une grammaire commune.

Cette grammaire commune (appelée "Gherkin") est simple, facile à apprendre, et compréhensible tout 
autant par un informaticien que par votre voisin, votre coinjoint(e)..., et par là-même par votre client.

Utiliser cette grammaire vous offrira un certain nombre d'avantages :

+ vous éviterez les quiproquos
+ les spécifications vous serviront de documentation
+ il sera plus facile d'identifier et de gérer les changements fonctionnels de dernière minute
+ vous saurez à tout moment ce qu'il vous reste à faire

Attention, le Développement piloté par le comporement a un pré-requis majeur et non
négociable : *le client doit s'investir*. Il faudra que le client (ou son représentant fonctionnel) 
soit disponible pour répondre à chacune de vos questions, en permanence. Si personne autour de vous 
n'est prêt à s'investir pour exprimer le besoin clairement, oubliez l'idée de pratiquer le Développement 
piloté par le comportement : vous n'aurez pas les moyens d'y parvenir.

> Gherkin est une grammaire accessible à tous pour structurer l'expression de besoin

## Le besoin doit être exprimé par des Fonctionnalités

Concrètement, comment démarrer ? La première tâche revient à votre client. C'est lui qui
va dévoir recencer la liste des Fonctionnalités qu'il souhaite voir dans son application.

Pour cela, il va devoir identifier chaque fonctionnalité par un titre simple, court, expli-
cite et unique :

    [code gherkin]
    # Le client va distinguer les fonctionnalités en leur donnant un titre 
    Fonctionnalité : Avoir accès à la liste des animaux de compagnie
    Fonctionnalité : Choisir un animal de compagnie
    Fonctionnalité : Acheter un animal de compagnie
    Fonctionnalité : ...

Il est impératif qu'à cette étape les titres des fonctionnalités soient clairs et explicites.

Ensuite, il va lui suffire de contextualiser un peu cette fonctionnalité. Par exemple, il devra 
indiquer qui est le bénéficiaire de cette fonctionnalité :


    [code gherkin]
    En tant que vendeur
    En tant que acheteur
    En tant que propriétaire de la boutique
    En tant que ...

Vous avez donc désormais connaissance de l'acteur bénéficiaire de la fonctionnalité. Il
reste encore à identifier le service offert par la fonctionnalité. C'est cela que vous aurez, finalement, à programmer :

    [code gherkin]
    Je veux acheter un animal de compagnie
    Je veux connaître la liste des animaux de compagnie de mon magasin
    Je veux connaître la liste des vendeurs
    Je veux ...

Pour lever toute ambiguïté, il est important pour vous de comprendre quel est le bénéfice
de la fonctionnalité que vous aller développer : à quoi sert-elle vraiment ?

Après tout, c'est indispensable de comprendre et de connaître le pourquoi de votre métier.
Lorsque vous travaillez, le fruit de votre travail est vraiment utile ; et il est toujours gratifiant 
de voir que l'on a rendu service à quelqu'un. C'est pour cela qu'il est important que le
client exprime le bénéfice obtenu par la fonctionnalité :

    [code gherkin]
    De telle sorte que je puisse repartir avec un nouveau compagnon
    De telle sorte que je sache s'il me faut renouveler le stock
    De telle sorte que je puisse organiser les congés de chacun
    De telle sorte que ...

Chaque fonctionnalité peut donc être exprimée en quatre lignes, très simples et compréhensibles par tous :


    [code gherkin]
    Fonctionnalité : Connaître la liste des animaux de compagnie disponibles
      En tant que vendeur
      Je veux connaître la liste des animaux de compagnie disponibles à la vente
      De telle sorte que je sache s'il me faut renouveler le stock

Pratique non ? Cette manière d'exprimer les fonctionnalités (dénommée "Gherkin"), est
simple, mais surtout est compréhensible par tous. C'est le rôle de votre client (ou fonctionnel) 
de recencer l'ensemble des fonctionnalités du projet de cette manière.
Mais, vous vous en doutez, cela ne suffit pas. Il va falloir maintenant comprendre en détail 
chaque fonctionnalité ; c'est un travail que vous ferez en commun



## Vous êtes un journaliste : interviewez !

Chaque Fonctionnalité peut donc être traduite en quatre petites phrases. Mais dans la majorité 
des cas cela ne suffira pas : il reste des ambiguïtés, ce n'est pas très précis...

C'est là que vous entrez en scène. A partir du moment où les fonctionnalités sont décrites
dans leurs grandes lignes, vous allez devoir les détailler. Oui, vous-même !
Comment ? Tout simplement en prenant quelques instants la casquette d'un journaliste :
interviewez votre fonctionnel !

Attention, je vous parle d'une vraie interview. Prenez le temps, pour chaque Fonctionnalité, 
de poser un maximum de questions pertinentes : "et qu'est-ce qui se passe quand ... ?" ;
ou encore "je ne comprend pas le bénéfice tiré de cette fonctionnalité pour l'utilisateur."

Posez, donc, toutes ces questions. Et surtout planifiez systématiquement un moment,
avant chaque itération, où votre fonctionnel et l'ensemble des équipes techniques (oui,
toutes lers personnes qui sont concernées), vont se réunir pour cette interview.

J'insiste : il ne s'agit pas d'une réunion facultative : comment ferait-on sans cela pour être
sûr d'avoir bien compris la demande du client ? Le temps passé n'est pas perdu, loin de là :
c'est du temps de gagné sur la future maintenance, les futures recettes et les futurs vas-et-vients 
entre le client et les équipes techniques.

L'objectif de ces interviews est de comprendre exactement, sans aucune ambiguïté possible, 
ce qui vous est demandé fonctionnellement. Pour cela, il faudra découper les prochaines 
Fonctionnalités que vous aurez à développer en "Scénarios".



## Chaque Fonctionnalité peut être découpé en Scénarios

Chaque Fonctionnalité peut être décrite en quatre points :

+ un titre
+ un rôle
+ un service rendu
+ un bénéfice métier


Mais cela ne suffira probablement pas. Il reste des questions en suspens. C'est le rôle des
Scénarios de lever ces incertitudes. Ce sera d'autant plus vrai qu'ils seront rédigés en commun 
avec le fonctionnel et les équipes techniques.

Chaque Fonctionnalité peut contenir un ou plusieurs Scénarios, et chaque Scénario doit
pouvoir se distinguer facilement des autres par un titre :

    [code gherkin]
    Fonctionnalité : acheter un animal de compagnie
      (...)
      Scénario : acheter un animal de compagnie disponible à la vente
      Scénario : tenter d'acheter un animal de compagnie qui est trop jeune
      Scénario : tenter d'acheter un animal de compagnie qui est réservé
      ...

Votre travail de développeur va consister à rendre ces Scénarios effectifs dans
l'application. En identifiant les Scénarios réalisés et ceux qui ne le sont pas vous saurez
donc exactement ce qu'il vous reste à faire.

Vous vous en doutez : un simple titre ne suffit pas ; il va falloir compléter chaque Scénario.
Posez par exemple cette question : "dans quel cadre ce scénario se situe t-il ?". Cela vous 
permettra de connaître le *Contexte de votre Scénario*:

    [code gherkin]
    Scénario : ...
      Etant donné que tel contexte existe
      Etant donné que je souhaite acheter un chien
      Etant donné qu'il n'y a plus un seul animal disponible
      Etant donné ...

Une application consiste généralement à réagir à des *événements*. Précisez-donc quels
sont ces événéments :

    [code gherkin]
    Scénario : ...
      (...)
      Quand j'essaye d'acheter un chien
      Quand je regarde quels sont les animaux disponibles
      Quand ...



Dès lors, il ne vous reste plus qu'à préciser le *Résultat attendu* lorsque cet événement
survient :

    [code gherkin]
    Scénario : ...
      (...)
      Alors je peux repartir avec un petit chien
      Alors je dois être informé que le chien est trop jeune pour être
      vendu
      Alors ...


Cette syntaxe, simple, que l'on nomme Gherkin, va suffire dans la majorité des cas pour
que le fonctionnel puisse exprimer son besoin et surtout pour que vous puissez comprendre 
sans ambiguïté ce qui vous ai demandé.

Voici un exemple de Fonctionnalité :

    [code gherkin]
    Fonctionnalité : acheter un chiot
    En tant que client du magasin
    Je veux pouvoir acheter un chiot
    De telle sorte que je puisse avoir un animal de compagnie
    
    Scénario : acheter un chiot disponible à la vente
      Etant donné qu'un chiot est disponible en stock
      Quand j'achète un chiot
      Alors on me donne un chiot
    
    Scénario: acheter un chiot sevré
      Etant donné qu'un chiot est trop jeune pour être vendu
      Quand j'essaye d'acheter ce chiot
      Alors je suis informé qu'il est trop jeune pour être vendu

Pratique non ?

## Demandez (exigez) des exemples précis

Rappelez-vous que l'objectif du Développement piloté par le comportement est de lever
toutes les ambiguïtés. Pourtant, dans le scénario suivant, il reste des questions :

    [code gherkin]
    Scénario:  acheter un chiot sevré
      Etant donné qu'un chiot est trop jeune pour être vendu
      Quand j'essaye d'acheter ce chiot
      Alors je suis informé qu'il est trop jeune pour être vendu
   
Ces questions sont nombreuses :

+ à partir de quel âge un chiot peut-il être vendu ?
+ quel est l'âge du chiot que l'on essaye d'acheter ?
+ comment suis-je informé ? par quelle phrase ?

Autant de questions auxquelles, si vous ne les posez pas maintenant, vous serez obligé de
répondre vous-même, avec tous les risques que cela comporte, notamment celui de devoir
recommencer son développement !

Heureusement, il est tout à fait possible, avec Gherkin, de préciser des valeurs à utiliser. Il
suffit de les encadrer dans des guillemets :

    [code gherkin]
    Scénario : acheter un chiot sevré
      Etant donné qu'un chiot ne peut être vendu avant qu'il n'ait "2 mois"
      Et que "Médor le chien" a actuellement "1 mois"
      Quand j'essaye d'acheter "Médor le chien"
      Alors on doit me dire "Médor le chient est trop jeune !"



Pas mal d'ambiguïtés sont levées non ? Et ça ne prend pas beaucoup plus de temps à écrire.
Maintenant, il peut arriver qu'un seul exemple ne suffise pas ; il en faudrait plusieurs. Pas
de problèmes, Gherkin vous permet d'utiliser des exemples facilement ; il suffit de les passer 
sous forme de tableaux. Les variables sont alors à encadrer par les symboles "<" (plus petit que) 
et ">" (plus grand que).

    [code gherkin]
    Plan de Scénario : acheter un chiot sevré
      Etant donné qu'un chiot ne peut être vendu avant qu'il n'ait "2 mois"
      Et que "<nom-du-chien>" a actuellement "<age-du-chien>"
      Quand j'essaye d'acheter "<nom-du-chien>"
      Alors on doit me dire "<nom-du-chien> est trop jeune !"
    
    Exemples :
      | nom-du-chien  | age-du-chien        |
      | médor       | 1 mois            |
      | rex         | 1 mois et 3 jours     |
      | rantanplan    | 15 jours            |

Remarquez qu'il ne s'agit plus dans ce cas d'un "Scénario" mais d'un "Plan de Scénario".

Vous voici donc avec un besoin simple, découpé et exprimé clairement. A vous de jouer !