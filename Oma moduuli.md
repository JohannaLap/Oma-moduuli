## Oma moduuli

Ajatuksenani oli tehdä ympäristö, jossa olisi hyödyllisiä ohjelmia kuten esim.vim,curl,fzf. Tämä olisi tarkoitus toteuttaa docker konttin avulla, ja sitten saltilla jakaa masterilta minionille.

Olin aiemmin aloittanut testailun dockerilla ja saltilla. 

Nyt kuitenkin kun olin jatkamssa työtäni virtuaalikoneiden kanssa tuli taas ongelmaa. 

Ensimmäinen ongelma tuli, kun minion koneella päivittäminen ei onnistunu. Osasin olettaa, että ongelma on päivämäärässä sillä päivä määrä nätty kirjautuessa hassulta. Määritin päivämäärän ja kellon ajan oikeaan, jonka jälkeen sain paketit päivitettyä.

![sudo apt update](https://github.com/JohannaLap/Oma-moduuli/blob/main/sudo%20apt%20update.png)

Saman tein master koneelle.

Tarkistin saltin käynnissä olon.

![minion status](https://github.com/JohannaLap/Oma-moduuli/blob/main/minion%20status.png)

Masterin kohdalla ilmeni ongelma. Koska olen koneelle testi kansioita jo tehnyt. Selvittelin mitä virhe voisi tarkoittaa ja tulin tulokseen, että musitin määrä oli myös tarkistaa (Out of memory, wikipedia; the kernel's command-line parameters)

![master status failed](https://github.com/JohannaLap/Oma-moduuli/blob/main/mater%20status%20failed%2C%20free%20-h.png)

Muistin määrän lisääminen olis siis tarpeen. Sammutin master koneen ja varmistin virtualboxista, että master on sammunut. 

![vagrant t001 sammutus](https://github.com/JohannaLap/Oma-moduuli/blob/main/vagrant%20t001%20sammutus.png)

![vagrant sammutettu](https://github.com/JohannaLap/Oma-moduuli/blob/main/vagrant%20sammutettu.png)

Käynnistin masterin uudelleen. 

![vagrant up](https://github.com/JohannaLap/Oma-moduuli/blob/main/vagrant%20up%20.png)

Varmistin saltin olevan aktiivinen myös masterilla.

![salt-master active](https://github.com/JohannaLap/Oma-moduuli/blob/main/salt-master%20active.png)

Jostain syystä testi pingi ei toiminut..

![test.ping](https://github.com/JohannaLap/Oma-moduuli/blob/main/test.ping.png)

Varmistin avainten tilanteen. Avaimissa näkyy minionilta hyväksytty avain. Kokeilin myös pingata minion konetta masterilta ja sekin toimi. Myös pingi minionilta masterille toimi. Tästä päättelin että ongelma lienee saltissa.

![avaimet masterilla ja ping](https://github.com/JohannaLap/Oma-moduuli/blob/main/avaimet%20masterilla%20ja%20ping.png)

Välissä olin kokeillut myös käynnistää saltin uudelleen, mutta se ei muuttanut mitään. Sitten kokeilin sammuttaa ja avata molemmat koneet uudelleen. Sitten ilmaantui ongelma minion koneen kanssa. Kone ei enää yksin kertaisesti käynnistynyt.
Päädyin tuohoamaan minion koneen ja luomaan sen uudelleen. Tämä ei kuitenkaan auttanut sillä näyttöön jäy vain pyörimään tuo sama näkymä ja lopulta time out.

![vagrant t002 ongelmia](https://github.com/JohannaLap/Oma-moduuli/blob/main/vagrant%20t002%20ongelma.png)

Tässä vaiheessa en ehtinyt enempää tätä asiaa selvittämään. Joudun jatkamaan siis plautus ajan umpeuduttua, ja toivon saavani tämän kuitenkin tehtyä. Täytynee yrittää vielä uudelleen tuota koneen uudelleen luontia ja selvittää mitä muuta olisi tehtävissä!

Ongelmien jatkuessa jouduin tuhoamaan kummankin virtuaalikoneen ja luomaan ne uudestaan.

Tästä pääsin jatkamaan vasta 6.12.

Eli nyt oli kaksi virtuaalikonetta. Testasin vielä että testi pingi toimii. 

![test ping masterilta minionille](https://github.com/JohannaLap/Oma-moduuli/blob/main/testping%20masterilta%20minionille.png)

Testasin ensiksi yksinkertaisella tiedostolla dockerin toiminnan vielä. Halusin testata tätä aluksi hyvin yksinkertaisella tavalla jotta näen toimiiko.
Testi sisältö näytti siis tältä:

![testidocker](https://github.com/JohannaLap/Oma-moduuli/blob/main/testidocker.png)

Dockerfilessä on määriteltynä siis mikä kuva ladataan (ubuntu), mikä tiedosto luodaan (teksti Testi), sekä millä komennolla tulostetaan teksti.

Rakensin dockerfilestä docker imagen. 

![dockerfile build](https://github.com/JohannaLap/Oma-moduuli/blob/main/dockerfile%20build.png)

Ajoin testi kontin. 

![run testidocker](https://github.com/JohannaLap/Oma-moduuli/blob/main/run%20testidocker.png)

Tässä vaiheessa olin jo minionillekin asentanut dockerin 'sudo apt-get install docker.io' . Asensin vielä puuttuvat riippuvuudet. Dockerin asennus minionille tosiaankin välttämätöntä jotta saadaan masterilta ajettua.

![tarvittavat riippuvuudet minionille](https://github.com/JohannaLap/Oma-moduuli/blob/main/tarvittavat%20riippuvuudet%20minionille.png)

Käynnistin testi kontin 'sudo salt t002 docker.run name=testi-docker image=ubuntu:latest cmd="echo Testi"'
Sitten ajoin käynnistys komennon. 

![docker luonti t002 ja start](https://github.com/JohannaLap/Oma-moduuli/blob/main/docker%20luonti%20t002%20ja%20start.png)

Huomasin testaillessa, että minionin tiedostoon on määriteltävä tiedostopolku, jotta masteri pystyy jakamaan tiedostot minionille. Tässä siis määrittelin kaksi eri polkua, koska dockeriin liittyvät tiedostot oli tarkoitus säilyttää omassa paikassaan. Samat polut oli masterille määritettynä.

![etcsaltminion](https://github.com/JohannaLap/Oma-moduuli/blob/main/etcsaltminion.png)

![etcsaltmaster](https://github.com/JohannaLap/Oma-moduuli/blob/main/etcsaltmaster.png)


Tein virallisen Dockerfilen jolla oli tarkoitus asentaa halutut ohjelmat. Tässä määriteltiin kontin käynnistys ja mitä halutaan asentaa.

![virallinen dockerfile](https://github.com/JohannaLap/Oma-moduuli/blob/main/virallinen%20dockerfile.png)


Rakensin docker imagen. Imagea rakentaessa komennolla määritellään hakemisto jossa Dockerfile on ja rakennetaan oma_moduuli niminen image.


![dockerkuvan rakentaminen](https://github.com/JohannaLap/Oma-moduuli/blob/main/dockerkuvan%20rakentaminen.png)
 
Kuvan rakennus ja kontin käynnistys tapahtui seuraavasti. Komennossa määäritellään että kontin nimeksi annetaan oma_moduuli_container ja käytetään aiemmin luotua imagea oma_moduuli. Komento käynnistää kontin taustalla.
![docker imagen ajaminen](https://github.com/JohannaLap/Oma-moduuli/blob/main/docker%20imagen%20ajaminen.png)




Loin docker3.sls tiedoston, jolla oli tarkoitus hallita docker imagen rakennusta ja kontin käynnistystä.

![docker3sls](https://github.com/JohannaLap/Oma-moduuli/blob/main/docker3sls.png)

Varmistin, että kontti näkyy masterilla. 

![docker kontti](https://github.com/JohannaLap/Oma-moduuli/blob/main/docker%20kontti.png)

Tämän jälkeen testastasin toimiiko homma. Oletuksena siis, että kontti käynnistyy ja docker image luodaan ja että määritellyt ohjelmat imlestyvät minionille.

![stateapply](https://github.com/JohannaLap/Oma-moduuli/blob/main/stateapply.png)

![dockerkontti2](https://github.com/JohannaLap/Oma-moduuli/blob/main/dockerkontti2.png)

Testasin vielä että halutut ohjelmat näkyvät minionilla.

![curl minion](https://github.com/JohannaLap/Oma-moduuli/blob/main/curl%20minionissa.png)
![fzf minion](https://github.com/JohannaLap/Oma-moduuli/blob/main/fzf%20minionilla.png)
![vim minion](https://github.com/JohannaLap/Oma-moduuli/blob/main/vim%20minionissa.png)


Yhteenvetona : 
- Dockerfileen määriteltiin mitkä ohjelmat halutaan asentaa konttiin. Tässä testattiin vim, curl ja fzf. 
- docker3.sls :
    - docker imagen rakennus komennolla docker build -t johon määritellään imagen nimi ja hakemistopolku
    - kontin luonti ja käynnistys. docker run -d --name johon määritellään juuri rakennetun imagen nimi jota halutaan käyttää
    - dockerfilen kopiointi masterilta minionille
 
- Lopussa vielä testattiin että halutut ohjelmat näkyivät minionilla.
 








## Lähteet:

Karvinen 2024 ; https://terokarvinen.com/palvelinten-hallinta/ 
Wikipedia, Out of memory : https://en.wikipedia.org/wiki/Out_of_memory
The kernel's command-line parameters : https://www.kernel.org/doc/html/latest/admin-guide/kernel-parameters.html#vm-oom-kill

https://hub.docker.com/search
https://docs.saltproject.io/en/latest/ref/modules/all/salt.modules.dockermod.html
https://docs.docker.com/reference/cli/docker/container/create/
https://docs.saltproject.io/en/latest/topics/tutorials/modules.html
https://timlwhite.medium.com/the-simplest-way-to-learn-saltstack-cd9f5edbc967
https://docs.saltproject.io/en/latest/topics/tutorials/docker_sls.html
https://docker-curriculum.com/
https://github.com/saltstack/salt/issues/27237
https://docs.saltproject.io/en/latest/topics/tutorials/docker_sls.html
https://github.com/saltstack-formulas/docker-formula
https://xebia.com/blog/combining-salt-with-docker/
https://hub.docker.com/

