# JSL :: JavaScript Standard Library

Fun times this one, it was during IE4 era, a browser with no `try/catch` statements.

- - -

**Update - New [JSL Revision](./JSL_V2.md) available**

Today JavaScript is strongly used from web or intranet developers and there are a lot of libraries to add special effects, simple way to implement ajax, simple way to implement complex components, forms, tabs or everything else. These different libraries are really cool but there are some problems:
  * the sum of the size of each library should be a problem for normal connections (and often each library has one or more workarounds)
  * each library should use a dedicated function with a global scope or name, more libraries in the same page should produce an error for an overwritten object, proto or function (while JSL doesn't add or modify anything on JS 1.6 compatible and standard browsers)
  * each library has some cross-browser work around or fix, more libraries should use everytime the same fixes or work arounds (increasing size of every script and sometimes creating incompatibility problems)
  * every developer need to copy and paste or to produce good workarounds for each library He's developping (but are these fixes good or portable enought ?)
I write components and some other usefull (at least for me) library and every time I need to find my "perfect" work around or fix. One day I've decieded to stop this horrible way to write JS code, that's why I've created the JSL.


### What is the JSL ?

JSL is a single and small file ( IE4 compatible packed version: 7.77 Kb ) with some JavaScript 1.6 standard methods or functions that are not present on some browsers. Its goals is to forget work arounds for every library or script that's included on a web page. You could just add JSL even before your scripts to add portability or more compatibility, then you don't need to rewrite anything.


### What's about compatibility ?

JSL is compatible with every browser that supports at least JavaScript 1.2 ( then is compatible with Safari, KDE, Internet Explorer 5, 5.5 and 4 too! ). Write your JavaScript standard code for FireFox 1.5 (for example ...) and run this code with every browser (at least with these core functions)
Which are JSL features ?

  * Global
    * encodeURI
    * encodeURIComponent
    * decodeURI
    * decodeURIComponent
    * Error (class)
  * document
    * getElementById
    * getElementsByTagName (nested elements too)
    * getElementsByName
  * Object
    * toSource (FireFox Object convertion)
  * Date
    * getYear (standard value: getFullYear() - 1900)
  * Array
    * pop
    * push
    * shift
    * splice
    * unshift
    * indexOf
    * lastIndexOf
    * every
    * filter
    * forEach
    * map
    * some
    * Function
    * apply
    * call
    * String
    * lastIndexOf
    * replace (function as second argument)
  * extra
    * XMLHttpRequest (NOTE: with IE4 or uncompatible browsers returns null)
    * undefined (empty var for an undefined value)


### How to use JSL ?

Just write this piece of code before every script tag
`<script type="text/javascript" src="JSL.js"><!--// (C) JSL //--></script>`
If you have a lot of libraries/pages, subdomains or subfolders you should use an absolute URL to cache JSL file ( I mean ... doesn't copy JSL.js in every folder :-) ), then after first download this library will be "0 size".


### F.A.Q.

Doh! ... a damn Object.prototype!!!
If you can "support" Function.call or Function.apply or if you think Array.pop, push or String.replace are "right protos", why do you think that a method such toSource is a problem or an issue ? Look to toSource as an implementation of "toString", it's a native Object proto and it need to be available with every kind of object. It's not an "Object.prototype.helloMyFavolousProto", it's just a FireFox Object standard method!
Ok ... I /(don't)?/ agree with you, then how to resolve for in loop problem ?
JSL version 1.1 or above has a dedicated method for for in loops. If you absolutely need to skip these Function, Array, Object, Date (getYear) or String prototypes you should simply use a loop like this:
for(var key in myObject) {
	if(!$JSL.has(key)) {
		// do everything you need
	};
};
$JSL.has proto verify if itself has modified JavaScript "core" with one or more prototype. With full JavaScript 1.6 compatible browsers this method doesn't affect performances and with an incremental version of internal array, old browsers will have always good performance too. Finally I think that a single "if" to add where you need to skip prototypes is not so terrible as solution.
No man, I don't want to change every for in loop, bye bye JSL ...
If you don't need toSource Object method in your script, you should simply add this single line of code after JSL code or as first line of first script present after JSL.
delete Object.prototype.toSource;
That's all !
Why I should care about IE4 ?
JSL exists to don't care about browsers, then you don't need to care about IE4, IE5, Safari, Opera, FireFox, Netscape or other JS >= 1.2 browsers. For example, did you know that encodeURIComponent is the only correct way to send iso or unicode strings with AJAX and it's not supported by IE5 or some other browser ? ... and did you know that escape function is not a good alternative way to implement encodeURICompoenent ? escape function converts unicode in a format that is not valid, for exampe %u0100 or %u10AF that aren't correct strings for uri interactions. Then just use encode/decodeURICompoent interactions, without tips or tricks, add JSL before your script and "magically" every browser will support this global funciton. And again, String.replace with function as second argument is "buggy" on Safari and KDE 3.5 too and not present on many other browsers. With this proto you can finally use the wonderful replace with function as argument method with every browser, forget split()for(length)join() or other tricks alternative, just replace!
What about DOM ?
DOM level 2 has a lot of features that are "in core", is not simple to creates a good, fast and small library to implement DOM methods for old browsers. Maybe one day I'll try to write DOMSL (DOM Standard Library) but now you can use JSL to create your DOM SL thanks to getElementById and getElementsByTagName implementation ( and after please contact me, I need a good and complete DOM SL library!!! :D ).
Why Open Source code is so horrible ?
Sorry for source code but it has been written to preserve compressed / packed size, then I've often used same names without a logic or I've removed each space that's not important.


### Source Code

```js
// (C) Andrea Giammarchi - JSL 1.4b
var undefined;
function $JSL(){
	this.inArray=function(){
		var tmp=false,i=arguments[1].length;
		while(i&&!tmp)tmp=arguments[1][--i]===arguments[0];
		return tmp;
	};
	this.has=function(str){return $JSL.inArray(str,$has)};
	this.random=function(elm){
		var tmp=$JSL.$random();
		while(typeof(elm[tmp])!=="undefined")tmp=$JSL.$random();
		return tmp;
	};
	this.$random=function(){return (Math.random()*1234567890).toString()};
	this.reverse=function(str){return str.split("").reverse().join("")};
	this.replace=function(str){
		var tmp=str.split(""),i=tmp.length;
		while(i>0)tmp[--i]=$JSL.$replace(tmp[i]);
		return tmp.join("");
	};
	this.$replace=function(tmp){
		var i=tmp.length===1?tmp.charCodeAt(0):0;
		switch(i) {
			case 8	:tmp="\\b";break;
			case 10	:tmp="\\n";break;
			case 11	:tmp="\\v";break;
			case 12	:tmp="\\f";break;
			case 13	:tmp="\\r";break;
			case 34	:tmp="\\\"";break;
			case 92	:tmp="\\\\";break;
			default:
				tmp=tmp.replace(/([\x00-\x07]|[\x0E-\x1F]|[\x7F-\xFF])/g,function(a,b){return "\\x"+$JSL.charCodeAt(b)}).
					replace(/([\u0100-\uFFFF])/g,function(a,b){b=$JSL.charCodeAt(b);return b.length<4?"\\u0"+b:"\\u"+b});
				break;
		};
		return tmp;
	};
	this.charCodeAt=function(str){return $JSL.$charCodeAt(str.charCodeAt(0))};
	this.$charCodeAt=function(i){
		var str=i.toString(16).toUpperCase();
		return str.length<2?"0"+str:str;
	};
	this.$toSource=function(elm){return elm.toSource().replace(/^(\(new \w+\()([^\000]+)(\)\))$/,"$2")};
	this.$toInternalSource=function(elm){
		var tmp=null;
		switch(elm.constructor) {
			case Boolean:
			case Number:
				tmp=elm;
				break;
			case String:
				tmp=$JSL.$toSource(elm);
				break;
			default:
				tmp=elm.toSource();
				break;
		};
		return tmp;
	};
	this.getElementsByTagName=function(scope,i,elm,str){
		var tmp=$JSL.$getElementsByTagName(scope),j=tmp.length,$tmp=[];
		while(i<j){if(tmp[i][str]===elm||elm==="*")$tmp.push($JSL.$getElementsByName(tmp[i]));++i};
		if(!$tmp.item){if(!$JSL.has("item"))$has.push("item");$tmp.item=function(tmp){return this[tmp]}};
		return $tmp;
	};
	this.$getElementsByTagName=function(scope){return scope.layers||scope.all};
	this.$getElementsByName=function(elm) {
		if(!elm.getElementsByTagName)	elm.getElementsByTagName=document.getElementsByTagName;
		return elm;
	};
	this.encodeURI=function(str){return str.replace(/"/g,"%22").replace(/\\/g,"%5C")};
	this.$encodeURI=function(str){return $JSL.$charCodeAt(str)};
	this.$encodeURIComponent=function(a,b){
		var i=b.charCodeAt(0),str=[];
		if(i<128)		str.push(i);
		else if(i<2048)		str.push(0xC0+(i>>6),0x80+(i&0x3F));
		else if(i<65536)	str.push(0xE0+(i>>12),0x80+(i>>6&0x3F),0x80+(i&0x3F));
		else			str.push(0xF0+(i>>18),0x80+(i>>12&0x3F),0x80+(i>>6&0x3F),0x80+(i&0x3F));
		return "%"+str.map($JSL.$encodeURI).join("%");
	};
	this.$decodeURIComponent=function(a,b,c,d,e){
		var i=0;
		if(e)	  i=parseInt(e.substr(1,2),16);
		else if(d)i=((parseInt(d.substr(1,2),16)-0xC0)<<6)+(parseInt(d.substr(4,2),16)-0x80);
		else if(c)i=((parseInt(c.substr(1,2),16)-0xE0)<<12)+((parseInt(c.substr(4,2),16)-0x80)<<6)+(parseInt(c.substr(7,2),16)-0x80);
		else	  i=((parseInt(b.substr(1,2),16)-0xF0)<<18)+((parseInt(b.substr(4,2),16)-0x80)<<12)+((parseInt(b.substr(7,2),16)-0x80)<<6)+(parseInt(b.substr(10,2),16)-0x80);
		return String.fromCharCode(i);
	};
	var $has=[];
	if(!Object.prototype.toSource){$has[$has.length]="toSource";Object.prototype.toSource=function(){
		var str=[];
		switch(this.constructor) {
			case Boolean:
				str.push("(new Boolean(",this,"))");
				break;
			case Number:
				str.push("(new Number(",this,"))");
				break;
			case String:
				str.push("(new String(\"",$JSL.replace(this),"\"))");
				break;
			case Date:
				str.push("(new Date(",this.getTime(),"))");
				break;
			case Error().constructor:
				str.push("(new Error(",$JSL.$toSource(this.message),",",$JSL.$toSource(this.fileName),",",this.lineNumber,"))");
				break;
			case Function:
				str.push("(",$JSL.$replace(this.toString()),")");
				break;
			case Array:
				var i=0,j=this.length;
				while(i<j)	str.push($JSL.$toInternalSource(this[i++]));
				str=["[",str.join(", "),"]"];
				break;
			default:
				var i=0,tmp;
				for(i in this){if(i!=="toSource")
					str.push($JSL.$toSource(i)+":"+$JSL.$toInternalSource(this[i]));
				};
				str=["{",str.join(", "),"}"];
				break;
		};
		return str.join("");
	}};
	if(!Function.prototype.apply){$has[$has.length]="apply";Function.prototype.apply=function(){
		var i=arguments.length===2?arguments[1].length:0,str,tmp=[],elm=(""+this).replace(/[^\(]+/,"function");
		if(!arguments[0])arguments[0]={};
		while(i)tmp.unshift("arguments[1]["+(--i)+"]");
		do{str="__".concat($JSL.random(arguments[0]).replace(/\./,"_"),"__")}while(new RegExp(str).test(elm));
		eval("var ".concat(str,"=arguments[0];tmp=(",elm.replace(/([^$])\bthis\b([^$])/g,"$1".concat(str,"$2")),")(",tmp.join(","),")"));
		return tmp;
	}};
	if(!Function.prototype.call){$has[$has.length]="call";Function.prototype.call=function(){
		var i=arguments.length,tmp=[];
		while(i>1)tmp.unshift(arguments[--i]);
		return this.apply((i?arguments[0]:{}),tmp);
	}};
	if(!Array.prototype.pop){$has[$has.length]="pop";Array.prototype.pop=function(){
		var a=this.length,r=this[--a];
		if(a>=0)this.length=a;
		return r;
	}};
	if(!Array.prototype.push){$has[$has.length]="push";Array.prototype.push=function(){
		var a=0,b=arguments.length,r=this.length;
		while(a<b)this[r++]=arguments[a++];
		return r;
	}};
	if(!Array.prototype.shift){$has[$has.length]="shift";Array.prototype.shift=function(){
		this.reverse();
		var r=this.pop();
		this.reverse();
		return r;
	}};
	if(!Array.prototype.splice){$has[$has.length]="splice";Array.prototype.splice=function(){
		var a,b,c,d=arguments.length,tmp=[],r=[];
		if(d>1){
			arguments[0]=parseInt(arguments[0]);
			arguments[1]=parseInt(arguments[1]);
			c=arguments[0]+arguments[1];
			for(a=0,b=this.length;a<b;a++){
				if(a<arguments[0]||a>=c){
					if(a===c&&d>2){
						for(a=2;a<d;a++)tmp.push(arguments[a]);
						a=c;
					};
					tmp.push(this[a]);
				}
				else
					r.push(this[a]);
			};
			for(a=0,b=tmp.length;a<b;a++)
				this[a]=tmp[a];
			this.length = a;
		};
		return r;
	}};
	if(!Array.prototype.unshift){$has[$has.length]="unshift";Array.prototype.unshift=function(){
		var i=arguments.length;
		this.reverse();
		while(i>0)this.push(arguments[--i]);
		this.reverse();
		return this.length;
	}};
	if(!Array.prototype.indexOf){$has[$has.length]="indexOf";Array.prototype.indexOf=function(elm,i){
		var j=this.length;
		if(!i)i=0;
		if(i>=0){while(i<j){if(this[i++]===elm){
			i=i-1+j;j=i-j;
		}}}
		else
			j=this.indexOf(elm,j+i);
		return j!==this.length?j:-1;
	}};
	if(!Array.prototype.lastIndexOf){$has[$has.length]="lastIndexOf";Array.prototype.lastIndexOf=function(elm,i){
		var j=-1;
		if(!i)i=this.length;
		if(i>=0){do{if(this[i--]===elm){
			j=i+1;i=0;
		}}while(i>0)}
		else if(i>-this.length)
			j=this.lastIndexOf(elm,this.length+i);
		return j;
	}};
	if(!Array.prototype.every){$has[$has.length]="every";Array.prototype.every=function(callback,elm){
		var b=false,i=0,j=this.length;
		if(!elm){	while(i<j&&!b)	b=!callback(this[i]||this.charAt(i),i++,this)}
		else {		while(i<j&&!b)	b=!callback.apply(elm,[this[i]||this.charAt(i),i++,this]);}
		return !b;
	}};
	if(!Array.prototype.filter){$has[$has.length]="filter";Array.prototype.filter=function(callback,elm){
		var r=[],i=0,j=this.length;
		if(!elm){while(i<j){if(callback(this[i],i++,this))
			r.push(this[i-1]);
		}} else {while(i<j){if(callback.apply(elm,[this[i],i++,this]))
			r.push(this[i-1]);
		}}
		return r;
	}};
	if(!Array.prototype.forEach){$has[$has.length]="forEach";Array.prototype.forEach=function(callback,elm){
		var i=0,j=this.length;
		if(!elm){	while(i<j)	callback(this[i],i++,this)}
		else {		while(i<j)	callback.apply(elm,[this[i],i++,this]);}
	}};
	if(!Array.prototype.map){$has[$has.length]="map";Array.prototype.map=function(callback,elm){
		var r=[],i=0,j=this.length;
		if(!elm){	while(i<j)	r.push(callback(this[i],i++,this))}
		else {		while(i<j)	r.push(callback.apply(elm,[this[i],i++,this]));}
		return r;
	}};
	if(!Array.prototype.some){$has[$has.length]="some";Array.prototype.some=function(callback,elm){
		var b=false,i=0,j=this.length;
		if(!elm){	while(i<j&&!b)	b=callback(this[i],i++,this)}
		else {		while(i<j&&!b)	b=callback.apply(elm,[this[i],i++,this]);}
		return b;
	}};
	if(!String.prototype.lastIndexOf){if(!this.inArray("lastIndexOf",$has))$has[$has.length]="lastIndexOf";String.prototype.lastIndexOf=function(elm,i){
		var str=$JSL.reverse(this),elm=$JSL.reverse(elm),r=str.indexOf(elm,i);
		return r<0?r:this.length-r;
	}};
	if("aa".replace(/\w/g,function(){return arguments[1]+" "})!=="0 1 "){$has[$has.length]="replace";String.prototype.replace=function(replace){return function(reg,func){
		var r="",tmp=$JSL.random(String);
		String.prototype[tmp]=replace;
		if(func.constructor!==Function)
			r=this[tmp](reg,func);
		else {
			function getMatches(reg,pos,a) {
				function io() {
					var a=reg.indexOf("(",pos),b=a;
					while(a>0&&reg.charAt(--a)==="\\"){};
					pos=b!==-1?b+1:b;
					return (b-a)%2===1?1:0;
				};
				do{a+=io()}while(pos!==-1);
				return a;
			};
			function $replace(str){
				var j=str.length-1;
				while(j>0)str[--j]='"'+str[j].substr(1,str[j--].length-2)[tmp](/(\\|")/g,'\\$1')+'"';
				return str.join("");
			};
			var p=-1,i=getMatches(""+reg,0,0),args=[],$match=this.match(reg),elm=$JSL.$random()[tmp](/\./,'_AG_');
			while(this.indexOf(elm)!==-1)elm=$JSL.$random()[tmp](/\./,'_AG_');
			while(i)args[--i]=[elm,'"$',(i+1),'"',elm].join("");
			if(!args.length)r="$match[i],(p=this.indexOf($match[i++],p+1)),this";
			else		r="$match[i],"+args.join(",")+",(p=this.indexOf($match[i++],p+1)),this";
			r=eval('['+$replace((elm+('"'+this[tmp](reg,'"'+elm+',func('+r+'),'+elm+'"')+'"')+elm).split(elm))[tmp](/\n/g,'\\n')[tmp](/\r/g,'\\r')+'].join("")');
		};
		delete String.prototype[tmp];
		return r;
	}}(String.prototype.replace)};
	if((new Date().getYear()).toString().length===4){$has[$has.length]="getYear";Date.prototype.getYear=function(){
		return this.getFullYear()-1900;
	}};
};$JSL=new $JSL();
if(typeof(encodeURI)==="undefined"){function encodeURI(str){
	var elm=/([\x00-\x20]|[\x25|\x3C|\x3E|\x5B|\x5D|\x5E|\x60|\x7F]|[\x7B-\x7D]|[\x80-\uFFFF])/g;
	return $JSL.encodeURI(str.toString().replace(elm,$JSL.$encodeURIComponent));
}};
if(typeof(encodeURIComponent)==="undefined"){function encodeURIComponent(str){
	var elm=/([\x23|\x24|\x26|\x2B|\x2C|\x2F|\x3A|\x3B|\x3D|\x3F|\x40])/g;
	return $JSL.encodeURI(encodeURI(str).replace(elm,function(a,b){return "%"+$JSL.charCodeAt(b)}));
}};
if(typeof(decodeURIComponent)==="undefined"){function decodeURIComponent(str){
	var elm=/(%F[0-9A-F]%E[0-9A-F]%[A-B][0-9A-F]%[8-9A-B][0-9A-F])|(%E[0-9A-F]%[A-B][0-9A-F]%[8-9A-B][0-9A-F])|(%[C-D][0-9A-F]%[8-9A-B][0-9A-F])|(%[0-9A-F]{2})/g;
	return str.toString().replace(elm,$JSL.$decodeURIComponent);
}};
if(typeof(decodeURI)==="undefined"){function decodeURI(str){
	return decodeURIComponent(str);
}};
if(!document.getElementById){document.getElementById=function(elm){
	return $JSL.$getElementsByName($JSL.$getElementsByTagName(this)[elm]);
}};
if(!document.getElementsByTagName){document.getElementsByTagName=function(elm){
	return $JSL.getElementsByTagName(this,0,elm.toUpperCase(),"tagName");
}};
if(!document.getElementsByName){document.getElementsByName=function(elm){
	return $JSL.getElementsByTagName(this,0,elm,"name");
}};
if(typeof(XMLHttpRequest)==="undefined"){XMLHttpRequest=function(){
	var tmp=null,elm=navigator.userAgent;
	if(elm.toUpperCase().indexOf("MSIE 4")<0&&window.ActiveXObject)
		tmp=elm.indexOf("MSIE 5")<0?new ActiveXObject("Msxml2.XMLHTTP"):new ActiveXObject("Microsoft.XMLHTTP");
	return tmp;
}};
if(typeof(Error)==="undefined")Error=function(){};
Error = function(base){return function(message){
	var tmp=new base();
	tmp.message=message||"";
	if(!tmp.fileName)
		tmp.fileName=document.location.href;
	if(!tmp.lineNumber)
		tmp.lineNumber=0;
	if(!tmp.stack)
		tmp.stack="Error()@:0\n(\""+this.message+"\")@"+tmp.fileName+":"+this.lineNumber+"\n@"+tmp.fileName+":"+this.lineNumber;
	if(!tmp.name)
		tmp.name="Error";
	return tmp;
}}(Error);
```
