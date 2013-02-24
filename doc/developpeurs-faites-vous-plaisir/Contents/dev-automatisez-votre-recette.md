# Automatisez votre recette

Vous l'avez vu, Gherkin est un moyen simple pour votre fonctionnel pour vous fournir
une expression de besoin claire et précise.

Que diriez-vous maintenant de traduire ce besoin en tests automatisés ? Car, oui, c'est possible !

Tout besoin, s'il est exprimé clairement, peut être testé. Si ce besoin est exprimé dans une
grammaire solide et complète, tout système d'information peut le tester automatiquement.

Je vous propose de voir comment traduire ce besoin en tests automatisés, de telle sorte
qu'une simple ligne de commande dans un terminal vous indique si ce que vous avez développé est 
conforme ou non.

L'ensemble des exemples que nous allons voir sont écrits en PHP. Pourquoi ? Tout simplement 
parce que la communauté PHP a très fortement et très rapidement adheré au Déve-
loppement piloté par le comportement. Les outils PHP pour le Développement piloté par
le comportement sont matures, nombreux, et surtout offrent une souplesse qui, à ce jour,
ne se retrouve pas dans d'autres langages.

Cependant, il existe des outils pour écrire et lancer des tests automatisés dans n'importe
quel langage :

+ Ruby : Cucumber (http://cukes.info)
+ Java : JBehave (http://jbehave.org)
+ C# : NBehave (https://github.com/nbehave/NBehave)
+ Python : Behave (http://packages.python.org/behave/)
+ PHP : Behat (http://behat.org) , PHPSpec (http://www.phpspec.net)
+ JavaScript : Jasmine (http://pivotal.github.com/jasmine/)
+ .Net: NBehave (http://nbehave.org)

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

    [code bash]
    Utilisez WampServer (http://www.wampserver.com)

Ubuntu, Debian :

    [code bash]
    # en ligne de commande
    apt-get install php5-common php5-cli php5-curl

Mac :

    [code bash]
    Utilisez MampServer (http://www.mamp.info)


Installer Behat est simple, peut être fait de plusieurs manières : sous forme d'archive PHP
(phar) ou en utilisant un gestionnaire de dépendance.

### Installation avec Composer

Créez un fichier composer.json à la racine de votre projet, avec le contenu suivant :

    [json]
    {
        "require": {
            "behat/behat": "2.4.*@stable"
        },
        "config": {
            "bin-dir": "bin/"
        }
    }


Exécutez ensuite la commande suivante dans votre terminal, en vous plaçant àa la racine
de votre projet :

    [code bash]
    curl http://getcomposer.org/installer | php
    php composer.phar install --prefer-source

### Installation sous forme d'archive Phar

Il vous suffit de télécharger le fichier behat.phar à l'adresse suivante : http://behat.org/
downloads/behat.phar, et de le placer dans un dossier `bin` à la racine de votre projet. Vous
aurez donc l'arborescence suivante :

    [code bash]
    /chemin/vers/mon/projet
        (...)
        - bin/
            - behat.phar


Pour des raisons de maintenabilité, il est préferable d'utiliser la méthode d'installation
avec Composer, qui vous permettra de mettre à jour très facilement l'ensemble des librairies 
PHP que vous utilisez, y compris Behat.

### Préparer le projet

Une fois que Behat est installé, il vous suffit d'exécuter la commande suivante :

    [code bash]
    php ./bin/behat --init

Cela aura pour effet de créer dans votre projet tous les éléments dont Behat a besoin pour
fonctionner. Votre arboresence ressemble désormais à :

Vous voici désormais avec les dossiers suivants :

+ `features` : contient la listes des fonctionnalités, sous forme de fichier `.feature`
+ `features/bootstrap` : contient les fichiers PHP de traduction de Fonctionnalité en code source

Chaque nouvelle fonctionnalité devra donc être ajoutée dans le dossier ̀ features`, sous forme d'un 
fichier `.feature`. Créez par exemple le fichier  `features/calculer-mon-age.feature`, avec le 
contenu suivant :


![ Les sources de cet exercice sont disponibles sur http://goo.gl/4usXU ]
(dev-qr-exo1-step1.png)


    [code gherkin]
    # language: fr
    Fonctionnalité: Calculer l'âge d'une personne
        En tant qu'utilisateur de l'application
        Je veux connaître le nombre d'années écoulées entre deux dates
        De telle sorte que je puisse connaître mon age

    Scénario: Calculer l'âge d'une personne depuis une date antérieure à aujourd'hui
        Etant donné que je suis né le 06/07/1986
        Et que nous sommes le 20/09/013
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

    [code bash]
    ./bin/behat

![ Les étapes de la fonctionnalité ne sont pas encore traduites : elles sont jaunes ]
(dev-behat-exo-date-undefined.jpg)

Comme vous pouvez le voir, le résultat de cette commande est jaune. Cela veut tout simplement dire 
que les phrases utilisées pour exprimer le besoin n'ont pas encore de signification pour Behat. Il va falloir 
les traduire.


## Traduire une Fonctionnalité en code source


Par chance, Behat est suffisammet bien fait pour vous fournir une base de travail pour traduire vos fonctionnalités. Il suffit 
de copier-coller dans le fichier `features/bootstrap/FeatureContext.php` le code PHP qui a été généré par la commande :

    [code bash]
    ./bin/behat

Pourquoi copier ce code dans ce fichier ? Tout simplement parce que les fichiers `features/bootstrap/*Context.php` vont servir 
de passerelle entre le besoin exprimé (sous forme de phrases) et votre application. 

Il vous appartient de modifier ces fichiers pour faire la traduction du besoin fonctionnel. C'est un travail 
qui semble long, mais en réalité il n'est pas beaucoup plus long que de tester vous-même à main que la demande initiale 
est respectée, mais à surtout l'avantage de rendre ce travail de recette interne totalement automatique.

Traduisez maintenant la fonctionnalité en code source dans le fichier `features/bootstrap/FeatureContext.php`, en adaptant 
le code PHP fourni par Behat selon votre besoin. Par exemple :

    [code php]
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
         * @Given /^je calcule mon âge$/
         */
        public function jeCalculeMonAge()
        {
            $this->output = shell_exec(sprintf('php src/age.php --birthdate=%s --today=%s', $this->birthDate->format('Y-m-d'), $this->today->format('Y-m-d')));
        }

        /**
         * @Given /^je suis informé que j\'ai (\d+) ans$/
         */
        public function jeSuisInformeQueJAiAns($age)
        {
            if(!preg_match('!'.$age.' ans!', $this->output)) {
                throw new Exception();
            }
        }

        /**
         * @Given /^je suis informé que je ne suis pas encore né$/
         */
        public function jeSuisInformeQueJeNeSuisPasEncoreNe()
        {
            if(!preg_match('!Vous n\'êtes pas encore né!', $this->output)) {
                throw new Exception();
            }
        }

        /**
         * @Given /^on me souhaite un joyeux anniversaire$/
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


Maintenant, il vous suffit de relancer Behat pour vérifier automatiquement que votre application a bien 
le comportement attendu :

    [code bash]
    ./bin/behat

![ Les fonctionnalités échouent : elles sont rouges ]
(dev-behat-exo-date-fail.jpg)


Il vous suffit finalement de développer votre application PHP, conforme aux attentes fonctionnelles, puis de relancer 
Behat pour vous assurer que vous avez bien traité la demande initiale.


## Exploitez les jeux d'exemples

![ Les sources de cet exercice sont disponibles sur http://goo.gl/fYZHv ]
(dev-qr-exo1-step2.png)

Vous l'avez vu, votre fonctionnel peut vous fournir des exemples, grâce à la syntaxe Gherkin.

On pourrait par exemple modifier notre fonctionnalité de la façon suivante :

    [code gherkin]
    # language: fr
    Fonctionnalité: Calculer l'âge d'une personne
        En tant qu'utilisateur de l'application
        Je veux connaître le nombre d'années écoulées entre deux dates
        De telle sorte que je puisse connaître mon age

        Plan du Scénario: Calculer l'âge d'une personne
          Etant donné que je suis né le "<dateNaissance>"
          Et que nous sommes le "<dateDuJour>"
          Quand je calcule mon âge
          Alors on me répond "<reponseAttendu>"

          Exemples:
            | dateNaissance | dateDuJour | reponseAttendu                           |
            | 06/07/1986    | 20/09/2013 | Vous avez 27 ans                         |
            | 06/07/1985    | 20/09/2013 | Vous avez 28 ans                         |
            | 26/11/2020    | 20/09/2013 | Vous n'êtes pas encore né                |
            | 06/07/1986    | 06/07/2013 | Vous avez 27 ans. Joyeux anniversaire    |


Vous voici avec une fonctionnalité simple, claire et complète.

Travailler avec des exemples ne change strictement rien pour vous. En réalité, Behat 
va travailler avec chaque jeu d'exemple, un à un, et va vous envoyer chaque information 
sous forme de paramètre de méthode, sans que cela n'ait le moindre impact sur votre code.

Voici une traduction possible de cette fonctionnalité :

    [code php]
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
         * @Given /^je calcule mon âge$/
         */
        public function jeCalculeMonAge()
        {
            $this->output = shell_exec(sprintf('php src/age.php --birthdate=%s --today=%s', $this->birthDate->format('Y-m-d'), $this->today->format('Y-m-d')));
        }

        /**
         * @Given /^on me répond "([^"]*)"$/
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

![ Les sources de cet exercice sont disponibles sur http://goo.gl/klmDP ]
(dev-qr-exo2-step1.png)


Vous l'avez vu, la phase de traduction d'une phrase (étape) en code source peut parfois être longue.

C'est pour cela que vous devez faire en sorte de faciliter votre travail : réutilisez au maximum vos définitions. Attention, je ne 
vous parle en aucun cas ici de découper votre code comme vous le feriez dans une application (refractoring, respect des 
principes SOLID, découpage des méthodes...).

Non, la réutilisabilité dans les Contextes de traduction (fichiers `*Context.php`) ne passe pas par une réutilisation du code source, 
mais bel et bien par une réutilisation de phrases. *En réalité, vous devez apprendre à faire du refractoring de phrases*.

Cette tâche n'est pas simple au départ, mais est en réalité assez naturelle. Chaque phrase, qui correspond à un contexte, un 
élément déclencheur ou un résultat attendu, peut être découpée en différentes étapes, qui *elles-mêmes pourront être décomposées 
jusqu'à arriver à des expressions atomiques simples*.

Prenez par exemple la phrase suivante :

    [code gherkin]
    Quand j'ajoute dans mon panier "télévision Sony"  depuis le catalogue produit

Cette phrase peut être découpée, par exemple :

    [code gherkin]
    Etant donné que je consulte le catalogue produit
    Quand j'ajoute dans mon panier "télévision Sony"

Qui elles-mêmes peuvent être découpées pour créer des expressions atomiques réutilisables :

    [code gherkin]
    Etant donné que je suis sur la page "/catalogue/produits"
    Quand je coche "télévision Sony"
    Et je clique sur "Ajouter au panier"

Vous pouvez constater que les trois dernières expressions sont en réalité des étapes (web) atomiques réutilisables à l'infini. 
Cela signifie qu'une fois que vous les aurez traduites, vous n'aurez plus jamais à revenir dessus, et vous pourrez vous en servir à votre gré. 

Vous pourriez désormais écrire, par exemple :

    [code gherkin]
    Etant donné que je suis sur la page "/home"
    Etant donné que je suis sur la page "/mon-compte"
    ...
    
Cette démarche permet donc de faire du refractoring de phrases pour arriver à des expressions atomiques. Par chance, c'est 
extrêmement simple à faire avec Behat. Regardez plutôt :

    [code php]
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
étape est en réalité un objet (`Given`, `When` ou `Then`) qui accepte en paramètre de constructeur la phrase que l'on souhaite 
utiliser. Pratique non ?

Ce qui donnerait par exemple dans notre cas :

    [code php]
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


Notez les trois dernières traduction (les méthodes ̀ queJeSuisSurLaPage()`, `jeCoche()` et `jeCliqueSur()`). Ces méthodes 
de traduction concernent le comportement d'un navigateur web, comme Firefox ou Chrome... Il arrive en effet très souvent 
qu'il faille effectuer des actions qui n'existent que dans un navigateur. Vous allez voir qu'avec Behat c'est très facile !

> Oubliez le refractoring de code. Vous devez apprendre à faire du refractoring de phrases.

## Testez dans un navigateur



## Organisez vos Contextes de tests