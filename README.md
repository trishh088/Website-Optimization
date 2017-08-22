# Website Performance Optimization portfolio project
For link to the website for review link [here](https://trishh088.github.io/trishh088.github.io-website-optimise/)
For link to the code click [here](https://github.com/trishh088/trishh088.github.io-website-optimise.git)

## Part 1: Optimizing index.html
The specs are as follows
1. PageSpeed Insights for Mobile : **93/100**
2. PageSpeed Insights for desktop = **92/100**

* The photos were minimized using Grunt with the
  * Pizzeria.jpg at quality 20 and width 400
  * profilepic.jpg pizza.png width 100 and quality 20
  * 2048,cam-be-like, mobilewebdev are all of quality 10 and width 50

* Gruntfile.js and package.json is added to the folder Grunt on {github}(https://github.com/trishh088/trishh088.github.io-website-optimise.git)

* style.css and print.css were inlined in index.html and the ,link> tags were commented
* async was added to the google analytics and perfmatters.js script tag
* fonts.googleapis.com/css?family=Open+Sans:400,700 was commented out

## Part 2: Optimizing  pizza.html and main.js
The specs are as follows
1. Time to generate pizzas on load : **16ms**
2. Time to resize pizzas : **1.05999ms**

3.Avegrade scripting time to generate last 10 frames: **4.38ms**

the dx in the code was unnecessary and was slowing down the resize animation so

`
// Returns the size difference to change a pizza element from one size to another. Called by changePizzaSlices(size).
function determineDx (elem, size) {
 var windowWidth = document.querySelector("#randomPizzas").offsetWidth;
//moved old width down to avoid
 var oldWidth = elem.offsetWidth;
 var oldSize = oldWidth / windowWidth;

 // Changes the slider value to a percent width
 function sizeSwitcher (size) {
   switch(size) {
     case "1":
       return 0.25;
     case "2":
       return 0.3333;
     case "3":
       return 0.5;
     default:
       console.log("bug in sizeSwitcher");
   }
 }

 var newSize = sizeSwitcher(size);
 var dx = (newSize - oldSize) * windowWidth;

 return dx;
}

// Iterates through pizza elements on the page and changes their widths
function changePizzaSizes(size) {
 var randomPizza = document.querySelectorAll(".randomPizzaContainer");
 for (var i = 0; i < randomPizza.length; i++) {
   var dx = determineDx(randomPizza[i], size);
   var newwidth = (randomPizza[i].offsetWidth + dx) + 'px';
   randomPizza[i].style.width = newwidth;
 }
}

changePizzaSizes(size);  `



was changed to



` function changePizzaSizes(size) {
  switch(size) {
    case "1":
    newwidth = 25;
    break;

    case "2":
    newwidth = 33.3;
    break;

    case "3":
    newwidth = 50;
    break;

    default:
    console.log("bug in sizeSwitcher");
  }
  var randomPizzas = document.querySelectorAll(".randomPizzaContainer");

  for(i = 0; i < randomPizzas.length; i++){
    randomPizzas[i].style.width = newwidth + "%";
  }
}
changePizzaSizes(size); `


 Below code is changed with the comment at each snippet code that's different then the original repository

` function updatePositions() {

//moved frame++ and window.performance above
  frame++;
  window.performance.mark("mark_start_frame");
  //addedthis
  var topSection = (document.body.scrollTop / 1250);
  var itemLength = items.length;
  //moved variable phase outside the loop so that a new variable is not made everytime the loop runs
  var phase;

  //used translateX is it better at rendering
  for (var i = 0; i < itemLength ; i++) {
     phase = Math.sin( topSection + (i % 5));
     var style = 600 * phase + 'px';
    items[i].style.transform = "translateX(" + style; +"px)";
  } `

 Now the loop was reduced from 200 to 35
` for (var i = 0; i < 35; i++) {....} `

 Moved this at the end of the loop
//moved this
` items = document.querySelectorAll('.mover');
  updatePositions(); `
