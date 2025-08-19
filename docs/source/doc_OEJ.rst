[Draft] Import METS/TEI pour OpenEdition Journals (Lodel 1) 
############################################


Description de l’archive ZIP contenant les fichiers 
==============================================

À sa racine, l’archive ZIP doit contenir :

- **un répertoire** ``files`` contenant les illustrations utilisées dans les unités éditoriales, ainsi que l’image de couverture du volume.  
*Remarque :* pour les images, les formats admis par Lodel sont le JPG, le PNG et le SVG (ce dernier format étant non compatible pour les couvertures). Les images de couverture doivent, de plus, posséder une résolution de 300 DPI et mesurer au minimum 1400 pixels de large.

- **un répertoire** ``sources`` contenant les fichiers de chaque unité éditoriale au format XML TEI et PDF. Ces PDF (nommés « fac-similés » dans Lodel) doivent être en basse définition si le volume est issu de la numérisation ; dans les autres cas, il doit s'agir des PDF éditeurs en mode texte. Ce répertoire peut également contenir le PDF complet du volume ; 

- **un fichier METS** nommé *``MANIFEST.xml``.

La suite de cette documentation présente le contenu du fichier METS.

Description du fichier METS décrivant le volume
==============================================

1. Trois sections sont obligatoires :

- ``<structMap>``
- ``<filSec>``
- ``<dmdSec>``

2. Une section est optionnelle : ``<amdSec>``


Déclaration des schémas METS et MODS dans l’élément racine
-----------------------------------------------

.. code-block:: xml

  <?xml version="1.0" encoding="utf-8"?>
    <mets:mets
  xsi:schemaLocation="http://www.loc.gov/METS/ http://lodel.org/ns/mets/mets.openedition.1.3/mets.openedition.1.3.xsd 
  http://www.loc.gov/mods/v3 http://lodel.org/ns/mods/mods.openedition.1.2/mods.openedition.1.2.xsd"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:xsd="http://www.w3.org/2001/XMLSchema"
           xmlns:xlink="http://www.w3.org/1999/xlink"
           xmlns:mods="http://www.loc.gov/mods/v3"
           xmlns:mets="http://www.loc.gov/METS/"
           xmlns:marcrel="http://www.loc.gov/loc.terms/relators">

.. _mets-schema:




``<structMap>``
-----------------------------------------------

La ``<structMap>`` décrit l'arborescence du volume. L’ordre des éléments dans la ``<structMap>`` définit l’ordre d’apparition de l’élément dans son parent (volume ou sous-partie) à l’import dans Lodel.

**I. Les balises** ``<mets:div>`` **doivent s’imbriquer pour décrire l’arborescence volume/sous-parties/texte.**

1. Pour chaque balise ``<mets:div>``, les attributs obligatoires sont :

- ``TYPE``  : voir la liste des types autorisés ci-dessous (le schéma XML METS restreint l’utilisation des types possibles) ;

2. Pour chaque balise ``<mets:div>`` les attributs facultatifs sont :

- ``DMDID`` : doit être égal à l’identifiant de la ``<dmdSec>`` décrivant l'élément ``/mets:mets/mets:dmdSec/@ID`` (s’il n’y a pas d’élément ``<dmdSec>`` relatif à un élément ``<div>``, il ne faut pas indiquer cet attribut) ;
- ``LABEL`` : titre du document (facultatif mais très utile pour faciliter la lecture de la ``<structMap>`` et repérer les erreurs).

3. Les différentes versions des documents (.xml, .pdf, .docx) doivent être décrites dans la ``<structMap>`` avec la balise ``<mets:fptr>``.
Le seul attribut obligatoire de ``<mets:fptr>`` est :

- ``FILEID`` : identifiant du fichier utilisé dans la section ``/mets:mets/mets:fileSec/mets:fileGrp/mets:file/@ID``.



**II. La** ``<structMap>`` **doit refléter la structure complète du volume**

Ce n’est pas un encodage de la table des matières.

Les volumes comportent souvent des incohérences entre la table des matières et le corps de l'ouvrage. Par exemple : pour les unités éditoriale, les titres de la table des matières peuvent différer de ceux indiqués en début d’unité. Il faut donc utiliser les titres des documents et des parties disponibles dans le corps du volume, et non ceux de la table de matières. La table des matières sert à comprendre la structure du volume, mais en cas d’incohérence entre la table des matières et le contenu du volume, c’est l’organisation du contenu du volume qui doit être conservée. Il peut y avoir une part d’interprétation pour faire des choix et décrire correctement cette structure.


**III. Types autorisés dans la** ``<structMap>``

**1. Types de la classe « textes »**

- editorial
- article
- compterendu
- chronique

**2. Types de la classe « publications »**

- numero
- souspartie
             
**3. Types de la classe « fichiers »**

- couverture1
- imageaccroche
- facsimile  

*Remarque :* au niveau du volume, OpenEdition Journals admet le type ``couverture1``, ``imageaccroche`` et ``facsimile``. Au niveau de l'unité éditoriale, OpenEdition Journals admet les types ``imageaccroche`` et ``facsimile``.



``<fileSec>``
-----------------------------------------------

Chaque fichier contenu dans l’archive ZIP doit être décrit dans cette section dans une balise ``<mets:file>``. 

*Remarque :* les métadonnées des unités éditoriales ne sont pas nécessaires, car elles sont déjà incluses dans les fichiers TEI de ces unités éditoriales.
  
La balise ``<mets:fileSec>`` doit contenir au moins une balise ``<mets:fileGrp`` avec un identifiant optionnel qui contient elle-même une balise ``<mets:file>``. 

*Remarque :* une balise ``<mets:fileGrp>`` est obligatoire pour la couverture, les fac-similés des unités ou celui du volume. Elle est également obligatoire pour les images et les fichiers annexes potentiels. Elle est cependant optionnelle pour les images présentes dans les unités éditoriales. 
  
**1. Les attributs obligatoires de** ``<mets:file>`` **sont :**

- ``ID`` : un identifiant unique ;
- ``MIMETYPE`` : le type MIME du fichier ;
- ``GROUPID`` : un identifiant commun à tous les fichiers représentant le même document et à toutes les images contenues dans ce document (par exemple : un même ``GROUPID`` pour les versions PDF et XML TEI, ainsi que pour les images appelées dans le fichier XML TEI).  
  
**2. Les attributs optionnels sont :**

- ``CHECKSUM`` : la valeur du checksum MD5 du fichier ;
- ``CHECKSUMTYPE`` : dont la valeur doit être ``MD5``.

Chaque balise ``<mets:file>`` doit contenir une balise ``<mets:FLocat>`` pointant vers le fichier. Deux attributs sont obligatoires pour cette dernière :

- ``LOCTYPE`` : dont la valeur est ``URL`` ;
- ``xlink:href`` : il s’agit du chemin relatif vers le fichier dans l’archive ZIP.


*Exemple : fac-similé d’un article :*

.. code-block:: xml

  <mets:fileSec>
    <mets:fileGrp ID="pdf_files">
     <mets:file ID="ID7-24-pdf1" MIMETYPE="application/pdf" GROUPID="7_24" CHECKSUM="0780cf4909d721838276a2e812859cf9" CHECKSUMTYPE="MD5">
        <mets:FLocat LOCTYPE="URL" xlink:href="sources/7-24.pdf"/>
     </mets:file>
   </mets:fileGrp>
  </mets:fileSec>

.. _mets-facsimile:

Le fichier décrit ici est un PDF nommé « 7-24 ». L'identifiant de la section ``<fileSec>`` de ce PDF (soit « ID7-24-pdf1 »)  est présent dans ``<structMap>`` :

.. code-block:: xml

  <mets:structMap>
    <mets:div TYPE="numero" DMDID="ID_2001_05_1">
      <mets:div TYPE="souspartie" ORDER="2" LABEL="titre de la sous-partie" DMDID="ID_2001_05_1-section1">
       <mets:div TYPE="article" ORDER="1" LABEL="Titre de l'article">
          <mets:fptr FILEID="ID7-24-tei1"/>
         <mets:fptr FILEID="ID7-24-pdf1"/>
       </mets:div>
     </mets:div>
   </mets:div>
  </mets:structMap>

.. _mets-descirption_fichier:




 ``<dmdSec>``
-----------------------------------------------

**I. Présentation**

Chaque élément ``<div>`` utilisé dans la ``<strucMap>`` peut être décrit dans une ``<dmdSec>`` en `MODS`_.

.. _MODS : http://www.loc.gov/standards/mods
  
La balise ``<mets:dmdSec>`` doit contenir un attribut obligatoire :

- ID : un identifiant unique correspondant à l'attribut ``DMDID`` utilisé dans la ``<structMap>``

Les éléments ``<dmdSec>`` sont nécessaires pour : 

- tous les objets de classe « fichiers » : types ``imageaccroche``, ``couverture1`` et ``facsimile`` ; 
- et toutes les publications : ``numero`` et ``souspartie``

qui sont présents dans ``<structMap>`` et auxquels des métadonnées sont associées. 

Les métadonnées des types ``couverture1``, ``couverture4``, ``tdm``, ``imageaccroche`` et ``facsimile`` sont nécessaires mais réduites. Dans la plupart des cas il n’y aura que le titre. 

En revanche, les éléments ``<dmdSec>`` peuvent être omis pour les éléments de la classe « textes » (types ``editorial``, ``article``, ``compterendu``, ``chronique`` ou pour les objets sans métadonnées.

Toutes les informations descriptives du volume doivent être présentées au format MODS dans l'élément METS suivant :
``/mets:mets/mets:dmdSec[@ID="IDNUMERO"]/mets:mdWrap/mets:xmlData``.

*Exemple : le fac-similé du volume :*

.. code-block:: xml

  <mets:dmdSec ID="volume1-facsimile">
    <mets:mdWrap MDTYPE="MODS" MIMETYPE="text/xml">
      <mets:xmlData>
       <mods:titleInfo>
          <mods:title>Titre du fac-similé</mods:title>
       </mods:titleInfo>
     </mets:xmlData>
    </mets:mdWrap>
  </mets:dmdSec>

.. _mets-facsimile_volume:

Où l’``ID="XXXX"`` de cette section renvoie dans la ``<structMap>`` à ``<mets:div DMDID >`` :

*Par exemple :*

.. code-block:: xml

  <mets:structMap>
    <mets:div TYPE="facsimile" ORDER="2" LABEL="fac-similé" DMDID="volume1-facsimile">
     <mets:fptr FILEID="volume1-pdf"/>
    </mets:div>
  </mets:structMap>

.. _mets-structmap:

On retrouve l'identifiant de ce PDF (``FILEID="YYYY"``) dans ``<mets:fileSec>`` :

.. code-block:: xml

  <mets:fileSec>
    <mets:fileGrp ID="pdf_files">
      <mets:file ID="volume1-pdf" MIMETYPE="application/pdf" GROUPID="volume1" CHECKSUM="527373ff4d089fdf38d4d2794ecd8787" CHECKSUMTYPE="MD5">
       <mets:FLocat LOCTYPE="URL" xlink:href="volume1.pdf" />
     </mets:file>
    </mets:fileGrp>
  </mets:fileSec>

.. _mets-id:

*Remarque :* les métadonnées du type ``souspartie`` sont nécessaires, mais réduites. Dans la plupart des cas il n’y aura que le titre.  

XPath : ``//mods:titleInfo/mods:title``

*Remarque :* une sous-partie peut posséder sa propre introduction (voir à ce propos `la documentation MODS spécifique de OpenEdition Journals`_.

.. _la documentation MODS spécifique de OpenEdition Journals : https://github.com/OpenEdition/mets.openedition/wiki/3.-Documentation-MODS-pour-OpenEdition-Journals

*Par exemple pour une sous-partie dans une revue :* 

.. code-block:: xml

  <mets:dmdSec ID="Z">
    <mets:mdWrap MDTYPE="MODS" MIMETYPE="text/xml">
     <mets:xmlData>
       <mods:titleInfo>
          <mods:title>Titre de la sous-partie</mods:title>
        </mods:titleInfo>
      </mets:xmlData>
    </mets:mdWrap>
  </mets:dmdSec>

.. _mets-sous-partie:

Lorsqu’est déclaré dans la ``<structMap>`` un élément de ``TYPE="souspartie"`` :

.. code-block:: xml

  <mets:structMap>
    <mets:div TYPE="numero" DMDID="X">    
      <mets:div TYPE="souspartie" LABEL="Titre de la sous-partie" DMDID="Z">
        <mets:div TYPE="article" LABEL="Titre du premier article de la sous-partie">
          <mets:fptr FILEID="Y-tei1"/>
          <mets:fptr FILEID="Y-pdf1"/>
       </mets:div>
     </mets:div>
    </mets:div>     
  </mets:structMap> 

.. _mets-sous-partie_bis: 

*Exemple pour la couverture :*

.. code-block:: xml

  <mets:dmdSec ID="X">
    <mets:mdWrap MDTYPE="MODS" MIMETYPE="text/xml">
      <mets:xmlData>
        <mods:titleInfo>
          <mods:title>Titre de l’image de couverture</mods:title>
       </mods:titleInfo>
     </mets:xmlData>
    </mets:mdWrap>
  </mets:dmdSec>

.. _mets-couv:

Lorsqu’est déclaré dans la ``<structMap>`` un élément de TYPE ``couverture1`` :

.. code-block:: xml

  <mets:structMap>
    ...
    <mets:div TYPE="couverture1" LABEL="Titre de l’image de couverture" DMDID="X">
     <mets:fptr FILEID="Y" />
    </mets:div>
    ...
  <mets:structMap>

.. _mets-couv_bis:

Et que l'on retrouve dans la ``<mets:fileSec>`` :

.. code-block:: xml

  <mets:fileSec>
    ...
    <mets:fileGrp ID="img_files">
      <mets:file ID="Y" MIMETYPE="image/png" CHECKSUM="W" CHECKSUMTYPE="WW">
       <mets:FLocat LOCTYPE="URL" xlink:href="files/Y.png" />
      </mets:file>
    </mets:fileGrp>
    ...
  </mets:fileSec>

.. _mets-couv_ter:


**II. Les éléments du format MODS**

Dans ``<mets:dmdSec>``, il faut placer tous les éléments descriptifs du volume au format MODS.

■ **Titre du volume**  

XPpath : ``./mods:titleInfo[not(@type)]/mods:title``

Ce champ peut contenir du texte brut ou des éléments HTML simples  (``<p>``, ``<em>``, ``<strong>``, ``<br/>``, ``<i>``, ``<sub>``, ``<sup>``, ``<span style="font-variant:small-caps;">``). Pour permettre la validation XML du fichier METS, il faut placer le code HTML dans un CDATA.
*Exemple :* ``<![CDATA[ <p>Lorem <em>Ipsum</em></p> ]]>``

■ **Sous-titre du volume**  

XPath : ``./mods:titleInfo/mods:subTitle``

Ce champ peut contenir du texte brut ou des éléments HTML simples  (``<p>``, ``<em>``, ``<strong>``, ``<br/>``, ``<i>``, ``<sub>``, ``<sup>``, ``<span style="font-variant:small-caps;">``). Pour permettre la validation XML du fichier METS, il faut placer le code HTML dans un CDATA.
*Exemple :* ``<![CDATA[ <p>Lorem <em>Ipsum</em></p> ]]>``

■ **Titre(s) traduit(s) du volume**  

XPath : ``./mods:titleInfo[@type="translated"]/mods:title[@xml:lang="LANG-ISO639-1"]``  

Où LANG-ISO639-1 correspond à la langue du titre traduit selon la norme ISO639-1 (fr, en, de...). L’élément ``<mods:title>`` est répétable avec des valeurs de ``xml:lang`` différentes si le titre est traduit dans plusieurs langues.


■ **ISBN**

XPath : ``./mods:identifier[@type="isbn"]``   

■ **Directeur(s) de la publication**  

- Nom de famille :  

XPath : ``./mods:name[mods:role/mods:roleTerm/text()="director"]/mods:namePart[@type="family"]``

- Prénom :  

XPath : ``./mods:name[mods:role/mods:roleTerm/text()="director"]/mods:namePart[@type="given"]``

- Description :  

XPath : ``./mods:name[mods:role/mods:roleTerm/text()="director"]/mods:description``  

Ce champ peut contenir du texte brut ou des éléments HTML simples  (``<p>``, ``<em>``, ``<strong>``, ``<br/>``, ``<i>``, ``<sub>``, ``<sup>``, ``<span style="font-variant:small-caps;">``). Pour permettre la validation XML du fichier METS, il faut placer le code HTML dans un CDATA.

*Exemple :* ``<![CDATA[ <p>Lorem <em>Ipsum</em></p> ]]>``


■ **Description physique de l’ouvrage**  

Contient la description physique de l’édition papier.  
XPath : ``./mets:xmlData/mods:physicalDescription/mods:note``

■ **Indexation**

Les éléments d'indexations doivent être placés dans des balises ``<mods:subjects>``, distingués par un attribut ``authority`` et parfois ``xml:lang`` 

- Mots-clés en français :  

XPath : ``./mods:subject[@authority="motsclesfr"]/mods:topic``

- Keywords :  

XPath : ``./mods:subject[@authority="motsclesen"]/mods:topic``

- Parole chiave :  

XPath : ``./mods:subject[@authority="motsclesit"]/mods:topic``

- Schlagwortindex :  

XPath : ``./mods:subject[@authority="motsclesde"]/mods:topic``

- Palabras claves :  

XPath : ``./mods:subject[@authority="motscleses"]/mods:topic``

- Palavras chaves :  

XPath : ``./mods:subject[@authority="motsclespt"]/mods:topic``

*Remarque :* les entrées d’index ne s’affichent pas au niveau du sommaire du numéro, elles sont en revanche visibles sur les pages d’index.

■ **Les métadonnées des unités éditoriales (article, compte rendu, etc.)**

Elles ne sont pas nécessaires, car elles sont déjà incluses dans les fichiers TEI de ces unités éditoriales.
 
XPath : ``//mods:titleInfo/mods:title``


``<amdSec>``
-----------------------------------------------

Il faut renseigner une balise ``<amdSec>`` si l’on souhaite indiquer la source utilisée pour l’encodage. Cette source est soit ``ocr``, soit n’importe quelle autre valeur (qui indique qu’il ne s’agit pas d’OCR). 

Il faudra ajouter :

■ *dans le 1er cas :*

.. code-block:: xml

  <mets:amdSec>
    <mets:digiprovMD ID="AMDID_XXX">
      <mets:mdWrap MDTYPE="MODS" MIMETYPE="text/xml">
        <mets:xmlData>
         <mods:note type="sourcetype">ocr</mods:note>
        </mets:xmlData>
      </mets:mdWrap>
    </mets:digiprovMD>
  </mets:amdSec>

.. _mets-amdsec:

■ *ou dans le 2e cas :*

.. code-block:: xml

  <mets:amdSec>
    <mets:digiprovMD ID="AMDID_XXX">
      <mets:mdWrap MDTYPE="MODS" MIMETYPE="text/xml">
        <mets:xmlData>
         <mods:note type="sourcetype">publisher pdf</mods:note>
       </mets:xmlData>
      </mets:mdWrap>
    </mets:digiprovMD>
  </mets:amdSec>

.. _mets-amdsec_bis:

La ``<dmdSec>`` de ce volume devra faire référence, à l’aide de l’attribut ``AMDID``, à l’`ID` spécifié dans la balise ``<amdSec>``.

*Exemple :*  
``<mets:dmdSec ID="IDLIVRE" ADMID="AMDID_XXX">``



