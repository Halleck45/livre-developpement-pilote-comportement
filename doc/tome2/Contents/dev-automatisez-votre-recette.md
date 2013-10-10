# Automatisez votre recette

Vous l'avez vu, Gherkin est un moyen simple pour votre fonctionnel pour vous fournir
une expression de besoin claire et précise.

Que diriez-vous maintenant de traduire ce besoin en tests automatisés ? Car, oui, c'est possible !

Tout besoin, s'il est exprimé clairement, peut être testé. Si ce besoin est exprimé dans une
grammaire solide et complète, tout système d'information peut le tester automatiquement.

Je vous propose de voir comment traduire ce besoin en tests automatisés, de telle sorte
qu'une simple ligne de commande dans un terminal vous indique si ce que vous avez développé est 
conforme ou non.

L'ensemble des exemples que nous allons voir est écrit en PHP. Pourquoi ? Tout simplement 
parce que la communauté PHP a très fortement et très rapidement adhéré au Développement 
piloté par le comportement. Les outils PHP pour le Développement piloté par
le comportement sont matures, nombreux, et surtout offrent une souplesse qui, à ce jour,
ne se retrouve pas dans d'autres langages.

Cependant, il existe des outils pour écrire et lancer des tests automatisés dans à 
peu près n'importe quel langage :

+ Ruby : Cucumber (http://cukes.info)
+ Java : JBehave (http://jbehave.org)
+ C# : NBehave (https://github.com/nbehave/NBehave)
+ Python : Behave (http://packages.python.org/behave/)
+ PHP : Behat (http://behat.org) , PHPSpec (http://www.phpspec.net)
+ JavaScript : Jasmine (http://pivotal.github.com/jasmine/)
+ Net : NBehave (http://nbehave.org)

Il en existe bien d'autres, la liste est loin d'être exhaustive, mais ceux-ci sont particulièrement 
utilisés dans le monde du développement aujourd'hui.




## Installez et utilisez Behat en PHP

Behat est un outil développé en PHP par Konstantin Kudryashov (@everzet) et soutenu
par la société KnpLabs. Cet outil efficace est aujourd'hui mature et largement utilisé.
Behat permet d'exécuter des tests automatisés sur tous types d'applications, PHP ou non.
Il peut tester :

+ des applications web, en pilotant un vrai navigateur ou non
+ des applications console (terminal)
+ des morceaux de code source

Du moment que ce qui est testé dispose d'un point d'entrée et d'une information de sortie,
Behat va vous permettre d'automatiser votre recette métier.

Dans tous les cas, il vous faudra PHP installé sur la machine qui exécutera les tests.
L'installation de PHP est résumée sur cette page : http://php.net/manual/fr/install.php.

Windows :

    [bash]
    Utilisez WampServer (http://www.wampserver.com)

Ubuntu, Debian :

    [bash]
    # en ligne de commande
    apt-get install php5-common php5-cli php5-curl

Mac :

    [bash]
    Utilisez MampServer (http://www.mamp.info)


Installer Behat est simple, et peut être fait de plusieurs manières : sous forme d'archive PHP
(phar) ou en utilisant un gestionnaire de dépendance.

### Installation avec Composer

Créez un fichier composer.json à la racine de votre projet, avec le contenu suivant :

    [json]
    {
        "require-dev": {
            "behat/behat": "2.5.*@stable"
        },
        "config": {
            "bin-dir": "bin/"
        }
    }


Exécutez ensuite la commande suivante dans votre terminal, en vous plaçant à la racine
de votre projet :

    [bash]
    curl http://getcomposer.org/installer | php
    php composer.phar install --prefer-source

### Installation sous forme d'archive Phar

Il vous suffit de télécharger le fichier behat.phar à l'adresse suivante : http://behat.org/
downloads/behat.phar. Placez-le ensuite dans un dossier `bin` à la racine de votre projet. Vous
aurez donc l'arborescence suivante :

    [bash]
    /chemin/vers/mon/projet
        (...)
        - bin/
            - behat.phar


Pour des raisons de maintenabilité, il est préférable d'utiliser la méthode d'installation
avec Composer, qui vous permettra de mettre à jour très facilement l'ensemble des librairies 
PHP que vous utilisez, y compris Behat.

### Préparer le projet

Une fois que Behat est installé, il vous suffit d'exécuter la commande suivante :

    [bash]
    php ./bin/behat --init

Cela aura pour effet de créer dans votre projet tous les éléments dont Behat a besoin pour
fonctionner.   

Vous voici désormais avec les dossiers suivants :

+ `features` : contient la listes des fonctionnalités, sous forme de fichier `.feature`
+ `features/bootstrap` : contient les fichiers PHP de traduction de Fonctionnalité en code source

Chaque nouvelle fonctionnalité devra donc être ajoutée dans le dossier `features`, sous forme d'un 
fichier `.feature`. Créez par exemple le fichier `features/calculer-mon-age.feature`, avec le 
contenu suivant :


![ Les sources de cet exercice sont disponibles sur http://goo.gl/lgWvy ]
(dev-qr-exo1-step1.png)


    [gherkin]
    # language: fr
    Fonctionnalité: Calculer l'âge d'une personne
        En tant qu'utilisateur de l'application
        Je veux connaître le nombre d'années écoulées entre deux dates
        De telle sorte que je puisse connaître mon age
        
    Scénario: Calculer l'âge d'une personne depuis une date antérieure à aujourd'hui
        Etant donné que je suis né le 06/07/1986
        Et que nous sommes le 20/09/2013
        Quand je calcule mon âge
        Alors je suis informé que j'ai 27 ans
        
    Scénario: Calculer l'âge d'une personne depuis une date postérieure à aujourd'hui
        Etant donné que je suis né le 06/07/3013
        Et que nous sommes le 20/09/2013
        Quand je calcule mon âge
        Alors je suis informé que je ne suis pas encore né
        
    Scénario: Calculer l'âge d'une personne dont c'est l'anniversaire aujourd'hui
        Etant donné que je suis né le 06/07/1986
        Et que nous sommes le 06/07/2013
        Quand je calcule mon âge
        Alors je suis informé que j'ai 27 ans
        Et on me souhaite un joyeux anniversaire

Puis exécutez la commande suivante pour lancer Behat :

    [bash]
    ./bin/behat

![ Les étapes de la fonctionnalité ne sont pas encore traduites : elles sont jaunes ]
(dev-behat-exo-date-undefined.jpg)

Comme vous pouvez le voir, le résultat de cette commande est jaune. Cela veut tout simplement dire 
que les phrases utilisées pour exprimer le besoin n'ont pas encore de signification pour Behat. Il va falloir 
les traduire.


## Traduire une Fonctionnalité en code source


Par chance, Behat est suffisamment bien fait pour vous fournir une base de travail pour traduire vos fonctionnalités. Il suffit 
de copier-coller dans le fichier `features/bootstrap/FeatureContext.php` le code PHP qui a été généré par la commande :

    [bash]
    ./bin/behat

Pourquoi copier ce code dans ce fichier ? Tout simplement parce que les fichiers `features/bootstrap/*Context.php` vont servir 
de passerelle entre le besoin exprimé (sous forme de phrases) et votre application. 

Il vous appartient de modifier ces fichiers pour faire la traduction du besoin fonctionnel. C'est un travail 
qui semble long, mais en réalité il n'est pas beaucoup plus long que de tester vous-même à main que la demande initiale 
est respectée, mais possède surtout l'avantage de rendre ce travail de recette interne totalement automatique.

Traduisez maintenant la fonctionnalité en code source dans le fichier `features/bootstrap/FeatureContext.php`, en adaptant 
le code PHP fourni par Behat selon votre besoin. Par exemple :

    [php]
    <?php
    // (...)
    
    class FeatureContext extends BehatContext
    {
    
        private $birthDate;
        private $today;
        private $output;
        
        /**
         * Initializes context.
         * Every scenario gets it's own context object.
         *
         * @param array $parameters context parameters (set them up through behat.yml)
         */
        public function __construct(array $parameters)
        {
        }
        
        /**
         * @Given /^que je suis né le (\d+)\/(\d+)\/(\d+)$/
         */
        public function queJeSuisNeLe($day, $month, $year)
        {
            $this->birthDate = new \DateTime(sprintf('%d-%d-%d', $year, $month, $day));
        }
        
        /**
         * @Given /^que nous sommes le (\d+)\/(\d+)\/(\d+)$/
         */
        public function queNousSommesLe($day, $month, $year)
        {
            $this->today = new \DateTime(sprintf('%d-%d-%d', $year, $month, $day));
        }
        
        /**
         * @When /^je calcule mon âge$/
         */
        public function jeCalculeMonAge()
        {
            $this->output = shell_exec(sprintf('php src/age.php --birthdate=%s --today=%s', $this->birthDate->format('Y-m-d'), $this->today->format('Y-m-d')));
        }
        
        /**
         * @Then /^je suis informé que j\'ai (\d+) ans$/
         */
        public function jeSuisInformeQueJAiAns($age)
        {
            if(!preg_match('!'.$age.' ans!', $this->output)) {
                throw new Exception();
            }
        }
        
        /**
         * @Then /^je suis informé que je ne suis pas encore né$/
         */
        public function jeSuisInformeQueJeNeSuisPasEncoreNe()
        {
            if(!preg_match('!Vous n\'êtes pas encore né!', $this->output)) {
                throw new Exception();
            }
        }
        
        /**
         * @Then /^on me souhaite un joyeux anniversaire$/
         */
        public function onMeSouhaiteUnJoyeuxAnniversaire()
        {
            if(!preg_match('!Joyeux anniversaire!', $this->output)) {
                throw new Exception();
            }
        }
    }

Notez ces différents aspects :

+ chaque phrase est convertie en une méthode PHP
+ le lien entre les phrases et les méthodes PHP est effectué par un expression régulière (par exemple : `/^on me souhaite un joyeux anniversaire$/`)
+ vous devez traduire chaque étape. Behat comprendra qu'une étape n'est pas valide si vous levez une exception, comme c'est le cas dans la méthode `onMeSouhaiteUnJoyeuxAnniversaire()`
+ il est possible de recevoir certaines informations (nombres, exemples...) sous forme de paramètres de méthodes.
+ l'application qui va être testée fonctionne en ligne de commande (`php src/age.php`)

Maintenant, il vous suffit de relancer Behat pour vérifier automatiquement que votre application a bien 
le comportement attendu :

    [bash]
    ./bin/behat

![ Les tests échouent ; c'est normal, les fonctionnalités n'ont pas encore été développées ]
(dev-behat-exo-date-fail.jpg)


Il vous suffit finalement de développer votre application PHP, conforme aux attentes fonctionnelles, puis de relancer 
Behat pour vous assurer que vous avez bien traité la demande initiale.


## Exploitez les jeux d'exemples

![ Les sources de cet exercice sont disponibles sur http://goo.gl/jaLxP ]
(dev-qr-exo1-step2.png)

Vous l'avez vu, votre fonctionnel peut vous fournir des exemples, grâce à la syntaxe Gherkin.

On pourrait par exemple modifier notre fonctionnalité de la façon suivante :

    [gherkin]
    # language: fr
    Fonctionnalité: Calculer l'âge d'une personne
        En tant qu'utilisateur de l'application
        Je veux connaître le nombre d'années écoulées entre deux dates
        De telle sorte que je puisse connaître mon age
        
        Plan du Scénario: Calculer l'âge d'une personne
          Etant donné que je suis né le "<dateNaissance>"
          Et que nous sommes le "<dateDuJour>"
          Quand je calcule mon âge
          Alors on me répond "<reponseAttendue>"
          
          Exemples:
            | dateNaissance | dateDuJour | reponseAttendue                          |
            | 06/07/1986    | 20/09/2013 | Vous avez 27 ans                         |
            | 06/07/1985    | 20/09/2013 | Vous avez 28 ans                         |
            | 26/11/2020    | 20/09/2013 | Vous n'êtes pas encore né                |
            | 06/07/1986    | 06/07/2013 | Vous avez 27 ans. Joyeux anniversaire    |


Vous voici avec une fonctionnalité simple, claire et complète.

Travailler avec des exemples ne change strictement rien pour vous. En réalité, Behat 
va travailler avec chaque jeu d'exemple, un à un, et va vous envoyer chaque information 
sous forme de paramètre de méthode, sans que cela n'ait le moindre impact sur votre code.

Voici une traduction possible de cette fonctionnalité :

    [php]
    <?php
    // file features/bootstrap/FeatureContext.php
    // (...)
    
    class FeatureContext extends BehatContext
    {
    
        private $birthDate;
        private $today;
        private $output;
        
        /**
         * @Given /^que je suis né le "([^"]*)"$/
         */
        public function queJeSuisNeLe($date)
        {
            $this->birthDate = DateTime::createFromFormat('d/m/Y', $date);
        }
        
        /**
         * @Given /^que nous sommes le "([^"]*)"$/
         */
        public function queNousSommesLe($date)
        {
            $this->today = DateTime::createFromFormat('d/m/Y', $date);
        }
        
        /**
         * @When /^je calcule mon âge$/
         */
        public function jeCalculeMonAge()
        {
            $this->output = shell_exec(sprintf('php src/age.php --birthdate=%s --today=%s', $this->birthDate->format('Y-m-d'), $this->today->format('Y-m-d')));
        }
        
        /**
         * @Then /^on me répond "([^"]*)"$/
         */
        public function onMeRepond($response)
        {
            if (false === strpos($this->output, $response)) {
                throw new Exception;
            }
        }
    }


Votre fonctionnel peut désormais ajouter autant d'exemples qui le souhaite, votre travail de traduction ne changera plus.

> Utiliser des exemples est très simple avec Behat.


## Réutilisez vos précédents tests

![ Les sources de cet exercice sont disponibles sur http://goo.gl/onk8x ]
(dev-qr-exo2.png)


Vous l'avez vu, la phase de traduction d'une phrase (étape) en code source peut parfois être longue.

C'est pour cela que vous devez faire en sorte de faciliter votre travail : réutilisez au maximum vos définitions. Attention, je ne 
vous parle en aucun cas ici de découper votre code comme vous le feriez dans une application (refactoring, respect des 
principes SOLID, découpage des méthodes...).

Non, la réutilisabilité dans les Contextes de traduction (fichiers `*Context.php`) ne passe pas par une réutilisation du code source, 
mais bel et bien par une réutilisation de phrases. **En réalité, vous devez apprendre à faire du refactoring de phrases**.

Cette tâche n'est pas simple au départ, mais est en réalité assez naturelle. Chaque phrase, qui correspond à un contexte, un 
élément déclencheur ou un résultat attendu, peut être découpée en différentes étapes, qui **elles-mêmes pourront être décomposées 
jusqu'à arriver à des expressions atomiques simples**.

Prenez par exemple la phrase suivante :

    [gherkin]
    Quand j'ajoute dans mon panier "télévision Sony"  depuis le catalogue produit

Cette phrase peut être découpée, par exemple :

    [gherkin]
    Etant donné que je consulte le catalogue produit
    Quand j'ajoute dans mon panier "télévision Sony"

Qui elles-mêmes peuvent être découpées pour créer des expressions atomiques réutilisables :

    [gherkin]
    Etant donné que je suis sur la page "/catalogue/produits"
    Quand je coche "télévision Sony"
    Et je clique sur "Ajouter au panier"

Vous pouvez constater que les trois dernières expressions sont en réalité des étapes (web) atomiques réutilisables à l'infini. 
Cela signifie qu'une fois que vous les aurez traduites, vous n'aurez plus jamais à revenir dessus, et vous pourrez vous en servir à votre gré. 

Vous pourriez désormais écrire, par exemple :

    [gherkin]
    Etant donné que je suis sur la page "/home"
    Etant donné que je suis sur la page "/mon-compte"
    ...
    
Cette démarche permet donc de faire du refactoring de phrases pour arriver à des expressions atomiques. Par chance, c'est 
extrêmement simple à faire avec Behat. Regardez plutôt :

    [php]
    <?php
    // (...)
    
    use Behat\Behat\Context\Step;
    
    class FeatureContext extends BehatContext
    {
    
        /**
         * @When /^j\'ajoute dans mon panier "([^"]*)"  depuis le catalogue produit$/
         */
        public function jAjouteDansMonPanierDepuisLeCatalogueProduit($produit)
        {
            return array(
                new Step\Given('que je consulte le catalogue produit')
                , new Step\When(sprintf('j\'ajoute dans mon panier "%s"', $produit))
            );
        }

Comme vous le voyez, il suffit simplement, dans la méthode de définition, de retourner un tableau d'étapes. Chaque 
étape est en réalité un objet PHP (`Given`, `When` ou `Then`) qui accepte en paramètre de constructeur la phrase que l'on souhaite 
utiliser. Pratique non ?

Ce qui donnerait par exemple dans notre cas :

    [php]
    <?php
    // (...)
    
    use Behat\Behat\Context\Step;
    
    class FeatureContext extends BehatContext
    {
    
        /**
         * @When /^j\'ajoute dans mon panier "([^"]*)"  depuis le catalogue produit$/
         */
        public function jAjouteDansMonPanierDepuisLeCatalogueProduit($produit)
        {
            return array(
                new Step\Given('que je consulte le catalogue produit')
                , new Step\When(sprintf('j\'ajoute dans mon panier "%s"', $produit))
            );
        }
        
        /**
         * @Given /^que je consulte le catalogue produit$/
         */
        public function queJeConsulteLeCatalogueProduit()
        {
            return array(
                new Step\Given('que je suis sur la page "/catalogue/produits"')
            );
        }
        
        /**
         * @When /^j\'ajoute dans mon panier "([^"]*)"$/
         */
        public function jAjouteDansMonPanier($produit)
        {
            return array(
                new Step\When(sprintf('je coche "%s"', $produit))
                , new Step\When('je clique sur "Ajouter au panier"')
            );
        }
        
        /**
         * @Given /^que je suis sur la page "([^"]*)"$/
         */
        public function queJeSuisSurLaPage($url)
        {
            // ?
        }
        
        /**
         * @When /^je coche "([^"]*)"$/
         */
        public function jeCoche($produit)
        {
            // ?
        }
        
        /**
         * @When /^je clique sur "([^"]*)"$/
         */
        public function jeCliqueSur($bouton)
        {
            // ?
        }
    }


Notez les trois dernières traduction (les méthodes  ̀ queJeSuisSurLaPage()`, `jeCoche()` et `jeCliqueSur()` ). Ces méthodes 
de traduction concernent le comportement d'un navigateur web, comme Firefox ou Chrome... Il arrive en effet très souvent 
qu'il faille effectuer des actions qui n'existent que dans un navigateur. Vous allez voir qu'avec Behat c'est très facile !

> Oubliez le refactoring de code. Vous devez apprendre à faire du refactoring de phrases.

## Testez une application Web

Vous savez désormais automatiser une recette fonctionnelle d'un applicatif. Cependant, dans le cadre d'une application internet, 
la recette fonctionnelle consiste le plus souvent à parcourir des pages web, vérifier leur conformité (délivrent-elles le service 
attendu ?), en soumettant un formulaire, en cliquant sur un lien... Bref, en surfant sur un site web.

Heureusement, il est aujourd'hui très simple d'automatiser une recette fonctionnelle, quand bien même elle concerne une 
application web. Allons-y !

Une fois de plus nous allons utiliser Behat. Plus précisément, nous allons utiliser une extension de Behat, appelée Mink, qui permet de 
naviguer dans une application web et de lancer des tests automatisés lors de cette navigation.

Concrètement, Mink permet une double approche :

+ Exécuter des tests fonctionnels dans un navigateur virtuel (émulateur)
+ Exécuter des tests fonctionnels au sein d'un vrai navigateur, comme Chrome ou Firefox par exemple

Il peut sembler étrange d'exécuter des tests dans un navigateur virtuel. Cependant, bien souvent le comportement fonctionnel 
que l'on souhaite tester est en réalité disponible directement au sein des pages web, dans le HTML. *On préférera alors privilégier 
les tests lancés dans un vrai navigateur uniquement lorsque le test porte sur des fonctionnalités complexes (en Javascript ou en Ajax 
par exemple), ces tests étant en général beaucoup plus lourd et long à s'exécuter que les autres*.

Pour commencer, installez `Mink` :

### Installer Mink avec Composer

Modifiez le fichier composer.json que vous avez préalablement créé, et ajoutez-y le code suivant :

    [json]
    "require-dev": {
        "behat/behat": "2.5.*@stable",
        "behat/mink": "1.4.*@stable",
        "behat/mink-extension": "~1.3",
        "behat/mink-goutte-driver": "~1.1"
    }

Puis relancez Composer :

    [bash]
    php composer.phar update

### Installer Mink sous forme d'archive Phar

Exécutez la commande suivante :

    [bash]
    wget https://github.com/downloads/Behat/Mink/mink.phar

Il vous suffit ensuite d'ajouter l'instruction suivantes dans vos fichiers PHP de Contexte :

    [php]
    require_once 'mink.phar';

### Configurer Mink

Maintenant, il suffit de configurer Behat pour lui indiquer que l'on utilise Mink. Mink étant une extension de Behat, C'est très simple. 
Il suffit de créer le fichier `behat.yml` à la racine de votre projet :

    [yaml]
    default:
      extensions:
        Behat\MinkExtension\Extension:
          base_url: http://url-de-votre-site.fr
          goutte: ~

Voilà, Mink est installé !

Rappelez-vous, le travail de Spécification fonctionnelle passe par l'utilisation d'expressions, elles-mêmes organisés autour 
d'une grammaire : Gherkin. Mink a ceci d'intéressant qu'il propose nativement un panel assez large d'expressions qui concernent 
un navigateur, que vous pouvez donc réutiliser (`comme "Quand je vais sur "http://..."`, ou encore `Quand je remplis "Votre prénom" avec 
"Jean-François"`...). 

Un besoin exprimé de la façon suivante sera donc automatiquement compris par Behat lorsque Mink est installé:

    [gherkin]
    Scenario: s'identifier au sein de l'application
      Etant donné que je suis sur "/accueil"
      Quand je suis le lien "Me connecter"
      Et que je remplis "Identifiant" avec "Jean-François"
      Et que je remplis "Mot de passe" avec "azerty"
      Et que je clique sur "Valider"
      Alors je dois voir "Bienvenue Jean-François"

Pratique non ? Constatez que vous n'avez même pas besoin de parler de nom (attribut `name`) des champs HTML ; non, Behat 
fera le lien pour vous entre les `label`, `title`, `class`... de vos champs de formulaire et l'expression que vous avez utilisée.

Au fait, avez-vous lancé Behat pour vérifier que cela fonctionne ? Allez-y ; Utiliser Mink ne change rien à l'utilisation de Behat, 
il suffit comme avant d'exécuter la commande suivante :

    [bash]
    php ./bin/behat

Bien entendu, il va souvent arriver que vous ayez besoin de gérer des cas plus complexes que ce qu'il est possible de faire avec
les expressions disponibles nativement dans Mink. 

Dans ce cas, il va s'agir, ni plus ni moins, de piloter votre navigateur (quel qu'il soit). Un très large éventail 
de méthodes sont disponibles :

    [php]
    <?php
    class FeatureContext extends MinkContext {
    
        // (...)
        $session = $this->getMink()->getSession();
        $page = $session->getPage();
        
        $button = $page->find('css', '.class-css-du-bouton');
        $button->click();
    }

Notez que désormais notre classe n'hérite plus de `BehatContext` mais de `MinkContext`.

Examinons ce code ensemble. Tout d'abord, nous récupérons la page courante affichée par le navigateur :

    [php]
        $session = $this->getMink()->getSession();
        $page = $session->getPage();


Ensuite, il suffit de récupérer un élément HTML dans la page, puis d'exécuter l'action souhaitée sur cet élément (ici un clic, 
mais on pourrait imaginer saisir une valeur, le cocher...) :

    [php]
        $button = $page->find('css', '.class-css-du-bouton');
        $button->click();

Vous avez bien vu : la méthode `find()` permet de récupérer des éléments en utilisant des sélecteurs CSS. Ce sont les mêmes 
que ce que vous utilisez dans vos feuilles de style CSS. Attention, les pseudo-styles (':hover', etc) ne sont pas supportés. 
Bien entendu, il est également possible de récupérer des éléments HTML en utilisant des expressions xPath. La 
[documentation de Mink est assez complète](http://mink.behat.org/#xpath-selectors) sur ce sujet.

Il ne vous reste plus qu'à traduire les expressions fonctionnelles de vos clients en différentes actions au sein 
d'un navigateur, et vous saurez traiter alors la majorité des cas.

Sachez également que Mink permet très facilement de piloter un vrai navigateur. C'est utile lorsque l'on souhaite, par exemple, tester 
des fonctionnalités qui nécessite un environnement Ajax ou JavaScript. Dans ce cas, juste au dessus des 
Scénarios ou des Fonctionnalités concernées, il suffit d'ajouter le tag `@javascript`. Ce tag indiquera à Behat que vous souhaitez, dans ces 
cas spécifiques, lancer un vrai navigateur pour les tests. Bien entendu, dans ce cas, la machine qui exécute les tests doit posséder un 
environnement graphique (comme c'est le cas pour les ordinateurs de Bureau).


Lorsque l'on souhaite piloter un vrai navigateur, Behat délègue en réalité le travail à des outils spécialisés, comme `Sahi`, ou encore 
comme `Selenium`. Dans ce cas, ces outils doivent être installés sur votre machine. La procédure pour les installer est en 
général très simple, et est bien expliquée dans la [documentation officielle](http://mink.behat.org/#different-browsers-drivers).

Voici la configuration à ajouter dans le fichier `behat.yml` pour indiquer que vous souhaitez utiliser Chrome par exemple, grâce au driver Sahi :

    [yaml]
    default:
        extensions:
            Behat\MinkExtension\Extension:
                base_url: http://url-de-votre-site.fr/
                goutte: ~
                default_session: sahi
                javascript_session: sahi
                browser_name: chrome
                sahi:
                    host: localhost
                    port: 9999

Voici celle que vous pourriez utiliser pour piloter Firefox :

    [yaml]
    default:
        extensions:
            Behat\MinkExtension\Extension:
                base_url: http://url-de-votre-site.fr/
                goutte: ~
                default_session: sahi
                javascript_session: sahi
                browser_name: firefox
                sahi:
                    host: localhost
                    port: 9999


Vous avez désormais toutes les connaissances requises pour commencer à tester automatiquement un projet web. Pourquoi ne pas vous 
entraîner par exemple sur une petite application web de guichet bancaire ? 

![ La correction et les sources de cet exercice sont disponibles sur http://goo.gl/LISNg ]
(dev-qr-exo3.png)

Je vous propose de vous entraîner en traduisant le besoin fonctionnel suivant :

    [gherkin]
    #language: fr
    Fonctionnalité: Gérer un compte bancaire
      Afin de gérer mon compte bancaire
      En tant qu'utilisateur connecté
      Je peux ajouter ou retirer de l'argent sur mon compte
      
      Contexte:
        Etant donné que je suis connecté en tant que "jean-françois"
        Et que j'ai "50" euro
        Et je suis sur "/"
        
      Scénario: Consulter mes comptes
        Alors je devrais voir "Vous avez 50 euro sur votre compte"
        
      Plan du Scénario: Ajouter de l'argent
        Etant donné que j'ai "<montantInitial>" euro
        Quand je sélectionne "<operation>" depuis "Operation"
        Et je remplis "Montant" avec "<montant>"
        Et je presse "Go"
        Alors je devrais voir "Vous avez <montantFinal> euro sur votre compte"
        
        Exemples:
         | operation     | montantInitial| montant   | montantFinal  |
         | Ajouter       | 50            | 10        | 60            |
         | Ajouter       | 50            | 20        | 70            |
         | Ajouter       | 50            | 5         | 55            |
         | Ajouter       | 50            | 0         | 50            |
         | Retirer       | 50            | 10        | 40            |
         | Retirer       | 50            | 20        | 30            |
         | Retirer       | 50            | 30        | 20            |
         
      Scénario: Les découverts sont interdits
        Etant donné que j'ai "50" euro
        Quand je sélectionne "Retirer" depuis "Operation"
        Et je remplis "Montant" avec "60"
        Et je presse "Go"
        Alors je devrais voir "Vous avez 50 euro sur votre compte"
        Et je devrais voir "Les découverts ne sont pas autorises"

A vous de jouer ! 

> Mink permet facilement de piloter un navigateur pour tester vos applications web.
