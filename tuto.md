
# Un assistant Gladys avec un miroir connect� !
Hello !
Vous avez r�v� d'avoir un assistant Gladys **et** un miroir connect� mais vous n'avez pas deux Raspberry Pi ? Alors cet article est fait pour vous !

## 1. Principe de fonctionnement
Pour aborder cette situation deux solutions s'ouvrent � nous :

 - Installer Raspbian Full (= interface graphique, etc) et installer par dessus Gladys et Magic Mirror (l'outil du miroir connect�) : ceci n'est pas pratique car l'on doit installer Gladys nous m�me et �tant donn� que Raspbian Full comporte beaucoup d'outils inutiles ce serait une perte de temps;
 - C'est alors que cette solution semble la meilleure : installer l'OS de Gladys 3 et rajouter par dessus une interface graphique puis le Magic Mirror.

## 2. Mat�riel
Voici la liste du mat�riel � avoir :
**Mat�riel "num�rique"**:

 1. [L'image d�compress�e de Gladys 3](https://bit.ly/gladys-3-8-0-rev2-mirror-fr2)
 2. Un outil pour formater la carte SD comme [SD Memory Card Formatter par exemple](https://www.sdcard.org/downloads/formatter/)
 3. Un outil pour flasher l'OS comme [Win32Disk Imager par exemple](https://sourceforge.net/projects/win32diskimager/)
 4. Un outil SSH (n�cessaire sur Windows seulement, les distributions Mac et Linux en poss�dent d�j� un). Je vous conseille [CMDER](https://cmder.net/) pour Windows.

**Mat�riel "physique"**;
 1. Un Raspberry Pi 3 B / B+
 2. Un adaptateur secteur vers mini-usb (un chargeur de t�l�phone fait l'affaire)
 3. Une carte micro-sd et un adaptateur si besoin pour la brancher sur votre ordinateur
 4. Un cable ethernet **si vous pr�voyez de construire votre miroir pr�s d'une box**. Je pr�f�re utiliser le WiFi dans mon cas
 5. Un boitier pour Raspberry Pi avec de pr�f�rence des trous pour mettre des vis pour le fixer au cadre du miroir
 6. **Un moniteur de plus de 20 pouces**, sans quoi le miroir sera tr�s petit
 7. Un cadre en bois : vous devrez l'assembler vous m�me afin que ce dernier fasse la taille de l'�cran et s'adapte
 8. Un miroir sans tain : vous pouvez vous contenter d'un film de miroir sans tain que vous pouvez trouver dans des magasins de bricolage ou encore sur [CDiscount](https://www.cdiscount.com/maison/r-miroir+sans+tain.html) ou [Amazon](https://www.amazon.fr/DUTISON-Miroir-Fen%C3%AAtre-Adh%C3%A9sif-Maison/dp/B07S1NG34R/r)

## 3. Installation de la partie "logicielle"
Allez hop, on commence par le soft !
![l'adaptateur usb](https://github.com/DarkScientist/gladys-mirror/raw/master/img/adaptateur.jpg)
On branche l'adaptateur et on d�marre SD Card Formatter.
![formatter](https://github.com/DarkScientist/gladys-mirror/raw/master/img/format.png)
On confirme l'op�ration et c'est bon ! Maintenant on flash Gladys 3.
![img](https://github.com/DarkScientist/gladys-mirror/raw/master/img/imager.png)
On s�lectionne le fichier **.img** de gladys et on clique sur Ecrire. On confirme et on patiente...
On peut d�sormais brancher le Raspberry Pi.
## 4. Configuration du Raspberry Pi
### 1. Premi�re configuration de la machine
Vous devez relier le Raspberry Pi en ethernet une premi�re fois afin d'y avoir acc�s en ssh.
Je vous laisse suivre cette [page](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md) pour configurer le wifi.
Ensuite, tapez **sudo raspi-config**, naviguez jusqu'� Advanced Options et s�lectionnez **Expand Filesystem**.
Quittez et red�marrez la b�te.
Retapez **sudo raspi config** puis allez � **Change User Password** et mettez un nouveau mot de passe pour l'utilisateur "pi".

Maintenant, testez si Gladys marche. Tapez, dans la barre d'URL, l'adresse IP de votre machine.
Si vous voyez des messages de Gladys, c'est bon !! Vous pouvez d�sormais utiliser ce dernier normalement.
![start](https://github.com/DarkScientist/gladys-mirror/raw/master/img/debut.png)
### 2. Installer une interface graphique
Gladys vient sans interface graphique, on va donc devoir en installer une.
Allez, courage !
Tapez ces commandes dans l'ordre ([source](https://dadarevue.com/ajouter-gui-raspbian-lite/)):

    sudo apt-get update -y  
	sudo apt-get dist-upgrade -y
    sudo reboot
    //Patientez beaucoup x)
   
   

    sudo apt-get install --no-install-recommends xserver-xorg -y
    sudo apt-get install raspberrypi-ui-mods -y
    sudo reboot

 Refaites un **sudo raspi-config**, allez dans Boot options puis Desktop/CLI puis s�lectionnez Desktop Autologin. Ainsi, le RPI d�marrera sur l'interface graphique. Quittez puis red�marrez.
 Refaites un **sudo raspi-config** puis allez dans Interfacing options puis VNC et activez le. Confirmez. Ceci va installer un serveur depuis lequel on pourra voir l'�cran du raspberry pi � distance. T�l�chargez ensuite [RealVNCViewer](https://www.realvnc.com/fr/connect/download/viewer/).
 Essayez de vous connecter via VNC Viewer. Vous verrez l'interface graphique !! Vous pouvez r�gler la r�solution dans les param�tres via l'interface graphique du RPI.
 ### 3. Installer Magic Mirror
 Tapez cette commande :
 

    bash -c "$(curl -sL https://raw.githubusercontent.com/MichMich/MagicMirror/master/installers/raspberry.sh)"
Laissez faire le script d'installation, il s'occupe de tout !! Confirmez juste en tapant **Y** � chaque fois que le script vous le demande.

Cependant, le port par d�faut du Magic Mirror est 8080, qui est aussi celui de Gladys. On va modifier �a dans les param�tres :

    cd /home/pi/MagicMirror/config
    nano config.js
Changez le port � 8090 et quittez en pressant Ctrl+X puis Y et entr�e.
Retournez dans le dossier du miroir **/home/pi/MagicMirror** et ex�cutez : 

    DISPLAY=:0 npm start

Red�marrez ensuite le raspberry pi pour que **pm2** prenne le relais. Je vous laisse vous documenter pour savoir comment utiliser Magic Mirror.

Voil� ! Le miroir est fini !!

Je vous laisse !!
Si vous avez des probl�mes ou voulez la partie r�alisation mat�rielle, contactez moi sur le forum ou sur discord (DarkScientist_#9449).
