---
  title: 'MainSection'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # MainSection.jsx
  # Explicación del código

El código proporcionado es un componente de React que representa una sección principal de una página web. Este componente consta de dos subcomponentes: `FloatingElements` y `MainSection`.

### Función `FloatingElements`

La función `FloatingElements` es un componente funcional de React que recibe tres propiedades como argumentos: `image`, `position` y `width. Estas propiedades son utilizadas para mostrar una imagen flotante en una posición específica en la sección principal.

- `image`: Es la ruta de la imagen que se va a mostrar.
- `position`: Es la posición en la que se va a mostrar la imagen. Por defecto, la posición es "top-10 right-5".
- `width`: Es el ancho de la imagen. Por defecto, el ancho es "w-44".

### Función `MainSection`

La función `MainSection` es un componente funcional de React que recibe dos propiedades como argumentos: `title` y `description`. Este componente representa la sección principal de la página web e incluye varios elementos, como texto, imágenes flotantes y un botón de llamado a la acción.

En el código proporcionado, se pueden ver varias instancias del componente `FloatingElements` con diferentes configuraciones de posición y ancho. Estas imágenes flotantes se colocan en diferentes esquinas de la sección principal.

El componente `MainSection` también incluye un título, una descripción y un botón de llamado a la acción que redirige a los usuarios a la página de autenticación de GitHub al hacer clic en él.

### Uso del código

Para utilizar este componente en una aplicación de React, se puede importar y renderizar en el componente principal de la siguiente manera:

```jsx
import MainSection from './MainSection';

function App() {
  return (
    <div>
      <MainSection 
        title="Título de la sección principal"
        description="Descripción de la sección principal"
      />
    </div>
  );
}

export default App;
```

Al renderizar el componente `MainSection` con un título y una descripción específicos, se mostrará la sección principal de la página con las imágenes flotantes y el botón de llamado a la acción.
  
  ## Component Code
  ```jsx
  const FloatingElements = ({ image, position = "top-10 right-5", width = "w-44" }) => {
    return (
      <div className={`absolute ${position} ${width}`}>
        <img src={image} alt="icon" className="w-full" />
      </div>
    );
  };
  
  const MainSection = ({ title, description }) => {
    return (
      <section
        className="flex bg-orange-100 relative flex-col items-center justify-center w-[100vw] h-[100vh] sm:text-center gap-6 p-8"
      >
        {/* Floating Elements */}
        <FloatingElements image="/icon1.svg" />
        <FloatingElements image="/icon2.svg" position="right-5 bottom-0" />
        <FloatingElements image="/icon3.svg" position="left-0 bottom-0" />
        <FloatingElements image="/icon4.svg" position="left-0 top-[20%] hidden sm:absolute" width="w-24" />
        <FloatingElements image="/icon5.svg" position="left-0 top-[10%]" width="w-24" />
  
        {/* Main Content */}
        <h1
          className="self-stretch z-30 text-5xl lg:text-8xl mx-auto font-black text-gray-900 max-w-screen-md"
        >
          {title}
        </h1>
  
        <p className="sm:text-2xl sm:leading-8 max-w-2xl">{description}</p>
  
        {/* Call to Action */}
        <div
          className="w-full flex flex-col-reverse justify-center sm:flex-row gap-3 text-sm sm:text-base font-semibold whitespace-nowrap"
        >
          <a href="/auth/github" className="btn btn-secondary px-12">
            Start Now
          </a>
        </div>
      </section>
    );
  };
  
  export default MainSection;
  ```
  