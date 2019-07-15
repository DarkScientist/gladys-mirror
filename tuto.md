
# Un assistant Gladys avec un miroir connecté !
Hello !
Vous avez rêvé d'avoir un assistant Gladys **et** un miroir connecté mais vous n'avez pas deux Raspberry Pi ? Alors cet article est fait pour vous !

## 1. Principe de fonctionnement
Pour aborder cette situation deux solutions s'ouvrent à nous :

 - Installer Raspbian Full (= interface graphique, etc) et installer par dessus Gladys et Magic Mirror (l'outil du miroir connecté) : ceci n'est pas pratique car l'on doit installer Gladys nous même et étant donné que Raspbian Full comporte beaucoup d'outils inutiles ce serait une perte de temps;
 - C'est alors que cette solution semble la meilleure : installer l'OS de Gladys 3 et rajouter par dessus une interface graphique puis le Magic Mirror.

## 2. Matériel
Voici la liste du matériel à avoir :
**Matériel "numérique"**:

 1. [L'image décompressée de Gladys 3](https://bit.ly/gladys-3-8-0-rev2-mirror-fr2)
 2. Un outil pour formater la carte SD comme [SD Memory Card Formatter par exemple](https://www.sdcard.org/downloads/formatter/)
 3. Un outil pour flasher l'OS comme [Win32Disk Imager par exemple](https://sourceforge.net/projects/win32diskimager/)
 4. Un outil SSH (nécessaire sur Windows seulement, les distributions Mac et Linux en possèdent déjà un). Je vous conseille [CMDER](https://cmder.net/) pour Windows.

**Matériel "physique"**;
 1. Un Raspberry Pi 3 B / B+
 2. Un adaptateur secteur vers mini-usb (un chargeur de téléphone fait l'affaire)
 3. Une carte micro-sd et un adaptateur si besoin pour la brancher sur votre ordinateur
 4. Un cable ethernet **si vous prévoyez de construire votre miroir près d'une box**. Je préfère utiliser le WiFi dans mon cas
 5. Un boitier pour Raspberry Pi avec de préférence des trous pour mettre des vis pour le fixer au cadre du miroir
 6. **Un moniteur de plus de 20 pouces**, sans quoi le miroir sera très petit
 7. Un cadre en bois : vous devrez l'assembler vous même afin que ce dernier fasse la taille de l'écran et s'adapte
 8. Un miroir sans tain : vous pouvez vous contenter d'un film de miroir sans tain que vous pouvez trouver dans des magasins de bricolage ou encore sur [CDiscount](https://www.cdiscount.com/maison/r-miroir+sans+tain.html) ou [Amazon](https://www.amazon.fr/DUTISON-Miroir-Fen%C3%AAtre-Adh%C3%A9sif-Maison/dp/B07S1NG34R/r)

## 3. Installation de la partie "logicielle"
Allez hop, on commence par le soft !
![l'adaptateur usb](https://github.com/DarkScientist/gladys-mirror/raw/master/img/adaptateur.jpg)
On branche l'adaptateur et on démarre SD Card Formatter.

![formatter](https://github.com/DarkScientist/gladys-mirror/raw/master/img/format.png)

On confirme l'opération et c'est bon ! Maintenant on flash Gladys 3.
![img](https://github.com/DarkScientist/gladys-mirror/raw/master/img/imager.png)
On sélectionne le fichier **.img** de gladys et on clique sur Ecrire. On confirme et on patiente...
On peut désormais brancher le Raspberry Pi.
## 4. Configuration du Raspberry Pi
### 1. Première configuration de la machine
Vous devez relier le Raspberry Pi en ethernet une première fois afin d'y avoir accès en ssh.
Je vous laisse suivre cette [page](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md) pour configurer le wifi.
Ensuite, tapez **sudo raspi-config**, naviguez jusqu'à Advanced Options et sélectionnez **Expand Filesystem**.
Quittez et redémarrez la bête.
Retapez **sudo raspi config** puis allez à **Change User Password** et mettez un nouveau mot de passe pour l'utilisateur "pi".

Maintenant, testez si Gladys marche. Tapez, dans la barre d'URL, l'adresse IP de votre machine.
Si vous voyez des messages de Gladys, c'est bon !! Vous pouvez désormais utiliser ce dernier normalement.
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

 Refaites un **sudo raspi-config**, allez dans Boot options puis Desktop/CLI puis sélectionnez Desktop Autologin. Ainsi, le RPI démarrera sur l'interface graphique. Quittez puis redémarrez.
 Refaites un **sudo raspi-config** puis allez dans Interfacing options puis VNC et activez le. Confirmez. Ceci va installer un serveur depuis lequel on pourra voir l'écran du raspberry pi à distance. Téléchargez ensuite [RealVNCViewer](https://www.realvnc.com/fr/connect/download/viewer/).
 Essayez de vous connecter via VNC Viewer. Vous verrez l'interface graphique !! Vous pouvez régler la résolution dans les paramètres via l'interface graphique du RPI.
 ### 3. Installer Magic Mirror
 Tapez cette commande :
 

    bash -c "$(curl -sL https://raw.githubusercontent.com/MichMich/MagicMirror/master/installers/raspberry.sh)"
Laissez faire le script d'installation, il s'occupe de tout !! Confirmez juste en tapant **Y** à chaque fois que le script vous le demande.

Cependant, le port par défaut du Magic Mirror est 8080, qui est aussi celui de Gladys. On va modifier ça dans les paramètres :

    cd /home/pi/MagicMirror/config
    nano config.js
Changez le port à 8090 et quittez en pressant Ctrl+X puis Y et entrée.
Retournez dans le dossier du miroir **/home/pi/MagicMirror** et exécutez : 

    DISPLAY=:0 npm start

Redémarrez ensuite le raspberry pi pour que **pm2** prenne le relais. Je vous laisse vous documenter pour savoir comment utiliser Magic Mirror.

Vue depuis VNC :

[vnc](https://github.com/DarkScientist/gladys-mirror/raw/master/img/vnc.png)

Voilà ! Le miroir est fini !! Il faut réaliser la partie "matérielle" maintenant, partie que je ne traiterai pas ici.

Je vous laisse !!
Si vous avez des problèmes ou voulez la partie réalisation matérielle, contactez moi sur le forum ou sur discord (DarkScientist_#9449).
