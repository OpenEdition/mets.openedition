[Draft] Import METS/TEI pour OpenEdition Books (Lodel 2) 
############################################

Description de l’archive ZIP contenant les fichiers
==============================================

À sa racine, l’archive ZIP doit contenir :

- **un répertoire** ``files`` contenant les illustrations utilisées dans les unités éditoriales, l’image de couverture du volume, ainsi que les images et fichiers annexes potentiels.  
  *Remarque :* pour les images utilisées dans les unités éditoriales, les formats admis par Lodel sont le JPG, le PNG et le SVG (ce dernier format étant non compatible pour les couvertures). Les images de couverture doivent, de plus, posséder une résolution de 300 DPI et mesurer au minimum 1400 pixels de large.

- **un répertoire** ``sources`` contenant les fichiers de chaque unité éditoriale au format XML TEI, Word le cas échéant, et PDF. Ces PDF (nommés « fac-similés » dans Lodel) doivent être en basse définition si le volume est issu de la numérisation ; dans les autres cas, il doit s’agir des PDF éditeurs en mode texte. Ce répertoire peut également contenir le PDF complet du volume. 

- **un fichier METS** nommé ``MANIFEST.xml``.

La suite de cette documentation présente le contenu du fichier METS.

Description du fichier METS décrivant l’ouvrage
==============================================

1. Trois sections sont obligatoires :

- ``<structMap>``

- ``<fileSec>``

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



Description de la section ``<structMap>``
-----------------------------------------------

La ``<structMap>`` décrit l’arborescence du volume. L’ordre des éléments dans la ``<structMap>`` définit l’ordre d’apparition de l’élément dans son parent (volume ou sous-partie) à l’import dans Lodel.

I. Les balises ``<mets:div>`` doivent s’imbriquer pour décrire l’arborescence volume/sous-parties/texte
***********************************************************************

Le contenu du livre est dans une balise englobante ``<mets:div>``, deux attributs sont obligatoires : 

- ``TYPE`` : dont la valeur doit être ``livre`` ;
- ``DMDID`` : dont la valeur doit être égal à l’identifiant de la ``<dmdSec>`` décrivant le volume.

1. Pour chaque balise ``<mets:div>`` enfant, un attribut est obligatoire :

- ``TYPE``  : voir la liste des types autorisés par Lodel pour la plateforme Books ci-dessous.

2. Pour chaque balise ``<mets:div>`` enfant, les attributs facultatifs sont :

- ``DMDID`` : doit être égal à l’identifiant de la ``<dmdSec>`` décrivant l’élément ``/mets:mets/mets:dmdSec/@ID`` (s’il n’y a pas d’élément ``<dmdSec>`` relatif à un élément ``<div>``, il ne faut pas indiquer cet attribut) ;
- ``LABEL`` : titre du document (facultatif mais très utile pour faciliter la lecture de la ``<structMap>`` et repérer les erreurs).

3. Les différentes versions des documents (.xml, .pdf, .docx), ainsi que les images et fichiers annexes potentiels, doivent être décrits dans la ``<structMap>`` avec la balise ``<mets:fptr>``. Le seul attribut obligatoire de ``<mets:fptr>`` est :

- ``FILEID`` : identifiant du fichier utilisé dans la section ``/mets:mets/mets:fileSec/mets:fileGrp/mets:file/@ID``.

II. La ``<structMap>`` doit refléter la structure complète du volume
***********************************************************************

Ce n’est pas un encodage de la table des matières.

Les volumes comportent souvent des incohérences entre la table des matières et le corps de l’ouvrage. Par exemple, pour les unités éditoriale, les titres de la table des matières peuvent différer de ceux indiqués en début d’unité. Il faut donc utiliser les titres des documents et des parties disponibles dans le corps du volume, et non ceux de la table de matières. La table des matières sert à comprendre la structure du volume, mais en cas d’incohérence entre la table des matières et le contenu du volume, c’est l’organisation du contenu du volume qui doit être conservée. Il peut y avoir une part d’interprétation pour faire des choix et décrire correctement cette structure.

III. Types autorisés pour **OpenEdition Books**, à indiquer dans la ``<structMap>``
***********************************************************************

**1. Types du modèle « textes »**

- ``pageliminaire`` ;
- ``avantpropos`` ;
- ``preface`` ;
- ``chapitre`` ;
- ``source`` ;
- ``postface`` ;
- ``bibliographie`` ;
- ``index`` ; 
- ``annexe``.

*Remarque :* le type ``index`` est réservé aux ouvrages issus de la numérisation ou à un index dans la version imprimée de l’ouvrage.   

**2. Types du modèle « publications »**

- ``livre`` ;
- ``souspartie``.

**3. Types du modèle « fichiers »**

- ``couverture1`` ; 
- ``facsimile`` ; 
- ``image`` ;
- ``fichierannexe``.

*Remarque :* au niveau du volume, OpenEdition Books admet les types ``couverture1``, ``facsimile``, ``image`` et ``fichierannexe``. Au niveau de l’unité éditoriale, OpenEdition Books admet le type ``facsimile`` et ``fichierannexe``.


Description de la section ``<fileSec>``
-----------------------------------------------

Chaque fichier contenu dans l’archive ZIP doit être décrit dans cette section dans une balise ``<mets:file>``. 

*Remarque :* les métadonnées des unités éditoriales ne sont pas nécessaires, car elles sont déjà incluses dans les fichiers TEI de ces unités éditoriales.

La balise ``<mets:fileSec>`` doit contenir au moins une balise ``<mets:fileGrp>`` avec un identifiant optionnel qui contient elle-même une balise ``<mets:file>``. 

*Remarque :* une balise ``<mets:fileGrp>`` est obligatoire pour la couverture, les fac-similés des unités ou celui du volume et les fichiers Word issus de la numérisation. Elle est également obligatoire pour les images et les fichiers annexes potentiels. Elle est cependant optionnelle pour les images présentes dans les unités éditoriales. 

**1. Les attributs obligatoires de** ``<mets:file>`` **sont :**

- ``ID`` : un identifiant unique ;
- ``MIMETYPE`` : le type MIME du fichier. 


**2. L’attribut obligatoire uniquement pour les fichiers qui sont insérés dans un champ Lodel :**

- ``USE`` : le nom du champ Lodel dans lequel le fichier doit être inséré.

Valeurs de ``USE`` à renseigner pour le ME (Modèle Éditorial) OpenEdition Books :

- couverture : ``document`` ;
- fac-similé : ``document`` ;
- fichier Word pour corrections : ``fichierwordnumerise``.

**3. Les attributs optionnels sont :**

- ``CHECKSUM`` : la valeur du checksum MD5 du fichier ;
- ``CHECKSUMTYPE`` : dont la valeur doit être ``MD5`` ;
- ``GROUPID`` : un identifiant permettant de repérer des éléments appartenant à un même groupe (par exemple : un même ``GROUPID`` pour les versions PDF, Word et XML TEI, ainsi que pour les images appelées dans le fichier XML TEI). 

La balise ``<mets:file>`` contient une balise ``<mets:FLocat>`` pointant vers le fichier. Deux attributs sont obligatoires pour cette dernière :

- ``LOCTYPE`` : dont la valeur est ``URL`` ;
- ``xlink:href`` : il s’agit du chemin relatif vers le fichier dans l’archive ZIP.

*Exemple pour le fac-similé d’un livre :*

.. code-block:: xml

  <mets:fileSec>
    <mets:fileGrp ID="pdf_files">
      <mets:file ID="ID_2001_05_1_pdf" USE="document" MIMETYPE="application/pdf">
                <mets:FLocat LOCTYPE="URL" xlink:href="sources/volume.pdf"/>
      </mets:file>
    </mets:fileGrp>
  </mets:fileSec>

.. _mets-facsimile:

Le fichier décrit ici est un PDF nommé « 7-24 ». L’identifiant de la section ``<fileSec>`` de ce PDF (soit « ID7-24-pdf1 ») est présent dans ``<structMap>`` :

.. code-block:: xml

  <mets:structMap>
    <mets:div TYPE="livre" DMDID="ID_2001_05_1">
      <mets:div TYPE="souspartie" LABEL="titre de la sous-partie" DMDID="ID_2001_05_1-section1">
        <mets:div TYPE="chapitre" LABEL="Titre du chapitre">
          <mets:fptr FILEID="ID7-24-tei1"/>
          <mets:fptr FILEID="ID7-24-pdf1"/>
        </mets:div>
      </mets:div>
    </mets:div>
  </mets:structMap>

.. _mets-descirption_fichier:

*Exemple pour la couverture d’un livre :*

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

.. _mets-couv:



Description de la section ``<dmdSec>``
-----------------------------------------------
I. Présentation des éléments de ``<dmdSec>``
***********************************************************************

Chaque élément ``<div>`` utilisé dans la ``<strucMap>`` peut être décrit dans une ``<dmdSec>`` en `MODS`_.

.. _MODS : http://www.loc.gov/standards/mods

La balise ``<mets:dmdSec>`` doit contenir un attribut obligatoire :

- ``ID`` : un identifiant unique correspondant à l’attribut ``DMDID`` utilisé dans la ``<structMap>``.

Les éléments ``<dmdSec>`` sont nécessaires pour : 

- tous les objets de classe « fichiers » : types ``couverture1``, ``facsimile``, ``image``, ``fichierannexe`` ; 
- et toutes les publications : ``livre`` et ``souspartie``

qui sont présents dans ``<structMap>`` et auxquels des métadonnées sont associées. 

Les métadonnées des types ``couverture1``, ``fichierannexe``, ``image`` et ``facsimile`` sont nécessaires mais réduites. Dans la plupart des cas il n’y aura que le titre. 

En revanche, les éléments ``<dmdSec>`` peuvent être omis pour les éléments de la classe « textes » (types ``chapitre``, ``preface``, etc.) ou pour les objets sans métadonnées.

Toutes les informations descriptives du volume doivent être présentées au format MODS dans l’élément METS suivant :
``/mets:mets/mets:dmdSec[@ID="IDNUMERO"]/mets:mdWrap/mets:xmlData``.

*Exemple pour le fac-similé du volume :*

.. code-block:: xml

  <mets:dmdSec ID="livre1-facsimile">
   <mets:mdWrap MDTYPE="MODS" MIMETYPE="text/xml">
     <mets:xmlData>
       <mods:titleInfo>
         <mods:title>Titre du fac-similé</mods:title>
       </mods:titleInfo>
     </mets:xmlData>
   </mets:mdWrap>
  </mets:dmdSec>

.. _mets-facsimile_volume:


Où l’``ID="XXXX"`` de cette section renvoie dans la ``<structMap>`` à ``<mets:div DMDID >``.

*Par exemple :*

.. code-block:: xml

  <mets:structMap>
    <mets:div TYPE="livre" DMDID="livre1>
      <mets:div TYPE="facsimile" LABEL="fac-similé" DMDID="livre1-facsimile">
       <mets:fptr FILEID="livre1-pdf"/>
     </mets:div>
   </mets:div>
  </mets:structMap>

.. _mets-structmap:

On retrouve l’identifiant de ce PDF (``FILEID="YYYY"``) dans ``<mets:fileSec>`` :

.. code-block:: xml

  <mets:fileSec>
    <mets:fileGrp ID="pdf_files">
      <mets:file ID="livre1-pdf" MIMETYPE="application/pdf" CHECKSUM="527373ff4d089fdf38d4d2794ecd8787" CHECKSUMTYPE="MD5">
        <mets:FLocat LOCTYPE="URL" xlink:href="livre1.pdf" />
      </mets:file>
    </mets:fileGrp>
  </mets:fileSec>

.. _mets-id:

*Remarque :* les métadonnées du type ``souspartie`` sont nécessaires, mais réduites. Dans la plupart des cas il n’y aura que le titre.  

XPath : ``//mods:titleInfo/mods:title``

*Par exemple pour une sous-partie :* 

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

.. _mets-couv_bis:

Lorsqu’est déclaré dans la ``<structMap>`` un élément de TYPE ``couverture1`` :

.. code-block:: xml

  <mets:structMap>
    ...
    <mets:div TYPE="couverture1" LABEL="Titre de l’image de couverture" DMDID="X">
     <mets:fptr FILEID="Y" />
    </mets:div>
    ...
  <mets:structMap>

.. _mets-couv_ter:

*Remarque :* Sur OpenEdition Books, tout élément de texte sans titre au début d’une sous-partie doit être traité en tant qu’unité documentaire différente avec un titre *ad hoc* (le type de document conseillé est ``avantpropos``).

II. Les éléments du format MODS
***********************************************************************

Dans ``<mets:dmdSec>``, il faut placer tous les éléments descriptifs du volume au format MODS.

■ **Titre du volume**  

XPath : ``./mods:titleInfo[not(@type)]/mods:title``

Il n'est pas possible d'ajouter des enrichissements typographiques dans le titre du volume.


■ **Sous-titre du volume**  

XPath : ``./mods:titleInfo/mods:subTitle``

Il n'est pas possible d'ajouter des enrichissements typographiques dans le sous-titre du volume.


■ **Titre(s) traduit(s) du volume**  

XPath : ``./mods:titleInfo[@type="translated"]/mods:title[@xml:lang="LANG-ISO639-1"]``  

Où LANG-ISO639-1 correspond à la langue du titre traduit selon la norme ISO639-1 (fr, en, de...). L’élément ``<mods:title>`` est répétable avec des valeurs de ``xml:lang`` différentes si le titre est traduit dans plusieurs langues.

■ **Auteurs**

XPath ``./mods:name``

- type d’auteurs (rôle) : ``./mods:name/mods:role/mods:roleTerm/[@authority="marcrelator"/text()``

Valeurs possible pour les types d’auteurs sur OpenEdition Books :

- ``aut`` : auteur ;
- ``pbd`` : directeur de publication ;
- ``edt`` : éditeur scientifique ;
- ``trl`` : traducteur.

■ **Informations sur les auteurs**

- nom de famille :  ``./mods:name/mods:namePart[@type="family"]`` ;
- prénom :  ``./mods:name/mods:namePart[@type="given"]`` ;
- affiliation : ``./mods:name/mods:affiliation`` ;
- e-mail : ``./mods:name/mods:nameIdentifier[@type="email"]`` ;
- ORCID : ``./mods:name/mods:nameIdentifier[@type="ORCID"]`` ;
- IDREF : ``./mods:name/mods:nameIdentifier[@type="IDREF"]`` ;
- description : ``./mods:name/mods:description`` 

La description peut contenir du texte brut ou des éléments HTML simples (``<p>``, ``<em>``, ``<strong>``, ``<br/>``, ``<i>``, ``<sub>``, ``<sup>``, ``<span style="font-variant:small-caps;">``). Pour permettre la validation XML du fichier METS, il faut placer le code HTML dans un CDATA.
*Exemple :* ``<![CDATA[ <p>Lorem <em>Ipsum</em></p> ]]>``.

■ **Autres métadonnées**

- résumé : ``./mods:abstract[@xml:lang="LANG-ISO639-1"]`` ;
- extrait : ``./mods:abstract[@type="excerpt"]`` ;
- nombre de pages : ``./mods:physicalDescription/mods:extent``
- langue : ``./mods:language[@usage="primary"]/mods:languageTerm[@type="code"][@authority="iso639-2b"]`` ;
- langue secondaire : ``./mods:language/mods:languageTerm[@type="code"][@authority="iso639-2b"]`` ;
- lieu édition : ``./mods:originInfo/mods:place/mods:placeTerm`` ;
- année édition  : ``./mods:originInfo/mods:dateIssued`` ;
- clé OpenEdition : ``./mods:identifier[@type="isbnhtml"]`` ;
- numéro : ``./mods:identifier[@type="issue number"]`` ;
- note de l’éditeur : ``./mods:note[@type="pbl"]`` ;
- note de l’auteur ou autrice : ``./mods:note[@type="aut"]``.

Les résumés, l'extrait ainsi que les notes de l'éditeur et de l'auteur ou autrice peuvent contenir du texte brut ou des éléments HTML simples (``<p>``, ``<em>``, ``<strong>``, ``<br/>``, ``<i>``, ``<sub>``, ``<sup>``, ``<span style="font-variant:small-caps;">``). Pour permettre la validation XML du fichier METS, il faut placer le code HTML dans un CDATA.
*Exemple :* ``<![CDATA[ <p>Lorem <em>Ipsum</em></p> ]]>``.

■ **Licences**

Xpath : ``./mods:accessCondition``

Valeurs possibles pour OpenEdition Books :

- CC-BY-4.0 ;
- CC-BY-SA-4.0 ;
- CC-BY-NC-SA-4.0 ;
- CC-BY-ND-4.0 ;
- CC-BY-NC-4.0 ;
- CC-BY-NC-ND-4.0 ;
- OpenEdition Books License.

■ **Indexation**

Les éléments d’indexations doivent être placés dans des balises ``<mods:subjects>``, distingués par un attribut ``authority``.


- mots-clés en français : ``./mods:subject[@authority="motsclesfr"]/mods:topic`` ;
- keywords : ``./mods:subject[@authority="motsclesen"]/mods:topic`` ;
- index BISAC : ``./mods:subject[@authority="bisac"]/mods:topic`` ;
- index OpenEdition : ``./mods:subject[@authority="openedition"]/mods:topic``.

Sous réserve d'activation de nouvelles langues :

- parole chiave : ``./mods:subject[@authority="motsclesit"]/mods:topic`` ;
- schlagwortindex : ``./mods:subject[@authority="motsclesde"]/mods:topic`` ;
- palabras claves : ``./mods:subject[@authority="motscleses"]/mods:topic`` ;
- palavras chaves : ``./mods:subject[@authority="motsclespt"]/mods:topic``.

Dautres langues sont disponibles. Vous pouvez vous rapprocher de notre équipe pour tout renseignement complémentaire.


``<amdSec>``
-----------------------------------------------

Il faut renseigner une balise ``<amdSec>`` si l’on souhaite indiquer la méthode utilisée pour l’encodage :

- ``born digital`` : ressource créée sous forme numérique et destinée à rester sous forme numérique. 
- ``reformatted digital`` : ressource créée par la numérisation d’une ressource analogique originale (à utiliser pour l’OCR notamment).

.. code-block:: xml
  <mets:amdSec>
    <mets:digiprovMD ID="AMDID_XXX">
     mets:mdWrap MDTYPE="MODS" MIMETYPE="text/xml">
                <mets:xmlData>
                    <mods:physicalDescription><mods:digitalOrigin>reformatted digital</mods:digitalOrigin></mods:physicalDescription>
                </mets:xmlData>
            </mets:mdWrap>
    </mets:digiprovMD>
  </mets:amdSec>
.. _mets-amdsec:

La ``<dmdSec>`` de ce volume devra faire référence, à l’aide de l’attribut ``AMDID``, à l’``ID`` spécifié dans la balise ``<amdSec>``.

*Exemple :*  
``<mets:dmdSec ID="IDLIVRE" ADMID="AMDID_XXX">``
