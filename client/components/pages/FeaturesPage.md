---
  title: 'FeaturesPage'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # FeaturesPage.jsx
  # Explicación del código

El código proporcionado es un componente de una página de características (FeaturesPage) que muestra una serie de elementos como características, héroes, servicios y una sección adicional. A continuación se detalla su funcionamiento:

1. Se importan diferentes componentes y animaciones desde rutas específicas en el proyecto.
2. Se importa la función `useLocalization` desde el contexto `LocalizationContext` para obtener datos generales.
3. Se define el componente `FeaturesPage` como una función de flecha (arrow function) que no recibe ningún argumento.
4. Se desestructuran los datos generales obtenidos a través de `useLocalization` en las variables `services`, `heroes`, `additionalSection` y `feature`.
5. Se retorna un fragmento de JSX que contiene los siguientes elementos:
   - Un componente `Feature` con las propiedades pasadas como argumento.
   - Un componente `SlidingRectangles`.
   - Una lista de componentes `Hero` generados a partir de los datos de `heroes`.
   - Un contenedor `<div>` con una clase específica que contiene una lista de componentes `ServiceItem` generados a partir de los datos de `services`. Cada elemento se envuelve en un componente `FadeInTransition` con una clave única y un retraso de animación de 0.2 segundos.
   - Un componente `CenterHeading` con las propiedades pasadas como argumento.

En resumen, este código representa una página de características que muestra una serie de elementos de manera dinámica y animada, utilizando datos generales obtenidos a través del contexto de localización.

## Ejemplo de uso

```jsx
// Importar FeaturesPage
import FeaturesPage from "./FeaturesPage";

// Renderizar FeaturesPage en la aplicación
const App = () => {
  return (
    <div>
      <FeaturesPage />
    </div>
  );
}
```

En este ejemplo, se importa y se renderiza el componente `FeaturesPage` en una aplicación de React para mostrar la página de características en la interfaz de usuario.
  
  ## Component Code
  ```jsx
  import Feature from "../ui/Feature";
import SlidingRectangles from "../animations/MovingRectangles";
import Hero from "../ui/Hero";
import FadeInTransition from "../animations/FadeTransition";
import ServiceItem from "../ui/ServiceItem";
import CenterHeading from "../ui/CenterHeading";
import { useLocalization } from "../context/LocalizationContext";
const FeaturesPage = () => {
  const { generalData } = useLocalization();
  const {  services, heroes, additionalSection, feature } = generalData;
  return (
    <Feature {...feature} >
      <SlidingRectangles />
      {heroes.map((hero) => (
        <Hero key={hero.title} {...hero} />
      ))}
      <div className="flex flex-wrap w-full gap-10 justify-center pt-36">
        {services.map((item, index) => (
          <FadeInTransition key={index} delay={0.2}>
            <ServiceItem
              {...item}
              className={
                index % 2 === 0
                  ? "rotate-[-5deg] bg-white"
                  : "rotate-[5deg] bg-base-200"
              }
            />
          </FadeInTransition>
        ))}
      </div>

      <CenterHeading {...additionalSection}/>
    </Feature>
  );
};

export default FeaturesPage;
  ```
  