## Oma moduuli
Aivan en nyt ollut varam ymmärsinkö tehtävää oikein, tai etenkään sitä kuinka laaja tämän pitäisi olla.

Ajatuksenani oli tehdä ympäristö, jossa olisi hyödyllisiä ohjelmia kuten esim.vim,curl,fzf. Tämä olisi tarkoitus automatisoida, jotta se olisi helposti ohjattavissa salt masterilta minioneille.

Koska minulla oli valmiiksi luotuna vagrantkone, jota pystyin ssh yhteyden kautta käyttämään pääätin käyttää sitä.

### Dockerin asennus ja testailut
Aloitin testailemaan dockeria. Tämä vaihe ehkä vähemmän liittyy itse moduuliin, mutta rapostoin sen kuitenkin. 

Ensiksi asensin dockerin.

![docker](https://github.com/JohannaLap/Oma-moduuli/blob/main/docker.png)

Käynnistin ja varmistin, että docker on käynnissä.

![docker käynnistys ja varmistus](https://github.com/JohannaLap/Oma-moduuli/blob/main/docker%20k%C3%A4ynnistys%20ja%20varmistus.png)

Testasin ajaa ensimmäisen kontin.

![dockerhellowold](https://github.com/JohannaLap/Oma-moduuli/blob/main/docker%20helloworld.png)

Muokkasin Dockerfilen sisältöä, niin että viestinä tulee testiviesti. Määrityksissä on määritetty millä komennolla tapahtuu mitäkin (tässä esim, että run komento kirjoittaa viestin "testiviesti!". 

![](https://github.com/JohannaLap/Oma-moduuli/blob/main/docerfile%20m%C3%A4%C3%A4ritykset.png)

Kokeilin itse rakentaa docker imagen. ' -t testing ' komennossa luo imagelle siis nimen testing. Ja lopussa oleva piste määrittelee sen, että olemme valmiiksi oikeassa hakemistossa, jossa komento suoritetaan.

![testikuvan rakentaminen](https://github.com/JohannaLap/Oma-moduuli/blob/main/testikuvan%20rakentaminen.png)

Ajoin testi imagen. Tässä siis toteutuu nyt nuo aiemmin määritellyt asiat, eli run komennolla saadaan tuo testi-teksti tulostettua.

![testikuvanajo](https://github.com/JohannaLap/Oma-moduuli/blob/main/testikuvanajo.png)

Tästä on nyt takoitus sitten jatkaa kehittämään tätä niin, että saadaan luotua docker kontti, jossa on halutut ohjelmat. Ja sitten saataisiin se jaettua saltin kautta..katsotaan miten etenee ja onnistuuko! :) 

## Lähteet:
https://docs.docker.com/reference/cli/docker/container/ls/
https://hub.docker.com/_/hello-world
https://docs.docker.com/get-started/docker-overview/
https://docs.docker.com/reference/dockerfile/
https://docs.docker.com/get-started/docker-concepts/building-images/writing-a-dockerfile/
https://www.buildpiper.io/blogs/how-to-create-a-dockerfile/
Tehtävänanto: Karvinen 2024: https://terokarvinen.com/palvelinten-hallinta/
