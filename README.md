# PHP/SQL - Premiers pas : formulaire de contact

## Avant d'écrire du PHP

- Dans le répertoire `www` de votre environnement de développement (composé *a minima* d'une distribution **L**inux - en l'occurence Debian si vous avez suivi [mon tuto « Install LAMP in WSL2 »](https://github.com/SolangeHarmoniePICARD/doc_wsl2-debian) -, d'**A**pache, de **M**ariaDB & de **P**HP), créez un dossier que vous nommerez `feature_contact-form` :

```
mkdir /var/www/feature_contact-form
```

> `/` est le répertoire racine d'une arborescence Linux.
>
> `/var` est le dossier système qui stocke des données variables.
>
> `/var/www` est le répertoire où Apache 2 va par défaut chercher les sites et les pages web.

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

- Dans votre navigateur, tapez `localhost/feature_contact-form` :

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
    - Tout d'abord, remplacez le `echo` devant `$_POST['data-username'];` par `$_SESSION[] =` ; 
    - Dans les crochets de `$_SESSION[]`, mettez des *simple quotes* et écrivez « *message* » dedans ;
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



**index.php**
- Maintenant, affichons notre message dans `index.php`. Pour se faire, sous votre balise `</form>`, ouvrez une balise `<p>` et fermez-la avec `</p>`. À l'intérieur, tapez `<?=` : c'est un raccourci pour écrire `<?php echo`. Ensuite, `$_SESSION['message'];` puis fermez avec ` ?>`. Ça devrait ressembler à ça :

index.php
```php
<p>
    <?= $_SESSION['message']; ?>
</p>
```

- Saisissez des données dans vos champs de formulaire, appuyez sur « Envoyer ». Les données sont envoyées à la page de traitement, mais vous ne vous en rendez pas compte ! Et la page `index.php` se recharge et affiche votre message :

![message](screenshots/message.png)

- Satisfaisant, non ? Sauf qu'on va avoir un petit problème supplémentaire. Si vous chargez votre page `index.php` alors qu'aucune donnée n'a encore été soumise via le formulaire, PHP va quand même essayer d'afficher `<?= $_SESSION['message']; ?>`. Vous aurez alors un magnifique message d'erreur de type :

![error](screenshots/error.png)

- La solution à ce problème, c'est un concept fondamental en programmation informatique : les conditions !