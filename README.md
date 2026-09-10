# CatQuiz Minijocs

Web de jocs de paraules en català (i dos d'acció) fets amb Three.js, companya del canal de TikTok @catquiz.cat. Cada joc és un únic fitxer HTML: sense build, sense dependències, sense servidor.

## Com publicar-la
1. Puja aquesta carpeta a un repositori de GitHub.
2. Settings → Pages → Branch `main`, carpeta `/ (root)`.
3. L'`index.html` és la portada; els jocs són a `jocs/`.

## Com afegir un joc
- Posa el teu HTML dins de `jocs/`.
- A `index.html`, afegeix-lo a l'array `mesJocs` (fitxer, títol, descripció, emoji, color) o copia una carta.

## Jocs inclosos
| Fitxer | Joc | Tipus |
|---|---|---|
| jocs/esquiva.html | Esquiva! | acció |
| jocs/estrelles.html | Caça estrelles | acció |
| jocs/motle.html | Motle (Wordle en català) | paraules |
| jocs/penjat.html | Penjat 3D | paraules |
| jocs/anagrama.html | Anagrama | paraules |
| jocs/catala.html | Com es diu en català? | paraules |
| jocs/intrus.html | L'intrús | paraules |
| jocs/accents.html | Accents! | paraules |
| jocs/perdudes.html | Lletres perdudes | paraules |
| jocs/sopa.html | Sopa de lletres | paraules |

El vocabulari de cada joc de paraules és una constant al principi del seu `<script>`: amplia-la amb les paraules que vulguis (sense accents).

Three.js es carrega des d'unpkg amb un `importmap` (versió 0.160.0).

## Reel automàtic (per gravar vídeos verticals)
- Obre `demo.html`: un reel de ~28 s per joc (hook → joc jugant sol amb subtítols i zooms → CTA), en bucle. Tria el joc amb els botons o amb `?joc=motle`.
- Els subtítols de cada reel són a l'objecte `REELS` de `demo.html`: [segon, text, dalt|baix, zoom].
- Qualsevol joc també accepta `?demo` directament (p. ex. `jocs/motle.html?demo`) per gravar-lo sol.
- Per gravar: obre-ho a Chrome, F12 → mode dispositiu → 1080×1920 (o directament al mòbil en vertical) i grava la pantalla.

## Gravar els reels
A `demo.html`, marca «⏺ Grava el reel» abans de triar-lo: Chrome demana compartir la pestanya (tria «Aquesta pestanya» i marca «Compartir àudio»); el reel fa una sola passada, es retalla al rectangle 9:16 (Region Capture) i en acabar surt el botó de descàrrega (MP4 a Chrome/Edge). Fes la finestra alta i estreta perquè el rectangle sigui gran: la resolució del vídeo és la del rectangle en pantalla. Per URL: `demo.html?joc=motle&quiz&grava`.

## Pantalla fixa
Tots els jocs bloquegen el zoom (pinça i doble toc) i el desplaçament lateral; la portada només permet el desplaçament vertical.

## Format quiz (per al canal CatQuiz)
- `demo.html?joc=motle&quiz` (també `penjat`, `anagrama`, `catala`, `intrus`, `accents`, `perdudes` i `sopa`): el joc juga sol fins a mig camí, apareix "QUINA ÉS? Escriu-la als comentaris" amb compte enrere, i després el bot revela la solució amb confeti. Acaba amb la carta "L'has encertada?".
- La pausa la decideix cada joc (`?demo&quiz`): Motle després de 2 intents, Penjat amb la meitat de lletres vistes, Anagrama a la segona paraula.

## Generador de vídeos quiz (`video.html`)
Reprodueix el format dels vídeos de CatQuiz («Com diries en català?»): carta d'entrada, pregunta amb ❌, barra de temps, revelació amb ✅, cartes de «dóna-li al m'agrada» i «comenta», i tancament. Es dibuixa en un canvas de 1080×1920 i, amb la casella «Grava» marcada, es grava amb so mentre es reprodueix i en acabar surt el botó de descàrrega (MP4 a Chrome/Edge, WebM a Firefox). Quatre modes (castellanismes, accents, intrús, paraula amb forats), 4 paletes, nombre de preguntes i segons configurables. `video.html?mode=catala&n=10&seg=7&pal=0&auto` arrenca directament; cada execució tria preguntes noves. Els textos de les cartes són a `CARTES`.

### Locució al generador de vídeos
Al menú de `video.html` hi ha un desplegable «Locució»:
- **Veu del navegador**: gratis i immediata (veu en català del sistema), però només per provar: no queda gravada al MP4.
- **ElevenLabs**: clau API + ID de veu; model multilingüe amb català. Es grava dins del vídeo. ~10.000 caràcters gratis al mes.
- **Google Cloud Text-to-Speech**: clau API amb l'API activada i nom de veu (`ca-ES-Standard-A`). 1 milió de caràcters gratis al mes.
Les frases es generen totes abans de començar (es veu el progrés) i es guarden en memòria cau. El text que es diu a cada moment és a l'objecte `GUIO`; la velocitat es regula al camp numèric (1,05 per defecte). La clau es desa al navegador (localStorage), no al fitxer.

### Àudio 100 % procedural
Tots els jocs i el generador de vídeos fan servir el mateix motor Web Audio, sense cap fitxer d'àudio:
- **Bus mestre**: guany + compressor abans de la sortida (i, al generador, també cap al gravador).
- **SFX**: oscil·ladors amb envolupant; cada so varia el to ±6 % a l'atzar perquè no soni repetitiu. Tipus: `ok`, `mal`, `clic`, `win` (arpegi), `bonus`, `pop`, `tic`.
- **Música**: seqüenciador de 32 passos amb *lookahead* (temporitzador de 25 ms que planifica 120 ms per endavant sobre el rellotge de l'AudioContext): bombo (sinus amb caiguda), xarles i caixa (soroll filtrat), baix (triangle) i melodia pentatònica generada a l'atzar. Estils `quiz` (126 bpm, major) i `suspens` (100 bpm, menor).
- Als jocs hi ha un botó 🎵 al HUD per activar-la o silenciar-la (es recorda). Al generador de vídeos es tria «Procedural: quiz/suspens» o un fitxer propi; baixa al 30 % mentre parla la veu i s'apaga en fos al final.
