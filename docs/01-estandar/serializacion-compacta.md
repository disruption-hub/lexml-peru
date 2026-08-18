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

## Reglas

1. **`dispositivos` es obligatorio y ordenado** — el orden del array ES el
   ordinal. Un dispositivo sin `eId` estable no es indexable.
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
