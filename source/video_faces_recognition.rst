.. video face recognition

D�tection et reconnaissance de visages dans les vid�os
======================================================


Demonstration
-------------
.. raw:: html

  <iframe title="YouTube video player" class="youtube-player" type="text/html" width="480" height="390" src="http://www.youtube.com/embed/g2OkGh0x8iE" frameborder="0" allowFullScreen></iframe>


T�l�charger les donn�es d'apprentisage 
--------------------------------------

Nous utiliserons les donn�es du corpus : Labeled Faces in the Wild
http://vis-www.cs.umass.edu/lfw/lfw-funneled.tgz

Detecter les visages 
--------------------
Utiliser script facedetector bas� sur le detecteur Haar d'OpenCV :
::

  facedetector.py <chemin de l image>


Formater les donn�es d'apprentisage
-----------------------------------
Dupliquer le r�pertoire *training* en *training_preprocessed*
Pour chaque image du r�pertoire training_preprocessed, extraire le visage en une image de taille 64x64 et en niveau de gris.

Extraire des videos les visages d�tect�s
----------------------------------------
Avec FFMpeg, extraire quelques donn�es de test dans le m�me format que pr�cedemment


Calculer les composantes principales du jeu de donn�es
------------------------------------------------------
#. Calculer le visage moyen 
#. Utiliser le script pca.py pour extraire les composantes principales
#. Transformer vos images en descripteur 

Classification des visages 
--------------------------
#. R�aliser d'apprentisage avec l'algorithme des SVM 
#. Utiliser une grille de recherche pour optimiser les parametres 
#. Utiliser votre classifieur sur les donn�es provenants des vid�os



Annexe 
------

Commande GNU/Linux
~~~~~~~~~~~~~~~~~~
::
  convert image.jpg -crop 32x32+16+16 -colorspace gray -compress none -depth 8 test.pgm 
  man paste 

FaceDetector
~~~~~~~~~~~~
.. literalinclude:: ../src/video_faces_recognition/facedetector.py

FaceRecognizer
~~~~~~~~~~~~~~ 
.. literalinclude:: ../src/video_faces_recognition/facerecognizer.py


FaceDetector + sampler
~~~~~~~~~~~~~~~~~~~~~~
.. literalinclude:: ../src/video_faces_recognition/facedetect_pic.py

Demo
~~~~
.. literalinclude:: ../src/video_faces_recognition/demo.py

