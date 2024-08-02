---
  title: 'DirStructureOptions'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # DirStructureOptions.jsx
  # Explicación del Código

El código proporcionado es un componente funcional de React que muestra diferentes opciones de estructuras de directorios para proyectos, con sus respectivas carpetas y subcarpetas. A continuación se detalla su funcionamiento:

1. **Importaciones**:
   - Se importa el ícono `FolderIcon` del paquete `@heroicons/react/24/outline`.
   - Se importa el hook `useLocalization` del contexto `LocalizationContext`.

2. **Textos en Inglés y Español**:
   - Se definen dos objetos, `englishText` y `spanishText`, que contienen las traducciones de los nombres de las estructuras de directorios en inglés y español respectivamente.

3. **Componente `DirStructureOptions`**:
   - Es un componente funcional que recibe dos propiedades: `onSelect` (función que se ejecuta al seleccionar una estructura) y `selectedStructure` (estructura seleccionada actualmente).
   - Utiliza el hook `useLocalization` para obtener el idioma actual (español o inglés).
   - Se selecciona el objeto de texto correspondiente al idioma actual.
   - Se define un array `folderStructures` que contiene objetos con el nombre de la estructura y un array con las carpetas que la componen.
   - Se renderiza una lista de estructuras de directorios, mostrando el nombre de la estructura y sus carpetas correspondientes.
   - Al hacer clic en una estructura, se ejecuta la función `onSelect` pasando como argumento la estructura seleccionada.

4. **Renderizado**:
   - Se renderiza un contenedor `div` con una clase de estilos CSS para mostrar las estructuras de directorios de forma horizontal y con un espacio entre ellas.
   - Por cada estructura en `folderStructures`, se renderiza un `div` con el nombre de la estructura, sus carpetas y un ícono de carpeta.
   - Al hacer clic en una estructura, se resalta visualmente y se activa la clase `border-primary`.

5. **Ejemplo de Uso**:
   ```jsx
   import DirStructureOptions from './DirStructureOptions';

   const App = () => {
     const handleSelectStructure = (structure) => {
       console.log(`Selected structure: ${structure.name}`);
     };

     return (
       <div>
         <h1>Choose Directory Structure:</h1>
         <DirStructureOptions onSelect={handleSelectStructure} selectedStructure={null} />
       </div>
     );
   };
   ```

En este ejemplo, se importa y se utiliza el componente `DirStructureOptions` en un componente principal de la aplicación. Al seleccionar una estructura de directorios, se muestra un mensaje en la consola con el nombre de la estructura seleccionada.
  
  ## Component Code
  ```jsx
  import { FolderIcon } from "@heroicons/react/24/outline";
import { useLocalization } from "../../../context/LocalizationContext";

const englishText = {
  defaultStructure: "Default Structure",
  extendedStructure: "Extended Structure",
  reactRecommended: "React Recommended",
  vue: "Vue",
  angular: "Angular",
};

const spanishText = {
  defaultStructure: "Estructura Predeterminada",
  extendedStructure: "Estructura Extendida",
  reactRecommended: "Recomendado por React",
  vue: "Vue",
  angular: "Angular",
};

const DirStructureOptions = ({ onSelect, selectedStructure }) => {
  const { isSpanish } = useLocalization();
  const text = isSpanish ? spanishText : englishText;

  const folderStructures = [
    {
      name: text.defaultStructure,
      structure: ["introduction", "ui", "sections", "pages", "animations", "helperFunctions"],
    },
    {
      name: text.extendedStructure,
      structure: ["docs", "src", "tests", "config", "scripts", "assets"],
    },
    {
      name: text.reactRecommended,
      structure: ["src", "public", "components", "hooks", "contexts", "utils"],
    },
    {
      name: text.vue,
      structure: ["src", "components", "views", "router", "store", "assets"],
    },
    {
      name: text.angular,
      structure: ["src", "app", "assets", "environments", "styles", "modules"],
    },
  ];

  return (
    <div className="flex overflow-x-scroll gap-4">
      {folderStructures.map((structure, index) => (
        <div
          key={index}
          onClick={() => onSelect(structure)}
          className={`border p-4 rounded-lg cursor-pointer shrink-0 min-w-[250px] ${
            selectedStructure && selectedStructure.name === structure.name ? "border-primary" : "border-transparent"
          }`}
        >
          <h3 className="font-bold mb-2">{structure.name}</h3>
          <ul className="ml-5">
            {structure.structure.map((folder) => (
              <li key={folder} className="flex gap-2">
                <FolderIcon className="w-4 h-4 mr-1" />
                {folder}
              </li>
            ))}
          </ul>
        </div>
      ))}
    </div>
  );
};

export default DirStructureOptions;
  ```
  