# Optimisez vos tests fonctionnels

## Organisez vos Contextes de tests

Vous l'avez vu, l'ensemble des définitions (traduction d'une fonctionnalité) est stocké au sein 
d'une classe PHP, ici `FeatureContext`.

Vous aurez très rapidement besoin d'organiser vos définitions de manière à vous y retrouver facilement. En effet, vous aurez 
de plus en plus de définitions au fur-et-à-mesure que votre application grossira.

Il est très simple avec Behat de créer plusieurs Contextes de définitions. Par exemple, pour une application bancaire, on pourrait créer 
les contextes de définition suivants :

+ Contexte de définitions pour les clients
+ Contexte de définitions pour les banquiers
+ Contexte de définitions pour les comptes bancaires
+ etc.

Pour cela, il suffit simplement de créer un fichier PHP (et une classe) par Contexte de définition, par exemple :

+ `ClientContext.php`
+ `BanquierContext.php`
+ `CompteContext.php`

Puis de les initialiser dans le fichier principal `FeatureContext` :

    [php]
    class FeatureContext extends BehatContext
    {
        public function __construct(array $parameters) {
            $this->useContext('client', new ClientContext($parameters));
            $this->useContext('banquier', new BanquierContext($parameters));
            $this->useContext('compte', new CompteContext($parameters));
        }
    }

Désormais Behat utilisera l'ensemble des contextes disponibles pour traduire les expressions qu'il reçoit.

Remarquez les alias de Contexte que nous avons utilisés, par exemple "mink" dans le code suivant :

    [php]
    $this->useContext('mink', new MinkContext($parameters));

Ces alias vont vous permettre de récupérer facilement vos Contextes d'un Contexte à l'autre. Par exemple 
pour récupérer votre contexte `mink` depuis le contexte `client`, il vous suffit d'utiliser le code suivant :

    [php]
    $mink = $this->getMainContext()->getSubContext('mink');
    $session = $mink->getSession();

N'hésitez pas à découper vos Contextes de manière à les rendre facilement réutilisables par la suite. Cela vous évitera de mauvaises surprises 
lorsque votre application grossira et que vous aurez à naviguer parmi plusieurs centaines de définitions.

> Découpez et organisez vos Contextes de définition

## Créez une couche d'isolation de l'IHM

Une grande (très grande!) difficulté du développement piloté par le Comportement consiste à réussir 
à s'abstraire de l'interface graphique.

Imaginez par exemple la situation suivante : vous avez développé un site web pour votre client. Tout marche à merveille, 
vous avez de très nombreuses définitions pour vérifier avec Behat que le besoin est bien implémenté... 

Votre client est tellement content que désormais il souhaite disposer d'une application mobile, identique au site web. 
Vous voyez le problème ? Vous allez devoir récrire toutes les définitions où vous avez écrit des choses comme

    [php]
    return array(
        'Etant donné que je suis sur "/login"'
        ,'Quand je presse "Me connecter"
    );

Car oui, sur mobile, il est fort probable que la manière de vérifier si le besoin est bien implémenté change radicalement par rapport 
à un site web.

Et même sans un changement aussi radical que le passage d'un site web à une application mobile, comment gérer facilement les changements 
d'interface graphique : une `table` devient une `div`, le texte du bouton `Se connecter` devient `Connexion`... 

Le seul moyen d'éviter de devoir réécrire toutes ses définitions lorsqu'un changement d'interface survient (ce qui est le cas dans les 
deux précédents exemples) est de **créer des couches d'isolation de l'interface**.

Concrètement, c'est très simple : il suffit de créer des Contextes de définition spécialisé dans l'affichage des pages. On peut 
imaginer par exemple un Contexte de définition consacré à l'affichage des pages Web, qui contiendra donc nos définitions liées à l'affichage :

    [php]
    namespace View;
    class WebContext extends BehatContext {
        // (...)
    }

Et une autre, le jour où l'on en a besoin, consacré à l'affichage sur mobile :

    [php]
    namespace View;
    class MobileContext extends BehatContext {
        // (...)
    }

Désormais, il suffit d'ajouter un paramètre de configuration spécifique dans le fichier `behat.yml`, que l'on utilisera 
pour définir quel Contexte de définition l'on souhaite utiliser :

    [yaml]
    default:
        context:
          parameters:
            view: web # mobile | web ...

Une simple condition dans le constructeur du Contexte principal suffit à nous permettre d'utiliser le bon Contexte :

    [php]
    public function __construct(array $parameters)
    {
        if ($parameters['view'] == 'mobile') {
            $this->useContext('view', new View\MobileContext($parameters));
        } else {
            $this->useContext('view', new View\WebContext($parameters));
        }
    }


Pour les applications assez conséquentes, il devient rapidement intéressant de créer même un Contexte 
de définition par page (ou par lot fonctionnel) :

    [php]
    $this->useContext('view.catalogue', new View\Web\CatalogueContext($parameters));
    $this->useContext('view.panier', new View\Web\PanierContext($parameters));

Cette pratique, que l'on pourrait désigner par Isolation de l'IHM, peut sembler complexe, mais est 
en réalité le seul moyen efficace de péreniser les définitions au cours de la vie d'un projet.

N'hésitez pas à consulter la documentation de [BehatPageObjectExtension](https://github.com/sensiolabs/BehatPageObjectExtension), 
qui est une extension de Behat dédiée à cet usage.

> Isolez ce qui concerne l'interface graphique dans des Contextes spécifiques

## Exploitez les compte-rendus de tests

Vous avez désormais un outil pour valider et vérifier qu'un besoin métier a bel et bien été implémenté. Pour l'instant, 
vous avez exécuté vous-même Behat pour afficher ces informations dans un terminal. Vous vous en doutez, Behat va pouvoir 
en faire bien plus.

Pour commencer, il est tout à fait possible de générer une page web synthétique des résultats des tests. Pour cela, 
il suffit d'exécuter Behat en spécifiant le `html` comme format de sortie :

    [bash]
    php ./bin/behat --format html --out resultat.html

Vous disposerez donc d'une page web, graphique, que vous pouvez fournir à votre fonctionnel pour lui 
indiquer de manière claire ce qui est fait ou reste à faire parmi les spécifications.

![ Le résultat de chaque Fonctionnalité et Scénario est visible dans une page web ](screen-export-html.jpg)

Bien entendu, ce n'est pas tout. La majorité des outils de tests automatisés sont capables de générer des 
fichiers de sortie dans un format standard (xml). Ces fichiers peuvent donc être fournis à des logiciels tiers, 
capables de les traiter ou de les transformer selon leurs spécificité.

Il est par exemple tout à fait possible d'intégrer Behat à une plate-forme d'intégration continue, comme Jenkins. 
Une plate-forme d'intégration continue permet de délivrer en permanence du code source (déploiement continu), 
c'est-à-dire de permettre à un site web d'être mis à jour très régulièrement.

Indirectement, les plate-formes d'intégration continue (PIC) fournissent une vision d'ensemble sur un projet : les 
bonnes pratiques sont-elles respectées ? Le code est-il fiable ? Le besoin est-il respecté ? Behat va pouvoir se greffer 
au coeur de cette plate-forme et va ainsi fournir des indices précieux pour la pilotage du projet.

Pour générer un résultat réutilisable par une plate-forme d'intégration continue, il suffit d'exécuter la commande suivante :

    [bash]
    php ./bin/behat --format junit --out resultat.xml   

Pour aller plus loin avec les plate-formes d'intégration continue, je vous invite par exemple à consulter le site http://jenkins-php.org/.

Il existe également des outils visuels, basés sur ce format de sortie, pour fournir aux fonctionnels une vraie interface graphique 
pour rédiger les spécifications, mais aussi pour avoir une vue d'ensemble des résultats des implémentations de ces spécifications.

![ Des outils graphiques pour visualiser et éditer les spécifications ](behat-wizard-home.jpg)

Parmi ces outils, on peut mentionner par exemple [BehatWizard](http://halleck45.github.com/BehatWizardBundle/demo/behat/wizard/list.html) 
ou [BehatViewer](https://github.com/behat-viewer/BehatViewer).

Pour éviter de préciser manuellement quel format de sortie vous souhaitez utiliser lorsque vous lancez Behat, vous pouvez tout à 
fait les préciser une bonne fois pour toute dans votre configuration, c'est-à-dire dans le fichier `behat.yml` :

    [yaml]
    default:
        formatter:
          name:                   pretty,junit,html
          parameters:
            output_path:          null,junit,report.html

Cette configation vous permet par exemple d'afficher le résultat dans votre terminal, mais aussi générer des fichiers 
de compte-rendu en HTML et en XML.

> Multiplier les formats de sortie de compte-rendus permet de multiplier les usages

