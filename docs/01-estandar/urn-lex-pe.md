# URN:LEX-PE — el identificador persistente de la norma peruana

Adaptación del espacio `urn:lex` (draft-spinosa-urn-lex, adoptado por LexML
Brasil) a la jurisdicción peruana. El URN identifica la **Obra** (FRBR) —
la norma como acto jurídico — con independencia de dónde viva su texto.

## Gramática

```
urn:lex:pe:<emisor>:<tipo>:<fecha-publicacion>;<numero>[~<eId>]
```

| Componente | Reglas | Ejemplos |
|---|---|---|
| `emisor` | minúsculas, puntos como separador, sin tildes | `congreso`, `poder.ejecutivo`, `minjusdh`, `tc` |
| `tipo` | catálogo controlado (abajo) | `ley`, `decreto.legislativo`, `decreto.supremo` |
| `fecha-publicacion` | ISO `YYYY-MM-DD` — la fecha de publicación en El Peruano | `2016-12-30` |
| `numero` | el número oficial, sin ceros a la izquierda | `27770`, `1296`, `004-2026-jus` |
| `eId` (fragmento) | opcional, referencia a un dispositivo | `~art_5`, `~disp_comp_final_3` |

## Ejemplos canónicos

```
urn:lex:pe:congreso:ley:2002-08-09;27770
urn:lex:pe:poder.ejecutivo:decreto.legislativo:2016-12-30;1296
urn:lex:pe:poder.ejecutivo:decreto.legislativo:2016-12-30;1296~art_47
```

## Catálogo inicial de tipos

`constitucion` · `ley` · `ley.organica` · `decreto.legislativo` ·
`decreto.supremo` · `decreto.urgencia` · `resolucion.ministerial` ·
`resolucion.suprema` · `ordenanza.regional` · `ordenanza.municipal` ·
`sentencia.tc`

El catálogo es **extensible por PR** — un tipo nuevo debe llegar con al
menos un ejemplo real codificado en `schema/ejemplos/`.

## Reglas de resolución

1. El URN es **inmutable**: una vez asignado, jamás se reescribe.
2. La resolución (URN → texto/metadata) es responsabilidad de cada
   implementación (`GET /resolver?urn=…`); el estándar solo fija la llave.
3. La normalización de entrada (tildes → ASCII, espacios → puntos,
   mayúsculas → minúsculas) es obligatoria ANTES de comparar URNs.
4. El fragmento `~eId` nunca participa de la identidad de la Obra — dos
   URNs que difieren solo en fragmento identifican la misma norma.
