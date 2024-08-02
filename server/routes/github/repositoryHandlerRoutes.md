---
  title: 'repositoryHandlerRoutes.mjs'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # repositoryHandlerRoutes.mjs
  # Explicación de código en Markdown

El código proporcionado es un archivo de rutas de un servidor web utilizando Express en un entorno de Node.js. A continuación se detalla la funcionalidad de cada ruta y las operaciones que realiza:

### Dependencias utilizadas:
- **Express**: Framework de Node.js para construir aplicaciones web.
- **Octokit**: Cliente para la API de GitHub.
- **Multer**: Middleware para manejar la carga de archivos.
- **Helpers personalizados**: Archivos con funciones de ayuda para diferentes tareas.

### Funciones y rutas:

1. **`setupOctokit(token)`**: Función que inicializa Octokit con un token de autenticación.

2. **`/create-repository`** (POST): Ruta para crear un repositorio en GitHub.
   - Obtiene el token de acceso del usuario autenticado.
   - Crea un repositorio con nombre y descripción proporcionados.
   - Crea un archivo README.md inicial en el repositorio.
   - Devuelve la información del repositorio creado.

3. **`/create-folder`** (POST): Ruta para crear una estructura de carpetas en un repositorio.
   - Obtiene el token de acceso del usuario y la estructura de carpetas.
   - Crea las carpetas con un archivo `.gitkeep` dentro de cada una.

4. **`/repository-contents`** (GET): Ruta para obtener el contenido de un repositorio.
   - Obtiene el token de acceso del usuario y el nombre del repositorio.
   - Devuelve una lista de archivos y carpetas en el repositorio.

5. **`/repositories/library`** (GET): Ruta para obtener una lista de repositorios del usuario que contienen "library" en el nombre.
   - Obtiene el token de acceso del usuario y su nombre de usuario.
   - Filtra los repositorios y devuelve la lista.

6. **`/upload-file`** (POST): Ruta para subir archivos a un repositorio en GitHub.
   - Obtiene los archivos subidos, la ruta de destino, el modelo y las instrucciones.
   - Procesa cada archivo, crea un archivo Markdown con su contenido y lo sube al repositorio.
   - Devuelve un mensaje de éxito y la información de los archivos subidos.

### Ejemplo de uso:

```javascript
// Importar el archivo de rutas
import router from "./routes";

// Usar las rutas en una aplicación Express
const app = express();
app.use("/api", router);

// Iniciar el servidor
app.listen(3000, () => {
  console.log("Servidor iniciado en el puerto 3000");
});
```

Este código define diferentes rutas para interactuar con la API de GitHub, como crear repositorios, subir archivos y obtener información de repositorios. Cada ruta realiza operaciones específicas utilizando el cliente Octokit y otras funciones auxiliares definidas en archivos separados.
  
  ## Component Code
  ```jsx
  import express from "express";
import { Octokit } from "@octokit/rest";
import { ensureAuthenticated } from "../middleware/authMiddleware.mjs";
import multer from "multer";
import { createFile } from "../helpers/aiHelper.mjs";
import {
  createMarkdown,
  createOrUpdateFile,
  fileExists,
  initialReadme,
  sendProgressUpdate,
  handleError
} from "../helpers/helpers.mjs"
import { sanitizeCreateRepository } from "../helpers/formValidations.mjs";


const router = express.Router();
const storage = multer.memoryStorage();
const upload = multer({ storage: storage });

const setupOctokit = (token) => new Octokit({ auth: token });



router.post("/create-repository", ensureAuthenticated, async (req, res) => {
  const token = req.user.accessToken;
  const octokit = setupOctokit(token);

  try {
    let { name, description } = req.body;

    const { sanitizedTitle, sanitizedDescription } = sanitizeCreateRepository(
      name,
      description
    );
    const repoName = `library-${sanitizedTitle}`;

    const response = await octokit.rest.repos.createForAuthenticatedUser({
      name: repoName,
      description: sanitizedDescription,
      private: false,
      auto_init: true,
    });

    const readmeResponse = await octokit.rest.repos.getContent({
      owner: response.data.owner.login,
      repo: repoName,
      path: "README.md",
    });

    const readmeContent = initialReadme(
      repoName,
      sanitizedDescription,
      new Date().getFullYear(),
      req.user.profile.displayName || req.user.profile.username
    );

    await octokit.rest.repos.createOrUpdateFileContents({
      owner: response.data.owner.login,
      repo: repoName,
      path: "README.md",
      message: "Update README with initial content",
      content: Buffer.from(readmeContent).toString("base64"),
      sha: readmeResponse.data.sha,
    });

    res.json({
      message: "Repository created successfully!",
      repository: response.data,
      repositoryUrl: response.data.html_url,
    });
  } catch (error) {
    handleError(res, "Failed to create repository", error);
  }
});

router.post("/create-folder", ensureAuthenticated, async (req, res) => {
  const token = req.user.accessToken;
  const { repository, structure } = req.body;
  const octokit = setupOctokit(token);

  try {
    for (const folder of structure.structure) {
      const path = `${folder}/.gitkeep`;
      await createOrUpdateFile(octokit, req.user.profile.username, repository, path, "Create folder with .gitkeep", "");
    }
    res.json({ message: "Selected folder structure created successfully!" });
  } catch (error) {
    handleError(res, "Failed to create selected folder structure", error);
  }
});

router.get("/repository-contents", ensureAuthenticated, async (req, res) => {
  const token = req.user.accessToken;
  const { repo, path = "" } = req.query;

  if (!repo) return res.status(400).send("Repository name is required.");

  const octokit = setupOctokit(token);

  try {
    const response = await octokit.rest.repos.getContent({
      owner: req.user.profile.username,
      repo: repo,
      path: path,
      ref: "main",
    });

    const contents = Array.isArray(response.data)
      ? response.data
      : [response.data];

    res.json({
      message: "Repository contents fetched successfully!",
      contents: contents.map((item) => ({
        name: item.name,
        path: item.path,
        type: item.type,
        download_url: item.download_url || null,
        content: item.content
          ? Buffer.from(item.content, "base64").toString("utf-8")
          : null,
      })),
    });
  } catch (error) {
    handleError(res, "Failed to fetch repository contents", error);
  }
});

router.get("/repositories/library", ensureAuthenticated, async (req, res) => {
  const token = req.user.accessToken;
  const username = req.user.profile.username;
  const octokit = setupOctokit(token);

  try {
    const { data } = await octokit.rest.search.repos({
      q: `user:${username} library in:name`,
      sort: "stars",
      order: "desc",
    });

    const filteredRepos = data.items.filter(
      (repo) => !repo.name.startsWith(`library-${username}-config`)
    );

    res.json({
      message: "Repositories fetched successfully!",
      repositories: filteredRepos,
    });
  } catch (error) {
    handleError(res, "Failed to fetch repositories", error);
  }
});

router.post(
  "/upload-file",
  ensureAuthenticated,
  upload.array("files", 4),
  async (req, res) => {
    if (!req.files || req.files.length === 0) {
      return res.status(400).send("No files uploaded.");
    }

    const { repository, path: uploadPath, model, instructions } = req.body;
    const token = req.user.accessToken;
    const octokit = setupOctokit(token);
    const author = req.user.profile.displayName || req.user.profile.username;

    try {
      const results = [];
      let processedFiles = 0;

      for (const file of req.files) {
        const content = file.buffer.toString("utf8");
        const completion = await createFile(content, model, instructions);
        const explanation = completion.text;
        const markdownContent = createMarkdown(
          file.originalname,
          explanation,
          content,
          author
        );

        const cleanPath = uploadPath.startsWith("/")
          ? uploadPath.substring(1)
          : uploadPath;
        let baseFileName = file.originalname.replace(
          /\.(js|jsx|ts|tsx|py|java|rb|php|html|css|cpp|c|go|rs|swift|kt|m|h|cs|json|xml|sh|yml|yaml|vue|svelte|qwik|sv|astro|mjs)$/,
          ""
        );
        let extension = ".md";
        let markdownFilePath = `${cleanPath}/${baseFileName}${extension}`;

        let version = 1;
        while (
          await fileExists(
            octokit,
            req.user.profile.username,
            repository,
            markdownFilePath
          )
        ) {
          markdownFilePath = `${cleanPath}/${baseFileName}_v${version}${extension}`;
          version++;
        }

        const response = await octokit.rest.repos.createOrUpdateFileContents({
          owner: req.user.profile.username,
          repo: repository,
          path: markdownFilePath,
          message: `Upload file ${file.originalname}`,
          content: Buffer.from(markdownContent).toString("base64"),
        });

        results.push(response.data);

        processedFiles++;
        sendProgressUpdate(processedFiles, req.files.length);

        if (model !== "3.5-turbo") {
          await new Promise((resolve) => setTimeout(resolve, 500));
        }
      }

      res.json({
        message: "Files uploaded successfully!",
        data: results,
      });
    } catch (error) {
      console.error("Error during file upload:", error);
      handleError(res, "Failed to upload files", error);
    }
  }
);



export default router;
  ```
  