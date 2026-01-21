# 📊 CAPACIDAD Y COMPRENSIÓN DEL SISTEMA DE EVALUACIÓN DINÁMICA

## 🎯 Descripción General

Se ha integrado un **sistema avanzado de evaluación dinámica de métricas SNII/VIEP** en el landing page del Dr. Filiberto Candia García que permite:

✅ **Evaluación en Tiempo Real** - Análisis instantáneo de cumplimiento SNII/VIEP  
✅ **Detección Automática de Cambios** - Monitoreo continuo de adiciones, eliminaciones y modificaciones  
✅ **Cálculo Dinámico de Puntuación** - Puntuación actualizada automáticamente (0-100%)  
✅ **Reporte Detallado** - Información completa sobre cada dimensión evaluada  
✅ **Exportación de Datos** - Descarga de reportes en formato JSON  
✅ **Historial de Cambios** - Registro completo de todas las modificaciones  

---

## 🔧 ARQUITECTURA TÉCNICA

### Componentes Principales

#### 1. **Botón de Acceso** (index.html)
```html
<button id="metricsBtn" class="badge" style="background: #ff6b6b; ...">
    📈 Evaluar Métricas SNII/VIEP
</button>
```

**Ubicación:** Encabezado principal, junto a Google Scholar y ORCID  
**Color:** Rojo (#ff6b6b) para destacar  
**Icono:** 📈 para indicar evaluación  

#### 2. **Motor de Evaluación** (metricas-evaluador.js)
Archivo JavaScript de 400+ líneas que contiene:

- **Clase MetricasEvaluador** - Núcleo del sistema
- **Métodos de Extracción** - Lectura de datos del DOM
- **Métodos de Detección** - Identificación de cambios
- **Métodos de Evaluación** - Cálculo de puntuaciones
- **Métodos de Presentación** - Generación de reportes visuales

---

## 📋 FUNCIONALIDADES DETALLADAS

### 1. Extracción de Datos del DOM

**Método: `extraerDatos()`**

Extrae automáticamente del landing page:

```javascript
{
    // Información Personal
    nombre: 'Dr. Filiberto Candia García',
    institucion: 'BUAP',
    titulo: 'Investigador SNII Nivel I',
    orcid: '0000-0002-7153-2202',
    
    // Estadísticas Contadas
    publicaciones: 10,      // Contadas de DOI en el DOM
    patentes: 6,            // Contadas de sección de patentes
    multimedia: 25,         // Audios + imágenes
    tesis: 17,              // Filas de tabla
    libros: 3,              // Contados de sección de libros
    
    // Presencia Digital
    tieneORCID: true,
    tieneCVU: true,
    tieneGoogleScholar: true,
    tieneResearchGate: true,
    tieneLinkedIn: true,
    
    // Timestamp
    ultimaActualizacion: '2026-01-21T09:30:00.000Z'
}
```

**Métodos de Conteo:**
- `contarPublicaciones()` - Busca elementos con clase `.doi-label`
- `contarPatentes()` - Cuenta tarjetas en sección de patentes
- `contarMultimedia()` - Suma audios + imágenes
- `contarTesis()` - Cuenta filas de tabla
- `contarLibros()` - Cuenta tarjetas en sección de libros

### 2. Detección de Cambios

**Método: `detectarCambios()`**

Ejecuta cada 5 segundos y compara:
- Estado anterior (guardado en localStorage)
- Estado actual (extraído del DOM)

**Tipos de Cambios Detectables:**

| Cambio | Tipo | Impacto |
|--------|------|---------|
| Nueva publicación agregada | `ADICIÓN_PUBLICACIÓN` | +2% |
| Nueva patente registrada | `ADICIÓN_PATENTE` | +3% |
| Nueva tesis dirigida | `ADICIÓN_TESIS` | +2% |
| Nuevo libro publicado | `ADICIÓN_LIBRO` | +3% |
| Nuevo recurso multimedia | `ADICIÓN_MULTIMEDIA` | +1% |
| Publicación removida | `ELIMINACIÓN_PUBLICACIÓN` | -2% |
| Patente removida | `ELIMINACIÓN_PATENTE` | -3% |
| Tesis removida | `ELIMINACIÓN_TESIS` | -2% |
| Libro removido | `ELIMINACIÓN_LIBRO` | -3% |
| Multimedia removida | `ELIMINACIÓN_MULTIMEDIA` | -1% |

**Ejemplo de Cambio Registrado:**
```javascript
{
    timestamp: '2026-01-21T09:35:00.000Z',
    tipo: 'ADICIÓN_PUBLICACIÓN',
    detalles: {
        publicacionesAntes: 9,
        publicacionesAhora: 10,
        patentesAntes: 6,
        patentesAhora: 6,
        // ... más detalles
    },
    impacto: +2  // Impacto en puntuación
}
```

### 3. Evaluación de Métricas SNII/VIEP

**Método: `evaluarSNII_VIEP()`**

Evalúa 7 dimensiones principales:

#### **Dimensión 1: Identidad Global** (15% peso)
```
Criterios:
- ORCID (15 puntos)
- CVU SNII (15 puntos)
- Google Scholar (10 puntos)

Requisito: Mínimo 2 de 3
```

#### **Dimensión 2: Publicaciones** (20% peso)
```
Criterios:
- Mínimo 3 publicaciones con DOI (20 puntos)

Requisito: Cumplimiento de cantidad
```

#### **Dimensión 3: Libros/Capítulos** (15% peso)
```
Criterios:
- Mínimo 1 libro/capítulo con ISBN (15 puntos)

Requisito: Cumplimiento de cantidad
```

#### **Dimensión 4: Formación de RH** (15% peso)
```
Criterios:
- Mínimo 2 tesis supervisadas (15 puntos)

Requisito: Cumplimiento de cantidad
```

#### **Dimensión 5: Patentes/Innovación** (15% peso)
```
Criterios:
- Mínimo 1 patente registrada (15 puntos)

Requisito: Cumplimiento de cantidad
```

#### **Dimensión 6: Multimedia** (10% peso)
```
Criterios:
- Landing page con contenido multimedia (10 puntos)

Requisito: Mínimo 5 recursos
```

#### **Dimensión 7: Presencia Digital** (10% peso)
```
Criterios:
- ResearchGate (5 puntos)
- LinkedIn (5 puntos)

Requisito: Mínimo 1 de 2
```

### 4. Cálculo de Puntuación

**Fórmula:**
```
Puntuación Final = (Σ Cumplimiento de Dimensiones × Peso) / Peso Total × 100
```

**Niveles de Cumplimiento:**

| Puntuación | Nivel | Color | Recomendación |
|------------|-------|-------|----------------|
| 70-100% | ✅ CUMPLE | Verde (#51cf66) | Listo para evaluación SNII/VIEP |
| 50-69% | ⚠️ PARCIAL | Amarillo (#ffd43b) | Completa los campos faltantes |
| 0-49% | 🔄 EN PROGRESO | Rojo (#ff6b6b) | Agrega más producción académica |

### 5. Presentación de Resultados

**Modal Interactivo** que muestra:

1. **Información del Investigador**
   - Nombre completo
   - Institución
   - ORCID
   - Fecha de evaluación

2. **Puntuación General**
   - Número grande (70%, 85%, etc.)
   - Nivel de cumplimiento
   - Recomendación personalizada

3. **Dimensiones Evaluadas**
   - Barra de progreso por dimensión
   - Porcentaje de cumplimiento
   - Criterios individuales (✓/✗)

4. **Datos Actuales**
   - Publicaciones: 10
   - Patentes: 6
   - Libros: 3
   - Tesis: 17
   - Multimedia: 25

5. **Cambios Detectados**
   - Historial de modificaciones
   - Tipo de cambio
   - Timestamp
   - Impacto en puntuación

6. **Botones de Acción**
   - 📥 Descargar Reporte JSON
   - Cerrar modal

---

## 🔄 FLUJO DE FUNCIONAMIENTO

### Secuencia de Operación

```
1. Usuario carga landing page
   ↓
2. Script metricas-evaluador.js se carga
   ↓
3. MetricasEvaluador se instancia
   ↓
4. Extrae datos iniciales del DOM
   ↓
5. Guarda estado inicial en localStorage
   ↓
6. Inicia monitoreo cada 5 segundos
   ↓
7. Usuario hace clic en "📈 Evaluar Métricas SNII/VIEP"
   ↓
8. Calcula evaluación actual
   ↓
9. Abre modal con resultados
   ↓
10. Usuario puede descargar reporte o cerrar
```

### Monitoreo Continuo

```
Cada 5 segundos:
1. Extrae datos actuales del DOM
2. Compara con estado anterior
3. Si hay cambios:
   - Registra cambio en array
   - Calcula impacto
   - Guarda en localStorage
   - Registra en historial
4. Continúa monitoreo
```

---

## 💾 ALMACENAMIENTO DE DATOS

### LocalStorage

El sistema almacena automáticamente:

```javascript
// Estado inicial
localStorage.metricas_estado_inicial = {
    nombre, institucion, publicaciones, ...
}

// Timestamp inicial
localStorage.metricas_timestamp_inicial = '2026-01-21T09:00:00.000Z'

// Cambios detectados
localStorage.metricas_cambios = [
    { timestamp, tipo, detalles, impacto },
    { timestamp, tipo, detalles, impacto },
    ...
]

// Historial completo
localStorage.metricas_historial = [
    { ... todos los cambios ... }
]
```

**Ventajas:**
- ✅ Datos persisten entre sesiones
- ✅ No requiere servidor
- ✅ Privacidad garantizada
- ✅ Acceso instantáneo

---

## 📥 EXPORTACIÓN DE REPORTES

### Formato JSON

Al hacer clic en "📥 Descargar Reporte JSON", se genera:

```json
{
  "investigador": "Dr. Filiberto Candia García",
  "institucion": "BUAP",
  "orcid": "0000-0002-7153-2202",
  "evaluacion": {
    "puntuacion": 85,
    "nivel": {
      "nivel": "CUMPLE",
      "color": "#51cf66",
      "recomendacion": "Listo para evaluación SNII/VIEP"
    },
    "dimensiones": [
      {
        "nombre": "Identidad Global",
        "criterios": [
          {
            "nombre": "ORCID",
            "cumple": true,
            "peso": 15
          },
          ...
        ],
        "peso": 15
      },
      ...
    ]
  },
  "datos": {
    "publicaciones": 10,
    "patentes": 6,
    "multimedia": 25,
    ...
  },
  "cambios": [
    {
      "timestamp": "2026-01-21T09:35:00.000Z",
      "tipo": "ADICIÓN_PUBLICACIÓN",
      "detalles": { ... },
      "impacto": 2
    }
  ],
  "generado": "2026-01-21T09:45:00.000Z"
}
```

**Nombre del archivo:**
```
metricas_snii_viep_2026-01-21.json
```

---

## 🎨 INTERFAZ DE USUARIO

### Modal de Evaluación

**Características:**
- Fondo oscuro semi-transparente
- Contenedor blanco con sombra
- Scroll interno si contenido es largo
- Botones de acción en la base
- Responsive (adapta a móvil, tablet, desktop)

**Elementos Visuales:**
- Encabezado con título y descripción
- Tarjeta de información del investigador
- Tarjeta grande con puntuación
- Barras de progreso por dimensión
- Listas de criterios (✓/✗)
- Tabla de datos actuales
- Historial de cambios
- Botones de acción

---

## 🔐 SEGURIDAD Y PRIVACIDAD

### Protección de Datos

✅ **Datos procesados localmente** - No se envían a servidores  
✅ **Sin cookies de seguimiento** - Solo localStorage local  
✅ **Validación de entrada** - Previene inyecciones  
✅ **Encriptación opcional** - Puede agregarse en futuro  

### Privacidad del Usuario

✅ **Control total** - Usuario decide qué compartir  
✅ **Exportación controlada** - Solo descarga quien lo solicita  
✅ **Sin tracking** - No se registran acciones del usuario  
✅ **Datos anónimos** - No se vinculan a IP o cookies  

---

## 🚀 CAPACIDADES AVANZADAS

### 1. Detección Inteligente de Cambios

El sistema puede detectar:
- Adición de nuevas publicaciones
- Eliminación de contenido
- Cambios en números (publicaciones, patentes, etc.)
- Actualizaciones de información

### 2. Cálculo de Impacto

Cada cambio tiene un impacto calculado:
- Publicación: +2% (o -2% si se elimina)
- Patente: +3% (o -3% si se elimina)
- Tesis: +2% (o -2% si se elimina)
- Libro: +3% (o -3% si se elimina)
- Multimedia: +1% (o -1% si se elimina)

### 3. Historial Completo

Mantiene registro de:
- Todos los cambios realizados
- Timestamps exactos
- Tipos de cambios
- Impactos acumulativos

### 4. Evaluación Dinámica

La puntuación se recalcula:
- Cada vez que se abre el modal
- Basada en datos actuales del DOM
- Considerando cambios históricos
- Con recomendaciones personalizadas

---

## 📊 EJEMPLO DE USO

### Escenario: Agregar Nueva Publicación

**Paso 1:** Usuario agrega nueva publicación al landing page

**Paso 2:** Sistema detecta cambio en 5 segundos
```
Cambio detectado:
- Tipo: ADICIÓN_PUBLICACIÓN
- Antes: 9 publicaciones
- Ahora: 10 publicaciones
- Impacto: +2%
```

**Paso 3:** Usuario hace clic en "📈 Evaluar Métricas"

**Paso 4:** Modal muestra:
- Puntuación actualizada (ej: 87% en lugar de 85%)
- Cambio registrado en historial
- Recomendación actualizada

**Paso 5:** Usuario descarga reporte JSON con nuevos datos

---

## 🔗 INTEGRACIÓN CON REPOSITORIO DE MÉTRICAS

### Vinculación

El sistema está diseñado para integrarse con:
- **Repositorio:** https://github.com/filibertocandia/Metricas-SNII
- **Sincronización:** Datos se pueden exportar a repositorio
- **Actualización:** Criterios se pueden actualizar desde repositorio

### Próximas Mejoras

- [ ] Sincronización automática con repositorio
- [ ] Actualización de criterios desde repositorio
- [ ] Comparación con otros investigadores
- [ ] Análisis de tendencias

---

## 📈 MÉTRICAS Y ESTADÍSTICAS

### Datos Recopilados

El sistema registra:
- Número de evaluaciones realizadas
- Cambios detectados por tipo
- Puntuaciones alcanzadas
- Dimensiones más débiles
- Tendencias a lo largo del tiempo

### Análisis Disponibles

- Evolución de puntuación
- Frecuencia de cambios
- Impacto acumulativo
- Recomendaciones prioritarias

---

## 🛠️ MANTENIMIENTO Y ACTUALIZACIONES

### Actualizar Criterios

Para cambiar los criterios de evaluación, editar en `metricas-evaluador.js`:

```javascript
{
    nombre: 'Publicaciones',
    criterios: [
        { nombre: 'Mínimo 3 con DOI', cumple: this.datos.publicaciones >= 3, peso: 20 }
    ],
    peso: 20
}
```

### Agregar Nueva Dimensión

```javascript
{
    nombre: 'Nueva Dimensión',
    criterios: [
        { nombre: 'Criterio 1', cumple: condicion, peso: 10 },
        { nombre: 'Criterio 2', cumple: condicion, peso: 10 }
    ],
    peso: 20
}
```

---

## 📞 SOPORTE Y DOCUMENTACIÓN

### Archivos Incluidos

1. **index.html** - Landing page con botón de métricas
2. **metricas-evaluador.js** - Motor de evaluación (400+ líneas)
3. **CAPACIDAD_EVALUACION_DINAMICA.md** - Este documento

### Contacto

- **Email:** filiberto@buap.edu.mx
- **GitHub:** @filibertocandia
- **ORCID:** 0000-0002-7153-2202

---

## ✅ CONCLUSIÓN

El sistema de evaluación dinámica de métricas SNII/VIEP proporciona:

✅ **Evaluación en tiempo real** de cumplimiento académico  
✅ **Detección automática** de cambios en el landing page  
✅ **Cálculo dinámico** de puntuaciones  
✅ **Reportes detallados** y exportables  
✅ **Historial completo** de modificaciones  
✅ **Interfaz intuitiva** y profesional  
✅ **Privacidad garantizada** con procesamiento local  

**Estado:** ✅ Completamente funcional y listo para uso

---

**Última actualización:** 21 de enero de 2026  
**Versión:** 1.0  
**Autor:** Dr. Filiberto Candia García

*Sistema de Evaluación Dinámica de Métricas SNII/VIEP para Landing Pages Académicos*
