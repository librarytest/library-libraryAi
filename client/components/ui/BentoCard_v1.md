---
  title: 'BentoCard'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # BentoCard.jsx
  # Explicación del código

El código proporcionado es un componente de React llamado `BentoCard` que renderiza una tarjeta con un título, una descripción y un enlace a una ruta específica utilizando React Router.

### Dependencias
- `@heroicons/react`: Esta biblioteca proporciona iconos listos para usar en aplicaciones web.
- `react-router-dom`: Proporciona enlaces de navegación para aplicaciones React.

### Componente BentoCard
- **Parámetros**:
  - `title`: Título de la tarjeta.
  - `description`: Descripción de la tarjeta.
  - `columnReverse`: Booleano que indica si se debe mostrar la tarjeta en columna inversa.
  - `flexSize`: Tamaño del contenedor flexible.

- **Funcionamiento**:
  - Se determina la clase de flexbox a aplicar según el valor de `columnReverse`.
  - Se renderiza un enlace (`Link`) que redirige a la ruta `/library/${title}`.
  - La tarjeta tiene estilos predefinidos de diseño, tamaño, bordes redondeados y sombra.
  - Se muestra el título y la descripción dentro de la tarjeta.
  - Se muestra un texto y un icono de flecha hacia la derecha al pasar el cursor sobre la tarjeta.

### Ejemplo de uso:
```jsx
<BentoCard
  title="Título de la tarjeta"
  description="Descripción de la tarjeta"
  columnReverse={false}
  flexSize="flex-1"
/>
```

En este ejemplo, se crea una instancia de `BentoCard` con un título, una descripción, sin inversión de columna y un tamaño flexible de `flex-1`.
  
  ## Component Code
  ```jsx
  import { ArrowLongRightIcon } from "@heroicons/react/24/outline";
import { Link } from "react-router-dom";

const BentoCard = ({ title, description, columnReverse = false, flexSize }) => {
  const flexClass = columnReverse ? "flex-col-reverse" : "flex-col";

  return (
    <Link
      to={`/library/${title}`}
      className={`flex ${flexClass} group justify-between bg-base-100 items-start text-sm p-4 border-2 border-black shadow-2xl rounded-2xl w-[250px] h-[290px] shrink-0 ${flexSize}`}
    >
      <div>
        <h3 className="text-2xl font-medium">{title}</h3>
        <p className="text-base-content/70 line-clamp-5">{description}</p>
      </div>

      <div className="flex gap-4 text-primary group-hover:gap-8 transition-all duration-500 ease-in-out">
        <span className="text-sm">Open Library</span>
        <ArrowLongRightIcon className="w-6" />
      </div>
    </Link>
  );
};

export default BentoCard;
  ```
  