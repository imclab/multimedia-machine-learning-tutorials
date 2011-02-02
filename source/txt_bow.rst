.. Text bag of word

Classification de documents textuels avec la m�thode des 'sacs de mots' et les s�parateurs � vaste marges
=========================================================================================================

Copier le jeu de donn�es Reuters
--------------------------------
Chaque nouvelle Reuters [#reuters_dataset]_ est class�e par cat�gorie/r�pertoire 

Pr�parer les donn�es 
--------------------
* Retirer la ponctuation du texte � l'aide de la commande tr.
Transformer les lettres majuscules en minuscules.
	
::

  r [:punct:] ' '
  tr [A-Z] [a-z]
	
* Normaliser les mots avec l'algorithme de stemming de Porter [#porter]_

  * compiler le fichier source porter.c 
  * utiliser : porter ./path/of/the/file 

Calculer le vocabulaire des mots les plus fr�quents :
-----------------------------------------------------

Variables n�cessaires :

* Une table de hashage globale pour compter les termes  
* Une table de hashage par document pour compter les termes  
* Un tableau de taille �gale au nombre de documents pour stocker les tables pr�cedemment d�crite
* Pour la d�tection des stop-words, utiliser une table de hashage pour compter le nombre de documents o� chaque terme est pr�sent.

Pseudo algorithme:
::
 
  tableau : term_counts_per_doc  
  hastab  : term_counts = {}
  hastab  : document_counts = {}

  Pour chaque document :
    hashtab : term_count_in_doc 
 
    Pour chaque mot dans le document:
      term_count_in_doc[term] += 1 
      term_counts[term] += 1
 
    Pour chaque terme dans la table :  term_count_in_doc
      document_counts[term] +=1 

Supprimer les stop-words
------------------------
Stop-words, mots pr�sents dans 80% des documents.


Conserver les N mots les plus fr�quents
---------------------------------------
Les sauvegarder dans un fichier texte (un terme par ligne) 
valeurs de N possibles : 500, 1000, 2000, 5000



A partir du vocabulaire, vectoriser les documents
-------------------------------------------------

:: 

  Pour chaque document : 
    Cr�er un vecteur vide de taille N (longueur du vocabulaire)
    Pour chaque terme dans le document : 
      hashtab : term_count_in_doc 
 
      Pour chaque mot dans le document:
        term_count_in_doc[term] += 1 

      Pour chaque term dans term_count_in_doc: 
        Si le terme est dans le vocabulaire :
	  i = numero de ligne du terme dans le vocabulaire
	  vecteur[i] = nombre de termes 
	
Classifier les donn�es
----------------------

* Entrainer le classifieur SVM et les diff�rents noyaux (rbf, linear, poly) avec les vecteurs d'entrainement. 
* Evaluer le taux d'erreurs avec les donn�es de tests

Produire la matrice de confusion pour le meilleur classifieur
-------------------------------------------------------------

Annexes
-------

Bag of words
~~~~~~~~~~~~
.. literalinclude:: ../src/txt_bow/bow.py  

Training
~~~~~~~~

.. literalinclude:: ../src/txt_bow/train.py

Classification 
~~~~~~~~~~~~~~

.. literalinclude:: ../src/txt_bow/classif.py


.. rubric:: Liens

.. [#reuters_dataset] http://disi.unitn.it/moschitti/corpora/Reuters21578-Apte-90Cat.tar.gz
.. [#porter] http://tartarus.org/~martin/PorterStemmer/