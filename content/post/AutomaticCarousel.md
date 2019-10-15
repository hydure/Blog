---
title: "Automatic Carousel"
date: 2019-10-15T16:15:17-04:00
draft: false
toc: false
comments: false
categories:
- Programming
tags:
- JavaScript
- HTML & CSS
---

One of the features someone wanted me to implement for their website was an automatic carousel for their site. This article explains some ways on how to implement one.
<!--more-->
We all see that automatic carousels are expected, or at least commonly appear, at the top of many website's homepage as well as throughout the website and its pages. Because of this, my friend wanted their webpage to have its own carousel as well and I happily obliged.

I found a useful and easy-to-use [Bootstrap JS Carousel tutorial](https://www.w3schools.com/bootstrap/bootstrap_ref_js_carousel.asp) that comes with indicators and controls already, and many other cool options.

Unfortunately, my friend did not want to use Bootstrap, so I had to find another way to implement an automatic carousel. [On the same website](https://www.w3schools.com/howto/howto_js_slideshow.asp), I found another way to make a responsive slideshow with only CSS and pure JavaScript merging the code I found on there and changing a little of the logic. Below is the code I came up with that made a decent automatic carousel:

Here was the CSS I used:

    <style>
    * {box-sizing: border-box;}
    body {font-family: Verdana, sans-serif;}
    .mySlides {display: none;}
    img {vertical-align: middle;}

    /* Slideshow container */
    .slideshow-container {
    max-width: 1000px;
    position: relative;
    margin: auto;
    }

    /* Caption text */
    .text {
    color: #f2f2f2;
    font-size: 15px;
    padding: 8px 12px;
    position: absolute;
    bottom: 8px;
    width: 100%;
    text-align: center;
    }

    /* The dots/bullets/indicators */
    .dot {
    cursor: pointer;
    height: 15px;
    width: 15px;
    margin: 0 2px;
    background-color: #bbb;
    border-radius: 50%;
    display: inline-block;
    transition: background-color 0.6s ease;
    }

    .active, .dot:hover {
    background-color: #717171;
    }

    /* Fading animation */
    .fade {
    -webkit-animation-name: fade;
    -webkit-animation-duration: 1.5s;
    animation-name: fade;
    animation-duration: 1.5s;
    }

    @-webkit-keyframes fade {
    from {opacity: .4} 
    to {opacity: 1}
    }

    @keyframes fade {
    from {opacity: .4} 
    to {opacity: 1}
    }

    /* On smaller screens, decrease text size */
    @media only screen and (max-width: 300px) {
    .text {font-size: 11px}
    }

    /* Next & previous buttons */
    .prev, .next {
    cursor: pointer;
    position: absolute;
    top: 50%;
    width: auto;
    margin-top: -22px;
    padding: 16px;
    color: white;
    font-weight: bold;
    font-size: 18px;
    transition: 0.6s ease;
    border-radius: 0 3px 3px 0;
    user-select: none;
    }

    /* Position the "next button" to the right */
    .next {
    right: 0;
    border-radius: 3px 0 0 3px;
    }

    /* On hover, add a black background color with a little bit see-through */
    .prev:hover, .next:hover {
    background-color: rgba(0,0,0,0.8);
    }
    </style>

The CSS will make more sence once you see the HTML I used:

    <div class="slideshow-container">

    <div class="mySlides fade">
    <img src="img_nature_wide.jpg" style="width:100%">
    <div class="text">Caption Text</div>
    </div>

    <div class="mySlides fade">
    <img src="img_snow_wide.jpg" style="width:100%">
    <div class="text">Caption Two</div>
    </div>

    <div class="mySlides fade">
    <img src="img_mountains_wide.jpg" style="width:100%">
    <div class="text">Caption Three</div>
    </div>

    <div class="mySlides fade">
    <img src="img_mountains_wide.jpg" style="width:100%">
    <div class="text">Caption Four</div>
    </div>

    <div class="mySlides fade">
    <img src="img_mountains_wide.jpg" style="width:100%">
    <div class="text">Caption Five</div>
    </div>

    <!-- Next and previous buttons -->
    <a class="prev" onclick="plusSlides(-2)">&#10094;</a>
    <a class="next" onclick="plusSlides(0)">&#10095;</a>
    </div>
    <br>

    <div style="text-align:center">
    <span class="dot" onclick="currentSlide(0)"></span> 
    <span class="dot" onclick="currentSlide(1)"></span> 
    <span class="dot" onclick="currentSlide(2)"></span>
    <span class="dot" onclick="currentSlide(3)"></span>
    <span class="dot" onclick="currentSlide(4)"></span>
    </div>

And below was the pure JavaScript I used to make the carousel automatically rotate, as well as change pictures depending on which dots were pressed:

    <script>
    var slideIndex = 0;
    var timeoutHandle;
    showSlides();

    function showSlides() {
    var i;
    var slides = document.getElementsByClassName("mySlides");
    if (slideIndex < 0) {slideIndex = slides.length - 1}
    var dots = document.getElementsByClassName("dot");
    for (i = 0; i < slides.length; i++) {
        slides[i].style.display = "none";  
    }
    slideIndex++;
    if (slideIndex > slides.length) {slideIndex = 1}
    
    for (i = 0; i < dots.length; i++) {
        dots[i].className = dots[i].className.replace(" active", "");
    }
    slides[slideIndex-1].style.display = "block";  
    dots[slideIndex-1].className += " active";
    timeoutHandle = setTimeout(showSlides, 5000);
    }

    // Next/previous controls
    function plusSlides(n) {
    clearTimeout(timeoutHandle);
    
    showSlides(slideIndex += n);
    }

    // Thumbnail image controls
    function currentSlide(n) {
    clearTimeout(timeoutHandle);
    showSlides(slideIndex = n);
    }
    </script>

I hope you enjoyed this and find the article useful!