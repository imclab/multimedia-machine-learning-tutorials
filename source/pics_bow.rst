.. Classification d'objets dans les images avec les sacs de mots visuels et l'algorithme de classification SVM

Classification d'objets dans les images avec les sacs de mots visuels
=====================================================================

R�sum�
------
L'objectif de ces travaux pratiques est d'utiliser l'algorithme des SVM [#svm]_ (S�parateurs � Vaste Marge) pour la d�tection d'objets dans les images.
Pour cela travaillerons avec un descripteur local appel� SURF [#surf]_ .
Nous utiliserons ce descripteur avec la m�thode des sacs de mots [#bow]_ et l'algorithme associ� de r�duction de dimensions K-Means [#kmeans]_ .
Nous terminerons enfin par l'utilisation des SVMs pour d�terminer la cat�gorie de l'objet repr�sent� dans l'image. 

Corpus de donn�es
------------------
Nous utiliserons le corpus Caltech256 [#caltech]_ pour ce TP. Nous n'utiliserons que deux classes d'objets, les motocycles et les avions. 

Visionner les images associ�es � ces deux cat�gories
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Calcul d'un descripteur
-----------------------

Calculer les SURFs � l'aide du script surfs.py
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nous utiliserons la librairie OpenCV [#opencv]_ pour l'extraction des points d'int�r�t et leurs descriptions. 
A la fin du traitement, deux fichiers sont produits :
* moto
* plane

Chacun de ces fichiers est un dictionnaire python (hashtable) de type : cl� (nom du fichier image) : valeurs (tableau de vecteurs 128 dim)}

Ouvrir le fichier moto : 

:: 
   import pickle 
   moto = pickle.load(open('moto'))
   moto.keys()[0]
   moto.values()[0]


Dictionnaire visuel 
-------------------

Calculer le dictionnaire visuel � l'aide du KMeans
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Utiliser les param�tres suivants : 
* LEARN\_SIZE = 100
* K = 8
* ITER = 10 


Quantifier les vecteurs d'apprentissage et d'�valuation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Utiliser les 100 premi�res images pour l'�valuation. 

Classification
--------------

Avec les SVM, classer l'ensemble des donn�es d'�valution
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Donnner les mesures de rappel, pr�cision, F-mesure.

Avec les Random Forest, classer l'ensemble des donn�es d'�valution
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Donnner les mesures de rappel, pr�cision, F-mesure.


Variations
----------
Augmenter la taille du vocabulaire
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Quels observations peut-on faire ? 

Augmenter le nombre d'it�ration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Quels observations peut-on faire ? 

Augmenter la taille d'apprentissage
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Quels observations peut-on faire ? 

 
Calculer les histogrammes � l'aide du script histo.py
-----------------------------------------------------
A la fin du traitement, deux fichiers sont produits :
* moto
* plane

Recommencer les �tapes, kmeans, vectorisations et classification
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 

Annexes 
-------

R�cup�ration et formatage des donn�es
-------------------------------------
.. literalinclude:: ../src/pics_bow/Makefile
   :language: make	    

Calcul des SURFs
----------------
.. literalinclude:: ../src/pics_bow/surfs.py


Calcul du dictionnaire de mots
------------------------------
.. literalinclude:: ../src/pics_bow/kmeans.py 


Quantification des vecteurs
---------------------------
.. literalinclude:: ../src/pics_bow/assign.py

Classification des images
-------------------------
.. literalinclude:: ../src/pics_bow/classif.py


R�sultats : 
-----------

:: 

   In [4]: run classif.py
   Precision : 90.00% (360/400)


.. rubric:: Liens 
.. [#svm] http://fr.wikipedia.org/wiki/Machine_%C3%A0_vecteurs_de_support
.. [#surf] http://fr.wikipedia.org/wiki/Speeded_Up_Robust_Features
.. [#bow] http://fr.wikipedia.org/wiki/Sac_de_mots
.. [#kmeans] http://fr.wikipedia.org/wiki/K-means
.. [#caltech] http://www.vision.caltech.edu/Image_Datasets/Caltech256/
.. [#opencv] http://opencv.willowgarage.com/