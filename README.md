# LexML Perú — Estándar abierto de datos jurídicos estructurados para el Perú

> *Aviso Legal: El contenido de este repositorio es estrictamente para fines
> informativos y de investigación arquitectónica. Las propuestas
> tecnológicas, estructurales y normativas aquí descritas no constituyen
> asesoramiento legal, institucional, técnico o profesional vinculante, ni
> representan políticas oficiales adoptadas por el Estado Peruano o sus
> instituciones. Este es un proyecto comunitario abierto que aspira a servir
> de insumo para una eventual adopción institucional (MINJUSDH, Congreso,
> PCM/SEGDI), al modo en que LexML Brasil sirvió al Estado brasileño.*

## La idea en una línea

Que cada norma peruana tenga un **identificador persistente** (`URN:LEX-PE`),
una **estructura de dispositivos estable** (eIds), un **catálogo cerrado de
eventos normativos** (deroga, modifica, remite — y la inferencia de
derogación tácita como categoría separada y NO autoritativa), y que sobre
esa base se materialicen el **grafo de conocimiento legal** y la **IA
verificable** — nunca al revés.

## El principio rector: cadena de valor unidireccional

```
ESTÁNDAR  →  DATOS  →  GRAFO  →  IA
(URN+eIds)   (fuente    (espejo    (GraphRAG +
              de         operativo  inferencia con
              verdad)    Cypher)    ratificación humana)
```

Aplicar LLMs directamente sobre el corpus desestructurado (PDFs de El
Peruano, SPIJ) produce sistemas propensos a alucinaciones peligrosas en un
dominio donde un falso positivo desencadena nulidades procesales. El XML/
JSON estructurado no compite con la IA: es su condición de posibilidad.

## Qué hay en este repositorio

| Ruta | Contenido |
|---|---|
| `docs/01-estandar/urn-lex-pe.md` | Gramática del identificador URN:LEX-PE |
| `docs/01-estandar/ids-dispositivos.md` | eIds estables de artículos/disposiciones |
| `docs/01-estandar/eventos-normativos.md` | Catálogo cerrado de relaciones + estados de ratificación |
| `docs/01-estandar/serializacion-compacta.md` | Formato JSON compacto para ingesta y LLMs |
| `docs/02-gobernanza.md` | Human-in-the-loop, clausura operativa, Fase 0 |
| `docs/03-comparativa.md` | Lecciones de LexML Brasil y LEOS (UE) |
| `schema/lexperu-compact.schema.json` | JSON Schema del formato compacto |
| `schema/ejemplos/` | Casuística peruana codificada (caso Ley 27770 ↔ DL 1296) |
| `graph/proyeccion/` | Modelo de grafo (Cypher/Neo4j) y consultas de impacto |

## Estado

**v0.1 — borrador comunitario.** El proyecto adopta la marca **LexML Perú** como capítulo peruano de la familia LexML (`urn:lex`), homologando el camino de LexML Brasil — misma familia de estándar, misma vocación de adopción estatal, taxonomía de repos espejo (`lexml-peru`, y a futuro una organización con `-schemas`, `-parser`, `-resolver`). Existe una implementación de referencia
operativa (ingesta, resolver URN, grafo, inferencia de derogaciones tácitas
con ratificación humana) corriendo en producción privada; este repositorio
publica el estándar para discusión y adopción. El XSD Akoma Ntoso-compatible
está en el roadmap — la serialización compacta JSON es la puerta de entrada
de menor fricción.

## Cómo contribuir

Issues y PRs bienvenidos — especialmente de: operadores jurídicos (¿los eIds
capturan la práctica real de citado?), archiveros/bibliotecarios (FRBR),
e ingenieros de datos del Estado. Español como idioma primario del proyecto.

## Licencia

- Código y schemas: **Apache-2.0**
- Documentación (`docs/`): **CC BY 4.0**

Inspirado en el camino de [LexML Brasil](https://projeto.lexml.gov.br/) y
[LEOS (UE)](https://joinup.ec.europa.eu/collection/justice-law-and-security/solution/leos-open-source-software-editing-legislation),
con las lecciones de ambos documentadas en `docs/03-comparativa.md`.
