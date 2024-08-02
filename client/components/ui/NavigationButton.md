---
  title: 'NavigationButton'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # NavigationButton.jsx
  # Explicación de código en Markdown

El código proporcionado es un componente de React que muestra botones de navegación y un botón de cierre de sesión en una interfaz de usuario. A continuación se detalla su funcionamiento:

1. Importaciones:
   - Se importa el componente `Link` de la librería "react-router-dom" para manejar la navegación en la aplicación.
   - Se importan los iconos `AdjustmentsHorizontalIcon` y `Cog6ToothIcon` de la librería "@heroicons/react/24/outline" para utilizarlos en los botones de navegación.
   - Se importa el hook `useLocalization` del contexto "LocalizationContext" para manejar la localización de los textos.

2. Definición de textos:
   - Se definen dos objetos `textEn` y `textEs` que contienen las traducciones de los textos en inglés y español respectivamente.

3. Función `NavigationButton`:
   - Es una función que recibe tres parámetros: `to` (ruta de navegación), `icon` (icono a mostrar) y `title` (título del botón).
   - Retorna un componente `Link` que muestra un botón de navegación con el icono y título proporcionados.

4. Componente `PromptNavigations`:
   - Es un componente funcional que muestra los botones de navegación y el botón de cierre de sesión.
   - Utiliza el hook `useLocalization` para obtener el idioma actual.
   - Define una función `handleSignOut` que realiza una petición de cierre de sesión al servidor.
   - Renderiza los botones de navegación con los textos correspondientes según el idioma actual y el icono asociado.
   - Renderiza un botón de cierre de sesión que llama a la función `handleSignOut` al hacer clic.

Para utilizar este código, se debe integrar en una aplicación de React que tenga configuradas las dependencias de las librerías mencionadas. A continuación se muestra un ejemplo de cómo se puede utilizar este componente en un archivo de la aplicación:

```jsx
import React from 'react';
import PromptNavigations from './PromptNavigations';

function App() {
  return (
    <div>
      <h1>My App</h1>
      <PromptNavigations />
    </div>
  );
}

export default App;
```

En este ejemplo, se importa y se muestra el componente `PromptNavigations` dentro del componente principal `App`. Al renderizar la aplicación, se mostrarán los botones de navegación y el botón de cierre de sesión según el idioma actual.
  
  ## Component Code
  ```jsx
  import { Link } from "react-router-dom";
import {
  AdjustmentsHorizontalIcon,
  Cog6ToothIcon,
} from "@heroicons/react/24/outline";
import { useLocalization } from "../context/LocalizationContext";

const textEn = {
  customPrompts: "Custom Prompts",
  createPrompt: "Create Prompt",
  signOut: "Sign Out",
};

const textEs = {
  customPrompts: "Prompts Personalizados",
  createPrompt: "Crear Prompt",
  signOut: "Cerrar Sesión",
};

function NavigationButton({ to, icon: Icon, title }) {
  return (
    <Link
      to={to}
      className={`rounded-2xl hidden lg:flex items-center gap-2  relative z-30`}
    >
      {Icon && <Icon className="w-6 " />}
      <p className="text-sm">{title}</p>
    </Link>
  );
}

export default function PromptNavigations() {
  const { isSpanish } = useLocalization();
  const text = isSpanish ? textEs : textEn;

  const handleSignOut = async () => {
    try {
      const response = await fetch('/signout', {
        method: 'GET',
        credentials: 'include', // To include cookies
      });

      if (response.ok) {
        window.location.href = '/'; // Fallback in case server does not handle redirect
      } else {
        console.error('Failed to sign out');
      }
    } catch (error) {
      console.error('An error occurred during sign out', error);
    }
  };

  return (
    <div className="flex flex-col gap-4 mt-auto">
      <NavigationButton
        to="/userPrompts"
        icon={AdjustmentsHorizontalIcon}
        title={text.customPrompts}
      />
      <NavigationButton
        to="/customizeprompt"
        icon={Cog6ToothIcon}
        title={text.createPrompt}
      />
      <button className="btn btn-secondary btn-sm" onClick={handleSignOut}>
        {text.signOut}
      </button>
    </div>
  );
}
  ```
  