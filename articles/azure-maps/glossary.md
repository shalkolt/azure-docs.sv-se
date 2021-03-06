---
title: Azure Maps-ordlista | Microsoft Docs
description: En ordlista med vanliga termer som är associerade med Azure Maps, Location Based Services och GIS.
author: rbrundritt
ms.author: richbrun
ms.date: 09/18/2018
ms.topic: resource
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: d220fea64f860ebe5b660f7ebe5ad7db7aec4534
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46368858"
---
# <a name="glossary"></a>Ordlista

Här följer en lista över vanliga ord som används med Azure Maps.

## <a name="a"></a>A

<a name="address-validation"></a> **Åtgärda verifiering**: processen med att verifiera förekomsten av en adress.

<a name="advanced-routing"></a> **Avancerade dirigering**: en samling tjänster som utför avancerade åtgärder med hjälp av data på resande fot routning, till exempel beräkna nås intervall (isochrones), avstånd matriser och batch dirigerar begäranden.

<a name="aerial-imagery"></a> **Stadssilhuett imagery**: se [satellit bilder](#satellite-imagery). 

<a name="along-a-route-search"></a> **Längs en väg sökning**: en spatial fråga som söker efter data som ligger inom en angiven detour tid eller avståndet från en väg sökväg.

<a name="altitude"></a> **Höjd**: höjd eller lodrät höjning av över en referens för angrepp. Höjd mått baseras på en given referens datum, till exempel mean havsnivå. Se även höjning.

<a name="ambiguous"></a> **Tvetydig**: tillståndet hos osäkerheten i klassificering av data som finns när ett objekt på rätt sätt kan tilldelas två eller flera värden för en viss attributet. Till exempel när geokodning ”CA” två tvetydig resultaten returneras; ”Kanada” och ”Kalifornien” som ”CA” är en land och statliga kod för varje respektive. 

<a name="annotation"></a> **Anteckningens**: Text eller bilder som visas på kartan för att ange information för användaren. Anteckningen kan identifiera beskriver en specifik kartan entitet, ange allmän information om ett område på kartan eller ange information om kartan själva.

<a name="antimeridian"></a> **Antimeridian**: kallas även 180<sup>th</sup> Kartmeridian är den punkt där-180 grader och 180 grader longitud uppfyller. Vilket är motsatt nollmeridianen i världen.

<a name="application-programming-interface-api"></a> **Application Programming Interface (API)**: en specifikation som gör att utvecklare kan skapa program.

<a name="api-key"></a> **API-nyckel**: finns i Azure Maps-nyckel.

<a name="area-of-interest-aoi"></a> **Området av intresse (AOI)**: den utsträckning som används för att definiera en Fokusområde för produktion för en karta eller en databas.

<a name="asset-tracking"></a> **Spårning av tillgångar**: processen med att spåra platsen för en tillgång som en person, fordon eller annat objekt.

<a name="asynchronous-request"></a> **Asynkron begäran**: en HTTP-begäran som öppnar en anslutning och gör en begäran till servern som returnerar en identifierare för asynkron begäran stänger sedan anslutningen. Servern fortsätter att bearbeta begäran och du kan kontrollera statusen med identifierare. Begäran är när bearbetningen har slutförts, användaren kan sedan hämta svaret. Den här typen av begäran används vanligtvis för långvariga processer som körs.

<a name="autocomplete"></a> **Automatisk komplettering**: en funktion i ett program kan du förutsäga resten av ett ord som en användare skriver. 

<a name="autosuggest"></a> **Automatiska förslag i**: en funktion i ett program i förutsäger logiska möjligheterna för vad användaren skriver.

<a name="azure-location-based-services-lbs"></a> **Azure Location Based Services (LBS)**: det tidigare namnet på Azure Maps när den fanns i förhandsversion.

<a name="azure-maps-key"></a> **Azure Maps-nyckel**: An Azure Maps-nyckel är en unik sträng som används för att autentisera en användares Azure Maps-program eller tjänstbegäran. 

## <a name="b"></a>B

<a name="base-map"></a> **Grundläggande karta**: en del av ett program för kartan som visar bakgrunden referensinformation som vägar, landmärken och politiska gränser.

<a name="batch-request"></a> **Batchbegäran**: kombinera flera begäranden till en enskild begäran.

<a name="bearing"></a> **Med**: horisontellt för en punkt i förhållande till en annan återställningspunkt. Detta uttrycks som en vinkel i förhållande till norra från 0-grader till 360 grader mot i medsols. 

<a name="boundary"></a> **Gräns**: ett linje- eller polygon att separera angränsande politiska enheter, till exempel land, distrikt och egenskaper. En gräns är en rad som eventuellt kan inte följa fysiska funktioner, till exempel vattendrag, bergen eller väggar.

<a name="bounds"></a> **Gränser**: se [Bounding box](#bounding-box).

<a name="bounding-box"></a> **Angränsande ruta**: en uppsättning koordinater som används för att representera ett rektangulärt område på kartan. 

## <a name="c"></a>C

<a name="cadastre"></a> **Cadastre**: en post på registrerade mark och egenskaper. Se även [kolli](#parcel).

<a name="camera"></a> **Kamera**: I samband med en interaktiv karta kontroll, definierar en kamera maps visningsfält. Visningsområdet kamera fastställs baserat på flera Kartparametrar. Center, zoomningsnivån, försäljningsargument, betydelse. 

<a name="centroid"></a> **Axelcentroid**: geometriska mitten av en funktion. Axelcentroid för en linje är mittpunkten medan axelcentroid av en polygon mitten av området.

<a name="choropleth-map"></a> **Choropleth kartan**: ett flertal kartan som områden är skuggade i förhållande till ett mått på en statistisk variabel som visas på kartan. Till exempel färgläggning gränsen för varje amerikanska statliga baserat på dess relativa population till alla andra tillstånd.

<a name="concave-hull"></a> **Konkava skrov**: en form som representerar ett möjligt konkava geometri som placerar alla former i den angivna datauppsättningen. Formen genererade liknar wrapping data med form radbyte och uppvärmning den, vilket leder till stora sträcker sig över mellan pekar cave mot andra datapunkter.

<a name="consumption-model"></a> **Förbrukningsmodell**: Information som definierar den hastighet som förbrukar en vehicle bränsle eller elektricitet. Se även de [förbrukning modellen dokumentation](consumption-model.md).

<a name="control"></a> **Kontrollen**: en självständig eller återanvändbara komponent som består av ett grafiskt användargränssnitt som definierar en uppsättning beteenden för gränssnittet. Exempelvis kan är en kartkontroll vanligtvis delen av användargränssnittet som läser in en interaktiv karta.

<a name="convex-hull"></a> **Konvex skrov**: Fartygsskrov konvex är en form som representerar den minsta konvexa geometri som placerar alla former i den angivna datauppsättningen. Formen genererade liknar omsluta en elastisk band runt datauppsättningen.

<a name="coordinate"></a> **Samordna**: består av de longitud och latitud värden som används för att representera en plats på en karta.

<a name="coordinate-system"></a> **Koordinatsystem**: en referensram som används för att definiera positioner punkter i utrymmet i två eller tre dimensioner.

<a name="country-code"></a> **Landskod**: en unik identifierare för ett land baserat på ISO-standarden. ISO2 är en kod på två tecken för ett land (till exempel USA), vilket motsvarar en tre tecken för ISO3 code (till exempel USA).

<a name="country-subdivision"></a> **Land delfältet**: en första nivån delfältet i ett land, ofta kallat en region.

<a name="country-secondary-subdivision"></a> **Land sekundära delfältet**: en på den andra nivån delfältet i ett land, ofta kallat en region.

<a name="country-tertiary-subdivision"></a> **Land tertiär delfältet**: en på tredje nivån delfältet i ett land, vanligtvis ett namngivet område, till exempel en ward.

<a name="cross-street"></a> **Mellan gata**: en punkt där två eller flera gator överlappar varandra.

<a name="cylindrical-projection"></a> **Cylindriska**: en projektion som omvandlar punkter från en spheroid eller sphere på en tangent eller secant cylinder. Cylindern är sedan delas uppifrån och ned och förenklas till en plan.

## <a name="d"></a>D

<a name="datum"></a> **Datum**: referens-specifikationerna för en måttsystem som ett system av koordinat placerar på surface (en vågrät datum) eller höjd ovan eller under en yta (en lodrät datum).

<a name="dbf-file"></a> **DBF-filen**: ett databas-filformat som används i kombination med formfiler (SHP).

<a name="degree-minutes-seconds-dms"></a> **Grad minuter sekunder (DMS)**: måttenhet för att beskriva latitud och longitud. En grad är 1/360<sup>th</sup> för en cirkel. En ytterligare indelade i 60 minuter och en minut är uppdelad i 60 sekunder.

<a name="delaunay-triangulation"></a> **Delaunay triangulering**: en teknik för att skapa ett nät av sammanhängande, icke-överlappande trianglar från en datauppsättning punkter. Varje triangel circumscribing cirkel innehåller inga punkter i datauppsättningen i dess inre.

<a name="demographics"></a> **Demografi**: statistiska egenskaper (till exempel ålder, födelsedatum hastighet och inkomst) för människor.

<a name="destination"></a> **Mål**: en slutpunkt eller en plats där någon åker till.

<a name="digital-elevation-model-dem"></a> **Digitala höjning modell (DEM)**: en datauppsättning med utökade privilegier värden som är relaterade till en yta hämtats under ett område i regelbundna intervall med hjälp av en gemensam datum. DEMs används vanligtvis för att representera terräng befrielse.

<a name="dijkstra's-algorithm"></a> **Dijkstras algoritmen**: en algoritm som undersöker anslutning av ett nätverk för att hitta den kortaste vägen mellan två punkter.

<a name="distance-matrix"></a> **Avståndsmatris**: en matris som innehåller information om resa tid och avståndet mellan en uppsättning ursprung och mål. 

## <a name="e"></a>E

<a name="elevation"></a> **Höjning**: lodräta avståndet av en punkt eller ett objekt som ovan eller under en referens för angrepp eller datum (Allmänt mean havsnivå). Höjning avser vanligtvis mark höjd.

<a name="envelope"></a> **Kuvert**: se [Bounding box](#bounding-box).

<a name="extended-postal-code"></a> **Utökat postnummer**: ett postnummer kan innehålla ytterligare information. Till exempel i USA, postnummer har fem siffrorna men ett utökat postnummer kallas zip + 4, innehåller fyra ytterligare siffror. Dessa ytterligare siffror används för att identifiera ett geografiska segment i området femsiffrigt leverans som ett block med stad, en grupp med avdelningar eller en post office ruta som hjälper till med effektiv e sortering och leverans.

<a name="extent"></a> **Utsträckning**: se [Bounding box](#bounding-box).

## <a name="f"></a>F

<a name="federated-authentication"></a> **Federerad autentisering**: en autentiseringsmetod som ger en enda inloggning/autentiseringsmekanism som ska användas i flera webb- och mobilappar. 

<a name="feature"></a> **Funktionen**: ett objekt som kombinerar en geometri med en ytterligare metadata-information. 

<a name="feature-collection"></a> **Funktionen samling**: en samling funktionsobjekt.

<a name="find-along-route"></a> **Hitta längs vägen**: en spatial fråga som söker efter data som ligger inom en angiven detour tid eller avståndet från en väg sökväg.

<a name="find-nearby"></a> **Hitta i närheten**: en spatial fråga som söker ett fast linjär avstånd (som fågelvägen) från en plats.

<a name="fleet-management"></a> **Hantering av flottan**: hantering av nyttofordon, till exempel bilar, människor, levereras och plan. Hantering av vagnpark kan innehålla flera olika funktioner, till exempel vehicle finansiering, underhåll, telematik (spårning och diagnostik) samt driver-hastighet, bränsle, och hälso- och hantering. Hantering av vagnpark är en process som används av företag som förlitar sig på transport i sin verksamhet att minimera riskerna och minska sina totala kostnader för transport och personal med government lagstiftning.

<a name="free-flow-speed"></a> **Kostnadsfri flow hastighet**: kostnadsfri flow-hastighet som förväntat perfekt villkor. Vanligtvis hastighet gränsen.

<a name="free-form-address"></a> **Fri Formuläradress**: en fullständig adress som representeras som en enskild rad med text.

<a name="fuzzy-search"></a> **Fuzzy-sökning**: en sökning som hämtas i en sträng med text som kan vara en adress eller orienteringspunkt. 

## <a name="g"></a>G

<a name="geocode"></a> **Geocode**: en adress eller plats som har konverterats till en koordinat som kan användas för att visa den platsen på en karta. 

<a name="geocoding"></a> **Geokodning**: kallas även vidarebefordra geokodning, är processen att konvertera-adressen för platsdata till koordinaterna. 

<a name="geodesic-path"></a> **Geodesic sökväg**: den kortaste vägen mellan två punkter på en kurvor yta. Vid återgivning på Azure Maps visas den här sökvägen som en kurva på grund av Mercator-projektion.

<a name="geofence"></a> **Geofence-området**: A definierade geografiska region som kan användas för att utlösa händelser när en enhet anländer till eller finns regionen.

<a name="geojson"></a> **GeoJSON**: är en gemensam JSON-baserade filformat som används för att lagra geografiska vektordata som punkter, linjer och polygoner. **Obs**: Azure Maps använder en utökad version av GeoJSON som [dokumenteras här](extend-geojson.md).

<a name="geometry"></a> **Geometri**: representerar en spatialobjektet, till exempel en punkt-, linje- eller polygon.

<a name="geometrycollection"></a> **GeometryCollection**: en samling geometri objekt.

<a name="geopol"></a> **GeoPol**: refererar till geopolitiskt känsliga data, till exempel omtvistad kantlinjer och platsnamn.

<a name="georeference"></a> **Georeference**: processen med att justera geografiska data eller bilder till ett känt koordinatsystem. Den här processen kan bestå av affärerna, rotera, skalning eller skeva data.

<a name="georss"></a> **GeoRSS**: en XML-tillägg för att lägga till spatialdata RSS-flöden.

<a name="gis"></a> **GIS**: en förkortning för ”geografisk Information System”. En vanlig term som används för att beskriva mappning branschen.

<a name="gml"></a> **GML**: kallas även Geography Markup Language. Ett XML-filtillägg för att lagra spatialdata.

<a name="gps"></a> **GPS**: kallas även Global positionering System, är ett system med satelliter som används för att fastställa enheter position på jorden. Orbiting satelliter överföra signaler som tillåter en GPS-mottagare var som helst på jorden att beräkna en egen plats via trilateration.

<a name="gpx"></a> **GPX**: kallas även GPS utbytesformat, är en XML-filformatet som oftast skapas från GPS-enheter.  

<a name="great-circle-distance"></a> **Bra cirkel avståndet**: det kortaste avståndet mellan två platser på ytan en sfär.

<a name="greenwich-mean-time-gmt"></a> **Greenwich Mean Time (GMT)**: när nollmeridianen som körs via Royal observationsorganet i Greenwich, England.

<a name="guid"></a> **GUID**: en globalt unik identifierare. En sträng som används för att unikt identifiera ett gränssnitt, klass, typbiblioteket, Komponentkategori eller post.

## <a name="h"></a>H

<a name="haversine-formula"></a> **Haversine formeln**: en gemensam formel som används för att beräkna bra cirkel avståndet mellan två punkter på en sfär.

<a name="hd-maps"></a> **HD maps**: också kallas hög Definition kartor, består av hög kvalitet väg nätverksinformation, till exempel lane märkning signering och riktning ljus krävs för att driva autonoma.

<a name="heading"></a> **Rubriken**: riktning något som pekar eller som riktas mot. Se även [med](#heading).

<a name="heatmap"></a> **Termisk karta**: en datavisualisering som representerar en uppsättning färger densitet punkter i ett visst område. Se även [prioriterade kartan](#thermatic-map).

<a name="hybrid-imagery"></a> **Hybridbilder**: satellit eller Flygfoto bilder som har väg data och etiketter som finns på den.

## <a name="i"></a>I

<a name="iana"></a> **IANA**: en förkortning för Internet Assigned Numbers Authority. En icke-vinstdrivande grupp som övervakar globala IP-adressallokering.

<a name="isochrone"></a> **Isochrone**: en isochrone definierar området där någon resa inom en angiven tid för läge transportläge i valfri riktning från en viss plats. Se även [nås intervallet](#reachable-range).

<a name="isodistance"></a> **Isodistance**: med en plats kan en isochrone definierar området där någon reser inom ett angivet avstånd för läge transportläge i valfri riktning. Se även [nås intervallet](#reachable-range).

## <a name="k"></a>K

<a name="kml"></a> **KML**: kallas även nyckelhål Markup Language, är ett gemensamt XML-filformat för att lagra geografiska vektordata som punkter, linjer och polygoner. 

## <a name="l"></a>L

<a name="landsat"></a> **Landsat**: Multispectral, jorden som kretsade satelliter har utvecklats av NASA som samlar in bilder av mark som används i många branscher som jordbruk, skogsbruk och kartografi.

<a name="latitude"></a> **Latitud**: vinkelavståndet mätt i grader från ekvatorn i en Nord eller södra riktning.

<a name="level-of-detail"></a> **Detaljnivå**: se [zoomnivå](#zoom-level).

<a name="lidar"></a> **Lidar**: förkortning för lätt identifiera och sträcker sig. En fjärr-sensor teknik som använder Laser för att mäta avstånd till reflekterande ytor.

<a name="linear-interpolation"></a> **Linjär interpolation**: uppskattning av ett okänt värde med hjälp av linjär avståndet mellan kända värden.

<a name="linestring"></a> **LineString**: en geometri som används för att representera en rad. Även känt som en polyline. 

<a name="localization"></a> **Lokalisering**: stöd för olika språk och kulturer.

<a name="logistics"></a> **Logistik**: processen att flytta personer, fordon, lagret eller tillgångar på ett samordnat sätt.

<a name="longitude"></a> **Longitud**: vinkelavståndet mätt i grader från nollmeridianen i en östra eller västra riktning.

## <a name="m"></a>M

<a name="map-tile"></a> **Mappa panel**: en rektangulär bilden som representerar en partition på en karta arbetsyta. Mer information finns i den [zoomningsnivåer och rutnät dokumentation](zoom-levels-and-tile-grid.md).

<a name="marker"></a> **Markörens**: också känt som en PIN-kod eller kartnålen, är en ikon som representerar en punkt-plats på en karta.

<a name="mercator-projection"></a> **Mercator-projektion**: en cylindrisk kartans projektion som blev standard kartans projektion för nautiska på grund av dess förmåga att representera rader självklart konstant kallas rhumb rader som direkt segment som sparar vinklar med meridianer. Alla fast kartan projektioner förvränger former och storlekar på kartan i förhållande till SANT layouten för jordytan. Mercator-projektion blir områden är långt från ekvatorn, så att mindre områden visas större på kartan som du hanterar poler. 

<a name="multilinestring"></a> **MultiLineString**: en geometri som representerar en samling LineString-objekt. 

<a name="multipoint"></a> **MultiPoint**: en geometri som representerar en samling av Point-objekt.

<a name="multipolygon"></a> **MultiPolygon**: en geometri som representerar en samling Polygon-objekt. Till exempel för att visa gränsen för Hawaii, varje ön skulle anges med en polygon och gränsen för Hawaii skulle därför vara en MultiPolygon.

<a name="municipality"></a> **Kommuner**: en stad eller ort. 

<a name="municipality-subdivision"></a> **Kommuner delfältet**: en uppdelning av en kommuner, till exempel en stadsdel eller lokalt namn, till exempel ”centralt”.

## <a name="n"></a>N

<a name="navigation-bar"></a> **Navigeringsfältet**: uppsättning kontroller på en karta som används för att justera zoomningsnivån, försäljningsargument, rotering och växla grundläggande kartskiktet.

<a name="nearby-search"></a> **Närliggande search**: en spatial fråga som söker ett fast linjär avstånd (som fågelvägen) från en plats.

<a name="neutral-ground-truth"></a> **Neutrala grunden sanningen**: en karta som återger etiketter i den region som representerar officiella språk och i lokala skript om de är tillgängliga.

## <a name="o"></a>O

<a name="origin"></a> **Ursprung**: en start- eller plats som en användare är.

## <a name="p"></a>P

<a name="panning"></a> **Panorering**: processen att flytta kartan i valfri riktning samtidigt som en konstant zoomnivå.

<a name="parcel"></a> **Kolli**: en rityta för mark eller egenskapen gräns.

<a name="pitch"></a> **Försäljningsargument**: mängden lutning kartan har i förhållande till lodrät där 0 tittar direkt ned på kartan.

<a name="point"></a> **Punkt**: en geometri som representerar en enskild position på kartan. 

<a name="points-of-interest-poi"></a> **Orienteringspunkter (POI)**: ett företag, landmärken eller vanliga plats i närheten.

<a name="polygon"></a> **Polygon**: en stabil geometri som representerar ett område på en karta. 

<a name="polyline"></a> **Polyline**: en geometri som används för att representera en rad. Även känt som en LineString. 

<a name="position"></a> **Position**: longitud, latitud och höjd (x, y, z-koordinater) för en punkt.

<a name="post-code"></a> **Skicka kod**: se [postnummer](#postal-code).

<a name="postal-code"></a> **Postnummer**: en serie bokstäver, siffror eller båda i ett visst format som används av posttjänster av ett land för att dela upp geografiska områden i zoner för att underlätta leverans av e-post.

<a name="prime-meridian"></a> **Nollmeridianen**: en verksamhetsspecifik longitud som representerar 0 grader longitud. I allmänhet longitudvärden minska resan i västlig riktning tills 180 grader och öka när du reser i easterly riktningar till-180-grader. 

<a name="prj"></a> **PRJ**: en textfil som ofta uppstår när en Formfilen-fil som innehåller information om det planerade koordinatsystemet datauppsättningen finns i.

<a name="projection"></a> **Projektion**: ett planerat koordinatsystem baserat på en karta projektion som färdas över Mercator, Albers samma område och Robinson. De ger möjlighet att projektet mappningar av sfärisk jordytan på ett tvådimensionellt Kartesiansk koordinat plan. Planerade koordinatsystem kallas ibland för kartan projektioner.

## <a name="q"></a>FRÅGOR OCH

<a name="quadkey"></a> **Quadkey**: ett base-4-adress-index för en panel i ett system för quadtree sida vid sida. Mer information finns i [zoomningsnivåer och rutnät](zoom-levels-and-tile-grid.md) dokumentationen för mer information.

<a name="quadtree"></a> **Quadtree**: en datastruktur som varje nod har exakt fyra underordnade. Sida vid sida-system som används i Azure Maps använder en quadtree struktur så att när en användare zoomar in en nivå, varje kartrutan delar i fyra underordnade paneler.  Mer information finns i [zoomningsnivåer och rutnät](zoom-levels-and-tile-grid.md) dokumentationen för mer information.

<a name="queries-per-second-qps"></a> **Frågor Per sekund (QPS)**: antalet frågor eller begäranden som kan göras inom en sekund till en tjänst eller plattform. 

## <a name="r"></a>R

<a name="radial-search"></a> **Radiell search**: en spatial fråga som söker ett fast linjär avstånd (som fågelvägen) från en plats. 

<a name="raster-data"></a> **Raster data**: en matris med celler (eller bildpunkter) som är ordnade i rader och kolumner (eller ett rutnät) där varje cell innehåller ett värde som representerar information, till exempel temperatur. Rasters omfattar digitala Flygfoto fotografier, bilder från satelliter, digitala bilder och skannade maps.

<a name="raster-layer"></a> **Raster layer**: en bildrutsskikt som består av rasterbilder.

<a name="reachable-range"></a> **Nå intervallet**: flera nås definierar området där någon resa inom en angiven tid eller avståndet för läge transportläge överföras i valfri riktning från en plats. Se även [Isochrone](#isochrone) och [Isodistance](#isodistance).

<a name="remote-sensing"></a> **Remote avkänning**: processen för att samla in och tolka sensordata från ett avstånd.

<a name="rest-service"></a> **REST-tjänst**: förkortningen REST står för Representational State Transfer. En REST-tjänst är en URL-baserad webbtjänst som förlitar sig på grundläggande webbteknik för att kommunicera med varandra, de vanligaste metoder som HTTP GET och POST-begäranden. Dessa typer av tjänster brukar mig mycket snabbare och mindre än traditionella SOAP-baserade tjänster.

<a name="reverse-geocode"></a> **Omvänt geokoda**: processen för att utföra en koordinat och avgör den adress som representerar på en karta.

<a name="reproject"></a> **Reproject**: se [omvandling](#transformation).

<a name="rest-service"></a> **REST-tjänst**: förkortning för Representational State Transfer. En arkitektur för att utbyta information mellan peer-datorer i en decentraliserad, distribuerad miljö. REST kan program på olika datorer kommunicera oberoende av ett operativsystem eller plattformen genom att skicka en begäran om Hypertext Transfer Protocol (HTTP) till en URL (uniform resource locator) och få tillbaka data.

<a name="route"></a> **Väg**: en sökväg mellan två eller flera platser, vilket kan också innehålla ytterligare information, till exempel instruktioner för waypoints längs vägen.

<a name="requests-per-second-rps"></a> **Begäranden Per sekund (RPS)**: se [frågor Per sekund (QPS)](#queries-per-second-qps). 

<a name="rss"></a> **RSS**: förkortning för verkligen enkelt syndikering, resurs beskrivning Framework (RDF) sammanfattning av webbplats eller sammanfattning av omfattande webbplats, beroende på källan. En enkel, strukturerade XML-format för att dela innehåll mellan olika webbplatser. RSS-dokument innehåller viktiga metadataelement som författare, datum, rubrik, en kort beskrivning och en hypertextlänk. Den här informationen hjälper dig att en användare (eller en RSS-utgivare-tjänsten) bestämmer vilka material som är värt att ytterligare undersökning.

## <a name="s"></a>S

<a name="satellite-imagery"></a> **Satellit bilder**: bilder som har tagits emot av plan och satelliter som pekar nedåt direkt.

<a name="software-development-kit-sdk"></a> **Ett software development kit (SDK)**: en samling av dokumentation, exempelkod och exempelappar för att en utvecklare som använder en API för att skapa appar.

<a name="shapefile-shp"></a> **Formfil (SHP)**: kallas även en ESRI-Formfil, är ett data storage vektorformat för lagring av plats, form och attribut för geografisk funktioner. En formfil lagras i en uppsättning relaterade filer.

<a name="spherical-mercator-projection"></a> **Sfärisk Mercator-projektion**: se [Web Mercator](#web-mercator). 

<a name="spatial-query"></a> **Spatial fråga**: en begäran till en tjänst som utför en åtgärd för spatial. Till exempel som en radiell sökning eller längs en väg sökning.

<a name="spatial-reference"></a> **Referens för spatial**: ett koordinat lokala, regionala eller globala prissystem används för att hitta exakt geografiska entiteter. Den definierar indikatorns koordinatsystem som används för att relatera kartkoordinater till platser i den verkliga världen. Spatial referenser se till att rumsliga data från olika lager eller källor kan integreras för korrekt visning och analys. Azure Maps använder den [EPSG:3857](https://epsg.io/3857) koordinera referenssystem och WGS 84 för inkommande geometridata. 

<a name="sql-spatial"></a> **SQL spatial**: refererar till den spatial hanteringsfunktioner som är inbyggda i SQL Azure och SQL Server 2008 och senare. Den här spatial funktionen är också tillgängligt som ett .NET-bibliotek som kan användas oberoende av SQL Server. Mer information finns i den [rumsliga Data (SQL Server) dokumentation](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) för mer information.

<a name="synchronous-request"></a> **Synkron begäran**: en HTTP-förfrågan öppnar en anslutning och väntar på svar. Webbläsare som begränsar antalet samtidiga HTTP-begäranden som kan göras från en sida. Om flera tidskrävande synkrona begäranden görs på samma gång, den här gränsen kan nås och begäranden vänta tills dess att en annan begäran har slutförts.

## <a name="t"></a>T

<a name="telematics"></a> **Telematik**: skicka, ta emot och lagra information via telekommunikation enheter tillsammans med påverkar kontrollen i fjärrobjekt. 

<a name="temporal-data"></a> **Den temporala data**: Data som specifikt refererar till gånger eller datum. Den temporala data kan referera till diskreta händelser, t.ex blixten träffar; Flytta objekt, till exempel tåg; eller upprepade observationer, till exempel antalet från trafik sensorer.

<a name="terrain"></a> **Terräng**: en del av mark med en viss egenskap, till exempel sandiga terräng eller terräng.

<a name="thematic-maps"></a> **Prioriterade maps**: ett flertal kartan är en enkel karta som görs enligt en temat om ett geografiskt område. Ett vanligt scenario för den här typen av kartan är att färg administrativa områden, till exempel länder baserat på vissa mått av data.

<a name="tile-layer"></a> **Bildrutsskikt**: ett lager som visas genom att sätta samman kartan paneler (rektangulär avsnitt) till ett kontinuerlig lager. Paneler är antingen raster bild paneler eller vector paneler. Raster bildrutsskikt är vanligtvis återges i tid och lagras som bilder på en server. Detta kan ta upp mycket lagringsutrymme. Vektor bildrutsskikt återges i farten klientprogrammet, därför lagringskraven för server-sida är mindre.

<a name="time-zone"></a> **Tidszon**: en region av världen där en enhetlig normaltid för juridiska, kommersiella och sociala. Tidszoner tenderar att följa gränserna för andra länder och deras underordnade enheter.

<a name="transaction"></a> **Transaktionen**: Azure Maps använder en transaktionell licensieringsmodell där;

- En transaktion skapas för varje 15 kartan eller trafik paneler som begärdes.
- En transaktion skapas för varje API-anrop till en av tjänsterna i Azure Maps t.ex Sök- eller routning.

<a name="transformation"></a> **Omvandling**: att konvertera data mellan olika geografiska koordinatsystem. Du kan till exempel ha vissa data som har registrerats i Storbritannien och baserat på OSGB 1936 geografiska koordinatsystem. Azure Maps använder den [EPSG:3857](https://epsg.io/3857) koordinaten referens system variant av WGS84. Därför för att visa data korrekt, måste ha dess koordinater omvandlas från ett system till ett annat.

<a name="traveling-salesmen-problem-tsp"></a> **Traveling säljare Problem (Telefontjänstprovider)**: Hamiltonian krets problem där en säljare måste få det mest effektiva sättet att besöka en rad steg, återgå sedan till startpositionen.  

<a name="trilateration"></a> **Trilateration**: process för att fastställa positionen för en punkt på jordytan med avseende på två andra punkter genom att mäta avståndet mellan alla tre punkter.

<a name="turn-by-turn-navigation"></a> **Aktivera genom att aktivera navigering**: ett program som innehåller vägen instruktioner för varje steg i en väg som användare närmar sig nästa manövrar.

## <a name="v"></a>V

<a name="vector-data"></a> **Vektordata**: koordinat-baserade data som representeras som punkter, rader eller polygoner.

<a name="vector-tile"></a> **Vektor panel**: en öppen data-specifikation för att lagra geospatiala vektordata med samma rutsystemet som kartkontrollen. Se även [bildrutsskikt](#tile-layer).

<a name="vehicle-routing-problem-vrp"></a> **Vehicle routning Problem (VRP)**: en klass av problem i vilken en uppsättning sorterad vägar för en vagnpark med fordon beräknas med i beräkningen som uppsättning restriktioner. Dessa villkor kan innehålla sådant som leverans tidsfönster, flera vägen kapaciteter och reser varaktighet begränsningar.

<a name="voronoi-diagram"></a> **Voronoi diagram**: en partition av utrymme i områden eller celler som omger en uppsättning geometriska objekt (vanligtvis punkt funktioner). Dessa celler eller polygoner, måste uppfylla villkoren för Delaunay trianglar. Alla platser inom ett område som är närmast objektet den omger än andra objekt i uppsättningen. Voronoi diagram används ofta för att avgränsa delar av inflytande runt geografiska funktioner. 

## <a name="w"></a>W

<a name="waypoint"></a> **Waypoint**: en waypoint är en angiven geografiska plats som definieras av longitud och latitud som används för navigering. Används ofta för att representera en punkt där någon navigerar en väg genom.

<a name="waypoint-optimization"></a> **Waypoint optimering**: processen för att ordna om en uppsättning waypoints att minimera den tid eller avståndet som krävs för att passera alla angivna waypoints. Kallas ofta den [Traveling säljare Problem](#traveling-salesmen-problem-tsp) eller [Vehicle routning problemet](#vehicle-routing-problem-vrp) beroende på komplexiteten för optimering.

<a name="web-map-service-wms"></a> **Web Service Map (WMS)**: WMS är en öppen geografiska Consortium (OGC)-standard som definierar avbildningsbaserad kartan tjänster. WMS tjänster tillhandahåller kartbilder för specifika områden inom en karta på begäran. Bilder inkluderar förväg renderade symbolik och kan återges i någon av flera namngivna format som definieras av tjänsten.

<a name="web-mercator"></a> **Web Mercator**: kallas även sfärisk Mercator-projektion är en liten variant av Mercator-projektion, en används främst i mappningen för webbaserade program. Den använder samma formler som standard Mercator-projektion som används för småskalig maps. Dock använda Web Mercator används sfärisk formler alls skalas medan storskaliga Mercator mappar vanligtvis elliptiska form av projektion. Skillnaden är syns i global skala, men gör mappningar av lokala områden att avvika något från SANT elliptiska Mercator mappar på samma skala.

<a name="wgs84"></a> **WGS84**: en uppsättning konstanter som används för att relatera spatial koordinater till platser på ytan på kartan. WGS84 datum är den standard som används av de flesta online mappning leverantörer och GPS-enheter. Azure Maps använder den [EPSG:3857](https://epsg.io/3857) koordinaten referens system variant av WGS84.

## <a name="z"></a>Z

<a name="z-coordinate"></a> **Z-koordinat**: se [höjd](#altitude). 

<a name="zip-code"></a> **Postnummer**: se [postnummer](#postal-code).

<a name="Zoom level"></a> **Zoomnivå**: Anger vilken nivå av information och hur mycket av kartan är synlig. När du zoomar hela vägen ut en nivå 0, fullständig världskarta blir ofta i vyn men visar begränsad information om till exempel Landsnamn kantlinjer och havet namn. När du zoomar in närmare till nivå 17, visas en del av ett par stad block med detaljerade vägen information i kartan. Mer information finns i den [zoomningsnivåer och rutnät](zoom-levels-and-tile-grid.md) dokumentation.

