# Serialización compacta (JSON) — la puerta de entrada

El formato de ingesta y de intercambio con LLMs. Es deliberadamente MÁS
SIMPLE que Akoma Ntoso completo: el XSD pleno está en el roadmap; esta
serialización captura lo esencial (identidad, estructura, eventos) con
fricción mínima para adopción temprana.

## Forma

```json
{
  "tipo": "decreto_legislativo",
  "numero": "1296",
  "emisor": "poder.ejecutivo",
  "fechaPublicacion": "2016-12-30",
  "titulo": "Decreto Legislativo que modifica el Código de Ejecución Penal en materia de beneficios penitenciarios",
  "materias": ["beneficios_penitenciarios", "ejecucion_penal"],
  "fuente": "el_peruano",
  "clausulaGenerica": true,
  "dispositivos": [
    { "eId": "art_1", "rotulo": "Artículo 1", "texto": "Objeto de la norma…" },
    { "eId": "art_47", "rotulo": "Artículo 47", "texto": "Redención de pena por trabajo o educación…" }
  ],
  "relaciones": [
    { "tipo": "MODIFICA", "destinoUrn": "urn:lex:pe:congreso:ley:1991-08-02;codigo.ejecucion.penal", "destinoEId": "art_47" }
  ]
}
```

Validable con `schema/lexperu-compact.schema.json`.

## Completitud del registro

Un corpus real nunca está completo: las normas nuevas derogan normas
antiguas cuyo texto puede no estar cargado todavía, y una relación
`DEROGA` necesita un destino identificable HOY. El campo `completitud`
distingue los dos niveles de registro:

- **`integra`** (default): la norma viaja con su texto articulado
  (`dispositivos` obligatorio). Es el registro pleno.
- **`referencia`**: solo metadata — identidad URN, título, fecha, materias
  y opcionalmente `estadoVigencia` conocido (p.ej. una ley de 1966 ya
  derogada, registrada como destino de una derogación). `dispositivos`
  puede omitirse.

Dos consecuencias para el consumidor: (1) de un registro `referencia`
**no se puede afirmar contenido** — solo identidad y estado declarado;
(2) el registro `referencia` es un estado transitorio por diseño — cuando
el texto llegue, la misma URN lo recibe y el registro pasa a `integra`
sin cambiar de identidad. `fuente: registro_historico` marca los
registros nacidos por esta vía.

## Reglas

1. **`dispositivos` es obligatorio en registros `integra` y ordenado** —
   el orden del array ES el ordinal. Un dispositivo sin `eId` estable no
   es indexable. Solo un registro `completitud: referencia` puede omitirlo.
2. **`materias` alimenta la recuperación limitada**: la inferencia de
   antinomias solo compara normas que comparten materia (Graph-RAG acotado
   — nunca el corpus completo contra sí mismo).
3. **`relaciones` solo admite tipos explícitos** (`DEROGA`/`MODIFICA`/
   `REMITE`). Las inferidas NO se declaran en la ingesta: las produce el
   pipeline con su confianza y su ciclo de ratificación.
4. Un `destinoUrn` aún no indexado no es error — la relación se re-declara
   idempotentemente cuando esa norma entre.
5. El URN de la norma NO viaja en el JSON: se **deriva** de
   (emisor, tipo, fecha, numero) — una sola fuente de identidad.
