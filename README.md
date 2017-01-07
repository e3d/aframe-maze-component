
aframe-maze-component
==
A component for building VR mazes
--
* * *
## NPM Installation

    $ npm init
    $ npm install aframe-maze-component --save
  
* * *

## Browser Usage

### Example

Run this example in a browser. Step off the birds-eye view platform and wander around the maze.

    <!DOCTYPE html>
    <html>
      <head>  
        <meta charset="utf-8">
        <title>demo: aframe-maze-component</title>
        <meta name="description" content="demo: aframe-maze-component">
        <script src="https://aframe.io/releases/0.4.0/aframe.min.js"></script>
        <script src="//cdn.rawgit.com/donmccurdy/aframe-extras/v3.2.0/dist/aframe-extras.min.js"></script>
        <script src="https://rawgit.com/ngokevin/aframe-look-at-component/master/dist/aframe-look-at-component.min.js"></script> 
        <!-- 
          <script src="../../dist/aframe-maze-component.min.js"></script> 
        -->
        <script src="../../dist/aframe-maze-component.js"></script>
      </head>
      <body>
        <a-scene>
          <a-assets>
            <img id="texture-wall" src="img/arrow-left.png">
            <a-box id="wall-one" 
              static-body material="src: #texture-wall" 
              depth="1" width="4" height="1"></a-box>
            <a-cylinder id="end-cap"
              static-body color="#444444" 
              depth="1.1" width="1.1" height="2"
              radius="0.5"></a-cylinder>
          </a-assets>
          <a-sky id="sky" color="#0000ff"></a-sky>
          <a-box id="center" 
              position="0 0 0"
              color="red" 
              depth="0.5" width="0.5" height="10"></a-box>
          <a-entity id="player"
              camera
              universal-controls
              kinematic-body
              position="-20 20.8 20">
          </a-entity>
          <!-- birds-eye view box to stand on -->
          <a-box static-body 
              depth="0.25" height="0.5" width="0.5" 
              position="-20 19.8 20"
              color="tomato"></a-box>

          <a-entity id="maze1" 
              maze='size: 5 6; wall: #wall-one; cap: #end-cap; open: S 0 N 1 2 4 E 5;' 
              position='0 0 0'></a-entity>

          <a-entity id="maze2" 
              maze='size: 7 3; wall: #wall-one; cap: #end-cap; open: W 2 N 2 6;' 
              position='30 0 0'></a-entity>

          <a-entity id="maze3A" 
              maze='size: 5 6; wall: #wall-one; cap: #end-cap; open: S 0 N 4;' 
              position='25 0 -30'></a-entity>

          <a-entity id="maze3B" 
              maze='size: 5 6; wall: #wall-one; cap: #end-cap -0.1;' 
              position='25 4 -30'></a-entity>

          <a-grid id="ground" width="200" height="200" static-body color="#444444"></a-grid>
        </a-scene>
      </body>
    </html>
    
### Define Assets

Create an __a-assets__ section and adding the following:

* an __img__ to act as a material for the maze walls
* an entity, like __a-box__ to act as a wall, which you can assign the img to as __material.src__.
* an entity, like __a-cylinder__ to act as an cap at each join and end wall in the maze.

Be sure to give each entity unique __id__ attributes. They will be needed to define the maze.

The example below uses the __static-body__ component from __[aframe-extras](https://github.com/donmccurdy/aframe-extras)__ so the player can not go through the walls.

    <a-assets>
      <img id="texture-wall" src="img/arrow-left.png">
      <a-box id="wall-one" 
        static-body material="src: #texture-wall" 
        depth="1" width="4" height="1"></a-box>
      <a-cylinder id="end-cap"
        static-body color="#444444" 
        depth="1.1" width="1.1" height="2"
        radius="0.5"></a-cylinder>
    </a-assets>

### Create an Entity with a Maze Component

A __maze__ component can consist of the following:

* __size__ - dimension, in cells, of the size of the maze (__size: 5 6;__)
* __wall__ - the __id__ of the entity asset the will be used to create walls of the maze (__wall: #wall-one;__)
  * the __width__ of the entity will be used to determine the cell size of the maze
  * the wall __height__ divided by 2 will be used to determine the position relative to the parent entity
* __cap__ - the __id__ of the entity asset that will be used to create the end caps of the maze (__cap: #end-cap;__)
  * the cap __height__ divided by 2 will be used to determine the position relative to the parent entity
  * __cap height adjust__ - a height adjustment value can be follow the cap id to adjust the cap in relation to the maze walls (cap: #end-cap __-0.1__;')
  * adjust the __width__ and __depth__ of the cap entity to be slightly larger than the maze wall to prevent video jitters as the planes fight to occupy the same space
* __open__ - a list of border walls that can be opened to allow entering and existing (__open: S 0 N 1 2 4 E 5;__)
  * format: *border cell ids*, *border cell ids*, ...
  * border can be __N__, __E__, __W__, __S__ to represent the north, east, west and south borders
  * cell ids are a zero based index along the border representing each cell wall to open
  * zero (__0__) is the upper left / first column (__N 0__ - opens top left wall, __E 0__ opens right-side top wall, __W 0__ opens left-side top wall, __S 0__ opens bottom left wall)
  * multiple border walls can be opened up (__N 0 3 4__)
  * multiple borders can be opened at the same time (__open: S 0 N 1 2 4 E 5;__)

Example:

An maze with no open walls:

    <a-entity id="maze" 
        maze='size: 5 6; wall: #wall-one; cap: #end-cap;' 
        position='0 0 0'></a-entity>

A maze with open walls:

    <a-entity id="maze1" 
        maze='size: 5 6; wall: #wall-one; cap: #end-cap; open: S 0 N 1 2 4 E 5;' 
        position='0 0 0'></a-entity>
        
Example with cap adjustment (cap: #end-cap __-0.1__;'). This is useful in cases where the maze is above the player. It prevents the bottoms of the walls and caps from battling to display in the same space. It's most evident when the player is walking under the maze. This moves the two bottoms away from each other.
          
    <a-entity id="maze3B" 
        maze='size: 5 6; wall: #wall-one; cap: #end-cap -0.1;' 
        position='25 4 -30'></a-entity>

## Testing

To test, go to the root folder and type (sans __$__):

    $ npm test
   
* * *
 
## Repo(s)

* [bitbucket.org/mitchallen/aframe-maze-component.git](https://bitbucket.org/mitchallen/aframe-maze-component.git)
* [github.com/mitchallen/aframe-maze-component.git](https://github.com/mitchallen/aframe-maze-component.git)

* * *

## Contributing

In lieu of a formal style guide, take care to maintain the existing coding style.
Add unit tests for any new or changed functionality. Lint and test your code.

* * *

## Version History

#### Version 0.1.0 

* initial release

* * *
