---
  title: 'authMiddleware.mjs'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # authMiddleware.mjs
  # Función ensureAuthenticated

La función `ensureAuthenticated` es una función middleware comúnmente utilizada en aplicaciones web para garantizar que un usuario esté autenticado antes de permitir el acceso a ciertas rutas o recursos protegidos.

## Parámetros
- `req`: Representa el objeto de solicitud (request) que contiene información sobre la solicitud HTTP entrante.
- `res`: Representa el objeto de respuesta (response) que se utilizará para enviar una respuesta al cliente.
- `next`: Es una función que se llama para pasar el control al siguiente middleware en la pila cuando la autenticación es exitosa.

## Funcionamiento
1. La función `ensureAuthenticated` verifica si el usuario está autenticado llamando al método `isAuthenticated()` en el objeto `req`, que generalmente es proporcionado por un sistema de autenticación como Passport.js.
2. Si el usuario está autenticado, la función llama a `next()` para pasar el control al siguiente middleware en la cadena de middleware.
3. Si el usuario no está autenticado, la función redirige al usuario a la ruta raíz ('/') utilizando `res.redirect('/')`.

## Ejemplo de uso
```javascript
// Importar la función ensureAuthenticated
import { ensureAuthenticated } from './middleware';

// Definir una ruta protegida que requiere autenticación
app.get('/profile', ensureAuthenticated, (req, res) => {
    res.render('profile');
});
```

En este ejemplo, la función `ensureAuthenticated` se utiliza como middleware en la ruta '/profile'. Si el usuario está autenticado, se renderiza la vista de perfil; de lo contrario, se redirige al usuario a la página de inicio.
  
  ## Component Code
  ```jsx
  export const ensureAuthenticated = (req, res, next) => {
    if (req.isAuthenticated()) {
        return next();
    } else {
        res.redirect('/');
    }
};
  ```
  