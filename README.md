# Dit is de eindopdracht voor Network programming essentials dat telt als examenopdracht ###

## Voor deze opdracht is het de bedoeling dat er minstens 2 API's worden aangesproken die samen in één broncode verwerkt zit.
Hiervoor maak ik al gebruik van een bestaande code die gebruikt is geweest tijdens de les als oefening.
Het gaat om de Joke API en deze ga ik combineren met SpotifyAPI.
Om de output te verkrijgen werk ik met webex teams.
Via bv /joke en /artist is het de bedoeling om de juiste API aan te spreken.
Documentatie voor Joke API vind je op "https://v2.jokeapi.dev", voor Spotify vind je de documentatie 
op dit adres "https://api.spotify.com".
Voor info over webvex maak ik gebruik van "https://webexapis.com".

## Ik heb de repository "herexamen---eindopdracht-rudikoso" van GIT gecloned naar mijn laptop om zo verder te werken en regelmatig de nodige aanpassingen in de broncode te uploaden.

Om Webex te gebruiken moet er iedere keer bij opstart van PC een Bearer Token toegevoegd worden tot je code.
Deze kan je vinden bij "Get Started" bij Webex developer Documentation.
Om gebruik te maken van Spotify API kan je de documentatie raadplegen van Spotify developer.
Er wordt aangegeven dat er altijd met een Token zal gewerkt worden al identificatie voor je eigen Spotify profiel.
Omdat ik de info wil verkrijgen van het nummer dat aan het spelen is in één van mijn Spotify apps zal ik gebruik maken van 
de token die ik verkrijg via "player - Get Currently Playing Track".
Omdat er bij "artist" soms een dubbele naam is van 2 of meerdere artisten wordt er een fout gegenereerd waardoor er geen output is 
in Webex room.  Hiervoor heb ik via dit kanaal https://www.youtube.com/watch?v=yKz38ThJWqE&list=PLj3VXTEtqaiO8onjiNxSWvsGXwuE-TxUz&index=42
een oplossing gevonden.

## Volgende wat we toevoegen is voor het opvragen van de tittel van de track via /song

Ook hier wordt er gewerkt met Spotify API en is de code volledig het zelfde enkel halen we het item "song" eruit en post ik dit in webex room.

# Extra toevoeging ###

Om het geheel wat spannender te maken ga ik nog een derde API toevoegen.
Dit zal de https://api.lyrics.ovh API worden.
Hiermee kan ik een lyric opvragen.  Er is geen extra Token nodig en de request is eenvoudig.
Je moet enkel de naam van de artist/groep en tittel van de song toevoegen.

### Basis script
### Bron: https://www.youtube.com/watch?v=TDODQ95MVl8&t=7s

### artist = input("type name of artist: ")
### song = input("type name of song: ")
### url = "https://api.lyrics.ovh/v1/" + artist "/" + song
### response = requests.get(url)
### data = json.loads(response.text)
### print(data["lyrics"])

Dit script loopt in de terminal zelf dus er zal veel aangepast worden zodat het in een room van Webex komt.

## automatisch artist en song toevoegen aan requests

Omdat het vorige script vast liep wanneer je /lyric had ingegeven, heb ik het script aangepast.
Wanneer je zelf een naam van artist en song had ingebracht werd dit in de terminal gepost en bleef je laatste post in 
de room op /lyric staan.  Je kon hierna geen andere categorie ingeven, enkel nog in de terminal een nieuwe request voor een lyric.

Het script is nu aangepast dat de variabele uit de spotify /artis en /song  worden toegevoegd aan de request voor een lyric.
Hierbij moet er niet meer gewerkt worden via de terminal en zal alles via Webex rooms verlopen.

## if en else toegevoegd aan "/lyric"

Omdat ik merkte dat bij sommige nummer het programma afsloot omdat er geen response was ben ik beginnen werken met if en else.
Met if wordt er gezocht naar ["lyrics"] in de jsondata, wanneer deze is gevonden zal de tekst worden gepost in webex room.
Met else zal er een post komen (No lyric found) die uit de ["error"] wordt gehaald.
Omdat het programma bleef vastlopen wanneer er toch geen lyric werd gevonden heb ik dit stukje toegevoegd

lastMessage = messages[-1]["text"]

Nolyrics = jsonData["error"]

Eigenlijk doet dit niet meer dan terug gaan naar de het laatste bericht in de webex room.  In dit geval is dit dus /lyric maar 
deze keer houdt het geen rekening met voorgaande ["lyrics"] en zoekt het achter ["error"] waardoor het programma niet meer vastloopt.
Als bevestiging waarom er geen lyric wordt getoond wordt de statuscode in de terminal geprint.

Om de code leesbaar te houden heb ik ook een variabele toegevoegd voor inloggen Spotify.

SPheaders = {"Authorization": f"Bearer {SPOTIFY_ACCESS_TOKEN}"}

Deze kan dan na de url toegevoegd worden als headers=SPheaders.

r = requests.get(SPOTIFY_GET_CURRENT_TRACK_URL, headers=SPheaders)

Er is ook een /cat toegevoegd, deze wordt geprint in de terminal zodat je weet welke categoriën er zijn.

## /cat is nu zichtbaar in webex room

Nu wordt er wanneer je het script start een melding gemaakt dat je in de webex room /cat kan ingeven om de catregorieën te zien.
Dit is handiger omdat je dan enkel eens het script runt alles kan doen via webex rooms.
