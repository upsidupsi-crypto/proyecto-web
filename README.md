# Estructuras de Datos - Aplicación Interactiva

## Descripción General

Esta es una aplicación web interactiva que permite visualizar y experimentar el funcionamiento de tres estructuras de datos fundamentales: Pila (Stack), Cola (Queue) y Lista Enlazada. La aplicación muestra tanto la visualización gráfica de las operaciones como el código C equivalente que se ejecutaría en cada caso.

## Flujo de la Aplicación

### 1. Página de Inicio (Proyecto.html)

Cuando accedes a la aplicación, llegamos a la página principal que presenta tres tarjetas educativas:

- **Pila (Stack)**: Estructura LIFO (Last In, First Out)
- **Cola (Queue)**: Estructura FIFO (First In, First Out)
- **Lista Enlazada**: Estructura flexible de nodos conectados

Cada tarjeta contiene:
- Un título descriptivo
- Una breve explicación de la estructura
- Un enlace para explorar esa estructura

**Código ejecutado:**
```javascript
// El navegador carga el HTML y los estilos CSS
// Se renderiza el contenido estático de las tarjetas
// Los enlaces permiten navegar a cada estructura
```

---

## 2. Pila (Stack) - Pila.html

### Concepto

Una Pila es una estructura de datos donde:
- El último elemento insertado es el primero en salir (LIFO)
- Las operaciones principales son PUSH (insertar) y POP (eliminar)
- Se visualiza de forma vertical, con elementos apilados de abajo hacia arriba

### Funciones Principales

#### a) Función `crearNueva()`

```javascript
crearNueva() {
    this.datos = [];
    this.render();
    this.mensaje('Pila nueva creada');
    document.getElementById('inputPila').value = '';
    document.getElementById('codigoDisplay').innerHTML = 'Seleccione una operación...';
}
```

**Explicación paso a paso:**
1. `this.datos = []`: Limpia el array de datos, dejándolo vacío
2. `this.render()`: Redibuja la visualización para mostrar la pila vacía
3. `this.mensaje('Pila nueva creada')`: Muestra un mensaje de confirmación
4. `document.getElementById('inputPila').value = ''`: Limpia el campo de entrada
5. `document.getElementById('codigoDisplay').innerHTML = 'Seleccione una operación...'`: Reinicia el panel de código

#### b) Función `descargarTxt()`

#### b) Función `descargarTxt()`

```javascript
descargarTxt() {
    if (this.datos.length === 0) return this.mensaje('Pila vacía, nada que descargar');
    const contenido = this.datos.join(',');
    const blob = new Blob([contenido], { type: 'text/plain' });
    const url = window.URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'pila.txt';
    a.click();
    window.URL.revokeObjectURL(url);
    this.mensaje('Pila descargada');
}
```

**Explicación paso a paso:**
1. `this.datos.length === 0`: Verifica si la pila está vacía
2. `this.datos.join(',')`: Une todos los elementos del array en un string separados por comas. Ejemplo: [1,2,3] se convierte en "1,2,3"
3. `new Blob([contenido], { type: 'text/plain' })`: Crea un objeto Blob (Binary Large Object) que representa un archivo de texto con el contenido
4. `window.URL.createObjectURL(blob)`: Genera una URL temporal que apunta al archivo en memoria del navegador
5. `document.createElement('a')`: Crea un elemento anchor (enlace) ficticio en memoria
6. `a.href = url`: Asigna la URL del archivo al enlace
7. `a.download = 'pila.txt'`: Establece el nombre del archivo a descargar
8. `a.click()`: Simula un click en el enlace, lo que inicia la descarga del archivo
9. `window.URL.revokeObjectURL(url)`: Libera la memoria de la URL temporal
10. `this.mensaje('Pila descargada')`: Muestra un mensaje de confirmación

#### c) Función `cargarTxt(event)`

```javascript
cargarTxt(event) {
    const file = event.target.files[0];
    if (!file) return;
    const reader = new FileReader();
    reader.onload = (e) => {
        const contenido = e.target.result.trim();
        if (!contenido) return this.mensaje('Archivo vacío');
        this.datos = contenido.split(',').map(d => d.trim()).filter(d => !isNaN(d) && d !== '');
        if (this.datos.length === 0) return this.mensaje('Archivo sin datos válidos');
        this.render();
        this.mensaje(`Cargados ${this.datos.length} elementos`);
    };
    reader.readAsText(file);
    event.target.value = '';
}
```

**Explicación paso a paso:**
1. `event.target.files[0]`: Accede al primer archivo seleccionado por el usuario
2. `new FileReader()`: Crea un objeto FileReader que permite leer archivos
3. `reader.onload`: Define una función que se ejecuta cuando el archivo se carga completamente
4. `e.target.result.trim()`: Obtiene el contenido del archivo y elimina espacios en blanco al inicio y final
5. `contenido.split(',')`: Divide el string en un array usando la coma como separador. Ejemplo: "1,2,3" se convierte en ["1", "2", "3"]
6. `.map(d => d.trim())`: Aplica una función a cada elemento del array para eliminar espacios. Concepto MAP: transforma cada elemento aplicando una función
7. `.filter(d => !isNaN(d) && d !== '')`: Mantiene solo elementos que son números válidos (not a Number = NaN). Concepto FILTER: selecciona elementos que cumplen una condición
8. `reader.readAsText(file)`: Lee el archivo como texto
9. `event.target.value = ''`: Limpia el input file para que se pueda cargar otro archivo

#### c) Función `push()`

```javascript
push() {
    const input = document.getElementById('inputPila');
    const val = input.value.trim();
    if (!val || isNaN(val)) return alert("Ingrese un número");
    
    this.datos.push(val);
    this.render();
    this.mostrarCodigo(CODIGO_C.push);
    this.mensaje(`Push: ${val}`);
    input.value = '';
    input.focus();
}
```

**Explicación paso a paso:**
1. `document.getElementById('inputPila')`: Obtiene el elemento input del DOM
2. `input.value.trim()`: Lee el valor del input y elimina espacios en blanco
3. `isNaN(val)`: Verifica si el valor NO es un número
4. `this.datos.push(val)`: Agrega el valor al final del array (concepto PUSH: añade un elemento)
5. `this.render()`: Redibuja la visualización de la pila
6. `this.mostrarCodigo(CODIGO_C.push)`: Muestra el código C correspondiente
7. `input.value = ''`: Limpia el input para la siguiente entrada
8. `input.focus()`: Coloca el cursor en el input automáticamente

#### d) Función `pop()`

```javascript
pop() {
    if (this.datos.length === 0) return this.mensaje('Pila vacía');
    const val = this.datos.pop();
    this.render();
    this.mostrarCodigo(CODIGO_C.pop);
    this.mensaje(`Pop: ${val}`);
}
```

**Explicación paso a paso:**
1. `this.datos.pop()`: Elimina y retorna el último elemento del array (concepto POP: saca el último elemento, implementando LIFO)
2. El resto funciona de manera similar a push()

#### e) Función `render()`

```javascript
render() {
    const div = document.getElementById('lienzo');
    div.innerHTML = '';
    if (this.datos.length === 0) {
        div.innerHTML = 'Vacía';
        return;
    }
    this.datos.slice().reverse().forEach(d => {
        const nodo = document.createElement('div');
        nodo.style.border = "1px solid black";
        nodo.style.width = "50px";
        nodo.style.padding = "5px";
        nodo.style.textAlign = "center";
        nodo.className = 'nodo-entrada';
        nodo.textContent = d;
        div.appendChild(nodo);
    });
}
```

**Explicación paso a paso:**
1. `document.getElementById('lienzo')`: Obtiene el contenedor donde se dibuja la pila
2. `div.innerHTML = ''`: Limpia el contenedor (elimina todo contenido anterior)
3. `this.datos.slice()`: Crea una COPIA del array sin modificar el original. Concepto SLICE: extrae una parte de un array
4. `.reverse()`: Invierte el orden de la copia del array
5. `.forEach(d => {...})`: Itera sobre cada elemento de la copia invertida. Concepto FOREACH: ejecuta una función para cada elemento
6. `document.createElement('div')`: Crea un nuevo elemento div en memoria
7. `nodo.style.border = ...`: Asigna estilos CSS al nodo (borde, ancho, relleno, etc.)
8. `div.appendChild(nodo)`: Agrega el nodo como hijo del contenedor. Concepto APPENDCHILD: añade un elemento HTML dentro de otro

**Por qué se invierte:**
- Los datos se guardan en el array en orden: [1, 2, 3]
- Pero visualmente la pila debe mostrar el tope (3) arriba
- Al invertir y renderizar: primero dibuja 3 (arriba), luego 2, luego 1 (abajo)

---

## 3. Cola (Queue) - Cola.html

### Concepto

Una Cola es una estructura de datos donde:
- El primer elemento insertado es el primero en salir (FIFO)
- Las operaciones principales son ENCOLAR (insertar al final) y DESENCOLAR (eliminar del inicio)
- Se visualiza horizontalmente de izquierda a derecha

### Funciones Principales

#### a) Función `crearNueva()`

```javascript
crearNueva() {
    this.datos = [];
    this.render();
    this.mensaje('Cola nueva creada');
    document.getElementById('inputCola').value = '';
    document.getElementById('codigoDisplay').innerHTML = 'Seleccione una operación...';
}
```

**Explicación paso a paso:**
1. `this.datos = []`: Limpia el array de datos, dejándolo vacío
2. `this.render()`: Redibuja la visualización para mostrar la cola vacía
3. `this.mensaje('Cola nueva creada')`: Muestra un mensaje de confirmación
4. `document.getElementById('inputCola').value = ''`: Limpia el campo de entrada
5. `document.getElementById('codigoDisplay').innerHTML = 'Seleccione una operación...'`: Reinicia el panel de código

#### b) Función `encolar()`

#### b) Función `encolar()`

Similar a push() en pila, pero conceptualmente diferente:
```javascript
encolar() {
    const input = document.getElementById('inputCola');
    const val = input.value.trim();
    if (!val || isNaN(val)) return alert("Ingrese un número");
    
    this.datos.push(val);  // Agrega al final (porque es una cola)
    this.render();
    this.mostrarCodigo(CODIGO_C.encolar);
    this.mensaje(`Encolado: ${val}`);
    input.value = '';
    input.focus();
}
```

**Diferencia con Pila:**
- Ambos usan `push()`, pero el significado es diferente
- En Pila: el nuevo elemento es el tope
- En Cola: el nuevo elemento se une a la fila

#### c) Función `desencolar()`

```javascript
desencolar() {
    if (this.datos.length === 0) return this.mensaje('Cola vacía');
    const val = this.datos.shift();  // Elimina el PRIMERO
    this.render();
    this.mostrarCodigo(CODIGO_C.desencolar);
    this.mensaje(`Desencolado: ${val}`);
}
```

**Explicación:**
1. `this.datos.shift()`: Elimina y retorna el PRIMER elemento del array (concepto SHIFT: saca del inicio, implementando FIFO)
2. Esto es diferente a pop() que saca del final

#### d) Función `render()` (Cola)

```javascript
render() {
    const div = document.getElementById('lienzo');
    div.innerHTML = '';
    if (this.datos.length === 0) {
        div.innerHTML = 'Vacía';
        return;
    }
    this.datos.forEach(d => {
        const nodo = document.createElement('div');
        nodo.style.border = "1px solid black";
        nodo.style.padding = "5px";
        nodo.className = 'nodo-entrada';
        nodo.textContent = d;
        div.appendChild(nodo);
    });
}
```

**Diferencia con Pila:**
- NO invierte el array
- Dibuja los elementos en el orden original (izquierda a derecha)
- No usa slice() ni reverse() porque NO necesita modificar el orden

---

## 4. Lista Enlazada - Lista.html

### Concepto

Una Lista Enlazada es una estructura donde:
- Cada elemento (nodo) tiene un valor y un puntero al siguiente
- Permite insertar y eliminar en cualquier posición
- Se visualiza horizontalmente con flechas entre elementos

### Funciones Principales

#### a) Función `insertarInicio()`

```javascript
insertarInicio() {
    const val = this.getVal();
    if (val === null) return;
    this.datos.unshift(val);  // Agrega al inicio
    this.render();
    this.mostrarCodigo(CODIGO_C.inicio);
    this.mensaje(`Insertado inicio: ${val}`);
}
```

**Explicación:**
1. `this.datos.unshift(val)`: Agrega el elemento al inicio del array (concepto UNSHIFT: inserta al principio)
2. Diferencia con push(): push() agrega al final, unshift() al inicio

#### b) Función `insertarFinal()`

```javascript
insertarFinal() {
    const val = this.getVal();
    if (val === null) return;
    this.datos.push(val);  // Agrega al final
    this.render();
    this.mostrarCodigo(CODIGO_C.final);
    this.mensaje(`Insertado final: ${val}`);
}
```

**Explicación:**
- Similar a encolar() en cola
- Agrega al final del array

#### c) Función `mostrarModalPosicion(operacion)`

```javascript
mostrarModalPosicion(operacion) {
    this.operacionPosicion = operacion;
    const modal = document.getElementById('posicionModal');
    const titulo = document.getElementById('modalTitulo');
    titulo.textContent = operacion === 'insertar' ? 'Insertar en Posición' : 'Eliminar por Posición';
    modal.classList.add('active');
    document.getElementById('inputPosicion').focus();
}
```

**Explicación:**
1. Almacena en `operacionPosicion` qué operación se va a realizar
2. Modifica el título del modal según la operación
3. Muestra el modal (agrega clase 'active')
4. Coloca el foco en el input de posición

#### d) Función `confirmarPosicion()`

```javascript
confirmarPosicion() {
    const posicion = parseInt(document.getElementById('inputPosicion').value);
    document.getElementById('posicionModal').classList.remove('active');
    
    if (this.operacionPosicion === 'insertar') {
        const val = this.getVal();
        if (val === null) return;
        if (posicion < 0 || posicion > this.datos.length) {
            return this.mensaje('Posición inválida');
        }
        this.datos.splice(posicion, 0, val);  // Inserta en posición
        this.render();
        this.mostrarCodigo(CODIGO_C.inicio);
        this.mensaje(`Insertado en posición ${posicion}: ${val}`);
    } else if (this.operacionPosicion === 'eliminar') {
        if (posicion < 0 || posicion >= this.datos.length) {
            return this.mensaje('Posición inválida');
        }
        const val = this.datos[posicion];
        this.datos.splice(posicion, 1);  // Elimina en posición
        this.render();
        this.mostrarCodigo(CODIGO_C.eliminar);
        this.mensaje(`Eliminado de posición ${posicion}: ${val}`);
    }
}
```

**Explicación:**
1. `parseInt()`: Convierte el string a número entero
2. `this.datos.splice(posicion, 0, val)`: 
   - Modifica el array directamente
   - Primer parámetro: posición donde hacer cambio
   - Segundo parámetro: cuántos elementos eliminar (0 = no eliminar)
   - Tercer parámetro: qué elemento insertar
   - Concepto SPLICE: modifica el array insertando o eliminando elementos
3. `this.datos.splice(posicion, 1)`:
   - Elimina 1 elemento en la posición especificada
   - El segundo parámetro (1) indica cuántos eliminar

#### e) Función `eliminarInicio()`

```javascript
eliminarInicio() {
    if (this.datos.length === 0) return this.mensaje('Lista vacía');
    const val = this.datos.shift();  // Elimina el primero
    this.render();
    this.mostrarCodigo(CODIGO_C.eliminar);
    this.mensaje(`Eliminado del inicio: ${val}`);
}
```

**Explicación:**
- `shift()`: Elimina el primer elemento del array
- Es lo opuesto a unshift()

#### f) Función `eliminarFinal()`

```javascript
eliminarFinal() {
    if (this.datos.length === 0) return this.mensaje('Lista vacía');
    const val = this.datos.pop();  // Elimina el último
    this.render();
    this.mostrarCodigo(CODIGO_C.eliminar);
    this.mensaje(`Eliminado del final: ${val}`);
}
```

**Explicación:**
- `pop()`: Elimina el último elemento del array
- Es lo opuesto a push()

#### g) Función `render()` (Lista)

```javascript
render() {
    const div = document.getElementById('lienzo');
    div.innerHTML = '';
    if (this.datos.length === 0) {
        div.innerHTML = 'NULL';
        return;
    }
    this.datos.forEach(d => {
        const caja = document.createElement('div');
        caja.style.border = "1px solid black";
        caja.style.padding = "5px";
        caja.className = 'nodo-entrada';
        caja.textContent = d;
        div.appendChild(caja);
        
        const flecha = document.createElement('span');
        flecha.innerHTML = "&rarr;";
        div.appendChild(flecha);
    });
    const fin = document.createElement('span');
    fin.textContent = "NULL";
    div.appendChild(fin);
}
```

**Explicación:**
1. Por cada elemento, crea un div con su valor
2. Agrega una flecha (->)
3. Al final, agrega "NULL" para indicar el final de la lista
4. **Concepto HTML Entity:** `&rarr;` es una entidad HTML que representa la flecha derecha (→)

---

## Conceptos Clave de JavaScript

### 1. Array Methods (Métodos de Array)

- **push()**: Agrega elemento al final. Retorna la nueva longitud
- **pop()**: Elimina el último elemento. Retorna el elemento eliminado
- **unshift()**: Agrega elemento al inicio. Retorna la nueva longitud
- **shift()**: Elimina el primer elemento. Retorna el elemento eliminado
- **splice(pos, deleteCount, item1, ...)**: Modifica el array eliminando e insertando. Retorna array de elementos eliminados
- **slice()**: Crea una copia del array sin modificar el original
- **reverse()**: Invierte el orden del array
- **join(separator)**: Convierte array a string con separador
- **split(separator)**: Convierte string a array usando separador
- **forEach()**: Ejecuta función para cada elemento
- **map()**: Transforma cada elemento aplicando una función, retorna nuevo array
- **filter()**: Selecciona elementos que cumplen condición, retorna nuevo array
- **indexOf()**: Busca posición de elemento, retorna índice o -1

### 2. Operaciones del DOM (Document Object Model)

- **document.getElementById(id)**: Busca elemento por ID
- **element.innerHTML**: Obtiene o modifica contenido HTML
- **element.textContent**: Obtiene o modifica contenido de texto
- **element.value**: Obtiene o modifica valor de input
- **element.classList.add(class)**: Agrega clase CSS
- **element.classList.remove(class)**: Elimina clase CSS
- **document.createElement(tag)**: Crea nuevo elemento HTML
- **element.appendChild(child)**: Agrega elemento hijo
- **element.style.propiedad**: Asigna estilo CSS inline
- **element.className**: Asigna o modifica clases

### 3. File API

- **FileReader()**: Lee archivos del navegador
- **reader.readAsText(file)**: Lee archivo como texto
- **reader.onload**: Evento que se dispara al terminar la lectura
- **Blob**: Objeto que representa datos binarios

### 4. Expresiones Regulares

- **isNaN(valor)**: Verifica si NO es un número
- **parseInt(string)**: Convierte string a número entero
- **trim()**: Elimina espacios al inicio y final

---

## Interactividad de la Aplicación

### Flujo Usuario:

1. Entra a Proyecto.html
2. Selecciona una estructura (Pila, Cola o Lista)
3. Realiza operaciones (insertar/eliminar)
4. Ve visualización en tiempo real
5. Ve código C equivalente
6. Puede descargar datos como TXT
7. Puede cargar datos desde TXT

### Validaciones:

- Verifica que los inputs sean números
- Verifica que los archivos tengan formato correcto
- Verifica que no haya operaciones en estructuras vacías
- Verifica que las posiciones sean válidas en listas

---

## Estructura de Archivos

```
Proyecto/
├── Proyecto.html          (Página de inicio)
├── Pila.html              (Estructura Pila)
├── Cola.html              (Estructura Cola)
├── Lista.html             (Estructura Lista)
├── styles.css             (Estilos compartidos)
└── README.md              (Este archivo)
```

---

## Cambios Recientes (Actualización - Enero 2026)

### 🎨 Integración de Bootstrap 5.3.0

Se ha integrado el framework CSS **Bootstrap 5.3.0** para mejorar la interfaz de usuario y proporcionar componentes visuales más profesionales.

#### Cambios en los Archivos HTML:

1. **CDN de Bootstrap agregado** (en la sección `<head>`):
```html
<link href="https://cdnjs.cloudflare.com/ajax/libs/bootstrap/5.3.0/css/bootstrap.min.css" rel="stylesheet">
```

2. **JavaScript de Bootstrap agregado** (al final de `</body>`):
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/bootstrap/5.3.0/js/bootstrap.bundle.min.js"></script>
```

3. **Clases de Bootstrap aplicadas a elementos interactivos:**

| Tipo | Clase | Color | Uso |
|------|-------|-------|-----|
| Principal | `btn btn-primary` | Púrpura | Push, Encolar, Confirmar |
| Peligro | `btn btn-danger` | Rojo | Pop, Desencolar, Eliminar |
| Éxito | `btn btn-success` | Verde | Crear Nueva, Insertar |
| Información | `btn btn-info` | Azul | Descargar, Cargar archivos |
| Inputs | `form-control` | - | Campos de entrada de datos |

#### Ejemplos de implementación:

**Antes:**
```html
<button onclick="app.push()">Push</button>
<input type="text" id="inputPila" placeholder="Ingrese un número">
```

**Después:**
```html
<button onclick="app.push()" class="btn btn-primary">Push</button>
<input type="text" id="inputPila" placeholder="Ingrese un número" class="form-control d-inline-block">
```

---

### ✨ Nuevas Animaciones de Entrada y Salida

Se han creado y mejorado animaciones CSS para proporcionar feedback visual cuando aparecen o desaparecen elementos interactivos.

#### Keyframes Agregados:

**1. `slideIn` - Entrada con escala y movimiento vertical**
```css
@keyframes slideIn {
    from {
        opacity: 0;
        transform: scale(0.8) translateY(-20px);
    }
    to {
        opacity: 1;
        transform: scale(1) translateY(0);
    }
}
```
- **Duración:** 0.5s
- **Timing:** cubic-bezier(0.34, 1.56, 0.64, 1) - Efecto elástico
- **Uso:** Entrada de nodos, modal-box

**2. `slideOut` - Salida suave**
```css
@keyframes slideOut {
    from {
        opacity: 1;
        transform: scale(1) translateY(0);
    }
    to {
        opacity: 0;
        transform: scale(0.8) translateY(-20px);
    }
}
```
- **Duración:** 0.4s
- **Timing:** ease-out
- **Uso:** Salida de nodos cuando se eliminan

**3. `fadeInUp` - Desvanecimiento hacia arriba**
```css
@keyframes fadeInUp {
    from {
        opacity: 0;
        transform: translateY(20px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}
```
- **Duración:** 0.4s - 0.6s
- **Uso:** Contenedores principales (visualization-container, code-container)

**4. `slideInLeft` - Deslizamiento desde la izquierda**
```css
@keyframes slideInLeft {
    from {
        opacity: 0;
        transform: translateX(-30px);
    }
    to {
        opacity: 1;
        transform: translateX(0);
    }
}
```
- **Duración:** 0.5s
- **Uso:** Control-section

**5. `slideOutRight` - Deslizamiento a la derecha**
```css
@keyframes slideOutRight {
    from {
        opacity: 1;
        transform: translateX(0);
    }
    to {
        opacity: 0;
        transform: translateX(30px);
    }
}
```
- **Uso:** Elementos cuando salen de la pantalla

---

### 🎯 Efectos Hover Mejorados

**Botones:**
```css
button:hover {
    transform: translateY(-2px);
    box-shadow: 0 6px 12px rgba(124, 58, 237, 0.3);
    background: linear-gradient(135deg, #8b5cf6 0%, #7c3aed 100%);
}
```
- Levantamiento visual de 2px
- Sombra aumentada
- Gradiente de color más intenso

**Nodos/Elementos (Pila, Cola, Lista):**
```css
#lienzo[data-structure="pila"] div:hover {
    transform: translateY(-4px) scale(1.05);
    box-shadow: 0 8px 16px rgba(124, 58, 237, 0.3);
}
```
- Levantamiento de 4px
- Escala 105%
- Sombra profunda

**Tarjetas (Structure Card):**
```css
.structure-card:hover {
    transform: translateY(-8px) scale(1.02);
    box-shadow: 0 16px 32px rgba(107, 70, 193, 0.3);
}
```
- Levantamiento de 8px
- Escala 102%
- Sombra muy pronunciada

**Contenedores:**
```css
.visualization-container:hover,
.code-container:hover {
    box-shadow: 0 8px 20px rgba(107, 70, 193, 0.2);
    transform: translateY(-2px);
}
```
- Sombra envolvente
- Levantamiento suave

---

### 🎨 Mejoras Visuales en Modales

**Modal-overlay:**
- Agregado `backdrop-filter: blur(4px)` para efecto de desenfoque de fondo
- Animación de entrada `fadeInUp`
- Sombra aumentada de `0 20px 60px`

**Modal-box:**
- Animación de entrada con elasticidad
- Sombra profunda
- Bordes redondeados de 12px

---

### 📋 Clases CSS Personalizadas Agregadas

```css
/* Soporte para Bootstrap spacing utilities */
.me-2 { margin-right: 0.5rem; }
.mb-3 { margin-bottom: 1rem; }
.d-inline-block { display: inline-block !important; }

/* Personalización de form-control */
.form-control {
    border: 2px solid #d8b4fe !important;
    border-radius: 6px !important;
    transition: all 0.3s ease !important;
}

.form-control:focus {
    border-color: #7c3aed !important;
    box-shadow: 0 0 0 3px rgba(124, 58, 237, 0.1) !important;
}
```

---

### 📁 Archivos Modificados

| Archivo | Cambios |
|---------|---------|
| `Proyecto.html` | CDN Bootstrap, script bundle |
| `Pila.html` | CDN Bootstrap, clases btn, form-control |
| `Cola.html` | CDN Bootstrap, clases btn, form-control |
| `Lista.html` | CDN Bootstrap, clases btn, form-control |
| `styles.css` | Nuevos keyframes, efectos hover, clases Bootstrap |

---

### 🚀 Beneficios de estos cambios

1. **Mejor UX**: Animaciones suaves y feedback visual claro
2. **Consistencia**: Bootstrap proporciona componentes estandarizados
3. **Accesibilidad**: Mejor contraste y tamaños de botón estándar
4. **Responsividad**: Bootstrap mejora adaptación a dispositivos
5. **Profesionalidad**: Interfaz más pulida y moderna

---



- La aplicación usa Vanilla JavaScript (sin frameworks)
- Los datos se almacenan en arrays de JavaScript
- Los estilos usan paleta de colores en morado (tema principal)
- La visualización es en tiempo real sin recargar la página
- Los archivos TXT se descargan con formato: dato1,dato2,dato3
- **Nuevo:** Bootstrap 5.3.0 integrado para componentes UI estándar
- **Nuevo:** Animaciones CSS personalizadas para entrada/salida de elementos
- **Nuevo:** Efectos hover mejorados con transformaciones suaves
- **Nuevo:** Backdrop blur en modales para mejor enfoque visual
