# eIds — identificadores estables de dispositivos

El eId identifica un dispositivo (artículo, párrafo, disposición) DENTRO de
una norma, de forma estable a través de versiones. Estilo Akoma Ntoso
simplificado.

## Gramática

`<clase>_<ordinal>[_<subclase>_<ordinal>…]` — minúsculas, guión bajo.

| Clase | Ejemplo | Dispositivo |
|---|---|---|
| `art` | `art_5` | Artículo 5 |
| `par` | `art_5_par_2` | Párrafo 2 del artículo 5 |
| `inc` | `art_5_inc_b` | Inciso b) |
| `disp_comp_final` | `disp_comp_final_3` | Tercera Disposición Complementaria Final |
| `disp_comp_trans` | `disp_comp_trans_1` | Disposición Complementaria Transitoria |
| `disp_comp_derog` | `disp_comp_derog_unica` | Disposición Complementaria Derogatoria |

## Reglas

1. El eId nace con la norma y NUNCA se renumera — si una modificación
   inserta un artículo 5-A, su eId es `art_5a`, no un corrimiento.
2. Ordinales textuales admitidos: `unica`, `primera`… cuando la norma no
   numera con cifras.
3. El fragmento URN (`urn…~art_5`) usa el eId tal cual.
