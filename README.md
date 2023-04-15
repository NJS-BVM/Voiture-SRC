# Cherokey 4WD Mobile Platform
Ce projet a été réalisé dans le cadre du module Systemes Robotisés Communicants en Licence 2 à l’Université Côte d'Azur.

# Description du projet
Aujourd'hui on va vous présenter le projet qu'on a réalisé en cours de systèmes robotisés communiquants. Pour ce projet on nous a fourni un dispositif cherokey et on devait écrire plusieurs codes pour pouvoir réaliser plusieurs tâches et on va vous présenter en quatre parties ce que l'on a effectué : dont trouver l'issue d'un labyrinthe, pouvoir télécommander cette voiture, suivre une route et retrouver une carte qui emmet des ondes Lora.

![IMG_20230125_170156](https://user-images.githubusercontent.com/127784182/231283957-0f7c9d0f-5690-4924-a7bf-8219262fbde1.jpg)


I/Tout d'abord on a commencé en travaillant sur la résolution d'un Labyrinthe grâce à un capteur ultrasonore :
Dans cette partie on a dû assembler une voiture et écrire un programme pour permettre à cette voiture de trouver un chemin dans un labyrinthe sans toucher les bords.

Pour effectuer cela on a monté la voiture, hors moteurs qui ont déjà été en place.
On a complété un programme qui permet de faire avancer et tourner la voiture, Pour que la voiture tourne il faut faire avancer les roues d'un côté et bloquer les roues de l'autre côté. On a aussi dû changer des délais pour que la voiture tourne à 90°.
Pour le programme du servo-moteur et du capteur qui est assemblé dessus, on devait écrire un code qui balaye un certain arc, enregistre la valeur de l'angle, et s'il repère un obstacle il décide de tourner à gauche ou à droite en fonction de la valeur de l’angle. On  a aussi changé la vitesse pour que le servo-moteur tourne à une vitesse adéquate, pour recevoir les informations moins vite et pouvoir éviter les obstacles efficacement, sans réagir trop vite. Le servo-moteur garde en mémoire la position dans laquelle il était pour réagir d'une manière adéquate a la situation. 

Cependant on a rencontré un problème durant cette partie, puisque ce système use rapidement la batterie alors on a dû la changer après s'être rendu compte que ça causait une incohérence par rapport au programme donné.

Finalement on a obtenu une voiture qui peut résoudre un labyrinthe assez serré sans problème et assez rapidement. En voici la présentation :

https://user-images.githubusercontent.com/127784182/232254390-d7910056-4b92-47e6-bdb7-7a0bebf553a1.mp4





II/Ensuite pour la deuxième partie de ce projet nous avons travaillé sur un programme permettant de télécommander la voiture par bluetooth grâce à l'application "Goble". On a pu réaliser cela puisque la carte peut être connectée au bluetooth. 

En effectuant cette partie, on a remarqué un fonctionnement par joystick sur l'application, où il y a deux axes, x en vertical et y en horizontal, sur l'axe x la voiture peut soit reculer soit d'avancer pour des valeurs comprises entre 1(recul) et 255(avance), et sur l'axe y la voiture va soit à droite soit à gauche pour des valeurs comprises entre 1(gauche) et 255(droite) et le point 128:128 est le milieu, ou la voiture ne bouge plus. 
Ces informations sont transmises via bluetooth a la carte Roméo BLE.
On a dû intégrer au code le programme pour faire avancer, reculer, tourner la voiture et des informations sur la direction et les vitesses. 
On a aussi fait en sorte de régler la vitesse de la voiture suivant si on était à la position maximale du joystick ou non. 
L'application était disponible sur Android et IOS, avec des interfaces différentes, sauf que la version Android était un peu moins facile à utiliser surtout pour l'arrêt de la voiture étant donné qu'on devait retrouver le milieu exact du joystick pour qu'elle s'arrête.

Finalement on a obtenu une voiture télécommandée par un téléphone qui effectue exactement ce que l'on veut par joystick jusqu'à l'augmentation de sa vitesse. En voici la présentation : 

https://user-images.githubusercontent.com/127784182/232251727-6b7297f3-1829-4280-8a73-28d97ea25409.mp4





III/Ensuite on devait manipuler une caméra qui permet de suivre une ligne grâce à des repères vectoriels, suivre une couleur, reconnaître un visage, un objet ou une couleur. Pour qu'elle sache quoi faire exactement on doit lui faire "apprendre" ce qu'elle doit suivre. On a utilisé cette caméra pour pouvoir faire une voiture qui suit une ligne, un objet et une couleur. L'intelligence se trouve dans la caméra car la carte qu'on utilise n'est pas assez puissante pour pouvoir faire cela.

Pour effectuer le suivi de ligne, on a travaillé avec un mode qui permet de repérer par des vecteurs sous forme de flèches sur l'écran une ligne à suivre, et renvoie l'origine de la flèche. La flèche suit la moyenne des lignes du circuit c’est-à-dire s'il y a un angle la flèche ne suivra pas parfaitement cet angle mais tournera d'une manière plus arrondie. Pour le code, on a baissé la vitesse de la voiture pour donner plus de temps à la caméra pour créer la trajectoire. Pour le côté manuel on a dû incliner légèrement la caméra pour ne pas avoir de lignes parasites que la caméra aurait pu suivre, sans trop la baisser pour qu'elle puisse avoir assez de temps pour prévoir la trajectoire s'il y a des angles droits. 
En voici la présentation :

https://user-images.githubusercontent.com/127784182/232253027-1f8a748c-b44f-47c1-84f9-f7255fdf5f84.mp4



Pour effectuer le suivi d'objets on a eu un problème de déplacement selon la position de l'objet sur l'écran, on a dû changer les valeurs du code de base. Pour analyser plus précisément le code dans certaines situations on a inséré des prints et on en a déduit que l'objet ne doit pas être trop près ~40 cm, sinon le programme bug.
Le suivi de couleurs marche sur le même principe que le suivi d'objets mais ici il suit une couleur précise qu'il détecte. 
En voici la présentation avec suivis d'objets : 

https://user-images.githubusercontent.com/127784182/232253471-dfdd43f2-8ec4-4cef-8527-36684830066b.mp4



Finalement on a pu effectuer une route avec un feu rouge et un feu vert en utilisant deux caméras puisqu'une seule caméra ne pouvait pas effectuer un suivie de ligne et de couleur en même temps. Alors on a branché d'un côté une caméra qui suit une ligne représentant la route et de l'autre côté une caméra qui distingue les couleurs. Ces deux caméras ont été branchés à deux adresses différentes, une sur le branchement de base 0x32 et la deuxième en I2C comme ça on peut les distinguer et manipuler les deux en même temps. Pour rajouter un feu vert et donc une autre couleur que le rouge on a créé une variable stockée de la dernière couleur aperçue; s'il avait détecté du rouge, la voiture reste arrêtée, s'il détecte du vert la voiture avance. En voici la présentation :

https://user-images.githubusercontent.com/127784182/232253787-6cb23b15-1874-41ea-987d-25507989b448.mp4




IV/La dernière partie de notre projet est d'obtenir un code qui permet à la voiture de retrouver une balise. On a utilisé deux cartes Lora, un récepteur branché sur la voiture qui allume les LEDs suivant la puissance reçue et un émetteur. 
Pour effectuer cette partie on a décidé que la manière la plus simple était que la voiture tournait en rond en se rapprochant de petit à petit de l'émetteur. Pour cela on a décidé que la voiture allait tourner tout le temps à gauche et quand elle apercevait une augmentation de la valeur reçue elle avançait et accélérait légèrement plus et quand l'émetteur est suffisamment proche du récepteur la voiture s'arrête grâce à un changement d'état dans le code.
Pour le code on a alors branché la carte sur 7 sorties digitales où on récupère la donnée et on la traduisit en binaire pour envoyer la donnée sur chaque bit de la sortie reliée à l'entrée sur la carte de la voiture on récupère et reconvertit le chiffre et si l'état est inférieur à un chiffre la voiture s'arrête.

Cependant on a rencontré un problème étant donné que les cartes n'apercevaient pas les bonnes informations puisque le problème était lié au blanchement sur la broche D13, qui était bloqué comme horloge et la broche D8, qui était bloqué comme reset. Donc, étant donné qu'il n'y avait pas d'autres pins disponibles, on a dû enlever 2 câbles et donc on n'a plus qu'une précision de 32 bits et non 128.

Finalement le code a marché et on a obtenu une voiture qui peut trouver une balise grâce aux ondes Lora envoyés par les cartes même si on a remarqué qu'il n'y a pas assez de précision et que la voiture tourne plus que prévu et atteint la carte moins rapidement. En voici la présentation :

https://user-images.githubusercontent.com/127784182/232253913-4a487686-56f7-4cae-b0eb-1fcbda7eb489.mp4

Pour conclure on a donc effectué pour ce projet plusieurs codes qui permettent à la voiture de résoudre un labyrinthe, d'être télécommandé, de pouvoir suivre une route avec des feux et même de retrouver une balise. C'était très intéressant de pouvoir manipuler cette voiture par ces différentes manières et cela nous a permis d'apprendre beaucoup de choses que ce soit en programmation ou en manipulation de différents composants qu'on a utilisés.
