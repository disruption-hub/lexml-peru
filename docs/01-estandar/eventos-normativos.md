# Catálogo de eventos normativos y estados de ratificación

El corazón del estándar: un catálogo **cerrado** de relaciones entre normas,
con la separación ontológica que evita el error capital — presentar una
inferencia algorítmica con la autoridad de un acto del legislador.

## Relaciones explícitas (actos del legislador — determinísticas)

| Tipo | Semántica | Efecto sobre vigencia |
|---|---|---|
| `DEROGA` | La norma origen deroga expresamente a la destino (o a un dispositivo vía `destinoEId`) | Sí: destino → `derogada` (total) |
| `MODIFICA` | Sustituye/añade texto en el destino | No (el texto cambia, la Obra sigue) |
| `REMITE` | Referencia normativa sin efecto modificatorio | No |

Estas nacen con `estadoRatificacion = EXPLICITA` y no requieren validación:
las declaró la propia norma.

## La relación inferida (señal algorítmica — probabilística)

| Tipo | Semántica |
|---|---|
| `POSIBLE_DEROGACION_TACITA` | Un clasificador (LLM u otro) detectó incompatibilidad insalvable o regulación íntegra de la misma materia (*lex posterior derogat priori*) |

Propiedades obligatorias: `confianzaInferida` (0.0–1.0),
`agenteInferencia` (quién/qué la produjo, versionado), `justificacion`
(cadena de razonamiento citando los eIds en conflicto).

### Ciclo de vida (estados de ratificación)

```
                    ┌─→ RATIFICADA_DOCTRINA        (sistematizador/MINJUSDH)
PENDIENTE ──────────┼─→ RATIFICADA_JURISPRUDENCIA  (sentencia; inter partes o
                    │                               erga omnes según el órgano)
                    ├─→ RECHAZADA                   (falso positivo)
                    └─→ ARCHIVADA                   (time-decay: sin
                                                    pronunciamiento en 24-36
                                                    meses, sale de las vistas
                                                    por defecto)
```

**Reglas no negociables:**

1. Una inferencia `PENDIENTE` **jamás** muta la vigencia de la norma
   destino. La vigencia solo cambia por acto explícito (`DEROGA`) o por
   ratificación jurisprudencial — y aun entonces el estado intermedio es
   `derogada_tacita_pendiente` (de consolidación editorial), nunca
   `derogada` directa.
2. Toda superficie que muestre una inferencia no ratificada la rotula como
   señal sin autoridad (advertencia explícita en la UI/API). El falso
   positivo que se cuela en una sentencia genera nulidades en cascada.
3. La ratificación registra QUIÉN (`ratificadaPor`) y CUÁNDO
   (`ratificadaAt`) — el sistema es bitemporal: distingue cuándo la
   derogación *rige* de cuándo el sistema *la supo*.

## Estados de vigencia de la norma

`vigente` · `derogada` · `derogada_tacita_pendiente` · `abrogada`
