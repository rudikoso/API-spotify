import requests
import json
from time import sleep

### token voor webex
accesToken = "Bearer NWY2NmI2YWEtYzM4Zi00NmFmLWE5OTEtNDE0YTkwYmE4YThjNDQwNDVmMDgtZjBm_PE93_9423ebf1-df21-4e89-8049-a08f8e9c5998"
headers = {"Authorization": accesToken}
###variabelen die gebruikt worden om identificatie en als URL van de huidige track
###omdat de URL en Spotify token verschillende keren nodig zijn, steek ik deze in een variabele
SPOTIFY_ACCESS_TOKEN = "BQB1ciYN21CdyjFkRbk7TUlJOIcCFhh1Tv5OY64tmtRElYO0hwWKdOqQCw2auJoeUMUzx5a0815vtvSdsYkiybZKJzLsLY2hVi8-S3GH5aP1zDAlZdti3lk5h7hg64GDOtajSgH8KI4URIwsaiC28VcsMIuwHaJH6S5R-RhI4ciHG4Fb"
SPOTIFY_GET_CURRENT_TRACK_URL = "https://api.spotify.com/v1/me/player/currently-playing"
##om niet altijd deze lange auth. in te geven bij de code is er een variabele aangemaakt
SPheaders = {"Authorization": f"Bearer {SPOTIFY_ACCESS_TOKEN}"}
r = requests.get("https://webexapis.com/v1/rooms", headers=headers)
jsonData = r.json()
rooms = jsonData["items"]
nameToSearch = "Zorrotje"
for room in rooms:
    if nameToSearch in room["title"]:
        print("Gevonden! " + room["title"])
        print()
        print("Als je wil beginnen typ dan /cat in de webex room zo krijg je een overzicht van de categorieën.")
        print()
        roomId = room["id"]
params = { "roomId": roomId}
while True:
    r = requests.get("https://webexapis.com/v1/messages", headers=headers, params=params)
    jsonData = r.json()
    messages = jsonData["items"]
    lastMessage = messages[0]["text"]

    if "/cat" in lastMessage:
        json_string = '''[geef een van deze categorieën op, voorafgaand met een /  "joke, artist, song, lyric" ]'''
        #cat = jsonData["json_string"]
        body = {"roomId": roomId, "text": json_string}
        # print(header)
        headers.update({"Content-Type": "application/json"})
        # post body in webex room
        post = requests.post("https://webexapis.com/v1/messages" , headers=headers, data=json.dumps(body))
        

    elif "/joke" in lastMessage:
        # vraag aan API voor joke
        r = requests.get("https://v2.jokeapi.dev/joke/Programming?type=single")
        jsonData = r.json()
        # print( jsonData )
        joke = jsonData["joke"]
        body = {"roomId": roomId, "text": joke}
        # print(header)
        headers.update({"Content-Type": "application/json"})
        # post body in webex room
        post = requests.post("https://webexapis.com/v1/messages" , headers=headers, data=json.dumps(body) )
        
    elif "/artist" in lastMessage:
        ##variabele met auth en URL die hierna bij de variabele jsonData wordt gestoken.
        ##Daarna kan ik uit de jsonData de nodige waarde uit de dictionary halen zonder telkens de URL en TOKEN te moeten ingeven
        r = requests.get(SPOTIFY_GET_CURRENT_TRACK_URL, headers=SPheaders)
        
        ##hier steek ik dus de vorige variabele in jsonData, de .json is om de data van json output te verkrijgen
        jsonData = r.json()
        
        #Bron: https://www.youtube.com/watch?v=yKz38ThJWqE&list=PLj3VXTEtqaiO8onjiNxSWvsGXwuE-TxUz&index=42
        ##Variabelen om de naam van de artist uit de jsonData te halen
        artists = jsonData["item"]["artists"]
       
        ##Als er meer dan 1 naam is vermeld wordt deze toegevoegd en gescheiden door een komma
        artists_names = ', '.join([artist['name'] for artist in artists])
        
        ##Variable om nadien in de post te zetten, zodat de naam van artist/groep gepost wordt in webex
        INFO = {"roomId": roomId, "text": artists_names}
        headers.update({"Content-Type": "application/json"})
        
        #post data in webex        
        post = requests.post("https://webexapis.com/v1/messages" , headers=headers, data=json.dumps(INFO) )

    elif "/song" in lastMessage:
        ##variabele met auth en URL die hierna bij de variabele jsonData wordt gestoken.
        ##Daarna kan ik uit de jsonData de nodige waarde uit de dictionary halen zonder telkens de URL en TOKEN te moeten ingeven.
        r = requests.get(SPOTIFY_GET_CURRENT_TRACK_URL, headers=SPheaders)

        ##hier steek ik dus de vorige variabele in jsonData, de .json is om de data van json output te verkrijgen.
        jsonData = r.json()

        ##Variabelen om de naam van de naam van de song uit de jsonData te halen
        song = jsonData["item"]["name"]
        
        ##Variable om nadien in de post te zetten, zodat de tittel van de song gepost wordt in webex
        info = {"roomId": roomId, "text": song}
        headers.update({"Content-Type": "application/json"})
       
        #post data in webex
        post = requests.post("https://webexapis.com/v1/messages" , headers=headers, data=json.dumps(info) )
 

    ### om een lyric op te vragen       
    elif "/lyric" in lastMessage:  
        ##variabele met auth en URL die hierna bij de variabele jsonData wordt gestoken.
        ##Daarna kan ik uit de jsonData de nodige waarde uit de dictionary halen zonder telkens de URL en TOKEN te moeten ingeven.                 
        r = requests.get(SPOTIFY_GET_CURRENT_TRACK_URL, headers=SPheaders)
        jsonData = r.json()

        ##Terug variabele van artist en song zodat deze als variabele achter url komen
        ##Op deze manier gebeurd de zoekfunctie automatisch volgens het nummer dat wordt afgespeeld.
        artists = jsonData["item"]["artists"]
        artists_names = ', '.join([artist['name'] for artist in artists]) 
        song = jsonData["item"]["name"]

        ##request naar api voor lyric 
        r = requests.get("https://api.lyrics.ovh/v1/" + artists_names + "/" + song)
        jsonData = json.loads(r.content)
        
        ##Als er een lyric gevonden is wordt deze getoond in webex room
        if "lyrics" in jsonData:
            
            ##Variable om nadien in de post te zetten, zodat de lyric gepost wordt in webex
            lyrics = jsonData["lyrics"]
            INFO = {"roomId": roomId, "text": lyrics}
            
            # print(headers)
            headers.update({"Content-Type": "application/json"})
                 
            # print(body)       
            post = requests.post("https://webexapis.com/v1/messages" , headers=headers, data=json.dumps(INFO))
                     
        else:
            ##om na een timeout omfdat er geen ["lyric"] is gevonden terug verder te gaan met "/lyric"
            lastMessage = messages[-1]["text"]

            ##om ["error"] nu uit de jsondata te halen         
            Nolyrics = jsonData["error"]
            
            NoINFO = {"roomId": roomId, "text": Nolyrics}
            
            # print(headers)
            headers.update({"Content-Type": "application/json"})
              
            # print(body)       
            post = requests.post("https://webexapis.com/v1/messages" , headers=headers, data=json.dumps(NoINFO))
            ## om te controleren welke status getoond wordt wanneer er geen lyric is gevonden
            print("Status code:", r.status_code)

    else:
        print(".", end="")
    sleep(3)
