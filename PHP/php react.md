
---

## 1. Configuration de base

### Installer Node.js et npm

```bash
# Vérifier si node et npm sont installés
node -v
npm -v

# Si non installés, les installer
# https://nodejs.org/
```

### Initialiser un projet React

```bash
npx create-react-app my-app
cd my-app
npm start
```

### Configurer un serveur PHP

```bash
# Créez un serveur PHP avec un fichier index.php
<?php
echo "Hello from PHP!";
?>
# Démarrer le serveur PHP
php -S localhost:8000
```

## 2. Communication entre PHP et React

### Exemple de fetch depuis React

```javascript
// App.js

import React, { useEffect, useState } from 'react';

function App() {
    const [data, setData] = useState(null);

    useEffect(() => {
        fetch('http://localhost:8000/api/data.php')
            .then(response => response.json())
            .then(data => setData(data));
    }, []);

    return (
        <div className="App">
            <header className="App-header">
                {data ? <p>{data.message}</p> : <p>Loading...</p>}
            </header>
        </div>
    );
}

export default App;
```

### Exemple de fichier PHP pour fournir des données

```php
// api/data.php

<?php
$data = array("message" => "Hello from PHP!");
echo json_encode($data);
?>
```

## 3. Envoyer des données de React à PHP

### Exemple de formulaire React

```javascript
// App.js

import React, { useState } from 'react';

function App() {
    const [name, setName] = useState('');

    const handleSubmit = (event) => {
        event.preventDefault();
        fetch('http://localhost:8000/api/submit.php', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({ name: name }),
        })
        .then(response => response.json())
        .then(data => {
            console.log('Success:', data);
        });
    };

    return (
        <div className="App">
            <form onSubmit={handleSubmit}>
                <input
                    type="text"
                    value={name}
                    onChange={(e) => setName(e.target.value)}
                />
                <button type="submit">Submit</button>
            </form>
        </div>
    );
}

export default App;
```

### Exemple de fichier PHP pour traiter les données

```php
// api/submit.php

<?php
// Récupérer le JSON POSTé
$postData = file_get_contents("php://input");
$request = json_decode($postData);

// Processus de traitement
$response = array("message" => "Hello, " . $request->name);
echo json_encode($response);
?>
```

## 4. Utiliser React avec un backend PHP

### Structure du projet

```
my-app/
├── public/
│   └── index.php
├── src/
│   ├── App.js
│   └── index.js
└── api/
    └── data.php
```

### Exemple de configuration Webpack pour proxy

```javascript
// setupProxy.js

const { createProxyMiddleware } = require('http-proxy-middleware');

module.exports = function(app) {
    app.use(
        '/api',
        createProxyMiddleware({
            target: 'http://localhost:8000',
            changeOrigin: true,
        })
    );
};
```

## 5. Gestion des routes avec React Router et PHP

### Installation de React Router

```bash
npm install react-router-dom
```

### Exemple de React Router

```javascript
// App.js

import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import Home from './components/Home';
import About from './components/About';

function App() {
    return (
        <Router>
            <Switch>
                <Route exact path="/" component={Home} />
                <Route path="/about" component={About} />
            </Switch>
        </Router>
    );
}

export default App;
```

### Exemple de fichiers PHP pour les routes

```php
// index.php

<?php
$request = $_SERVER['REQUEST_URI'];

switch ($request) {
    case '/' :
        require __DIR__ . '/index.html';
        break;
    case '' :
        require __DIR__ . '/index.html';
        break;
    case '/about' :
        require __DIR__ . '/index.html';
        break;
    default:
        http_response_code(404);
        require __DIR__ . '/404.html';
        break;
}
?>
```

## 6. Conclusion

Ce cheatsheet couvre les bases de l'utilisation de PHP avec React. Pour des projets plus complexes, il est recommandé de se familiariser davantage avec les concepts avancés et les meilleures pratiques en matière de développement web.