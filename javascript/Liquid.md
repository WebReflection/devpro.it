# Liquid

Pure JavaScript Liquid Effect.

[Demo](../demo/liquid.html)

### Source Code

```js
(function(round, window){
/** Liquid - Pure JavaScript Liquid Effect
 * @author          Andrea Giammarchi
 * @blog            webreflection.blogspot.com
 * @license         Mit Style License
 * @version         1.4 - less DOM, smaller, faster
 * @compatibility   all but Safari <= 2 and IE4
 */
function onload(config){
    function onload(){
        wrap.removeChild(img);
        img = $img = null;
        config.interval = 0;
        config.callee = config.pause = config.play = null;
        if(config.callback)
            config.callback(config)
        ;
    };
    var wrap    = config.target.appendChild(config.document.createElement("div")),
        div     = wrap.cloneNode(true),
        width   = this.width,
        height  = this.height,
        img     = this,
        $wrap   = wrap.style,
        $div    = div.style,
        $img    = img.style,
        scale   = config.scale || 2000,
        speed   = config.speed || 10,
        left, top, $
    ;
    margin($wrap, $div, $img);
    fontSize($wrap, $div);
    position($div, $img);
    $wrap.position = "relative";
    $wrap.width = width + "px";
    $wrap.height = height + "px";
    $wrap.overflow = "hidden";
    $img.zIndex = 1;
    $div.zIndex = 2;
    scale /= 100;
    switch(config.direction){
        case    "top":
            $div.height = 0 + "px";
            $div.width = width + "px";
            $img.top = (top = height) + "px";
            img.setAttribute("width", width);
            img.setAttribute("height", round(scale * height));
            left = 0;
            scale = speed / scale;
            config.interval = setTimeout(config.callee = function(){
                if(left < height){
                    top -= speed;
                    $img.top = round(top + left) + "px";
                    if(top < 0)
                        $div.height = round(left += scale) + "px"
                    ;
                    config.interval = setTimeout(config.callee, 15);
                } else
                    onload()
                ;
            }, 15);
            break;
        case    "bottom":
            $div.height = 0 + "px";
            $div.width = width + "px";
            $div.top = height + "px";
            $img.top = (top = -round(scale * height)) + "px";
            img.setAttribute("width", width);
            img.setAttribute("height", -top);
            width = -round(scale * height - height);
            left = 0;
            scale = speed / scale;
            config.interval = setTimeout(config.callee = function(){
                if(left < height){
                    top += speed;
                    $img.top = round(top - left) + "px";
                    if(width < top){
                        $div.height = ($ = round(left += scale)) + "px";
                        $div.top = (height - $) + "px";
                    };
                    config.interval = setTimeout(config.callee, 15);
                } else
                    onload()
                ;
            }, 15);
            break;
        case    "right":
            $div.width = 0 + "px";
            $div.left = width + "px";
            $div.height = height + "px";
            $img.left = (left = -round(scale * width)) + "px";
            img.setAttribute("width", -left);
            img.setAttribute("height", height);
            height = -round(scale * width - width);
            top = 0;
            scale = speed / scale;
            config.interval = setTimeout(config.callee = function(){
                if(top < width){
                    left += speed;
                    $img.left = round(left - top) + "px";
                    if(height < left){
                        $div.width = ($ = round(top += scale)) + "px";
                        $div.left = (width - $) + "px";
                    };
                    config.interval = setTimeout(config.callee, 15);
                } else
                    onload()
                ;
            }, 15);
            break;
        default:
            $div.width = 0 + "px";
            $img.top = "0px";
            $div.height = height + "px";
            $img.left = (left = width) + "px";
            img.setAttribute("width", round(scale * width));
            img.setAttribute("height", height);
            top = 0;
            scale = speed / scale;
            config.interval = setTimeout(config.callee = function(){
                if(top < width){
                    left -= speed;
                    $img.left = round(left + top) + "px";
                    if(left <= 0)
                        $div.width = round(top += scale) + "px"
                    ;
                    config.interval = setTimeout(config.callee, 15);
                } else
                    onload()
                ;
            }, 15);
            break;
    };
    $div.background = (config.background || "transparent") + " url(" + config.src + ") no-repeat " + (config.direction || "left");
    wrap.appendChild(img);
    wrap.appendChild(div);
};
function $onload(config){
    function onload(){
        wrap.parentNode.removeChild(wrap);
        wrap.removeChild(img);
        wrap.removeChild(div);
        wrap = div = img = $wrap = $div = $img = null;
        config.interval = 0;
        config.callee = config.pause = config.play = null;
        if(config.callback)
            config.callback(config)
        ;
    };
    var div     = config.target.getElementsByTagName("div"),
        wrap    = div[0],
        div     = div[1],
        img     = config.document.createElement("img"),
        $wrap   = wrap.style,
        $div    = div.style,
        $img    = img.style,
        width   = parseInt($wrap.width, 10),
        height  = parseInt($wrap.height, 10),
        scale   = config.scale || 2000,
        speed   = config.speed || 10,
        left, top, $
    ;
    position($img);
    margin($img);
    $img.zIndex = 1;
    $div.backgroundPosition = config.direction || "left";
    scale /= 100;
    img.onload = function(){
        switch(config.direction){
            case    "top":
                img.setAttribute("width", width);
                img.setAttribute("height", width = round(scale * height));
                $img.top = (top = -width + height) + "px";
                left = 0;
                scale = speed / scale;
                config.interval = setTimeout(config.callee = function(){
                    if(top + left < height){
                        top += speed;
                        $img.top = round(top + left) + "px";
                        if(-height < ($ = round(left -= scale)))
                            $div.height = (height + $) + "px"
                        ;
                        config.interval = setTimeout(config.callee, 15);
                    } else
                        onload()
                    ;
                }, 15);
                break;
            case    "bottom":
                img.setAttribute("width", width);
                img.setAttribute("height", width = round(scale * height));
                width += left = height;
                top = 0;
                scale = speed / scale;
                config.interval = setTimeout(config.callee = function(){
                    if(-top < width){
                        top -= speed;
                        $img.top = round(top + (height - left)) + "px";
                        $div.top = (height - ($ = round(left -= scale))) + "px";
                        if(0 < $)
                            $div.height = $ + "px"
                        ;
                        config.interval = setTimeout(config.callee, 15);
                    } else
                        onload()
                    ;
                }, 15);
                break;
            case    "right":
                img.setAttribute("height", height);
                img.setAttribute("width", height = round(scale * width));
                left = top = 0;
                scale = speed / scale;
                config.interval = setTimeout(config.callee = function(){
                    if(-left - top < height){
                        $img.left = round((left -= speed) + top) + "px";
                        if(($ = round(top += scale)) < width){
                            $div.width = (width - $) + "px";
                            $div.left = $ + "px";
                        };
                        config.interval = setTimeout(config.callee, 15);
                    } else
                        onload()
                    ;
                }, 15);
                break;
            default:
                img.setAttribute("height", height);
                img.setAttribute("width", left = round(scale * width));
                $img.left = (left = -(left - width)) + "px";
                top = 0;
                scale = speed / scale;
                config.interval = setTimeout(config.callee = function(){
                    if(left - top < width){
                        $img.left = round((left += speed) - top) + "px";
                        $ = round(top += scale);
                        if($ < width)
                            $div.width = (width - $) + "px"
                        ;
                        config.interval = setTimeout(config.callee, 15);
                    } else
                        onload()
                    ;
                }, 15);
                break;
        };
    };
    wrap.insertBefore(img, div);
    img.src = config.src;
};
function margin(){
    for(var i = 0, length = arguments.length; i < length; ++i)
        arguments[i].padding = arguments[i].margin = arguments[i].border = "0px"
    ;
};
function fontSize(){
    for(var i = 0, length = arguments.length; i < length; ++i){
        arguments[i].fontSize = arguments[i].lineHeight = "0px";
        arguments[i].textAlign = "left";
    };
};
function position(){
    for(var i = 0, length = arguments.length; i < length; ++i){
        arguments[i].top = arguments[i].left = "0px";
        arguments[i].position = "absolute";
    };
};
function Liquid(config){
    if(config.reverse)
        $onload(config)
    ; else {
        var img = (config.document || (config.document = document)).createElement("img");
        img.onload = function(){
            img.removeAttribute("onload");
            if(config.onload)
                config.onload(img)
            ;
            onload.call(img, config);
        };
        img.src = config.src;
        config.pause = function(){clearTimeout(config.interval)};
        config.play = function(){config.interval = setTimeout(config.callee, 15)};
    };
    return config;
};

window.Liquid = Liquid;

})(Math.round, window);
```