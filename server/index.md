---
  title: 'index.mjs'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # index.mjs
  # Explicación del código

El código proporcionado es un ejemplo de un servidor web utilizando Express en Node.js. A continuación se detalla la funcionalidad y las dependencias utilizadas en el código:

### Dependencias utilizadas:
- **Express**: Framework de Node.js para construir aplicaciones web.
- **Passport**: Middleware de autenticación para Node.js.
- **Passport-Github**: Estrategia de autenticación para Passport utilizando GitHub.
- **Express-session**: Middleware de gestión de sesiones para Express.
- **Dotenv**: Para cargar variables de entorno desde un archivo `.env`.
- **Ws**: Biblioteca para implementar WebSockets en Node.js.

### Explicación del código:

1. Se importan las dependencias necesarias, como Express, Passport, etc.
2. Se configura Express y se define el puerto en el que se ejecutará el servidor.
3. Se obtiene la ruta del directorio actual del módulo.
4. Se configuran los middleware de Express para manejar JSON y servir archivos estáticos desde el directorio `dist`.
5. Se configura Passport con la estrategia de autenticación de GitHub, definiendo las credenciales de la aplicación y la URL de callback.
6. Se configura el manejo de sesiones en Express.
7. Se inicializa Passport y se establecen las funciones de serialización y deserialización de usuarios.
8. Se definen rutas para la autenticación con GitHub, el cierre de sesión, y la obtención del perfil de usuario.
9. Se utilizan rutas definidas en archivos separados para diferentes funcionalidades.
10. Se inicia el servidor Express en el puerto especificado.
11. Se crea un servidor WebSocket sobre el servidor Express para habilitar la comunicación en tiempo real.
12. Se maneja la conexión de un cliente WebSocket, asignando un ID de usuario y mostrando un mensaje en la consola.

### Ejemplo de uso:

Para utilizar este código, se debe tener en cuenta lo siguiente:
- Configurar las variables de entorno en un archivo `.env` con las credenciales de GitHub y la clave secreta de sesión.
- Implementar las rutas y funcionalidades en los archivos `githubRoutes.mjs`, `clientRoutes.mjs` y `aiRoutes.mjs`.
- Ejecutar el servidor Node.js con el código proporcionado.

```bash
npm install express passport passport-github express-session dotenv ws
node app.js
```

Una vez que el servidor esté en funcionamiento, se puede acceder a las rutas definidas, como la autenticación con GitHub, el cierre de sesión y la obtención del perfil de usuario. Además, se pueden establecer conexiones WebSocket para comunicación en tiempo real.

Este código es un ejemplo básico de un servidor web con autenticación OAuth utilizando Passport y comunicación en tiempo real con WebSockets.
  
  ## Component Code
  ```jsx
  import express from 'express';
import passport from 'passport';
import { Strategy as GitHubStrategy } from 'passport-github';
import session from 'express-session';
import { config } from 'dotenv';
import path from 'path';
import { fileURLToPath } from 'url';
import { WebSocketServer } from 'ws';
import githubRoutes from './routes/github/githubRoutes.mjs';
import clientRoutes from './routes/clientRoutes.mjs';
import aiRoutes from './routes/aiRoutes.mjs';

config();
const app = express();
const port = process.env.PORT || 8080;

// Get the directory name of the current module file
const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

app.use(express.json());
// Serve static files from the dist directory
app.use(express.static(path.join(__dirname, 'dist')));

passport.use(new GitHubStrategy({
    clientID: process.env.GITHUB_CLIENT_ID,
    clientSecret: process.env.GITHUB_CLIENT_SECRET,
    callbackURL: "https://libraryai-efbafc7d6218.herokuapp.com/auth/github/callback"
}, (accessToken, refreshToken, profile, cb) => {
    profile.accessToken = accessToken; // Attach accessToken
    const user = {
        profile: profile,
        accessToken: accessToken
    };
    return cb(null, user); // Pass the whole structured user object
}));

app.use(session({
    secret: process.env.SESSION_SECRET,
    resave: false,
    saveUninitialized: true
}));

app.use(passport.initialize());
app.use(passport.session());

passport.serializeUser((user, done) => done(null, user));
passport.deserializeUser((obj, done) => done(null, obj));

app.get('/auth/github', passport.authenticate('github', { scope: ['public_repo'] }));

app.get('/auth/github/callback', 
    passport.authenticate('github', { failureRedirect: '/login' }),
    (req, res) => {
        res.redirect('/library');
    });

app.get('/signout', (req, res, next) => {
    req.logout((err) => {
        if (err) {
            return next(err);
        }
        res.redirect('/'); // Redirect to home page after logging out
    });
});

app.get('/api/user/profile', (req, res) => {
    if (req.isAuthenticated()) {
        res.json({ profile: req.user.profile });
    } else {
        res.status(401).send('Not authenticated');
    }
});

// Use the client routes
app.use('/', clientRoutes);
app.use('/api', githubRoutes);
app.use('/ai', aiRoutes);

// Setup WebSocket server
const server = app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});

const wss = new WebSocketServer({ server });

wss.on('connection', (ws, req) => {
    const userId = req.headers['sec-websocket-protocol']; // Assuming user ID is passed as a subprotocol
    ws.userId = userId;
    console.log('WebSocket connection established for user:', userId);
});

export { wss };
//////
  ```
  