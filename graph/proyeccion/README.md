# Proyección al grafo (Cypher/Neo4j)

El grafo es ESPEJO del repositorio estructurado — nunca fuente de verdad.
Se reconstruye por completo desde los datos (backfill idempotente).

## Modelo

```cypher
(:LexNorma {urn, tipo, numero, emisor, titulo, fechaPublicacion,
            estadoVigencia, clausulaGenerica, materias})
(:LexDispositivo {eId, rotulo, texto})-[:DISPOSITIVO_DE]->(:LexNorma)
(:LexNorma)-[:DEROGA|MODIFICA|REMITE {estadoRatificacion:'EXPLICITA'}]->(:LexNorma)
(:LexNorma)-[:POSIBLE_DEROGACION_TACITA
   {confianzaInferida, estadoRatificacion, agenteInferencia}]->(:LexNorma)
```

## La consulta de travesía condicionada (del documento de diseño)

"Normas vigentes aplicables, excluyendo las que tienen inferencia de
derogación tácita con confianza > 0.8":

```cypher
MATCH (n:LexNorma {estadoVigencia: 'vigente'})
WHERE NOT EXISTS {
  MATCH (:LexNorma)-[r:POSIBLE_DEROGACION_TACITA]->(n)
  WHERE r.confianzaInferida > 0.8 AND r.estadoRatificacion = 'PENDIENTE'
}
RETURN n.urn, n.titulo
```

## Impacto normativo

```cypher
MATCH (o:LexNorma)-[r]->(me:LexNorma {urn: $urn})
RETURN type(r), o.urn, r.confianzaInferida, r.estadoRatificacion
```

Toda vista que incluya aristas no ratificadas DEBE rotularlas como señal
sin autoridad (ver docs/01-estandar/eventos-normativos.md).
