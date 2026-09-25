# Desarrollo Guiado por Calidad (Quality-Driven Development)

Estas reglas se aplican a **todo** proyecto y **toda** tarea de desarrollo de código, sin excepción.

## 1. Desarrollo Guiado por Pruebas (TDD)

- **Siempre** escribe o solicita las pruebas unitarias **antes** de escribir el código de producción.
- Si el usuario proporciona pruebas que fallan, genera e itera el código **únicamente** para hacer que todas pasen **sin modificar los tests**.
- El ciclo obligatorio es: **Red → Green → Refactor**.
  1. **Red**: Definir tests que fallen.
  2. **Green**: Escribir el código mínimo para que pasen.
  3. **Refactor**: Mejorar el código sin romper los tests.
- Si no existen tests previos, **créalos primero** basándote en los requisitos del usuario antes de implementar la funcionalidad.

## 2. Definición BDD / Gherkin

- Cuando el usuario proporcione historias de usuario o requisitos funcionales, **tradúcelos a escenarios BDD** en formato Gherkin (`Dado... / Cuando... / Entonces...`) antes de implementar.
- Implementa la funcionalidad para que cumpla con cada escenario BDD definido.
- Los escenarios Gherkin sirven como documentación viva y contrato de comportamiento.
- Estructura de escenario obligatoria:
  ```gherkin
  Característica: [Nombre descriptivo]
    Escenario: [Caso de uso específico]
      Dado [contexto inicial]
      Cuando [acción del usuario]
      Entonces [resultado esperado]
  ```

## 3. Quality Gates (Métricas de Cobertura y Calidad)

- **Cobertura de tests**: Apunta a una cobertura **superior al 90%** en cada módulo/componente.
- **Linter**: Todo el código debe pasar las reglas del linter configurado en el proyecto (ESLint, Pylint, etc.) sin warnings ni errores.
- **Análisis estático**: Ejecuta herramientas de análisis estático disponibles y corrige todos los hallazgos antes de dar la tarea por finalizada.
- **No dar por terminada** ninguna tarea hasta que se cumplan todos los quality gates.
- Si el proyecto no tiene linter o herramientas de análisis estático configuradas, **sugiere y configura** las apropiadas para el stack tecnológico.

## 4. Mutation Testing

- Tras completar la suite de tests, **ejecuta pruebas de mutación** (cuando las herramientas estén disponibles) para garantizar que:
  - Las pruebas realmente detectan fallos (no son falsos positivos).
  - No existen caminos muertos en el código.
  - La suite tiene una **tasa de mutantes eliminados (mutation score)** aceptable (>80%).
- Herramientas por ecosistema:
  - **JavaScript/TypeScript**: Stryker Mutator
  - **Python**: mutmut, cosmic-ray
  - **Java/Kotlin**: PIT (pitest)
  - **C#/.NET**: Stryker.NET
- Si mutation testing no es viable (ej. proyecto muy pequeño o sin herramientas), **documentar la razón** y proponer alternativas.

## 5. Verificación en Entorno Aislado (Sandbox)

- **Antes de entregar la solución final**, ejecuta:
  1. Compilación completa sin errores.
  2. Suite completa de tests (unitarios, integración, e2e si aplica).
  3. Verificar que **no haya errores en consola**.
  4. Verificar que no haya errores de integración.
- Solo entregar la solución cuando **todos los pasos anteriores pasen limpiamente**.
- Si hay errores, **iterar y corregir** antes de presentar el resultado al usuario.

## Flujo de Trabajo Obligatorio (Resumen)

```
Requisitos → Escenarios BDD → Tests (Red) → Código (Green) → Refactor →
Quality Gates (cobertura, linter, análisis) → Mutation Testing →
Verificación Sandbox → ✅ Entrega
```

## Excepciones

- Tareas puramente de investigación o consulta (no generan código).
- Correcciones triviales de una sola línea donde el contexto es claro y el riesgo es mínimo.
- Si el usuario **explícitamente** indica que no desea seguir este flujo para una tarea específica.
