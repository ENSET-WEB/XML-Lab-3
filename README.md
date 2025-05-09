# Sujet N°1 : Expressions XPath

Cette première section se concentrera sur l'utilisation de XPath, qui est un langage de requête pour sélectionner des nœuds dans un document XML.

## Partie 1 : Spaghetti.XML

Ici, nous devons écrire des expressions XPath pour un document de recette.

### 1.1 Ensemble des éléments ingredient
Pour sélectionner tous les éléments ingredient, peu importe leur emplacement dans le document :
```xpath
//ingredient
```
Cela utilise la double barre oblique pour rechercher dans tout le document des éléments nommés "ingredient".

### 1.2 Les commentaires de la partie preparation
Pour sélectionner tous les commentaires dans la section preparation :
```xpath
//preparation//comment()
```
Cela sélectionne tous les nœuds de commentaire qui sont des descendants des éléments preparation.

### 1.3 Les deux derniers commentaires du document
Pour sélectionner les deux derniers commentaires dans l'ensemble du document :
```xpath
(//comment())[position() >= last()-1]
```
Cela trouve tous les commentaires, puis utilise des prédicats pour sélectionner uniquement les deux derniers.

### 1.4 Nombre de nœuds suivant la première phase de préparation
Pour compter les nœuds suivant la première phase de préparation :
```xpath
count((//preparation/phase)[1]/following::*)
```
Cela compte tous les éléments qui suivent le premier élément phase dans la section preparation.

### 1.5 Nœuds suivants la fiche technique et nœuds précédents
Pour les nœuds suivant la "fiche technique" :
```xpath
//fiche_technique/following::*
```

Pour les nœuds précédant la "fiche technique" :
```xpath
//fiche_technique/preceding::*
```

### 1.6 Nombre de phases de préparation avec texte < 180 caractères
```xpath
count(//preparation/phase[string-length(text()) < 180])
```
Cela compte les éléments phase dans preparation dont le contenu textuel est inférieur à 180 caractères.

## Partie 2 : GrandPrix.XML

### 2.1 Tous les nœuds éléments de nom attr
```xpath
//attr
```

### 2.2 Tous les nœuds éléments qui ont un attribut order
```xpath
//*[@order]
```
Cela sélectionne tout élément qui a un attribut nommé "order".

### 2.3 Le fils de nom attr de cast
```xpath
//cast/attr
```
Cela sélectionne les éléments attr qui sont des enfants directs de cast.

### 2.4 Le premier fils de nom name d'un Composer
```xpath
//Composer/name[1]
```
Cela sélectionne le premier élément name qui est un enfant direct de tout élément Composer.

### 2.5 Le nœud attribut de nom Crew (attribut de filmography)
```xpath
//filmography/@Crew
```
Cela sélectionne l'attribut Crew de l'élément filmography.

### 2.6 Le quatrième fils de filmography
```xpath
//filmography/*[4]
```
Cela sélectionne le quatrième élément enfant de filmography.

### 2.7 Les quatre premiers fils de filmography
```xpath
//filmography/*[position() <= 4]
```
Cela sélectionne les quatre premiers éléments enfants de filmography.

### 2.8 Les nœuds attributs de nom order
```xpath
//@order
```
Cela sélectionne tous les attributs order dans le document.

### 2.9 Depuis remainder, retourner le sous-arbre de racine Editor
```xpath
//remainder//Editor
```
Avec remainder comme nœud de contexte, cela trouve l'élément Editor dans son sous-arbre.

### 2.10 Le nombre d'acteurs de premier plan (cité dans cast)
```xpath
count(//cast/*)
```
Cela compte tous les enfants directs des éléments cast.

### 2.11 Le nombre total de comédiens (cast, remainder, miscellaneous)
```xpath
count(//cast/*) + count(//remainder/*) + count(//miscellaneous/*)
```
Cela compte tous les acteurs dans les trois catégories.

### 2.12 Une chaîne de caractères présentant le film
```xpath
concat(//title/text(), ' (', //year/text(), ') - ', //Director/name/text())
```
Cela concatène le titre, l'année et le nom du réalisateur avec un formatage approprié.

### 2.13 L'ensemble des intervenants dont le prénom est Bob
```xpath
//*[contains(name/text(), 'Bob') or starts-with(name/text(), 'Bob ')]
```
Cela trouve les éléments dont le nom contient ou commence par "Bob".

# Sujet N°2 : Transformation XSLT

Cette deuxième section se concentre sur XSLT, qui est utilisé pour transformer des documents XML en d'autres formats.

Pour transformer le document biblio.xml en HTML, nous créerions un fichier XSLT comme ceci :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
  <xsl:template match="/">
    <html>
      <head>
        <title>Ma Bibliothèque</title>
        <style>
          table {
            border-collapse: collapse;
            width: 100%;
          }
          th, td {
            border: 1px solid black;
            padding: 8px;
            text-align: left;
          }
          th {
            background-color: #f2f2f2;
          }
        </style>
      </head>
      <body>
        <h1>Ma Bibliothèque</h1>
        <table>
          <tr>
            <th>Titre</th>
            <th>Auteur</th>
            <th>Année</th>
            <th>Prix</th>
          </tr>
          <xsl:for-each select="bibliotheque/livre">
            <tr>
              <td><xsl:value-of select="titre"/></td>
              <td><xsl:value-of select="auteur"/></td>
              <td><xsl:value-of select="annee"/></td>
              <td><xsl:value-of select="prix"/></td>
            </tr>
          </xsl:for-each>
        </table>
      </body>
    </html>
  </xsl:template>
</xsl:stylesheet>
```

Pour trier la bibliothèque par titre par ordre alphabétique, nous ajouterions une directive de tri dans le for-each :

```xml
<xsl:for-each select="bibliotheque/livre">
  <xsl:sort select="titre"/>
  <!-- reste du code -->
</xsl:for-each>
```

# Sujet N°3 : XSLT pour la Recette de Spaghetti

Pour transformer le Spaghetti.XML en un document HTML affichant les ingrédients :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
  <xsl:template match="/">
    <html xmlns="http://www.w3.org/1999/xhtml">
      <head>
        <title>Recette de Spaghetti - Ingrédients</title>
        <style>
          body { font-family: Arial, sans-serif; margin: 20px; }
          h1 { color: #d04a00; }
          ul { list-style-type: circle; }
          li { margin: 5px 0; }
        </style>
      </head>
      <body>
        <h1>Ingrédients pour la recette de Spaghetti</h1>
        <ul>
          <xsl:for-each select="//ingredient">
            <li>
              <xsl:value-of select="."/>
              <xsl:if test="@quantite">
                <xsl:text> - </xsl:text>
                <xsl:value-of select="@quantite"/>
              </xsl:if>
            </li>
          </xsl:for-each>
        </ul>
      </body>
    </html>
  </xsl:template>
</xsl:stylesheet>
```
