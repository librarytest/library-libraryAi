---
  title: 'helpers.mjs'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # helpers.mjs
  # Explicación del Código

El código proporcionado es un conjunto de funciones en JavaScript que se utilizan para crear y actualizar archivos Markdown, verificar la existencia de archivos, generar contenido inicial para un archivo README y enviar actualizaciones de progreso a través de WebSockets.

### Función `getCurrentDateFormatted()`
Esta función devuelve la fecha actual formateada en el formato "mes día, año" (por ejemplo, "June 1, 2022").

### Función `createMarkdown(filename, explanation, code, author)`
Esta función crea contenido Markdown completo con metadatos (frontmatter), título, explicación y código. Toma como parámetros el nombre del archivo, la explicación, el código y el autor.

### Función `createOrUpdateFile(octokit, owner, repo, path, message, content)`
Esta función crea un nuevo archivo si no existe o actualiza un archivo existente en un repositorio de GitHub utilizando la API de GitHub. Toma como parámetros el cliente Octokit, el propietario del repositorio, el repositorio, la ruta del archivo, el mensaje de commit y el contenido del archivo.

### Función `fileExists(octokit, owner, repo, path)`
Esta función verifica si un archivo existe en un repositorio de GitHub utilizando la API de GitHub. Toma como parámetros el cliente Octokit, el propietario del repositorio, el repositorio y la ruta del archivo.

### Función `initialReadme(repoName, sanitizedDescription, currentYear, fullName)`
Esta función genera el contenido inicial para un archivo README de un repositorio. Incluye información sobre el repositorio, su descripción, el año actual y el nombre completo del autor.

### Función `sendProgressUpdate(processedFiles, totalFiles)`
Esta función calcula y envía actualizaciones de progreso a través de WebSockets a los clientes conectados. Muestra el progreso en porcentaje y envía la información a cada cliente conectado.

### Función `handleError(res, message, error)`
Esta función maneja errores, mostrando un mensaje de error en la consola y devolviendo un estado de error 500 al cliente.

### Dependencias Externas
- `wss`: Se asume que esta variable global `wss` hace referencia a un servidor de WebSockets para enviar actualizaciones de progreso a los clientes.

### Ejemplo de Uso
```javascript
import { createMarkdown, initialReadme, sendProgressUpdate } from "./utils.js";

const markdownContent = createMarkdown("example.jsx", "This is an example component", "const example = () => { return <div>Hello, World!</div> }", "John Doe");
console.log(markdownContent);

const readmeContent = initialReadme("MyRepo", "A repository for storing code snippets", 2022, "John Doe");
console.log(readmeContent);

sendProgressUpdate(5, 10);
``` 

En este ejemplo, se importan las funciones necesarias del archivo `utils.js` y se utilizan para crear contenido Markdown, generar un README inicial y enviar una actualización de progreso.
  
  ## Component Code
  ```jsx
  import { wss } from "../../index.mjs";


function getCurrentDateFormatted() {
  const date = new Date();
  const options = { year: "numeric", month: "long", day: "numeric" };
  return date.toLocaleDateString("en-US", options);
}

// Function to create comprehensive Markdown content
export function createMarkdown(filename, explanation, code, author) {
  const currentDate = getCurrentDateFormatted();
  // Frontmatter added to the markdown content
  const frontmatter = `---
  title: '${filename.replace(".jsx", "")}'
  description: 'component description'
  pubDate: '${currentDate}'
  author: '${author}'
  ---
  
  `;
  return `${frontmatter}
  
  # ${filename}
  ${explanation}
  
  ## Component Code
  \`\`\`jsx
  ${code.trim()}
  \`\`\`
  `;
}

export async function createOrUpdateFile(
  octokit,
  owner,
  repo,
  path,
  message,
  content
) {
  try {
    const { data } = await octokit.rest.repos.getContent({ owner, repo, path });
    // Update the existing file with the correct sha
    return octokit.rest.repos.createOrUpdateFileContents({
      owner,
      repo,
      path,
      message,
      content: Buffer.from(content).toString("base64"),
      sha: data.sha,
    });
  } catch (error) {
    if (error.status === 404) {
      // Create the file if it does not exist
      return octokit.rest.repos.createOrUpdateFileContents({
        owner,
        repo,
        path,
        message,
        content: Buffer.from(content).toString("base64"),
      });
    } else {
      throw error;
    }
  }
}

export async function fileExists(octokit, owner, repo, path) {
  try {
    await octokit.rest.repos.getContent({
      owner,
      repo,
      path,
    });
    return true;
  } catch (error) {
    if (error.status === 404) {
      return false;
    }
    throw error;
  }
}

export function initialReadme(
  repoName,
  sanitizedDescription,
  currentYear,
  fullName
) {
  return `
# ${repoName}

${sanitizedDescription}

## About

This repository is created with the Code-Library-App, an auto-documentation software powered by AI. It allows you to store your code in markdown files, creating documentation and storing them in GitHub. This tool is useful for creating UI, tutorials, guides, or simply storing and ensuring you don't lose your code or component code.

With Code Library, you can easily drop your code, create documentation, and build your guide or component library. Find all your components in one place and never lose your code.

MIT License

Copyright (c) ${currentYear} ${fullName}

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
`;
}


export  function sendProgressUpdate  (processedFiles, totalFiles)  {
  const progress = (processedFiles / totalFiles) * 100;
  console.log(`Processed Files: ${processedFiles}, Progress: ${progress}%`);
  wss.clients.forEach((client) => {
    if (client.readyState === 1) {
      console.log(`Sending progress to client ${client.userId}: ${progress}%`);
      client.send(JSON.stringify({ progress }));
    }
  });
};


export function handleError  (res, message, error)  {
  console.error(message, error);
  res.status(500).send(message);
};
  ```
  