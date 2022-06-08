# L'objet window

> L'objet window représente une fenêtre contenant un document DOM ; la propriété document pointe vers le document DOM chargé dans cette fenêtre.

````html
<!DOCTYPE html>
<html>
    <body onload="myFunction()">
    
        <h1>Hello World!</h1>
    
        <script>
            function myFunction() {
                alert("Page is loaded");
                // on peut aussi le tester avec un setTimeout pour voir
                //setTimeout(function(){ alert("Hello"); }, 2000);
            }
        // output : Hello World! puis affichage de l'alert Page is loaded    
        </script>
    
    </body>
</html>
````

## scroll

```javascript
useEffect(()=> {
        const onScroll = (e) => {
            console.log(e.target.documentElement.scrollTop);
        }
        window.addEventListener("scroll", onScroll)

        return () => window.removeEventListener("scroll", onScroll)
    }, [])
```

## screen

```javascript
import {useState, useEffect} from "react";

export default function useDimension() {
  const [dimension, setDimension] = useState();

  useEffect(() => {
    window.onresize = resizeFunc;

    function resizeFunc() {
      /**
       * "window.screen.width" because "window.innerWidth" on mobile device
       *  can return a width more great of destop
       */
      setDimension(window.screen.width);
    }

    resizeFunc();

    /**
     * Clean up function
     */
    return () => {
      window.onresize = resizeFunc;
      /**
       * Another way we can use
       * window.addEventListener("resize", resizeFunc);
       */
    };
  }, []);

  return dimension;
}
```
