# TrailRendererJS-fixed
Basic trail renderer for Three.js. This library allows for the straight-forward attachment of motion trails to any 3D object. The programmer simply has to specify the shape of the trail and its target (the target must be a Three.js Object3D instance). 

The shape of the trail is specified by supplying a list of 3D points. These points make up the local-space head of the trail. During the update phase for the trail, a new instance of the head is created by transforming the local points into world-space using the target Object3D instance's local-to-world transformation matrix. These points are then connected to the existing head of the trail to extend the trail's geometry. Lastly, the trail must be activated with `.activate()` followed by an update each frame using `.advance()` followed by `.updateHead()`.

The trail renderer currently supports both textured and non-textured trails. Non-textured trails work well with many trail shapes in both translucent and opaque modes. Textured trails work well with many shapes as long as the trail is opaque; if the trail is not opaque, flat shapes work best.

You may also edit the trail's `head` and `tail` colors using `.uniforms.<part>.color.value`.

Demo of the effect can be seen [here](http://projects.markkellogg.org/threejs/demoTrailRenderer.php)

The following code shows how to attach a trail renderer in a scene named 'scene' to an existing Object3D instance named 'trailTarget'.

```js
// specify points to create planar trail-head geometry
var trailHeadGeometry = [
  new THREE.Vector3(-10.0, 0.0, 0.0), 
  new THREE.Vector3(0.0, 0.0, 0.0), 
  new THREE.Vector3(10.0, 0.0, 0.0)
];

// create the trail renderer object
var trail = new THREE.TrailRenderer(scene, false);

// create material for the trail renderer
var trailMaterial = THREE.TrailRenderer.createBaseMaterial();	

// specify length of trail
var trailLength = 150;

// initialize the trail
trail.initialize(trailMaterial, trailLength, false, 0, trailHeadGeometry, trailTarget);

// set colors for the trail (rgb 0-1)
trailMaterial.uniforms.headColor.value.set(1.0, 1.0, 1.0, 1.0)
trailMaterial.uniforms.tailColor.value.set(1.0, 0.0, 0.0, 0.0)

// make sure to activate the trail
trail.activate();

var loop = setInterval(() => {
  //...
  // update the trail each time the scene is re-rendered
  trail.advance()
  trail.updateHead()
  //...
}, 16);
```
