# Three JS

Il faut 3 choses pour faire un rendu 3D :

- Une scène
- Une caméra
- Un rendu

## La scène

La scène est un objet qui contient tous les objets que l'on veut afficher. On peut y ajouter des objets, des lumières, des caméras, etc.

```javascript
var scene = new THREE.Scene();
```

## La caméra

La caméra est un objet qui permet de définir le point de vue de l'utilisateur. Il existe plusieurs types de caméra, mais la plus simple est la caméra perspective.

```javascript
var camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
```

Les paramètres de la caméra sont:

- Le champ de vision (en degrés)
- Le ratio de l'écran
- La distance minimale de rendu
- La distance maximale de rendu

## Le rendu

Le rendu est un objet qui permet de définir comment on va afficher la scène. Il existe plusieurs types de rendu, mais le plus simple est le rendu WebGL.

```javascript
var renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);
```

---

## Les objets

Les objets sont des éléments que l'on peut ajouter à la scène. Il existe plusieurs types d'objets, mais le plus simple est le cube.

```javascript
var geometry = new THREE.BoxGeometry(1, 1, 1);
var material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
var cube = new THREE.Mesh(geometry, material);
scene.add(cube);
```

Les paramètres du cube sont:

- La largeur
- La hauteur

## Un Mesh

Dans Three.js, un `Mesh` est un objet qui combine une `Geometry` et un `Material` pour créer une forme visible dans notre scène 3D.

La `Geometry` représente la forme de l'objet (par exemple, un cube, une sphère, etc.) et le `Material` définit l'apparence de la surface de l'objet (par exemple, sa couleur, comment il réfléchit la lumière, etc.).

Voici un exemple de création d'un `Mesh` dans Three.js :

```javascript
// Créer une géométrie de cube
const geometry = new THREE.BoxGeometry(1, 1, 1);

// Créer un matériau de couleur rouge
const material = new THREE.MeshBasicMaterial({ color: 0xff0000 });

// Créer un Mesh à partir de la géométrie et du matériau
const cube = new THREE.Mesh(geometry, material);

// Ajouter le cube à la scène
scene.add(cube);
```

Dans cet exemple, nous créons un cube rouge et l'ajoutons à la scène.

## La boucle de rendu

La boucle de rendu est une fonction qui permet de mettre à jour la scène à chaque frame.

```javascript
function animate() {
    requestAnimationFrame(animate);
    renderer.render(scene, camera);
}
animate();
```

## Les lumières

Les lumières sont des objets qui permettent d'éclairer la scène. Il existe plusieurs types de lumières, mais la plus simple est la lumière ambiante.

```javascript
var light = new THREE.AmbientLight(0x404040);
scene.add(light);
```

## Les textures

Les textures sont des images que l'on peut appliquer sur les objets. Il existe plusieurs types de textures, mais la plus simple est la texture de base.

```javascript
var texture = new THREE.TextureLoader().load('texture.jpg');
var material = new THREE.MeshBasicMaterial({ map: texture });
```

## Les contrôles

Les contrôles sont des objets qui permettent de déplacer la caméra. Il existe plusieurs types de contrôles, mais les plus simples sont les contrôles orbitaux.

```javascript
var controls = new THREE.OrbitControls(camera, renderer.domElement);
```

## Les animations

Les animations sont des objets qui permettent de déplacer les objets. Il existe plusieurs types d'animations, mais la plus simple est l'animation linéaire.

```javascript
var animation = new THREE.Animation(object, mixer);
var mixer = new THREE.AnimationMixer(object);
var clip = new THREE.AnimationClip('name', duration, tracks);
var track = new THREE.NumberKeyframeTrack('property', times, values);
```

## Les shaders

Les shaders sont des objets qui permettent de définir le rendu des objets. Il existe plusieurs types de shaders, mais le plus simple est le shader de base.

```javascript
var material = new THREE.ShaderMaterial({
    uniforms: {
        time: { value: 0.0 },
        resolution: { value: new THREE.Vector2() }
    },
    vertexShader: document.getElementById('vertexShader').textContent,
    fragmentShader: document.getElementById('fragmentShader').textContent
});
```

## Les géométries

Les géométries sont des objets qui permettent de définir la forme des objets. Il existe plusieurs types de géométries, mais le plus simple est la géométrie de base.

```javascript
var geometry = new THREE.Geometry();
geometry.vertices.push(new THREE.Vector3(0, 0, 0));
geometry.faces.push(new THREE.Face3(0, 1, 2));
```

## Les matériaux

Les matériaux sont des objets qui permettent de définir l'apparence des objets. Il existe plusieurs types de matériaux, mais le plus simple est le matériau de base.

```javascript
var material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
```

## Les ombres

Les ombres sont des objets qui permettent de définir l'apparence des objets. Il existe plusieurs types d'ombres, mais le plus simple est l'ombre de base.

```javascript
renderer.shadowMap.enabled = true;
renderer.shadowMap.type = THREE.PCFSoftShadowMap;
light.castShadow = true;
object.receiveShadow = true;
object.castShadow = true;
```

## Les particules

Les particules sont des objets qui permettent de définir l'apparence des objets. Il existe plusieurs types de particules, mais le plus simple est la particule de base.

```javascript
var geometry = new THREE.Geometry();
var material = new THREE.PointsMaterial({ size: 0.1 });
var particles = new THREE.Points(geometry, material);
```

## Les sprites

Les sprites sont des objets qui permettent de définir l'apparence des objets. Il existe plusieurs types de sprites, mais le plus simple est le sprite de base.

```javascript
var texture = new THREE.TextureLoader().load('texture.jpg');
var material = new THREE.SpriteMaterial({ map: texture });
var sprite = new THREE.Sprite(material);
```

## Les sons

Les sons sont des objets qui permettent de définir l'apparence des objets. Il existe plusieurs types de sons, mais le plus simple est le son de base.

```javascript
var listener = new THREE.AudioListener();
var sound = new THREE.Audio(listener);
var audioLoader = new THREE.AudioLoader();
audioLoader.load('sound.mp3', function(buffer) {
    sound.setBuffer(buffer);
    sound.setLoop(true);
    sound.setVolume(0.5);
    sound.play();
});
```

## Les vidéos

Les vidéos sont des objets qui permettent de définir l'apparence des objets. Il existe plusieurs types de vidéos, mais le plus simple est la vidéo de base.

```javascript
var video = document.createElement('video');
video.src = 'video.mp4';
video.play();
var texture = new THREE.VideoTexture(video);
var material = new THREE.MeshBasicMaterial({ map: texture });
```

## Les effets

Les effets sont des objets qui permettent de définir l'apparence des objets. Il existe plusieurs types d'effets, mais le plus simple est l'effet de base.

```javascript
var composer = new THREE.EffectComposer(renderer);
var renderPass = new THREE.RenderPass(scene, camera);
composer.addPass(renderPass);
var effectPass = new THREE.ShaderPass(THREE.Shader);
effectPass.renderToScreen = true;
composer.addPass(effectPass);
```

## Les post-traitements

Les post-traitements sont des objets qui permettent de définir l'apparence des objets. Il existe plusieurs types de post-traitements, mais le plus simple est le post-traitement de base.

```javascript
var composer = new THREE.EffectComposer(renderer);
var renderPass = new THREE.RenderPass(scene, camera);
composer.addPass(renderPass);
var effectPass = new THREE.ShaderPass(THREE.Shader);
effectPass.renderToScreen = true;
composer.addPass(effectPass);
```

## Les modèles

Les modèles sont des objets qui permettent de définir l'apparence des objets. Il existe plusieurs types de modèles, mais le plus simple est le modèle de base.

```javascript
var loader = new THREE.GLTFLoader();
loader.load('model.glb', function(gltf) {
    scene.add(gltf.scene);
});
```

## Les collisions

Les collisions sont des objets qui permettent de définir l'apparence des objets. Il existe plusieurs types de collisions, mais le plus simple est la collision de base.

```javascript
var raycaster = new THREE.Raycaster();
var intersects = raycaster.intersectObject(object);
```
