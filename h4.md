# Ympäristö

Käyttöjärjestelmä: Debian 13
Laitteisto: HP Pavilion, Oracle VirtualBox Manager  
Selain: Google Chrome
Verkko: NAT

# x)
- Ghidra on ilmainen ohjelma käänteismallintamiseen jossa ohjelmia tutkitaan ilman lähdekoodia.
- Videossa käytiin esimerkki tehtävässä läpi Ghidraa, että mitä ohjelma tekee ja muuttaa ohjelmankoodia helpompaan luettavaan muotoon eli decompile.
- Tärkein asia on Ghidran avulla voidaan selvittää ohjelman toimintaa pelkästään tutkimalla binääriä.


# a) Install Ghidra
- Päivitän ensin linuxin **sudo apt update** ja sitten lataan ghidran githubista sillä Debianin linux kirjastossa ei ole suoraan Ghidraa toisin kuin Kalissa. Joten ladataan JDK ja sitten githubista virallinen Ghidran zip tiedosto.

<img width="647" height="132" alt="image" src="https://github.com/user-attachments/assets/37147c4f-b8d4-4fc2-a247-17f674cfb6b1" />

# b) Rever-C
- Unzipataan ezbin-challenges kansio ja lähdetään tutkimaan tehtävää. Eli packd tiedostoa. Muokataan ensin tiedosto muotoon, että voidaan avata ghidralla se ja decompilata. Joten ensiksi **UPX**-komennolla unpackataan tiedosto ja ghidrassa otetaan **import file** ja valitaan kyseinen unpacked packd tiedosto.

<img width="787" height="113" alt="image" src="https://github.com/user-attachments/assets/64736cb3-c57f-4da8-b547-849c3610846a" />

<img width="771" height="198" alt="image" src="https://github.com/user-attachments/assets/9d6435f6-eccf-489f-8eed-a44f1e84cea4" />

- Kun Ghidrassa päästään **main** funktiota tutkimaan tarkemmin niin näemme decompile kohdasta koodina.

<img width="1043" height="413" alt="image" src="https://github.com/user-attachments/assets/46b930ce-d2e9-4dbf-8af4-c82374014f39" />

- Muokataan kuitenkin koodista pari muuttujaa helpommin ymmärrettäväksi.

````
int iVar1 -> int input - käyttäjän syöte
char local_28 -> char password_input - syöte vertaa oikeaan salasanaan
````

<img width="635" height="317" alt="image" src="https://github.com/user-attachments/assets/4fe61b9f-5da6-48cf-b6f3-c387ad45b5c3" />


- Koodissa näkee myös suoraan salasanan ja Teron FLAG:in.
- Elikkäs

````
FLAG{Tero-0e3bed0a89d8851da933c64fefad4ff2}
Salasana piilos-AnAnAs
````

- Kyseinen ohjelma toimii siis vertaamalla käyttäjän annetun syötteen oikeaan salasanaan.

# c) If backwards
- Tuodaan passtr tiedosto Ghidraan ja muokataan sen koodia.

<img width="625" height="322" alt="image" src="https://github.com/user-attachments/assets/774831b0-a3fb-4b5e-8939-4a2681dc1997" />

- Katsotaan tarkemmin listing osiosta **main function** toimintaa ja mikä vaikuttaa salasanan toimintaan ja kääntää sen väärinpäin. Muistan tunnille Teron käyneen **JNZ** ja **JZ** muuttajan eron.
````
JNZ = Jump if Not Zero
JZ = Jump if Zero
````
- Kyselin hieman Claude AI:lta mitä tuo oikein tarkoittaa ja sain selitykseksi, että **JNZ** tarkoittaa jos salasana on **VÄÄRIN** niin hypätään pois funktiosta. ja **JZ** toisaalta jos salasana on **OIKEIN** niin funktiosta hypätään pois.

<img width="367" height="21" alt="image" src="https://github.com/user-attachments/assets/7237d0de-8a34-4bcb-ba7d-e48efcacc099" />

- Muokataan tuo muuttuja toiseen muuttujaan.

<img width="373" height="17" alt="image" src="https://github.com/user-attachments/assets/1f7e8b3d-24e1-4fc1-8ffa-b73c466ab319" />

- Samalla huomataan, että decompile kohdasta koodi on muuttunut.

<img width="625" height="315" alt="image" src="https://github.com/user-attachments/assets/0c5c5149-83af-47d5-a2a9-11e4e7eeed17" />

- Lähdetään testaamaan tiedoston ohjelmaa.
- Exportataan muokattu passtr tiedosto ja tuodaan se ghidrasta **original file** formaatiksi ja nimen muutoksen kerana eli korjattu versio.

  <img width="370" height="35" alt="image" src="https://github.com/user-attachments/assets/ebce1636-7744-46b5-b9a9-a3c54792c9f0" />
- Annetaan tiedostolle tarvittavat oikeudet, että voidaan ajaa ohjelma.

- Testataan oikealla salasanalla ja sitten jollain keksityllä. Jolloin oikean pitäisi sano, että salasana on väärä ja keksityn salasanan antaa FLAG.
<img width="676" height="152" alt="image" src="https://github.com/user-attachments/assets/cbd7b988-d578-4569-b607-f6785f9a26cb" />

- Ohjelma toimii odotetusti väärinpäin.

# d) Nora CrackMe
- Ladataam tiedostot ja puretaan ne varmistetaan että crackme tehtävät toimivat.
<img width="703" height="177" alt="image" src="https://github.com/user-attachments/assets/7007ef1c-a231-47ba-a241-ce0731c98b7e" />

- **Make**-komennolla päivittyy crackme tehtävä ajantasalle ja toimintavarmuuden.
<img width="591" height="175" alt="image" src="https://github.com/user-attachments/assets/0a98026c-4dae-4db2-96fe-b840f70f6b50" />

# e) Nora Crackme01

- Avataan crackme01 tiedosto Ghidraan ja etsitään **main function** ja katsotaan decompileristä koodia.

<img width="472" height="426" alt="image" src="https://github.com/user-attachments/assets/59233fff-0c43-440a-918b-052f4c26396e" />

- Sieltä nähdään koodista salasana **password1** joten testaan sen ohjelmaan ja katsotaan tuleeko tulosta.

<img width="607" height="36" alt="image" src="https://github.com/user-attachments/assets/78d1135a-912d-4094-be3a-fb59309fef34" />

- Ohjelma toimii annetulla salasanalla!

# e) Nora crackme01e. Solve the binary.

- Lähestytään tehtävää samalla tavalla kuin aikasempaa eli ghidraan annettu crackme01e. tiedosto ja katsotaan decompileristä tietoa koodista.
- crackme01e koodi decompileristä näyttää tältä.

<img width="387" height="445" alt="image" src="https://github.com/user-attachments/assets/c36a74c7-f43c-4fde-9bc6-0fead729295f" />

- Huomataan tuo "slm!paas.k" salasana kohta joten testataan.

<img width="635" height="57" alt="image" src="https://github.com/user-attachments/assets/28eddaf7-6197-478b-903f-00e9a51069cc" />

- Laitetaan salasana hipsuihin ja testataan niin se on oikea salasana.

# f) Nora crackme02 tehtävä

- Ghidraan avataan crackme02 tiedosto ja muokataan muuttujia decompilerin avulla. Jäin tähän hetkeksi miettimään koodia ja siinä olevan funktion tapahtuvaa ketjureaktiota. Kysyin Claude AI:lta vinkkiä muuttujien muuttamiseen.

<img width="362" height="455" alt="image" src="https://github.com/user-attachments/assets/60f21f2f-6efc-4c87-aa68-db2f30019b40" />

- Muokattu muuttujien lopputulos decompilerissä. Ja koodissa huomaan, että salasana verrataan **char - 1 ** kohtaan eli joudun ASCII taulukolla laskemaan tai pythonilla jokaisen kirjaimen salasanasta.
- Hyödynnän pythonia niin **chr(ord('')-1)**-komennolla päästään hakemaan oikeat kirjaimet salasanasta. Se lasku tapahtuu **password1** kirjainten **-1** tulos.

<img width="700" height="385" alt="image" src="https://github.com/user-attachments/assets/f6875a62-6941-4fa7-80b9-22c548762116" />

- Laskemisen jälkeen saadaan sanaksi : "o`rrvnqc0". Laitetaan testiin tuohon crackme02 ohjelman salasanaksi.

<img width="625" height="86" alt="image" src="https://github.com/user-attachments/assets/47376500-be50-4293-8dbb-360f1d345ba2" />

- Annettu salasanalla saan ohjelman suoritettua! Joten se oli tällä selvä crackme02 tiedoston ohjelma.


# Lähteet

- Ghidra Scripting to Speed Up Reverse Engineering - https://www.youtube.com/watch?v=z7SO6CF3guE
- Karvinen, Tero Sovellusten hakkeroint - https://terokarvinen.com/application-hacking/#homework
- everything is open source if you know reverse engineering (hack with me!)  - https://www.youtube.com/watch?v=m0XAPRAOJ8A
- GHIDRA for Reverse Engineering (PicoCTF 2022 #42 'bbbloat') - https://www.youtube.com/watch?v=oTD_ki86c9I
