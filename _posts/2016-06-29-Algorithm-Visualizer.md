---
layout:     post
title:      Algorithm Visualizer
date:       2016-06-29 12:32:18
summary:    Algorithm Visualizer
categories: Algorithm
thumbnail: jekyll
tags:
 - algorithm
 - open source
---

I was looking for some open source repositories on Github on which I could contribute, and then I came across Jason Park‘s Algorithm Visualizer on Github. This project seemed prefect to me since I was always interested in algorithm, and this seemed to be a great opportunity to enter in the field of open source contribution.

# What is Algorithm Visualizer ?

As the name suggests, it is a project to visualize standard algorithms, making it easier for people to understand the algorithm by giving a visual representation.



The project is mostly written in javascript, and you can have a look at the project here. Till now, nearly 56 algorithms from 11 different topics are present in the project.

### Build/Run the project

### Build System 

From the wiki of Algorithm Visualizer:
Our project uses gulp to:

* combine all individual modules into a single file
* transpile ES6 code to ES5 with babel.js
* minimize JS and CSS code
* generate source maps
* add vendor prefixer to the css
* provide a server with live-reload


### Installation

We need gulp to install the project in our local directory. It can be installed in the following way:

```
# install gulp globally so you can run it from the command line
npm install -g gulp-cli

# install all dependencies
npm install

# run gulp to start the livereload server on http://localhost:8080 
gulp
```

Check out the version of Nodejs by the command:

```
node --version
```

For Node.js version below 0.12, we need to install an ES6-style Promise Polyfill. This can be done by:-

```
npm install es6-promise
```

Finally, add this line at the top of the gulpfile.babel.js file:

```
var Promise = require ('es6-promise').Promise;
```

### Adding a new algorithm

For adding a new algorithm, we will first have to move on to the respective category folder (or create one category folder if not present) and create a folder for that algorithm. For instance, if we want to add the Sieve of Eratosthenes,  we will have to create a folder on “AlgorithmVisualizer/category_name/name_of_the_algorithm“, which in this case is “AlgorithmVisualizer/number_theory/sieve_of_eratosthenes“. Also, we need to add the information on the category.json script present in the algorithm folder, as shown below :



Moving on to the folder we created for our algorithm  (“AlgorithmVisualizer/number_theory/sieve_of_eratosthenes” in this case), we have 3 types of files :

* desc.json : The json script which contains the details of the algorithm which we are adding. These mainly include:
- Name of  the algorithm and a short description
- The complexity of the algorithm (both Runtime and Space complexities)
- Any online reference, and
- the folder in which the code can be found

seive_desc.jpg

- desc.json file for seive of eratosthenes
- data.js : This is where we declare the data required for the given algorithm. Here, we declare the two 1D arrays (one will store whether the number is prime or not, and the other is for displaying purpose), the tracer and the logTracer required for the implementation.
- code.js : This is where the actual code for the algorithm is written. It takes the values from the data.js and implements the output and displays the result on the Tracer.

seive_blog.jpg
