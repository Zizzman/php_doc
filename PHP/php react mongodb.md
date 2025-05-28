### PHP avec MongoDB et React.js

#### 2.1 Installer MongoDB PHP Driver

```bash
# Installer MongoDB PHP Extension
sudo pecl install mongodb
```

Ajoutez ensuite l'extension à votre fichier `php.ini` :

```ini
extension=mongodb.so
```

#### 2.2 Connexion à MongoDB

```php
<?php
require 'vendor/autoload.php'; // include Composer's autoloader

$client = new MongoDB\Client("mongodb://localhost:27017");
$collection = $client->mydatabase->mycollection;

echo "Connected to MongoDB";
?>
```

#### 2.3 Opérations CRUD

- **Créer un document**

```php
<?php
$document = array(
    "name" => "Alice",
    "age" => 25,
    "email" => "alice@example.com"
);

$insertResult = $collection->insertOne($document);
echo "Inserted with Object ID '{$insertResult->getInsertedId()}'";
?>
```

- **Lire des documents**

```php
<?php
$cursor = $collection->find();
foreach ($cursor as $document) {
    echo $document["name"], "\n";
}
?>
```

- **Mettre à jour un document**

```php
<?php
$updateResult = $collection->updateOne(
    ['name' => 'Alice'],
    ['$set' => ['age' => 26]]
);
echo "Matched {$updateResult->getMatchedCount()} document(s)\n";
echo "Modified {$updateResult->getModifiedCount()} document(s)\n";
?>
```

- **Supprimer un document**

```php
<?php
$deleteResult = $collection->deleteOne(['name' => 'Alice']);
echo "Deleted {$deleteResult->getDeletedCount()} document(s)\n";
?>
```

#### 2.4 Utiliser PHP avec React.js

**Structure du projet**

```
my-project/
├── backend/
│   ├── vendor/
│   ├── getData.php
│   └── composer.json
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── App.js
│   │   └── index.js
│   ├─   ─ package.json
└── README.md
```

**Backend en PHP**

- **Créer un fichier `composer.json`**

```json
{
  "require": {
    "mongodb/mongodb": "^1.8"
  }
}
```

- **Installer les dépendances**

```bash
cd backend
composer install
```

- **Créer un fichier `getData.php`**

```php
<?php
require 'vendor/autoload.php';

$client = new MongoDB\Client("mongodb://localhost:27017");
$collection = $client->mydatabase->mycollection;

$data = $collection->find()->toArray();
echo json_encode($data);
?>
```

- **Démarrer le serveur PHP**

```bash
cd backend
php -S localhost:8000
```

**Frontend en React.js**

- **Créer une application React**

```bash
npx create-react-app frontend
```

- **Installer Axios pour les requêtes HTTP**

```bash
cd frontend
npm install axios
```

- **Créer un composant React pour afficher les données**

```jsx
import React, { useEffect, useState } from 'react';
import axios from 'axios';

function App() {
  const [data, setData] = useState([]);

  useEffect(() => {
    axios.get('http://localhost:8000/getData.php')
      .then(response => {
        setData(response.data);
      })
      .catch(error => {
        console.error("There was an error fetching the data!", error);
      });
  }, []);

  return (
    <div>
      <h1>Data from MongoDB</h1>
      <ul>
        {data.map(item => (
          <li key={item._id}>{item.name} - {item.age} - {item.email}</li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```

- **Démarrer le serveur React**

```bash
npm start
```
