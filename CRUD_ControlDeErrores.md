#  Composición del CRUD a desarrollar 🗃

## 🛍 Obtener todos los productos
- [x] En el primer endpoint deberás poder leer todos los productos que se encuentran en
la colección. Deberás definir también un control de errores por si la base de datos
no está disponible.
```javascript
// API listar todos los registros del servidor
app.get('/prendas', async (req, res) => {
    const client = await connectToMongoDB();
    if (!client) {
        res.status(500).send('Error al conectarse a MongoDB');
        return('Error al conectarse a MongoDB');
    } 
        
    const db = client.db(mibd); //nombre base de datos a conectarse
    const prendas = await db.collection(micoll).find().toArray();
        
    console.log('obteniendo datos')
         
    res.json(prendas);

});
```
## 👚 Obtener un producto
- [x] El segundo endpoint deberá permitir obtener un producto por su :id. (Nosotros nos vasamos en el campo _*Codigo*_) Recuerda que
solo debe mostrar uno solo que coincida, y también deberás incluir un control de
errores.
```javascript
// find x codigo
app.get('/prendas/codigo/:codigo', async (req, res) => {
    const prendaId = parseInt(req.params.codigo); //parse.int es para convertir el dato de string a integer
    
    if (isNaN(prendaId)) {  //este if se incluye para verificar si prendaId contiene un numero, en caso negativo se devuelve un aviso al usuario
        console.log(prendaId, ' no es un numero')
        res.status(400).send('Datos invalidos, por favor ingrese un valor numérico');
        return;
      } else {

    const client = await connectToMongoDB();
    if (!client) {
        res.status(500).send('Error al conectarse a MongoDB');
        return;
    } 

    const db = client.db(mibd); //nombre base de datos a conectarse
    const miColl = await db.collection(micoll); //nombre de la coleccion a utilizar
    
    const prenda = await miColl.find({codigo:prendaId}).toArray();

    if (!prenda){
        res.json(prenda)
        console.log('if')
    } else {
        res.json(prenda)
        console.log ('else')            
        }

  }
    }
  );
```

## 👖 Filtrar productos
- [x] En el tercer endpoint deberás permitir filtrar uno o más productos, por el nombre de
los mismos. La búsqueda por :nombre debe permitir ingresar un valor parcial del
mismo. Controla también cualquier posible error si no se encuentran coincidencias o
si la base de datos no está disponible.
```javascript
//Busqueda por nombre
app.get('/prendas/nombre/:nombre', async (req, res) => {
    const nombreprenda = req.params.nombre;
    const client = await connectToMongoDB();
         if (!client) {
             res.status(500).send('Error al conectarse a MongoDB');
              return;
         }

    const regex = new RegExp(nombreprenda.toLowerCase(), 'i')
    const db = client.db(mibd);
    const prenda = await db.collection(micoll).find({ nombre: regex }).toArray();
    await disconnectFromMongoDB();
         !prenda ? res.status(404).send(`No se encontró la prenda: ${nombreprenda}`) : res.json(prenda)
});
```
## 👗 Agregar un nuevo producto
- [x] En el cuarto endpoint deberás permitir agregar un nuevo producto. Recuerda validar
cualquier posible error mediante una respuesta acorde.
- [ ] Respecto a su código numérico, puedes generar uno a partir de la totalización + 1
de productos que existan en tu colección, o directamente crear alguna función de
retorno al azar.
```javascript
//   API Crear
app.post('/prendas', async (req, res) => {
    const nuevaprenda = req.body; //la prenda viene en el body
        if (nuevaprenda === undefined) {
            res.status(400).send('Error en el formato de datos a crear.');
        }

    const client = await connectToMongoDB();
        if (!client) {
            res.status(500).send('Error al conectarse a MongoDB');
        }

    const collection = client.db(mibd).collection(micoll); //obtener colección
          collection.insertOne(nuevaprenda)
            .then(() => {
                console.log('Nueva prenda creada:');
                res.status(201).send(nuevaprenda);
            })
            .catch(error => {
                console.error(error);
            })
            .finally(()=> {
                client.close();
            });
});
```
## 👚 Modificar el precio de un producto
- [x] Este endpoint permitirá cambiar solamente el precio de un producto (no el resto de
la información del mismo). Recuerda que PUT es para modificar uno o más valores
de un producto, y PATCH es para modificar alguno o algunos de los valores de un
producto. Utiliza esta última funcionalidad.
- [x] Agrega también un control de errores acorde para controlar cualquier inconveniente
que surja en este proceso.
```javascript
// API Modificar
app.patch('/prendas/codigo/:codigo', async (req, res) => {
    const codigo = parseInt(req.params.codigo);
    const nuevosDatos = await req.body;


    if (isNaN(codigo)) {  //este if se incluye para verificar si prendaId contiene un numero, en caso negativo se devuelve un aviso al usuario

        res.status(400).send('Datos invalidos, por favor ingrese un valor numérico');
        return;

      } else if (!nuevosDatos){  

        res.status(400).send('Error en el formato de datos recibido.')
    } else {
        const client = await connectToMongoDB();
        
        if (!client) {
            res.status(500).send('Error al conectarse a MongoDB');
        }
        
        const collection = client.db(mibd).collection(micoll);
        collection.updateOne({ codigo: codigo }, { $set: nuevosDatos })
        .then(() => {
            console.log('prenda modificada:');
            res.status(200).send(nuevosDatos);
        })
        .catch((error) => {
            res.status(500).json({descripcion: 'Error al modificar la prenda' });
        })
        .finally(()=> {
            client.close();
        });
    }
  });
```

#  Control de errores 🆗
- [x] Al solicitar controlar errores en cada endpoint a crear, ten presente que se deben
controlar errores en la estructura que se envía para realizar alguna operación, como
también en cualquier posible error de respuesta por parte de la base de datos.
Cada error aquí mencionado corresponde a un código de error específico.

- [x] También ten presente controlar el acceso a cualquier ruta no existente, con un error
acorde.

```javascript
app.use((req, res, next) => {
    console.log('app use error')
    res.status(404).send('Lo siento, la pagina solicitada no existe.');
    // next();
});

```
- [x] Para todo el control de errores solicitados, envía un mensaje de respuesta acorde y
también el código de error correspondiente.
