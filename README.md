quiz_continue
=============

sample1
 
 router.js
 =========
 
 
var rnc = rnc || {};

// wait for everything to be ready then run the demo code
rnc.resolver.initialize(function () {

  console.log("starting our app");

  if (typeof(StatusBar) !== "undefined") {
    console.log("Setting StatusBar");
    StatusBar.overlaysWebView(false);
    StatusBar.backgroundColorByHexString('#000000');
  } else {
    console.log("NO StatusBar");
  }
  rnc.calculator();
});

swipe.js
======

/*
 * Swipe 1.0
 *
 * Brad Birdsall, Prime
 * Copyright 2011, Licensed GPL & MIT
 *
*/

window.Swipe = function(element, options) {

  // return immediately if element doesn't exist
  if (!element) return null;

  var _this = this;
    
  // retreive options
  this.options = options || {};
  this.index = this.options.startSlide || 0;
  this.speed = this.options.speed || 300;
  this.callback = this.options.callback || function() {};
  this.delay = this.options.auto || 0;

  // reference dom elements
  this.container = element;
  this.element = this.container.children[0]; // the slide pane

  // static css
  this.container.style.overflow = 'hidden';
  this.element.style.listStyle = 'none';

  // trigger slider initialization
  this.setup();

  // add event listeners
  if (this.element.addEventListener) {
    this.element.addEventListener('webkitTransitionEnd', this, false);
    this.element.addEventListener('transitionend', this, false);
    window.addEventListener('resize', this, false);
  }

};

Swipe.prototype = {

  setup: function() {

    // get and measure amt of slides
    this.slides = this.element.children;
    this.length = this.slides.length;

    // return immediately if their are less than two slides
    if (this.length < 2) return null;

    // determine width of each slide
     this.width = this.container.getBoundingClientRect().width;
      //alert(this.container.getBoundingClientRect().width);
   /*   
      if($('body').width() < 768){
          if($('body').width()==480){
              slideWidth = 440;
              this.width = 440;
          }
          else {
              slideWidth = 279;
             this.width = 279; 
          }
      }
      else {
          if($('body').width()==1024){
              slideWidth = 925;
              this.width = 925;
          }
          else {
              slideWidth = 670;
              this.width = 670; 
          }
      }
      */
    // return immediately if measurement fails
    if (!this.width) return null;

    // hide slider element but keep positioning during setup
    //this.container.style.visibility = 'hidden';
      this.container.style.display = 'none';

    // dynamic css
	
    this.element.style.width = (this.slides.length * this.width) + 'px';
    var index = this.slides.length;
    while (index--) {
      var el = this.slides[index];
      el.style.width = this.width + 'px';
      el.style.display = 'table-cell';
      el.style.verticalAlign = 'top';
    }

    // set start position and force translate to remove initial flickering
    this.slide(this.index, 0); 

    // show slider element
    //this.container.style.visibility = 'visible';
      this.container.style.display = 'block';

  },

  slide: function(index, duration) {
    var style = this.element.style;
    style.webkitTransitionDuration = style.transitionDuration = duration + 'ms';
    style.webkitTransform = 'translate3d(' + -(index * this.width) + 'px,0,0)';
    this.index = index;
  },

  getPos: function() {
    
    // return current index position
    return this.index;

  },

  prev: function(delay) {
    // cancel next scheduled automatic transition, if any
    // if not at first slide
    if (this.index) this.slide(this.index-1, this.speed);

  },

  next: function(delay) {
    // cancel next scheduled automatic transition, if any
      if (this.index < this.length - 1){
      this.slide(this.index+1, this.speed);
      }// if not last slide
    //else this.slide(0, this.speed); //if last slide return to start

  },


  handleEvent: function(e) {
    switch (e.type) {
      case 'webkitTransitionEnd':
      case 'transitionend': this.transitionEnd(e); break;
      case 'resize': this.setup(); break;
    }
  },

  transitionEnd: function(e) {
      var item = {prev:this.slides[this.index-1],curr:this.slides[this.index],next:this.slides[this.index+1]};
    //this.callback(e, this.index, this.slides[this.index]);
      this.callback(e, this.index, item);
      
      var oId = window.location.href;
      oId = oId.substr(oId.indexOf("#"));
      if(oId=='#test1_1'){
          adjustTimer(i);
      }
      else if(oId=='#test2_1'){
          adjustTimer(j);
      }
      else if(oId=='#test3_1'){
          adjustTimer(k);
      }
      else if(oId=='#test4_1'){
          adjustTimer(m);
      }
  } 

};


vkeypad.js
===========

