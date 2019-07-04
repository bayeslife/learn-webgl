# Learning Web GL

I want to learn WebGL to render some 3d scenes.

I want to render 3d scenes and capture then into a 2d image and feedback these into a object detection network in order to get a feel for the accuracy of the network.  Basically I want to use webgl to generated large number of data sets.


## What do I want to do

I want to render a scene with a moving ball on a playing field.
Below is the initial scene.

There is a tennis ball at the center of the image and the tennis court revolves around the tennis ball.

~[Resolving Tennis Ball Scene]()

The code which realizes this is [here](https://github.com/bayeslife/react-webgl-scene-render/tree/InitialCommit).


A *div* element is selected.
```
const container = document.getElementById('thescene');
```
A webgl *Scene* is created. Objects willl be rendered into the scene.
```
let scene = new Scene();
```
A webgl *PerspectiveCamera* is created to view the scene and position at a height at the eye level of a person.
```
let camera = new PerspectiveCamera( distance, window.innerWidth / window.innerHeight, 0.1, 1000 );
camera.position.set( 0, HEIGHT_ABOVE_COURT, 0 ); //This set up the Y dimension to be above the X,Z plane
```

A renderer is set up which will add a canvas element into the containing the schene.
```
var renderer = new WebGLRenderer();
renderer.setSize( imageWidth, imageHeight );
container.appendChild( renderer.domElement );
```

The court is then rendered as a series of lines where the lines are specified by an array of Vertices. The *Line* are added to the scene.
```
let geometry = new Geometry();
geometry.vertices.push(new Vector3( courtLengthScale(50) , 0, -singlesWidthScale(100)))
geometry.vertices.push(new Vector3( courtLengthScale(50) , 0, singlesWidthScale(100)))
let serviceLine1 = new Line( geometry, material );
scene.add(serviceLine1) 
```

The animate function is defined to continously render the scene. This function when called requests a subsequent callback and then renders the updated scene.
```
function animate() {
    requestAnimationFrame( animate );
    render()
}
```

The render function moves the camera around the ball location.
```
function render() {
    var speed = Date.now() * 0.0001;
    camera.position.x = ballposition.x + Math.cos(speed) * viewradius ;
    camera.position.z = ballposition.z + Math.sin(speed) * viewradius;
    camera.lookAt(-courtLengthScale(100) , 0, 0)
    renderer.render(scene, camera);
}
```