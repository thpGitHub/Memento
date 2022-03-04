# event

## `event.target`

C'est une référence à l'objet qui a envoyé **l'événement**. Représente les attibuts qui à lancé l'évènement.

Ici `<input />`

```javascript
<input
    id="email"
    name="email"
    type="text"
    value={form.email.value || ''}
    onChange={e => handlerInputChange(e)}
/>    
```

```javascript
const handlerInputChange = e => {
    switch (e.target.name) { // email
      case 'firstName':
        setForm({
          ...form,
          ...{firstName: {value: e.target.value, empty: false}},
        })
        break
      case 'name':
        setForm({
          ...form,
          ...{name: {value: e.target.value, empty: false}},
        })
        break
        .
        .
        .
```

Ici `<form>`

````javascript
<form onSubmit={e => handleSubmit(e)} id="toto">
              <div>
                <label htmlFor="email">* E-mail</label>
                <input
                  id="email"
                  name="email"
                  type="text"
                  value={form.email.value || ''}
                  onChange={e => handlerInputChange(e)}
                />
              </div>
````

````javascript
console.log(e.target.id); // toto
````
