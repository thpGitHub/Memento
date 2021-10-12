# Icone Hamburger

## Technique avec checkbox

````html
 <input type="checkbox" class="checkbox-toggler" id="nav">
    <label for="nav" class="nav-btn">
      <span class="nav-icon"></span>
    </label>

    <!-- <nav class="navigation">
      <ul class="nav-list">
        <li>><a href="#" class="nav-link">Accueil</a></li>
        <li>><a href="#" class="nav-link">Nos produits</a></li>
        <li>><a href="#" class="nav-link">Blog</a></li>
        <li>><a href="#" class="nav-link">A propos</a></li>
        <li>><a href="#" class="nav-link">Contact</a></li>
      </ul>
    </nav> -->
````

````css
*, ::before, ::after {
    box-sizing: border-box;
    padding: 0;
    margin: 0;
}

body {
    height: 100vh;
    background: #fff;
    font-family: Raleway, sans-serif;
}
.checkbox-toggler {
    display: none;
}
.nav-btn {
    background-color: #fff;
    height: 100px;
    width: 100px;
    border-radius: 50%;
    position: fixed;
    top: 45px;
    right: 65px;
    z-index: 20;
    box-shadow: 0 5px 10px rgba(0,0,0,0.5);
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
}
.nav-icon {
    width: 50px;
    height: 2px;
    background: #333;
    position: relative;
}
.nav-icon::before,
.nav-icon::after {
    content:"";
    width: 50px;
    height: 2px;
    background: #333;
    transition: all 0.2s;
    position: absolute;
}
.nav-icon::before {
    transform: translateY(-12px);
}
.nav-icon::after {
    transform: translateY(12px);
}   

/* barre du milieu */
.checkbox-toggler:checked + .nav-btn .nav-icon {
    background-color: transparent;
}
.checkbox-toggler:checked + .nav-btn .nav-icon::before {
    transform: translateY(0px) rotate(135deg);
}
.checkbox-toggler:checked + .nav-btn .nav-icon::after {
    transform: translateY(0px) rotate(-135deg);
}
````