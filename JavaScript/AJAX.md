AJAX (Asynchronous JavaScript And XML)
-
> Asynchrone signifie que l'application Web peut envoyer et recevoir des données du serveur Web sans actualiser la page.

> AJAX utilise un objet ``XMLHttpRequest`` intégré au navigateur pour demander des données à un serveur Web.
> Tous les navigateurs modernes ont un XMLHttpRequestobjet intégré.
  
> L'objet ``XMLHttpRequest`` : Est une API sous la forme d'un objet dont les méthodes aident au transfert de données entre un navigateur Web et un serveur Web.

>  Les applications AJAX peuvent utiliser XML pour transporter des données, mais il est tout aussi courant de transporter des données en texte brut ou en texte JSON.   

Créez un objet XMLHttpRequest:
````javascript
    let xhttp = new XMLHttpRequest();
````
Propriétés de l'objet XMLHttpRequest:   
(attention il y en a d'autres : responseText, responseXML, statusText)   

``readystate`` est une propriété de l'objet XMLHttpRequest qui contient le statut de XMLHttpRequest.

- 0: demande non initialisée
- 1: connexion au serveur établie
- 2: demande reçue
- 3: traitement de la demande
- 4: demande terminée et réponse prête   

``onreadystatechange`` est une propriété de l'objet XMLHttpRequest qui définit une fonction à appeler lorsque la propriété readyState change.   
   
``status`` est une propriété de l'objet XMLHttpRequest qui retourne le numéro d'état d'une requête
- 200: "OK"
- 403: "Interdit"
- 404: "Introuvable"   

Méthodes d'objet ``XMLHttpRequest``:   

Pour envoyer une demande à un serveur Web, nous utilisons les méthodes ``open()`` et ``send()`` de l'objet ``XMLHttpRequest``.
````javascript
    function changeContent() {
        let xhttp = new XMLHttpRequest();
        // onreadystatechange est un gestionnaire d'évènement (EventHandler) invoqué à chaque fois que l'attribut readyState change.
        xhttp.onreadystatechange = function() {
            if (this.readyState === 4 && this.status === 200) {
             document.getElementById("foo").innerHTML = this.responseText;
            }
        };
        // XMLHttpRequest.open(method, url, async, user, password)
        // le 3eme parametre par default est a true, si false la requete devient synchrone
        xhttp.open("GET", "content.txt", true);
        xhttp.send();
    }
````
> Ici on envoie une requête mais nous ne traitons pas la réponse reçue : 
````javascript
    const request = new XMLHttpRequest();
    request.open("GET","https://www.prevision-meteo.ch/services/json/vaucresson");
    request.send();
````

````javascript
    const request = new XMLHttpRequest();
    
    request.onreadystatechange = function () {
      //if (httpRequest.readyState === XMLHttpRequest.DONE) {
        if (this.readyState === 4 && this.status === 200) {
            let reponse = JSON.parse(this.responseText);
            console.log(reponse.current_condition.condition);
            document.body.innerHTML = reponse.current_condition.condition;
        }
    };
    request.open("GET","https://www.prevision-meteo.ch/services/json/vaucresson");
    request.send();
````

Notez que si vous voulez envoyer des données avec la méthode POST, vous aurez peut-être à changer le type MIME de la requête. Par exemple, utilisez ce qui suit avant d’appeler send() pour envoyer des données de formulaire en tant que chaîne de requête :
httpRequest.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');