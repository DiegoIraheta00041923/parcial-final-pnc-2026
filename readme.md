# Diego Alejandro Iraheta Monterrosa 00041923

## Indicaciones

Recientemente, se utilizó AI para crear un sistema de gestion de una biblioteca, el cual ha generado varios errores, su trabajo es arreglarlo. Dado el siguiente caso de uso, explique y/o resuelva cada problema según se le pida.

---

## Consideraciones

La libreria crea automaticamente un correo con los nombres de la persona

---

## Problemas

### 1. Filtro por autor y género (10%)

QA ha reportado que el endpoint para obtener los libros puede filtrar por **autor** y por **género**, o por cualquiera de los dos de manera individual.

Actualmente:

- Filtrar únicamente por autor funciona correctamente.
- Filtrar únicamente por género funciona correctamente.
- Filtrar por **autor y género al mismo tiempo** provoca que el servidor falle.

**Instrucción:** Explique la causa del problema y resuélvalo.

**Respuesta:**
En el BookRepository cuando se hace una búsqueda por autor y género como paramétro se utiliza un String para referirse al Género
Sin embargo genero es un enum. Se realizó un cambio para que dejara de usar un String y usara el Enum como en la funcion de findByGenre
En el service se pasa como String, pero se hace un ajuste para que busque según el enum.


---

### 2. Error al volver a prestar un libro (10%)

Un usuario reportó que al pedir prestado el libro **The Selfish Gene**, devolverlo e intentar pedirlo prestado nuevamente, el servidor falla.

**Instrucción:** Explique la causa del problema y resuélvalo.

**Respuesta:**
Si me funciona, pero si no funciona es porque posiblemente no se maneje el stock, no se incremente de nuevo.
---

### 3. Cantidad de libros por género (10%)

Existe un endpoint que devuelve la cantidad de libros disponibles por género. Sin embargo, actualmente dicho endpoint falla.

**Instrucción:** Explique la causa del problema y resuélvalo.

**Respuesta:**
Da problemas debido a que hay un solo libro que no tiene género, por lo tanto crashea al hacer el mapeo.
La solución es hacer una validación, si el género no es nulo se toma en cuenta para hacer el conteo
---

### 4. Error al consultar un libro por ID (10%)

Un miembro del equipo de frontend reporta que la siguiente llamada falla:

```http
GET /books?id=ed16ed1e-7017-4697-a08a-d28c09a74acf
```

**Instrucción:** Explique la causa del problema.

**Respuesta:**
A la hora de hacer el llamado se traen todos los libros (no se si todos pero no trae uno en específico), en este caso en el controller
no se toma como un Request Param sino como un Path Variable, entonces la ruta adecuada debe ser /books/ed16ed1e-7017-4697-a08a-d28c09a74acf
Sería un error del llamdo más no implementación
---

### 5. Error al crear un libro (10%)

QA ha reportado que el siguiente payload enviado al endpoint `POST /books` provoca un error:

```json
{
  "title": "Clean Code",
  "author": "Robert C. Martin",
  "genre": "classic",
  "isbn": "978-0132350884",
  "available": true,
  "availableCount": 5
}
```

**Instrucción:** Explique la causa del problema.

**Respuesta:**
A la hora de hacerle post al objeto da problema debido al genre, al estar usando un enum (Genre) el género debería escribirse en mayúsculas
por como funcionan los enum. El cambio que se debe hacer para que funcione es que en lugar de "classic" sea "CLASSIC"
---

### 6. Devolución de libros no prestados (20%)

QA ha reportado que un usuario es capaz de devolver libros que nunca ha solicitado en préstamo.

**Instrucción:**

- Confirme si este comportamiento es realmente posible.
- Si es posible, explique la causa y resuelva el problema.
- Si no es posible, explique por qué, haciendo referencia al código correspondiente.

**Respuesta:**
Si es posible el comportamiento, es posible debido a que no hay una comprobación de que ese libro este prestado a dicho usuario
por eso siempre se crea una devolución. 

---