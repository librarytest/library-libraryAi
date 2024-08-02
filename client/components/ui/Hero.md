---
  title: 'Hero'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # Hero.jsx
  # Explicación de código en Markdown

El código proporcionado es un componente funcional de React que representa un "Hero" (sección destacada) en una página web. A continuación se detalla su funcionamiento:

1. **Importación de FadeInSpring:**
   - Se importa un componente llamado `FadeInSpring` desde el archivo "../animations/FadeInSpring". Este componente probablemente se encarga de aplicar una animación de desvanecimiento al contenido que envuelve.

2. **Componente Hero:**
   - Se define un componente funcional llamado `Hero` que recibe como parámetros las propiedades `reverse`, `title`, `description` e `image`.
   - La propiedad `reverse` es opcional y se establece por defecto en `false`.
   - Las propiedades `title`, `description` e `image` son obligatorias y se utilizan para mostrar el título, la descripción y la imagen del "Hero" respectivamente.

3. **Estructura del componente:**
   - El componente `Hero` devuelve un fragmento de JSX que contiene un contenedor principal `<div>` con clases de tailwindcss para estilizar el diseño.
   - Dependiendo del valor de la propiedad `reverse`, se aplican clases diferentes para cambiar el orden de los elementos en dispositivos de tamaño pequeño (`sm`).
   - Dentro del contenedor principal, se encuentran dos elementos:
     - Un contenedor con clase `w-[350px] h-[350px]` que envuelve la imagen. Esta imagen se carga dinámicamente a partir de la URL proporcionada en la propiedad `image`.
     - Un contenedor con clase `grid` que contiene el título y la descripción del "Hero".

4. **Animación FadeInSpring:**
   - El contenido del componente `Hero` está envuelto por el componente `FadeInSpring`, lo que sugiere que el contenido se animará con un efecto de desvanecimiento al ser renderizado en la pantalla.

En resumen, este componente `Hero` se encarga de mostrar una sección destacada en la interfaz de usuario, con un título, una descripción y una imagen, aplicando estilos de diseño responsivo y una animación de entrada.

### Ejemplo de uso:

```jsx
<Hero
  title="Bienvenidos a mi sitio web"
  description="Explora nuestras increíbles ofertas"
  image="url_de_la_imagen.jpg"
/>
```

En este ejemplo, se utiliza el componente `Hero` pasando como propiedades un título, una descripción y la URL de una imagen para mostrar en la sección destacada.
  
  ## Component Code
  ```jsx
  import FadeInSpring from "../animations/FadeInSpring";
export default function Hero({ reverse = false, title, description, image }) {
  return (
    <FadeInSpring>
    <div
      className={`flex flex-col  w-full items-center justify-center gap-10 max-w-5xl mx-auto  ${
        reverse ? "sm:flex-row-reverse" : "sm:flex-row"
      }`}
    >
      <div className="w-[350px] h-[350px]  relative  overflow-hidden border border-white/10 rounded-2xl box-shadow relative">
        <img src={image} alt={image} className="w-full" />
      </div>
      <div className="grid gap-4 sm:w-[30%] text-start">
        <h2 className="text-4xl font-bold tracking-tighter">{title}</h2>
        <p className="text-base-content/70">{description}</p>
      </div>
    </div>
    </FadeInSpring>
  );
}
  ```
  