# Design Pattern

## Singleton

>Singleton  est un modèle de design pattern spécial où une seule instance d'une classe peut exister (ou bien à quelques objets seulement). Si aucune instance de la classe singleton n'existe alors une nouvelle instance est créée et retournée mais si une instance existe déjà alors la référence à l'instance existante est retournée. Dans beaucoup de langages de type objet, il faudra veiller à ce que le constructeur de la classe soit privé, afin de s'assurer que la classe ne puisse être instanciée autrement que par la méthode de création contrôlée.

````javascript
class Database {
  constructor(data) {
    if (Database.exists) {
      return Database.instance;
    }
    this._data = data;
    Database.instance = this;
    Database.exists = true;
    return this;
  }

  getData() {
    return this._data;
  }

  setData(data) {
    this._data = data;
  }
}

// usage
const mongo = new Database('mongo');
console.log(mongo.getData()); // mongo

const mysql = new Database('mysql');
console.log(mysql.getData()); // mongo
````
