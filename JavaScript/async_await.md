# async & await

> Le but des fonctions async/await est de simplifier l'utilisation synchrone des promesses et d'opérer sur des groupes de promesses. De la même façon que les promesses sont semblables à des callbacks structurés, async/await est semblable à la combinaison des générateurs et des promesses.

- ``async`` s'utilise avec une fonction :

````javascript
    let func = async () => {
        
    };
        
    // ou 
    
    async function func1 () {
    
    }
````

Dès que l'on met async devant une fonction cela transforme cette fonction en promesse !

````javascript
    let func = async () => {
        console.log(('ok'));
        return 'test';
    };
    
    func().then(test => console.log(test));
````

- ``await`` s'utilise a l'intérieur d'une fonction async :

````javascript
    let func2 = () => {
      return new Promise((resolve) => {
          setTimeout(()=> {
              resolve('test');
          },500);
      })
    };
    
    let func = async () => {
        console.log(('ok'));
        let text = await func2(); // await attend le résultat d'une promesse !
        return text;
    };
    
    func().then(test => console.log(test));
````

---

## Exemple avec axios

````javascript
const axios = require('axios');
 
(async () => {
  try {
        const response = await axios.get('https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY')
        console.log(response.data.url);
        console.log(response.data.explanation);
  } catch (error) {
        console.log(error.response.body);
  }
})();
````

`axios.all`

````javascript
const axios = require('axios');
 
(async () => {
  try {
        const [response1, response2] = await axios.all([
          axios.get('https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY&date=2020-03-18'),
          axios.get('https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY&date=2020-03-17')
        ]);
        console.log(response1.data.url);
        console.log(response1.data.explanation);
 
        console.log(response2.data.url);
        console.log(response2.data.explanation);
  } catch (error) {
        console.log(error.response.body);
  }
})();
````

---

un autre exemple

````javascript
getUsers = async () => {
    let res = await axios.get("https://reqres.in/api/users?page=1");
    let { data } = res.data;
    this.setState({ users: data });
};

//axios.post('https://votredomaine.com', { nom : 'Aditya' })
````

---

````javascript
class App extends React.Component{
    async getData() {
        const res = await axios('/data');
        return await res.json(); // (Or whatever)
    }
    constructor(...args) {
        super(...args);
        this.state = {data: null};
    }
    componentDidMount() {
        if (!this.state.data) {
            this.getData().then(data => this.setState({data}))
                          .catch(err => { /*...handle the error...*/});
        }
    }
    render() {
        return (
            <div>
                {this.state.data ? <em>Loading...</em> : this.state.data}
            </div>
        );
    }
}
````

---

````javascript
class App extends React.Component{
  constructor(){
   super();
   this.state = {
    serverResponse: ''
   }
  }
  componentDidMount(){
     this.getData();
  }
  async getData(){
   const res = await axios.get('url-to-get-the-data');
   const { data } = await res;
   this.setState({serverResponse: data})
 }
 render(){
  return(
     <div>
       {this.state.serverResponse}
     </div>
  );
 }
}
````

---

````javascript
const MyFunctionnalComponent: React.FC = props => {
  useEffect(() => {
    // Create an scoped async function in the hook
    async function anyNameFunction() {
      await loadContent();
    }    // Execute the created function directly
    anyNameFunction();
  }, []);
  
  return <div></div>;
};
````

---

````javascript
function Example() {
  const [data, dataSet] = useState<any>(null)

  useEffect(() => {
    async function fetchMyAPI() {
      let response = await fetch('api/data')
      response = await response.json()
      dataSet(response)
    }

    fetchMyAPI()
  }, [])

  return <div>{JSON.stringify(data)}</div>
````
