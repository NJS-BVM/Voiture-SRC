# Voiture-SRC
Aujourd'hui on vas vous présenter le projet qu'on a réalisé en cours de système robotisés communiquants. Pour ce projet on nous a fourni un dispositif cherokey et on devait écrire plusieurs codes pour pouvoir réaliser plusieurs tâches et on vas vous présenter en quatres parties ce que l'on a effectué : dont trouver l'issu d'un labyrinthe, pouvoir télécommander cette voiture, suivre une  une route et retrouver une carte qui emmet des ondes bleutooh. ![IMG_20230125_170156](https://user-images.githubusercontent.com/127784182/231283957-0f7c9d0f-5690-4924-a7bf-8219262fbde1.jpg)


I/Tout d'abord on a commencé en travaillant sur la résolution d'un Labyrinthe grâce a un capteur ultrasonore:
Dans cette partie on a dû assembler une voiture et écrire un programme pour permettre à cette voiture de trouver un chemin dans un labyrinthe sans toucher les bords.

 Pour effectuer cela on a monté la voiture, hors moteurs qui ont déjà étés en place.
On a complété un programme qui permets de faire avancer et tourner la voiture, Pour que la voiture tourne il faut faire avancer les roues d'un seul côté. On a aussi du changer des délais pour que la voiture tourne a 90°.
Pour le programme du servo morteur et du capteur qui est assemblé dessus, on a changé la vitesse pour que le servo moteur tourne a une vitesse adéquate, pour recevoir les informations moins vite et pouvoir éviter les obstacles efficacement. Le servo moteur garde en mémoire la position dans laquelle il était pour
réagir d'une manière adéquate a la situation. 

Cependant on a rencontré plusieurs problèmes durant cette partie, puisque ce système use rapidement la batterie alors on a du la changer après s'être rendu compte que ça causait une incohérence par rapport au programme donné. On a aussi du raccorder la vitesse de rotation du servomoteur a la rotation de la voiture pour éviter des blocages liés au fait qu'il n'as pas le temps de tourner avant qu'il ne remarque encore une fois le même problème.

Finalement on a obtenu une voiture qui peut se résoudre un labyrinthe assez serré sans problèmes et assez rapidement, en voici la présentation :
https://user-images.githubusercontent.com/127784182/231291048-27291312-7802-4d3a-b18c-f4ac66f48377.mp4



2/Ensuite pour la deuxième Partie de ce projet on a travaillé sur un programme permettant de télécommander la voiture par bleutooh grâce à l'application "GoBle". On a pu réaliser cela puisque la carte peut être connecté au bleutooh. 

Pour en effectuant cette partie, on a remarqué un fonctionnement par joystick sur l'application, où il y a deux axes,x en vertical et y en horizontal, sur l'axe x la voiture peut soit reculer soit avancer pour des valeurs comprises entre 1(recul) et 255(avance), et sur l'axe y la voiture vas soit a droite soit a gauche pour des valeurs comprises entre 1(gauche) et 255(droite) et le point 128;128 est le milieu, ou la voiture ne bouge plus. Ces informations sont transmises via bluetooth a la carte Roméo Blé.
On a dû intégrer au code le programme pour faire avancer, reculer et tourner la voiture et des informations sur la direction et les vitesses. On a aussi réglé la vitesse de la voiture suivant si on était au bord du joystick ou non.

Finalement on a obtenu une voiture télécommandée par un téléphone qui effectue exactement ce que l'on veut par joystick jusqu'à l'augmentation de sa vitesse, en voici la présentation (montrer vidéo).




III/Ensuite on devait manipuler une caméra qui permet de suivre une ligne grâce à des repères vectoriels, suivre une couleur, reconnaître un visage, un objet ou une couleur. Pour qu'elle sache quoi faire exactement on doit lui faire "apprendre" ce qu'elle doit suivre. On a utilisé cette caméra pour pouvoir faire une voiture qui suit une ligne, un objet et une couleur. L'intelligence se trouve dans la caméra car la carte qu'on utilise n'est pas assez puissante pour pouvoir faire cela. 

Pour effectuer le suivi de ligne, on a travaillé avec un mode qui permets de repérer par des vecteurs sous forme de flèches sur l'écran une ligne a suivre, et renvoie l'origine de la flèche. La flèche suit la moyenne des lignes du circuit c'est a dire si il y a un angle la flèche ne suivras pas parfaitement cet angle mais tournera d'une manière plus arrondie. Pour le code, on a du changer la vitesse de la voiture pour donner plus de temps à la caméra pour créer la trajectoire. Pour le côté manuel on a dû incliner légèrement la caméra pour ne pas avoir de lignes parasites que la caméra aurait pu suivre, mais si on l'a baissait trop elle ne pouvais plus avoir assez de temps pour prévoir la trajectoire si il y avait des angles droits. 

Pour effectuer le suivi d'objets le code était assez similaire mais on a eu un problème de déplacement selon la position de l'objet sur l'écran, on a du changer les valeurs du code de base. Pour analyser plus précisément le code dans certaines situations on a inséré des print et on en a déduit que l'objet ne doit pas être trop près ~40cm, sinon le programme bug a cause d'un problème de vitesse.
Le suivi de couleurs marche sur le même principe que le suivi d'objets mais ici il suit une couleur précise qu'il détecte. 

Finalement on a pu effectuer une route avec un feu rouge et un feu vert en utilisant deux cameras puisque une seule caméra ne pouvait pas effectuer une suivie de ligne et de couleur en même temps. Alors on a branché d'un côté une caméra qui suit une ligne représentant la route et de l'autre côté une caméra qui distingue les couleurs. Ces deux caméras ont étés branches a deux adresses différentes, une sur le branchement de base 0x32 et la deuxième en I2C comme ça on peut les distinguer et manipuler les deux en même temps. Pour rajouter un feu vert et donc une autre couleur que le rouge on a créé une variable stockée de la dernière couleur aperçue; si il avait détecté du rouge, la voiture reste arrêtée, si il détecte du vert la voiture avance. En voici la présentation (montrer la vidéo).




IV/La dernière partie de notre projet est d'obtenir un code qui permets a la voiture de retrouver une balise. On a utilisé deux cartes Lora, un récepteur branché sur la voiture qui allume les LEDs suivant la puissance reçue et un emetteur. 
Pour effectuer cette partie on a décidé que la manière la plus simple était que la voiture tournait en rond en se rapprochant de petit a petit de l'émetteur. Pour cela on a décidé que la voiture allait tourner tout le temps a gauche et quand elle apercevait une augmentation de la valeur reçue elle avançait et accélérait légèrement plus et quand l'émetteur est suffisamment proche du récepteur la voiture s'arrête grâce a un changement d'état dans le code. 
Pour le code on a alors branché la carte sur 7 sorties Digitales où on récupère la donnée et on la traduit en binaire pour envoyer la donnée sur chaque bit de la sortie reliée a l'entrée sur la carte de la voiture on récupère et reconverti le chiffre et si l'état est inférieur a un chiffre la voiture s'arrête.

Cependant on a rencontré un problème étant donné que les cartes n'apercevaient pas les bonnes informations puisque le problème était lié au blanchement sur la broche D13, qui était bloqué comme horloge et la broche D8, qui était bloqué comme reset. Donc, étant donné qu'il n'y avait pas d'autres pin disponibles, on a dû enlever 2 bits a la précision et donc on n'as plus qu'une précision de 32bits et non 128. 

Finalement le code a marché et on a obtenu une voiture qui peut trouver une balise grâce aux ondes bleutooh envoyés par les cartes même si on a remarqué qu'il n'y a pas assez de précision et que la voiture tourne plus que prévu et atteint la carte moins rapidement, en voici la présentation (montrer vidéo).


Pour conclure on a donc effectué pour ce projet plusieurs codes qui permettent à la voiture de résoudre un labyrinthe, d'être télécommandé, de pouvoir suivre une route avec des feux et même de retrouver une balise. C'était très intéressant de pouvoir manipuler cette voiture par ces différentes manières et cela nous a permis d'apprendre beaucoup de choses que ce soit en programmation ou en manipulation de différents composants qu'on a utilisé.
