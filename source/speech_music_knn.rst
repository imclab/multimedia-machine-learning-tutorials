.. Segmentation d'un flux audio en parole ou musique par classification en k plus proches voisins

Segmentation d'un flux audio en parole ou musique par classification en k plus proches voisins
==============================================================================================

R�sum� 
-------

L'objectif de ces travaux pratiques sera donc la mise en oeuvre d'un segmenteur de flux audio capable de distinguer les segments de parole de la musique.
Pour cela nous aborderons l'utilisation du descripteur audio MFCC (Mel-frequency cepstrum [#mfcc]_)

Nous �tudierons l'impact du nombre de voisins k utilis� pour la classification. Nous r�aliserons ensuite un simple classifieur.

R�cup�ration des donn�es
------------------------
Pour la parole, nous allons utiliser le journal du jour d'une dur�e 
de 20 minutes, disponible en podcast sur le site de la radio France Culture. 

Pour les donn�es musicales, s�lectionner 5 � 6 titres de musique libres sur le portail d�di� `Jamendo <http://jamendo.com>`_ 

Une selection est disponible � cette `adresse <http://5c4m9.free.fr/courses/music.tar.gz>`_

T�l�charger les donn�es, en s'inspiirant des scripts donn�s en annexe
---------------------------------------------------------------------

Organisation des fichiers : 

:: 

  |-- data
  |   |-- music
  |   |   |-- 01_-_occidental_indigene_-_yaada.mp3
  |   |   |-- Balrog_Boogie.mp3
  |   |   |-- Dirty_angel.mp3
  |   |   |-- Hope.mp3
  |   |   |-- readme
  |   |   |-- yaada.mp3
  |   |   `-- zero-project-Infinity.mp3
  |   |-- query
  |   |   |-- 101216.mp3
  |   |   |-- both.mp3
  |   |   |-- music.mp3
  |   |   `-- speech.mp3
  |   `-- speech
  |       `-- 10060-21.12.2010-ITEMA_20259115-0.mp3


Extraction des descripteurs audio MFCC
--------------------------------------
A l'aide de la boite � outils spro,
nous allons extraire les coefficients MFCC. 

T�l�charger, configurer et compiler les outils spro
---------------------------------------------------
Les outils spro sont disponible sur la page d�di�e au projet de la forge INRIA, https://gforge.inria.fr/projects/spro


Pour chaque fichier audio, extraire les coefficients mfcc
---------------------------------------------------------

* **Transcodage MP3 > WAV** 

Avant le calcul des MFCC, les fichiers doivent �tre transcod�s au formant WAV[#wav]_ mono-canal, avec un d�bit de 16kB, et un �chantillonage � 16KHz. 
Pour cela utiliser **ffmpeg**, le couteau suisse multim�dia, via la commande suivante : 
::
      ffmpeg -i ifile.mp3 -acodec pcm_s16le -ar 16000 -ac 1 ofile.wav

* **Extraire les MFCC**

L'executable **sfbcep** produit les descripteurs mfcc pour chaque frame d'une dur�e de 20ms avec une p�riode de 10ms.
Il prend en entr�e les fichiers WAV et produit un fichier binaire.
L'utiliser avec les param�tres suivants : 
::
  sfbcep -f 16000 -p 14 -r 24 -e -Z -R -L 400 -D -A ifile.wav ofile.mfcc

* **Formatage ASCII des coefficients**

Pour les besoins du TP, nous transformerons les r�sultats binaires en ascii, via l'outil **scopy**

Algorithmes des k plus proches voisins
--------------------------------------

Impl�menter un objet knn
~~~~~~~~~~~~~~~~~~~~~~~~

Cet objet doit disposer des deux fonctions:

**Knn:**

* **fit(mfcc,class[0:speech,1:music])** fonction d'appentisage, elle sert uniquement dans cette algorithme a stocker les valeurs.

* **predict(mfcc,k)** fonction de calcul du plus proche voisins du param�tre 'mfcc', 'k' nombre de voisins utilis�s pour le vote. 

Etudier l'influence de la valeur k sur les performances
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Prener un �chantillon d'une seconde (100 descripteurs) de chaque classe et varier les valeurs de k et produire un graphique du r�sultat

Que pouvez-vous d�duire des r�sultats ?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Segmentation d'un flux
----------------------
Nous allons segmenter une �mission de radio intitul�e *L�-Bas si j'y suis*  et diffus�e sur France Inter le 16 d�cembre 2010.

Extraire trois segments de 10 secondes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Vos segments audio doivent d�buter � : 

* 0:01:20 : parole 
* 0:55:05 : musique
* 1:01:05 : parole puis musique 

Un exemple de script : 
.. literalinclude:: ../src/speech_music_knn/query.sh
   :language: bash 

D�terminer par tranche d'une seconde, la classe de chaque segment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Optimisation
------------
La cr�ation de plusieurs fichiers interm�diaires p�nalise fortement les performances de part le nombre �lev� d'entr�e-sortie. Elle complexifie �galement la gestion des fichiers et n�cessite d'utilisation d'espace disque. 

Utiliser les pipe Unix pour supprimer l'utilisation de fichier interm�diaires
------------------------------------------------------------------------------

Appendices
----------

Mise en oeuvre avec Python
~~~~~~~~~~~~~~~~~~~~~~~~~~
.. literalinclude:: ../src/speech_music_knn/speech_music.py

R�sultats
~~~~~~~~~

:: 

  Loading file,  10060-21.12.2010-ITEMA_20259115-0.mp3
  Loading file,  yaada.mp3
  Loading file,  01_-_occidental_indigene_-_yaada.mp3
  Loading file,  zero-project-Infinity.mp3
  Loading file,  Balrog_Boogie.mp3
  Loading file,  Dirty_angel.mp3
  Loading file,  Hope.mp3
  music | sample 0 | music | m[077] p[023]
  music | sample 1 | music | m[075] p[025]
  music | sample 2 | music | m[081] p[019]
  music | sample 3 | music | m[079] p[021]
  music | sample 4 | music | m[068] p[032]
  music | sample 5 | music | m[065] p[035]
  music | sample 6 | music | m[071] p[029]
  music | sample 7 | music | m[071] p[029]
  music | sample 8 | speech | m[048] p[052]
  *****************************************
  speech | sample 0 | speech | m[021] p[079]
  speech | sample 1 | speech | m[045] p[055]
  speech | sample 2 | speech | m[020] p[080]
  speech | sample 3 | speech | m[040] p[060]
  speech | sample 4 | speech | m[030] p[070]
  speech | sample 5 | speech | m[040] p[060]
  speech | sample 6 | speech | m[042] p[058]
  speech | sample 7 | speech | m[029] p[071]
  speech | sample 8 | speech | m[032] p[068]
  ******************************************
  both | sample 0 | speech | m[047] p[053]
  both | sample 1 | music | m[054] p[046]
  both | sample 2 | speech | m[045] p[055]
  both | sample 3 | speech | m[033] p[067]
  both | sample 4 | music | m[086] p[014]
  both | sample 5 | music | m[080] p[020]
  both | sample 6 | music | m[087] p[013]
  both | sample 7 | music | m[086] p[014]
  both | sample 8 | music | m[093] p[007]
  both | sample 9 | music | m[093] p[007]
  both | sample 10 | music | m[095] p[005]

.. rubric:: Liens 

.. [#mfcc] http://en.wikipedia.org/wiki/Mel-frequency_cepstrum
.. [#wav] http://en.wikipedia.org/wiki/WAV