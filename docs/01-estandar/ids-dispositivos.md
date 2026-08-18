# eIds — identificadores estables de dispositivos

El eId identifica un dispositivo (artículo, párrafo, disposición,
agrupador estructural) DENTRO de una norma, de forma estable a través de
versiones. Estilo Akoma Ntoso simplificado.

## Gramática

`<clase>_<ordinal>[_<subclase>_<ordinal>…]` — minúsculas, guión bajo.

| Clase | Ejemplo | Dispositivo |
|---|---|---|
| `art` | `art_5` | Artículo 5 |
| `par` | `art_5_par_2` | Párrafo 2 del artículo 5 |
| `inc` | `art_5_inc_b` | Inciso b) |
| `dcf` | `dcf_3` | Tercera Disposición Complementaria Final |
| `dct` | `dct_11` | Décima Primera Disposición (Complementaria) Transitoria |
| `dcd` | `dcd_unica` | Disposición Complementaria Derogatoria Única |
| `dcm` | `dcm_1` | Primera Disposición Complementaria Modificatoria |

> Nota de compatibilidad: versiones tempranas de este documento usaban las
> formas largas (`disp_comp_final_3`). La forma canónica es la corta
> (`dcf_3`) — es la que produce el pipeline de indexación y la que vive en
> los corpus reales. Un consumidor puede tratar las largas como alias de
> lectura, pero no debe emitirlas.

## Agrupadores estructurales (LIBRO / SECCIÓN / TÍTULO / CAPÍTULO)

La serialización compacta es PLANA, así que la jerarquía estructural de la
norma se expresa como dispositivos **agrupadores**, con eId compuesto por
la ruta de contenedores vigente, unida con `__` (doble guión bajo, estilo
Akoma Ntoso):

| Clase | Ejemplo | Contenedor |
|---|---|---|
| `lib` | `lib_2` | Libro Segundo |
| `sec` | `lib_2__sec_3` | Sección Tercera (dentro del Libro Segundo) |
| `tit` | `lib_2__sec_2__tit_3` | Título III (dentro de esa Sección) |
| `cap` | `tit_1__cap_2` | Capítulo II (dentro del Título I) |

- Ordinales admitidos: cifras, romanos (`TÍTULO III` → `tit_3`), ordinales
  en palabra (`SECCIÓN TERCERA` → `sec_3`, incluidos los compuestos:
  «DÉCIMA PRIMERA» → 11), y los especiales `preliminar`
  (`tit_preliminar`) y `unico`/`unica` (`tit_unico`).
- El `rotulo` del agrupador es su encabezado («TÍTULO III»); su `texto` es
  la **denominación** («OBLIGACIONES CONVERTIBLES»), o el propio rótulo si
  la norma no trae una.
- **Los artículos siguen PLANOS** (`art_234`, nunca
  `lib_2__sec_7__art_234`): el eId del artículo no depende de dónde cae en
  la jerarquía, así una reorganización estructural no rompe ninguna
  referencia. La posición se lee del ORDEN del array.
- El separador `__` es exclusivo de agrupadores: distingue la unión de
  ruta (`sec_3__tit_1`) del anidamiento intra-dispositivo con `_` simple
  (`art_5_par_2`).

Consecuencia para el consumidor: **contar artículos es contar eIds con
prefijo `art_`** — los agrupadores y las disposiciones no entran en ese
conteo.

## Reglas

1. El eId nace con la norma y NUNCA se renumera — si una modificación
   inserta un artículo 5-A, su eId es `art_5a`, no un corrimiento.
2. Ordinales textuales admitidos: `unica`, `primera`… cuando la norma no
   numera con cifras.
3. El fragmento URN (`urn…~art_5`) usa el eId tal cual.
