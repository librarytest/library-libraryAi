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
- `@heroicons/react`: Esta biblioteca proporciona iconos listos para usar en aplicaciones web. En este caso, se está utilizando el icono `ArrowLongRightIcon` para mostrar una flecha hacia la derecha.
- `react-router-dom`: Esta biblioteca se utiliza para la navegación en una aplicación React. Se utiliza el componente `Link` para crear enlaces a diferentes rutas de la aplicación.

### Componente BentoCard
- **Props**:
  - `title`: Título de la tarjeta.
  - `description`: Descripción de la tarjeta.
  - `columnReverse`: Un booleano que indica si se debe mostrar la tarjeta en una disposición de columnas inversa.
  - `flexSize`: Clases de tamaño personalizado para el contenedor de la tarjeta.

- **Funcionamiento**:
  - Se determina la clase de flexbox a aplicar según el valor de `columnReverse`.
  - Se renderiza un enlace (`Link`) que redirige a la ruta `/library/${title}` al hacer clic en la tarjeta.
  - La tarjeta tiene estilos CSS predefinidos que incluyen flexbox, colores, tamaño, sombra y bordes redondeados.
  - El contenido de la tarjeta incluye el título y la descripción, con estilos de texto específicos.
  - Se muestra un texto y un icono alineados a la derecha de la tarjeta, con efectos de transición al pasar el cursor sobre la tarjeta.

### Ejemplo de uso:
```jsx
<BentoCard
  title="Título de la tarjeta"
  description="Descripción de la tarjeta"
  columnReverse={false}
  flexSize="w-[300px] h-[350px]"
/>
```

En este ejemplo, se crea una instancia de `BentoCard` con un título, una descripción y un tamaño personalizado para el contenedor de la tarjeta. La tarjeta se mostrará en una disposición de columnas estándar (no inversa).
  
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
  