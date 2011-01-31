.. Tutorial on Self-Organizing Map for pictures classification.


Classification non-supervis�e d'images par carte auto-adaptative
================================================================

R�sum�
------
L'objectif de ces travaux pratiques est d'utiliser une m�thode descriptive, la carte auto-adaptative [#wikipedia_som]_ , pour la classification et la visualisation de donn�es multidimensionnelles non-supervis�es. 
Nous �tudierons tout d'abord le comportement des param�tres d'apprentissage d'une carte pour la classification de couleurs.
Ensuite,nous utiliserons ces cartes pour la classification de paysages sous forme d'images fixes via le descripteur GIST � 960 dimensions.

Prise en main
-------------

Rappel de l'algorithme : 
  1. Initialiser chaque noeud de la grille.
  2. Un vecteur d'entr�e est choisi al�atoirement dans le jeu de donn�es d'entrainement et est pr�sent� au r�seau.
  3. Pour chaque noeud du r�seau, calculer la distance avec le vecteur pr�sent�. Le plus proche, le gagnant, est commun�ment appell� ``Best Matching Unit (BMU)``.
  4. Calculer le rayon de voisinage :math:`\sigma` du BMU.  Cette valeur, grande au d�part, diminue � chaque it�ration.  

:math:`\sigma(t) = \sigma_e^{(-t/\lambda)}`

Constante de temps : :math:`\lambda` = nombre d'it�rations / largeur de la carte

  5. Tous les noeuds pr�sents dans le rayon du BMU sont ajust�s pour
  ressembler au vecteur d'entr�e. Plus le noeud est proche du BMU, 
  plus ses poids sont alt�r�s.  

:math:`w_{r}^{t+1} = w_{r}^{t}+ \Delta w_{r}^{t}`

:math:`\Delta w_{r}^{t} =  \epsilon \cdot h \cdot (v - w_{r}^{t})`


Coefficient d'apprentisage : :math:`\epsilon = \epsilon(t) = \epsilon_0e^{(-t/\lambda)}`

Fonction de voisinage : :math:`h(r,s,t)= \exp \left( - \frac{\left\| \vec{r} - \vec{s} \right\|}{2 \sigma^{2}(t)} \right)`


  6. R�p�ter depuis l'�tape 2 pour N it�rations.


Utiliser le module python kohonen pour classer 6 couleurs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Les couleurs seront nos descripteurs multidimensionnelles (3 dimensions [r,g,b])

Nous entrainerons une carte initaliser al�atoirement avec 6 couleurs diff�rentes.
Vous devriez obtenir les r�sultats suivants : 

.. figure:: _static/color_map_init.jpg
   :scale: 50 %
   :alt: Avant apprentissage 
   :align: center 

   Avant apprentissage 	     

.. figure:: _static/color_map_final.jpg
   :scale: 50 %
   :align: center 
   :alt: Apr�s apprentissage 

   Apr�s apprentissage des 6 couleurs 

Faire varier les param�tres et commenter l'impl�mentation du module kohonen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* pas d'apprentisage
* constante de temps 
* rayon de voisinage 


Classification de paysages
---------------------------

Nous utiliserons le corpus `spatial envelope <http://people.csail.mit.edu/torralba/code/spatialenvelope>`_

T�l�charger les donn�es
~~~~~~~~~~~~~~~~~~~~~~~

Inspirez-vous du Makefile en annexe

Extraire les descripteurs GISTS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* Via python utiliser le module pyleargist : https://bitbucket.org/scamp/pyleargist
* Via l'application C++ : http://lear.inrialpes.fr/src/lear_gist-1.1.tgz

Utiliser le module Kohonen pour g�n�rer une classification des paysages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Le corpus pr�sente huit classes de paysage (mountain, open country, tall building, forest, coast, highway, inside city, street)

.. figure:: _static/gist_map.jpg
   :align: center 
   :alt: Classification des paysages

   Classification des paysages

Appendice
---------

R�cup�ration et formatage des donn�es 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


.. literalinclude:: ../src/pics_som/Makefile
   :language: make	    


Calcul des GISTS
~~~~~~~~~~~~~~~~

.. literalinclude:: ../src/pics_som/kohonen.py


Utiliser le script color_map.py pour cr�er des carte auto-adaptatives
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

python color_map.py -h
Usage: color_map.py [options]

Options:
  -h, --help            show this help message and exit
  -s SIZE, --size=SIZE  size of the map
  -i NBITER, --iter=NBITER
                        number of iteration
  -l LR, --lr=LR        learning rate

.. literalinclude:: ../src/pics_som/color_map.py


Classification des paysages
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. literalinclude:: ../src/pics_som/gist_map.py


Implementation des cartes de kohonen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. literalinclude:: ../src/pics_som/kohonen.py


.. rubric:: Notes

.. [#wikipedia_som] http://fr.wikipedia.org/wiki/Carte_auto_adaptative