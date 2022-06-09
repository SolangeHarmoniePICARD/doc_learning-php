# PHP/SQL - Premiers pas : formulaire de contact

## Avant d'écrire du PHP

- Dans le répertoire `www` de votre environnement de développement (composé *a minima* d'une distribution **L**inux - en l'occurence Debian si vous avez suivi [mon tuto « Install LAMP in WSL2 »](https://github.com/SolangeHarmoniePICARD/doc_wsl2-debian-fr) -, d'**A**pache, de **M**ariaDB & de **P**HP, **mais aussi de Postfix et de MailDev**), créez un dossier que vous nommerez `feature_contact-form` :

```
mkdir /var/www/feature_contact-form
```

> `/` est le répertoire racine d'une arborescence Linux.
>
> `/var` est le dossier système qui stocke des données variables.
>
> `/var/www` est le répertoire où Apache2 va par défaut chercher les sites et les pages web.

- Positionnez-vous à l'intérieur de ce dossier : 

```
cd /var/www/feature_contact-form
```

- Créez un fichier `index.php` :

```
touch index.php
```

## Les bases : un formulaire en HTML

- Ouvrez `index.php` avec VSCode. Il n'y aura presque pas de PHP dans ce fichier, puisqu'il contiendra notre formulaire, donc principalement du HTML. Toutefois, nous ferons passer des messages de succès ou d'erreurs depuis la page de traitement vers la page de formulaire via une variable superglobale : `$_SESSION`. Le fichier `index.php` doit donc commencer par la fonction PHP `session_start()`. Ouvrez la balise PHP : `<?php`, et fermez là sur la même ligne : `?>`. Entre ces deux balises, écrivez `session_start()` :

**index.php**
```php
<?php session_start(); ?>
```

- Ensuite, ligne 3, générez le `DOCTYPE` en tapant `!` puis en appuyant sur la touche `TAB` de votre clavier dans VSCode+. Personnalisez-le, par exemple en changeant la langue : notre page sera en français, donc changez la valeur de l'attribut `lang="en"` par `lang="fr"`. Entre les balises ouvrantes et fermantes `<title>` et `</title>`, tapez un titre (en français) pour votre page web, par exemple `Formulaire de Contact`. Enfin, rajoutez la très importante balise `meta:desc`. Pour ce faire, sous la balise `meta:viewport`, tapez `meta:desc`, appuyez sur la touche `ENTRÉE` de votre clavier, et rédigez une description en français de votre page web comme valeur à l'attribut `content=""` :

**index.php**
```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Un formulaire de contact en PHP/SQL.">
    <title>Formulaire de Contact</title>
</head>
<body>

</body>
</html>

```

- À l'intérieur des balises HTML ouvrantes et fermantes `<body>` et `</body>`, tapez `form:post` et appuyez sur la touche `ENTRÉE` de votre clavier. Entre les guillemets de l'attribut `action=""` de la balise `form` générée, tapez `handler.php`, qui sera votre page de traitement :

**index.php**
```html
<form action="handler.php" method="post"></form>
```

- Dans votre terminal BASH, créez le fichier `handler.php` en tapant :

```
touch /var/www/feature_contact-form/handler.php
```

- Entre notre balise ouvrante `<form>` et sa balise fermante `</form>`, on va créer trois nouvelles balises. Tout d'abord une balise `label` dont l'attribut `for=""` prendra comme valeur `field-username`. Pour la générer, tapez `label` et appuyez sur la touche `ENTRÉE` de votre clavier, puis renseignez la valeur. Puis, entre la balise ouvrante `<label>` et sa balise fermante `</label>`, tapez `Nom :`. En effet, ce premier champs permettra à l'utilisateur de votre formulaire qui souhaite vous contacter d'indiquer son nom !

**index.php**
```html
<label for="field-username">Name: </label>
```

- Ensuite, créez une balise `input` de type `text`. Tapez `input:text` et appuyez sur la touche `ENTRÉE` de votre clavier. En plus du type, vous remarquez 2 attributs dont il faut renseigner les valeurs. L'attribut `id` doit être le même que l'attribut `for` du `label`, écrivez donc `id="field-username"`. L'attribut `name=""` est autrement plus important. C'est grâce à lui que la variable superglobale `$_POST` qu'on créera dans votre fichier `handler.php` va récupérer les informations rentrées par vos utilisateurs dans les champs de votre formulaire pour effectuer des traitements. L'attribut `name=""` prend donc comme valeur un nom qui va correspondre à la donnée saisie par l'utilisateur. On lui donnera donc comme valeur `name="data-username"`. Vous pouvez rajouter un quatrième attribut, le `placeholder`. C'est cette information dans le champs de formulaire écrite de façon grisée et qui disparaît quand vous placez votre curseur dans le champs, mais qui donne une information supplémentaire à votre utilisateur sur ce qu'il doit renseigner dans ce champs. Vous pouvez par exemple écrire en français « Votre nom » : 

**index.php**
```html
<input type="text" name="data-username" id="field-username" placeholder="Votre nom">
```

- Enfin, on va créer notre troisième `input`, qui ne sera en fait pas un champs de formulaire mais le bouton qui envoie le formulaire à la page de traitement spécifiée dans l'attribut `action=""`. Cet `input` sera de type `submit`, Tapez `input:text` et appuyez sur la touche `ENTRÉE` de votre clavier. Il prend comme attribut `value=""` le mot que vous voulez faire apparaître dans votre bouton. Par exemple le mot français « Envoyer » : 

**index.php**
```html
<input type="submit" value="Envoyer">
```

- Le résultat auquel vous devriez être arrivé : 

**index.php**
```html
<?php session_start(); ?>

<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Un formulaire de contact en PHP/SQL.">
    <title>Formulaire de Contact</title>
</head>
<body>
    
    <form action="handler.php" method="post">
        <label for="field-name">Nom : </label>
        <input type="text" name="data-username" id="field-name" placeholder="Votre nom">
        <input type="submit" value="Envoyez">
    </form>

</body>
</html>
```

- Dans la barre d'URL de votre navigateur, tapez `localhost/feature_contact-form` :

![Contact form](screenshots/contact-form.png)

- Et si vous saisissez une information dans votre champs de formulaire et que vous appuyez sur votre bouton « Envoyez », que se passe-t-il ? Testez !

![nothing](screenshots/nothing.png)

- En apparence, **il ne s'est rien passé**. Vous constatez juste que vous n'êtes plus sur votre page `index.php` et que vous êtes passé dans la page `handler.php`. **Mais c'est en apparence seulement !** Car en réalité... Faites un clic droit dans votre navigateur, et choisissez l'inspecteur dans le menu contextuel :

![inspect](screenshots/inspect.png)

- Dans l'onglet `Réseau`, cliquez sur `handler.php`. Pour que `handler.php` s'affiche, réactualisez la page ! Entre l'onglet `En-têtes` et l'onglet `Aperçu`, vous avez un onglet dont je ne me souviens plus du nom en français... En anglais c'est `Payload`. Cliquez sur cet onglet. Et là ! La valeur (`data-username`) de l'attribut `name` de votre `input` **et** la donnée saisie par l'utilisateur s'affichent ! Ce qui signifie qu'elle est disponible dans votre page `handler.php`. Reste à apprendre à la manipuler en PHP...

![payload](screenshots/payload.png)


### Un peu de pratique !

> Dans un formulaire de contact, on a besoin de 4 informations :
> - [X] le nom de la personne qui cherche à nous contacter
> - [ ] son adresse email pour pouvoir lui répondre
> - [ ] l'objet de la prise de contact
> - [ ] son message
>
> **Votre mission :** Complétez le formulaire de contact en rajoutant les 3 champs manquant !

⚠️ Il y a quelques spécificités : 
- les champs de formulaire pour les emails ne sont pas de type `text` !
- les champs de formulaire pour les longs messages ne sont pas des balises `input` ! 



## Les bases : la page de traitement en PHP

- Dans notre fichier `handler.php`, commençons par ouvrir notre balise `<?php`. Une spécificité par rapport à ce que nous avons vu précédemment : nous ne la refermerons pas. En effet, cette page de traitement n'a vocation à ne contenir **que du PHP**, on ne ferme donc pas la balise. Pour des questions de symétrie du code (car oui, les développeurs sont des esthètes !), on peut indiquer le bas de page par un commentaire : `// EOF`, qui signifie « *End of file* » :

**handler.php**
```php
<?php 

/* Votre code ici */

// EOF
```

- La première chose qu'on va faire, comme dans notre fichier `index.php`, c'est de démarrer la session avec la fonction `session_start()`, ainsi nous pourrons faire passer des informations de notre page de traitement à notre page de formulaire. Ensuite, nous allons afficher au moyen de la fonction `echo` la donnée soumise via le formumlaire (au moyen de l'`input` dont l'attribut `name` à la valeur `data-username`) qui est stockée dans la variable superglobale `$_POST['data-name']` :

**handler.php**
```php
<?php 

session_start();

echo $_POST['data-username'];

// EOF
```

L'incroyable résultat :

![L'incroyable résultat !](screenshots/result.png)

- Vous n'êtes pas très impressionnés ? Normal, ce n'est que le début. Bon, en réalité on ne veut pas afficher pour de vrai des informations dans notre page de traitement. Comme nous l'avons vu précédemment, elle ne doit contenir **que du PHP**. Il n'y a donc pas de `DOCTYPE` dans cette page, etc. d'où l'intérêt de notre fonction `session_start()` que nous avons ouverte sur nos deux pages : nous allons demander à PHP d'afficher le résultat de notre traitement sur notre page `index.php`. Procédons par étape :
    - Tout d'abord, remplacez le `echo` devant `$_POST['data-username'];` par `$_SESSION[] =` : on affecte la valeur `$_POST['data-username'];` à la variable superglobale de session ; 
        - Dans les crochets de `$_SESSION[]`, mettez des *simple quotes* (`'`) et écrivez « *message* » dedans pour donner une clé d'indexation. Le résultat : `$_SESSION['message'] = $_POST['data-username'];` ;
    - Entre le `=` et le `$_POST['data-username'];`, vous allez ouvrir des *simple quotes* pour spécifier que nous allons écrire une *string*, c'est à dire une chaine de caractères. Entre vos *simple quotes*, écrivez « Le nom d'utilisateur saisi est » : l'idée étant d'afficher un message en français confirmant le nom d'utilisateur saisi. Sauf que... ça ne fonctionnera pas en l'état...
        - Dans la phrase « Le nom d'utilisateur saisi est », il y a une apostrophe après le  « d ». Or une apostrophe, c'est une *simple quote* ! Donc la *simple quote* s'ouvre avant le mot « le » mais se referme après le « d' ». Vous allez donc devoir « échapper » votre apostrophe en séparant le « d » et l'apostrophe par une anti-slash « \ ». Votre phrase devrait ressembler à ça : `'Le nom d\'utilisateur saisi est'` ;
    - Ce n'est pas fini ! Entre votre *simple quote* fermante et `$_POST['data-username'];`, il va falloir concaténer. En PHP, l'opérateur de concaténation est le point. Vous devriez donc écrire quelque chose comme ça : `'Le nom d\'utilisateur saisi est ' . $_POST['data-username'];` ;
        - Remarquez l'espace entre le mot « est » et la *simple quote*. Il n'est pas là par hasard ! Si vous ne mettez pas d'espace, la concaténation se fera entre la lettre « t » et la donnée saisie dans le formulaire, de qui ne sera pas très esthétique...
    - Enfin, il vous reste à écrire sur la ligne suivante la fonction `header()` et à lui passer en paramètre `'Location : index.php'` ;
    - Le résultat final devrait ressembler à ça : 

**handler.php**
```php
<?php 

session_start(); 

$_SESSION['message'] = 'Le nom d\'utilisateur saisi est ' . $_POST['data-username'];

header('Location: index.php');

// EOF
```

> Notez que le code est plus court que les explications. 🤣

- Maintenant, affichons notre message dans `index.php`. Pour se faire, sous votre balise `</form>`, ouvrez une balise `<p>` et fermez-la avec `</p>`. À l'intérieur, tapez `<?=` : c'est un raccourci pour écrire `<?php echo`. Ensuite, `$_SESSION['message'];` puis fermez avec ` ?>`. Ça devrait ressembler à ça :

**index.php**
```php
<p>
    <?= $_SESSION['message']; ?>
</p>
```

- Saisissez des données dans vos champs de formulaire, appuyez sur « Envoyer ». Les données sont envoyées à la page de traitement, mais vous ne vous en rendez pas compte ! Et la page `index.php` se recharge et affiche votre message :

![message](screenshots/message.png)

- Satisfaisant, non ? Sauf qu'on va avoir un petit problème supplémentaire. Si vous chargez votre page `index.php` alors qu'aucune donnée n'a encore été soumise via le formulaire, PHP va quand même essayer d'afficher `<?= $_SESSION['message']; ?>`. Vous aurez alors un magnifique message d'erreur de type :

![error](screenshots/error.png)

- La solution à ce problème, c'est un concept fondamental en programmation informatique : les conditions ! On va donc écrire notre première condition. Supprimez la ligne `<?= $_SESSION['message']; ?>` entre vos balises `<p>` et `</p>`. À la place, ouvrez et fermez PHP : 

**index.php**
```php
<p>
    <?php
        // Votre code ici
    ?>
</p>
```

- La structure d'une condition est assez simple, elle se compose du mot clé `if` suivi de parenthèses qui prennent en paramètres la vérification à effectuer, et d'accolades pour écrire des instructions. Si la condition passée en paramètre se vérifie, l'instruction s'exécute : 

**index.php**
```php
<p>
    <?php
            if(/* la vérification */){
                /* l'instruction */                
            }
    ?>
</p>
```

- Dans notre cas, on va vérifier la présence d'un message stocké dans la variable superglobale `$_SESSION`. Si `$_SESSION['message']` est vraie, alors on affiche le message avec la fonction `echo`. Sinon, rien ne s'affiche. Pour tester une égalité, on utilise l'opérateur de comparaison `==` (compare les valeurs) ou `===` (comparaison plus stricte : valeur + type). 

> ⚠️ Ne confondez jamais les **opérateurs de comparaison** `==` et `===`, et l'**opérateur d'affectation** `=`. En PHP, `=` ne signifie pas « égal » mais permet d'affecter une valeur à une variable. 

**index.php**
```php
    if($_SESSION === true){
        echo $_SESSION['message'] ;
    } 
```

- En réalité, dans ce cas on peut simplifier en retirant `=== true` : passer `$_SESSION` en paramètre revient à vérifier si `$_SESSION` est vrai. Pour faire le choses proprement, on va mettre une seconde instruction après notre `echo` : on va nettoyer le contenu de `$_SESSION['message']` en lui ré-affectant une chaîne de caractère vide :

**index.php**
```php
    if($_SESSION){
        echo $_SESSION['message'] ;
        $_SESSION['message'] = "";
    }
```

- Testez dans le navigateur en vous connectant à  `localhost/feature_contact-form`. Si il n'y a pas de message d'erreur en dessous de votre formulaire, vous êtes pas mal. Entrez un nom dans votre champs de formulaire, appuyez sur votre bouton `Envoyez`, vous devriez avoir le même résultat que précédemment. Réactualisez la page : si tout s'est bien passé, le message disparaît. 

> ## Ce que nous avons vu jusqu'à présent
> - [X] On écrit du PHP dans un fichier portant l'extension `.php`
> - [X] On peut écrire du HTML dans les fichiers PHP (en fait, PHP est un moteur de *template*, il génère du HTML pour le client)
> - [X] On ouvre PHP avec la balise `<?php`
> - [X] On ferme PHP avec la balise `?>`
>   - [X] Il n'y a pas besoin de fermer PHP **si le fichier ne contient que du PHP** (on peut alors indiquer la fin du fichier par `// EOF`)
> - [X] On envoie des données à une page de traitement en PHP via des formulaires HTML :
>   - [X] la balise `<form>` doit contenir un attribut `action` qui prend pour valeur le chemin de la page de traitement, et un attribut `method` qui prend pour valeur `post`. Exemple : `<form action="handler.php" method="post"></form>`
>   - [X] la page de traitement stocke les données envoyées via le formulaire dans la variable superglobale `$_POST`, qui est un `array` qui associe comme valeur la donnée envoyée à une clé qui correspond à l'attribut `name` du champs de formulaire. Par exemple, la donnée renseignée dans le champs de formulaire `<input type="text" name="data-username">` sera récuprée avec `$_POST['data-username']`
> - [X] `$_POST` et `$_SESSION` sont des variables superglobales
> - [X] Ce qui est écrit entre *simple quotes* est de type *string* (chaîne de caractère)
> - [X] une instruction se termine par `;`
> - [X] `echo` permet d'afficher quelque chose
>   - [X] `<?= ?>` est un raccourci que l'on utilise dans les fichiers HTML pour `<?php echo ?>` pour afficher quelque chose qui tient sur une seule ligne
> - [X] `=` est un opérateur d'affection
> - [X] `==` et `===` sont des opérateurs de comparaison
> - [X] `if(){}` est la structure d'une condition
> - [X] `session_start();` est une fonction qui permet de démarrer une session
> - [X] Ce qui est écrit entre parenthèses est le paramètre d'une fonction
> - [X] `header();` est une fonction qui prend en paramètre la *string* `'Location: chemin/de/la/page/de/destination.php'` et permet de faire automatiquement la redirection

## L'envoi du mail en PHP

- Si vous avec réussi le défi « Un peu de pratique » un peu plus haut dans ce tutoriel, vous devez avoir un fichier `index.php` qui ressemble à ça : 

**index.php**

```html
<?php session_start(); ?>

<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Un formulaire de contact en PHP/SQL.">
    <title>Formulaire de Contact</title>
</head>
<body>
    
    <form action="handler.php" method="post">
        <label for="field-name">Nom : </label>
        <input type="text" name="data-username" id="field-name" placeholder="Votre nom">
        <label for="input-email">Email</label>
        <input type="text" name="data-email" id="input-email">
        <label for="input-subject">Subject</label> 
        <input type="text" name="data-subject" id="input-subject" />
        <label for="input-message">Message</label> 
        <textarea name="data-message" id="input- message"></textarea>
        <input type="submit" value="Envoyez">
    </form>

    <p>
        <?php
            if($_SESSION){
                echo $_SESSION['message'] ;
                $_SESSION['message'] = "";
            }
        ?>
    </p>

</body>
</html>
```

- Testez dans un navigateur : 

![form](screenshots/form-full.png)

- Pour la suite du tuto, assurez-vous que MailDev est lancé. Dans la barre URL de votre navigateur, tapez :

```
http://127.0.0.1:1080
```

- Vous devriez avoir ce résultat :

![MailDev](screenshots/maildev.png)

- Si ce n'est pas le cas, dans votre terminal Debian, tapez : 

```
maildev --ip 127.0.0.1
```

- Passons aux choses sérieuses. Dans votre fichier `handler.php`, juste après la fonction `session_start`, créez un `if`, et mettez le `$_SESSION` et la fonction `header` dans ce `if` :

**handler.php**
```php
<?php 

session_start(); 

if(){

    $_SESSION['message'] = 'Le nom d\'utilisateur saisi est ' . $_POST['data-username'];

    header('Location: index.php');

}

// EOF
```

- Maintenant il faut écrire une condition en paramètre du `if`. Ce qu'on veut tester, c'est que les champs sont configurés et qu'ils sont « différents de vide » (je sais, en français ça fait nulle). Pour chaque champs, il va donc falloir utiliser les fonctions `isset()` et `!empty()` (`!` signifie « différent de », ou « le contraire de »). Passez les données de chaque champs en paramètre de ces deux fonctions. Au passage, changez le message. Ça devrait ressembler à ça :

**handler.php**
```php
isset('data-username') && !empty('data-username') 
&& isset('data-email') && !empty('data-email') 
&& isset('data-subject') && !empty('data-subject')
&& isset('data-message') && !empty('data-message')
```

- Du coup, il faut mettre ça en paramètre de votre `if()` : 

**handler.php**
```php
<?php 

session_start(); 

if(isset($_POST['data-username']) && !empty($_POST['data-username']) 
&& isset($_POST['data-email']) && !empty($_POST['data-email']) 
&& isset($_POST['data-subject']) && !empty($_POST['data-subject'])
&& isset($_POST['data-message']) && !empty($_POST['data-message'])){

    $_SESSION['message'] = 'Ça marche !';

    header('Location: index.php');

}

// EOF
```

- Et rajoutez un `else` : si l'une des conditions de votre `if()` n'est pas vérifié, le script ne pourra pas entrer dans les instructions de votre `if()` (les instructions, c'est tout ce qui est entre les accolades `{}` après les paramètres `()`), et exécutera les instructions du `else`. Écrivez quelque chose comme ça : 

**handler.php**
```php
<?php 

session_start(); 

if(isset($_POST['data-username']) && !empty($_POST['data-username']) 
&& isset($_POST['data-email']) && !empty($_POST['data-email']) 
&& isset($_POST['data-subject']) && !empty($_POST['data-subject'])
&& isset($_POST['data-message']) && !empty($_POST['data-message'])){

echo 'ça marche' ;

    $_SESSION['message'] = 'Ça marche !';

    header('Location: index.php');

} else {

    $_SESSION['message'] = 'Remplissez tous les champs !';

    header('Location: index.php');

}

// EOF
```

- Testez dans un navigateur, en remplissant tous les champs, vous devriez obtenir le message « `Ça marche !` » :

![form success](screenshots/form.png)

- Puis faites le même test en laissant un seul champs vide avant d'appuyez sur le bouton `Envoyez`, puis recommencez en laissant un autre champs vide, puis en laissant plusieurs champs vide, etc. À chaque fois, vous devriez obtenir « `Remplissez tous les champs !` ». Si c'est le cas, c'est que votre script fonctionne :

![form error](screenshots/form-error.png)