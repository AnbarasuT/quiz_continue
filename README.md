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

/* This is Virtual KeyPad (vKeyPad)
 * vKeyPad 1.3.1
 * developed by Lapiz
 * Release Date 22.07.14
 *
 * usage: new vKeyPad(".textBoxClass", "#activtyHolder");
 *
 * KeyFormat:
 * "id":id, "innerHTML":innerHTML, "value":value, "isSpecialKey":false, "rowIndex":0, "colSpan":1, "specialClass":"", "isInactive":false
 *
 * extend: -- Extend the key pad with additional elements passed as string
 * 	"beforeKeys":"";
 * 
*/

window.vKeyPad = function(TextBoxSelector, ParentContainerId, Options){
    this.selector = TextBoxSelector;
    this.Options = Options ? Options : {};
    this.ParentContainerId = ParentContainerId;
    this.TextBox = [];
    this.colCount = this.Options.colCount ? this.Options.colCount : 3;
    if (!this.Options.keyPadType) this.Options.keyPadType = "numeric";
    this.Options.isAlphaPad = this.Options.keyPadType.indexOf("alpha") >= 0;
    this.Options.isNumPad = this.Options.keyPadType.indexOf("num") >= 0;
    this.buttonClickColor = this.Options.buttonClickColor ? this.Options.buttonClickColor : "#ffcc66";
    this.buttonBlinkTime = this.Options.buttonBlinkTime ? this.Options.buttonBlinkTime : 100;
    this.keyWidth = this.Options.keyWidth ? this.Options.keyWidth : 45;
    this.keyHeight = this.Options.keyHeight ? this.Options.keyHeight : 45;
    this.autoClear = this.Options.autoClear ? true : false;
    this.clearZero = this.Options.clearZero == false ? false : true;
    this.maximumLength = this.Options.maximumLength ? this.Options.maximumLength : 0; //maxWidth
    this.maxLenMargin = this.Options.maxLenMargin ? this.Options.maxLenMargin : 0;
    this.isDefaultVal = isNaN(this.Options.defaultValue) ? false : true;
    this.defaultValue = this.Options.defaultValue ? this.Options.defaultValue : 0;
    this.isMinLimit = isNaN(this.Options.minValue) ? false : true;
    this.minValue = this.Options.minValue ? this.Options.minValue : 0;
    this.isMaxLimit = isNaN(this.Options.maxValue) ? false : true;
    this.maxValue = this.Options.maxValue ? this.Options.maxValue : 0;
    this.extend = this.Options.extend ? this.Options.extend : {};
    this.onFocusTxtBoxBg = this.Options.onFocusTxtBoxBg ? this.Options.onFocusTxtBoxBg : "rgb(204, 204, 204)";
    this.focusEvent = this.Options.focusEvent ? this.Options.focusEvent:"click";
    this.closeOnBlur = this.Options.closeOnBlur == false ? false:true;
    this.isBlurOnWindowClick = this.Options.isBlurOnWindowClick == false ? false:true;
    this.isFocusOnClick = this.Options.isFocusOnClick == false ? false:true;
    if (!this.Options.isAlphaPad) this.useComplexMath = this.Options.useComplexMath == false ? false : true;
    else this.useComplexMath = false;
    this.isMultiline = this.Options.isMultiline ? true : false;
    this.isSingleExpr = this.Options.isSingleExpr == false ? false : true;
    this.isDraggable = this.Options.isDraggable == false ? false : true;
    this.isForceTaggable = this.Options.isForceTaggable ? true : false;
    this.moveSensitivity = this.Options.moveSensitivity ? this.Options.moveSensitivity : 10;
    this.customTxtBoxCol = this.Options.customTxtBoxCol ? this.Options.customTxtBoxCol : null;
    this.applyTxtBoxBG = this.Options.applyTxtBoxBG ? true:false;
    this.deleteMethod = this.Options.deleteMethod ? this.Options.deleteMethod : "byTag";//byTag, byChar
    this.useParseMath = this.Options.useParseMath ? true : false;
    this.minusSymbol = this.Options.minusSymbol ? this.Options.minusSymbol : "\u2013";
    this.multiSymbol = this.Options.multiSymbol ? this.Options.multiSymbol : "\u00B7";
    this.isFrac = this.Options.isFrac ? true : false;
    this.autoCorrect = this.Options.autoCorrect ? true : false;
    this.hiddenKeys = this.Options.hiddenKeys ? this.Options.hiddenKeys : [];
    this.isAutoAdjustPosition = this.Options.isAutoAdjustPosition == false ? false : true;
    if (this.Options.isAlphaPad) this.colCount = 11;
    this.keys = this.getKeyManifest(this.Options.keys, this.Options.additionalKeys, this.Options.addKeyRowId);
    this.KeyPad;
    this.KeyPadHolder;
    this.CurrentFocus = null;
    this.textBoxOrgBg = this.customTxtBoxCol;
    this.isMultilineBox = false;
    this.isTaggableBox = false;
    this.checkBeforeFocus = false;
    this.mouseDownPos = false;
    this.vKeyPadAttr = "vKeyPadAttr";
    this.disabledTextboxes = [];
    this.vkItalicClass = "vK_italic";
    this.vkBoldClass = "vK_bold";
    
    //Toggleable ButtonState
    this.isShift = false;
    this.isSup = false;
    this.isSub = false;
    this.isItalic = false;
    this.isBold = false;
    this.fractionMode = "";
    this.buttonState = [];
    this.initPos = {x:0, y:0};
    
    this.loadControls();
    this.init();
};

vKeyPad.prototype = {
    init:function(){
        this.KeyPad = $("<div class=\"keypadHolder " + this.Options.keyPadType + "\" />");
	this.KeyPadHolder = $("<div class=\"keypadContainer\" style=\"display:inline-block;\"></div>");
	var vKey = this;
	this.formKey(this.keys["closeKey"][0], this.KeyPad);
	if (this.extend.beforeKeys) this.KeyPad.append(this.extend.beforeKeys);
	for(var keySetKey in this.keys){
	    if (keySetKey != "closeKey") {
		var keySetValue = this.keys[keySetKey];
		var container = $("<div class=\"vKeyPadKeySet vKeyPad_"+keySetKey+"\" style=\"\"></div>");
		this.KeyPad.append(container)
		var index = 0;
		var row;
		
		var lastRowIndex = -1;
		for (index=0; index<keySetValue.length;) {
		    var keyProperty = keySetValue[index];
		    if (keyProperty.rowIndex != lastRowIndex) {
			lastRowIndex = keyProperty.rowIndex;
			var row = $("<div style=\"text-align:center;\"></div>");
			container.append(row);
		    }
		    index++;
		    this.formKey(keyProperty, row);
		}
		this.KeyPad.append(container);
	    }
    	}
	this.KeyPadHolder.append(this.KeyPad);
	$(this.ParentContainerId).append(this.KeyPadHolder);
	try {
	    if (this.isDraggable) 
		$(this.KeyPadHolder).draggable({containment: this.ParentContainerId});
	}
	catch(e){ }
	this.KeyPadHolder.css({"position":"absolute","width":this.KeyPadHolder.width()+"px","height":this.KeyPadHolder.height()+"px"});
	var extH = this.keyHeight/2;
	this.KeyPad.css({"top":(extH-10)+"px","width":this.KeyPad.width()+"px","position":"absolute"});
	this.KeyPadHolder.css({"width":(this.KeyPadHolder.width()+this.keyWidth/2)+"px", "height":(this.KeyPadHolder.height()+extH)+"px"});
	$(window).bind("touchstart mousedown", function(e){vKey.onWindowClick(e,vKey)});
	this.checkBtnState();
	var position = this.KeyPadHolder.position();
	this.initPos = {x:position.left, y:position.top};
	this.hidePad();
    },
    formKey:function(keyProperty, container){
	if (keyProperty) {
	    var vKey = this;
	    var isDoubleKey = false;
	    var innerHTML = keyProperty.innerHTML;
	    var innerHTMLType = (typeof keyProperty.innerHTML).toString();
	    if(innerHTMLType == "object" || innerHTMLType == "array")
		if(keyProperty.innerHTML.length == 2){
		    isDoubleKey = true;
		    innerHTML = "";
		    for(var i=0; i<keyProperty.innerHTML.length; i++){
			innerHTML += "<span class=\"key_multi_"+i+"\">" + keyProperty.innerHTML[i] + "</span>";
			if (i!=keyProperty.innerHTML.length-1) innerHTML += "<br/>";
		    }
		}
	    var isCloseBtn = keyProperty.id == "close";
	    var key = $("<div id=\"key_"+keyProperty.id+"\" class=\"vkBtn "+ (keyProperty.specialClass ? keyProperty.specialClass : "vkBtnKey") + (isDoubleKey ? " vkMultiKey" : "") +"\" " +
		    (!isCloseBtn ? "style=\"display:inline-block; width:"+(this.keyWidth*(keyProperty.colSpan?keyProperty.colSpan:1))+"px; height:"+this.keyHeight+"px; \"" : "") +
		    "></div>");
	    key.html(innerHTML);
	    container.append(key);
	    key.bind("click", function(e){vKey.onNumClick(e, vKey);});
	}
    },
    getKeyManifest:function(customKeys, additionalKeys, addKeyRowId){
	var closeKey=[], headerKeys=[], keys=[], footerKeys=[];
	this.addKeysTo(closeKey, this.eachManifest("close", "<svg version=\"1.1\" xmlns=\"http://www.w3.org/2000/svg\" xmlns:xlink=\"http://www.w3.org/1999/xlink\" width=\"20px\" height=\"20px\" viewBox=\"0 0 20 20\" preserveAspectRatio=\"none\"> <g> <image width=\"20\" height=\"20\" xlink:href=\"data:image/png;base64, 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\"/> </g> </svg>", "close", true, 1, 1, "vkBtnEmpty"));
	if (customKeys) {
	    for (var i=0; i<customKeys.length; i++) {
		var keyInfo = customKeys[i];
		if (typeof keyInfo != "object") {
		    var rowIndex = Math.ceil((i+1)/this.colCount);
		    keyInfo = this.eachManifest(i+1, keyInfo, keyInfo, false, rowIndex, 1);
		}
		this.addKeysTo(keys, keyInfo);
	    }
	}
	else if (this.Options.isAlphaPad) {
	    if(this.Options.isNumPad){
		this.addKeysTo(keys, this.eachManifest("tilde", ["~","`"], ["~","`"], false, 1, 1));
		this.addKeysTo(keys, this.eachManifest("1", ["!","1"], ["!","1"], false, 1, 1));
		this.addKeysTo(keys, this.eachManifest("2", ["@","2"], ["@","2"], false, 1, 1));
		this.addKeysTo(keys, this.eachManifest("3",["#","3"], ["#","3"], false, 1, 1));
		this.addKeysTo(keys, this.eachManifest("4",["$","4"], ["$","4"], false, 1, 1));
		this.addKeysTo(keys, this.eachManifest("5",["%","5"], ["%","5"], false, 1, 1));
		this.addKeysTo(keys, this.eachManifest("6",["^","6"], ["^","6"], false, 1, 1));
		this.addKeysTo(keys, this.eachManifest("7",["&","7"], ["&","7"], false, 1, 1));
		this.addKeysTo(keys, this.eachManifest("8",["*","8"], ["*","8"], false, 1, 1));
		this.addKeysTo(keys, this.eachManifest("9",["(","9"], ["(","9"], false, 1, 1));
		this.addKeysTo(keys, this.eachManifest("0",[")","0"], [")","0"], false, 1, 1));
		this.addKeysTo(keys, this.eachManifest("hyphen",["_","&ndash;"], ["_","<span class=\"vDelByTag\">"+this.minusSymbol+"</span>"], false, 1, 1));
		this.addKeysTo(keys, this.eachManifest("equals",["+","="], ["+","="], false, 1, 1));
	    }
	    this.addKeysTo(keys, this.eachManifest("q","q", "q", false, 2, 1));
	    this.addKeysTo(keys, this.eachManifest("w","w", "w", false, 2, 1));
	    this.addKeysTo(keys, this.eachManifest("e","e", "e", false, 2, 1));
	    this.addKeysTo(keys, this.eachManifest("r","r", "r", false, 2, 1));
	    this.addKeysTo(keys, this.eachManifest("t","t", "t", false, 2, 1));
	    this.addKeysTo(keys, this.eachManifest("y","y", "y", false, 2, 1));
	    this.addKeysTo(keys, this.eachManifest("u","u", "u", false, 2, 1));
	    this.addKeysTo(keys, this.eachManifest("i","i", "i", false, 2, 1));
	    this.addKeysTo(keys, this.eachManifest("o","o", "o", false, 2, 1));
	    this.addKeysTo(keys, this.eachManifest("p","p", "p", false, 2, 1));
	    this.addKeysTo(keys, this.eachManifest("openBracket",["{","["], ["{","["], false, 2, 1));
	    this.addKeysTo(keys, this.eachManifest("closeBracket",["}","]"], ["}","]"], false, 2, 1));
	    this.addKeysTo(keys, this.eachManifest("backwardSlash", ["|","\\"], ["|","\\"], false, 2, 1));
	    this.addKeysTo(keys, this.eachManifest("a", "a", "a", false, 3, 1));
	    this.addKeysTo(keys, this.eachManifest("s", "s", "s", false, 3, 1));
	    this.addKeysTo(keys, this.eachManifest("d","d", "d", false, 3, 1));
	    this.addKeysTo(keys, this.eachManifest("f","f", "f", false, 3, 1));
	    this.addKeysTo(keys, this.eachManifest("g","g", "g", false, 3, 1));
	    this.addKeysTo(keys, this.eachManifest("h","h", "h", false, 3, 1));
	    this.addKeysTo(keys, this.eachManifest("j","j", "j", false, 3, 1));
	    this.addKeysTo(keys, this.eachManifest("k","k", "k", false, 3, 1));
	    this.addKeysTo(keys, this.eachManifest("l","l", "l", false, 3, 1));
	    this.addKeysTo(keys, this.eachManifest("colons",[":",";"], [":",";"], false, 3, 1));
	    this.addKeysTo(keys, this.eachManifest("quotes",["\"","'"], ["\"","'"], false, 3, 1));
	    this.addKeysTo(keys, this.eachManifest("enter","<svg version=\"1.0\" xmlns=\"http://www.w3.org/2000/svg\" width=\"30\" height=\"30\" viewBox=\"0 0 300.000000 300.000000\" preserveAspectRatio=\"xMidYMid meet\"> <g transform=\"translate(0.000000,300.000000) scale(0.100000,-0.100000)\" fill=\"#000000\" stroke=\"none\"> <path d=\"M2398 2038 l-3 -673 -27 -47 c-19 -32 -41 -52 -71 -67 -42 -20 -56 -21 -550 -21 l-507 0 0 201 c0 187 -1 201 -17 195 -23 -9 -1184 -679 -1181 -681 13 -10 1188 -685 1193 -685 3 0 5 90 5 199 l0 200 553 3 c617 5 600 3 752 78 190 93 314 240 386 455 l24 70 3 723 3 722 -280 0 -280 0 -3 -672z\"/> </g> </svg>", "enter", true, 3, 1.5));
	    this.addKeysTo(keys, this.eachManifest("z","z", "z", false, 4, 1));
	    this.addKeysTo(keys, this.eachManifest("x","x", "x", false, 4, 1));
	    this.addKeysTo(keys, this.eachManifest("c","c", "c", false, 4, 1));
	    this.addKeysTo(keys, this.eachManifest("v","v", "v", false, 4, 1));
	    this.addKeysTo(keys, this.eachManifest("b","b", "b", false, 4, 1));
	    this.addKeysTo(keys, this.eachManifest("n","n", "n", false, 4, 1));
	    this.addKeysTo(keys, this.eachManifest("m","m", "m", false, 4, 1));
	    this.addKeysTo(keys, this.eachManifest("comma",["&lt;",","], ["&lt;",","], false, 4, 1));
	    this.addKeysTo(keys, this.eachManifest("fullStop",["&gt;","."], ["&gt;","."], false, 4, 1));
	    this.addKeysTo(keys, this.eachManifest("forwardSlash",["?","/"], ["?","/"], false, 4, 1));
	    this.addKeysTo(keys, this.eachManifest("shift","<svg version=\"1.0\" xmlns=\"http://www.w3.org/2000/svg\" width=\"30\" height=\"30\" viewBox=\"0 0 300.000000 300.000000\" preserveAspectRatio=\"xMidYMid meet\"> <g transform=\"translate(0.000000,300.000000) scale(0.100000,-0.100000)\" fill=\"#000000\" stroke=\"none\"> <path d=\"M999 2268 c-282 -375 -515 -685 -517 -690 -2 -4 116 -8 262 -8 l266 0 0 -765 0 -765 505 0 505 0 0 765 0 765 265 0 c146 0 265 2 265 5 0 4 -1012 1350 -1029 1368 -4 4 -239 -300 -522 -675z\"/> </g> </svg>", "shift", true, 4, 2));
	    this.addKeysTo(footerKeys, this.eachManifest("bold","<b>b</b>", "bold", true, 1, 1));
	    this.addKeysTo(footerKeys, this.eachManifest("italic","<i>i</i>", "italic", true, 1, 1));
	    this.addKeysTo(footerKeys, this.eachManifest("sup","<i>x<sup>y</sup></i>", "sup", true, 1, 1));
	    this.addKeysTo(footerKeys, this.eachManifest("space","Space", " ", false, 1, 8));
	    this.addKeysTo(footerKeys, this.eachManifest("sub","<i>x<sub>y</sub></i>", "sub", true, 1, 1));
	}
	else {
	    this.addKeysTo(keys, this.eachManifest("1", "1", "1", false, Math.ceil(1/this.colCount), 1));
	    this.addKeysTo(keys, this.eachManifest("2", "2", "2", false, Math.ceil(2/this.colCount), 1));
	    this.addKeysTo(keys, this.eachManifest("3", "3", "3", false, Math.ceil(3/this.colCount), 1));
	    this.addKeysTo(keys, this.eachManifest("4", "4", "4", false, Math.ceil(4/this.colCount), 1));
	    this.addKeysTo(keys, this.eachManifest("5", "5", "5", false, Math.ceil(5/this.colCount), 1));
	    this.addKeysTo(keys, this.eachManifest("6", "6", "6", false, Math.ceil(6/this.colCount), 1));
	    this.addKeysTo(keys, this.eachManifest("7", "7", "7", false, Math.ceil(7/this.colCount), 1));
	    this.addKeysTo(keys, this.eachManifest("8", "8", "8", false, Math.ceil(8/this.colCount), 1));
	    this.addKeysTo(keys, this.eachManifest("9", "9", "9", false, Math.ceil(9/this.colCount), 1));
	    this.addKeysTo(keys, this.eachManifest("0", "0", "0", false, Math.ceil(10/this.colCount), 1));
	    this.addKeysTo(keys, this.eachManifest("fullstop", ".", ".", false, Math.ceil(11/this.colCount), 1));
	    this.addKeysTo(keys, this.eachManifest("hyphen","&ndash;", "<span class=\"vDelByTag\">"+this.minusSymbol+"</span>", false, Math.ceil(12/this.colCount), 1));
	    this.addKeysTo(keys, this.eachManifest("multi","&middot;", this.multiSymbol, false, Math.ceil(13/this.colCount), 1));
	    this.addKeysTo(keys, this.eachManifest("sup","<i>x<sup>y</sup></i>", "sup", true, Math.ceil(14/this.colCount), 1));
	    this.addKeysTo(keys, this.eachManifest("sub","<i>x<sub>y</sub></i>", "sub", true, Math.ceil(15/this.colCount), 1));
	    if (this.isFrac) {
		this.addKeysTo(keys, this.eachManifest("frac_space","<svg xmlns:dc=\"http://purl.org/dc/elements/1.1/\" xmlns:cc=\"http://creativecommons.org/ns#\" xmlns:rdf=\"http://www.w3.org/1999/02/22-rdf-syntax-ns#\" xmlns:svg=\"http://www.w3.org/2000/svg\" xmlns=\"http://www.w3.org/2000/svg\" xmlns:sodipodi=\"http://sodipodi.sourceforge.net/DTD/sodipodi-0.dtd\" xmlns:inkscape=\"http://www.inkscape.org/namespaces/inkscape\" width=\"45\" height=\"45\" id=\"svg2\" version=\"1.1\" inkscape:version=\"0.48.4 r9939\" sodipodi:docname=\"Frac_Tri.svg\"> <defs id=\"defs4\" /> <sodipodi:namedview id=\"base\" pagecolor=\"#ffffff\" bordercolor=\"#666666\" borderopacity=\"1.0\" inkscape:pageopacity=\"0.0\" inkscape:pageshadow=\"2\" inkscape:zoom=\"11.2\" inkscape:cx=\"20.126333\" inkscape:cy=\"18.911717\" inkscape:document-units=\"px\" inkscape:current-layer=\"layer1\" showgrid=\"false\" inkscape:window-width=\"1440\" inkscape:window-height=\"844\" inkscape:window-x=\"-4\" inkscape:window-y=\"-4\" inkscape:window-maximized=\"1\" /> <metadata id=\"metadata7\"> <rdf:RDF> <cc:Work  rdf:about=\"\">  <dc:format>image/svg+xml</dc:format>  <dc:type  rdf:resource=\"http://purl.org/dc/dcmitype/StillImage\" />  <dc:title /> </cc:Work> </rdf:RDF> </metadata> <g inkscape:label=\"Layer 1\" inkscape:groupmode=\"layer\" id=\"layer1\" transform=\"translate(0,-1007.3621)\"> <rect style=\"fill:#dadbdc;fill-opacity:1;stroke:#000000;stroke-width:1.02014887;stroke-linecap:butt;stroke-linejoin:miter;stroke-miterlimit:4;stroke-opacity:1;stroke-dasharray:2.04029769, 1.02014885;stroke-dashoffset:2.75440188\" id=\"rect3008\" width=\"19.059156\" height=\"17.202045\" x=\"24.959759\" y=\"1009.2578\" rx=\"4.1992636\" ry=\"3.6011755\" /> <rect ry=\"3.6011755\" rx=\"4.1992636\" y=\"1022.1646\" x=\"1.0030185\" height=\"17.202045\" width=\"19.059156\" id=\"rect3778\" style=\"fill:#000000;fill-opacity:0;stroke:#000000;stroke-width:1.02;stroke-linecap:butt;stroke-linejoin:miter;stroke-miterlimit:4;stroke-opacity:1;stroke-dasharray:2.04,1.02;stroke-dashoffset:2.75399992\" /> <rect ry=\"3.6011755\" rx=\"4.1992636\" y=\"1033.4001\" x=\"24.959759\" height=\"17.202045\" width=\"19.059156\" id=\"rect3780\" style=\"fill:#000000;fill-opacity:0;stroke:#000000;stroke-width:1.02014887;stroke-linecap:butt;stroke-linejoin:miter;stroke-miterlimit:4;stroke-opacity:1;stroke-dasharray:2.04029769, 1.02014885;stroke-dashoffset:2.75440188\" /> <path style=\"fill:none;stroke:#000000;stroke-width:1.03998303;stroke-linecap:butt;stroke-linejoin:miter;stroke-miterlimit:4;stroke-opacity:1;stroke-dasharray:none\" d=\"m 24.9716,1029.8092 19.035404,0.093\" id=\"path3782\" inkscape:connector-curvature=\"0\" /> </g> </svg>", "frac_space", true, Math.ceil(16/this.colCount), 1));
		this.addKeysTo(keys, this.eachManifest("frac_slash","<svg xmlns:dc=\"http://purl.org/dc/elements/1.1/\" xmlns:cc=\"http://creativecommons.org/ns#\" xmlns:rdf=\"http://www.w3.org/1999/02/22-rdf-syntax-ns#\" xmlns:svg=\"http://www.w3.org/2000/svg\" xmlns=\"http://www.w3.org/2000/svg\" xmlns:sodipodi=\"http://sodipodi.sourceforge.net/DTD/sodipodi-0.dtd\" xmlns:inkscape=\"http://www.inkscape.org/namespaces/inkscape\" width=\"45\" height=\"45\" id=\"svg2\" version=\"1.1\" inkscape:version=\"0.48.4 r9939\" sodipodi:docname=\"Frac_Bi.svg\"> <defs id=\"defs4\" /> <sodipodi:namedview id=\"base\" pagecolor=\"#ffffff\" bordercolor=\"#666666\" borderopacity=\"1.0\" inkscape:pageopacity=\"0.0\" inkscape:pageshadow=\"2\" inkscape:zoom=\"11.2\" inkscape:cx=\"31.360593\" inkscape:cy=\"24.435448\" inkscape:document-units=\"px\" inkscape:current-layer=\"layer1\" showgrid=\"false\" inkscape:window-width=\"1440\" inkscape:window-height=\"844\" inkscape:window-x=\"-4\" inkscape:window-y=\"-4\" inkscape:window-maximized=\"1\" /> <metadata id=\"metadata7\"> <rdf:RDF> <cc:Work  rdf:about=\"\">  <dc:format>image/svg+xml</dc:format>  <dc:type  rdf:resource=\"http://purl.org/dc/dcmitype/StillImage\" />  <dc:title /> </cc:Work> </rdf:RDF> </metadata> <g inkscape:label=\"Layer 1\" inkscape:groupmode=\"layer\" id=\"layer1\" transform=\"translate(0,-1007.3621)\"> <rect style=\"fill:#dadbdc;fill-opacity:0;stroke:#000000;stroke-width:1.03771377;stroke-linecap:butt;stroke-linejoin:miter;stroke-miterlimit:4;stroke-opacity:1;stroke-dasharray:2.07542751, 1.03771376;stroke-dashoffset:2.80182708\" id=\"rect3008\" width=\"19.380491\" height=\"17.492069\" x=\"13.511601\" y=\"1008.9626\" rx=\"4.2700624\" ry=\"3.6618907\" /> <rect ry=\"3.6618907\" rx=\"4.2700624\" y=\"-1051.0043\" x=\"13.511601\" height=\"17.492069\" width=\"19.380491\" id=\"rect3780\" style=\"fill:#dadbdc;fill-opacity:1;stroke:#000000;stroke-width:1.03734839;stroke-linecap:butt;stroke-linejoin:miter;stroke-miterlimit:4;stroke-opacity:1;stroke-dasharray:2.07469678, 1.0373484;stroke-dashoffset:2.80084065\" transform=\"scale(1,-1)\" /> <path style=\"fill:none;stroke:#000000;stroke-width:1.05751705;stroke-linecap:butt;stroke-linejoin:miter;stroke-miterlimit:4;stroke-opacity:1;stroke-dasharray:none\" d=\"m 13.523642,1029.8607 19.356338,0.094\" id=\"path3782\" inkscape:connector-curvature=\"0\" /> </g> </svg>", "frac_slash", true, Math.ceil(17/this.colCount), 1));
	    }
	}
	this.addKeysTo(footerKeys, this.eachManifest("backspace", "<svg class=\"delete\" width=\"40\" height=\"35\" viewBox=\"0 0 1024 1024\"><g><path d=\"M921.6 153.6h-489.165c-22.528 0-54.835 12.134-71.782 26.982l-347.955 304.435c-16.947 14.848-16.947 39.117 0 53.965l347.955 304.486c16.947 14.797 49.254 26.931 71.782 26.931h489.165c56.371 0 102.4-46.080 102.4-102.4v-512c0-56.32-46.029-102.4-102.4-102.4zM777.779 716.8l-130.918-130.918-130.816 130.918-73.933-73.882 130.867-130.918-130.867-130.867 73.933-73.933 130.867 130.867 130.867-130.867 73.882 73.933-130.816 130.867 130.867 130.867-73.933 73.933z\"></path></g></svg>", "backspace", true, 1, 1));
	if (additionalKeys) {
	    addKeyRowId = addKeyRowId ? addKeyRowId : 0;
	    for (var i=0; i<additionalKeys.length; i++) {
		var keyInfo = additionalKeys[i];
		if (typeof keyInfo != "object") {
		    var rowIndex = Math.ceil((i+1)/this.colCount)+addKeyRowId;
		    keyInfo = this.eachManifest(keyInfo, keyInfo, keyInfo, false, rowIndex, 1);
		}
		this.addKeysTo(keys, keyInfo, true);
	    }
	}
	return {"closeKey":closeKey, "headerKeys":headerKeys, "contentKeys":keys, "footerKeys":footerKeys};
    },
    addKeysTo:function(array, key, addKeys){
	if (this.hiddenKeys.indexOf(key.id) < 0 || addKeys) array.push(key);
    },
    eachManifest:function(id, innerHTML, value, isSpecialKey, rowIndex, colSpan, specialClass, isInactive) {
	var keyManifest = {"id":id, "innerHTML":innerHTML, "value":value, "isSpecialKey":isSpecialKey, "rowIndex":rowIndex, "colSpan":colSpan, "specialClass":specialClass ? specialClass : "", "isInactive":isInactive ? true : false};
	return keyManifest;
    },
    loadControls:function(){
	this.TextBox = $(this.selector);
	for (var t=0; t<this.TextBox.length; t++) {
	    var txtBox = $(this.TextBox[t]);
	    if (txtBox[0]) {
		try{
		    txtBox.unbind("click");
		    txtBox.unbind("focus");
		    txtBox.unbind("blur");
		}
		catch(e){ }
		var vKey = this;
		var focusEvent = this.focusEvent.toLowerCase();
		if (focusEvent.indexOf("start") < 0 && focusEvent.indexOf("down") < 0) {
		    this.checkBeforeFocus = true;
		    if (this.isTouchDevice()) txtBox.bind("touchstart",function(e){vKey.storeDownPos(e, vKey)});
		    else txtBox.bind("mousedown",function(e){vKey.storeDownPos(e, vKey)});
		}
		if (this.isTouchDevice()) txtBox.bind("touchend",function(e){vKey.tryToFocus(e, vKey)});
		else txtBox.bind("mouseup",function(e){vKey.tryToFocus(e, vKey)});
		txtBox.attr("readonly", "readonly");
		txtBox.blur();
	    }
	}
    },
    setKeyState:function(keyCode, state, noCheck){
	var keyProperty = this.getKeyWithId(keyCode);
	if (keyProperty) {
	    keyProperty.isInactive = !state;
	    if (!noCheck) this.checkBtnState();
	}
    },
    saveBtnState:function(){
	if (this.CurrentFocus) {
	    var buttonState = {};
	    buttonState.isSup = this.isSup;
	    buttonState.isSub = this.isSub;
	    buttonState.isItalic = this.isItalic;
	    buttonState.isBold = this.isBold;
	    buttonState.fractionMode = this.fractionMode;
	    var string_btnState = JSON.stringify(buttonState).replace(/"/g, "'");
	    $(this.CurrentFocus).attr(this.vKeyPadAttr, string_btnState);
	}
    },
    loadBtnState:function(){
	if (this.CurrentFocus) {
	    var string_btnState = $(this.CurrentFocus).attr(this.vKeyPadAttr);
	    if (string_btnState) {
		string_btnState = string_btnState.replace(/'/g, "\"");
		var buttonState = JSON.parse(string_btnState);
		this.isSup = buttonState.isSup;
		this.isSub = buttonState.isSub;
		this.isBold = buttonState.isBold;
		this.isItalic = buttonState.isItalic;
		this.fractionMode = buttonState.fractionMode;
	    }
	    else {
		this.isSup = false;
		this.isSub = false;
		this.isItalic = false;
		this.isBold = false;
		this.fractionMode = "";
	    }
	}
	this.checkBtnState();
    },
    checkBtnState:function(){
	if (this.isShift) {
	    this.KeyPad.find(".key_multi_0").removeClass("inactive").addClass("active");
	    this.KeyPad.find(".key_multi_1").addClass("inactive").removeClass("active");
	    this.KeyPad.find("#key_shift").removeClass("inactive").addClass("active");
	}
	else{
	    this.KeyPad.find(".key_multi_0").addClass("inactive").removeClass("active");
	    this.KeyPad.find(".key_multi_1").removeClass("inactive").addClass("active");
	    this.KeyPad.find("#key_shift").addClass("inactive").removeClass("active");
	}
	
	if (this.isSup) this.KeyPad.find("#key_sup").removeClass("inactive").addClass("active");
	else this.KeyPad.find("#key_sup").addClass("inactive").removeClass("active");
	if (this.isSub) this.KeyPad.find("#key_sub").removeClass("inactive").addClass("active");
	else this.KeyPad.find("#key_sub").addClass("inactive").removeClass("active");
	if (this.isItalic) this.KeyPad.find("#key_italic").removeClass("inactive").addClass("active");
	else this.KeyPad.find("#key_italic").addClass("inactive").removeClass("active");
	if (this.isBold) this.KeyPad.find("#key_bold").removeClass("inactive").addClass("active");
	else this.KeyPad.find("#key_bold").addClass("inactive").removeClass("active");
	if (this.fractionMode == "denominator") this.KeyPad.find("#key_frac_slash").removeClass("inactive").addClass("active");
	else this.KeyPad.find("#key_frac_slash").addClass("inactive").removeClass("active");
	this.saveBtnState();
	
	if (this.isMultilineBox) this.setKeyState("enter", true, true);
	else this.setKeyState("enter", false, true);
	for(var keySetKey in this.keys){
	    var keySetValue = this.keys[keySetKey];
	    for (var index=0; index<keySetValue.length;index++) {
		keyProperty = keySetValue[index];
		var id = "key_"+keyProperty.id;
		var key = $("#"+id);
		if (keyProperty.isInactive) key.addClass("disabled");
		else key.removeClass("disabled");
		if (typeof keyProperty.innerHTML == "string")
		    if (keyProperty.innerHTML.length == 1){
			if(this.isShift) keyProperty.innerHTML = keyProperty.innerHTML.toUpperCase();
			else keyProperty.innerHTML = keyProperty.innerHTML.toLowerCase();
			key.html(keyProperty.innerHTML);
		    }
	    }
	}
    },
    onWindowClick:function(event, vKey){
	var vK = vKey.getSelf();
	var isTextBox = vK.getCurrentFocus(event.target);
	var isKeyPad = $(event.target).hasClass("keypadHolder");
	if (!isKeyPad){
	    isKeyPad = $(event.target).parents(".keypadHolder");
	    isKeyPad = isKeyPad.length != 0;
	}
	if (!isTextBox && !isKeyPad && this.isBlurOnWindowClick && this.handleWindowClick(event)){
	    if (this.closeOnBlur) vK.hidePad();
	    else this.blur();
	}
    },
    handleWindowClick:function(event){ return true; },
    getKeyWithId:function(id){
	id = id.replace("key_","");
	var keyProperty;
	for(var keySetKey in this.keys){
	    var keySetValue = this.keys[keySetKey];
	    for (var index=0; index<keySetValue.length;index++) {
		if (keySetValue[index].id == id) {
		    keyProperty = keySetValue[index];
		    break;
		}
		if (keyProperty) break;
	    }
	}
	return keyProperty;
    },
    blinkKey:function(key){
	if (key.hasClass("vkBtnKey")) {
	    key.css({"background":this.buttonClickColor});
	    setTimeout(function(){
		key.css({"background":""});
	    },this.buttonBlinkTime);
	}
    },
    onNumClick:function(event, vKey) {
	var vK = vKey.getSelf();
        if (!event) event = window.event;
	var key = $(event.target);
	if (!vK.CurrentFocus) return;
	var isDiv = vK.CurrentFocus.tagName.toLowerCase() == "div";
	if (!key.hasClass("vkBtn")) key = key.parents(".vkBtn");
	var keyDetails = this.getKeyWithId(key.attr("id"));
	if (keyDetails.isInactive) return;
	var keyText = keyDetails.value;
	var typeOfValue = (typeof keyText).toString().toLowerCase();
	if(typeOfValue == "object" || typeOfValue == "array") {
	    if (this.isShift || keyText.length == 1) keyText = keyText[0].toString();
	    else if (keyText.length >= 2) keyText = keyText[1].toString();
	}
	else keyText = keyText.toString();
	if (!this.isTaggableBox) {
	    keyText = $("<span>"+keyText+"</span>").text();
	}
	if (keyDetails.isSpecialKey) {
	    if (keyText == "shift"){
		this.isShift = !this.isShift;
		this.checkBtnState();
		return;
	    }
	    else if (keyText == "close"){
		this.hidePad();
		return;
	    }
	    else if (keyText == "sup"){
		this.isSup = !this.isSup;
		if (this.isSup) this.isSub = false;
		this.checkBtnState();
		return;
	    }
	    else if (keyText == "sub"){
		this.isSub = !this.isSub;
		if (this.isSub) this.isSup = false;
		this.checkBtnState();
		return;
	    }
	    else if (keyText == "italic"){
		this.isItalic = !this.isItalic;
		if (this.isItalic) this.isBold = false;
		this.checkBtnState();
		return;
	    }
	    else if (keyText == "bold"){
		this.isBold = !this.isBold;
		if (this.isBold) this.isItalic = false;
		this.checkBtnState();
		return;
	    }
	}
	this.blinkKey(key);
	if (vK.CurrentFocus && keyDetails) {
	    var pass = true;
	    var existingText = isDiv ? $(vK.CurrentFocus).html() : vK.CurrentFocus.value;
	    if ($(vK.CurrentFocus).children().length > 0){
		if ($(vK.CurrentFocus).children()[0].tagName.toString().toLowerCase() == "fmath")
		    existingText = $(vK.CurrentFocus).children().eq(0).attr("alttext");
	    }
	    if (!existingText) existingText = "";
	    else if (existingText.trim() == "") existingText = "";
	    if(keyDetails.isSpecialKey){
		pass = false;
		if (keyText == "enter"){
		    if(this.isMultilineBox && this.isTaggableBox) keyText = "<br />";
		    else if(this.isMultilineBox && !this.isTaggableBox) keyText = "\n";
		    pass = true;
		}
		else if (keyText == "backspace") {
		    var backspaceResult = this.backspaceText(existingText);
		    existingText = backspaceResult.text;
		    pass = backspaceResult.pass;
		}
		if (keyText == "frac_space"){
		    existingText = this.onFracSpace(existingText);
		    pass = true;
		    this.checkBtnState();
		}
		else if (keyText == "frac_slash"){
		    existingText = this.onFracSlash(existingText);
		    pass = true;
		    this.checkBtnState();
		}
		else if (keyDetails.callback) {
		    try{
			var newText = keyDetails.callback(this, existingText, keyDetails);
			if ((typeof newText).toString() == "string" && this.checkWithinLength(newText)) {
			    if (isDiv) $(vK.CurrentFocus).html(newText);
			    else vK.CurrentFocus.value = newText;
			}
		    }
		    catch(e){ pass = false; }
		}
	    }
	    if(pass) {
		if (this.checkWithinLength(existingText) || keyText == "backspace") {
		    var keyedText = keyText;
		    if (vK.isShift) keyedText = keyedText.toUpperCase();
		    else keyedText = keyedText.toLowerCase();
		    
		    var newText = existingText;
		    var textAsObj = $("<s>"+existingText+"</s>");
		    
		    if(vK.useComplexMath && keyText!="backspace" && !keyDetails.isSpecialKey){
			var _existingText = textAsObj.text();
			var lastChar = "", lastBtOne = "", lastSet = "";
			if (_existingText.length > 0) lastChar = _existingText.substr(_existingText.length-1);
			if (_existingText.length > 1) lastBtOne = _existingText.substr(_existingText.length-2, 1);
			//Common Check >>
			if (keyedText == " "){
			    if (_existingText.length <= 0) pass = false;
			}
			else if (keyedText == "/") {
			    if (_existingText.length <= 0) pass = false;
			    else if (_existingText.length >= 0){
				if (lastChar == "." || lastChar == " ") pass = false;
			    }
			}
			//Common Check <<
			//Single Check >>
			if (!vK.isMultiline && pass) {
			    if (keyedText == " "){
				if (_existingText.indexOf(".") >= 0 || _existingText.indexOf("/") >= 0) pass = false;
			    }
			    else if (keyedText == "."){
				if (_existingText.indexOf(".") >= 0 || _existingText.indexOf("/") >= 0) pass = false;
			    }
			    else if (keyedText == "/") {
				if (_existingText.indexOf("/") >= 0 || _existingText.indexOf(".") >= 0) pass = false;
			    }
			    else if (keyedText == this.minusSymbol) {
				if (_existingText != "") pass = false;
			    }
			}
			//Single Check <<
			//MultiTarget Check >>
			else if(vK.isMultiline && pass) {
			    lastSet = _existingText;
			    if (_existingText.length > 0){
				var plsIndx = _existingText.lastIndexOf("+");
				var minIndx = _existingText.lastIndexOf(this.minusSymbol);
				var eqIndx = _existingText.lastIndexOf("=");
				var lastIndx = Math.max(plsIndx, minIndx, eqIndx);
				if (lastIndx >= 0) lastSet = _existingText.substr(lastIndx+1);
			    }
			    if (keyedText == " "){
				if(lastChar == "" || lastChar == " " || lastChar == "/" || lastChar == ".") pass = false;
			    }
			    else if (keyedText == "."){
				if(lastChar == "." || lastChar == "." || lastSet.indexOf(".") >= 0) pass = false;
			    }
			    else if (keyedText == "/") {
				if (lastChar == "." || lastChar == this.minusSymbol || lastChar == " " || lastChar == "+" || lastSet.indexOf("/") >= 0) pass = false;
			    }
			    else if (keyedText == this.minusSymbol) {
				if (lastChar == "." || lastChar == "/" || lastChar == this.minusSymbol || lastChar == " ") pass = false;
			    }
			    else if (keyedText == "+") {
				if (lastChar == "." || lastChar == "/" || lastChar == this.minusSymbol || lastChar == "+" || lastChar == "") pass = false;
			    }
			    else if (keyedText == "=") {
				if (lastChar == "." || lastChar == "/" || lastChar == this.minusSymbol || lastChar == "+" || lastChar == "=") pass = false;
				else if (lastChar == " " && (lastBtOne == this.minusSymbol || lastBtOne == "+" || lastBtOne == "." || lastBtOne == "/" || lastBtOne == "=")) pass = false;
			    }
			}
			//MultiTarget Check <<
			//FinalCheck >>
			if (keyedText == "."){
			    if (_existingText.length <= 0 || lastChar == "+" || lastChar == this.minusSymbol || lastChar == "=" || lastChar == " " || lastChar == "/"){
				keyedText = "0"+keyedText;
				if (lastBtOne == " " && lastBtOne != "" && (lastChar != "+" && lastChar !=this.minusSymbol && lastChar != "=")) keyedText = "+"+keyedText;
			    }
			}
			if (!isNaN(keyedText) && lastBtOne != "") {
			    if (lastChar == " " && !isNaN(lastBtOne) && lastSet.indexOf("/") >= 0) keyedText = "+"+keyedText;
			}
			//FinalCheck <<
		    }
		    
		    if (pass) {
			if (keyText != "backspace" && !keyDetails.isSpecialKey) {
			    var reqTags = [];
			    if (this.isSup) reqTags.push("sup");
			    else if (this.isSub) reqTags.push("sub");
			    if (this.isItalic) reqTags.push("i");
			    else if (this.isBold) reqTags.push("b");
			    newText = this.createTaggedText(newText, keyedText, reqTags);
			}
			var _newText = newText;
			
			try{
			    var minusToHyphReg = new RegExp(this.minusSymbol, "g");
			    var txtAsNum = parseFloat(newText.replace(minusToHyphReg, "-").replace(/\s/g, "").replace(/,/g, ""));
			    if (this.isMaxLimit) if (txtAsNum > this.maxValue){
				if (this.autoCorrect) newText = this.maxValue.toString();
				else newText = existingText;
			    }
			    if (this.isMinLimit) if (txtAsNum < this.minValue){
				if (this.autoCorrect) newText = this.minValue.toString();
				else newText = existingText;
			    }
			    if (isNaN(newText)) newText = _newText;
			}
			catch(e){ newText = _newText; }
			
			if (!this.checkWithinLength(newText) && keyText != "backspace") return;
			if (isDiv) {
			    var displayVal = newText;
			    if (this.useParseMath) displayVal = "$"+newText+"$";
			    $(vK.CurrentFocus).html(displayVal);
			    if (this.useParseMath) vK.parseMath();
			    if (vK.isMultiline && $(vK.CurrentFocus).children().length > 0) {
				var _innerWidth = $(vK.CurrentFocus).children().eq(0).width();
				var _outterWidth = $(vK.CurrentFocus).width();
				if (_outterWidth - _innerWidth < 10) {
				    displayVal = existingText;
				    if (this.useParseMath) displayVal = "$"+displayVal+"$";
				    $(vK.CurrentFocus).html(displayVal);
				    if (this.useParseMath) vK.parseMath();
				}
			    }
			}
			else vK.CurrentFocus.value = newText;
		    }
		}
	    }
	    vK.onTextChanged(event, vK);
	}
    },
    checkWithinLength:function(newText){
	var result = false;
	if ((typeof this.maximumLength).toLowerCase() == "string") {
	    if (this.CurrentFocus) {
		var testingCopy = $("<div></div>");
		testingCopy.css({"white-space":"nowrap", "width":"auto", "display":"inline-block", "min-width":"auto", "max-width":"auto", "font-size":$(this.CurrentFocus).css("font-size"), "font-family":$(this.CurrentFocus).css("font-family"), "word-spacing":$(this.CurrentFocus).css("word-spacing"), "letter-spacing":$(this.CurrentFocus).css("letter-spacing")});
		$("body").append(testingCopy);
		try{
		    testingCopy.html(newText);
		    if (this.maximumLength == "maxWidth") result = $(this.CurrentFocus).width() >= testingCopy.width() + this.maxLenMargin;
		}
		catch(e){ }
		testingCopy.remove();
	    }
	}
	else result = (!this.maximumLength || this.textLength(newText) < this.maximumLength);
	return result;
    },
    backspaceText:function(text) {
	if (!text) text = "";
	var pass;
	if(text.length > 0){
	    var textAsObject = $("<t>"+text+"</t>");
	    text = this.removeText(textAsObject);
	}
	this.checkBtnState();
	return {text:text, pass:true};
    },
    removeText:function(object){
	var childrenLen = object.children().length;
	var taggedContent = object.html();
	var lastChar = taggedContent[taggedContent.length-1];
	if(lastChar == ";" && this.isTaggableBox){
	    var lastEntity = taggedContent.substr(taggedContent.lastIndexOf("&"));
	    if (lastEntity.length > 3 && lastEntity.indexOf(" ") < 0 && lastEntity.indexOf(";") == lastEntity.length-1)
		taggedContent = taggedContent.substr(0, taggedContent.length-lastEntity.length);
	    else taggedContent = taggedContent.substr(0, taggedContent.length-1);
	    object.html(taggedContent);
	}
	else if(lastChar == ">" && this.isTaggableBox && childrenLen){
	    var tag = object.children().eq(childrenLen-1);
	    if (object.hasClass("vK_fractionSet")) {
		this.fractionMode = "denominator";
		var fracRoot = object;
		tag = fracRoot.find(".vK_denominator");
		if (!tag.text()) {
		    tag = fracRoot.find(".vK_numerator");
		    this.fractionMode = "numerator";
		}
	    }
	    textContent = this.removeText(tag);
	    if (textContent) {
		if (tag.hasClass("vdelbytag")) tag.remove();
		else if (this.isOutOfFrac(tag)) {
		    var balanceText = tag.parents(".vK_fractionSet").text();
		    var parent = tag.parents(".vK_fractionSet").parent();
		    tag.parents(".vK_fractionSet").remove();
		    parent.html(parent.html() + balanceText);
		    this.fractionMode = "";
		    otherPro = true;
		}
		tag.html(textContent);
	    }
	    else {
		var tagName = tag[0].tagName.toLowerCase();
		if (tagName == "span" && (tag.hasClass("vK_numerator") || tag.hasClass("vK_denominator"))) {
		    if (tag.hasClass("vK_denominator")) this.fractionMode = "denominator";
		    else if (tag.hasClass("vK_numerator")) this.fractionMode = "numerator";
		    if (tag.text().trim() == "" && tag.hasClass("vK_numerator")) {
			var balanceText = tag.parents(".vK_fractionSet").text();
			var parent = tag.parents(".vK_fractionSet").parent();
			tag.parents(".vK_fractionSet").remove();
			parent.html(parent.html() + balanceText);
			this.fractionMode = "";
		    }
		}
		else {
		    tag.remove();
		    if(tagName == "sup") this.isSup = false;
		    else if(tagName == "sub") this.isSub = false;
		}
		if (tag.hasClass("vdelbytag")) tag.remove();
	    }
	}
	else if (taggedContent) {
	    if (!this.isOutOfFrac(object)) object.html(taggedContent.substr(0, taggedContent.length-1));
	    var tagName = object[0].tagName.toLowerCase();
	    if(tagName == "sup") this.isSup = true;
	    else this.isSup = false;
	    if(tagName == "sub") this.isSub = true;
	    else this.isSub = false;
	}
	return object.html();
    },
    isOutOfFrac:function(tag){
	if(tag.hasClass("vK_numerator") || tag.parents(".vK_numerator").length > 0){
	    var parentHtml = tag.parents(".vK_fractionSet").html();
	    if(parentHtml.indexOf("<span class=\"vK_fraction\">") == 0){
		return true;
	    }
	}
	return false;
    },
    addText:function(object, keyedText, tags, classes){
	var childrenLen = object.children().length;
	var taggedContent = object.html();
	var isAddToTag = false;
	var isAdded = false;
	var toTag = [], toClass = [];
	var preDeterminedTag;
	if (this.fractionMode) {
	    preDeterminedTag = object.find(".vK_"+this.fractionMode);
	    if (preDeterminedTag.length) preDeterminedTag = preDeterminedTag[preDeterminedTag.length-1];
	    else preDeterminedTag = null;
	}
	if (tags != null && tags.length != 0 && !isAdded && !preDeterminedTag) {
	    var lastChar = taggedContent[taggedContent.length-1];
	    if(lastChar == ">" && this.isTaggableBox && childrenLen){
		var childTag = object.children().eq(childrenLen-1);
		var tagName = childTag[0].tagName.toLowerCase();
		var tagIndex = tags.indexOf(tagName);
		if (tagIndex >= 0){
		    var c = classes[tagIndex];
		    if ((c && childTag.hasClass(c)) || !c) isAdded = this.addText(childTag, keyedText, tags, classes).isAdded;
		}
	    }
	    if(!isAdded) {
		var tagName = object[0].tagName.toLowerCase();
		for(var t=0; t<tags.length; t++){
		    tag = tags[t];
		    var c = classes[t];
		    if (c) {
			if(tagName != tag && !object.hasClass(c) && object.parents(tag+"."+c).length <= 0) {
			    toTag.push(tag);
			    toClass.push(c);
			}
		    }
		    else if(tagName != tag && object.parents(tag).length <= 0) {
			toTag.push(tag);
			toClass.push(c);
		    }
		}
		isAddToTag = true;
	    }
	}
	else if(preDeterminedTag) {
	    isAdded = true;
	    var taggedContent = $(preDeterminedTag).html();
	    $(preDeterminedTag).html(taggedContent+this.tagText(keyedText, tags, classes));
	}
	else isAddToTag = true;
	if (isAddToTag && !isAdded) {
	    isAdded = true;
	    object.html(taggedContent + this.tagText(keyedText, toTag, toClass));
	}
	return {object:object, isAdded:isAdded};
    },
    tagText:function(text, tags, classes){
	var newText = text;
	for (var t=0; t<tags.length; t++) {
	    var tag = tags[t];
	    var c = classes[t];
	    newText = "<"+tag + (c ? " class=\"" + c + "\"" : "") + ">"+newText+"</"+tag+">"
	}
	return newText;
    },
    createTaggedText:function(existingText, keyedText, tags){
	var textAsObject = $("<t>"+existingText+"</t>");
	var _tags = [];
	var _class = [];
	for (var t=0; t<tags.length; t++) {
	    var tag = tags[t];
	    var openInd = tag.indexOf("[");
	    if(openInd > 0){
		_tags.push(tag.substr(0, openInd));
		_class.push(tag.substr(openInd+1,tag.indexOf("]")-openInd-1));
	    }
	    else {
		_tags.push(tag);
		_class.push("");
	    }
	}
	return this.addText(textAsObject, keyedText, _tags, _class).object.html();
    },
    onFracSpace:function(existingText){
	var returnVal = existingText;
	var prevFracMode = this.fractionMode;
	if (this.fractionMode == "") returnVal = this.onAddFrac(existingText, "space");
	if (!this.checkWithinLength(returnVal)) {
	    returnVal = existingText;
	    this.fractionMode = prevFracMode;
	}
	return returnVal;
    },
    onFracSlash:function(existingText){
	var returnVal = existingText;
	var prevFracMode = this.fractionMode;
	var textAsObj = $("<t>"+existingText+"</t>");
	if(this.fractionMode == "denominator"){
	    var denominatorDiv = textAsObj.find(".vK_"+this.fractionMode);
	    if (denominatorDiv.text().trim() != "") this.fractionMode = "";
	    this.doChangeStates();
	}
	else if(this.fractionMode == "numerator"){
	    var denominatorDiv = textAsObj.find(".vK_"+this.fractionMode);
	    if (denominatorDiv.text().trim() != "") this.fractionMode = "denominator";
	    this.doChangeStates();
	}
	else returnVal = this.onAddFrac(existingText, "slash");
	if (!this.checkWithinLength(returnVal)) {
	    returnVal = existingText;
	    this.fractionMode = prevFracMode;
	}
	return returnVal;
    },
    onAddFrac:function(existingText, fracType){
	var pass = false;
	var newText = existingText;
	var textAsObj = $("<t>"+existingText+"</t>");
	if (textAsObj.text()) {
	    var innerDiv = textAsObj;
	    while (this.checkIfLastIsElement(innerDiv) && innerDiv.children().length)
		innerDiv = innerDiv.children().eq(innerDiv.children().length-1);
	    var innerText = innerDiv.html();
	    if (innerText) {
		var splitVal = this.splitValue(innerText);
		if(!innerDiv.hasClass("vK_fractionSet") && innerDiv.parents(".vK_fractionSet").length <= 0){
		    if (splitVal[1] && fracType == "space") {
			this.fractionMode = "numerator";
			innerDiv.html(splitVal[0] + "<span class=\"vK_fractionSet\">"+splitVal[1]+"<span class=\"vK_fraction\"><span class=\"vK_numerator\"></span><span class=\"vK_denominator\"></span></span></span>");
			newText = textAsObj.html();
			pass = true;
			this.doChangeStates();
		    }
		    else if (splitVal[1] && fracType == "slash") {
			this.fractionMode = "denominator";
			innerDiv.html(splitVal[0] + "<span class=\"vK_fractionSet vK_noMixed\"><span class=\"vK_fraction\"><span class=\"vK_numerator\">"+splitVal[1]+"</span><span class=\"vK_denominator\"></span></span></span>");
			newText = textAsObj.html();
			pass = true;
			this.doChangeStates();
		    }
		}
		else if (fracType == "slash" && this.fractionMode == "numerator") {
		    this.fractionMode = "denominator";
		    this.doChangeStates();
		}
	    }
	}
	if (pass) return newText;
	else return existingText;
    },
    doChangeStates:function(){
	this.isSup = false;
	this.isSub = false;
	this.checkBtnState();
    },
    splitValue:function(text){
	var splitVal = [];
	var lastNum = text.match(/\d+$/g);
	if (!lastNum) splitVal = [text, ""];
	else if(lastNum[0].length) splitVal = [text.substr(0, text.length-lastNum[0].length), lastNum[0]];
	return splitVal;
    },
    checkIfLastIsElement:function(elem){
	var htmlText = elem.html();
	return htmlText.substr(htmlText.length-1,1) == ">";
    },
    textLength:function(htmlText){
	var textElem = $("<span>"+htmlText+"</span>");
	var textLen = textElem.text().replace(/\\s|-|(\u2013)|\./g, "").length;
	textElem.remove();
	return textLen;
    },
    getText:function(){
	var text = "";
	if(this.CurrentFocus){
	    var isDiv = this.CurrentFocus.tagName.toLowerCase() == "div";
	    text = isDiv ? $(this.CurrentFocus).html() : this.CurrentFocus.value;
	}
	return text;
    },
    parseMath:function(){
	try{
	    M.parseMath(this.CurrentFocus);
	}
	catch(e){ }
    },
    storeDownPos:function(event, vKey){
	if (this.checkBeforeFocus) {
	    this.mouseDownPos = this.touchCoordsFromEvent(event, true);
	}
    },
    tryToFocus:function(event, vKey){
	var isStableFocus = false;
	if (!this.checkBeforeFocus) isStableFocus = true;
	else {
	    try{
		var mouseUpPos = this.touchCoordsFromEvent(event, false);
		if (this.mouseDownPos.length == 1 && mouseUpPos.length == 1) {
		    var xStatic = Math.abs(this.mouseDownPos[0].x - mouseUpPos[0].x) < this.moveSensitivity;
		    var yStatic = Math.abs(this.mouseDownPos[0].y - mouseUpPos[0].y) < this.moveSensitivity;
		    isStableFocus = xStatic && yStatic;
		}
	    }
	    catch(e){ }
	}
	if (isStableFocus) this.onFocus(event, vKey);
    },
    showPad:function(){
	this.KeyPad.show();
	this.KeyPadHolder.show();
    },
    isCoveringTextBoxes:function(currentFocus){
	var isCovering = false;
	var textboxes = $(this.selector);
	var keypadOffset = this.KeyPadHolder.offset();
	var keypadW = this.KeyPad.outerWidth();
	var keypadH = this.KeyPad.outerHeight();
	keypadOffset.top += this.KeyPad.position().top;
	for (var t=0; t<textboxes.length; t++) {
	    var textbox = textboxes.eq(t);
	    var textboxOffset = $(currentFocus).offset();
	    var textboxW = $(currentFocus).outerWidth();
	    var textboxH = $(currentFocus).outerHeight();
	    var _xVar = textboxOffset.left-keypadOffset.left;
	    var _yVar = textboxOffset.top-keypadOffset.top;
	    var xVar = Math.abs(_xVar);
	    var yVar = Math.abs(_yVar);
	    var isYOver, isXOver;
	    if (_xVar > 0) isXOver = xVar < keypadW;
	    else isXOver = xVar < textboxW;
	    if (_yVar > 0) isYOver = yVar < keypadH;
	    else isYOver = yVar < textboxH;
	    isCovering = isXOver && isYOver;
	    if (isCovering) break;
	}
	return isCovering;
    },
    onFocus:function(event, vKey) {
	event.preventDefault();
	$(".keypadHolder").css("display","block");
	if (this.isFocusOnClick) {
	    
	    var vK = vKey.getSelf();
	    var currentFocus = vK.getCurrentFocus(event.target);
	    if (currentFocus != vK.CurrentFocus) {
		var isVisible = vK.KeyPadHolder.css("display") != "none";
		if (vK.CurrentFocus) vK.hidePad();
		vK.CurrentFocus = null;
		if (this.disabledTextboxes.indexOf(currentFocus) < 0) {
		    vK.CurrentFocus = currentFocus;
		    if (vK.CurrentFocus) {
			vK.loadBtnState();
			var textboxType = vK.CurrentFocus.tagName.toString().toLowerCase();
			var text="";
			this.showPad();
			if (!isVisible && this.isAutoAdjustPosition)
			    if(this.isCoveringTextBoxes(currentFocus)) this.KeyPadHolder.css({"left":this.initPos.x+"px", "top":this.initPos.y+"px"});
			if(this.isMultiline){
			    if (textboxType == "div"){
				this.isMultilineBox = true;
				this.isTaggableBox = true;
			    }
			    else if (textboxType == "textarea"){
				this.isMultilineBox = true;
				this.isTaggableBox = false;
			    }
			    else if (textboxType == "input"){
				this.isMultilineBox = false;
				this.isTaggableBox = false;
			    }
			}
			else{
			    this.isMultilineBox = false;
			    if (textboxType == "div") this.isTaggableBox = true;
			    else this.isTaggableBox = false;
			}
			if (this.isForceTaggable) this.isTaggableBox = this.isForceTaggable;
			if (textboxType == "div") text = vK.CurrentFocus.innerHTML;
			else if (textboxType == "input" || textboxType == "textarea") text = vK.CurrentFocus.value;
			if(vK.autoClear || (vK.clearZero && text.replace(/0/g,"").replace(/\s/g, "")=="")) {
			    if (textboxType == "div") vK.CurrentFocus.innerHTML = "";
			    else if (textboxType == "textarea") vK.CurrentFocus.value = "";
			    else if (textboxType == "input") vK.CurrentFocus.value = "";
			}
			
			if (!this.customTxtBoxCol) this.textBoxOrgBg = $(vK.CurrentFocus).css("background");
			$(vK.CurrentFocus).css({"background": this.onFocusTxtBoxBg});
			this.checkBtnState();
			this.onTextBoxFocus(event,vK);
		    }
		}
	    }
	}
    },
    getCurrentFocus:function(target){
	var focusElement = null;
	if (this.selector.indexOf("#") == 0) {
	    if ($(target).attr("id") == this.selector.substr(1)) focusElement = $(target)[0];
	    else focusElement = $(target).parents(this.selector)[0];
	}
	else if (this.selector.indexOf(".") == 0){
	    if ($(target).hasClass(this.selector.substr(1))) focusElement = $(target)[0];
	    else focusElement = $(target).parents(this.selector)[0];
	}
	return focusElement;
    },
    hidePad:function(){
	var vK = this.getSelf();
	var currentFocus = vK.CurrentFocus;
	vK.KeyPad.hide();
        vK.KeyPadHolder.hide();
	this.blur();
	this.onKeypadHide(currentFocus);
	$(".keypadHolder").css("display","none");
    },
    blur:function(){
	var vK = this.getSelf();
	if (this.textBoxOrgBg && vK.CurrentFocus)
	    if(this.applyTxtBoxBG) $(vK.CurrentFocus).css({"background": this.textBoxOrgBg});
	    else $(vK.CurrentFocus).css({"background": ""});
	if (vK.CurrentFocus){
	    vK.triggerChange(vK.CurrentFocus);
	    if (this.isDefaultVal){
		var isDiv = vK.CurrentFocus.tagName.toLowerCase() == "div";
		var existingText = isDiv ? $(vK.CurrentFocus).html() : vK.CurrentFocus.value;
		var txtAsNum = 0;
		try{
		    var minusToHyphReg = new RegExp(this.minusSymbol, "g");
		    existingText = existingText.replace(minusToHyphReg, "-").replace(/\s/g, "");
		    txtAsNum = parseFloat(existingText);
		}
		catch(e) { }
		if (!txtAsNum) {
		    if (isDiv) {
			$(vK.CurrentFocus).html(this.defaultValue);
			this.parseMath();
		    }
		    else vK.CurrentFocus.value = this.defaultValue;
		}
	    }
	}
	vK.CurrentFocus = null;
    },
    touchCoordsFromEvent:function(event, isDown){
        var touches = [];
        if (isDown) {
            if (this.isTouchDevice())
                for (var tInd = 0; tInd < event.originalEvent.touches.length; tInd++)
                    touches[tInd] = { x: event.originalEvent.touches[tInd].pageX, y: event.originalEvent.touches[tInd].pageY };
            else touches[0] = { x: event.pageX, y: event.pageY };
        }
        else{
            if (this.isTouchDevice())
                for (var tInd = 0; tInd < event.originalEvent.changedTouches.length; tInd++)
                    touches[tInd] = { x: event.originalEvent.changedTouches[tInd].pageX, y: event.originalEvent.changedTouches[tInd].pageY };
            else touches[0] = { x: event.pageX, y: event.pageY };
        }
        return touches;
    },
    isTouchDevice: function () {
        return "ontouchstart" in document;
    },
    onTextBoxFocus:function(){},
    onKeypadHide:function(){},
    triggerChange:function(){},
    getSelf:function(){
        return this;
    },
    onTextChanged:function(){ }
};


sciencepst.js
=============



var ques = new Array();
var totalQues = 0;
var currQues = 0;
var slideWidth;
var audinst = new Audio('audio/instruct.mp3');
var audinst1 = new Audio('audio/instruct.mp3');
var audinst2 = new Audio('audio/instruct.mp3');
/* Touch Move Code*/
function preventBehavior(e) 
{ 
    e.preventDefault(); 
};
//document.addEventListener("touchmove", preventBehavior, false);
/* Touch Move Code*/
var slider;
var slider1;
var slider2;
var slider3;

/*function resizePage()
{
    if(window.orientation=="0" || window.orientation=="180")
    {
        $('.defaultCountdown').css({'position':'absolute', 'bottom':'0px', 'margin-left':'235px'});
        $('.testpageWidth').css({'width':'70%'});
        $('.defaultSubmit').css({'width':'120px', 'margin-left':'20px'});
        slideWidth = 925;
        document.addEventListener("touchmove", preventBehavior, false);
    }
    else 
    {
        $('.testPageGreyAnswerBoxWidth').css({'width':'98%'});
        $('.defaultCountdown').css({'position':'absolute', 'bottom':'0px', 'margin-left':'360px'});
        $('.testpageWidth').css({'width':'65%'});
        $('.defaultSubmit').css({'width':'120px', 'margin-left':'50px'});
        slideWidth = 670;
        document.removeEventListener("touchmove", preventBehavior, false);
    }
}*/
/* Side Swipe Feature */
var x1=0;
var x2=0;
var y1=0;
var y2=0;
var allowSwipe=false;
var lessonsArray = new Array();
$(document).ready(function()
                  {
                  
                  $.mobile.page.prototype.options.domCache = true;
                  
                  for(var les=0; les<20; les++){
                    lessonsArray[les] = $("#lesson"+(les+1)+"_p1"); 
                  }
                  
                  
               window.location.href = "#index";
                  
				  slider = new Swipe(document.getElementById('slider'));
                  slider1 = new Swipe(document.getElementById('slider1'));
                  slider2 = new Swipe(document.getElementById('slider2'));
                  slider3 = new Swipe(document.getElementById('slider3'));
                  
                  newloaded();
                  
                  $('.blueButtonOpacityRedu1').css("opacity","0.5");
                  
                                   
                  $(document).scroll(function (){
                                     document.removeEventListener("touchend", onTouchEndFunc);
                                     });
                  
                  document.addEventListener("touchstart", function(event){
                                          if(event.touches.length == 1){
                                          var touch = event.changedTouches[0];
                                          x1 = touch.clientX;
                                          y1 = touch.clientY;
                                          x2 = 0;
                                          y2 = 0;
                                          }
                });
                  
                  document.addEventListener("touchmove", function(event){
                                          if(event.touches.length == 1){ 
                                          var touch = event.changedTouches[0];
                                          x2 = touch.clientX;
                                          y2 = touch.clientY;
                                          document.addEventListener("touchend", onTouchEndFunc);
                                          }
                });
                  
                  function onTouchEndFunc (event){
                  
                  
                                          
                  if(y2-y1<25 && y2-y1>-25)
                  {
                    if(x1-x2>100 && x2>0)
                    {
                      /*  var nextpage = $('div.ui-page:visible').next('div[data-role="page"]');
                  
                        var oId = window.location.href;
                        oId = oId.substr(oId.indexOf("#"));
                  
                        if(oId.indexOf('lesson')!=-1 && oId.indexOf('lesson33')==-1)
                        {
                            $(".showYellowBG").removeClass('yellowBg');
                            $(".showGreenBG").removeClass('greenBg');
                            $(".showRedBG").removeClass('redBg');
                            $(".showBlueBG").removeClass('blueBg');
                  
                        
                            $.mobile.changePage(nextpage, {transition:'slide', reverse:false});
                        }*/
                  return;
                    }
                  
                    if(x1-x2<-100 && x2>0)
                    {
                       /* var prevpage = $('div.ui-page:visible').prev('div[data-role="page"]');
                        if($(prevpage).attr('id') =="about")
                        {
                            prevpage = $("#index");
                        }
                        var oId = window.location.href;
                        oId = oId.substr(oId.indexOf("#"));
                        
                        if(oId.indexOf('lesson')!=-1)
                        {
                            $(".showYellowBG").removeClass('yellowBg');
                            $(".showGreenBG").removeClass('greenBg');
                            $(".showRedBG").removeClass('redBg');
                            $(".showBlueBG").removeClass('blueBg');

                            $.mobile.changePage(prevpage, {transition:'slide', reverse:true});
                  
                            
                        }*/
                  return;
                    }
                    
                    
				  }
                  x1=0;
                  x2=0;
                  y1=0;
                  y2=0;
                }
		DragNDrop();
		
		loadKeypad();
		loadCalculator();	
		
});
/* Side Swipe Feature */


function adjustTimer(vari){
    /*
    var oId = window.location.href;
    oId = oId.substr(oId.indexOf("#"));
    
    id = oId;
    var quesId = $(id).find('form')[vari-1].name;
    var quesNum = parseInt(quesId.substr(1));
    
    classes = $('.defaultCountdown:eq('+(quesNum-1)+')').attr('class');
    if(classes.indexOf("expCountdown") == -1){
        $('.defaultCountdown:eq('+(quesNum-1)+')').countdown({until: 60, compact:true, format: 'S'});
    }
    else {	
        if($('#correctAns'+quesNum).is(':visible')==false){
        var divContent = $('.defaultCountdown:eq('+(quesNum-1)+')').html();
        $('.defaultCountdown:eq('+(quesNum-1)+')').removeClass('expCountdown');
            if(divContent==""){
                divContent = 60;
            }
        $('.defaultCountdown:eq('+(quesNum-1)+')').countdown({until:divContent, compact:true, format: 'S'});
        }
    }
    */
}


$('.ui-page').live('pageshow', function (){
    
                  
                    var oId = "#"+$.mobile.activePage.attr('id');
        

                    if(oId == "#index"){
                        //$('#index').live('swipeleft', indexLeftEvent);
                    }
                    else {
                       // $('#index').die('swipeleft', indexLeftEvent);
                    }
    
                   if(oId == "#test1_1"){

					slider.setup();
                   $('#test1_1').live('swipeleft', next1);
                   $('#test1_1').live('swiperight', back1);
                   
                   if(scroll1){
                   scroll1.destroy();
                   scroll2.destroy();
                   scroll3.destroy();
                   scroll4.destroy();
                   }
                   scroll1 = new iScroll("scroll1", { scrollbarClass: 'myScrollbar' });
                   
                       adjustTimer(i);
                       
                   }
                   else {
                   $('#test1_1').die('swipeleft', next1);
                   $('#test1_1').die('swiperight', back1);
                   }
                   
                   
                   if(oId == "#test2_1"){
					slider1.setup();
                   $('#test2_1').live('swipeleft', next2);
                   $('#test2_1').live('swiperight', back2);
                   
                   if(scroll1){
                   scroll1.destroy();
                   scroll2.destroy();
                   scroll3.destroy();
                   scroll4.destroy();
                   }
                   scroll1 = new iScroll("scroll2", { scrollbarClass: 'myScrollbar' });
                   
                       adjustTimer(j);
                       
                   }
                   else {
                   $('#test2_1').die('swipeleft', next2);
                   $('#test2_1').die('swiperight', back2);
                   }
                   
                   
                   if(oId == "#test3_1"){
					slider2.setup();
                   $('#test3_1').live('swipeleft', next3);
                   $('#test3_1').live('swiperight', back3);
                   
                   if(scroll1){
                   scroll1.destroy();
                   scroll2.destroy();
                   scroll3.destroy();
                   scroll4.destroy();
                   }
                   scroll1 = new iScroll("scroll3", { scrollbarClass: 'myScrollbar' });
                       adjustTimer(k);
                       
                   }
                   else {
                   $('#test3_1').die('swipeleft', next3);
                   $('#test3_1').die('swiperight', back3);
                   }
                   
                   
                   if(oId == "#test4_1"){
					slider3.setup();
                   $('#test4_1').live('swipeleft', next4);
                   $('#test4_1').live('swiperight', back4);
                   
                   if(scroll1){
                   scroll1.destroy();
                   scroll2.destroy();
                   scroll3.destroy();
                   scroll4.destroy();
                   }
                   scroll1 = new iScroll("scroll4", { scrollbarClass: 'myScrollbar' });
                   adjustTimer(m);
                   }
                   else {
                   $('#test4_1').die('swipeleft', next4);
                   $('#test4_1').die('swiperight', back4);
                   }
                   
                    if(oId == "#result1"){
                        $('#result1').live('swipeleft', result1LeftEvent);
                        $('#result1').live('swiperight', result1RightEvent);  
                    }
                    else {
                        $('#result1').die('swipeleft', result1LeftEvent);
                        $('#result1').die('swiperight', result1RightEvent);
                    }
    
    if(oId == "#result2"){
        $('#result2').live('swipeleft', result2LeftEvent);
        $('#result2').live('swiperight', result2RightEvent);  
    }
    else {
        $('#result2').die('swipeleft', result2LeftEvent);
        $('#result2').die('swiperight', result2RightEvent);
    }
    if(oId == "#result3"){
        $('#result3').live('swipeleft', result3LeftEvent);
        $('#result3').live('swiperight', result3RightEvent);  
    }
    else {
        $('#result3').die('swipeleft', result3LeftEvent);
        $('#result3').die('swiperight', result3RightEvent);
    }
    if(oId == "#result4"){
        $('#result4').live('swipeleft', result4LeftEvent);
        $('#result4').live('swiperight', result4RightEvent);  
    }
    else {
        $('#result4').die('swipeleft', result4LeftEvent);
        $('#result4').die('swiperight', result4RightEvent);
    }
                    
                    
                  for(var les=0; les<9; les++){
                   try
                   {
                    lessonsArray[les].die('swipeleft', lessonNextEvent);
                    lessonsArray[les].die('swiperight', lessonBackEvent);
                   }
                   catch(e){
                   
                   }
                   }
                   
                   if(oId.indexOf("lesson")!=-1){
                        var lessonIndex = parseInt(oId.substr(oId.indexOf("n")+1, (oId.indexOf("_") - oId.indexOf("n") - 1)));
                                    
                        lessonsArray[lessonIndex - 1].live('swipeleft', lessonNextEvent);
                        lessonsArray[lessonIndex - 1].live('swiperight', lessonBackEvent);
                   }

});


function next1(event, ui){
                   
                   if(y2-y1<25 && y2-y1>-25)
                   {
                   nextpage="#result1";
                   chkquestime(nextpage);
                   event.preventDefault();
                   }
}
function next2(event, ui){
if(y2-y1<25 && y2-y1>-25)
                  {
nextpage="#result2";
chkquestime1(nextpage);
event.preventDefault();
}
}

function next3(event, ui){
if(y2-y1<25 && y2-y1>-25)
                  {
nextpage="#result3";
chkquestime2(nextpage);
event.preventDefault();
}
}
function next4(event, ui){
if(y2-y1<25 && y2-y1>-25)
                  {
nextpage="#result4";
chkquestime3(nextpage);
event.preventDefault();
}
}

function back1(event, ui){
if(y2-y1<25 && y2-y1>-25)
                  {
prevpage="#index";
goBack(prevpage);
event.preventDefault();

}
}

function back2(event, ui){
if(y2-y1<25 && y2-y1>-25)
                  {
prevpage="#result1";
goBack1(prevpage);
event.preventDefault();
}
}

function back3(event, ui){
if(y2-y1<25 && y2-y1>-25)
                  {
prevpage="#result2";
goBack2(prevpage);
event.preventDefault();
}
}

function back4(event, ui){
if(y2-y1<25 && y2-y1>-25)
                  {
prevpage="#result3";
goBack3(prevpage);
event.preventDefault();
}
}

function indexLeftEvent(event, ui){
 if(y2-y1<25 && y2-y1>-25)
                  {                  
                   $.mobile.changePage("#test1_1", {transition:'slide', reverse:false});
                   
                      adjustTimer(i);
                   }
                   }

function result1LeftEvent(event, ui){
                   if(y2-y1<25 && y2-y1>-25)
                  {
                   $.mobile.changePage("#test2_1", {transition:'slide', reverse:false});
                   
                   adjustTimer(j);
                   }
}
function result1RightEvent(event, ui){
                   if(y2-y1<25 && y2-y1>-25)
                  {
                   $.mobile.changePage("#test1_1", {transition:'slide', reverse:true});
                   
                   adjustTimer(i);
                   }
}
function result2LeftEvent(event, ui){
                   if(y2-y1<25 && y2-y1>-25)
                  {
                   $.mobile.changePage("#test3_1", {transition:'slide', reverse:false});
                   
                   adjustTimer(k);
                   }
                   }
function result2RightEvent(event, ui){
                   if(y2-y1<25 && y2-y1>-25)
                  {
                   $.mobile.changePage("#test2_1", {transition:'slide', reverse:true});
                   
                   adjustTimer(j);
                   }
                   }
function result3LeftEvent(event, ui){
                   if(y2-y1<25 && y2-y1>-25)
                  {
                   $.mobile.changePage("#test4_1", {transition:'slide', reverse:false});
                   adjustTimer(m);
                   }
                   }
function result3RightEvent(event, ui){
                   if(y2-y1<25 && y2-y1>-25)
                  {
                   $.mobile.changePage("#test3_1", {transition:'slide', reverse:true});
                   
                   adjustTimer(k);
                   }
                   }
function result4LeftEvent(event, ui){
                   if(y2-y1<25 && y2-y1>-25)
                  {
                   $.mobile.changePage("#lesson1_p1", {transition:'slide', reverse:false});
                   
                   
                   }
                   }
function result4RightEvent(event, ui){
                   if(y2-y1<25 && y2-y1>-25)
                  {
                   $.mobile.changePage("#test4_1", {transition:'slide', reverse:true});
                   
                   adjustTimer(m);
                   }
                   }

function lessonNextEvent(event, ui){
    
    if(y2-y1<25 && y2-y1>-25)
                  {
                             var oId = $.mobile.activePage.attr('id');
                              if(oId.indexOf('lesson9')==-1)
                      {
                   var nextpage = $('div.ui-page:visible').next('div[data-role="page"]');
                                $.mobile.changePage(nextpage, {transition:'slide', reverse:false});
                          
                              }
                              }
}

function lessonBackEvent(event, ui){
if(y2-y1<25 && y2-y1>-25)
                  {
           
                   
                   var prevpage = $('div.ui-page:visible').prev('div[data-role="page"]');
                   $.mobile.changePage(prevpage, {transition:'slide', reverse:true});
                   }
                   }

/* Validate New Answer */
var ans=1;
function validateNewAns(frm, ansDiv){
    var Uans = valButton(frm.r1);
    var Cans = ansButton(frm.r1);
    
    var qId = frm.name;
    
	if($(frm).find('fieldset').find('input')[Uans].value == "answer")
	{
		ques[parseFloat(frm.name.substr(1))-1] = "Correct";
	}
	else {
		$(frm).find('fieldset').find('label').eq(Uans).find('.ui-btn-text').find('.wrong').remove();
		var choiceHTML = $(frm).find('fieldset').find('label').eq(Uans).find('.ui-btn-text').html()+'<div style="display:inline;" class="wrong"><nobr>&nbsp;<img src="images/wrong.png" align="absmiddle" height="20" width="21"/></nobr></div>';
		$(frm).find('fieldset').find('label').eq(Uans).find('.ui-btn-text').html(choiceHTML);
		
		ques[parseFloat(frm.name.substr(1))-1] = "Wrong";
	}
	
	
	$(frm).find('fieldset').find('input').attr('disabled','');
	$(frm).find('input').attr('disabled','');
    
	$(".ans"+parseFloat(frm.name.substr(1))).find('.ui-btn-text').find('.tick').remove();
    var choiceHTML = $(".ans"+parseFloat(frm.name.substr(1))).find('.ui-btn-text').html()+'<div style="display:inline" class="tick" ><nobr>&nbsp;<img src="images/tick.png" align="absmiddle" width="21" height="20"/></nobr></div>';
    
	$(".ans"+parseFloat(frm.name.substr(1))).find('.ui-btn-text').html(choiceHTML);
	$('#'+ansDiv).fadeIn();
    
    ans++;
	//if(ans==3){
    var oId = window.location.href;
	
    if(oId.indexOf("#")!=-1){
        oId = oId.substr(oId.indexOf("#"));
        if(oId!="#index"){
            if(oId.indexOf("test1")!=-1){
            var quesId1 = $(oId).find('form')[i-1].name;
            }
            else if(oId.indexOf("test2")!=-1){
                var quesId1 = $(oId).find('form')[j-1].name;
            }
            else if(oId.indexOf("test3")!=-1){
                var quesId1 = $(oId).find('form')[k-1].name;
            }
            else if(oId.indexOf("test4")!=-1){
                var quesId1 = $(oId).find('form')[m-1].name;
            }
            var quesNum1 = parseInt(quesId1.substr(1));
            var divContent = $('.defaultCountdown:eq('+(quesNum1-1)+')').html();
            $('.defaultCountdown:eq('+(quesNum1-1)+')').countdown('destroy');
            $('.defaultCountdown:eq('+(quesNum1-1)+')').html(divContent);
            $('.defaultCountdown:eq('+(quesNum1-1)+')').addClass('expCountdown');
			
			//Hide Timer
			$('.defaultCountdown:eq('+(quesNum1-1)+')').hide();
			$('.defaultSubmit:eq('+(quesNum1-1)+')').hide();
            
            /*setTimeout(function(){
                       $('#'+ansDiv).hide();
                       },1000);
            
            setTimeout(function(){
                       $('#'+ansDiv).show();
                       },1200);*/
        }
        
    }
	//}
    var totCor=0;
    for(var quiz=0; quiz<14; quiz++){
        
        if(ques[quiz]=="Correct"){
            totCor++;
            $('#q'+(quiz+1)+'result').find('img').eq(0).show();
            $('#q'+(quiz+1)+'result').find('img').eq(1).hide();
        }
        else if(ques[quiz]=="Wrong"){
            
            $('#q'+(quiz+1)+'result').find('img').eq(0).hide();
            $('#q'+(quiz+1)+'result').find('img').eq(1).show();
        }
        else {
            $('#q'+(quiz+1)+'result').find('img').eq(0).hide();
            $('#q'+(quiz+1)+'result').find('img').eq(1).show();
        }
        $('#test1TotAns').text("You answered "+totCor+" out of 14 questions correctly."
                               )
    }
    var totCor1=0;
    for(var quiz=14; quiz<28; quiz++){
        if(ques[quiz]=="Correct"){
            totCor1++;
            $('#q'+(quiz+1)+'result').find('img').eq(0).show();
            $('#q'+(quiz+1)+'result').find('img').eq(1).hide();
        }
        else if(ques[quiz]=="Wrong"){
            
            $('#q'+(quiz+1)+'result').find('img').eq(0).hide();
            $('#q'+(quiz+1)+'result').find('img').eq(1).show();
        }
        else {
            $('#q'+(quiz+1)+'result').find('img').eq(0).hide();
            $('#q'+(quiz+1)+'result').find('img').eq(1).show();
        }
        $('#test2TotAns').text("You answered "+totCor1+" out of 14 questions correctly.")
    }
    var totCor2=0;
    for(var quiz=28; quiz<42; quiz++){
        if(ques[quiz]=="Correct"){
            totCor2++;
            $('#q'+(quiz+1)+'result').find('img').eq(0).show();
            $('#q'+(quiz+1)+'result').find('img').eq(1).hide();
        }
        else if(ques[quiz]=="Wrong"){
            
            $('#q'+(quiz+1)+'result').find('img').eq(0).hide();
            $('#q'+(quiz+1)+'result').find('img').eq(1).show();
        }
        else {
            $('#q'+(quiz+1)+'result').find('img').eq(0).hide();
            $('#q'+(quiz+1)+'result').find('img').eq(1).show();
        }
        $('#test3TotAns').text("You answered "+totCor2+" out of 14 questions correctly.")
    }
    var totCor3=0;
    for(var quiz=42; quiz<56; quiz++){
        if(ques[quiz]=="Correct"){
            totCor3++;
            $('#q'+(quiz+1)+'result').find('img').eq(0).show();
            $('#q'+(quiz+1)+'result').find('img').eq(1).hide();
        }
        else if(ques[quiz]=="Wrong"){
            
            $('#q'+(quiz+1)+'result').find('img').eq(0).hide();
            $('#q'+(quiz+1)+'result').find('img').eq(1).show();
        }
        else {
            $('#q'+(quiz+1)+'result').find('img').eq(0).hide();
            $('#q'+(quiz+1)+'result').find('img').eq(1).show();
        }
        $('#test4TotAns').text("You answered "+totCor3+" out of 14 questions correctly.")
    }
}

/* Validate New Answer */

function valButton(btn) 
{
    for (var j=0; j <= btn.length-1; j++) 
    {
        if (btn[j].checked) 
        {
            return j;
        }
    }
}

function ansButton(btn) 
{
    for (var j=0; j <= btn.length-1; j++) 
    {
        if (btn[j].value=="answer") 
        {
            return btn[j];
        }
    }
}
/* Answer Validation */

/* Tick Image Code */
function showAns(frm)
{
    $(".ans"+parseFloat(frm.name.substr(1))).find('.ui-btn-text').find('.tick').remove();
    $(".ans"+parseFloat(frm.name.substr(1))).find('.ui-btn-text').append('<nobr><span class="tick">&nbsp;<img src="images/tick.png" align="absmiddle"/></span></nobr>');
    // $(frm).find('.ans1').find('.ui-btn-text').find('.tick').remove();
    // $(frm).find('.ans1').find('.ui-btn-text').append('<span class="tick" style="float:right;"><img src="images/tick.png"/></span>');
}
/* Tick Image Code */

/* Timer Code */
function chkTimer(id){
$("#calculatorsource").hide();
imgUp();
if(id.indexOf("#lesson")!=-1)
{
$(".showYellowBG").removeClass('yellowBg');
$(".showGreenBG").removeClass('greenBg');
$(".showRedBG").removeClass('redBg');
$(".showBlueBG").removeClass('blueBg');
}
var oId = window.location.href;

if(oId.indexOf("#")!=-1){
	oId = oId.substr(oId.indexOf("#"));
	if(oId!="#index" && oId!="#about" && oId.indexOf("#lesson")==-1 && oId.indexOf("#result")==-1){
	
		var quizID;
        if (oId == "#test1_1"){
           quizID = $(oId).find('form')[i-1].name;
        }
        else if	(oId == "#test2_1") {
            quizID = $(oId).find('form')[j-1].name;
        }
        else if(oId == "#test3_1"){
            quizID = $(oId).find('form')[k-1].name;
        }
        else if(oId == "#test4_1") {
            quizID = $(oId).find('form')[m-1].name;
        }
        
		var quesNum1 = parseInt(quizID.substr(1));
		var divContent = $('.defaultCountdown:eq('+(quesNum1-1)+')').html();
		$('.defaultCountdown:eq('+(quesNum1-1)+')').countdown('destroy');
		$('.defaultCountdown:eq('+(quesNum1-1)+')').html(divContent);
		$('.defaultCountdown:eq('+(quesNum1-1)+')').addClass('expCountdown');
		
	}
	
}
    if(y2==0){
    y2=y1;
    }
    if(oId.indexOf("index")!=-1){
       if(y2-y1 < 10 && y2-y1 > -10){
		   
           $.mobile.changePage(id, {transition:'slide', reverse:false});
        }
    }
    else {
            $.mobile.changePage(id, {transition:'slide', reverse:true});
    }
    loaded();   

}

/* Timer Code */
var i = 1;
function chkquestime(nextpage)
{
	
	$("#calculatorsource").hide();
    if(i<5)
	{
	    $("#simple_Q").text(i+1);
	    if (i==4) {
		$("#sub_1").show();
			$("#next1").hide();
			
	    }
	    
		document.getElementById('que_number').innerHTML=i+1;
		$('#Back_But').css('visibility','visible');
		var oId = window.location.href;
			oId = oId.substr(oId.indexOf("#"));
				
				var quesId1 = $(oId).find('form')[i-1].name;
			   
				var quesNum1 = parseInt(quesId1.substr(1));
				
				var divContent = $('.defaultCountdown:eq('+(quesNum1-1)+')').html(); 
				$('.defaultCountdown:eq('+(quesNum1-1)+')').countdown('destroy');
				$('.defaultCountdown:eq('+(quesNum1-1)+')').html(divContent);
				$('.defaultCountdown:eq('+(quesNum1-1)+')').addClass('expCountdown');

        //$(oId).find('form').eq(i-1).find('fieldset').find('input').attr('disabled','');
        //$(oId).find('form').eq(i-1).find('input').attr('disabled','');
        $('#slider > ul').find('li').eq(slider.index + 1).find('div').eq(0).show();
        //$('#slider > ul').find('li').eq(slider.index + 1).find('div').eq(0).css({'display':'block'});
        slider.next();
        
        $('#slider > ul').find('li').eq(slider.index - 1).find('div').eq(0).hide();
        
		i++;
		
        return true;
	}
	else {
        var oId = window.location.href;
        oId = oId.substr(oId.indexOf("#"));
        
        var quesId1 = $(oId).find('form')[i-1].name;
        
        var quesNum1 = parseInt(quesId1.substr(1));
        
        var divContent = $('.defaultCountdown:eq('+(quesNum1-1)+')').html();
        $('.defaultCountdown:eq('+(quesNum1-1)+')').countdown('destroy');
        $('.defaultCountdown:eq('+(quesNum1-1)+')').html(divContent);
        $('.defaultCountdown:eq('+(quesNum1-1)+')').addClass('expCountdown');
        
		$.mobile.changePage("#result1", {transition:'slide', reverse:false});
        return true;
	}
	
}

function goBack(prevpage)
{
	$("#calculatorsource").hide();
	$("#next1").show();
	if(i<=5 && i>1)
	{
	   $("#simple_Q").text(i-1);
	   $(".submit_icon,#feed").hide();

	document.getElementById('que_number').innerHTML=i-1;
	if(i==2)
	{
			$('#Back_But').css('visibility','hidden');
	//$('#answerchoice'+(i-1)).hide();
	}
	
	
	var oId = window.location.href;
        oId = oId.substr(oId.indexOf("#"));
            
            var quesId1 = $(oId).find('form')[i-1].name;
            var quesNum1 = parseInt(quesId1.substr(1));
            var divContent = $('.defaultCountdown:eq('+(quesNum1-1)+')').html();
            $('.defaultCountdown:eq('+(quesNum1-1)+')').countdown('destroy');
            $('.defaultCountdown:eq('+(quesNum1-1)+')').html(divContent);
            $('.defaultCountdown:eq('+(quesNum1-1)+')').addClass('expCountdown');
        
        /*$(oId).find('form').eq(i-1).find('fieldset').find('input').attr('disabled','');
        $(oId).find('form').eq(i-1).find('input').attr('disabled','');*/
        
        $('#slider > ul').find('li').eq(slider.index - 1).find('div').eq(0).show();
        slider.prev();
        $('#slider > ul').find('li').eq(slider.index + 1).find('div').eq(0).hide();
        
    i--;

        return true;
}
    else {
    if(i==1){
        var oId = window.location.href;
        oId = oId.substr(oId.indexOf("#"));
        
        var quesId1 = $(oId).find('form')[i-1].name;
        var quesNum1 = parseInt(quesId1.substr(1));
        var divContent = $('.defaultCountdown:eq('+(quesNum1-1)+')').html();
        $('.defaultCountdown:eq('+(quesNum1-1)+')').countdown('destroy');
        $('.defaultCountdown:eq('+(quesNum1-1)+')').html(divContent);
        $('.defaultCountdown:eq('+(quesNum1-1)+')').addClass('expCountdown');
        
        
        $.mobile.changePage(prevpage, {transition:'slide', reverse:true});
        
        /*var prevID = $(prevpage).attr('id');
        var scrollerId = $('#'+prevID).find('.wrapper').attr('id');
        
        loaded(scrollerId);*/
   
    }
        return true;
    }
    
}

var j = 1;
function chkquestime1(nextpage)
{
    $("#calculatorsource").hide();
	if(j<5)
	{
	    $("#medium_Q").text(j+1);
	    if (j==4) {
		$("#sub_2").show();
		$("#next2").hide();
	    }
		
	document.getElementById('que_number1').innerHTML=j+1;
	$('#Back_But1').css('visibility','visible');
	
	
    var oId = window.location.href;
        oId = oId.substr(oId.indexOf("#"));
            
            var quesId1 = $(oId).find('form')[j-1].name;
           
			var quesNum1 = parseInt(quesId1.substr(1));
			
            var divContent = $('.defaultCountdown:eq('+(quesNum1-1)+')').html(); 
            $('.defaultCountdown:eq('+(quesNum1-1)+')').countdown('destroy');
            $('.defaultCountdown:eq('+(quesNum1-1)+')').html(divContent);
            $('.defaultCountdown:eq('+(quesNum1-1)+')').addClass('expCountdown');
    
        /*$(oId).find('form').eq(j-1).find('fieldset').find('input').attr('disabled','');
        $(oId).find('form').eq(j-1).find('input').attr('disabled','');*/
    
    $('#slider1 > ul').find('li').eq(slider1.index + 1).find('div').eq(0).show();
        slider1.next();
    $('#slider1 > ul').find('li').eq(slider1.index - 1).find('div').eq(0).hide();
	j++;
	
        return true;
	}
	else {
        var oId = window.location.href;
        oId = oId.substr(oId.indexOf("#"));
        
        var quesId1 = $(oId).find('form')[j-1].name;
        
        var quesNum1 = parseInt(quesId1.substr(1));
        
        var divContent = $('.defaultCountdown:eq('+(quesNum1-1)+')').html();
        $('.defaultCountdown:eq('+(quesNum1-1)+')').countdown('destroy');
        $('.defaultCountdown:eq('+(quesNum1-1)+')').html(divContent);
        $('.defaultCountdown:eq('+(quesNum1-1)+')').addClass('expCountdown');
        
		$.mobile.changePage("#result2", {transition:'slide', reverse:false});
        return true;
	}
}

function goBack1(prevpage)
{
	$("#calculatorsource").hide();
	$("#next2").show();
	if(j<=5 && j>1)
	{
	    $("#medium_Q").text(j-1);
	    $(".submit_icon,#feed2").hide();
		document.getElementById('que_number1').innerHTML=j-1;
	if(j==2)
	{
		$('#Back_But1').css('visibility','hidden');
	}
	
    var oId = window.location.href;
        oId = oId.substr(oId.indexOf("#"));
            
            var quesId1 = $(oId).find('form')[j-1].name;
            var quesNum1 = parseInt(quesId1.substr(1));
            var divContent = $('.defaultCountdown:eq('+(quesNum1-1)+')').html();
            $('.defaultCountdown:eq('+(quesNum1-1)+')').countdown('destroy');
            $('.defaultCountdown:eq('+(quesNum1-1)+')').html(divContent);
            $('.defaultCountdown:eq('+(quesNum1-1)+')').addClass('expCountdown');
        
		/*$(oId).find('form').eq(j-1).find('fieldset').find('input').attr('disabled','');
        $(oId).find('form').eq(j-1).find('input').attr('disabled','');*/
        
        $('#slider1 > ul').find('li').eq(slider1.index - 1).find('div').eq(0).show();
        slider1.prev();
        $('#slider1 > ul').find('li').eq(slider1.index + 1).find('div').eq(0).hide();
    j--;
	
        return true;
}
else {
    if(j==1){
        var oId = window.location.href;
        oId = oId.substr(oId.indexOf("#"));
        
        var quesId1 = $(oId).find('form')[j-1].name;
        var quesNum1 = parseInt(quesId1.substr(1));
        var divContent = $('.defaultCountdown:eq('+(quesNum1-1)+')').html();
        $('.defaultCountdown:eq('+(quesNum1-1)+')').countdown('destroy');
        $('.defaultCountdown:eq('+(quesNum1-1)+')').html(divContent);
        $('.defaultCountdown:eq('+(quesNum1-1)+')').addClass('expCountdown');
        $.mobile.changePage(prevpage, {transition:'slide', reverse:true});
        
        /*var prevID = $(prevpage).attr('id');
        var scrollerId = $('#'+prevID).find('.wrapper').attr('id');
        
        loaded(scrollerId);*/
        
        
    }
    return true;
    }
}

var k = 1;
function chkquestime2(nextpage)
{
	$("#calculatorsource").hide();
	audinst.pause();
	audinst.currentTime = 0;
	if(k<5)
	{
		$("#hard_Q").text(k+1);
		if (k==4) {
		$("#sub_3").show();
			$("#next3").hide();
	    }
		document.getElementById('que_number2').innerHTML=k+1;
		$('#Back_But2').css('visibility','visible');
		
		
		var oId = window.location.href;
			oId = oId.substr(oId.indexOf("#"));
				
				var quesId1 = $(oId).find('form')[k-1].name;
			   
				var quesNum1 = parseInt(quesId1.substr(1));
				
				var divContent = $('.defaultCountdown:eq('+(quesNum1-1)+')').html(); 
				$('.defaultCountdown:eq('+(quesNum1-1)+')').countdown('destroy');
				$('.defaultCountdown:eq('+(quesNum1-1)+')').html(divContent);
				$('.defaultCountdown:eq('+(quesNum1-1)+')').addClass('expCountdown');

		/*$(oId).find('form').eq(k-1).find('fieldset').find('input').attr('disabled','');
        $(oId).find('form').eq(k-1).find('input').attr('disabled','');*/
        
        $('#slider2 > ul').find('li').eq(slider2.index + 1).find('div').eq(0).show();
        slider2.next();
        $('#slider2 > ul').find('li').eq(slider2.index - 1).find('div').eq(0).hide();
		k++;
		
        return true;
	}
	else {
        var oId = window.location.href;
        oId = oId.substr(oId.indexOf("#"));
        
        var quesId1 = $(oId).find('form')[k-1].name;
        
        var quesNum1 = parseInt(quesId1.substr(1));
        
        var divContent = $('.defaultCountdown:eq('+(quesNum1-1)+')').html();
        $('.defaultCountdown:eq('+(quesNum1-1)+')').countdown('destroy');
        $('.defaultCountdown:eq('+(quesNum1-1)+')').html(divContent);
        $('.defaultCountdown:eq('+(quesNum1-1)+')').addClass('expCountdown');
        
		$.mobile.changePage("#result3", {transition:'slide', reverse:false});
        return true;
	}
	
}

function goBack2(prevpage)
{
	$("#calculatorsource").hide();
	audinst.pause();
	audinst.currentTime = 0;
	$("#next3").show();
	if(k<=5 && k>1)
	{
	    $("#hard_Q").text(k-1);
	    $(".submit_icon,#feed3").hide();
	document.getElementById('que_number2').innerHTML=k-1;
	if(k==2)
	{
			$('#Back_But2').css('visibility','hidden');
	}
	
    var oId = window.location.href;
        oId = oId.substr(oId.indexOf("#"));
            
            var quesId1 = $(oId).find('form')[k-1].name;
            var quesNum1 = parseInt(quesId1.substr(1));
            var divContent = $('.defaultCountdown:eq('+(quesNum1-1)+')').html();
            $('.defaultCountdown:eq('+(quesNum1-1)+')').countdown('destroy');
            $('.defaultCountdown:eq('+(quesNum1-1)+')').html(divContent);
            $('.defaultCountdown:eq('+(quesNum1-1)+')').addClass('expCountdown');
    
        /*$(oId).find('form').eq(k-1).find('fieldset').find('input').attr('disabled','');
        $(oId).find('form').eq(k-1).find('input').attr('disabled','');*/
        
    $('#slider2 > ul').find('li').eq(slider2.index - 1).find('div').eq(0).show();
        slider2.prev();
    $('#slider2 > ul').find('li').eq(slider2.index + 1).find('div').eq(0).hide();
    k--;

        return true;
}
else {
    if(k==1){
        var oId = window.location.href;
        oId = oId.substr(oId.indexOf("#"));
        
        var quesId1 = $(oId).find('form')[k-1].name;
        var quesNum1 = parseInt(quesId1.substr(1));
        var divContent = $('.defaultCountdown:eq('+(quesNum1-1)+')').html();
        $('.defaultCountdown:eq('+(quesNum1-1)+')').countdown('destroy');
        $('.defaultCountdown:eq('+(quesNum1-1)+')').html(divContent);
        $('.defaultCountdown:eq('+(quesNum1-1)+')').addClass('expCountdown');
        
        $.mobile.changePage(prevpage, {transition:'slide', reverse:true});
        
        /*var prevID = $(prevpage).attr('id');
        var scrollerId = $('#'+prevID).find('.wrapper').attr('id');
        
        loaded(scrollerId);*/
        
        
    }
    return true;
    }
}

var m = 1;
function chkquestime3(nextpage)
{
	$("#calculatorsource").hide();
	audinst1.pause();
	audinst1.currentTime = 0;
	audinst2.pause();
	audinst2.currentTime = 0;    
    
	if(m<5)
	{
	    $("#hardest_Q").text(m+1);
	    if (m==4) {
		$("#sub_4").show();
			$("#next4").hide();
	    }
		document.getElementById('que_number3').innerHTML=m+1;
		$('#Back_But3').css('visibility','visible');
		
		var oId = window.location.href;
			oId = oId.substr(oId.indexOf("#"));
				
				var quesId1 = $(oId).find('form')[m-1].name;
			   
				var quesNum1 = parseInt(quesId1.substr(1));
				
				var divContent = $('.defaultCountdown:eq('+(quesNum1-1)+')').html(); 
				$('.defaultCountdown:eq('+(quesNum1-1)+')').countdown('destroy');
				$('.defaultCountdown:eq('+(quesNum1-1)+')').html(divContent);
				$('.defaultCountdown:eq('+(quesNum1-1)+')').addClass('expCountdown');
		
        /*$(oId).find('form').eq(m-1).find('fieldset').find('input').attr('disabled','');
        $(oId).find('form').eq(m-1).find('input').attr('disabled','');*/
        
		$('#slider3 > ul').find('li').eq(slider3.index + 1).find('div').eq(0).show();
        slider3.next();
        $('#slider3 > ul').find('li').eq(slider3.index - 1).find('div').eq(0).hide();
		m++;
		
        return true;
        
	}
	else {
        var oId = window.location.href;
        oId = oId.substr(oId.indexOf("#"));
        
        var quesId1 = $(oId).find('form')[m-1].name;
        
        var quesNum1 = parseInt(quesId1.substr(1));
        
        var divContent = $('.defaultCountdown:eq('+(quesNum1-1)+')').html();
        $('.defaultCountdown:eq('+(quesNum1-1)+')').countdown('destroy');
        $('.defaultCountdown:eq('+(quesNum1-1)+')').html(divContent);
        $('.defaultCountdown:eq('+(quesNum1-1)+')').addClass('expCountdown');
        
		$.mobile.changePage("#result4", {transition:'slide', reverse:false});
        return true;
	}
	
}

function goBack3(prevpage)
{
	$("#calculatorsource").hide();
	audinst1.pause();
	audinst1.currentTime = 0;
	audinst2.pause();
	audinst2.currentTime = 0;
	
	$("#next4").show();
	if(m<=5 && m>1)
	{
	    $("#hardest_Q").text(m-1);
	    $(".submit_icon,#feed4").hide();
	document.getElementById('que_number3').innerHTML=m-1;
	if(m==2)
	{
			$('#Back_But3').css('visibility','hidden');
	}
	
    var oId = window.location.href;
        oId = oId.substr(oId.indexOf("#"));
            
            var quesId1 = $(oId).find('form')[m-1].name;
            var quesNum1 = parseInt(quesId1.substr(1));
            var divContent = $('.defaultCountdown:eq('+(quesNum1-1)+')').html();
            $('.defaultCountdown:eq('+(quesNum1-1)+')').countdown('destroy');
            $('.defaultCountdown:eq('+(quesNum1-1)+')').html(divContent);
            $('.defaultCountdown:eq('+(quesNum1-1)+')').addClass('expCountdown');
            
        /*$(oId).find('form').eq(m-1).find('fieldset').find('input').attr('disabled','');
        $(oId).find('form').eq(m-1).find('input').attr('disabled','');*/
        
    $('#slider3 > ul').find('li').eq(slider3.index - 1).find('div').eq(0).show();
        slider3.prev();
    $('#slider3 > ul').find('li').eq(slider3.index + 1).find('div').eq(0).hide();
	m--;
	
        return true;
}
    else {
    if(m==1){
        var oId = window.location.href;
        oId = oId.substr(oId.indexOf("#"));
        
        var quesId1 = $(oId).find('form')[m-1].name;
        var quesNum1 = parseInt(quesId1.substr(1));
        var divContent = $('.defaultCountdown:eq('+(quesNum1-1)+')').html();
        $('.defaultCountdown:eq('+(quesNum1-1)+')').countdown('destroy');
        $('.defaultCountdown:eq('+(quesNum1-1)+')').html(divContent);
        $('.defaultCountdown:eq('+(quesNum1-1)+')').addClass('expCountdown');
        $.mobile.changePage(prevpage, {transition:'slide', reverse:true});
        
        /*var prevID = $(prevpage).attr('id');
        var scrollerId = $('#'+prevID).find('.wrapper').attr('id');
        
        loaded(scrollerId);*/
        
        
    }
        return true;
    }
}

/* Steps Code */
function showStep(e)
{
    var target = e.target;
    $(target).parentsUntil('.ui-page').find('.stepsContent').hide();
    $(target).parentsUntil('.ui-page').find('.stepsContent').eq($(target).parent().index()-1).show();
}

function changeImg(target)
{
    for(var itm=0; itm<=5; itm++)
    {
        $(target).parent().parent().find(".steps").eq(itm).attr('src', 'images/step_'+(itm+1)+'.png');
    }
    var imgFile = $(target).attr('src');
    target.src = imgFile.substr(0, imgFile.indexOf("."))+"_over.png";
}
/* Steps Code */

/* Page Reload Function */
function reloadPage()
{
    window.location = "index.html";
}
/* Page Reload Function */

/* OnClick Yellow Change */
function showHintYellow()
{
    $(".showYellowBG").removeClass('yellowBg');
    $(".showGreenBG").removeClass('greenBg');
    $(".showRedBG").removeClass('redBg');
    $(".showBlueBG").removeClass('blueBg');
    
    $(".showYellowBG").addClass('yellowBg');
}
/* OnClick Yellow Change */

/* OnClick Green Change */
function showHintGreen()
{
    $(".showYellowBG").removeClass('yellowBg');
    $(".showGreenBG").removeClass('greenBg');
    $(".showRedBG").removeClass('redBg');
    $(".showBlueBG").removeClass('blueBg');
    
    $(".showGreenBG").addClass('greenBg');
}
/* OnClick Green Change */

/* OnClick Red Change */
function showHintRed()
{
    $(".showYellowBG").removeClass('yellowBg');
    $(".showGreenBG").removeClass('greenBg');
    $(".showRedBG").removeClass('redBg');
    $(".showBlueBG").removeClass('blueBg');
    
    $(".showRedBG").addClass('redBg');
}
/* OnClick Red Change */

/* OnClick Blue Change */
function showHintBlue()
{
    $(".showYellowBG").removeClass('yellowBg');
    $(".showGreenBG").removeClass('greenBg');
    $(".showRedBG").removeClass('redBg');
    $(".showBlueBG").removeClass('blueBg');
    
    $(".showBlueBG").addClass('blueBg');
}
/* OnClick Blue Change */

/* Menu page button effect */
var downId;
function imgDown(id) 
{
    $('#'+id).attr('src', 'images/'+id+'_click_ipad.png');
    downId = id;
}
function imgUp() 
{
    $('#'+downId).attr('src', 'images/'+downId+'_ipad.png');
}
/* Menu page button effect */

var swipnolink=true;

/* Menu Page Lesson Blue effect */

function changeBlue(id)
{
    $('#'+id).addClass('blueFont');
    
}
function changeWhite(id)
{
    
    $('#'+id).removeClass('blueFont');
}
/* Menu Page Lesson Blue effect */

function correctAns(target, ans){
	$(target).parentsUntil('.ui-page').find('.ansChoice').find('li').eq(ans-1).find('.corrAns').remove();
	$(target).parentsUntil('.ui-page').find('.ansChoice').find('li').eq(ans-1).append('<nobr><span class="corrAns">&nbsp;<img src="images/tick.png" align="absmiddle"/></span></nobr>');
	$(".showYellowBG").removeClass('yellowBg');
	$(".showGreenBG").removeClass('greenBg');
	$(".showRedBG").removeClass('redBg');
	$(".showBlueBG").removeClass('blueBg');
}
function showstrike(id)
{
    $(id).show();
}
var isStrike = 0;
function changeText(elm)
{
    isStrike++;
	for(var itm=0; itm<$('.'+elm).length; itm++){
		var pnl = $('.'+elm).eq(itm);
		if((isStrike%2)==0){
			$('.'+elm).eq(itm).html($('.'+elm).eq(itm).text());
		}
		else {
			$('.'+elm).eq(itm).html("<S style='color:#FF0000'>"+$('.'+elm).eq(itm).html()+"</S>");
		}
	}
}

/* Number Change */

function blueButton1(target)
{
	$(target).parent().find('.answerChoice1').css({'display':'block'});
	$(target).parent().find('.answerChoice2, .answerChoice3').css({'display':'none'});
	$(target).parent().find('.blueButtonOpacityRedu1').css({'opacity':'0.5'});
	$(target).parent().find('.blueButtonOpacityRedu2, .blueButtonOpacityRedu3').css({'opacity':'1'});
}
function blueButton2(target)
{
	$(target).parent().find('.answerChoice2').css({'display':'block'});
	$(target).parent().find('.answerChoice1, .answerChoice3').css({'display':'none'});
	$(target).parent().find('.blueButtonOpacityRedu2').css({'opacity':'0.5'});
	$(target).parent().find('.blueButtonOpacityRedu1, .blueButtonOpacityRedu3').css({'opacity':'1'});
}
function blueButton3(target)
{
	$(target).parent().find('.answerChoice3').css({'display':'block'});
	$(target).parent().find('.answerChoice1, .answerChoice2').css({'display':'none'});
	$(target).parent().find('.blueButtonOpacityRedu3').css({'opacity':'0.5'});
	$(target).parent().find('.blueButtonOpacityRedu1, .blueButtonOpacityRedu2').css({'opacity':'1'});
}

/* Number Change */

/* Scroll Bar */

var scroll1;
function loaded() {
	//For 3 questions in same page. It is related to validateNewAns() function 
	ans =0;
	//End
	if(scroll1){
        scroll1.destroy();
    }
    var oId = window.location.href;
    if(oId.indexOf("#")!=-1){
        oId = oId.substr(oId.indexOf("#")+1);
    }
    if(oId == "test1_1"){
        scroll1 = new iScroll("scroll1", { scrollbarClass: 'myScrollbar' });
    }
    else if(oId == "test2_1"){
        scroll1 = new iScroll("scroll2", { scrollbarClass: 'myScrollbar' });
    }
    else if(oId == "test3_1"){
	    scroll1 = new iScroll("scroll3", { scrollbarClass: 'myScrollbar' });
    }
    else if(oId == "test4_1"){
        scroll1 = new iScroll("scroll4", { scrollbarClass: 'myScrollbar' });
    }
}

function newloaded(id) {
        scroll1 = new iScroll("scroll1", { scrollbarClass: 'myScrollbar' });
        scroll2 = new iScroll("scroll2", { scrollbarClass: 'myScrollbar' });
	scroll3 = new iScroll("scroll3", { scrollbarClass: 'myScrollbar' });
        scroll4 = new iScroll("scroll4", { scrollbarClass: 'myScrollbar' });
}

//document.addEventListener('touchmove', function (e) { e.preventDefault(); }, false);

//document.addEventListener('DOMContentLoaded', loaded, false);

/* Scroll Bar */

function historyBack()
{
    var backPage = $.mobile.urlHistory.stack[($.mobile.urlHistory.activeIndex-1)];
    if(backPage.url.indexOf('lesson')==-1 && backPage.url.indexOf("index")==-1){
        $.mobile.changePage(("#"+backPage.url), {transition:'none', reverse:false});
    }
    else if(backPage.url.indexOf('index')!=-1) {
        $.mobile.changePage("#index", {transition:'none', reverse:false});
    }
    else {
        window.history.back();
    }
    //window.location.href = "#";
}

function showResult(ev){
	var sourceContant1 =$('#txt1').val();
	$('#resultpg_1').html(sourceContant1);
	var sourceContant2 =$('#txt2').val();
	$('#resultpg_2').html(sourceContant2);
	var sourceContant3 =$('#txt3').val();
	$('#resultpg_3').html(sourceContant3);
	var sourceContant4 =$('#txt4').val();
	$('#resultpg_4').html(sourceContant4);
	var sourceContant5 =$('#txt5').val();
	$('#resultpg_5').html(sourceContant5);
	var sourceContant6 =$('#txt6').val();
	$('#resultpg_6').html(sourceContant6);
	var sourceContant7 =$('#txt7').val();
	$('#resultpg_7').html(sourceContant7);
	var sourceContant8 =$('#txt8').val();
	$('#resultpg_8').html(sourceContant8);
}

function reset() {
	$('#txt1').val("");
	$('#resultpg_1').html("");
	$('#txt2').val("");
	$('#resultpg_2').html("");
	$('#txt3').val("");
	$('#resultpg_3').html("");
	$('#txt4').val("");
	$('#resultpg_4').html("");
	$('#txt5').val("");
	$('#resultpg_5').html("");
	$('#txt6').val("");
	$('#resultpg_6').html("");
	$('#txt7').val("");
	$('#resultpg_7').html("");
	$('#txt8').val("");
	$('#resultpg_8').html("");
	$("#txt1, #txt2, #txt3, #txt4, #txt5, #txt6, #txt7, #txt8").css("height","200px");
}


function DragNDrop() {
    try{
	$('.drgDiv,.drgDiv1').draggable("destroy");
    }
    catch(e){}
    
    
    $('.drgDiv').draggable({
	containment: "#answerchoose1" ,
	helper:"clone",
	revert:"invalid",
	zIndex: 10000
    });
    
    
    $(".dropDiv").droppable({
	accept: ".drgDiv",
	drop:function(event,ui){
		$(this).css("background","#8080ff");
	    	$(this).html($(ui.helper)[0].innerText);
	}
    });
    $('.drgDiv1').draggable({
	containment: "#answerchoose4" ,
	helper:"clone",
	revert:"invalid",
	zIndex: 10000
    });
    
    
    $(".dropDiv1").droppable({
	accept: ".drgDiv1",
	drop:function(event,ui){
		$(this).css("background","#8080ff");
	    	$(this).html($(ui.helper)[0].innerText);
	}
    });
}
var answer1=0;
function submit_1() {
    answer1=0;
    for (var i=1;i<6;i++) {
	var temp1=$('input[name=r1]:checked', '#q'+i).val();
	if (temp1=="answer") {
	    answer1++;
	}
    }
    if ($(".numTxt").val()==5) {
	answer1++;
    }
    feedresize();
	$("#cor_ans").text(answer1);
	$("#wro_ans").text(5-answer1);
	$("#blocker,#feedback").show();
}
function submit_2() {
    answer1=0;
    for (var i=11;i<16;i++) {
	var temp1=$('input[name=r1]:checked', '#q'+i).val();
	if (temp1=="answer") {
	    answer1++;
	}
    }
feedresize();
	$("#cor_ans").text(answer1);
	$("#wro_ans").text(5-answer1);
	$("#blocker,#feedback").show();
}
function submit_3() {
    answer1=0;
    for (var i=30;i<35;i++) {
	var temp1=$('input[name=r1]:checked', '#q'+i).val();
	if (temp1=="answer") {
	    answer1++;
	}
    }
    if ($(".dropDiv ").text()=="HTML") {
	answer1++;
    }
    if ($(".dropDiv1").text()=="Dynamic") {
	answer1++;
    }
feedresize();
	$("#cor_ans").text(answer1);
	$("#wro_ans").text(4-answer1);
	$("#blocker,#feedback").show();
}
function submit_4() {
    answer1=0;
    
    for (var i=43;i<48;i++) {
	var temp2=0;
	for (var p=1;p<5;p++) {
	    if($("#q"+i).find("#r"+p).is(":checked"))
	    {
		if($("#q"+i).find("#r"+p).val()=="answer")
		{
		    temp2++;
		}
		if (temp2==2) {
		answer1++;
		}
	    }
	    
	}
	
    }

    feedresize();
	$("#cor_ans").text(answer1);
	$("#wro_ans").text(3-answer1);
	$("#blocker,#feedback").show();
}
function block_close()
{
	$("#blocker,#feedback").hide();
}

function AudioState() {
    
    if (audinst.paused == false) {
	audinst.pause();
	audinst.currentTime = 0;
	audinst.play();
    }
    else{
	audinst.play()
    }
}
function AudioState1() {
    if (audinst1.paused == false) {
	audinst1.pause();
	audinst1.currentTime = 0;
	audinst1.play();
    }
    else{
	audinst1.play()
    }
}
function AudioState2() {
    if (audinst2.paused == false) {
	audinst2.pause();
	audinst2.currentTime = 0;
	audinst2.play();
    }
    else{
	audinst2.play()
    }
}

var activityHolder;
function loadKeypad() {
     keypad = new vKeyPad(".numTxt", ".testPageContainer", {
		hiddenKeys:["sub", "hyphen","multi", "sup", "bold"],
		useComplexMath:false,
		colCount:3,
		useParseMath:false, deleteMethod:"byChar", isForceTaggable:true,
		isMultiline:false,
		maximumLength:3,
		maxLenMargin:40,
		clearZero :false,
		multiSymbol:"\u00D7",	
	});
     
     $('.keypadHolder ').draggable({containment:'.testPageContainer'});
}
function feedresize(){
    
    var feedwidth=eval($("#feedback").css("width").split("px")[0])
    var temp_1=feedwidth/3;
    if (temp_1<50) {
	temp_1=51;
    }
    $("#feedback").css({"font-size":temp_1+"%"});
    
}
$(window).resize(function(){
  feedresize();
  //$(".calc-container").css("left","16%");
});
