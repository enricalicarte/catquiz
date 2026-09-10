# Xerinola · Minijocs 3D

Web de minijocs fets amb Three.js. Cada joc és un únic fitxer HTML: sense build, sense dependències, sense servidor.

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

El vocabulari de cada joc de paraules és una constant al principi del seu `<script>`: amplia-la amb les paraules que vulguis (sense accents).

Three.js es carrega des d'unpkg amb un `importmap` (versió 0.160.0).

## Reel automàtic (per gravar vídeos verticals)
- Obre `demo.html`: un reel de ~28 s per joc (hook → joc jugant sol amb subtítols i zooms → CTA), en bucle. Tria el joc amb els botons o amb `?joc=motle`.
- Els subtítols de cada reel són a l'objecte `REELS` de `demo.html`: [segon, text, dalt|baix, zoom].
- Qualsevol joc també accepta `?demo` directament (p. ex. `jocs/motle.html?demo`) per gravar-lo sol.
- Per gravar: obre-ho a Chrome, F12 → mode dispositiu → 1080×1920 (o directament al mòbil en vertical) i grava la pantalla.

## Format quiz (per al canal CatQuiz)
- `demo.html?joc=motle&quiz` (també `penjat` i `anagrama`): el joc juga sol fins a mig camí, apareix "QUINA ÉS? Escriu-la als comentaris" amb compte enrere, i després el bot revela la solució amb confeti. Acaba amb la carta "L'has encertada?".
- La pausa la decideix cada joc (`?demo&quiz`): Motle després de 2 intents, Penjat amb la meitat de lletres vistes, Anagrama a la segona paraula.
