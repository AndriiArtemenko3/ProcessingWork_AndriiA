### Image Magnifier - Overview 

My sketch Image Magnifier does the following tasks:
- displays uploaded image ( from the folder )
- magnifies a selected portion of the image area based on the mouse cursor position, so the magnification is interactive with the user
- has a circular design of the zoom icon - just like other popular image editors
- translates rasterized image ( picture ) into vectors to display zoomed area
- uses transformation matrix to display zoom ( push and pop matrix ) 
- adjustable zoom level with text ui eleemnt which displays the zoom level

### Process - Design and defining varibales 
First I had to define the global variables that i might need - the most essential one i think is the zoom scope because this is the essential function of this sketch. The 
second varibale I will need is the size of displayed area. I have to display an image as well so I also have to create a variable for img. 

```java
PImage pic;
float zoomFactor = 2.0; //zoom level
float rad = 35; //circle display radius

void setup() {
  size(800, 600); 
  pic = loadImage("wizard.jpg"); // uploading pre-downloaded image to my setup
  noCursor(); // Hiding the cursor
}
```
### Process - Zoom area calculation 

Before I can do visualisatiuon I need to prepare my zoom area and assign my mouse to the zoom itself. I first calculate my diameter from my radius and then divide it by zoom factor to get 
a value of the zoomed in area for width and height, which I will use later in the code for visualisations. As I want the zoomed area to be at the position of my cursor, I have
to assign my mouse to the right position on my image. These lines are crucial in terms of correct interpreting the zoomed area based on the cursor position because before 
I translated my mouse position from the screen to an image, the zoom was working very weird and not very accurately. float mouseXpos = (mouseX / (float) width) * pic.width;
float mouseYpos = (mouseY / (float) height) * pic.height; 


```java
void draw() {
  background(255);
  image(pic, 0, 0, width, height); // displaying image on the screen
  

// Calculating the diameter of the zoom circle
float diameter = rad * 2;

// Calculating the width and height of the zoomed area 
float zoomedWidth = diameter / zoomFactor;
float zoomedHeight = diameter / zoomFactor;

// Calculating mouse position from the canvas to an image itself
float mouseXpos = (mouseX / (float) width) * pic.width;
float mouseYpos = (mouseY / (float) height) * pic.height;

// Centering zoom area
float x = mouseXpos - zoomedWidth / 2;
float y = mouseYpos - zoomedHeight / 2;

// Constraining the area
x = constrain(x, 0, pic.width - zoomedWidth);
y = constrain(y, 0, pic.height - zoomedHeight);

// Getting the zoomed area from the image
PImage zoomedArea = pic.get(int(x), int(y), int(zoomedWidth), int(zoomedHeight));
```

the transformation matrix push/pop transforms the origins of the coordinate from the original 0,0 which is top left corner to the position of the mouse and all further 
calculations will be done according to the new origins after transformation 

```java
 pushMatrix();
  translate(mouseX - rad, mouseY - rad); 
  Vectorisation(zoomedArea, diameter, diameter, rad); 
  popMatrix();
  ```
### UI visuals part

Now I create an ellipse where the zoom area will be shown, with a red border stroke for better visual contrast. I also add the text to display the correct zoom level

```java
noFill();
  stroke(255, 0, 0);
  ellipse(mouseX, mouseY, diameter, diameter);
  
 
  fill(0); 
  textSize(12); 
  textAlign(LEFT, CENTER);
  text(int(zoomFactor * 100) + "%", mouseX + rad + 5, mouseY); 
```

### Converting raster image into pixels - Vectorisation

I need to make a function to convert raster image into pixels or let say break it down into vectorised mosaic pieces based on the colors each individual pixel has 

```java
void Vectorisation(PImage img, float w, float h, float radius) {
```
float w - the width of the zoomed area 
float h - the height of the zoomed area

```java
 img.loadPixels();
```
img.loadPixels() -> method in Processing that loads pixels into an array 

```java
float pixelWidth = w / img.width;
float pixelHeight = h / img.height;
```
Here the code calculates the dimensions for each individual pixel displayed. so as it is dynamic the pixel's dimensions depend on the zoom factor 

```java
for (int i = 0; i < img.pixels.length; i++) {
    // Calculate the x and y position of the pixel in the image
    int x = i % img.width;
    int y = i / img.width;
    
    // Get the color of the current pixel
    int col = img.pixels[i];
```

To find the color of each indivisual pixel we need a loop that will run through each individual pixel in the zoomed area 

float dx = x * pixelWidth - w / 2; // dx - delta x
    float dy = y * pixelHeight - h / 2; // dy - delta y

```java
// constraining pixels and fitting to the ellipse frame
if (dist(dx, dy, 0, 0) < radius) {
// Set the fill color to the pixel's color
fill(col);
      
noStroke();
      
rect(x * pixelWidth, y * pixelHeight, pixelWidth, pixelHeight);
}
```
Have to check if pixels fir inside ellipse and if so color each individual pixel based on the color of the rasterised image where pixel is located. Then draw them out as 
rectangles and have no stroke to display them better. 

```java
void keyPressed() {
  if (key == '+') {
    zoomFactor *= 1.1; 
  } else if (key == '-') {
    zoomFactor /= 1.1; 
  }
}
```
Adding key commands - to regulate the zoom level with keyboard 

