# Composición del CRUD a desarrollar

## Obtener todos los productos
En el primer endpoint deberás poder leer todos los productos que se encuentran en
la colección. Deberás definir también un control de errores por si la base de datos
no está disponible.

## Obtener un producto
El segundo endpoint deberá permitir obtener un producto por su :id. Recuerda que
solo debe mostrar uno solo que coincida, y también deberás incluir un control de
errores.

## Filtrar productos
En el tercer endpoint deberás permitir filtrar uno o más productos, por el nombre de
los mismos. La búsqueda por :nombre debe permitir ingresar un valor parcial del
mismo. Controla también cualquier posible error si no se encuentran coincidencias o
si la base de datos no está disponible.

## Agregar un nuevo producto
En el cuarto endpoint deberás permitir agregar un nuevo producto. Recuerda validar
cualquier posible error mediante una respuesta acorde.

Respecto a su código numérico, puedes generar uno a partir de la totalización + 1
de productos que existan en tu colección, o directamente crear alguna función de
retorno al azar.
Te dejamos un ejemplo a continuación:
Si tienes alguna solución innovadora para resolver el valor numérico del nuevo
producto, bienvenida.

## Modificar el precio de un producto
Este endpoint permitirá cambiar solamente el precio de un producto (no el resto de
la información del mismo). Recuerda que PUT es para modificar uno o más valores
de un producto, y PATCH es para modificar alguno o algunos de los valores de un
producto. Utiliza esta última funcionalidad.
Agrega también un control de errores acorde para controlar cualquier inconveniente
que surja en este proceso.

# Control de errores
Al solicitar controlar errores en cada endpoint a crear, ten presente que se deben
controlar errores en la estructura que se envía para realizar alguna operación, como
también en cualquier posible error de respuesta por parte de la base de datos.
Cada error aquí mencionado corresponde a un código de error específico.

También ten presente controlar el acceso a cualquier ruta no existente, con un error
acorde.

Para todo el control de errores solicitados, envía un mensaje de respuesta acorde y
también el código de error correspondiente.
