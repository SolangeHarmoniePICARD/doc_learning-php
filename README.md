# PHP/SQL - Premiers pas : formulaire de contact

## Avant d'écrire du PHP

- Dans le répertoire `www` de votre environnement de développement (composé *a minima* d'une distribution **L**inux (en l'occurence Debian si vous avez suivi [mon tuto « Install LAMP in WSL2 »](https://github.com/SolangeHarmoniePICARD/doc_wsl2-debian)), d'**A**pache, **M**ariaDB & **P**HP), créez un dossier que vous nommerez `feature_contact-form` :

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

## Les bases

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

```html
<form action="handler.php" method="post"></form>
```

- Dans votre terminal BASH, créez le fichier `handler.php` en tapant :

```
touch /var/www/feature_contact-form/handler.php
```

- Entre notre balise ouvrante `<form>` et sa balise fermante `</form>`, on va créer trois nouvelles balises. Tout d'abord une balise `label` dont l'attribut `for=""` prendra comme valeur `field-username`. Pour la générer, tapez `label` et appuyez sur la touche `ENTRÉE` de votre clavier, puis renseignez la valeur. Puis, entre la balise ouvrante `<label>` et sa balise fermante `</label>`, tapez `Nom :`. En effet, ce premier champs permettra à l'utilisateur de votre formulaire qui souhaite vous contacter d'indiquer son nom !

```html
<label for="field-username">Name: </label>
```

- Ensuite, créez une balise `input` de type `text`. Tapez `input:text` et appuyez sur la touche `ENTRÉE` de votre clavier. En plus du type, vous remarquez 2 attributs dont il faut renseigner les valeurs. L'attribut `id` doit être le même que l'attribut `for` du `label`, écrivez donc `id="field-username"`. L'attribut `name=""` est autrement plus important. C'est grâce à lui que la variable superglobale `$_POST` qu'on créera dans votre fichier `handler.php` va récupérer les informations rentrées par vos utilisateurs dans les champs de votre formulaire pour effectuer des traitements. L'attribut `name=""` prend donc comme valeur un nom qui va correspondre à la donnée saisie par l'utilisateur. On lui donnera donc comme valeur `name="data-username"`. Vous pouvez rajouter un quatrième attribut, le `placeholder`. C'est cette information dans le champs de formulaire écrite de façon grisée et qui disparaît quand vous placez votre curseur dans le champs, mais qui donne une information supplémentaire à votre utilisateur sur ce qu'il doit renseigner dans ce champs. Vous pouvez par exemple écrire en français « Votre nom » : 

```html
<input type="text" name="data-username" id="field-username" placeholder="Votre nom">
```

- Enfin, on va créer notre troisième `input`, qui ne sera en fait pas un champs de formulaire mais le bouton qui envoie le formulaire à la page de traitement spécifiée dans l'attribut `action=""`. Cet `input` sera de type `submit`, Tapez `input:text` et appuyez sur la touche `ENTRÉE` de votre clavier. Il prend comme attribut `value=""` le mot que vous voulez faire apparaître dans votre bouton. Par exemple le mot français « Envoyer » : 

```html
<input type="submit" value="Envoyer">
```

- Le résultat auquel vous devriez être arrivé : 

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

## Un peu de pratique !

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
