# Passerelle Lora/Raspberry 
Ceci est la description de la mise en oeuvre d'une passerelle lora à base raspberry et module inAir9. Il se base generalement des traveaux et librairies de M.Congduc Pham.
à travers la conceptions jusqu'à arriver à piloter le gateway, nous verrons ici les differents etapes et codes utilisés pour arriver à celà.


###### Etape 1 liaison Raspberry_Lora.

pour avoir une liaison du Raspberry avec le module Lora, il faut connecter ce dernier au GPIO de Raspberry. Il suffit de connecter la broche SPI correspondante (MOSI, MISO, CLK, CS)comme indiqué ci-apres.

RPI                      Radio module
   GND pin 25----------GND   (ground in)
   3V3 pin 17----------3.3V  (3.3V in)
CS/CE0 pin 24----------NSS   (CS chip select in)
   SCK pin 23----------SCK   (SPI clock in)
  MOSI pin 19----------MOSI  (SPI Data in)
  MISO pin 21----------MISO  (SPI Data out)
  
 une image explicative montrant cette liaison a ete ajouté  ete ajoute dans le dossier images.

###### Etape 2 installation du systeme et mise en marche du gateway.

le systeme d'exploitation utilisé est Raspbian jessie se basant sur le systeme Rasbian mais avec quelques paquets d'installation deja ajoutes,ce systeme developpé par Pham est disponible et telechargeable sur le lien suivant: http://cpham.perso.univ-pau.fr/LORA/WAZIUP/raspberrypi-jessie-WAZIUP-demo.dmg.zip
ainsi, on  boute ce dernier  notre image de carte SD de 8 Go (minimum) afin d'effectuer l'installation de la passerelle à partir de cette image, Apres installation on inserre la carte SD sur notre raspberry et on allume. 
Notons que cette la distribution prend en charge Raspberry 1B +, ​​RPI2, RPI3, RPI0 et RPI0W. Pour RPI1, RPI0 et RPI0W, vous devez exécuter make lora_gatewayla version par défaut pour RPI2 et RPI3 .L'avantage majeure est qu'il prend charge un support Wi-Fi prêt à l'emploi pour RPI3 et RPI0W.En s y connectant sur ce point d'acces on peut avec une connection ssh (L'adresse du Raspberry reste 192.168.200.1) se connectant sur le gateway.

Avec les identifiants utilisateur suivant:

- login: pi
- password: loragateway

Une fois avoir avez flashé la carte SD avec image Raspberry jessie,une premiere connection s'agira de mettre à jour la passerelle, plusieurs options sont possibles et decrites sur le github de Pham,la plus simple et c'est celle qui est utilisé ici est de connecter son gateway sur internet en utilisant un cable Ethernet, et ainsi avoir l'interface d'administration Web ( en tapant l'adresse de gateway 192.168.200.1 sur le navigateur), 
la page web fourni en effet une administration de mise à jour assez simple en permettant nottament de mettre à niveau l'ID de Gateway et d'activer l'option PABOOST permettant d’avoir une application de puissance pour augmenter la portée, pour notre cas ce sera pas necessaire car cette option ne fonctionne pas avec les modules inAir4. 
source  : https://www.youtube.com/watch?v=mj8ItKA14PY

###### Etape 3 Premier essai de reception de données. 

les tests ont commencé en utilisant un simple programme Ping-Pong permettant d'envoyer le mot ping et la passerelle reponds pong, ce programme est téléchargé sur une carte Arduino.
Ensuite, dans le gateway on y execute la commande suivante > sudo ./lora_gateway en changeant au prealable la frequence utilisé sur 433.3 Mhz(Module inAir4) sur le fichier lora_gateway.cpp (on decommente #define Bande433 ) qui est le fichier de compilation pour ecouter les données envoyées par les noeuds ( cette modication à été faite sur le code publié).
Si celà ne permet pas  de recevoir les données envoyées, chose qui peut arriver, il suiffit d'eteindre le gateway et le rallumer par la suite afin qu'il prenne en compte les modifications faites, sinon il faut juste executer la commande :

> sudo ./lora_gateway --mode 1 --freq 433.3

On reçoit par la suite les données envoyées.

De la meme maniere pour des tests concernant des données recueillies par les capteurs comme un capteur de temperature par exemple.
Notons que le script post_processing.py publié sera edité pour gerer l'envoie des données reçus dans une base de données locale, avant l'envoie au niveau d'un serveur.



