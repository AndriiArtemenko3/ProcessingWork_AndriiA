### How I chose to simulate motion and forces in the sketch 
The main forces affecting the behaviour of the particles in the sketch is gravity and wind simulation. I had to first make sure that I defined these varibales and 
set them as PVectors to manipulate them in my sketch as vector objects. Then in my setup window I set the float values for gravitation and wind. for the gravity I had 
basically go with trial & error approach and see what values work best to simulate the realistic leaves behaviour. I tried 0.1 at first but this force was too high and 
seemed leaves fall down too fast as if they were heavy physical objects, so I found out that 'ideal' value for gravity vould be around 0.03. I did the same method for 
wind force and found out 0.15 to be working best. I also have to point out that there forces affect particles on different axis, so the gravity affects y axis and wind 
affects x axis. Gravity is a constant force and wind is only applied when 'w' key is pressed. 
```java
PVector gravity; 
PVector wind; 
ArrayList<LeafParticle> leaves; 

void setup() {
   size(1000, 800);
   
   gravity = new PVector(0, 0.03); 
   wind = new PVector(0.15, 0);
   leaves = new ArrayList<LeafParticle>();
}
```
Also, the behaviour of each individual particle is defined by the class Leaf Particle I made, where I assigned the particles with 3 fundamental forces or variables 
for the physical motion and vector processing - PVector Position, PVector Velocity and PVector Acceleration. The position determines the initial coordinates at which the 
object is placed at the sketch. Velocity determines the speed of the object by x and y axis. Acceleration determines with which rate the object is being accelerated. 
These 3 forces are fundamnetal to basically any particle simulation in processing and useful whenerver we need to use PVectors. The velocity parameter in my sketch is 
basically set to random within the range of -1 to 1 to give particles chaotic movement. Acceleratin at my skethc starts at 0, as I feel it should be in the real life as well. 
The acceleration is affected by the gravity force defined in my main sketch. if !onGround is needed to apply the forces only to the particles that have not yet reached the
ground because i want the particles that did reach the ground lie down still, so in the class i make sure i only apply forces to the objects that are "in the air" and have 
not reached the bottom of the sketch yet. 

```java
class LeafParticle {
  PVector position; // defining position 
  PVector velocity; // defining velocity force
  PVector acceleration; // defining acceleration force
  boolean onGround = false; // to track if the particle is on the ground
  
  LeafParticle(float x, float y) {
    position = new PVector(x, y);
    velocity = new PVector(random(-1, 1), random(-1, 1));
    acceleration = new PVector();
  }

 void addForce(PVector f) {
    if (!onGround) {
      acceleration.add(f);
    }
  }

```
