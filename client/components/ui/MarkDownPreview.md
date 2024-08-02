---
  title: 'MarkDownPreview'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # MarkDownPreview.jsx
  # Explicación del código en Markdown

El código proporcionado es un componente de React que muestra una vista previa de un archivo Markdown con funcionalidades adicionales como resaltado de sintaxis y eliminación de metadatos al inicio del archivo.

### Dependencias externas
- **Markdown**: Se utiliza para renderizar el contenido Markdown en el componente.
- **rehypeRaw**: Plugin de rehype que permite el uso de HTML sin procesar en el Markdown.
- **rehypeSanitize**: Plugin de rehype que limpia y sanitiza el HTML en el Markdown.
- **SyntaxHighlighter**: Componente de React utilizado para resaltar la sintaxis del código en el Markdown.
- **materialDark**: Estilo de resaltado de sintaxis proporcionado por react-syntax-highlighter.

### Funciones
1. **removeFrontMatter**: Función que elimina el front matter del contenido Markdown. El front matter es un bloque de metadatos al inicio del archivo delimitado por `---`.
2. **MarkdownComponents**: Objeto que define un componente personalizado para renderizar bloques de código en el Markdown. Si se detecta un lenguaje específico en el bloque de código, se utiliza SyntaxHighlighter para resaltarlo.

### Uso del componente
El componente `MarkDownPreview` recibe dos propiedades:
- **selectedFileContent**: Contenido Markdown a ser renderizado.
- **customStyle**: Estilo personalizado para el contenedor del Markdown (opcional).

Al renderizar, el componente elimina el front matter del contenido, define el componente personalizado para el resaltado de sintaxis y renderiza el contenido Markdown con las configuraciones especificadas.

### Ejemplo de uso
```jsx
import MarkDownPreview from './MarkDownPreview';

const App = () => {
    const markdownContent = `
        ---
        title: Ejemplo de Markdown
        description: Este es un ejemplo de Markdown
        pubDate: 2022-12-31
        author: Usuario
        ---

        # Título

        \`\`\`javascript
        const greeting = "Hola Mundo";
        console.log(greeting);
        \`\`\`
    `;

    return (
        <div>
            <MarkDownPreview selectedFileContent={markdownContent} customStyle="px-4 py-4" />
        </div>
    );
};
```

En este ejemplo, se muestra cómo se puede utilizar el componente `MarkDownPreview` para renderizar un contenido Markdown con un bloque de código resaltado en JavaScript.
  
  ## Component Code
  ```jsx
  import Markdown from 'react-markdown';
import rehypeRaw from 'rehype-raw';
import rehypeSanitize from 'rehype-sanitize';
import { Prism as SyntaxHighlighter } from 'react-syntax-highlighter';
import { materialDark } from 'react-syntax-highlighter/dist/esm/styles/prism';
import 'github-markdown-css/github-markdown.css';

const MarkDownPreview = ({ selectedFileContent, customStyle='px-10 pt-10 pb-32' }) => {
    // Function to remove the front matter
    const removeFrontMatter = (content) => {
        return content.replace(/---\n[\s\S]*?title:.*\n[\s\S]*?description:.*\n[\s\S]*?pubDate:.*\n[\s\S]*?author:.*\n[\s\S]*?---\n/, '');
    };

    // Clean the selected file content
    const cleanedContent = removeFrontMatter(selectedFileContent);

    const MarkdownComponents = {
        code({ inline, className, children, ...props }) {
            const match = /language-(\w+)/.exec(className || "");
            return !inline && match ? (
                <SyntaxHighlighter
                    style={materialDark}
                    PreTag="div"
                    language={match[1]}
                    // eslint-disable-next-line react/no-children-prop
                    children={String(children).replace(/\n$/, "")}
                    {...props}
                />
            ) : (
                <code className={className ? className : ""} {...props}>
                    {children}
                </code>
            );
        },
    };

    return (
        <div className={`markdown-body w-full h-full bg-white border border-2 border-black overflow-y-scroll  ${customStyle}`}>
            <Markdown
                rehypePlugins={[rehypeRaw, rehypeSanitize]}
                components={MarkdownComponents}
            >
                {cleanedContent}
            </Markdown>
            
        </div>
    );
};

export default MarkDownPreview;
  ```
  