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

    [bash]
    Utilisez WampServer (http://www.wampserver.com)

Ubuntu, Debian :

    [bash]
    # en ligne de commande
    apt-get install php5-common php5-cli php5-curl

Mac :

    [bash]
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

    [bash]
    curl http://getcomposer.org/installer | php
    php composer.phar install --prefer-source

### Installation sous forme d'archive Phar

Il vous suffit de télécharger le fichier behat.phar à l'adresse suivante : http://behat.org/
downloads/behat.phar, et de le placer dans un dossier `bin` à la racine de votre projet. Vous
aurez donc l'arborescence suivante :

    [bash]
    /chemin/vers/mon/projet
        (...)
        - bin/
            - behat.phar


Pour des raisons de maintenabilité, il est préferable d'utiliser la méthode d'installation
avec Composer, qui vous permettra de mettre à jour très facilement l'ensemble des librairies 
PHP que vous utilisez, y compris Behat.

### Préparer le projet

Une fois que Behat est installé, il vous suffit d'exécuter la commande suivante :

    [bash]
    php ./bin/behat --init

Cela aura pour effet de créer dans votre projet tous les éléments dont Behat a besoin pour
fonctionner. Votre arboresence ressemble désormais à :

*Screenshot*


## Traduire une Fonctionnalité en code source
## Testez dans un vrai navigateur
## Organisez vos Contextes de tests
## Réutilisez vos précédents tests