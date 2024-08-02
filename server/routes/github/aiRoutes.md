---
  title: 'aiRoutes.mjs'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # aiRoutes.mjs
  # Explicación de código en Markdown

El código proporcionado es un archivo de enrutador de Express que maneja dos rutas diferentes para procesar archivos y transformar código. A continuación se detalla su funcionamiento:

1. **Importación de módulos y dependencias**:
   - Se importan los módulos `express` y `multer` de las bibliotecas externas.
   - Se importan las funciones `createFile` y `transformCode` desde el archivo `aiHelper.mjs`.
   - Se importa la función `ensureAuthenticated` desde el archivo `authMiddleware.mjs`.

2. **Configuración de Express**:
   - Se crea un nuevo enrutador utilizando `express.Router()`.
   - Se inicializa el middleware `multer` para manejar la carga de archivos.

3. **Ruta "/test-prompt"**:
   - Se define una ruta `POST` que requiere autenticación mediante `ensureAuthenticated`.
   - Se utiliza el middleware `upload.single("file")` para procesar la carga de un solo archivo.
   - Se extraen los datos del modelo y las instrucciones del cuerpo de la solicitud.
   - Se verifica si se ha cargado un archivo y se devuelve un mensaje de error si no.
   - Se lee el contenido del archivo cargado y se convierte a una cadena de texto.
   - Se llama a la función `createFile` con el contenido, el modelo y las instrucciones.
   - Se envía una respuesta JSON con el nombre del archivo original y la explicación generada.

4. **Ruta "/transform-code"**:
   - Se define otra ruta `POST` que requiere autenticación mediante `ensureAuthenticated`.
   - Se extraen el contenido y las instrucciones de transformación del cuerpo de la solicitud.
   - Se llama a la función `transformCode` con el contenido y las instrucciones de transformación.
   - Se envía la respuesta con el texto transformado.

5. **Manejo de errores**:
   - En caso de que ocurra un error durante el procesamiento de archivos o la transformación de código, se registra en la consola y se envía una respuesta de error al cliente.

6. **Ejemplo de uso**:
   - Para utilizar este código, se debe montar el enrutador en una aplicación Express y definir las funciones `createFile` y `transformCode` en el archivo `aiHelper.mjs`. Además, se necesita la función `ensureAuthenticated` del archivo `authMiddleware.mjs`.

```javascript
import express from "express";
import router from "./router.js";

const app = express();

app.use(express.json());
app.use(router);

app.listen(3000, () => {
  console.log("Server is running on port 3000");
});
```

En este ejemplo, se crea una aplicación Express, se configura para usar JSON y se monta el enrutador definido en el código proporcionado. Finalmente, se inicia el servidor en el puerto 3000.
  
  ## Component Code
  ```jsx
  import express from "express";
import multer from "multer";
import { createFile, transformCode } from "./helpers/aiHelper.mjs";
import { ensureAuthenticated } from "./middleware/authMiddleware.mjs";

const router = express.Router();
const upload = multer();

router.post(
  "/test-prompt",
  ensureAuthenticated,
  upload.single("file"),
  async (req, res) => {
    const { model, instructions } = req.body;

    if (!req.file) {
      return res.status(400).send("No file uploaded.");
    }

    const file = req.file;
    const content = file.buffer.toString("utf8");

    try {
    

      const completion = await createFile(content, model, instructions);
      const explanation = completion.text;
      console.log(completion.text);
      res.json({
        message: "File processed successfully!",
        data: {
          fileName: file.originalname,
          prompt: explanation,
        },
      });
    } catch (error) {
      console.error("Failed to process file:", error);
      res.status(500).send("Failed to process file");
    }
  }
);


router.post('/transform-code', ensureAuthenticated, async (req, res) => {
  const { content, transformInstructions } = req.body;

  try {
      const result = await transformCode(content, transformInstructions);
      console.log(result.text)
      res.json(result.text);
  } catch (error) {
      console.error('Failed to transform code:', error);
      res.status(500).send('Failed to transform code');
  }
});


export default router;
  ```
  