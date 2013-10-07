# Exprimez votre besoin


## Tous la même grammaire

Les spécifications classiques, on l'a vu, échouent souvent : elles
acceptent mal le changement, sont souvent lourdes et ne concernent pas
toujours que le besoin métier.


Pour remédier à cela, il est possible d'utiliser une autre approche,
celle préconisée par le Développement Piloté par le Comportement.


"Quel terme barbare !" me direz-vous. C'est vrai. Mais, en fait, rien de
technique ou de geek derrière ce terme. C'est simplement une démarche
qui consiste à communiquer avec vos développeurs d'une autre manière, de
sorte qu'il vous comprennent.


C'est cette démarche que je vous propose de découvrir à partir de
maintenant.


Cette approche a au moins trois avantages :

+ L'écrit sert à la fois à spécifier et à échanger avec les équipes
+ Le changement est mieux géré
+ Vous savez à tout instant ce qui est fait et ce qu'il reste à faire


Pour au moins un inconvénient :

+ L'investissement et la disponibilité du Propriétaire du Produit doivent
être importants


Cette approche se base sur la rédaction des spécifications sous un
formalisme différent, dénommé généralement Gherkin. Ce formalisme n'est
pas obligatoire, mais il a été prouvé à plusieurs reprises qu'il est
pratique et répond bien aux problématiques que vous rencontrerez
probablement.


Si vous souhaitez faire du Développement Piloté par le Comportement
(Behaviour Driven Development en anglais, ou BDD), chaque acteur de
votre projet doit apprendre à lire et à comprendre ce formalisme. Pas de
panique, le coût d'apprentissage est quasi nul ; c'est justement l'un
des nombreux atouts de Gherkin.


## Décrivez vos Fonctionnalités métiers

Vous avez votre Vision du Produit, il est temps d'expliquer ce que vous
attendez aux développeurs.


Rien de plus simple : commencez par identifier les aspects fonctionnels
que vous souhaitez qu'ils développent, et ce en les décrivant avec une
courte phrase. Juste une phrase, courte, pas plus. Cette phrase sera le
titre de votre fonctionnalité.


**Chaque fonctionnalité doit pouvoir se distinguer des autres par son
titre**. Prenez les fonctionnalités suivantes :

    [gherkin]
    # Distinguez vos fonctionnalités en leur donnant un titre :
    Fonctionnalité : Retirer de l’argent au distributeur
    Fonctionnalité : Consulter le solde de son compte au distributeur

Il est facile de les distinguer et de les comprendre non ?


Identifier clairement ces fonctionnalités est important : cela vous
permettra de les prioriser plus facilement.


Bon, vous allez me dire qu'une petite phrase de rien du tout ça lasse la
place à beaucoup d'ambiguïtés ; et vous aurez bien raison !


Vous le savez, un Produit n'est rien sans un utilisateur pour
l'utiliser. **Précisez donc qui est l'utilisateur de votre
fonctionnalité** :

    [gherkin]
    # Précisez qui est l’acteur bénéficiaire de votre fonctionnalité :
    En tant que rôle
    En tant que administrateur du site
    En tant que conseiller clientèle
    En tant que ...

Voilà une bonne chose de faite ! **Identifiez maintenant le Service
rendu par votre fonctionnalité**. Je conseille toujours de voir le
service comme ni plus ni moins ce que votre commercial entend lorsqu'il
dit "c'est génial, en tant que ... vous pouvez faire ça !". Par exemple
:

    [gherkin]
    # Identifiez le service délivré par votre fonctionnalité à l’utilisateur :
    Je peux un service rendu
    Je peux retirer de l’argent au distributeur
    Je peux connaître la quantité d’argent qu’il me reste sur mon compte
    Je peux ...

On commence déjà à y voir plus clair. **Il ne reste plus qu'à préciser
le bénéfice métier** de votre fonctionnalité. Car oui, une
fonctionnalité sans bénéfice pour l'utilisateur ne sert à rien. Si vous
ne voyez pas de bénéfice pour votre fonctionnalité, c'est sans doute qu'elle ne
mérite pas d'être développée. Décrivez ce bénéfice ainsi :

    [gherkin]
    # Décrivez le bénéfice fonctionnel obtenu par votre fonctionnalité :
    De telle sorte que bénéfice attendu
    De telle sorte que je puisse obtenir de l’argent même quand la banque est fermée
    De telle sorte que ...

Ce qui fait que **toute fonctionnalité peut être décrite, de manière
explicite, en quatre courtes lignes** :

    [gherkin]
    Fonctionnalité : Retirer de l’argent au distributeur
        En tant que client de la banque
        Je peux retirer de l’argent au distributeur
        De telle sorte que je puisse obtenir de l’argent même quand la banque est fermée

N'importe quel membre de votre équipe comprendra ce qu'est cette
fonctionnalité. N'oubliez de n'employer que le vocabulaire de votre
Langue Commune pour éviter toute ambiguïté.


## Des scénarios pour vos fonctionnalités

Un titre, un rôle, le service rendu et le bénéfice métier c'est déjà pas
mal. Mais en général ça ne va pas suffire : vous pouvez avoir envie
d'illustrer vos fonctionnalités.


C'est le rôle des scénarios. **Chaque fonctionnalité peut contenir un
ensemble de scénarios. Ces scénarios doivent tous être clairs, uniques
et explicites**. Ils représentent le comportement concret que vous
attendez pour votre fonctionnalité. Par exemple :

    [gherkin]
    # Un Scénario est une situation concrète de votre Fonctionnalité :
    Fonctionnalité : Retirer de l’argent au distributeur
    
    Scénario : Retirer de l’argent avec une carte valide
    Scénario : Retirer de l’argent avec une carte expirée
    Scénario : Retirer de l’argent sur un distributeur qui ne contient
      plus d’argent
    Scénario : ...

La réalisation (ou non) dans votre application de ces scénarios détermine l'avancée de votre
fonctionnalité.


Il est possible que ces scénarios évoluent : certains seront réalisés
tôt, d'autres seront réalisés plus tard ou seront modifiés... Dans tous
les cas, **jeter un oeil à l'avancée de ces scénarios vous permettra de
maîtriser l'avancée de votre fonctionnalité**.


Vous vous en doutez, il peut arriver qu'une simple phrase ne suffise pas
à décrire avec précision un scénario. Il sera alors temps d'y introduire
un contexte, un événement déclencheur ou un résultat attendu.


Le contexte du scénario est introduit par l'expression "étant donné". Il situe les
conditions d'existence de votre scénario :

    [gherkin]
    # Un Scénario s’inscrit dans un contexte, une situation initiale :
    Scénario : ...
        Etant donné que tel contexte existe
        Etant donné que que je n’ai plus d’argent sur mon compte
        Etant donné que ma carte est expirée
        Etant donné que ...

Toute interaction de l'utilisateur avec votre produit peut se traduire
sous forme d'événement. Un événement est le déclencheur de votre
scénario, qui vient juste après que le contexte initiale soit situé :

    [gherkin]
    # Un Scénario se démarque par un événement :
    Scénario : ...
        ...
        Quand tel événement déclencheur survient
        Quand je demande à retirer de l’argent
        Quand je demande le solde de mon compte
        Quand ...

Si chaque action a une conséquence, il en va de même pour les événements : 
chaque événement attend un résultat, exprimé par l'expression "alors" :

    [gherkin]
    # Chaque Scénario aboutit à un résultat :
    Scénario : ...
        ...
        Alors tel résultat est attendu
        Alors je reçois 20 euros
        Alors ma carte est aspirée
        Alors ...

Toute cette syntaxe, qui permet de rendre vos fonctionnalités
explicites, sera comprise par vos développeurs. Et en plus elle n'est
pas très compliquée, si ?


Au final, voici une fonctionnalité plus complète :

    [gherkin]
    # Toute personne qui connaître votre Langue Commune comprend
    # votre fonctionnalité :
    
    Fonctionnalité : Retirer de l’argent au distributeur
        En tant que client de la banque
        Je peux retirer de l’argent au distributeur
        De telle sorte que je puisse obtenir de l’argent
          même quand la banque est fermée
    
    Scénario : Retirer de l’argent avec une carte valide et un
      compte approvisionné
        Etant donné que je détiens un compte bancaire soldé de 100 euros
        Et que ma carte expire l’an prochain
        Quand je demande à retirer 20 euros
        Alors je reçois 20 euros
        Et ma carte m’est restituée
    
    Scénario : Retirer de l’argent avec une carte expirée
        Etant donné que ma carte est expirée
        Quand je demande à retirer 20 euros
        Alors ma carte est aspirée


## Illustrez le tout avec des exemples

Ce formalisme est utile. Il n'est en aucun cas obligatoire bien entendu,
mais il a démontré son efficacité, et a l'énorme avantage d'être **clair
tout autant pour les fonctionnels que pour les développeurs.**


Maintenant, ce formalisme peut manquer de concret. Qu'à cela ne tienne,
vous pouvez y ajouter vos exemples. Car on est bien d'accords, **rien de
mieux que des exemples**. Prenons celui-ci :

    [gherkin]
    # Votre fonctionnalité peut contenir un exemple :

    Scénario : Retirer de l’argent avec un compte approvisionné
        Etant donné que ma carte expire l’an prochain
        Quand je demande à retirer "20" euros
        Alors je reçois "20" euros

Bon, c'est simple, mais limité, car cela nous oblige à recopier le
scénario autant de fois que nous avons d'exemples. Heureusement, notre
grammaire est riche, et vous pouvez utiliser la syntaxe suivante (votre
scénario devient alors un Plan du scénario) : 

    [gherkin]
    # Votre fonctionnalité peut contenir plusieurs exemples facilement :

    Plan du Scénario : Retirer de l’argent avec un compte approvisionné
        Etant donné que ma carte expire l’an prochain
        Quand je demande à retirer "<montant-demandé>" euros
        Alors je reçois "<montant-reçu>" euros
    
    Exemples :
        | montant-demandé   | montant-reçu  |
        | 20                | 20            |
        | 1000              | 1000          |
        | 50                | 50            |

Pratique non ? Les symboles < et \> indiquent une valeur d'exemple, à
rechercher dans le tableau juste en dessous.

Le tableau est dessiné grâce au caractère "pipe". Si vous ne savez pas le situer 
sur votre clavier, il suffit de taper `Alt Gr" + 6`, ou `Alt + Maj + L` si vous 
utilisez un Mac. Rassurez-vous, comme vous le verrez par la suite, il existe des 
outils graphiques pour vous faciliter la vie.

## Travaillez en équipe

Vous êtes maître de votre Produit, et c'est vous qui décidez de vos
Fonctionnalités. Par contre, n'oubliez pas que **le dialogue et
l'échange sont primordiaux pour éviter les ambiguïtés dans votre
projet**.


Lorsque vous allez rédiger vos scénarios, voici ce qui risque de se
passer :

+ Vous allez devoir les expliquer à votre équipe technique lorsque vous les leur fournirez
+ Vous n'allez peut être pas penser à tout, et vos équipes devront vous solliciter très (trop) régulièrement pour des aspects que vous aurez oublié
+ Certains de vos scénarios ne seront pas clairs ou n'emploieront pas le vocabulaire de la Langue Commune (on a toujours tendance à se trouver soi-même très clair ; ce n'est pas toujours vrai...)
+ L'équipe risque de ne pas se sentir suffisamment impliquée


Ces écueils ne sont bien sûr systématiques, mais le risque est là.
Pourquoi prendre ce risque alors que la solution est simple ?


Il suffit en effet de faire participer toutes les équipes lors de la
rédaction des scénarios. Attention, il ne s'agit pas de laisser
n'importe qui faire n'importe quoi... Non, **c'est a vous d'expliquer
votre besoin**, dans la Langue Commune, et **ensuite vous allez ensemble
le traduire en scénarios**, de telle sorte que tous comprennent
clairement ce que vous attendez d'eux.


Pour résumer, écoutez les remarques de vos équipes lors de la rédaction
des scénarios. Idéalement, vous devriez entendre des "oui mais qu'est-ce
qui se passe si..." et "je ne comprend ce que devient...". Si c'est le
cas, le pari est réussi !


Dans tous les cas, gardez vos scénarios clairs et concis. Personne n'a
envie de lire des scénarios de 20 ou 30 lignes...


> "Ecrivez vos scénarios avec l'équipe technique ; le temps passé sera
compensé par le gain de compréhension et l'inutilité d'avoir à faire des
va-et-vient incessants au cours du développement"


## Des assistants visuels

Vous aviez déjà le vocabulaire (la Langue Commune), vous voici désormais
avec la grammaire nécessaire pour exprimer votre besoin. Mais on est
tous les mêmes : personne n'aime pas la grammaire !


Après tout, si votre véritable objectif est de vous focaliser sur
l'expression de votre besoin, sur la description du comportement de
votre Produit, à quoi bon se concentrer sur la manière de l'exprimer ?


Pour vous simplifier la vie il existe un certain nombre d'outils qui
vont vous assister dans la  rédaction de vos Fonctionnalités. Ils vont
vous permettre de rédiger vos features selon une approche plus agréable,
tandis même qu'ils vont présenter à vos développeurs votre besoin sous
forme de fichiers clairs qu'ils pourront directement exploiter.

![ Il existe des assistants visuels pour faciliter votre expression de besoin ](behat-wizard-edit.jpg)

Il existe plusieurs outils, qui ont tous leurs avantages. On peut penser
par exemple à BehatViewer, ou à BehatWizard... N'hésitez pas à demander
vos équipes techniques de vous installer ces outils.


