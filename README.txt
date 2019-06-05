Vorbereitungen

@Betreuer: Eignene Benutzer fr jede Gruppe anlegen:
$ sudo adduser <username>
Passwort: ****

Benutzer Docker Nutzung erlauben
$ sudo usermod -aG docker <username>

Ordner vorbereiten
mkdir Notebooks
mkdir Data

@Daten von HDD auf SSD kopieren, damit Batch schnell erstellt wird:
cp -r /DATA/F-Praktikum/Data/* /home/<username>/Data*

Mit Account auf GPU-Server einloggen und Docker Container starten:
$ screen
$ docker run --runtime=nvidia --rm --name ${USER}_jupyter_tensorflow -u $(id -u):docker -v /home/fprak/Data:/DATA -v ${HOME}/Notebooks:/tf/F_Praktikum -it tensorflow/tensorflow:1.13.1-gpu-py3-jupyter
(Wenn die letzte Version ausprobieren mchte: tensorflow/tensorflow:latest-gpu-py3-jupyter)
Ctrl-a c

Information: Token unbedingt aufschreiben, wenn nicht geschehen:
Ctrl-a c
docker exec -it -u $(id -u):$(id -g) ${USER}_jupyter_tensorflow /bin/bash
jupyter notebook list
exit
exit

Optional: Tensorboard starten
$ docker exec -it -u $(id -u):$(id -g) ${USER}_jupyter_tensorflow /bin/bash
$ tensorboard --logdir tensorboard --logdir /tf/F_Praktikum/logs
Ctrl-a c

Die IP Adresse des Containers herausfinden und aufschreiben!
$ docker inspect ${USER}_jupyter_tensorflow | grep IPAddress


$ docker exec -it -u 0 ${USER}_jupyter_tensorflow /bin/bash
$ apt update
$ apt install libpng12-dev libtiff-dev libwebp-dev xcftools
$ pip install --upgrade pip
$ pip install imread scipy tables pyevtk scikit-image

Oder als Einzeiler;
apt update && apt install -y libpng12-dev libtiff-dev libwebp-dev xcftools && pip install --upgrade pip && pip install imread==0.7.0 scipy==1.2.1 tables==3.4.4 pyevtk==1.1.1 scikit-image==0.14.2 

auf Studierendencomputer (Putty muss installiert sein!):
plink.exe <username>@<GPU Server IP> -N -L 6006:<Docker Container IP>:6006 -L 8888:<Docker Container IP>:8888
