

#### Part 2: Optimize Frames per Second in pizza.html

the dx in the code was unnecessary and was slowing down the resize animation so

```// Returns the size difference to change a pizza element from one size to another. Called by changePizzaSlices(size).
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

changePizzaSizes(size); ```




was changed to

``` function changePizzaSizes(size) {
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
changePizzaSizes(size); ```


****next to change   Time to generate pizzas on load: 23.09499999999999ms to 11.669999999999987ms  
``  window.performance.mark("mark_end_resize");
  window.performance.measure("measure_pizza_resize", "mark_start_resize", "mark_end_resize");
  var timeToResize = window.performance.getEntriesByName("measure_pizza_resize");
  console.log("Time to resize pizzas: " + timeToResize[timeToResize.length-1].duration + "ms");
`` on line 491 to
``  var timeToResize = window.performance.getEntriesByName("measure_pizza_resize");
  console.log("Time to resize pizzas: " + timeToResize[timeToResize.length-1].duration + "ms");
  //moved these two lines to stop Forced synchronous Layout
  window.performance.mark("mark_end_resize");
  window.performance.measure("measure_pizza_resize", "mark_start_resize", "mark_end_resize");
``

Also this was changed

``function updatePositions() {

//moved frame++ and window.performance below to stop Forced Synchronous Layout
  var items = document.querySelectorAll('.mover');

  for (var i = 0; i < items.length; i++) {
    var phase = Math.sin((document.body.scrollTop / 1000) + (i % 2));
    items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
    window.performance.mark("mark_start_frame");
    frame++;
  }
``
 and

`` // To improve scrolling the for loop limit was changed from 200 to 100
 for (var i = 0; i < 100; i++)`` on .ne 566 of main.js ``
