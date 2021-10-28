# Reset Css

````css
*, ::before, ::after {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}
````

---

````css
::before,
::after {
  -webkit-box-sizing: border-box;
          box-sizing: border-box;
  margin: 0;
  padding: 0;
}
````

---

````css
*,
*::before,
*::after {
    margin: 0;
    padding: 0;
    box-sizing: inherit;
}

html {
    box-sizing: border-box;
    font-size: 62.5%;
}
````
