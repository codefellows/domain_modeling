## Domain Modeling

Domain modeling is the process of creating a conceptual model in code for a specific problem. A model describes the various entities, their attributes and behaviors, as well as the constraints that govern the problem domain. An entity that stores data in properties and encapsulates behaviors in methods is commonly referred to as an **object-oriented** model.

A domain model that's articulated well can verify and validate the understanding of a specific problem among various stakeholders. As a communication tool, it defines a vocabulary that can be used within and between both technical and business teams.

### Model epic fails videos

Imagine you've been tasked to build a program that models the popularity of [epic fail](http://www.urbandictionary.com/define.php?term=epic+fail) videos. After months of painstaking research, you've determined that the two essential metrics for gauging popularity are **an epic rating** and **whether or not the video has animals**.

Since you'll be modeling the popularity of many types of videos—parkour epic fails, corgi epic fails, etc.—you'll want to build self-contained objects with the **same attributes and behaviors**. That way, when you need to change the algorithm for determining popularity, the changes will be small and targeted.

![](http://www.gifbin.com/bin/082011/1312218114_corgi_rodeo.gif)

As you read this article, type out and run all code samples you come across. **Do not copy and paste**. Writing out and testing your code will help you remember how to implement domain models in JavaScript later.

### Define a constructor and initialize properties

To define the same properties between many objects, you'll want to use a constructor function. Below is a table that summarizes a JavaScript representation of an `EpicFailVideo` object.

| Property     | Data              | Type    |
|--------------|-------------------|---------|
| `epicRating` | `1` to `10`       | Number  |
| `hasAnimals` | `true` or `false` | Boolean |

Here's an implementation of the `EpicFailVideo` constructor function.

```javascript
var EpicFailVideo = function(epicRating, hasAnimals) {
  this.epicRating = epicRating;
  this.hasAnimals = hasAnimals;
}

var parkourFail = new EpicFailVideo(7, false);
var corgiFail = new EpicFailVideo(4, true);

console.log(parkourFail);
console.log(corgiFail);
```

As you can see, the constructor function is defined using a **function expression**. In other words, the variable `EpicFailVideo` is declared and then assigned a function with two parameters called `epicRating` and `hasAnimals`.

When the function is called, the data inside these parameters are stored inside the `this.epicRating` and `this.hasAnimals` properties respectively. Storing data within properties ensures any newly created object can access that data later.

After the constructor function definition, two objects are **instantiated** with the `new` keyword and their properties are **initialized** by calling the `EpicFailVideo` constructor function. After being instantiated and initialized, these objects are stored inside the `parkourFail` and `corgiFail` variables.

Finally, the two newly created objects are logged to the console.

![](https://i.imgur.com/G2bPElF.png)

This is **object-oriented programming** in JavaScript at its most fundamental level.

1. The `new` keyword instantiates (i.e. creates) an object.
1. The constructor function initializes properties inside that object using the `this` variable.
1. The object is stored in a variable for later use.

### Generate random numbers

To model the random nature of user behavior, you'll need the help of a random number generator. Fortunately, the JavaScript standard library includes a `Math.random()` function for just this sort of occasion.

```javascript
var EpicFailVideo = function(epicRating, hasAnimals) {
  this.epicRating = epicRating;
  this.hasAnimals = hasAnimals;
}

EpicFailVideo.prototype.generateRandom = function(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

var parkourFail = new EpicFailVideo(7, false);
var corgiFail = new EpicFailVideo(4, true);

console.log(parkourFail.generateRandom(1, 5));
console.log(corgiFail.generateRandom(1, 5));
```

As you can see, methods can be added to a constructor function's **prototype**. Think of the prototype as an object's stunt double. Whenever a scene is too dangerous, you can substitute in the prototype to do the work while the object takes all the glory. More on how that works below.

`EpicFailVideo`'s prototype is given a `generateRandom` method which is assigned a function with two parameters called `min` and `max`. The function uses both [Math.floor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/floor) and [Math.random](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random?redirectlocale=en-US&redirectslug=JavaScript%2FReference%2FGlobal_Objects%2FMath%2Frandom) to calculate and return a random integer between `min` and `max`.

When `parkourFail` is asked to run the `generateRandom()` method, it searches through all of its own methods. When it doesn't find the `generateRandom()` method there, `parkourFail` then searches through all of the methods on its `prototype` object. When it finds the `generateRandom()` method on its `prototype` object, `parkourFail` calls the method, passing in `1` and `5` as the arguments. The `generateRandom(1, 5)` method runs and returns a random number between 1 and 5. The exact same process happens for `corgiFail` too.

While it certainly takes longer to locate a method on the prototype object, this technique is an established best practice in JavaScript. When a prototype is shared between two or more objects, like it is for `parkourFail` and `corgiFail`, those objects execute the same code when the `generateRandom()` method is called. And shared code means a running program consumes less memory which is important for devices like smart phones and tablets.

![](https://i.imgur.com/aPEgyN2.png)

With a random number generator in place, you have all pieces you need to start modeling the popularity of epic fail videos.

### Calculate daily Likes

Popularity of a video is measured in Likes. And the formula for calculating Likes is the number of viewers times the percentage of viewers who'll Like a video. In other words, **viewers times percentage**.

To calculate the number of viewers per day, generate a random number between 10 and 30 and then multiply it by the epic rating of that video.

| Random number | Epic rating | Viewers per day |
|---------------|-------------|-----------------|
| 10            | 10          | 100             |
| 20            | 9           | 180             |
| 30            | 8           | 240             |

The percentage of viewers who'll Like a video depends on whether or not the video contains animals.

| Animals | Percentage |
|---------|------------|
| Yes     | 75%        |
| No      | 40%        |

With this knowledge in hand, let's model the number daily Likes an epic fail video will receive.

```javascript
var EpicFailVideo = function(epicRating, hasAnimals) {
  this.epicRating = epicRating;
  this.hasAnimals = hasAnimals;
}

EpicFailVideo.prototype.generateRandom = function(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

EpicFailVideo.prototype.dailyLikes = function() {
  var viewers, percentage;

  viewers = this.generateRandom(10, 30) * this.epicRating;

  if (this.hasAnimals) {
    percentage = 0.75;
  } else {
    percentage = 0.40;
  }

  return Math.round(viewers * percentage);
}

var parkourFail = new EpicFailVideo(7, false);
var corgiFail = new EpicFailVideo(4, true);

console.log(parkourFail.dailyLikes());
console.log(corgiFail.dailyLikes());
```

The `dailyLikes()` method on `EpicFailVideo`'s prototype is assigned a function with no parameters. This method starts by defining two variables called `viewers` and `percentage`.

The `viewers` variable is assigned the value of `this.generateRandom(10, 30)` multiplied by `this.epicRating`. Using the `this` variable, methods can access any property or method on the same object that called the `dailyLikes()` method.

The `percentage` variable is assigned a decimal percentage based on the object's `this.hasAnimals` property. Again, using the `this` variable, methods can access any property of the same object that called the `dailyLikes()` method.

Finally, `viewers` and `percentage` are multiplied, rounded to the nearest integer with [Math.round](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/round), and the value is returned. When the `dailyLikes()` method is called on both `parkourFail` and `corgiFail`, the result is logged to the console.

![](https://i.imgur.com/Ad263R3.png)

Congratulations! You've just built a program that models the daily popularity of epic fail videos.

### Calculate weekly Likes

In addition to modeling the daily Likes, your boss has asked you to model the weekly Likes too. Little does your boss know how easy it is to add behaviors to your domain model.

```javascript
var EpicFailVideo = function(epicRating, hasAnimals) {
  this.epicRating = epicRating;
  this.hasAnimals = hasAnimals;
}

EpicFailVideo.prototype.generateRandom = function(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

EpicFailVideo.prototype.dailyLikes = function() {
  var viewers, percentage;

  viewers = this.generateRandom(10, 30) * this.epicRating;

  if (this.hasAnimals) {
    percentage = 0.75;
  } else {
    percentage = 0.40;
  }

  return Math.round(viewers * percentage);
}

EpicFailVideo.prototype.weeklyLikes = function() {
  var total = 0;

  for (var i = 0; i < 7; i++) {
    total += this.dailyLikes();
  }

  return total;
}

var parkourFail = new EpicFailVideo(7, false);
var corgiFail = new EpicFailVideo(4, true);

console.log(parkourFail.weeklyLikes());
console.log(corgiFail.weeklyLikes());
```

The `weeklyLikes()` method on `EpicFailVideo`'s prototype is assigned a function with no parameters. This method starts by declaring a `total` variable that's initialized to `0`.

The `weeklyLikes()` method proceeds to call `this.dailyLikes()` seven times and adds the returning number of Likes to `total` using the `+=` operator. Afterwards, `total` is returned.

When `weeklyLikes()` is called on both `parkourFail` and `corgiFail`, the returning value is logged to the console.

![](https://i.imgur.com/5inrScd.png)

Congratulations! You've just built a program that models the weekly popularity of epic fail videos.

### Summary

Domain modeling is the process of creating a conceptual model for a specific problem. And a domain model that's articulated well can verify and validate your understanding of that problem.

Here's some tips to follow when building your own domain models.

1. When modeling a single entity that'll have many instances, build self-contained objects with the same attributes and behaviors.
1. Model its attributes with a constructor function that defines and initializes properties.
1. Model its behaviors with small methods that focus on doing one job well.
1. Create instances using the `new` keyword followed by a call to a constructor function.
1. Store the newly created object in a variable so you can access its properties and methods from **outside**.
1. Use the `this` variable within methods so you can access the object's properties and methods from **inside**.

Good luck!
