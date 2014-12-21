---
layout: post
title:  "Configuring Cuda on AWS for Mumax3"
date:   2014-12-07 10:10:25
categories: magnets
---
<!DOCTYPE html>
<html>
<head>

<meta charset="utf-8" />
<title>Notebook</title>

<script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.1.10/require.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.0.3/jquery.min.js"></script>

<style type="text/css">
    .clearfix{*zoom:1}.clearfix:before,.clearfix:after{display:table;content:"";line-height:0}
.clearfix:after{clear:both}
.hide-text{font:0/0 a;color:transparent;text-shadow:none;background-color:transparent;border:0}
.input-block-level{display:block;width:100%;min-height:30px;-webkit-box-sizing:border-box;-moz-box-sizing:border-box;box-sizing:border-box}
article,aside,details,figcaption,figure,footer,header,hgroup,nav,section{display:block}
audio,canvas,video{display:inline-block;*display:inline;*zoom:1}
audio:not([controls]){display:none}
html{font-size:100%;-webkit-text-size-adjust:100%;-ms-text-size-adjust:100%}
a:focus{outline:thin dotted #333;outline:5px auto -webkit-focus-ring-color;outline-offset:-2px}
a:hover,a:active{outline:0}
sub,sup{position:relative;font-size:75%;line-height:0;vertical-align:baseline}
sup{top:-0.5em}
sub{bottom:-0.25em}
img{max-width:100%;width:auto\9;height:auto;vertical-align:middle;border:0;-ms-interpolation-mode:bicubic}
#map_canvas img,.google-maps img{max-width:none}
button,input,select,textarea{margin:0;font-size:100%;vertical-align:middle}
button,input{*overflow:visible;line-height:normal}
button::-moz-focus-inner,input::-moz-focus-inner{padding:0;border:0}
button,html input[type="button"],input[type="reset"],input[type="submit"]{-webkit-appearance:button;cursor:pointer}
label,select,button,input[type="button"],input[type="reset"],input[type="submit"],input[type="radio"],input[type="checkbox"]{cursor:pointer}
input[type="search"]{-webkit-box-sizing:content-box;-moz-box-sizing:content-box;box-sizing:content-box;-webkit-appearance:textfield}
input[type="search"]::-webkit-search-decoration,input[type="search"]::-webkit-search-cancel-button{-webkit-appearance:none}
textarea{overflow:auto;vertical-align:top}
@media print{*{text-shadow:none !important;color:#000 !important;background:transparent !important;box-shadow:none !important} a,a:visited{text-decoration:underline} a[href]:after{content:" (" attr(href) ")"} abbr[title]:after{content:" (" attr(title) ")"} .ir a:after,a[href^="javascript:"]:after,a[href^="#"]:after{content:""} pre,blockquote{border:1px solid #999;page-break-inside:avoid} thead{display:table-header-group} tr,img{page-break-inside:avoid} img{max-width:100% !important} @page {margin:.5cm}p,h2,h3{orphans:3;widows:3} h2,h3{page-break-after:avoid}}body{margin:0;font-family:"Helvetica Neue",Helvetica,Arial,sans-serif;font-size:13px;line-height:20px;color:#000;background-color:#fff}
a{color:#08c;text-decoration:none}
a:hover,a:focus{color:#005580;text-decoration:underline}
.img-rounded{border-radius:6px;-webkit-border-radius:6px;-moz-border-radius:6px;border-radius:6px}
.img-polaroid{padding:4px;background-color:#fff;border:1px solid #ccc;border:1px solid rgba(0,0,0,0.2);-webkit-box-shadow:0 1px 3px rgba(0,0,0,0.1);-moz-box-shadow:0 1px 3px rgba(0,0,0,0.1);box-shadow:0 1px 3px rgba(0,0,0,0.1)}
.img-circle{border-radius:500px;-webkit-border-radius:500px;-moz-border-radius:500px;border-radius:500px}
.row{margin-left:-20px;*zoom:1}.row:before,.row:after{display:table;content:"";line-height:0}
.row:after{clear:both}
[class*="span"]{float:left;min-height:1px;margin-left:20px}
.container,.navbar-static-top .container,.navbar-fixed-top .container,.navbar-fixed-bottom .container{width:940px}
.span12{width:940px}
.span11{width:860px}
.span10{width:780px}
.span9{width:700px}
.span8{width:620px}
.span7{width:540px}
.span6{width:460px}
.span5{width:380px}
.span4{width:300px}
.span3{width:220px}
.span2{width:140px}
.span1{width:60px}
.offset12{margin-left:980px}
.offset11{margin-left:900px}
.offset10{margin-left:820px}
.offset9{margin-left:740px}
.offset8{margin-left:660px}
.offset7{margin-left:580px}
.offset6{margin-left:500px}
.offset5{margin-left:420px}
.offset4{margin-left:340px}
.offset3{margin-left:260px}
.offset2{margin-left:180px}
.offset1{margin-left:100px}
.row-fluid{width:100%;*zoom:1}.row-fluid:before,.row-fluid:after{display:table;content:"";line-height:0}
.row-fluid:after{clear:both}
.row-fluid [class*="span"]{display:block;width:100%;min-height:30px;-webkit-box-sizing:border-box;-moz-box-sizing:border-box;box-sizing:border-box;float:left;margin-left:2.127659574468085%;*margin-left:2.074468085106383%}
.row-fluid [class*="span"]:first-child{margin-left:0}
.row-fluid .controls-row [class*="span"]+[class*="span"]{margin-left:2.127659574468085%}
.row-fluid .span12{width:100%;*width:99.94680851063829%}
.row-fluid .span11{width:91.48936170212765%;*width:91.43617021276594%}
.row-fluid .span10{width:82.97872340425532%;*width:82.92553191489361%}
.row-fluid .span9{width:74.46808510638297%;*width:74.41489361702126%}
.row-fluid .span8{width:65.95744680851064%;*width:65.90425531914893%}
.row-fluid .span7{width:57.44680851063829%;*width:57.39361702127659%}
.row-fluid .span6{width:48.93617021276595%;*width:48.88297872340425%}
.row-fluid .span5{width:40.42553191489362%;*width:40.37234042553192%}
.row-fluid .span4{width:31.914893617021278%;*width:31.861702127659576%}
.row-fluid .span3{width:23.404255319148934%;*width:23.351063829787233%}
.row-fluid .span2{width:14.893617021276595%;*width:14.840425531914894%}
.row-fluid .span1{width:6.382978723404255%;*width:6.329787234042553%}
.row-fluid .offset12{margin-left:104.25531914893617%;*margin-left:104.14893617021275%}
.row-fluid .offset12:first-child{margin-left:102.12765957446808%;*margin-left:102.02127659574467%}
.row-fluid .offset11{margin-left:95.74468085106382%;*margin-left:95.6382978723404%}
.row-fluid .offset11:first-child{margin-left:93.61702127659574%;*margin-left:93.51063829787232%}
.row-fluid .offset10{margin-left:87.23404255319149%;*margin-left:87.12765957446807%}
.row-fluid .offset10:first-child{margin-left:85.1063829787234%;*margin-left:84.99999999999999%}
.row-fluid .offset9{margin-left:78.72340425531914%;*margin-left:78.61702127659572%}
.row-fluid .offset9:first-child{margin-left:76.59574468085106%;*margin-left:76.48936170212764%}
.row-fluid .offset8{margin-left:70.2127659574468%;*margin-left:70.10638297872339%}
.row-fluid .offset8:first-child{margin-left:68.08510638297872%;*margin-left:67.9787234042553%}
.row-fluid .offset7{margin-left:61.70212765957446%;*margin-left:61.59574468085106%}
.row-fluid .offset7:first-child{margin-left:59.574468085106375%;*margin-left:59.46808510638297%}
.row-fluid .offset6{margin-left:53.191489361702125%;*margin-left:53.085106382978715%}
.row-fluid .offset6:first-child{margin-left:51.063829787234035%;*margin-left:50.95744680851063%}
.row-fluid .offset5{margin-left:44.68085106382979%;*margin-left:44.57446808510638%}
.row-fluid .offset5:first-child{margin-left:42.5531914893617%;*margin-left:42.4468085106383%}
.row-fluid .offset4{margin-left:36.170212765957444%;*margin-left:36.06382978723405%}
.row-fluid .offset4:first-child{margin-left:34.04255319148936%;*margin-left:33.93617021276596%}
.row-fluid .offset3{margin-left:27.659574468085104%;*margin-left:27.5531914893617%}
.row-fluid .offset3:first-child{margin-left:25.53191489361702%;*margin-left:25.425531914893618%}
.row-fluid .offset2{margin-left:19.148936170212764%;*margin-left:19.04255319148936%}
.row-fluid .offset2:first-child{margin-left:17.02127659574468%;*margin-left:16.914893617021278%}
.row-fluid .offset1{margin-left:10.638297872340425%;*margin-left:10.53191489361702%}
.row-fluid .offset1:first-child{margin-left:8.51063829787234%;*margin-left:8.404255319148938%}
[class*="span"].hide,.row-fluid [class*="span"].hide{display:none}
[class*="span"].pull-right,.row-fluid [class*="span"].pull-right{float:right}
.container{margin-right:auto;margin-left:auto;*zoom:1}.container:before,.container:after{display:table;content:"";line-height:0}
.container:after{clear:both}
.container-fluid{padding-right:20px;padding-left:20px;*zoom:1}.container-fluid:before,.container-fluid:after{display:table;content:"";line-height:0}
.container-fluid:after{clear:both}
p{margin:0 0 10px}
.lead{margin-bottom:20px;font-size:19.5px;font-weight:200;line-height:30px}
small{font-size:85%}
strong{font-weight:bold}
em{font-style:italic}
cite{font-style:normal}
.muted{color:#999}
a.muted:hover,a.muted:focus{color:#808080}
.text-warning{color:#c09853}
a.text-warning:hover,a.text-warning:focus{color:#a47e3c}
.text-error{color:#b94a48}
a.text-error:hover,a.text-error:focus{color:#953b39}
.text-info{color:#3a87ad}
a.text-info:hover,a.text-info:focus{color:#2d6987}
.text-success{color:#468847}
a.text-success:hover,a.text-success:focus{color:#356635}
.text-left{text-align:left}
.text-right{text-align:right}
.text-center{text-align:center}
h1,h2,h3,h4,h5,h6{margin:10px 0;font-family:inherit;font-weight:bold;line-height:20px;color:inherit;text-rendering:optimizelegibility}h1 small,h2 small,h3 small,h4 small,h5 small,h6 small{font-weight:normal;line-height:1;color:#999}
h1,h2,h3{line-height:40px}
h1{font-size:35.75px}
h2{font-size:29.25px}
h3{font-size:22.75px}
h4{font-size:16.25px}
h5{font-size:13px}
h6{font-size:11.049999999999999px}
h1 small{font-size:22.75px}
h2 small{font-size:16.25px}
h3 small{font-size:13px}
h4 small{font-size:13px}
.page-header{padding-bottom:9px;margin:20px 0 30px;border-bottom:1px solid #eee}
ul,ol{padding:0;margin:0 0 10px 25px}
ul ul,ul ol,ol ol,ol ul{margin-bottom:0}
li{line-height:20px}
ul.unstyled,ol.unstyled{margin-left:0;list-style:none}
ul.inline,ol.inline{margin-left:0;list-style:none}ul.inline>li,ol.inline>li{display:inline-block;*display:inline;*zoom:1;padding-left:5px;padding-right:5px}
dl{margin-bottom:20px}
dt,dd{line-height:20px}
dt{font-weight:bold}
dd{margin-left:10px}
.dl-horizontal{*zoom:1}.dl-horizontal:before,.dl-horizontal:after{display:table;content:"";line-height:0}
.dl-horizontal:after{clear:both}
.dl-horizontal dt{float:left;width:160px;clear:left;text-align:right;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.dl-horizontal dd{margin-left:180px}
hr{margin:20px 0;border:0;border-top:1px solid #eee;border-bottom:1px solid #fff}
abbr[title],abbr[data-original-title]{cursor:help;border-bottom:1px dotted #999}
abbr.initialism{font-size:90%;text-transform:uppercase}
blockquote{padding:0 0 0 15px;margin:0 0 20px;border-left:5px solid #eee}blockquote p{margin-bottom:0;font-size:16.25px;font-weight:300;line-height:1.25}
blockquote small{display:block;line-height:20px;color:#999}blockquote small:before{content:'\2014 \00A0'}
blockquote.pull-right{float:right;padding-right:15px;padding-left:0;border-right:5px solid #eee;border-left:0}blockquote.pull-right p,blockquote.pull-right small{text-align:right}
blockquote.pull-right small:before{content:''}
blockquote.pull-right small:after{content:'\00A0 \2014'}
q:before,q:after,blockquote:before,blockquote:after{content:""}
address{display:block;margin-bottom:20px;font-style:normal;line-height:20px}
code,pre{padding:0 3px 2px;font-family:monospace;font-size:11px;color:#333;border-radius:3px;-webkit-border-radius:3px;-moz-border-radius:3px;border-radius:3px}
code{padding:2px 4px;color:#d14;background-color:#f7f7f9;border:1px solid #e1e1e8;white-space:nowrap}
pre{display:block;padding:9.5px;margin:0 0 10px;font-size:12px;line-height:20px;word-break:break-all;word-wrap:break-word;white-space:pre;white-space:pre-wrap;background-color:#f5f5f5;border:1px solid #ccc;border:1px solid rgba(0,0,0,0.15);border-radius:4px;-webkit-border-radius:4px;-moz-border-radius:4px;border-radius:4px}pre.prettyprint{margin-bottom:20px}
pre code{padding:0;color:inherit;white-space:pre;white-space:pre-wrap;background-color:transparent;border:0}
.pre-scrollable{max-height:340px;overflow-y:scroll}
form{margin:0 0 20px}
fieldset{padding:0;margin:0;border:0}
legend{display:block;width:100%;padding:0;margin-bottom:20px;font-size:19.5px;line-height:40px;color:#333;border:0;border-bottom:1px solid #e5e5e5}legend small{font-size:15px;color:#999}
label,input,button,select,textarea{font-size:13px;font-weight:normal;line-height:20px}
input,button,select,textarea{font-family:"Helvetica Neue",Helvetica,Arial,sans-serif}
label{display:block;margin-bottom:5px}
select,textarea,input[type="text"],input[type="password"],input[type="datetime"],input[type="datetime-local"],input[type="date"],input[type="month"],input[type="time"],input[type="week"],input[type="number"],input[type="email"],input[type="url"],input[type="search"],input[type="tel"],input[type="color"],.uneditable-input{display:inline-block;height:20px;padding:4px 6px;margin-bottom:10px;font-size:13px;line-height:20px;color:#555;-webkit-border-radius:4px;-moz-border-radius:4px;border-radius:4px;vertical-align:middle}
input,textarea,.uneditable-input{width:206px}
textarea{height:auto}
textarea,input[type="text"],input[type="password"],input[type="datetime"],input[type="datetime-local"],input[type="date"],input[type="month"],input[type="time"],input[type="week"],input[type="number"],input[type="email"],input[type="url"],input[type="search"],input[type="tel"],input[type="color"],.uneditable-input{background-color:#fff;border:1px solid #ccc;-webkit-box-shadow:inset 0 1px 1px rgba(0,0,0,0.075);-moz-box-shadow:inset 0 1px 1px rgba(0,0,0,0.075);box-shadow:inset 0 1px 1px rgba(0,0,0,0.075);-webkit-transition:border linear .2s, box-shadow linear .2s;-moz-transition:border linear .2s, box-shadow linear .2s;-o-transition:border linear .2s, box-shadow linear .2s;transition:border linear .2s, box-shadow linear .2s}textarea:focus,input[type="text"]:focus,input[type="password"]:focus,input[type="datetime"]:focus,input[type="datetime-local"]:focus,input[type="date"]:focus,input[type="month"]:focus,input[type="time"]:focus,input[type="week"]:focus,input[type="number"]:focus,input[type="email"]:focus,input[type="url"]:focus,input[type="search"]:focus,input[type="tel"]:focus,input[type="color"]:focus,.uneditable-input:focus{border-color:rgba(82,168,236,0.8);outline:0;outline:thin dotted \9;-webkit-box-shadow:inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(82,168,236,.6);-moz-box-shadow:inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(82,168,236,.6);box-shadow:inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(82,168,236,.6)}
input[type="radio"],input[type="checkbox"]{margin:4px 0 0;*margin-top:0;margin-top:1px \9;line-height:normal}
input[type="file"],input[type="image"],input[type="submit"],input[type="reset"],input[type="button"],input[type="radio"],input[type="checkbox"]{width:auto}
select,input[type="file"]{height:30px;*margin-top:4px;line-height:30px}
select{width:220px;border:1px solid #ccc;background-color:#fff}
select[multiple],select[size]{height:auto}
select:focus,input[type="file"]:focus,input[type="radio"]:focus,input[type="checkbox"]:focus{outline:thin dotted #333;outline:5px auto -webkit-focus-ring-color;outline-offset:-2px}
.uneditable-input,.uneditable-textarea{color:#999;background-color:#fcfcfc;border-color:#ccc;-webkit-box-shadow:inset 0 1px 2px rgba(0,0,0,0.025);-moz-box-shadow:inset 0 1px 2px rgba(0,0,0,0.025);box-shadow:inset 0 1px 2px rgba(0,0,0,0.025);cursor:not-allowed}
.uneditable-input{overflow:hidden;white-space:nowrap}
.uneditable-textarea{width:auto;height:auto}
input:-moz-placeholder,textarea:-moz-placeholder{color:#999}
input:-ms-input-placeholder,textarea:-ms-input-placeholder{color:#999}
input::-webkit-input-placeholder,textarea::-webkit-input-placeholder{color:#999}
.radio,.checkbox{min-height:20px;padding-left:20px}
.radio input[type="radio"],.checkbox input[type="checkbox"]{float:left;margin-left:-20px}
.controls>.radio:first-child,.controls>.checkbox:first-child{padding-top:5px}
.radio.inline,.checkbox.inline{display:inline-block;padding-top:5px;margin-bottom:0;vertical-align:middle}
.radio.inline+.radio.inline,.checkbox.inline+.checkbox.inline{margin-left:10px}
.input-mini{width:60px}
.input-small{width:90px}
.input-medium{width:150px}
.input-large{width:210px}
.input-xlarge{width:270px}
.input-xxlarge{width:530px}
input[class*="span"],select[class*="span"],textarea[class*="span"],.uneditable-input[class*="span"],.row-fluid input[class*="span"],.row-fluid select[class*="span"],.row-fluid textarea[class*="span"],.row-fluid .uneditable-input[class*="span"]{float:none;margin-left:0}
.input-append input[class*="span"],.input-append .uneditable-input[class*="span"],.input-prepend input[class*="span"],.input-prepend .uneditable-input[class*="span"],.row-fluid input[class*="span"],.row-fluid select[class*="span"],.row-fluid textarea[class*="span"],.row-fluid .uneditable-input[class*="span"],.row-fluid .input-prepend [class*="span"],.row-fluid .input-append [class*="span"]{display:inline-block}
input,textarea,.uneditable-input{margin-left:0}
.controls-row [class*="span"]+[class*="span"]{margin-left:20px}
input.span12,textarea.span12,.uneditable-input.span12{width:926px}
input.span11,textarea.span11,.uneditable-input.span11{width:846px}
input.span10,textarea.span10,.uneditable-input.span10{width:766px}
input.span9,textarea.span9,.uneditable-input.span9{width:686px}
input.span8,textarea.span8,.uneditable-input.span8{width:606px}
input.span7,textarea.span7,.uneditable-input.span7{width:526px}
input.span6,textarea.span6,.uneditable-input.span6{width:446px}
input.span5,textarea.span5,.uneditable-input.span5{width:366px}
input.span4,textarea.span4,.uneditable-input.span4{width:286px}
input.span3,textarea.span3,.uneditable-input.span3{width:206px}
input.span2,textarea.span2,.uneditable-input.span2{width:126px}
input.span1,textarea.span1,.uneditable-input.span1{width:46px}
.controls-row{*zoom:1}.controls-row:before,.controls-row:after{display:table;content:"";line-height:0}
.controls-row:after{clear:both}
.controls-row [class*="span"],.row-fluid .controls-row [class*="span"]{float:left}
.controls-row .checkbox[class*="span"],.controls-row .radio[class*="span"]{padding-top:5px}
input[disabled],select[disabled],textarea[disabled],input[readonly],select[readonly],textarea[readonly]{cursor:not-allowed;background-color:#eee}
input[type="radio"][disabled],input[type="checkbox"][disabled],input[type="radio"][readonly],input[type="checkbox"][readonly]{background-color:transparent}
.control-group.warning .control-label,.control-group.warning .help-block,.control-group.warning .help-inline{color:#c09853}
.control-group.warning .checkbox,.control-group.warning .radio,.control-group.warning input,.control-group.warning select,.control-group.warning textarea{color:#c09853}
.control-group.warning input,.control-group.warning select,.control-group.warning textarea{border-color:#c09853;-webkit-box-shadow:inset 0 1px 1px rgba(0,0,0,0.075);-moz-box-shadow:inset 0 1px 1px rgba(0,0,0,0.075);box-shadow:inset 0 1px 1px rgba(0,0,0,0.075)}.control-group.warning input:focus,.control-group.warning select:focus,.control-group.warning textarea:focus{border-color:#a47e3c;-webkit-box-shadow:inset 0 1px 1px rgba(0,0,0,0.075),0 0 6px #dbc59e;-moz-box-shadow:inset 0 1px 1px rgba(0,0,0,0.075),0 0 6px #dbc59e;box-shadow:inset 0 1px 1px rgba(0,0,0,0.075),0 0 6px #dbc59e}
.control-group.warning .input-prepend .add-on,.control-group.warning .input-append .add-on{color:#c09853;background-color:#fcf8e3;border-color:#c09853}
.control-group.error .control-label,.control-group.error .help-block,.control-group.error .help-inline{color:#b94a48}
.control-group.error .checkbox,.control-group.error .radio,.control-group.error input,.control-group.error select,.control-group.error textarea{color:#b94a48}
.control-group.error input,.control-group.error select,.control-group.error textarea{border-color:#b94a48;-webkit-box-shadow:inset 0 1px 1px rgba(0,0,0,0.075);-moz-box-shadow:inset 0 1px 1px rgba(0,0,0,0.075);box-shadow:inset 0 1px 1px rgba(0,0,0,0.075)}.control-group.error input:focus,.control-group.error select:focus,.control-group.error textarea:focus{border-color:#953b39;-webkit-box-shadow:inset 0 1px 1px rgba(0,0,0,0.075),0 0 6px #d59392;-moz-box-shadow:inset 0 1px 1px rgba(0,0,0,0.075),0 0 6px #d59392;box-shadow:inset 0 1px 1px rgba(0,0,0,0.075),0 0 6px #d59392}
.control-group.error .input-prepend .add-on,.control-group.error .input-append .add-on{color:#b94a48;background-color:#f2dede;border-color:#b94a48}
.control-group.success .control-label,.control-group.success .help-block,.control-group.success .help-inline{color:#468847}
.control-group.success .checkbox,.control-group.success .radio,.control-group.success input,.control-group.success select,.control-group.success textarea{color:#468847}
.control-group.success input,.control-group.success select,.control-group.success textarea{border-color:#468847;-webkit-box-shadow:inset 0 1px 1px rgba(0,0,0,0.075);-moz-box-shadow:inset 0 1px 1px rgba(0,0,0,0.075);box-shadow:inset 0 1px 1px rgba(0,0,0,0.075)}.control-group.success input:focus,.control-group.success select:focus,.control-group.success textarea:focus{border-color:#356635;-webkit-box-shadow:inset 0 1px 1px rgba(0,0,0,0.075),0 0 6px #7aba7b;-moz-box-shadow:inset 0 1px 1px rgba(0,0,0,0.075),0 0 6px #7aba7b;box-shadow:inset 0 1px 1px rgba(0,0,0,0.075),0 0 6px #7aba7b}
.control-group.success .input-prepend .add-on,.control-group.success .input-append .add-on{color:#468847;background-color:#dff0d8;border-color:#468847}
.control-group.info .control-label,.control-group.info .help-block,.control-group.info .help-inline{color:#3a87ad}
.control-group.info .checkbox,.control-group.info .radio,.control-group.info input,.control-group.info select,.control-group.info textarea{color:#3a87ad}
.control-group.info input,.control-group.info select,.control-group.info textarea{border-color:#3a87ad;-webkit-box-shadow:inset 0 1px 1px rgba(0,0,0,0.075);-moz-box-shadow:inset 0 1px 1px rgba(0,0,0,0.075);box-shadow:inset 0 1px 1px rgba(0,0,0,0.075)}.control-group.info input:focus,.control-group.info select:focus,.control-group.info textarea:focus{border-color:#2d6987;-webkit-box-shadow:inset 0 1px 1px rgba(0,0,0,0.075),0 0 6px #7ab5d3;-moz-box-shadow:inset 0 1px 1px rgba(0,0,0,0.075),0 0 6px #7ab5d3;box-shadow:inset 0 1px 1px rgba(0,0,0,0.075),0 0 6px #7ab5d3}
.control-group.info .input-prepend .add-on,.control-group.info .input-append .add-on{color:#3a87ad;background-color:#d9edf7;border-color:#3a87ad}
input:focus:invalid,textarea:focus:invalid,select:focus:invalid{color:#b94a48;border-color:#ee5f5b}input:focus:invalid:focus,textarea:focus:invalid:focus,select:focus:invalid:focus{border-color:#e9322d;-webkit-box-shadow:0 0 6px #f8b9b7;-moz-box-shadow:0 0 6px #f8b9b7;box-shadow:0 0 6px #f8b9b7}
.form-actions{padding:19px 20px 20px;margin-top:20px;margin-bottom:20px;background-color:#f5f5f5;border-top:1px solid #e5e5e5;*zoom:1}.form-actions:before,.form-actions:after{display:table;content:"";line-height:0}
.form-actions:after{clear:both}
.help-block,.help-inline{color:#262626}
.help-block{display:block;margin-bottom:10px}
.help-inline{display:inline-block;*display:inline;*zoom:1;vertical-align:middle;padding-left:5px}
.input-append,.input-prepend{display:inline-block;margin-bottom:10px;vertical-align:middle;font-size:0;white-space:nowrap}.input-append input,.input-prepend input,.input-append select,.input-prepend select,.input-append .uneditable-input,.input-prepend .uneditable-input,.input-append .dropdown-menu,.input-prepend .dropdown-menu,.input-append .popover,.input-prepend .popover{font-size:13px}
.input-append input,.input-prepend input,.input-append select,.input-prepend select,.input-append .uneditable-input,.input-prepend .uneditable-input{position:relative;margin-bottom:0;*margin-left:0;vertical-align:top;border-radius:0 4px 4px 0;-webkit-border-radius:0 4px 4px 0;-moz-border-radius:0 4px 4px 0;border-radius:0 4px 4px 0}.input-append input:focus,.input-prepend input:focus,.input-append select:focus,.input-prepend select:focus,.input-append .uneditable-input:focus,.input-prepend .uneditable-input:focus{z-index:2}
.input-append .add-on,.input-prepend .add-on{display:inline-block;width:auto;height:20px;min-width:16px;padding:4px 5px;font-size:13px;font-weight:normal;line-height:20px;text-align:center;text-shadow:0 1px 0 #fff;background-color:#eee;border:1px solid #ccc}
.input-append .add-on,.input-prepend .add-on,.input-append .btn,.input-prepend .btn,.input-append .btn-group>.dropdown-toggle,.input-prepend .btn-group>.dropdown-toggle{vertical-align:top;border-radius:0;-webkit-border-radius:0;-moz-border-radius:0;border-radius:0}
.input-append .active,.input-prepend .active{background-color:#a9dba9;border-color:#46a546}
.input-prepend .add-on,.input-prepend .btn{margin-right:-1px}
.input-prepend .add-on:first-child,.input-prepend .btn:first-child{border-radius:4px 0 0 4px;-webkit-border-radius:4px 0 0 4px;-moz-border-radius:4px 0 0 4px;border-radius:4px 0 0 4px}
.input-append input,.input-append select,.input-append .uneditable-input{border-radius:4px 0 0 4px;-webkit-border-radius:4px 0 0 4px;-moz-border-radius:4px 0 0 4px;border-radius:4px 0 0 4px}.input-append input+.btn-group .btn:last-child,.input-append select+.btn-group .btn:last-child,.input-append .uneditable-input+.btn-group .btn:last-child{border-radius:0 4px 4px 0;-webkit-border-radius:0 4px 4px 0;-moz-border-radius:0 4px 4px 0;border-radius:0 4px 4px 0}
.input-append .add-on,.input-append .btn,.input-append .btn-group{margin-left:-1px}
.input-append .add-on:last-child,.input-append .btn:last-child,.input-append .btn-group:last-child>.dropdown-toggle{border-radius:0 4px 4px 0;-webkit-border-radius:0 4px 4px 0;-moz-border-radius:0 4px 4px 0;border-radius:0 4px 4px 0}
.input-prepend.input-append input,.input-prepend.input-append select,.input-prepend.input-append .uneditable-input{border-radius:0;-webkit-border-radius:0;-moz-border-radius:0;border-radius:0}.input-prepend.input-append input+.btn-group .btn,.input-prepend.input-append select+.btn-group .btn,.input-prepend.input-append .uneditable-input+.btn-group .btn{border-radius:0 4px 4px 0;-webkit-border-radius:0 4px 4px 0;-moz-border-radius:0 4px 4px 0;border-radius:0 4px 4px 0}
.input-prepend.input-append .add-on:first-child,.input-prepend.input-append .btn:first-child{margin-right:-1px;border-radius:4px 0 0 4px;-webkit-border-radius:4px 0 0 4px;-moz-border-radius:4px 0 0 4px;border-radius:4px 0 0 4px}
.input-prepend.input-append .add-on:last-child,.input-prepend.input-append .btn:last-child{margin-left:-1px;border-radius:0 4px 4px 0;-webkit-border-radius:0 4px 4px 0;-moz-border-radius:0 4px 4px 0;border-radius:0 4px 4px 0}
.input-prepend.input-append .btn-group:first-child{margin-left:0}
input.search-query{padding-right:14px;padding-right:4px \9;padding-left:14px;padding-left:4px \9;margin-bottom:0;border-radius:15px;-webkit-border-radius:15px;-moz-border-radius:15px;border-radius:15px}
.form-search .input-append .search-query,.form-search .input-prepend .search-query{border-radius:0;-webkit-border-radius:0;-moz-border-radius:0;border-radius:0}
.form-search .input-append .search-query{border-radius:14px 0 0 14px;-webkit-border-radius:14px 0 0 14px;-moz-border-radius:14px 0 0 14px;border-radius:14px 0 0 14px}
.form-search .input-append .btn{border-radius:0 14px 14px 0;-webkit-border-radius:0 14px 14px 0;-moz-border-radius:0 14px 14px 0;border-radius:0 14px 14px 0}
.form-search .input-prepend .search-query{border-radius:0 14px 14px 0;-webkit-border-radius:0 14px 14px 0;-moz-border-radius:0 14px 14px 0;border-radius:0 14px 14px 0}
.form-search .input-prepend .btn{border-radius:14px 0 0 14px;-webkit-border-radius:14px 0 0 14px;-moz-border-radius:14px 0 0 14px;border-radius:14px 0 0 14px}
.form-search input,.form-inline input,.form-horizontal input,.form-search textarea,.form-inline textarea,.form-horizontal textarea,.form-search select,.form-inline select,.form-horizontal select,.form-search .help-inline,.form-inline .help-inline,.form-horizontal .help-inline,.form-search .uneditable-input,.form-inline .uneditable-input,.form-horizontal .uneditable-input,.form-search .input-prepend,.form-inline .input-prepend,.form-horizontal .input-prepend,.form-search .input-append,.form-inline .input-append,.form-horizontal .input-append{display:inline-block;*display:inline;*zoom:1;margin-bottom:0;vertical-align:middle}
.form-search .hide,.form-inline .hide,.form-horizontal .hide{display:none}
.form-search label,.form-inline label,.form-search .btn-group,.form-inline .btn-group{display:inline-block}
.form-search .input-append,.form-inline .input-append,.form-search .input-prepend,.form-inline .input-prepend{margin-bottom:0}
.form-search .radio,.form-search .checkbox,.form-inline .radio,.form-inline .checkbox{padding-left:0;margin-bottom:0;vertical-align:middle}
.form-search .radio input[type="radio"],.form-search .checkbox input[type="checkbox"],.form-inline .radio input[type="radio"],.form-inline .checkbox input[type="checkbox"]{float:left;margin-right:3px;margin-left:0}
.control-group{margin-bottom:10px}
legend+.control-group{margin-top:20px;-webkit-margin-top-collapse:separate}
.form-horizontal .control-group{margin-bottom:20px;*zoom:1}.form-horizontal .control-group:before,.form-horizontal .control-group:after{display:table;content:"";line-height:0}
.form-horizontal .control-group:after{clear:both}
.form-horizontal .control-label{float:left;width:160px;padding-top:5px;text-align:right}
.form-horizontal .controls{*display:inline-block;*padding-left:20px;margin-left:180px;*margin-left:0}.form-horizontal .controls:first-child{*padding-left:180px}
.form-horizontal .help-block{margin-bottom:0}
.form-horizontal input+.help-block,.form-horizontal select+.help-block,.form-horizontal textarea+.help-block,.form-horizontal .uneditable-input+.help-block,.form-horizontal .input-prepend+.help-block,.form-horizontal .input-append+.help-block{margin-top:10px}
.form-horizontal .form-actions{padding-left:180px}
table{max-width:100%;background-color:transparent;border-collapse:collapse;border-spacing:0}
.table{width:100%;margin-bottom:20px}.table th,.table td{padding:8px;line-height:20px;text-align:left;vertical-align:top;border-top:1px solid #ddd}
.table th{font-weight:bold}
.table thead th{vertical-align:bottom}
.table caption+thead tr:first-child th,.table caption+thead tr:first-child td,.table colgroup+thead tr:first-child th,.table colgroup+thead tr:first-child td,.table thead:first-child tr:first-child th,.table thead:first-child tr:first-child td{border-top:0}
.table tbody+tbody{border-top:2px solid #ddd}
.table .table{background-color:#fff}
.table-condensed th,.table-condensed td{padding:4px 5px}
.table-bordered{border:1px solid #ddd;border-collapse:separate;*border-collapse:collapse;border-left:0;border-radius:4px;-webkit-border-radius:4px;-moz-border-radius:4px;border-radius:4px}.table-bordered th,.table-bordered td{border-left:1px solid #ddd}
.table-bordered caption+thead tr:first-child th,.table-bordered caption+tbody tr:first-child th,.table-bordered caption+tbody tr:first-child td,.table-bordered colgroup+thead tr:first-child th,.table-bordered colgroup+tbody tr:first-child th,.table-bordered colgroup+tbody tr:first-child td,.table-bordered thead:first-child tr:first-child th,.table-bordered tbody:first-child tr:first-child th,.table-bordered tbody:first-child tr:first-child td{border-top:0}
.table-bordered thead:first-child tr:first-child>th:first-child,.table-bordered tbody:first-child tr:first-child>td:first-child,.table-bordered tbody:first-child tr:first-child>th:first-child{-webkit-border-top-left-radius:4px;-moz-border-radius-topleft:4px;border-top-left-radius:4px}
.table-bordered thead:first-child tr:first-child>th:last-child,.table-bordered tbody:first-child tr:first-child>td:last-child,.table-bordered tbody:first-child tr:first-child>th:last-child{-webkit-border-top-right-radius:4px;-moz-border-radius-topright:4px;border-top-right-radius:4px}
.table-bordered thead:last-child tr:last-child>th:first-child,.table-bordered tbody:last-child tr:last-child>td:first-child,.table-bordered tbody:last-child tr:last-child>th:first-child,.table-bordered tfoot:last-child tr:last-child>td:first-child,.table-bordered tfoot:last-child tr:last-child>th:first-child{-webkit-border-bottom-left-radius:4px;-moz-border-radius-bottomleft:4px;border-bottom-left-radius:4px}
.table-bordered thead:last-child tr:last-child>th:last-child,.table-bordered tbody:last-child tr:last-child>td:last-child,.table-bordered tbody:last-child tr:last-child>th:last-child,.table-bordered tfoot:last-child tr:last-child>td:last-child,.table-bordered tfoot:last-child tr:last-child>th:last-child{-webkit-border-bottom-right-radius:4px;-moz-border-radius-bottomright:4px;border-bottom-right-radius:4px}
.table-bordered tfoot+tbody:last-child tr:last-child td:first-child{-webkit-border-bottom-left-radius:0;-moz-border-radius-bottomleft:0;border-bottom-left-radius:0}
.table-bordered tfoot+tbody:last-child tr:last-child td:last-child{-webkit-border-bottom-right-radius:0;-moz-border-radius-bottomright:0;border-bottom-right-radius:0}
.table-bordered caption+thead tr:first-child th:first-child,.table-bordered caption+tbody tr:first-child td:first-child,.table-bordered colgroup+thead tr:first-child th:first-child,.table-bordered colgroup+tbody tr:first-child td:first-child{-webkit-border-top-left-radius:4px;-moz-border-radius-topleft:4px;border-top-left-radius:4px}
.table-bordered caption+thead tr:first-child th:last-child,.table-bordered caption+tbody tr:first-child td:last-child,.table-bordered colgroup+thead tr:first-child th:last-child,.table-bordered colgroup+tbody tr:first-child td:last-child{-webkit-border-top-right-radius:4px;-moz-border-radius-topright:4px;border-top-right-radius:4px}
.table-striped tbody>tr:nth-child(odd)>td,.table-striped tbody>tr:nth-child(odd)>th{background-color:#f9f9f9}
.table-hover tbody tr:hover>td,.table-hover tbody tr:hover>th{background-color:#f5f5f5}
table td[class*="span"],table th[class*="span"],.row-fluid table td[class*="span"],.row-fluid table th[class*="span"]{display:table-cell;float:none;margin-left:0}
.table td.span1,.table th.span1{float:none;width:44px;margin-left:0}
.table td.span2,.table th.span2{float:none;width:124px;margin-left:0}
.table td.span3,.table th.span3{float:none;width:204px;margin-left:0}
.table td.span4,.table th.span4{float:none;width:284px;margin-left:0}
.table td.span5,.table th.span5{float:none;width:364px;margin-left:0}
.table td.span6,.table th.span6{float:none;width:444px;margin-left:0}
.table td.span7,.table th.span7{float:none;width:524px;margin-left:0}
.table td.span8,.table th.span8{float:none;width:604px;margin-left:0}
.table td.span9,.table th.span9{float:none;width:684px;margin-left:0}
.table td.span10,.table th.span10{float:none;width:764px;margin-left:0}
.table td.span11,.table th.span11{float:none;width:844px;margin-left:0}
.table td.span12,.table th.span12{float:none;width:924px;margin-left:0}
.table tbody tr.success>td{background-color:#dff0d8}
.table tbody tr.error>td{background-color:#f2dede}
.table tbody tr.warning>td{background-color:#fcf8e3}
.table tbody tr.info>td{background-color:#d9edf7}
.table-hover tbody tr.success:hover>td{background-color:#d0e9c6}
.table-hover tbody tr.error:hover>td{background-color:#ebcccc}
.table-hover tbody tr.warning:hover>td{background-color:#faf2cc}
.table-hover tbody tr.info:hover>td{background-color:#c4e3f3}
[class^="icon-"],[class*=" icon-"]{display:inline-block;width:14px;height:14px;*margin-right:.3em;line-height:14px;vertical-align:text-top;background-image:url("../img/glyphicons-halflings.png");background-position:14px 14px;background-repeat:no-repeat;margin-top:1px}
.icon-white,.nav-pills>.active>a>[class^="icon-"],.nav-pills>.active>a>[class*=" icon-"],.nav-list>.active>a>[class^="icon-"],.nav-list>.active>a>[class*=" icon-"],.navbar-inverse .nav>.active>a>[class^="icon-"],.navbar-inverse .nav>.active>a>[class*=" icon-"],.dropdown-menu>li>a:hover>[class^="icon-"],.dropdown-menu>li>a:focus>[class^="icon-"],.dropdown-menu>li>a:hover>[class*=" icon-"],.dropdown-menu>li>a:focus>[class*=" icon-"],.dropdown-menu>.active>a>[class^="icon-"],.dropdown-menu>.active>a>[class*=" icon-"],.dropdown-submenu:hover>a>[class^="icon-"],.dropdown-submenu:focus>a>[class^="icon-"],.dropdown-submenu:hover>a>[class*=" icon-"],.dropdown-submenu:focus>a>[class*=" icon-"]{background-image:url("../img/glyphicons-halflings-white.png")}
.icon-glass{background-position:0 0}
.icon-music{background-position:-24px 0}
.icon-search{background-position:-48px 0}
.icon-envelope{background-position:-72px 0}
.icon-heart{background-position:-96px 0}
.icon-star{background-position:-120px 0}
.icon-star-empty{background-position:-144px 0}
.icon-user{background-position:-168px 0}
.icon-film{background-position:-192px 0}
.icon-th-large{background-position:-216px 0}
.icon-th{background-position:-240px 0}
.icon-th-list{background-position:-264px 0}
.icon-ok{background-position:-288px 0}
.icon-remove{background-position:-312px 0}
.icon-zoom-in{background-position:-336px 0}
.icon-zoom-out{background-position:-360px 0}
.icon-off{background-position:-384px 0}
.icon-signal{background-position:-408px 0}
.icon-cog{background-position:-432px 0}
.icon-trash{background-position:-456px 0}
.icon-home{background-position:0 -24px}
.icon-file{background-position:-24px -24px}
.icon-time{background-position:-48px -24px}
.icon-road{background-position:-72px -24px}
.icon-download-alt{background-position:-96px -24px}
.icon-download{background-position:-120px -24px}
.icon-upload{background-position:-144px -24px}
.icon-inbox{background-position:-168px -24px}
.icon-play-circle{background-position:-192px -24px}
.icon-repeat{background-position:-216px -24px}
.icon-refresh{background-position:-240px -24px}
.icon-list-alt{background-position:-264px -24px}
.icon-lock{background-position:-287px -24px}
.icon-flag{background-position:-312px -24px}
.icon-headphones{background-position:-336px -24px}
.icon-volume-off{background-position:-360px -24px}
.icon-volume-down{background-position:-384px -24px}
.icon-volume-up{background-position:-408px -24px}
.icon-qrcode{background-position:-432px -24px}
.icon-barcode{background-position:-456px -24px}
.icon-tag{background-position:0 -48px}
.icon-tags{background-position:-25px -48px}
.icon-book{background-position:-48px -48px}
.icon-bookmark{background-position:-72px -48px}
.icon-print{background-position:-96px -48px}
.icon-camera{background-position:-120px -48px}
.icon-font{background-position:-144px -48px}
.icon-bold{background-position:-167px -48px}
.icon-italic{background-position:-192px -48px}
.icon-text-height{background-position:-216px -48px}
.icon-text-width{background-position:-240px -48px}
.icon-align-left{background-position:-264px -48px}
.icon-align-center{background-position:-288px -48px}
.icon-align-right{background-position:-312px -48px}
.icon-align-justify{background-position:-336px -48px}
.icon-list{background-position:-360px -48px}
.icon-indent-left{background-position:-384px -48px}
.icon-indent-right{background-position:-408px -48px}
.icon-facetime-video{background-position:-432px -48px}
.icon-picture{background-position:-456px -48px}
.icon-pencil{background-position:0 -72px}
.icon-map-marker{background-position:-24px -72px}
.icon-adjust{background-position:-48px -72px}
.icon-tint{background-position:-72px -72px}
.icon-edit{background-position:-96px -72px}
.icon-share{background-position:-120px -72px}
.icon-check{background-position:-144px -72px}
.icon-move{background-position:-168px -72px}
.icon-step-backward{background-position:-192px -72px}
.icon-fast-backward{background-position:-216px -72px}
.icon-backward{background-position:-240px -72px}
.icon-play{background-position:-264px -72px}
.icon-pause{background-position:-288px -72px}
.icon-stop{background-position:-312px -72px}
.icon-forward{background-position:-336px -72px}
.icon-fast-forward{background-position:-360px -72px}
.icon-step-forward{background-position:-384px -72px}
.icon-eject{background-position:-408px -72px}
.icon-chevron-left{background-position:-432px -72px}
.icon-chevron-right{background-position:-456px -72px}
.icon-plus-sign{background-position:0 -96px}
.icon-minus-sign{background-position:-24px -96px}
.icon-remove-sign{background-position:-48px -96px}
.icon-ok-sign{background-position:-72px -96px}
.icon-question-sign{background-position:-96px -96px}
.icon-info-sign{background-position:-120px -96px}
.icon-screenshot{background-position:-144px -96px}
.icon-remove-circle{background-position:-168px -96px}
.icon-ok-circle{background-position:-192px -96px}
.icon-ban-circle{background-position:-216px -96px}
.icon-arrow-left{background-position:-240px -96px}
.icon-arrow-right{background-position:-264px -96px}
.icon-arrow-up{background-position:-289px -96px}
.icon-arrow-down{background-position:-312px -96px}
.icon-share-alt{background-position:-336px -96px}
.icon-resize-full{background-position:-360px -96px}
.icon-resize-small{background-position:-384px -96px}
.icon-plus{background-position:-408px -96px}
.icon-minus{background-position:-433px -96px}
.icon-asterisk{background-position:-456px -96px}
.icon-exclamation-sign{background-position:0 -120px}
.icon-gift{background-position:-24px -120px}
.icon-leaf{background-position:-48px -120px}
.icon-fire{background-position:-72px -120px}
.icon-eye-open{background-position:-96px -120px}
.icon-eye-close{background-position:-120px -120px}
.icon-warning-sign{background-position:-144px -120px}
.icon-plane{background-position:-168px -120px}
.icon-calendar{background-position:-192px -120px}
.icon-random{background-position:-216px -120px;width:16px}
.icon-comment{background-position:-240px -120px}
.icon-magnet{background-position:-264px -120px}
.icon-chevron-up{background-position:-288px -120px}
.icon-chevron-down{background-position:-313px -119px}
.icon-retweet{background-position:-336px -120px}
.icon-shopping-cart{background-position:-360px -120px}
.icon-folder-close{background-position:-384px -120px;width:16px}
.icon-folder-open{background-position:-408px -120px;width:16px}
.icon-resize-vertical{background-position:-432px -119px}
.icon-resize-horizontal{background-position:-456px -118px}
.icon-hdd{background-position:0 -144px}
.icon-bullhorn{background-position:-24px -144px}
.icon-bell{background-position:-48px -144px}
.icon-certificate{background-position:-72px -144px}
.icon-thumbs-up{background-position:-96px -144px}
.icon-thumbs-down{background-position:-120px -144px}
.icon-hand-right{background-position:-144px -144px}
.icon-hand-left{background-position:-168px -144px}
.icon-hand-up{background-position:-192px -144px}
.icon-hand-down{background-position:-216px -144px}
.icon-circle-arrow-right{background-position:-240px -144px}
.icon-circle-arrow-left{background-position:-264px -144px}
.icon-circle-arrow-up{background-position:-288px -144px}
.icon-circle-arrow-down{background-position:-312px -144px}
.icon-globe{background-position:-336px -144px}
.icon-wrench{background-position:-360px -144px}
.icon-tasks{background-position:-384px -144px}
.icon-filter{background-position:-408px -144px}
.icon-briefcase{background-position:-432px -144px}
.icon-fullscreen{background-position:-456px -144px}
.dropup,.dropdown{position:relative}
.dropdown-toggle{*margin-bottom:-3px}
.dropdown-toggle:active,.open .dropdown-toggle{outline:0}
.caret{display:inline-block;width:0;height:0;vertical-align:top;border-top:4px solid #000;border-right:4px solid transparent;border-left:4px solid transparent;content:""}
.dropdown .caret{margin-top:8px;margin-left:2px}
.dropdown-menu{position:absolute;top:100%;left:0;z-index:1000;display:none;float:left;min-width:160px;padding:5px 0;margin:2px 0 0;list-style:none;background-color:#fff;border:1px solid #ccc;border:1px solid rgba(0,0,0,0.2);*border-right-width:2px;*border-bottom-width:2px;-webkit-border-radius:6px;-moz-border-radius:6px;border-radius:6px;-webkit-box-shadow:0 5px 10px rgba(0,0,0,0.2);-moz-box-shadow:0 5px 10px rgba(0,0,0,0.2);box-shadow:0 5px 10px rgba(0,0,0,0.2);-webkit-background-clip:padding-box;-moz-background-clip:padding;background-clip:padding-box}.dropdown-menu.pull-right{right:0;left:auto}
.dropdown-menu .divider{*width:100%;height:1px;margin:9px 1px;*margin:-5px 0 5px;overflow:hidden;background-color:#e5e5e5;border-bottom:1px solid #fff}
.dropdown-menu>li>a{display:block;padding:3px 20px;clear:both;font-weight:normal;line-height:20px;color:#333;white-space:nowrap}
.dropdown-menu>li>a:hover,.dropdown-menu>li>a:focus,.dropdown-submenu:hover>a,.dropdown-submenu:focus>a{text-decoration:none;color:#fff;background-color:#0081c2;background-image:-moz-linear-gradient(top, #08c, #0077b3);background-image:-webkit-gradient(linear, 0 0, 0 100%, from(#08c), to(#0077b3));background-image:-webkit-linear-gradient(top, #08c, #0077b3);background-image:-o-linear-gradient(top, #08c, #0077b3);background-image:linear-gradient(to bottom, #08c, #0077b3);background-repeat:repeat-x;filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#ff0088cc', endColorstr='#ff0077b3', GradientType=0)}
.dropdown-menu>.active>a,.dropdown-menu>.active>a:hover,.dropdown-menu>.active>a:focus{color:#fff;text-decoration:none;outline:0;background-color:#0081c2;background-image:-moz-linear-gradient(top, #08c, #0077b3);background-image:-webkit-gradient(linear, 0 0, 0 100%, from(#08c), to(#0077b3));background-image:-webkit-linear-gradient(top, #08c, #0077b3);background-image:-o-linear-gradient(top, #08c, #0077b3);background-image:linear-gradient(to bottom, #08c, #0077b3);background-repeat:repeat-x;filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#ff0088cc', endColorstr='#ff0077b3', GradientType=0)}
.dropdown-menu>.disabled>a,.dropdown-menu>.disabled>a:hover,.dropdown-menu>.disabled>a:focus{color:#999}
.dropdown-menu>.disabled>a:hover,.dropdown-menu>.disabled>a:focus{text-decoration:none;background-color:transparent;background-image:none;filter:progid:DXImageTransform.Microsoft.gradient(enabled = false);cursor:default}
.open{*z-index:1000}.open>.dropdown-menu{display:block}
.dropdown-backdrop{position:fixed;left:0;right:0;bottom:0;top:0;z-index:990}
.pull-right>.dropdown-menu{right:0;left:auto}
.dropup .caret,.navbar-fixed-bottom .dropdown .caret{border-top:0;border-bottom:4px solid #000;content:""}
.dropup .dropdown-menu,.navbar-fixed-bottom .dropdown .dropdown-menu{top:auto;bottom:100%;margin-bottom:1px}
.dropdown-submenu{position:relative}
.dropdown-submenu>.dropdown-menu{top:0;left:100%;margin-top:-6px;margin-left:-1px;border-radius:0 6px 6px 6px;-webkit-border-radius:0 6px 6px 6px;-moz-border-radius:0 6px 6px 6px;border-radius:0 6px 6px 6px}
.dropdown-submenu:hover>.dropdown-menu{display:block}
.dropup .dropdown-submenu>.dropdown-menu{top:auto;bottom:0;margin-top:0;margin-bottom:-2px;border-radius:5px 5px 5px 0;-webkit-border-radius:5px 5px 5px 0;-moz-border-radius:5px 5px 5px 0;border-radius:5px 5px 5px 0}
.dropdown-submenu>a:after{display:block;content:" ";float:right;width:0;height:0;border-color:transparent;border-style:solid;border-width:5px 0 5px 5px;border-left-color:#ccc;margin-top:5px;margin-right:-10px}
.dropdown-submenu:hover>a:after{border-left-color:#fff}
.dropdown-submenu.pull-left{float:none}.dropdown-submenu.pull-left>.dropdown-menu{left:-100%;margin-left:10px;border-radius:6px 0 6px 6px;-webkit-border-radius:6px 0 6px 6px;-moz-border-radius:6px 0 6px 6px;border-radius:6px 0 6px 6px}
.dropdown .dropdown-menu .nav-header{padding-left:20px;padding-right:20px}
.typeahead{z-index:1051;margin-top:2px;border-radius:4px;-webkit-border-radius:4px;-moz-border-radius:4px;border-radius:4px}
.well{min-height:20px;padding:19px;margin-bottom:20px;background-color:#f5f5f5;border:1px solid #e3e3e3;-webkit-border-radius:4px;-moz-border-radius:4px;border-radius:4px;-webkit-box-shadow:inset 0 1px 1px rgba(0,0,0,0.05);-moz-box-shadow:inset 0 1px 1px rgba(0,0,0,0.05);box-shadow:inset 0 1px 1px rgba(0,0,0,0.05)}.well blockquote{border-color:#ddd;border-color:rgba(0,0,0,0.15)}
.well-large{padding:24px;border-radius:6px;-webkit-border-radius:6px;-moz-border-radius:6px;border-radius:6px}
.well-small{padding:9px;border-radius:3px;-webkit-border-radius:3px;-moz-border-radius:3px;border-radius:3px}
.fade{opacity:0;-webkit-transition:opacity .15s linear;-moz-transition:opacity .15s linear;-o-transition:opacity .15s linear;transition:opacity .15s linear}.fade.in{opacity:1}
.collapse{position:relative;height:0;overflow:hidden;-webkit-transition:height .35s ease;-moz-transition:height .35s ease;-o-transition:height .35s ease;transition:height .35s ease}.collapse.in{height:auto}
.close{float:right;font-size:20px;font-weight:bold;line-height:20px;color:#000;text-shadow:0 1px 0 #fff;opacity:.2;filter:alpha(opacity=20)}.close:hover,.close:focus{color:#000;text-decoration:none;cursor:pointer;opacity:.4;filter:alpha(opacity=40)}
button.close{padding:0;cursor:pointer;background:transparent;border:0;-webkit-appearance:none}
.btn{display:inline-block;*display:inline;*zoom:1;padding:4px 12px;margin-bottom:0;font-size:13px;line-height:20px;text-align:center;vertical-align:middle;cursor:pointer;color:#333;text-shadow:0 1px 1px rgba(255,255,255,0.75);background-color:#f5f5f5;background-image:-moz-linear-gradient(top, #fff, #e6e6e6);background-image:-webkit-gradient(linear, 0 0, 0 100%, from(#fff), to(#e6e6e6));background-image:-webkit-linear-gradient(top, #fff, #e6e6e6);background-image:-o-linear-gradient(top, #fff, #e6e6e6);background-image:linear-gradient(to bottom, #fff, #e6e6e6);background-repeat:repeat-x;filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#ffffffff', endColorstr='#ffe6e6e6', GradientType=0);border-color:#e6e6e6 #e6e6e6 #bfbfbf;border-color:rgba(0,0,0,0.1) rgba(0,0,0,0.1) rgba(0,0,0,0.25);*background-color:#e6e6e6;filter:progid:DXImageTransform.Microsoft.gradient(enabled = false);border:1px solid #ccc;*border:0;border-bottom-color:#b3b3b3;-webkit-border-radius:4px;-moz-border-radius:4px;border-radius:4px;*margin-left:.3em;-webkit-box-shadow:inset 0 1px 0 rgba(255,255,255,.2), 0 1px 2px rgba(0,0,0,.05);-moz-box-shadow:inset 0 1px 0 rgba(255,255,255,.2), 0 1px 2px rgba(0,0,0,.05);box-shadow:inset 0 1px 0 rgba(255,255,255,.2), 0 1px 2px rgba(0,0,0,.05)}.btn:hover,.btn:focus,.btn:active,.btn.active,.btn.disabled,.btn[disabled]{color:#333;background-color:#e6e6e6;*background-color:#d9d9d9}
.btn:active,.btn.active{background-color:#ccc \9}
.btn:first-child{*margin-left:0}
.btn:hover,.btn:focus{color:#333;text-decoration:none;background-position:0 -15px;-webkit-transition:background-position .1s linear;-moz-transition:background-position .1s linear;-o-transition:background-position .1s linear;transition:background-position .1s linear}
.btn:focus{outline:thin dotted #333;outline:5px auto -webkit-focus-ring-color;outline-offset:-2px}
.btn.active,.btn:active{background-image:none;outline:0;-webkit-box-shadow:inset 0 2px 4px rgba(0,0,0,.15), 0 1px 2px rgba(0,0,0,.05);-moz-box-shadow:inset 0 2px 4px rgba(0,0,0,.15), 0 1px 2px rgba(0,0,0,.05);box-shadow:inset 0 2px 4px rgba(0,0,0,.15), 0 1px 2px rgba(0,0,0,.05)}
.btn.disabled,.btn[disabled]{cursor:default;background-image:none;opacity:.65;filter:alpha(opacity=65);-webkit-box-shadow:none;-moz-box-shadow:none;box-shadow:none}
.btn-large{padding:11px 19px;font-size:16.25px;border-radius:6px;-webkit-border-radius:6px;-moz-border-radius:6px;border-radius:6px}
.btn-large [class^="icon-"],.btn-large [class*=" icon-"]{margin-top:4px}
.btn-small{padding:2px 10px;font-size:11.049999999999999px;border-radius:3px;-webkit-border-radius:3px;-moz-border-radius:3px;border-radius:3px}
.btn-small [class^="icon-"],.btn-small [class*=" icon-"]{margin-top:0}
.btn-mini [class^="icon-"],.btn-mini [class*=" icon-"]{margin-top:-1px}
.btn-mini{padding:0 6px;font-size:9.75px;border-radius:3px;-webkit-border-radius:3px;-moz-border-radius:3px;border-radius:3px}
.btn-block{display:block;width:100%;padding-left:0;padding-right:0;-webkit-box-sizing:border-box;-moz-box-sizing:border-box;box-sizing:border-box}
.btn-block+.btn-block{margin-top:5px}
input[type="submit"].btn-block,input[type="reset"].btn-block,input[type="button"].btn-block{width:100%}
.btn-primary.active,.btn-warning.active,.btn-danger.active,.btn-success.active,.btn-info.active,.btn-inverse.active{color:rgba(255,255,255,0.75)}
.btn-primary{color:#fff;text-shadow:0 -1px 0 rgba(0,0,0,0.25);background-color:#006dcc;background-image:-moz-linear-gradient(top, #08c, #04c);background-image:-webkit-gradient(linear, 0 0, 0 100%, from(#08c), to(#04c));background-image:-webkit-linear-gradient(top, #08c, #04c);background-image:-o-linear-gradient(top, #08c, #04c);background-image:linear-gradient(to bottom, #08c, #04c);background-repeat:repeat-x;filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#ff0088cc', endColorstr='#ff0044cc', GradientType=0);border-color:#04c #04c #002a80;border-color:rgba(0,0,0,0.1) rgba(0,0,0,0.1) rgba(0,0,0,0.25);*background-color:#04c;filter:progid:DXImageTransform.Microsoft.gradient(enabled = false)}.btn-primary:hover,.btn-primary:focus,.btn-primary:active,.btn-primary.active,.btn-primary.disabled,.btn-primary[disabled]{color:#fff;background-color:#04c;*background-color:#003bb3}
.btn-primary:active,.btn-primary.active{background-color:#039 \9}
.btn-warning{color:#fff;text-shadow:0 -1px 0 rgba(0,0,0,0.25);background-color:#faa732;background-image:-moz-linear-gradient(top, #fbb450, #f89406);background-image:-webkit-gradient(linear, 0 0, 0 100%, from(#fbb450), to(#f89406));background-image:-webkit-linear-gradient(top, #fbb450, #f89406);background-image:-o-linear-gradient(top, #fbb450, #f89406);background-image:linear-gradient(to bottom, #fbb450, #f89406);background-repeat:repeat-x;filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#fffbb450', endColorstr='#fff89406', GradientType=0);border-color:#f89406 #f89406 #ad6704;border-color:rgba(0,0,0,0.1) rgba(0,0,0,0.1) rgba(0,0,0,0.25);*background-color:#f89406;filter:progid:DXImageTransform.Microsoft.gradient(enabled = false)}.btn-warning:hover,.btn-warning:focus,.btn-warning:active,.btn-warning.active,.btn-warning.disabled,.btn-warning[disabled]{color:#fff;background-color:#f89406;*background-color:#df8505}
.btn-warning:active,.btn-warning.active{background-color:#c67605 \9}
.btn-danger{color:#fff;text-shadow:0 -1px 0 rgba(0,0,0,0.25);background-color:#da4f49;background-image:-moz-linear-gradient(top, #ee5f5b, #bd362f);background-image:-webkit-gradient(linear, 0 0, 0 100%, from(#ee5f5b), to(#bd362f));background-image:-webkit-linear-gradient(top, #ee5f5b, #bd362f);background-image:-o-linear-gradient(top, #ee5f5b, #bd362f);background-image:linear-gradient(to bottom, #ee5f5b, #bd362f);background-repeat:repeat-x;filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#ffee5f5b', endColorstr='#ffbd362f', GradientType=0);border-color:#bd362f #bd362f #802420;border-color:rgba(0,0,0,0.1) rgba(0,0,0,0.1) rgba(0,0,0,0.25);*background-color:#bd362f;filter:progid:DXImageTransform.Microsoft.gradient(enabled = false)}.btn-danger:hover,.btn-danger:focus,.btn-danger:active,.btn-danger.active,.btn-danger.disabled,.btn-danger[disabled]{color:#fff;background-color:#bd362f;*background-color:#a9302a}
.btn-danger:active,.btn-danger.active{background-color:#942a25 \9}
.btn-success{color:#fff;text-shadow:0 -1px 0 rgba(0,0,0,0.25);background-color:#5bb75b;background-image:-moz-linear-gradient(top, #62c462, #51a351);background-image:-webkit-gradient(linear, 0 0, 0 100%, from(#62c462), to(#51a351));background-image:-webkit-linear-gradient(top, #62c462, #51a351);background-image:-o-linear-gradient(top, #62c462, #51a351);background-image:linear-gradient(to bottom, #62c462, #51a351);background-repeat:repeat-x;filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#ff62c462', endColorstr='#ff51a351', GradientType=0);border-color:#51a351 #51a351 #387038;border-color:rgba(0,0,0,0.1) rgba(0,0,0,0.1) rgba(0,0,0,0.25);*background-color:#51a351;filter:progid:DXImageTransform.Microsoft.gradient(enabled = false)}.btn-success:hover,.btn-success:focus,.btn-success:active,.btn-success.active,.btn-success.disabled,.btn-success[disabled]{color:#fff;background-color:#51a351;*background-color:#499249}
.btn-success:active,.btn-success.active{background-color:#408140 \9}
.btn-info{color:#fff;text-shadow:0 -1px 0 rgba(0,0,0,0.25);background-color:#49afcd;background-image:-moz-linear-gradient(top, #5bc0de, #2f96b4);background-image:-webkit-gradient(linear, 0 0, 0 100%, from(#5bc0de), to(#2f96b4));background-image:-webkit-linear-gradient(top, #5bc0de, #2f96b4);background-image:-o-linear-gradient(top, #5bc0de, #2f96b4);background-image:linear-gradient(to bottom, #5bc0de, #2f96b4);background-repeat:repeat-x;filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#ff5bc0de', endColorstr='#ff2f96b4', GradientType=0);border-color:#2f96b4 #2f96b4 #1f6377;border-color:rgba(0,0,0,0.1) rgba(0,0,0,0.1) rgba(0,0,0,0.25);*background-color:#2f96b4;filter:progid:DXImageTransform.Microsoft.gradient(enabled = false)}.btn-info:hover,.btn-info:focus,.btn-info:active,.btn-info.active,.btn-info.disabled,.btn-info[disabled]{color:#fff;background-color:#2f96b4;*background-color:#2a85a0}
.btn-info:active,.btn-info.active{background-color:#24748c \9}
.btn-inverse{color:#fff;text-shadow:0 -1px 0 rgba(0,0,0,0.25);background-color:#363636;background-image:-moz-linear-gradient(top, #444, #222);background-image:-webkit-gradient(linear, 0 0, 0 100%, from(#444), to(#222));background-image:-webkit-linear-gradient(top, #444, #222);background-image:-o-linear-gradient(top, #444, #222);background-image:linear-gradient(to bottom, #444, #222);background-repeat:repeat-x;filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#ff444444', endColorstr='#ff222222', GradientType=0);border-color:#222 #222 #000;border-color:rgba(0,0,0,0.1) rgba(0,0,0,0.1) rgba(0,0,0,0.25);*background-color:#222;filter:progid:DXImageTransform.Microsoft.gradient(enabled = false)}.btn-inverse:hover,.btn-inverse:focus,.btn-inverse:active,.btn-inverse.active,.btn-inverse.disabled,.btn-inverse[disabled]{color:#fff;background-color:#222;*background-color:#151515}
.btn-inverse:active,.btn-inverse.active{background-color:#080808 \9}
button.btn,input[type="submit"].btn{*padding-top:3px;*padding-bottom:3px}button.btn::-moz-focus-inner,input[type="submit"].btn::-moz-focus-inner{padding:0;border:0}
button.btn.btn-large,input[type="submit"].btn.btn-large{*padding-top:7px;*padding-bottom:7px}
button.btn.btn-small,input[type="submit"].btn.btn-small{*padding-top:3px;*padding-bottom:3px}
button.btn.btn-mini,input[type="submit"].btn.btn-mini{*padding-top:1px;*padding-bottom:1px}
.btn-link,.btn-link:active,.btn-link[disabled]{background-color:transparent;background-image:none;-webkit-box-shadow:none;-moz-box-shadow:none;box-shadow:none}
.btn-link{border-color:transparent;cursor:pointer;color:#08c;border-radius:0;-webkit-border-radius:0;-moz-border-radius:0;border-radius:0}
.btn-link:hover,.btn-link:focus{color:#005580;text-decoration:underline;background-color:transparent}
.btn-link[disabled]:hover,.btn-link[disabled]:focus{color:#333;text-decoration:none}
.btn-group{position:relative;display:inline-block;*display:inline;*zoom:1;font-size:0;vertical-align:middle;white-space:nowrap;*margin-left:.3em}.btn-group:first-child{*margin-left:0}
.btn-group+.btn-group{margin-left:5px}
.btn-toolbar{font-size:0;margin-top:10px;margin-bottom:10px}.btn-toolbar>.btn+.btn,.btn-toolbar>.btn-group+.btn,.btn-toolbar>.btn+.btn-group{margin-left:5px}
.btn-group>.btn{position:relative;border-radius:0;-webkit-border-radius:0;-moz-border-radius:0;border-radius:0}
.btn-group>.btn+.btn{margin-left:-1px}
.btn-group>.btn,.btn-group>.dropdown-menu,.btn-group>.popover{font-size:13px}
.btn-group>.btn-mini{font-size:9.75px}
.btn-group>.btn-small{font-size:11.049999999999999px}
.btn-group>.btn-large{font-size:16.25px}
.btn-group>.btn:first-child{margin-left:0;-webkit-border-top-left-radius:4px;-moz-border-radius-topleft:4px;border-top-left-radius:4px;-webkit-border-bottom-left-radius:4px;-moz-border-radius-bottomleft:4px;border-bottom-left-radius:4px}
.btn-group>.btn:last-child,.btn-group>.dropdown-toggle{-webkit-border-top-right-radius:4px;-moz-border-radius-topright:4px;border-top-right-radius:4px;-webkit-border-bottom-right-radius:4px;-moz-border-radius-bottomright:4px;border-bottom-right-radius:4px}
.btn-group>.btn.large:first-child{margin-left:0;-webkit-border-top-left-radius:6px;-moz-border-radius-topleft:6px;border-top-left-radius:6px;-webkit-border-bottom-left-radius:6px;-moz-border-radius-bottomleft:6px;border-bottom-left-radius:6px}
.btn-group>.btn.large:last-child,.btn-group>.large.dropdown-toggle{-webkit-border-top-right-radius:6px;-moz-border-radius-topright:6px;border-top-right-radius:6px;-webkit-border-bottom-right-radius:6px;-moz-border-radius-bottomright:6px;border-bottom-right-radius:6px}
.btn-group>.btn:hover,.btn-group>.btn:focus,.btn-group>.btn:active,.btn-group>.btn.active{z-index:2}
.btn-group .dropdown-toggle:active,.btn-group.open .dropdown-toggle{outline:0}
.btn-group>.btn+.dropdown-toggle{padding-left:8px;padding-right:8px;-webkit-box-shadow:inset 1px 0 0 rgba(255,255,255,.125), inset 0 1px 0 rgba(255,255,255,.2), 0 1px 2px rgba(0,0,0,.05);-moz-box-shadow:inset 1px 0 0 rgba(255,255,255,.125), inset 0 1px 0 rgba(255,255,255,.2), 0 1px 2px rgba(0,0,0,.05);box-shadow:inset 1px 0 0 rgba(255,255,255,.125), inset 0 1px 0 rgba(255,255,255,.2), 0 1px 2px rgba(0,0,0,.05);*padding-top:5px;*padding-bottom:5px}
.btn-group>.btn-mini+.dropdown-toggle{padding-left:5px;padding-right:5px;*padding-top:2px;*padding-bottom:2px}
.btn-group>.btn-small+.dropdown-toggle{*padding-top:5px;*padding-bottom:4px}
.btn-group>.btn-large+.dropdown-toggle{padding-left:12px;padding-right:12px;*padding-top:7px;*padding-bottom:7px}
.btn-group.open .dropdown-toggle{background-image:none;-webkit-box-shadow:inset 0 2px 4px rgba(0,0,0,.15), 0 1px 2px rgba(0,0,0,.05);-moz-box-shadow:inset 0 2px 4px rgba(0,0,0,.15), 0 1px 2px rgba(0,0,0,.05);box-shadow:inset 0 2px 4px rgba(0,0,0,.15), 0 1px 2px rgba(0,0,0,.05)}
.btn-group.open .btn.dropdown-toggle{background-color:#e6e6e6}
.btn-group.open .btn-primary.dropdown-toggle{background-color:#04c}
.btn-group.open .btn-warning.dropdown-toggle{background-color:#f89406}
.btn-group.open .btn-danger.dropdown-toggle{background-color:#bd362f}
.btn-group.open .btn-success.dropdown-toggle{background-color:#51a351}
.btn-group.open .btn-info.dropdown-toggle{background-color:#2f96b4}
.btn-group.open .btn-inverse.dropdown-toggle{background-color:#222}
.btn .caret{margin-top:8px;margin-left:0}
.btn-large .caret{margin-top:6px}
.btn-large .caret{border-left-width:5px;border-right-width:5px;border-top-width:5px}
.btn-mini .caret,.btn-small .caret{margin-top:8px}
.dropup .btn-large .caret{border-bottom-width:5px}
.btn-primary .caret,.btn-warning .caret,.btn-danger .caret,.btn-info .caret,.btn-success .caret,.btn-inverse .caret{border-top-color:#fff;border-bottom-color:#fff}
.btn-group-vertical{display:inline-block;*display:inline;*zoom:1}
.btn-group-vertical>.btn{display:block;float:none;max-width:100%;border-radius:0;-webkit-border-radius:0;-moz-border-radius:0;border-radius:0}
.btn-group-vertical>.btn+.btn{margin-left:0;margin-top:-1px}
.btn-group-vertical>.btn:first-child{border-radius:4px 4px 0 0;-webkit-border-radius:4px 4px 0 0;-moz-border-radius:4px 4px 0 0;border-radius:4px 4px 0 0}
.btn-group-vertical>.btn:last-child{border-radius:0 0 4px 4px;-webkit-border-radius:0 0 4px 4px;-moz-border-radius:0 0 4px 4px;border-radius:0 0 4px 4px}
.btn-group-vertical>.btn-large:first-child{border-radius:6px 6px 0 0;-webkit-border-radius:6px 6px 0 0;-moz-border-radius:6px 6px 0 0;border-radius:6px 6px 0 0}
.btn-group-vertical>.btn-large:last-child{border-radius:0 0 6px 6px;-webkit-border-radius:0 0 6px 6px;-moz-border-radius:0 0 6px 6px;border-radius:0 0 6px 6px}
.alert{padding:8px 35px 8px 14px;margin-bottom:20px;text-shadow:0 1px 0 rgba(255,255,255,0.5);background-color:#fcf8e3;border:1px solid #fbeed5;border-radius:4px;-webkit-border-radius:4px;-moz-border-radius:4px;border-radius:4px}
.alert,.alert h4{color:#c09853}
.alert h4{margin:0}
.alert .close{position:relative;top:-2px;right:-21px;line-height:20px}
.alert-success{background-color:#dff0d8;border-color:#d6e9c6;color:#468847}
.alert-success h4{color:#468847}
.alert-danger,.alert-error{background-color:#f2dede;border-color:#eed3d7;color:#b94a48}
.alert-danger h4,.alert-error h4{color:#b94a48}
.alert-info{background-color:#d9edf7;border-color:#bce8f1;color:#3a87ad}
.alert-info h4{color:#3a87ad}
.alert-block{padding-top:14px;padding-bottom:14px}
.alert-block>p,.alert-block>ul{margin-bottom:0}
.alert-block p+p{margin-top:5px}
.nav{margin-left:0;margin-bottom:20px;list-style:none}
.nav>li>a{display:block}
.nav>li>a:hover,.nav>li>a:focus{text-decoration:none;background-color:#eee}
.nav>li>a>img{max-width:none}
.nav>.pull-right{float:right}
.nav-header{display:block;padding:3px 15px;font-size:11px;font-weight:bold;line-height:20px;color:#999;text-shadow:0 1px 0 rgba(255,255,255,0.5);text-transform:uppercase}
.nav li+.nav-header{margin-top:9px}
.nav-list{padding-left:15px;padding-right:15px;margin-bottom:0}
.nav-list>li>a,.nav-list .nav-header{margin-left:-15px;margin-right:-15px;text-shadow:0 1px 0 rgba(255,255,255,0.5)}
.nav-list>li>a{padding:3px 15px}
.nav-list>.active>a,.nav-list>.active>a:hover,.nav-list>.active>a:focus{color:#fff;text-shadow:0 -1px 0 rgba(0,0,0,0.2);background-color:#08c}
.nav-list [class^="icon-"],.nav-list [class*=" icon-"]{margin-right:2px}
.nav-list .divider{*width:100%;height:1px;margin:9px 1px;*margin:-5px 0 5px;overflow:hidden;background-color:#e5e5e5;border-bottom:1px solid #fff}
.nav-tabs,.nav-pills{*zoom:1}.nav-tabs:before,.nav-pills:before,.nav-tabs:after,.nav-pills:after{display:table;content:"";line-height:0}
.nav-tabs:after,.nav-pills:after{clear:both}
.nav-tabs>li,.nav-pills>li{float:left}
.nav-tabs>li>a,.nav-pills>li>a{padding-right:12px;padding-left:12px;margin-right:2px;line-height:14px}
.nav-tabs{border-bottom:1px solid #ddd}
.nav-tabs>li{margin-bottom:-1px}
.nav-tabs>li>a{padding-top:8px;padding-bottom:8px;line-height:20px;border:1px solid transparent;border-radius:4px 4px 0 0;-webkit-border-radius:4px 4px 0 0;-moz-border-radius:4px 4px 0 0;border-radius:4px 4px 0 0}.nav-tabs>li>a:hover,.nav-tabs>li>a:focus{border-color:#eee #eee #ddd}
.nav-tabs>.active>a,.nav-tabs>.active>a:hover,.nav-tabs>.active>a:focus{color:#555;background-color:#fff;border:1px solid #ddd;border-bottom-color:transparent;cursor:default}
.nav-pills>li>a{padding-top:8px;padding-bottom:8px;margin-top:2px;margin-bottom:2px;border-radius:5px;-webkit-border-radius:5px;-moz-border-radius:5px;border-radius:5px}
.nav-pills>.active>a,.nav-pills>.active>a:hover,.nav-pills>.active>a:focus{color:#fff;background-color:#08c}
.nav-stacked>li{float:none}
.nav-stacked>li>a{margin-right:0}
.nav-tabs.nav-stacked{border-bottom:0}
.nav-tabs.nav-stacked>li>a{border:1px solid #ddd;border-radius:0;-webkit-border-radius:0;-moz-border-radius:0;border-radius:0}
.nav-tabs.nav-stacked>li:first-child>a{-webkit-border-top-right-radius:4px;-moz-border-radius-topright:4px;border-top-right-radius:4px;-webkit-border-top-left-radius:4px;-moz-border-radius-topleft:4px;border-top-left-radius:4px}
.nav-tabs.nav-stacked>li:last-child>a{-webkit-border-bottom-right-radius:4px;-moz-border-radius-bottomright:4px;border-bottom-right-radius:4px;-webkit-border-bottom-left-radius:4px;-moz-border-radius-bottomleft:4px;border-bottom-left-radius:4px}
.nav-tabs.nav-stacked>li>a:hover,.nav-tabs.nav-stacked>li>a:focus{border-color:#ddd;z-index:2}
.nav-pills.nav-stacked>li>a{margin-bottom:3px}
.nav-pills.nav-stacked>li:last-child>a{margin-bottom:1px}
.nav-tabs .dropdown-menu{border-radius:0 0 6px 6px;-webkit-border-radius:0 0 6px 6px;-moz-border-radius:0 0 6px 6px;border-radius:0 0 6px 6px}
.nav-pills .dropdown-menu{border-radius:6px;-webkit-border-radius:6px;-moz-border-radius:6px;border-radius:6px}
.nav .dropdown-toggle .caret{border-top-color:#08c;border-bottom-color:#08c;margin-top:6px}
.nav .dropdown-toggle:hover .caret,.nav .dropdown-toggle:focus .caret{border-top-color:#005580;border-bottom-color:#005580}
.nav-tabs .dropdown-toggle .caret{margin-top:8px}
.nav .active .dropdown-toggle .caret{border-top-color:#fff;border-bottom-color:#fff}
.nav-tabs .active .dropdown-toggle .caret{border-top-color:#555;border-bottom-color:#555}
.nav>.dropdown.active>a:hover,.nav>.dropdown.active>a:focus{cursor:pointer}
.nav-tabs .open .dropdown-toggle,.nav-pills .open .dropdown-toggle,.nav>li.dropdown.open.active>a:hover,.nav>li.dropdown.open.active>a:focus{color:#fff;background-color:#999;border-color:#999}
.nav li.dropdown.open .caret,.nav li.dropdown.open.active .caret,.nav li.dropdown.open a:hover .caret,.nav li.dropdown.open a:focus .caret{border-top-color:#fff;border-bottom-color:#fff;opacity:1;filter:alpha(opacity=100)}
.tabs-stacked .open>a:hover,.tabs-stacked .open>a:focus{border-color:#999}
.tabbable{*zoom:1}.tabbable:before,.tabbable:after{display:table;content:"";line-height:0}
.tabbable:after{clear:both}
.tab-content{overflow:auto}
.tabs-below>.nav-tabs,.tabs-right>.nav-tabs,.tabs-left>.nav-tabs{border-bottom:0}
.tab-content>.tab-pane,.pill-content>.pill-pane{display:none}
.tab-content>.active,.pill-content>.active{display:block}
.tabs-below>.nav-tabs{border-top:1px solid #ddd}
.tabs-below>.nav-tabs>li{margin-top:-1px;margin-bottom:0}
.tabs-below>.nav-tabs>li>a{border-radius:0 0 4px 4px;-webkit-border-radius:0 0 4px 4px;-moz-border-radius:0 0 4px 4px;border-radius:0 0 4px 4px}.tabs-below>.nav-tabs>li>a:hover,.tabs-below>.nav-tabs>li>a:focus{border-bottom-color:transparent;border-top-color:#ddd}
.tabs-below>.nav-tabs>.active>a,.tabs-below>.nav-tabs>.active>a:hover,.tabs-below>.nav-tabs>.active>a:focus{border-color:transparent #ddd #ddd #ddd}
.tabs-left>.nav-tabs>li,.tabs-right>.nav-tabs>li{float:none}
.tabs-left>.nav-tabs>li>a,.tabs-right>.nav-tabs>li>a{min-width:74px;margin-right:0;margin-bottom:3px}
.tabs-left>.nav-tabs{float:left;margin-right:19px;border-right:1px solid #ddd}
.tabs-left>.nav-tabs>li>a{margin-right:-1px;border-radius:4px 0 0 4px;-webkit-border-radius:4px 0 0 4px;-moz-border-radius:4px 0 0 4px;border-radius:4px 0 0 4px}
.tabs-left>.nav-tabs>li>a:hover,.tabs-left>.nav-tabs>li>a:focus{border-color:#eee #ddd #eee #eee}
.tabs-left>.nav-tabs .active>a,.tabs-left>.nav-tabs .active>a:hover,.tabs-left>.nav-tabs .active>a:focus{border-color:#ddd transparent #ddd #ddd;*border-right-color:#fff}
.tabs-right>.nav-tabs{float:right;margin-left:19px;border-left:1px solid #ddd}
.tabs-right>.nav-tabs>li>a{margin-left:-1px;border-radius:0 4px 4px 0;-webkit-border-radius:0 4px 4px 0;-moz-border-radius:0 4px 4px 0;border-radius:0 4px 4px 0}
.tabs-right>.nav-tabs>li>a:hover,.tabs-right>.nav-tabs>li>a:focus{border-color:#eee #eee #eee #ddd}
.tabs-right>.nav-tabs .active>a,.tabs-right>.nav-tabs .active>a:hover,.tabs-right>.nav-tabs .active>a:focus{border-color:#ddd #ddd #ddd transparent;*border-left-color:#fff}
.nav>.disabled>a{color:#999}
.nav>.disabled>a:hover,.nav>.disabled>a:focus{text-decoration:none;background-color:transparent;cursor:default}
.navbar{overflow:visible;margin-bottom:20px;*position:relative;*z-index:2}
.navbar-inner{min-height:36px;padding-left:20px;padding-right:20px;background-color:#fafafa;background-image:-moz-linear-gradient(top, #fff, #f2f2f2);background-image:-webkit-gradient(linear, 0 0, 0 100%, from(#fff), to(#f2f2f2));background-image:-webkit-linear-gradient(top, #fff, #f2f2f2);background-image:-o-linear-gradient(top, #fff, #f2f2f2);background-image:linear-gradient(to bottom, #fff, #f2f2f2);background-repeat:repeat-x;filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#ffffffff', endColorstr='#fff2f2f2', GradientType=0);border:1px solid #d4d4d4;-webkit-border-radius:4px;-moz-border-radius:4px;border-radius:4px;-webkit-box-shadow:0 1px 4px rgba(0,0,0,0.065);-moz-box-shadow:0 1px 4px rgba(0,0,0,0.065);box-shadow:0 1px 4px rgba(0,0,0,0.065);*zoom:1}.navbar-inner:before,.navbar-inner:after{display:table;content:"";line-height:0}
.navbar-inner:after{clear:both}
.navbar .container{width:auto}
.nav-collapse.collapse{height:auto;overflow:visible}
.navbar .brand{float:left;display:block;padding:8px 20px 8px;margin-left:-20px;font-size:20px;font-weight:200;color:#777;text-shadow:0 1px 0 #fff}.navbar .brand:hover,.navbar .brand:focus{text-decoration:none}
.navbar-text{margin-bottom:0;line-height:36px;color:#777}
.navbar-link{color:#777}.navbar-link:hover,.navbar-link:focus{color:#333}
.navbar .divider-vertical{height:36px;margin:0 9px;border-left:1px solid #f2f2f2;border-right:1px solid #fff}
.navbar .btn,.navbar .btn-group{margin-top:3px}
.navbar .btn-group .btn,.navbar .input-prepend .btn,.navbar .input-append .btn,.navbar .input-prepend .btn-group,.navbar .input-append .btn-group{margin-top:0}
.navbar-form{margin-bottom:0;*zoom:1}.navbar-form:before,.navbar-form:after{display:table;content:"";line-height:0}
.navbar-form:after{clear:both}
.navbar-form input,.navbar-form select,.navbar-form .radio,.navbar-form .checkbox{margin-top:3px}
.navbar-form input,.navbar-form select,.navbar-form .btn{display:inline-block;margin-bottom:0}
.navbar-form input[type="image"],.navbar-form input[type="checkbox"],.navbar-form input[type="radio"]{margin-top:3px}
.navbar-form .input-append,.navbar-form .input-prepend{margin-top:5px;white-space:nowrap}.navbar-form .input-append input,.navbar-form .input-prepend input{margin-top:0}
.navbar-search{position:relative;float:left;margin-top:3px;margin-bottom:0}.navbar-search .search-query{margin-bottom:0;padding:4px 14px;font-family:"Helvetica Neue",Helvetica,Arial,sans-serif;font-size:13px;font-weight:normal;line-height:1;border-radius:15px;-webkit-border-radius:15px;-moz-border-radius:15px;border-radius:15px}
.navbar-static-top{position:static;margin-bottom:0}.navbar-static-top .navbar-inner{border-radius:0;-webkit-border-radius:0;-moz-border-radius:0;border-radius:0}
.navbar-fixed-top,.navbar-fixed-bottom{position:fixed;right:0;left:0;z-index:1030;margin-bottom:0}
.navbar-fixed-top .navbar-inner,.navbar-static-top .navbar-inner{border-width:0 0 1px}
.navbar-fixed-bottom .navbar-inner{border-width:1px 0 0}
.navbar-fixed-top .navbar-inner,.navbar-fixed-bottom .navbar-inner{padding-left:0;padding-right:0;border-radius:0;-webkit-border-radius:0;-moz-border-radius:0;border-radius:0}
.navbar-static-top .container,.navbar-fixed-top .container,.navbar-fixed-bottom .container{width:940px}
.navbar-fixed-top{top:0}
.navbar-fixed-top .navbar-inner,.navbar-static-top .navbar-inner{-webkit-box-shadow:0 1px 10px rgba(0,0,0,.1);-moz-box-shadow:0 1px 10px rgba(0,0,0,.1);box-shadow:0 1px 10px rgba(0,0,0,.1)}
.navbar-fixed-bottom{bottom:0}.navbar-fixed-bottom .navbar-inner{-webkit-box-shadow:0 -1px 10px rgba(0,0,0,.1);-moz-box-shadow:0 -1px 10px rgba(0,0,0,.1);box-shadow:0 -1px 10px rgba(0,0,0,.1)}
.navbar .nav{position:relative;left:0;display:block;float:left;margin:0 10px 0 0}
.navbar .nav.pull-right{float:right;margin-right:0}
.navbar .nav>li{float:left}
.navbar .nav>li>a{float:none;padding:8px 15px 8px;color:#777;text-decoration:none;text-shadow:0 1px 0 #fff}
.navbar .nav .dropdown-toggle .caret{margin-top:8px}
.navbar .nav>li>a:focus,.navbar .nav>li>a:hover{background-color:transparent;color:#333;text-decoration:none}
.navbar .nav>.active>a,.navbar .nav>.active>a:hover,.navbar .nav>.active>a:focus{color:#555;text-decoration:none;background-color:#e5e5e5;-webkit-box-shadow:inset 0 3px 8px rgba(0,0,0,0.125);-moz-box-shadow:inset 0 3px 8px rgba(0,0,0,0.125);box-shadow:inset 0 3px 8px rgba(0,0,0,0.125)}
.navbar .btn-navbar{display:none;float:right;padding:7px 10px;margin-left:5px;margin-right:5px;color:#fff;text-shadow:0 -1px 0 rgba(0,0,0,0.25);background-color:#ededed;background-image:-moz-linear-gradient(top, #f2f2f2, #e5e5e5);background-image:-webkit-gradient(linear, 0 0, 0 100%, from(#f2f2f2), to(#e5e5e5));background-image:-webkit-linear-gradient(top, #f2f2f2, #e5e5e5);background-image:-o-linear-gradient(top, #f2f2f2, #e5e5e5);background-image:linear-gradient(to bottom, #f2f2f2, #e5e5e5);background-repeat:repeat-x;filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#fff2f2f2', endColorstr='#ffe5e5e5', GradientType=0);border-color:#e5e5e5 #e5e5e5 #bfbfbf;border-color:rgba(0,0,0,0.1) rgba(0,0,0,0.1) rgba(0,0,0,0.25);*background-color:#e5e5e5;filter:progid:DXImageTransform.Microsoft.gradient(enabled = false);-webkit-box-shadow:inset 0 1px 0 rgba(255,255,255,.1), 0 1px 0 rgba(255,255,255,.075);-moz-box-shadow:inset 0 1px 0 rgba(255,255,255,.1), 0 1px 0 rgba(255,255,255,.075);box-shadow:inset 0 1px 0 rgba(255,255,255,.1), 0 1px 0 rgba(255,255,255,.075)}.navbar .btn-navbar:hover,.navbar .btn-navbar:focus,.navbar .btn-navbar:active,.navbar .btn-navbar.active,.navbar .btn-navbar.disabled,.navbar .btn-navbar[disabled]{color:#fff;background-color:#e5e5e5;*background-color:#d9d9d9}
.navbar .btn-navbar:active,.navbar .btn-navbar.active{background-color:#ccc \9}
.navbar .btn-navbar .icon-bar{display:block;width:18px;height:2px;background-color:#f5f5f5;-webkit-border-radius:1px;-moz-border-radius:1px;border-radius:1px;-webkit-box-shadow:0 1px 0 rgba(0,0,0,0.25);-moz-box-shadow:0 1px 0 rgba(0,0,0,0.25);box-shadow:0 1px 0 rgba(0,0,0,0.25)}
.btn-navbar .icon-bar+.icon-bar{margin-top:3px}
.navbar .nav>li>.dropdown-menu:before{content:'';display:inline-block;border-left:7px solid transparent;border-right:7px solid transparent;border-bottom:7px solid #ccc;border-bottom-color:rgba(0,0,0,0.2);position:absolute;top:-7px;left:9px}
.navbar .nav>li>.dropdown-menu:after{content:'';display:inline-block;border-left:6px solid transparent;border-right:6px solid transparent;border-bottom:6px solid #fff;position:absolute;top:-6px;left:10px}
.navbar-fixed-bottom .nav>li>.dropdown-menu:before{border-top:7px solid #ccc;border-top-color:rgba(0,0,0,0.2);border-bottom:0;bottom:-7px;top:auto}
.navbar-fixed-bottom .nav>li>.dropdown-menu:after{border-top:6px solid #fff;border-bottom:0;bottom:-6px;top:auto}
.navbar .nav li.dropdown>a:hover .caret,.navbar .nav li.dropdown>a:focus .caret{border-top-color:#333;border-bottom-color:#333}
.navbar .nav li.dropdown.open>.dropdown-toggle,.navbar .nav li.dropdown.active>.dropdown-toggle,.navbar .nav li.dropdown.open.active>.dropdown-toggle{background-color:#e5e5e5;color:#555}
.navbar .nav li.dropdown>.dropdown-toggle .caret{border-top-color:#777;border-bottom-color:#777}
.navbar .nav li.dropdown.open>.dropdown-toggle .caret,.navbar .nav li.dropdown.active>.dropdown-toggle .caret,.navbar .nav li.dropdown.open.active>.dropdown-toggle .caret{border-top-color:#555;border-bottom-color:#555}
.navbar .pull-right>li>.dropdown-menu,.navbar .nav>li>.dropdown-menu.pull-right{left:auto;right:0}.navbar .pull-right>li>.dropdown-menu:before,.navbar .nav>li>.dropdown-menu.pull-right:before{left:auto;right:12px}
.navbar .pull-right>li>.dropdown-menu:after,.navbar .nav>li>.dropdown-menu.pull-right:after{left:auto;right:13px}
.navbar .pull-right>li>.dropdown-menu .dropdown-menu,.navbar .nav>li>.dropdown-menu.pull-right .dropdown-menu{left:auto;right:100%;margin-left:0;margin-right:-1px;border-radius:6px 0 6px 6px;-webkit-border-radius:6px 0 6px 6px;-moz-border-radius:6px 0 6px 6px;border-radius:6px 0 6px 6px}
.navbar-inverse .navbar-inner{background-color:#1b1b1b;background-image:-moz-linear-gradient(top, #222, #111);background-image:-webkit-gradient(linear, 0 0, 0 100%, from(#222), to(#111));background-image:-webkit-linear-gradient(top, #222, #111);background-image:-o-linear-gradient(top, #222, #111);background-image:linear-gradient(to bottom, #222, #111);background-repeat:repeat-x;filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#ff222222', endColorstr='#ff111111', GradientType=0);border-color:#252525}
.navbar-inverse .brand,.navbar-inverse .nav>li>a{color:#999;text-shadow:0 -1px 0 rgba(0,0,0,0.25)}.navbar-inverse .brand:hover,.navbar-inverse .nav>li>a:hover,.navbar-inverse .brand:focus,.navbar-inverse .nav>li>a:focus{color:#fff}
.navbar-inverse .brand{color:#999}
.navbar-inverse .navbar-text{color:#999}
.navbar-inverse .nav>li>a:focus,.navbar-inverse .nav>li>a:hover{background-color:transparent;color:#fff}
.navbar-inverse .nav .active>a,.navbar-inverse .nav .active>a:hover,.navbar-inverse .nav .active>a:focus{color:#fff;background-color:#111}
.navbar-inverse .navbar-link{color:#999}.navbar-inverse .navbar-link:hover,.navbar-inverse .navbar-link:focus{color:#fff}
.navbar-inverse .divider-vertical{border-left-color:#111;border-right-color:#222}
.navbar-inverse .nav li.dropdown.open>.dropdown-toggle,.navbar-inverse .nav li.dropdown.active>.dropdown-toggle,.navbar-inverse .nav li.dropdown.open.active>.dropdown-toggle{background-color:#111;color:#fff}
.navbar-inverse .nav li.dropdown>a:hover .caret,.navbar-inverse .nav li.dropdown>a:focus .caret{border-top-color:#fff;border-bottom-color:#fff}
.navbar-inverse .nav li.dropdown>.dropdown-toggle .caret{border-top-color:#999;border-bottom-color:#999}
.navbar-inverse .nav li.dropdown.open>.dropdown-toggle .caret,.navbar-inverse .nav li.dropdown.active>.dropdown-toggle .caret,.navbar-inverse .nav li.dropdown.open.active>.dropdown-toggle .caret{border-top-color:#fff;border-bottom-color:#fff}
.navbar-inverse .navbar-search .search-query{color:#fff;background-color:#515151;border-color:#111;-webkit-box-shadow:inset 0 1px 2px rgba(0,0,0,.1), 0 1px 0 rgba(255,255,255,.15);-moz-box-shadow:inset 0 1px 2px rgba(0,0,0,.1), 0 1px 0 rgba(255,255,255,.15);box-shadow:inset 0 1px 2px rgba(0,0,0,.1), 0 1px 0 rgba(255,255,255,.15);-webkit-transition:none;-moz-transition:none;-o-transition:none;transition:none}.navbar-inverse .navbar-search .search-query:-moz-placeholder{color:#ccc}
.navbar-inverse .navbar-search .search-query:-ms-input-placeholder{color:#ccc}
.navbar-inverse .navbar-search .search-query::-webkit-input-placeholder{color:#ccc}
.navbar-inverse .navbar-search .search-query:focus,.navbar-inverse .navbar-search .search-query.focused{padding:5px 15px;color:#333;text-shadow:0 1px 0 #fff;background-color:#fff;border:0;-webkit-box-shadow:0 0 3px rgba(0,0,0,0.15);-moz-box-shadow:0 0 3px rgba(0,0,0,0.15);box-shadow:0 0 3px rgba(0,0,0,0.15);outline:0}
.navbar-inverse .btn-navbar{color:#fff;text-shadow:0 -1px 0 rgba(0,0,0,0.25);background-color:#0e0e0e;background-image:-moz-linear-gradient(top, #151515, #040404);background-image:-webkit-gradient(linear, 0 0, 0 100%, from(#151515), to(#040404));background-image:-webkit-linear-gradient(top, #151515, #040404);background-image:-o-linear-gradient(top, #151515, #040404);background-image:linear-gradient(to bottom, #151515, #040404);background-repeat:repeat-x;filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#ff151515', endColorstr='#ff040404', GradientType=0);border-color:#040404 #040404 #000;border-color:rgba(0,0,0,0.1) rgba(0,0,0,0.1) rgba(0,0,0,0.25);*background-color:#040404;filter:progid:DXImageTransform.Microsoft.gradient(enabled = false)}.navbar-inverse .btn-navbar:hover,.navbar-inverse .btn-navbar:focus,.navbar-inverse .btn-navbar:active,.navbar-inverse .btn-navbar.active,.navbar-inverse .btn-navbar.disabled,.navbar-inverse .btn-navbar[disabled]{color:#fff;background-color:#040404;*background-color:#000}
.navbar-inverse .btn-navbar:active,.navbar-inverse .btn-navbar.active{background-color:#000 \9}
.breadcrumb{padding:8px 15px;margin:0 0 20px;list-style:none;background-color:#f5f5f5;border-radius:4px;-webkit-border-radius:4px;-moz-border-radius:4px;border-radius:4px}.breadcrumb>li{display:inline-block;*display:inline;*zoom:1;text-shadow:0 1px 0 #fff}.breadcrumb>li>.divider{padding:0 5px;color:#ccc}
.breadcrumb>.active{color:#999}
.pagination{margin:20px 0}
.pagination ul{display:inline-block;*display:inline;*zoom:1;margin-left:0;margin-bottom:0;-webkit-border-radius:4px;-moz-border-radius:4px;border-radius:4px;-webkit-box-shadow:0 1px 2px rgba(0,0,0,0.05);-moz-box-shadow:0 1px 2px rgba(0,0,0,0.05);box-shadow:0 1px 2px rgba(0,0,0,0.05)}
.pagination ul>li{display:inline}
.pagination ul>li>a,.pagination ul>li>span{float:left;padding:4px 12px;line-height:20px;text-decoration:none;background-color:#fff;border:1px solid #ddd;border-left-width:0}
.pagination ul>li>a:hover,.pagination ul>li>a:focus,.pagination ul>.active>a,.pagination ul>.active>span{background-color:#f5f5f5}
.pagination ul>.active>a,.pagination ul>.active>span{color:#999;cursor:default}
.pagination ul>.disabled>span,.pagination ul>.disabled>a,.pagination ul>.disabled>a:hover,.pagination ul>.disabled>a:focus{color:#999;background-color:transparent;cursor:default}
.pagination ul>li:first-child>a,.pagination ul>li:first-child>span{border-left-width:1px;-webkit-border-top-left-radius:4px;-moz-border-radius-topleft:4px;border-top-left-radius:4px;-webkit-border-bottom-left-radius:4px;-moz-border-radius-bottomleft:4px;border-bottom-left-radius:4px}
.pagination ul>li:last-child>a,.pagination ul>li:last-child>span{-webkit-border-top-right-radius:4px;-moz-border-radius-topright:4px;border-top-right-radius:4px;-webkit-border-bottom-right-radius:4px;-moz-border-radius-bottomright:4px;border-bottom-right-radius:4px}
.pagination-centered{text-align:center}
.pagination-right{text-align:right}
.pagination-large ul>li>a,.pagination-large ul>li>span{padding:11px 19px;font-size:16.25px}
.pagination-large ul>li:first-child>a,.pagination-large ul>li:first-child>span{-webkit-border-top-left-radius:6px;-moz-border-radius-topleft:6px;border-top-left-radius:6px;-webkit-border-bottom-left-radius:6px;-moz-border-radius-bottomleft:6px;border-bottom-left-radius:6px}
.pagination-large ul>li:last-child>a,.pagination-large ul>li:last-child>span{-webkit-border-top-right-radius:6px;-moz-border-radius-topright:6px;border-top-right-radius:6px;-webkit-border-bottom-right-radius:6px;-moz-border-radius-bottomright:6px;border-bottom-right-radius:6px}
.pagination-mini ul>li:first-child>a,.pagination-small ul>li:first-child>a,.pagination-mini ul>li:first-child>span,.pagination-small ul>li:first-child>span{-webkit-border-top-left-radius:3px;-moz-border-radius-topleft:3px;border-top-left-radius:3px;-webkit-border-bottom-left-radius:3px;-moz-border-radius-bottomleft:3px;border-bottom-left-radius:3px}
.pagination-mini ul>li:last-child>a,.pagination-small ul>li:last-child>a,.pagination-mini ul>li:last-child>span,.pagination-small ul>li:last-child>span{-webkit-border-top-right-radius:3px;-moz-border-radius-topright:3px;border-top-right-radius:3px;-webkit-border-bottom-right-radius:3px;-moz-border-radius-bottomright:3px;border-bottom-right-radius:3px}
.pagination-small ul>li>a,.pagination-small ul>li>span{padding:2px 10px;font-size:11.049999999999999px}
.pagination-mini ul>li>a,.pagination-mini ul>li>span{padding:0 6px;font-size:9.75px}
.pager{margin:20px 0;list-style:none;text-align:center;*zoom:1}.pager:before,.pager:after{display:table;content:"";line-height:0}
.pager:after{clear:both}
.pager li{display:inline}
.pager li>a,.pager li>span{display:inline-block;padding:5px 14px;background-color:#fff;border:1px solid #ddd;border-radius:15px;-webkit-border-radius:15px;-moz-border-radius:15px;border-radius:15px}
.pager li>a:hover,.pager li>a:focus{text-decoration:none;background-color:#f5f5f5}
.pager .next>a,.pager .next>span{float:right}
.pager .previous>a,.pager .previous>span{float:left}
.pager .disabled>a,.pager .disabled>a:hover,.pager .disabled>a:focus,.pager .disabled>span{color:#999;background-color:#fff;cursor:default}
.modal-backdrop{position:fixed;top:0;right:0;bottom:0;left:0;z-index:1040;background-color:#000}.modal-backdrop.fade{opacity:0}
.modal-backdrop,.modal-backdrop.fade.in{opacity:.8;filter:alpha(opacity=80)}
.modal{position:fixed;top:10%;left:50%;z-index:1050;width:560px;margin-left:-280px;background-color:#fff;border:1px solid #999;border:1px solid rgba(0,0,0,0.3);*border:1px solid #999;-webkit-border-radius:6px;-moz-border-radius:6px;border-radius:6px;-webkit-box-shadow:0 3px 7px rgba(0,0,0,0.3);-moz-box-shadow:0 3px 7px rgba(0,0,0,0.3);box-shadow:0 3px 7px rgba(0,0,0,0.3);-webkit-background-clip:padding-box;-moz-background-clip:padding-box;background-clip:padding-box;outline:none}.modal.fade{-webkit-transition:opacity .3s linear, top .3s ease-out;-moz-transition:opacity .3s linear, top .3s ease-out;-o-transition:opacity .3s linear, top .3s ease-out;transition:opacity .3s linear, top .3s ease-out;top:-25%}
.modal.fade.in{top:10%}
.modal-header{padding:9px 15px;border-bottom:1px solid #eee}.modal-header .close{margin-top:2px}
.modal-header h3{margin:0;line-height:30px}
.modal-body{position:relative;overflow-y:auto;max-height:400px;padding:15px}
.modal-form{margin-bottom:0}
.modal-footer{padding:14px 15px 15px;margin-bottom:0;text-align:right;background-color:#f5f5f5;border-top:1px solid #ddd;-webkit-border-radius:0 0 6px 6px;-moz-border-radius:0 0 6px 6px;border-radius:0 0 6px 6px;-webkit-box-shadow:inset 0 1px 0 #fff;-moz-box-shadow:inset 0 1px 0 #fff;box-shadow:inset 0 1px 0 #fff;*zoom:1}.modal-footer:before,.modal-footer:after{display:table;content:"";line-height:0}
.modal-footer:after{clear:both}
.modal-footer .btn+.btn{margin-left:5px;margin-bottom:0}
.modal-footer .btn-group .btn+.btn{margin-left:-1px}
.modal-footer .btn-block+.btn-block{margin-left:0}
.tooltip{position:absolute;z-index:1030;display:block;visibility:visible;font-size:11px;line-height:1.4;opacity:0;filter:alpha(opacity=0)}.tooltip.in{opacity:.8;filter:alpha(opacity=80)}
.tooltip.top{margin-top:-3px;padding:5px 0}
.tooltip.right{margin-left:3px;padding:0 5px}
.tooltip.bottom{margin-top:3px;padding:5px 0}
.tooltip.left{margin-left:-3px;padding:0 5px}
.tooltip-inner{max-width:200px;padding:8px;color:#fff;text-align:center;text-decoration:none;background-color:#000;border-radius:4px;-webkit-border-radius:4px;-moz-border-radius:4px;border-radius:4px}
.tooltip-arrow{position:absolute;width:0;height:0;border-color:transparent;border-style:solid}
.tooltip.top .tooltip-arrow{bottom:0;left:50%;margin-left:-5px;border-width:5px 5px 0;border-top-color:#000}
.tooltip.right .tooltip-arrow{top:50%;left:0;margin-top:-5px;border-width:5px 5px 5px 0;border-right-color:#000}
.tooltip.left .tooltip-arrow{top:50%;right:0;margin-top:-5px;border-width:5px 0 5px 5px;border-left-color:#000}
.tooltip.bottom .tooltip-arrow{top:0;left:50%;margin-left:-5px;border-width:0 5px 5px;border-bottom-color:#000}
.popover{position:absolute;top:0;left:0;z-index:1010;display:none;max-width:276px;padding:1px;text-align:left;background-color:#fff;-webkit-background-clip:padding-box;-moz-background-clip:padding;background-clip:padding-box;border:1px solid #ccc;border:1px solid rgba(0,0,0,0.2);-webkit-border-radius:6px;-moz-border-radius:6px;border-radius:6px;-webkit-box-shadow:0 5px 10px rgba(0,0,0,0.2);-moz-box-shadow:0 5px 10px rgba(0,0,0,0.2);box-shadow:0 5px 10px rgba(0,0,0,0.2);white-space:normal}.popover.top{margin-top:-10px}
.popover.right{margin-left:10px}
.popover.bottom{margin-top:10px}
.popover.left{margin-left:-10px}
.popover-title{margin:0;padding:8px 14px;font-size:14px;font-weight:normal;line-height:18px;background-color:#f7f7f7;border-bottom:1px solid #ebebeb;border-radius:5px 5px 0 0;-webkit-border-radius:5px 5px 0 0;-moz-border-radius:5px 5px 0 0;border-radius:5px 5px 0 0}.popover-title:empty{display:none}
.popover-content{padding:9px 14px}
.popover .arrow,.popover .arrow:after{position:absolute;display:block;width:0;height:0;border-color:transparent;border-style:solid}
.popover .arrow{border-width:11px}
.popover .arrow:after{border-width:10px;content:""}
.popover.top .arrow{left:50%;margin-left:-11px;border-bottom-width:0;border-top-color:#999;border-top-color:rgba(0,0,0,0.25);bottom:-11px}.popover.top .arrow:after{bottom:1px;margin-left:-10px;border-bottom-width:0;border-top-color:#fff}
.popover.right .arrow{top:50%;left:-11px;margin-top:-11px;border-left-width:0;border-right-color:#999;border-right-color:rgba(0,0,0,0.25)}.popover.right .arrow:after{left:1px;bottom:-10px;border-left-width:0;border-right-color:#fff}
.popover.bottom .arrow{left:50%;margin-left:-11px;border-top-width:0;border-bottom-color:#999;border-bottom-color:rgba(0,0,0,0.25);top:-11px}.popover.bottom .arrow:after{top:1px;margin-left:-10px;border-top-width:0;border-bottom-color:#fff}
.popover.left .arrow{top:50%;right:-11px;margin-top:-11px;border-right-width:0;border-left-color:#999;border-left-color:rgba(0,0,0,0.25)}.popover.left .arrow:after{right:1px;border-right-width:0;border-left-color:#fff;bottom:-10px}
.thumbnails{margin-left:-20px;list-style:none;*zoom:1}.thumbnails:before,.thumbnails:after{display:table;content:"";line-height:0}
.thumbnails:after{clear:both}
.row-fluid .thumbnails{margin-left:0}
.thumbnails>li{float:left;margin-bottom:20px;margin-left:20px}
.thumbnail{display:block;padding:4px;line-height:20px;border:1px solid #ddd;-webkit-border-radius:4px;-moz-border-radius:4px;border-radius:4px;-webkit-box-shadow:0 1px 3px rgba(0,0,0,0.055);-moz-box-shadow:0 1px 3px rgba(0,0,0,0.055);box-shadow:0 1px 3px rgba(0,0,0,0.055);-webkit-transition:all .2s ease-in-out;-moz-transition:all .2s ease-in-out;-o-transition:all .2s ease-in-out;transition:all .2s ease-in-out}
a.thumbnail:hover,a.thumbnail:focus{border-color:#08c;-webkit-box-shadow:0 1px 4px rgba(0,105,214,0.25);-moz-box-shadow:0 1px 4px rgba(0,105,214,0.25);box-shadow:0 1px 4px rgba(0,105,214,0.25)}
.thumbnail>img{display:block;max-width:100%;margin-left:auto;margin-right:auto}
.thumbnail .caption{padding:9px;color:#555}
.media,.media-body{overflow:hidden;*overflow:visible;zoom:1}
.media,.media .media{margin-top:15px}
.media:first-child{margin-top:0}
.media-object{display:block}
.media-heading{margin:0 0 5px}
.media>.pull-left{margin-right:10px}
.media>.pull-right{margin-left:10px}
.media-list{margin-left:0;list-style:none}
.label,.badge{display:inline-block;padding:2px 4px;font-size:10.998px;font-weight:bold;line-height:14px;color:#fff;vertical-align:baseline;white-space:nowrap;text-shadow:0 -1px 0 rgba(0,0,0,0.25);background-color:#999}
.label{border-radius:3px;-webkit-border-radius:3px;-moz-border-radius:3px;border-radius:3px}
.badge{padding-left:9px;padding-right:9px;border-radius:9px;-webkit-border-radius:9px;-moz-border-radius:9px;border-radius:9px}
.label:empty,.badge:empty{display:none}
a.label:hover,a.label:focus,a.badge:hover,a.badge:focus{color:#fff;text-decoration:none;cursor:pointer}
.label-important,.badge-important{background-color:#b94a48}
.label-important[href],.badge-important[href]{background-color:#953b39}
.label-warning,.badge-warning{background-color:#f89406}
.label-warning[href],.badge-warning[href]{background-color:#c67605}
.label-success,.badge-success{background-color:#468847}
.label-success[href],.badge-success[href]{background-color:#356635}
.label-info,.badge-info{background-color:#3a87ad}
.label-info[href],.badge-info[href]{background-color:#2d6987}
.label-inverse,.badge-inverse{background-color:#333}
.label-inverse[href],.badge-inverse[href]{background-color:#1a1a1a}
.btn .label,.btn .badge{position:relative;top:-1px}
.btn-mini .label,.btn-mini .badge{top:0}
@-webkit-keyframes progress-bar-stripes{from{background-position:40px 0} to{background-position:0 0}}@-moz-keyframes progress-bar-stripes{from{background-position:40px 0} to{background-position:0 0}}@-ms-keyframes progress-bar-stripes{from{background-position:40px 0} to{background-position:0 0}}@-o-keyframes progress-bar-stripes{from{background-position:0 0} to{background-position:40px 0}}@keyframes progress-bar-stripes{from{background-position:40px 0} to{background-position:0 0}}.progress{overflow:hidden;height:20px;margin-bottom:20px;background-color:#f7f7f7;background-image:-moz-linear-gradient(top, #f5f5f5, #f9f9f9);background-image:-webkit-gradient(linear, 0 0, 0 100%, from(#f5f5f5), to(#f9f9f9));background-image:-webkit-linear-gradient(top, #f5f5f5, #f9f9f9);background-image:-o-linear-gradient(top, #f5f5f5, #f9f9f9);background-image:linear-gradient(to bottom, #f5f5f5, #f9f9f9);background-repeat:repeat-x;filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#fff5f5f5', endColorstr='#fff9f9f9', GradientType=0);-webkit-box-shadow:inset 0 1px 2px rgba(0,0,0,0.1);-moz-box-shadow:inset 0 1px 2px rgba(0,0,0,0.1);box-shadow:inset 0 1px 2px rgba(0,0,0,0.1);border-radius:4px;-webkit-border-radius:4px;-moz-border-radius:4px;border-radius:4px}
.progress .bar{width:0;height:100%;color:#fff;float:left;font-size:12px;text-align:center;text-shadow:0 -1px 0 rgba(0,0,0,0.25);background-color:#0e90d2;background-image:-moz-linear-gradient(top, #149bdf, #0480be);background-image:-webkit-gradient(linear, 0 0, 0 100%, from(#149bdf), to(#0480be));background-image:-webkit-linear-gradient(top, #149bdf, #0480be);background-image:-o-linear-gradient(top, #149bdf, #0480be);background-image:linear-gradient(to bottom, #149bdf, #0480be);background-repeat:repeat-x;filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#ff149bdf', endColorstr='#ff0480be', GradientType=0);-webkit-box-shadow:inset 0 -1px 0 rgba(0,0,0,0.15);-moz-box-shadow:inset 0 -1px 0 rgba(0,0,0,0.15);box-shadow:inset 0 -1px 0 rgba(0,0,0,0.15);-webkit-box-sizing:border-box;-moz-box-sizing:border-box;box-sizing:border-box;-webkit-transition:width .6s ease;-moz-transition:width .6s ease;-o-transition:width .6s ease;transition:width .6s ease}
.progress .bar+.bar{-webkit-box-shadow:inset 1px 0 0 rgba(0,0,0,.15), inset 0 -1px 0 rgba(0,0,0,.15);-moz-box-shadow:inset 1px 0 0 rgba(0,0,0,.15), inset 0 -1px 0 rgba(0,0,0,.15);box-shadow:inset 1px 0 0 rgba(0,0,0,.15), inset 0 -1px 0 rgba(0,0,0,.15)}
.progress-striped .bar{background-color:#149bdf;background-image:-webkit-gradient(linear, 0 100%, 100% 0, color-stop(.25, rgba(255,255,255,0.15)), color-stop(.25, transparent), color-stop(.5, transparent), color-stop(.5, rgba(255,255,255,0.15)), color-stop(.75, rgba(255,255,255,0.15)), color-stop(.75, transparent), to(transparent));background-image:-webkit-linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent);background-image:-moz-linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent);background-image:-o-linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent);background-image:linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent);-webkit-background-size:40px 40px;-moz-background-size:40px 40px;-o-background-size:40px 40px;background-size:40px 40px}
.progress.active .bar{-webkit-animation:progress-bar-stripes 2s linear infinite;-moz-animation:progress-bar-stripes 2s linear infinite;-ms-animation:progress-bar-stripes 2s linear infinite;-o-animation:progress-bar-stripes 2s linear infinite;animation:progress-bar-stripes 2s linear infinite}
.progress-danger .bar,.progress .bar-danger{background-color:#dd514c;background-image:-moz-linear-gradient(top, #ee5f5b, #c43c35);background-image:-webkit-gradient(linear, 0 0, 0 100%, from(#ee5f5b), to(#c43c35));background-image:-webkit-linear-gradient(top, #ee5f5b, #c43c35);background-image:-o-linear-gradient(top, #ee5f5b, #c43c35);background-image:linear-gradient(to bottom, #ee5f5b, #c43c35);background-repeat:repeat-x;filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#ffee5f5b', endColorstr='#ffc43c35', GradientType=0)}
.progress-danger.progress-striped .bar,.progress-striped .bar-danger{background-color:#ee5f5b;background-image:-webkit-gradient(linear, 0 100%, 100% 0, color-stop(.25, rgba(255,255,255,0.15)), color-stop(.25, transparent), color-stop(.5, transparent), color-stop(.5, rgba(255,255,255,0.15)), color-stop(.75, rgba(255,255,255,0.15)), color-stop(.75, transparent), to(transparent));background-image:-webkit-linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent);background-image:-moz-linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent);background-image:-o-linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent);background-image:linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent)}
.progress-success .bar,.progress .bar-success{background-color:#5eb95e;background-image:-moz-linear-gradient(top, #62c462, #57a957);background-image:-webkit-gradient(linear, 0 0, 0 100%, from(#62c462), to(#57a957));background-image:-webkit-linear-gradient(top, #62c462, #57a957);background-image:-o-linear-gradient(top, #62c462, #57a957);background-image:linear-gradient(to bottom, #62c462, #57a957);background-repeat:repeat-x;filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#ff62c462', endColorstr='#ff57a957', GradientType=0)}
.progress-success.progress-striped .bar,.progress-striped .bar-success{background-color:#62c462;background-image:-webkit-gradient(linear, 0 100%, 100% 0, color-stop(.25, rgba(255,255,255,0.15)), color-stop(.25, transparent), color-stop(.5, transparent), color-stop(.5, rgba(255,255,255,0.15)), color-stop(.75, rgba(255,255,255,0.15)), color-stop(.75, transparent), to(transparent));background-image:-webkit-linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent);background-image:-moz-linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent);background-image:-o-linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent);background-image:linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent)}
.progress-info .bar,.progress .bar-info{background-color:#4bb1cf;background-image:-moz-linear-gradient(top, #5bc0de, #339bb9);background-image:-webkit-gradient(linear, 0 0, 0 100%, from(#5bc0de), to(#339bb9));background-image:-webkit-linear-gradient(top, #5bc0de, #339bb9);background-image:-o-linear-gradient(top, #5bc0de, #339bb9);background-image:linear-gradient(to bottom, #5bc0de, #339bb9);background-repeat:repeat-x;filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#ff5bc0de', endColorstr='#ff339bb9', GradientType=0)}
.progress-info.progress-striped .bar,.progress-striped .bar-info{background-color:#5bc0de;background-image:-webkit-gradient(linear, 0 100%, 100% 0, color-stop(.25, rgba(255,255,255,0.15)), color-stop(.25, transparent), color-stop(.5, transparent), color-stop(.5, rgba(255,255,255,0.15)), color-stop(.75, rgba(255,255,255,0.15)), color-stop(.75, transparent), to(transparent));background-image:-webkit-linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent);background-image:-moz-linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent);background-image:-o-linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent);background-image:linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent)}
.progress-warning .bar,.progress .bar-warning{background-color:#faa732;background-image:-moz-linear-gradient(top, #fbb450, #f89406);background-image:-webkit-gradient(linear, 0 0, 0 100%, from(#fbb450), to(#f89406));background-image:-webkit-linear-gradient(top, #fbb450, #f89406);background-image:-o-linear-gradient(top, #fbb450, #f89406);background-image:linear-gradient(to bottom, #fbb450, #f89406);background-repeat:repeat-x;filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#fffbb450', endColorstr='#fff89406', GradientType=0)}
.progress-warning.progress-striped .bar,.progress-striped .bar-warning{background-color:#fbb450;background-image:-webkit-gradient(linear, 0 100%, 100% 0, color-stop(.25, rgba(255,255,255,0.15)), color-stop(.25, transparent), color-stop(.5, transparent), color-stop(.5, rgba(255,255,255,0.15)), color-stop(.75, rgba(255,255,255,0.15)), color-stop(.75, transparent), to(transparent));background-image:-webkit-linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent);background-image:-moz-linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent);background-image:-o-linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent);background-image:linear-gradient(45deg, rgba(255,255,255,0.15) 25%, transparent 25%, transparent 50%, rgba(255,255,255,0.15) 50%, rgba(255,255,255,0.15) 75%, transparent 75%, transparent)}
.accordion{margin-bottom:20px}
.accordion-group{margin-bottom:2px;border:1px solid #e5e5e5;border-radius:4px;-webkit-border-radius:4px;-moz-border-radius:4px;border-radius:4px}
.accordion-heading{border-bottom:0}
.accordion-heading .accordion-toggle{display:block;padding:8px 15px}
.accordion-toggle{cursor:pointer}
.accordion-inner{padding:9px 15px;border-top:1px solid #e5e5e5}
.carousel{position:relative;margin-bottom:20px;line-height:1}
.carousel-inner{overflow:hidden;width:100%;position:relative}
.carousel-inner>.item{display:none;position:relative;-webkit-transition:.6s ease-in-out left;-moz-transition:.6s ease-in-out left;-o-transition:.6s ease-in-out left;transition:.6s ease-in-out left}.carousel-inner>.item>img,.carousel-inner>.item>a>img{display:block;line-height:1}
.carousel-inner>.active,.carousel-inner>.next,.carousel-inner>.prev{display:block}
.carousel-inner>.active{left:0}
.carousel-inner>.next,.carousel-inner>.prev{position:absolute;top:0;width:100%}
.carousel-inner>.next{left:100%}
.carousel-inner>.prev{left:-100%}
.carousel-inner>.next.left,.carousel-inner>.prev.right{left:0}
.carousel-inner>.active.left{left:-100%}
.carousel-inner>.active.right{left:100%}
.carousel-control{position:absolute;top:40%;left:15px;width:40px;height:40px;margin-top:-20px;font-size:60px;font-weight:100;line-height:30px;color:#fff;text-align:center;background:#222;border:3px solid #fff;-webkit-border-radius:23px;-moz-border-radius:23px;border-radius:23px;opacity:.5;filter:alpha(opacity=50)}.carousel-control.right{left:auto;right:15px}
.carousel-control:hover,.carousel-control:focus{color:#fff;text-decoration:none;opacity:.9;filter:alpha(opacity=90)}
.carousel-indicators{position:absolute;top:15px;right:15px;z-index:5;margin:0;list-style:none}.carousel-indicators li{display:block;float:left;width:10px;height:10px;margin-left:5px;text-indent:-999px;background-color:#ccc;background-color:rgba(255,255,255,0.25);border-radius:5px}
.carousel-indicators .active{background-color:#fff}
.carousel-caption{position:absolute;left:0;right:0;bottom:0;padding:15px;background:#333;background:rgba(0,0,0,0.75)}
.carousel-caption h4,.carousel-caption p{color:#fff;line-height:20px}
.carousel-caption h4{margin:0 0 5px}
.carousel-caption p{margin-bottom:0}
.hero-unit{padding:60px;margin-bottom:30px;font-size:18px;font-weight:200;line-height:30px;color:inherit;background-color:#eee;border-radius:6px;-webkit-border-radius:6px;-moz-border-radius:6px;border-radius:6px}.hero-unit h1{margin-bottom:0;font-size:60px;line-height:1;color:inherit;letter-spacing:-1px}
.hero-unit li{line-height:30px}
.pull-right{float:right}
.pull-left{float:left}
.hide{display:none}
.show{display:block}
.invisible{visibility:hidden}
.affix{position:fixed}
@-ms-viewport{width:device-width}.hidden{display:none;visibility:hidden}
.visible-phone{display:none !important}
.visible-tablet{display:none !important}
.hidden-desktop{display:none !important}
.visible-desktop{display:inherit !important}
@media (min-width:768px) and (max-width:979px){.hidden-desktop{display:inherit !important} .visible-desktop{display:none !important} .visible-tablet{display:inherit !important} .hidden-tablet{display:none !important}}@media (max-width:767px){.hidden-desktop{display:inherit !important} .visible-desktop{display:none !important} .visible-phone{display:inherit !important} .hidden-phone{display:none !important}}.visible-print{display:none !important}
@media print{.visible-print{display:inherit !important} .hidden-print{display:none !important}}@media (min-width:1200px){.row{margin-left:-30px;*zoom:1}.row:before,.row:after{display:table;content:"";line-height:0} .row:after{clear:both} [class*="span"]{float:left;min-height:1px;margin-left:30px} .container,.navbar-static-top .container,.navbar-fixed-top .container,.navbar-fixed-bottom .container{width:1170px} .span12{width:1170px} .span11{width:1070px} .span10{width:970px} .span9{width:870px} .span8{width:770px} .span7{width:670px} .span6{width:570px} .span5{width:470px} .span4{width:370px} .span3{width:270px} .span2{width:170px} .span1{width:70px} .offset12{margin-left:1230px} .offset11{margin-left:1130px} .offset10{margin-left:1030px} .offset9{margin-left:930px} .offset8{margin-left:830px} .offset7{margin-left:730px} .offset6{margin-left:630px} .offset5{margin-left:530px} .offset4{margin-left:430px} .offset3{margin-left:330px} .offset2{margin-left:230px} .offset1{margin-left:130px} .row-fluid{width:100%;*zoom:1}.row-fluid:before,.row-fluid:after{display:table;content:"";line-height:0} .row-fluid:after{clear:both} .row-fluid [class*="span"]{display:block;width:100%;min-height:30px;-webkit-box-sizing:border-box;-moz-box-sizing:border-box;box-sizing:border-box;float:left;margin-left:2.564102564102564%;*margin-left:2.5109110747408616%} .row-fluid [class*="span"]:first-child{margin-left:0} .row-fluid .controls-row [class*="span"]+[class*="span"]{margin-left:2.564102564102564%} .row-fluid .span12{width:100%;*width:99.94680851063829%} .row-fluid .span11{width:91.45299145299145%;*width:91.39979996362975%} .row-fluid .span10{width:82.90598290598291%;*width:82.8527914166212%} .row-fluid .span9{width:74.35897435897436%;*width:74.30578286961266%} .row-fluid .span8{width:65.81196581196582%;*width:65.75877432260411%} .row-fluid .span7{width:57.26495726495726%;*width:57.21176577559556%} .row-fluid .span6{width:48.717948717948715%;*width:48.664757228587014%} .row-fluid .span5{width:40.17094017094017%;*width:40.11774868157847%} .row-fluid .span4{width:31.623931623931625%;*width:31.570740134569924%} .row-fluid .span3{width:23.076923076923077%;*width:23.023731587561375%} .row-fluid .span2{width:14.52991452991453%;*width:14.476723040552828%} .row-fluid .span1{width:5.982905982905983%;*width:5.929714493544281%} .row-fluid .offset12{margin-left:105.12820512820512%;*margin-left:105.02182214948171%} .row-fluid .offset12:first-child{margin-left:102.56410256410257%;*margin-left:102.45771958537915%} .row-fluid .offset11{margin-left:96.58119658119658%;*margin-left:96.47481360247316%} .row-fluid .offset11:first-child{margin-left:94.01709401709402%;*margin-left:93.91071103837061%} .row-fluid .offset10{margin-left:88.03418803418803%;*margin-left:87.92780505546462%} .row-fluid .offset10:first-child{margin-left:85.47008547008548%;*margin-left:85.36370249136206%} .row-fluid .offset9{margin-left:79.48717948717949%;*margin-left:79.38079650845607%} .row-fluid .offset9:first-child{margin-left:76.92307692307693%;*margin-left:76.81669394435352%} .row-fluid .offset8{margin-left:70.94017094017094%;*margin-left:70.83378796144753%} .row-fluid .offset8:first-child{margin-left:68.37606837606839%;*margin-left:68.26968539734497%} .row-fluid .offset7{margin-left:62.393162393162385%;*margin-left:62.28677941443899%} .row-fluid .offset7:first-child{margin-left:59.82905982905982%;*margin-left:59.72267685033642%} .row-fluid .offset6{margin-left:53.84615384615384%;*margin-left:53.739770867430444%} .row-fluid .offset6:first-child{margin-left:51.28205128205128%;*margin-left:51.175668303327875%} .row-fluid .offset5{margin-left:45.299145299145295%;*margin-left:45.1927623204219%} .row-fluid .offset5:first-child{margin-left:42.73504273504273%;*margin-left:42.62865975631933%} .row-fluid .offset4{margin-left:36.75213675213675%;*margin-left:36.645753773413354%} .row-fluid .offset4:first-child{margin-left:34.18803418803419%;*margin-left:34.081651209310785%} .row-fluid .offset3{margin-left:28.205128205128204%;*margin-left:28.0987452264048%} .row-fluid .offset3:first-child{margin-left:25.641025641025642%;*margin-left:25.53464266230224%} .row-fluid .offset2{margin-left:19.65811965811966%;*margin-left:19.551736679396257%} .row-fluid .offset2:first-child{margin-left:17.094017094017094%;*margin-left:16.98763411529369%} .row-fluid .offset1{margin-left:11.11111111111111%;*margin-left:11.004728132387708%} .row-fluid .offset1:first-child{margin-left:8.547008547008547%;*margin-left:8.440625568285142%} input,textarea,.uneditable-input{margin-left:0} .controls-row [class*="span"]+[class*="span"]{margin-left:30px} input.span12,textarea.span12,.uneditable-input.span12{width:1156px} input.span11,textarea.span11,.uneditable-input.span11{width:1056px} input.span10,textarea.span10,.uneditable-input.span10{width:956px} input.span9,textarea.span9,.uneditable-input.span9{width:856px} input.span8,textarea.span8,.uneditable-input.span8{width:756px} input.span7,textarea.span7,.uneditable-input.span7{width:656px} input.span6,textarea.span6,.uneditable-input.span6{width:556px} input.span5,textarea.span5,.uneditable-input.span5{width:456px} input.span4,textarea.span4,.uneditable-input.span4{width:356px} input.span3,textarea.span3,.uneditable-input.span3{width:256px} input.span2,textarea.span2,.uneditable-input.span2{width:156px} input.span1,textarea.span1,.uneditable-input.span1{width:56px} .thumbnails{margin-left:-30px} .thumbnails>li{margin-left:30px} .row-fluid .thumbnails{margin-left:0}}@media (min-width:768px) and (max-width:979px){.row{margin-left:-20px;*zoom:1}.row:before,.row:after{display:table;content:"";line-height:0} .row:after{clear:both} [class*="span"]{float:left;min-height:1px;margin-left:20px} .container,.navbar-static-top .container,.navbar-fixed-top .container,.navbar-fixed-bottom .container{width:724px} .span12{width:724px} .span11{width:662px} .span10{width:600px} .span9{width:538px} .span8{width:476px} .span7{width:414px} .span6{width:352px} .span5{width:290px} .span4{width:228px} .span3{width:166px} .span2{width:104px} .span1{width:42px} .offset12{margin-left:764px} .offset11{margin-left:702px} .offset10{margin-left:640px} .offset9{margin-left:578px} .offset8{margin-left:516px} .offset7{margin-left:454px} .offset6{margin-left:392px} .offset5{margin-left:330px} .offset4{margin-left:268px} .offset3{margin-left:206px} .offset2{margin-left:144px} .offset1{margin-left:82px} .row-fluid{width:100%;*zoom:1}.row-fluid:before,.row-fluid:after{display:table;content:"";line-height:0} .row-fluid:after{clear:both} .row-fluid [class*="span"]{display:block;width:100%;min-height:30px;-webkit-box-sizing:border-box;-moz-box-sizing:border-box;box-sizing:border-box;float:left;margin-left:2.7624309392265194%;*margin-left:2.709239449864817%} .row-fluid [class*="span"]:first-child{margin-left:0} .row-fluid .controls-row [class*="span"]+[class*="span"]{margin-left:2.7624309392265194%} .row-fluid .span12{width:100%;*width:99.94680851063829%} .row-fluid .span11{width:91.43646408839778%;*width:91.38327259903608%} .row-fluid .span10{width:82.87292817679558%;*width:82.81973668743387%} .row-fluid .span9{width:74.30939226519337%;*width:74.25620077583166%} .row-fluid .span8{width:65.74585635359117%;*width:65.69266486422946%} .row-fluid .span7{width:57.18232044198895%;*width:57.12912895262725%} .row-fluid .span6{width:48.61878453038674%;*width:48.56559304102504%} .row-fluid .span5{width:40.05524861878453%;*width:40.00205712942283%} .row-fluid .span4{width:31.491712707182323%;*width:31.43852121782062%} .row-fluid .span3{width:22.92817679558011%;*width:22.87498530621841%} .row-fluid .span2{width:14.3646408839779%;*width:14.311449394616199%} .row-fluid .span1{width:5.801104972375691%;*width:5.747913483013988%} .row-fluid .offset12{margin-left:105.52486187845304%;*margin-left:105.41847889972962%} .row-fluid .offset12:first-child{margin-left:102.76243093922652%;*margin-left:102.6560479605031%} .row-fluid .offset11{margin-left:96.96132596685082%;*margin-left:96.8549429881274%} .row-fluid .offset11:first-child{margin-left:94.1988950276243%;*margin-left:94.09251204890089%} .row-fluid .offset10{margin-left:88.39779005524862%;*margin-left:88.2914070765252%} .row-fluid .offset10:first-child{margin-left:85.6353591160221%;*margin-left:85.52897613729868%} .row-fluid .offset9{margin-left:79.8342541436464%;*margin-left:79.72787116492299%} .row-fluid .offset9:first-child{margin-left:77.07182320441989%;*margin-left:76.96544022569647%} .row-fluid .offset8{margin-left:71.2707182320442%;*margin-left:71.16433525332079%} .row-fluid .offset8:first-child{margin-left:68.50828729281768%;*margin-left:68.40190431409427%} .row-fluid .offset7{margin-left:62.70718232044199%;*margin-left:62.600799341718584%} .row-fluid .offset7:first-child{margin-left:59.94475138121547%;*margin-left:59.838368402492065%} .row-fluid .offset6{margin-left:54.14364640883978%;*margin-left:54.037263430116376%} .row-fluid .offset6:first-child{margin-left:51.38121546961326%;*margin-left:51.27483249088986%} .row-fluid .offset5{margin-left:45.58011049723757%;*margin-left:45.47372751851417%} .row-fluid .offset5:first-child{margin-left:42.81767955801105%;*margin-left:42.71129657928765%} .row-fluid .offset4{margin-left:37.01657458563536%;*margin-left:36.91019160691196%} .row-fluid .offset4:first-child{margin-left:34.25414364640884%;*margin-left:34.14776066768544%} .row-fluid .offset3{margin-left:28.45303867403315%;*margin-left:28.346655695309746%} .row-fluid .offset3:first-child{margin-left:25.69060773480663%;*margin-left:25.584224756083227%} .row-fluid .offset2{margin-left:19.88950276243094%;*margin-left:19.783119783707537%} .row-fluid .offset2:first-child{margin-left:17.12707182320442%;*margin-left:17.02068884448102%} .row-fluid .offset1{margin-left:11.32596685082873%;*margin-left:11.219583872105325%} .row-fluid .offset1:first-child{margin-left:8.56353591160221%;*margin-left:8.457152932878806%} input,textarea,.uneditable-input{margin-left:0} .controls-row [class*="span"]+[class*="span"]{margin-left:20px} input.span12,textarea.span12,.uneditable-input.span12{width:710px} input.span11,textarea.span11,.uneditable-input.span11{width:648px} input.span10,textarea.span10,.uneditable-input.span10{width:586px} input.span9,textarea.span9,.uneditable-input.span9{width:524px} input.span8,textarea.span8,.uneditable-input.span8{width:462px} input.span7,textarea.span7,.uneditable-input.span7{width:400px} input.span6,textarea.span6,.uneditable-input.span6{width:338px} input.span5,textarea.span5,.uneditable-input.span5{width:276px} input.span4,textarea.span4,.uneditable-input.span4{width:214px} input.span3,textarea.span3,.uneditable-input.span3{width:152px} input.span2,textarea.span2,.uneditable-input.span2{width:90px} input.span1,textarea.span1,.uneditable-input.span1{width:28px}}@media (max-width:767px){body{padding-left:20px;padding-right:20px} .navbar-fixed-top,.navbar-fixed-bottom,.navbar-static-top{margin-left:-20px;margin-right:-20px} .container-fluid{padding:0} .dl-horizontal dt{float:none;clear:none;width:auto;text-align:left} .dl-horizontal dd{margin-left:0} .container{width:auto} .row-fluid{width:100%} .row,.thumbnails{margin-left:0} .thumbnails>li{float:none;margin-left:0} [class*="span"],.uneditable-input[class*="span"],.row-fluid [class*="span"]{float:none;display:block;width:100%;margin-left:0;-webkit-box-sizing:border-box;-moz-box-sizing:border-box;box-sizing:border-box} .span12,.row-fluid .span12{width:100%;-webkit-box-sizing:border-box;-moz-box-sizing:border-box;box-sizing:border-box} .row-fluid [class*="offset"]:first-child{margin-left:0} .input-large,.input-xlarge,.input-xxlarge,input[class*="span"],select[class*="span"],textarea[class*="span"],.uneditable-input{display:block;width:100%;min-height:30px;-webkit-box-sizing:border-box;-moz-box-sizing:border-box;box-sizing:border-box} .input-prepend input,.input-append input,.input-prepend input[class*="span"],.input-append input[class*="span"]{display:inline-block;width:auto} .controls-row [class*="span"]+[class*="span"]{margin-left:0} .modal{position:fixed;top:20px;left:20px;right:20px;width:auto;margin:0}.modal.fade{top:-100px} .modal.fade.in{top:20px}}@media (max-width:480px){.nav-collapse{-webkit-transform:translate3d(0, 0, 0)} .page-header h1 small{display:block;line-height:20px} input[type="checkbox"],input[type="radio"]{border:1px solid #ccc} .form-horizontal .control-label{float:none;width:auto;padding-top:0;text-align:left} .form-horizontal .controls{margin-left:0} .form-horizontal .control-list{padding-top:0} .form-horizontal .form-actions{padding-left:10px;padding-right:10px} .media .pull-left,.media .pull-right{float:none;display:block;margin-bottom:10px} .media-object{margin-right:0;margin-left:0} .modal{top:10px;left:10px;right:10px} .modal-header .close{padding:10px;margin:-10px} .carousel-caption{position:static}}@media (max-width:979px){body{padding-top:0} .navbar-fixed-top,.navbar-fixed-bottom{position:static} .navbar-fixed-top{margin-bottom:20px} .navbar-fixed-bottom{margin-top:20px} .navbar-fixed-top .navbar-inner,.navbar-fixed-bottom .navbar-inner{padding:5px} .navbar .container{width:auto;padding:0} .navbar .brand{padding-left:10px;padding-right:10px;margin:0 0 0 -5px} .nav-collapse{clear:both} .nav-collapse .nav{float:none;margin:0 0 10px} .nav-collapse .nav>li{float:none} .nav-collapse .nav>li>a{margin-bottom:2px} .nav-collapse .nav>.divider-vertical{display:none} .nav-collapse .nav .nav-header{color:#777;text-shadow:none} .nav-collapse .nav>li>a,.nav-collapse .dropdown-menu a{padding:9px 15px;font-weight:bold;color:#777;border-radius:3px;-webkit-border-radius:3px;-moz-border-radius:3px;border-radius:3px} .nav-collapse .btn{padding:4px 10px 4px;font-weight:normal;border-radius:4px;-webkit-border-radius:4px;-moz-border-radius:4px;border-radius:4px} .nav-collapse .dropdown-menu li+li a{margin-bottom:2px} .nav-collapse .nav>li>a:hover,.nav-collapse .nav>li>a:focus,.nav-collapse .dropdown-menu a:hover,.nav-collapse .dropdown-menu a:focus{background-color:#f2f2f2} .navbar-inverse .nav-collapse .nav>li>a,.navbar-inverse .nav-collapse .dropdown-menu a{color:#999} .navbar-inverse .nav-collapse .nav>li>a:hover,.navbar-inverse .nav-collapse .nav>li>a:focus,.navbar-inverse .nav-collapse .dropdown-menu a:hover,.navbar-inverse .nav-collapse .dropdown-menu a:focus{background-color:#111} .nav-collapse.in .btn-group{margin-top:5px;padding:0} .nav-collapse .dropdown-menu{position:static;top:auto;left:auto;float:none;display:none;max-width:none;margin:0 15px;padding:0;background-color:transparent;border:none;-webkit-border-radius:0;-moz-border-radius:0;border-radius:0;-webkit-box-shadow:none;-moz-box-shadow:none;box-shadow:none} .nav-collapse .open>.dropdown-menu{display:block} .nav-collapse .dropdown-menu:before,.nav-collapse .dropdown-menu:after{display:none} .nav-collapse .dropdown-menu .divider{display:none} .nav-collapse .nav>li>.dropdown-menu:before,.nav-collapse .nav>li>.dropdown-menu:after{display:none} .nav-collapse .navbar-form,.nav-collapse .navbar-search{float:none;padding:10px 15px;margin:10px 0;border-top:1px solid #f2f2f2;border-bottom:1px solid #f2f2f2;-webkit-box-shadow:inset 0 1px 0 rgba(255,255,255,.1), 0 1px 0 rgba(255,255,255,.1);-moz-box-shadow:inset 0 1px 0 rgba(255,255,255,.1), 0 1px 0 rgba(255,255,255,.1);box-shadow:inset 0 1px 0 rgba(255,255,255,.1), 0 1px 0 rgba(255,255,255,.1)} .navbar-inverse .nav-collapse .navbar-form,.navbar-inverse .nav-collapse .navbar-search{border-top-color:#111;border-bottom-color:#111} .navbar .nav-collapse .nav.pull-right{float:none;margin-left:0} .nav-collapse,.nav-collapse.collapse{overflow:hidden;height:0} .navbar .btn-navbar{display:block} .navbar-static .navbar-inner{padding-left:10px;padding-right:10px}}@media (min-width:979px + 1){.nav-collapse.collapse{height:auto !important;overflow:visible !important}}@font-face{font-family:'FontAwesome';src:url('../components/font-awesome/font/fontawesome-webfont.eot?v=3.2.1');src:url('../components/font-awesome/font/fontawesome-webfont.eot?#iefix&v=3.2.1') format('embedded-opentype'),url('../components/font-awesome/font/fontawesome-webfont.woff?v=3.2.1') format('woff'),url('../components/font-awesome/font/fontawesome-webfont.ttf?v=3.2.1') format('truetype'),url('../components/font-awesome/font/fontawesome-webfont.svg#fontawesomeregular?v=3.2.1') format('svg');font-weight:normal;font-style:normal}[class^="icon-"],[class*=" icon-"]{font-family:FontAwesome;font-weight:normal;font-style:normal;text-decoration:inherit;-webkit-font-smoothing:antialiased;*margin-right:.3em}
[class^="icon-"]:before,[class*=" icon-"]:before{text-decoration:inherit;display:inline-block;speak:none}
.icon-large:before{vertical-align:-10%;font-size:1.3333333333333333em}
a [class^="icon-"],a [class*=" icon-"]{display:inline}
[class^="icon-"].icon-fixed-width,[class*=" icon-"].icon-fixed-width{display:inline-block;width:1.1428571428571428em;text-align:right;padding-right:.2857142857142857em}[class^="icon-"].icon-fixed-width.icon-large,[class*=" icon-"].icon-fixed-width.icon-large{width:1.4285714285714286em}
.icons-ul{margin-left:2.142857142857143em;list-style-type:none}.icons-ul>li{position:relative}
.icons-ul .icon-li{position:absolute;left:-2.142857142857143em;width:2.142857142857143em;text-align:center;line-height:inherit}
[class^="icon-"].hide,[class*=" icon-"].hide{display:none}
.icon-muted{color:#eee}
.icon-light{color:#fff}
.icon-dark{color:#333}
.icon-border{border:solid 1px #eee;padding:.2em .25em .15em;border-radius:3px;-webkit-border-radius:3px;-moz-border-radius:3px;border-radius:3px}
.icon-2x{font-size:2em}.icon-2x.icon-border{border-width:2px;border-radius:4px;-webkit-border-radius:4px;-moz-border-radius:4px;border-radius:4px}
.icon-3x{font-size:3em}.icon-3x.icon-border{border-width:3px;border-radius:5px;-webkit-border-radius:5px;-moz-border-radius:5px;border-radius:5px}
.icon-4x{font-size:4em}.icon-4x.icon-border{border-width:4px;border-radius:6px;-webkit-border-radius:6px;-moz-border-radius:6px;border-radius:6px}
.icon-5x{font-size:5em}.icon-5x.icon-border{border-width:5px;border-radius:7px;-webkit-border-radius:7px;-moz-border-radius:7px;border-radius:7px}
.pull-right{float:right}
.pull-left{float:left}
[class^="icon-"].pull-left,[class*=" icon-"].pull-left{margin-right:.3em}
[class^="icon-"].pull-right,[class*=" icon-"].pull-right{margin-left:.3em}
[class^="icon-"],[class*=" icon-"]{display:inline;width:auto;height:auto;line-height:normal;vertical-align:baseline;background-image:none;background-position:0 0;background-repeat:repeat;margin-top:0}
.icon-white,.nav-pills>.active>a>[class^="icon-"],.nav-pills>.active>a>[class*=" icon-"],.nav-list>.active>a>[class^="icon-"],.nav-list>.active>a>[class*=" icon-"],.navbar-inverse .nav>.active>a>[class^="icon-"],.navbar-inverse .nav>.active>a>[class*=" icon-"],.dropdown-menu>li>a:hover>[class^="icon-"],.dropdown-menu>li>a:hover>[class*=" icon-"],.dropdown-menu>.active>a>[class^="icon-"],.dropdown-menu>.active>a>[class*=" icon-"],.dropdown-submenu:hover>a>[class^="icon-"],.dropdown-submenu:hover>a>[class*=" icon-"]{background-image:none}
.btn [class^="icon-"].icon-large,.nav [class^="icon-"].icon-large,.btn [class*=" icon-"].icon-large,.nav [class*=" icon-"].icon-large{line-height:.9em}
.btn [class^="icon-"].icon-spin,.nav [class^="icon-"].icon-spin,.btn [class*=" icon-"].icon-spin,.nav [class*=" icon-"].icon-spin{display:inline-block}
.nav-tabs [class^="icon-"],.nav-pills [class^="icon-"],.nav-tabs [class*=" icon-"],.nav-pills [class*=" icon-"],.nav-tabs [class^="icon-"].icon-large,.nav-pills [class^="icon-"].icon-large,.nav-tabs [class*=" icon-"].icon-large,.nav-pills [class*=" icon-"].icon-large{line-height:.9em}
.btn [class^="icon-"].pull-left.icon-2x,.btn [class*=" icon-"].pull-left.icon-2x,.btn [class^="icon-"].pull-right.icon-2x,.btn [class*=" icon-"].pull-right.icon-2x{margin-top:.18em}
.btn [class^="icon-"].icon-spin.icon-large,.btn [class*=" icon-"].icon-spin.icon-large{line-height:.8em}
.btn.btn-small [class^="icon-"].pull-left.icon-2x,.btn.btn-small [class*=" icon-"].pull-left.icon-2x,.btn.btn-small [class^="icon-"].pull-right.icon-2x,.btn.btn-small [class*=" icon-"].pull-right.icon-2x{margin-top:.25em}
.btn.btn-large [class^="icon-"],.btn.btn-large [class*=" icon-"]{margin-top:0}.btn.btn-large [class^="icon-"].pull-left.icon-2x,.btn.btn-large [class*=" icon-"].pull-left.icon-2x,.btn.btn-large [class^="icon-"].pull-right.icon-2x,.btn.btn-large [class*=" icon-"].pull-right.icon-2x{margin-top:.05em}
.btn.btn-large [class^="icon-"].pull-left.icon-2x,.btn.btn-large [class*=" icon-"].pull-left.icon-2x{margin-right:.2em}
.btn.btn-large [class^="icon-"].pull-right.icon-2x,.btn.btn-large [class*=" icon-"].pull-right.icon-2x{margin-left:.2em}
.nav-list [class^="icon-"],.nav-list [class*=" icon-"]{line-height:inherit}
.icon-stack{position:relative;display:inline-block;width:2em;height:2em;line-height:2em;vertical-align:-35%}.icon-stack [class^="icon-"],.icon-stack [class*=" icon-"]{display:block;text-align:center;position:absolute;width:100%;height:100%;font-size:1em;line-height:inherit;*line-height:2em}
.icon-stack .icon-stack-base{font-size:2em;*line-height:1em}
.icon-spin{display:inline-block;-moz-animation:spin 2s infinite linear;-o-animation:spin 2s infinite linear;-webkit-animation:spin 2s infinite linear;animation:spin 2s infinite linear}
a .icon-stack,a .icon-spin{display:inline-block;text-decoration:none}
@-moz-keyframes spin{0%{-moz-transform:rotate(0deg)} 100%{-moz-transform:rotate(359deg)}}@-webkit-keyframes spin{0%{-webkit-transform:rotate(0deg)} 100%{-webkit-transform:rotate(359deg)}}@-o-keyframes spin{0%{-o-transform:rotate(0deg)} 100%{-o-transform:rotate(359deg)}}@-ms-keyframes spin{0%{-ms-transform:rotate(0deg)} 100%{-ms-transform:rotate(359deg)}}@keyframes spin{0%{transform:rotate(0deg)} 100%{transform:rotate(359deg)}}.icon-rotate-90:before{-webkit-transform:rotate(90deg);-moz-transform:rotate(90deg);-ms-transform:rotate(90deg);-o-transform:rotate(90deg);transform:rotate(90deg);filter:progid:DXImageTransform.Microsoft.BasicImage(rotation=1)}
.icon-rotate-180:before{-webkit-transform:rotate(180deg);-moz-transform:rotate(180deg);-ms-transform:rotate(180deg);-o-transform:rotate(180deg);transform:rotate(180deg);filter:progid:DXImageTransform.Microsoft.BasicImage(rotation=2)}
.icon-rotate-270:before{-webkit-transform:rotate(270deg);-moz-transform:rotate(270deg);-ms-transform:rotate(270deg);-o-transform:rotate(270deg);transform:rotate(270deg);filter:progid:DXImageTransform.Microsoft.BasicImage(rotation=3)}
.icon-flip-horizontal:before{-webkit-transform:scale(-1, 1);-moz-transform:scale(-1, 1);-ms-transform:scale(-1, 1);-o-transform:scale(-1, 1);transform:scale(-1, 1)}
.icon-flip-vertical:before{-webkit-transform:scale(1, -1);-moz-transform:scale(1, -1);-ms-transform:scale(1, -1);-o-transform:scale(1, -1);transform:scale(1, -1)}
a .icon-rotate-90:before,a .icon-rotate-180:before,a .icon-rotate-270:before,a .icon-flip-horizontal:before,a .icon-flip-vertical:before{display:inline-block}
.icon-glass:before{content:"\f000"}
.icon-music:before{content:"\f001"}
.icon-search:before{content:"\f002"}
.icon-envelope-alt:before{content:"\f003"}
.icon-heart:before{content:"\f004"}
.icon-star:before{content:"\f005"}
.icon-star-empty:before{content:"\f006"}
.icon-user:before{content:"\f007"}
.icon-film:before{content:"\f008"}
.icon-th-large:before{content:"\f009"}
.icon-th:before{content:"\f00a"}
.icon-th-list:before{content:"\f00b"}
.icon-ok:before{content:"\f00c"}
.icon-remove:before{content:"\f00d"}
.icon-zoom-in:before{content:"\f00e"}
.icon-zoom-out:before{content:"\f010"}
.icon-power-off:before,.icon-off:before{content:"\f011"}
.icon-signal:before{content:"\f012"}
.icon-gear:before,.icon-cog:before{content:"\f013"}
.icon-trash:before{content:"\f014"}
.icon-home:before{content:"\f015"}
.icon-file-alt:before{content:"\f016"}
.icon-time:before{content:"\f017"}
.icon-road:before{content:"\f018"}
.icon-download-alt:before{content:"\f019"}
.icon-download:before{content:"\f01a"}
.icon-upload:before{content:"\f01b"}
.icon-inbox:before{content:"\f01c"}
.icon-play-circle:before{content:"\f01d"}
.icon-rotate-right:before,.icon-repeat:before{content:"\f01e"}
.icon-refresh:before{content:"\f021"}
.icon-list-alt:before{content:"\f022"}
.icon-lock:before{content:"\f023"}
.icon-flag:before{content:"\f024"}
.icon-headphones:before{content:"\f025"}
.icon-volume-off:before{content:"\f026"}
.icon-volume-down:before{content:"\f027"}
.icon-volume-up:before{content:"\f028"}
.icon-qrcode:before{content:"\f029"}
.icon-barcode:before{content:"\f02a"}
.icon-tag:before{content:"\f02b"}
.icon-tags:before{content:"\f02c"}
.icon-book:before{content:"\f02d"}
.icon-bookmark:before{content:"\f02e"}
.icon-print:before{content:"\f02f"}
.icon-camera:before{content:"\f030"}
.icon-font:before{content:"\f031"}
.icon-bold:before{content:"\f032"}
.icon-italic:before{content:"\f033"}
.icon-text-height:before{content:"\f034"}
.icon-text-width:before{content:"\f035"}
.icon-align-left:before{content:"\f036"}
.icon-align-center:before{content:"\f037"}
.icon-align-right:before{content:"\f038"}
.icon-align-justify:before{content:"\f039"}
.icon-list:before{content:"\f03a"}
.icon-indent-left:before{content:"\f03b"}
.icon-indent-right:before{content:"\f03c"}
.icon-facetime-video:before{content:"\f03d"}
.icon-picture:before{content:"\f03e"}
.icon-pencil:before{content:"\f040"}
.icon-map-marker:before{content:"\f041"}
.icon-adjust:before{content:"\f042"}
.icon-tint:before{content:"\f043"}
.icon-edit:before{content:"\f044"}
.icon-share:before{content:"\f045"}
.icon-check:before{content:"\f046"}
.icon-move:before{content:"\f047"}
.icon-step-backward:before{content:"\f048"}
.icon-fast-backward:before{content:"\f049"}
.icon-backward:before{content:"\f04a"}
.icon-play:before{content:"\f04b"}
.icon-pause:before{content:"\f04c"}
.icon-stop:before{content:"\f04d"}
.icon-forward:before{content:"\f04e"}
.icon-fast-forward:before{content:"\f050"}
.icon-step-forward:before{content:"\f051"}
.icon-eject:before{content:"\f052"}
.icon-chevron-left:before{content:"\f053"}
.icon-chevron-right:before{content:"\f054"}
.icon-plus-sign:before{content:"\f055"}
.icon-minus-sign:before{content:"\f056"}
.icon-remove-sign:before{content:"\f057"}
.icon-ok-sign:before{content:"\f058"}
.icon-question-sign:before{content:"\f059"}
.icon-info-sign:before{content:"\f05a"}
.icon-screenshot:before{content:"\f05b"}
.icon-remove-circle:before{content:"\f05c"}
.icon-ok-circle:before{content:"\f05d"}
.icon-ban-circle:before{content:"\f05e"}
.icon-arrow-left:before{content:"\f060"}
.icon-arrow-right:before{content:"\f061"}
.icon-arrow-up:before{content:"\f062"}
.icon-arrow-down:before{content:"\f063"}
.icon-mail-forward:before,.icon-share-alt:before{content:"\f064"}
.icon-resize-full:before{content:"\f065"}
.icon-resize-small:before{content:"\f066"}
.icon-plus:before{content:"\f067"}
.icon-minus:before{content:"\f068"}
.icon-asterisk:before{content:"\f069"}
.icon-exclamation-sign:before{content:"\f06a"}
.icon-gift:before{content:"\f06b"}
.icon-leaf:before{content:"\f06c"}
.icon-fire:before{content:"\f06d"}
.icon-eye-open:before{content:"\f06e"}
.icon-eye-close:before{content:"\f070"}
.icon-warning-sign:before{content:"\f071"}
.icon-plane:before{content:"\f072"}
.icon-calendar:before{content:"\f073"}
.icon-random:before{content:"\f074"}
.icon-comment:before{content:"\f075"}
.icon-magnet:before{content:"\f076"}
.icon-chevron-up:before{content:"\f077"}
.icon-chevron-down:before{content:"\f078"}
.icon-retweet:before{content:"\f079"}
.icon-shopping-cart:before{content:"\f07a"}
.icon-folder-close:before{content:"\f07b"}
.icon-folder-open:before{content:"\f07c"}
.icon-resize-vertical:before{content:"\f07d"}
.icon-resize-horizontal:before{content:"\f07e"}
.icon-bar-chart:before{content:"\f080"}
.icon-twitter-sign:before{content:"\f081"}
.icon-facebook-sign:before{content:"\f082"}
.icon-camera-retro:before{content:"\f083"}
.icon-key:before{content:"\f084"}
.icon-gears:before,.icon-cogs:before{content:"\f085"}
.icon-comments:before{content:"\f086"}
.icon-thumbs-up-alt:before{content:"\f087"}
.icon-thumbs-down-alt:before{content:"\f088"}
.icon-star-half:before{content:"\f089"}
.icon-heart-empty:before{content:"\f08a"}
.icon-signout:before{content:"\f08b"}
.icon-linkedin-sign:before{content:"\f08c"}
.icon-pushpin:before{content:"\f08d"}
.icon-external-link:before{content:"\f08e"}
.icon-signin:before{content:"\f090"}
.icon-trophy:before{content:"\f091"}
.icon-github-sign:before{content:"\f092"}
.icon-upload-alt:before{content:"\f093"}
.icon-lemon:before{content:"\f094"}
.icon-phone:before{content:"\f095"}
.icon-unchecked:before,.icon-check-empty:before{content:"\f096"}
.icon-bookmark-empty:before{content:"\f097"}
.icon-phone-sign:before{content:"\f098"}
.icon-twitter:before{content:"\f099"}
.icon-facebook:before{content:"\f09a"}
.icon-github:before{content:"\f09b"}
.icon-unlock:before{content:"\f09c"}
.icon-credit-card:before{content:"\f09d"}
.icon-rss:before{content:"\f09e"}
.icon-hdd:before{content:"\f0a0"}
.icon-bullhorn:before{content:"\f0a1"}
.icon-bell:before{content:"\f0a2"}
.icon-certificate:before{content:"\f0a3"}
.icon-hand-right:before{content:"\f0a4"}
.icon-hand-left:before{content:"\f0a5"}
.icon-hand-up:before{content:"\f0a6"}
.icon-hand-down:before{content:"\f0a7"}
.icon-circle-arrow-left:before{content:"\f0a8"}
.icon-circle-arrow-right:before{content:"\f0a9"}
.icon-circle-arrow-up:before{content:"\f0aa"}
.icon-circle-arrow-down:before{content:"\f0ab"}
.icon-globe:before{content:"\f0ac"}
.icon-wrench:before{content:"\f0ad"}
.icon-tasks:before{content:"\f0ae"}
.icon-filter:before{content:"\f0b0"}
.icon-briefcase:before{content:"\f0b1"}
.icon-fullscreen:before{content:"\f0b2"}
.icon-group:before{content:"\f0c0"}
.icon-link:before{content:"\f0c1"}
.icon-cloud:before{content:"\f0c2"}
.icon-beaker:before{content:"\f0c3"}
.icon-cut:before{content:"\f0c4"}
.icon-copy:before{content:"\f0c5"}
.icon-paperclip:before,.icon-paper-clip:before{content:"\f0c6"}
.icon-save:before{content:"\f0c7"}
.icon-sign-blank:before{content:"\f0c8"}
.icon-reorder:before{content:"\f0c9"}
.icon-list-ul:before{content:"\f0ca"}
.icon-list-ol:before{content:"\f0cb"}
.icon-strikethrough:before{content:"\f0cc"}
.icon-underline:before{content:"\f0cd"}
.icon-table:before{content:"\f0ce"}
.icon-magic:before{content:"\f0d0"}
.icon-truck:before{content:"\f0d1"}
.icon-pinterest:before{content:"\f0d2"}
.icon-pinterest-sign:before{content:"\f0d3"}
.icon-google-plus-sign:before{content:"\f0d4"}
.icon-google-plus:before{content:"\f0d5"}
.icon-money:before{content:"\f0d6"}
.icon-caret-down:before{content:"\f0d7"}
.icon-caret-up:before{content:"\f0d8"}
.icon-caret-left:before{content:"\f0d9"}
.icon-caret-right:before{content:"\f0da"}
.icon-columns:before{content:"\f0db"}
.icon-sort:before{content:"\f0dc"}
.icon-sort-down:before{content:"\f0dd"}
.icon-sort-up:before{content:"\f0de"}
.icon-envelope:before{content:"\f0e0"}
.icon-linkedin:before{content:"\f0e1"}
.icon-rotate-left:before,.icon-undo:before{content:"\f0e2"}
.icon-legal:before{content:"\f0e3"}
.icon-dashboard:before{content:"\f0e4"}
.icon-comment-alt:before{content:"\f0e5"}
.icon-comments-alt:before{content:"\f0e6"}
.icon-bolt:before{content:"\f0e7"}
.icon-sitemap:before{content:"\f0e8"}
.icon-umbrella:before{content:"\f0e9"}
.icon-paste:before{content:"\f0ea"}
.icon-lightbulb:before{content:"\f0eb"}
.icon-exchange:before{content:"\f0ec"}
.icon-cloud-download:before{content:"\f0ed"}
.icon-cloud-upload:before{content:"\f0ee"}
.icon-user-md:before{content:"\f0f0"}
.icon-stethoscope:before{content:"\f0f1"}
.icon-suitcase:before{content:"\f0f2"}
.icon-bell-alt:before{content:"\f0f3"}
.icon-coffee:before{content:"\f0f4"}
.icon-food:before{content:"\f0f5"}
.icon-file-text-alt:before{content:"\f0f6"}
.icon-building:before{content:"\f0f7"}
.icon-hospital:before{content:"\f0f8"}
.icon-ambulance:before{content:"\f0f9"}
.icon-medkit:before{content:"\f0fa"}
.icon-fighter-jet:before{content:"\f0fb"}
.icon-beer:before{content:"\f0fc"}
.icon-h-sign:before{content:"\f0fd"}
.icon-plus-sign-alt:before{content:"\f0fe"}
.icon-double-angle-left:before{content:"\f100"}
.icon-double-angle-right:before{content:"\f101"}
.icon-double-angle-up:before{content:"\f102"}
.icon-double-angle-down:before{content:"\f103"}
.icon-angle-left:before{content:"\f104"}
.icon-angle-right:before{content:"\f105"}
.icon-angle-up:before{content:"\f106"}
.icon-angle-down:before{content:"\f107"}
.icon-desktop:before{content:"\f108"}
.icon-laptop:before{content:"\f109"}
.icon-tablet:before{content:"\f10a"}
.icon-mobile-phone:before{content:"\f10b"}
.icon-circle-blank:before{content:"\f10c"}
.icon-quote-left:before{content:"\f10d"}
.icon-quote-right:before{content:"\f10e"}
.icon-spinner:before{content:"\f110"}
.icon-circle:before{content:"\f111"}
.icon-mail-reply:before,.icon-reply:before{content:"\f112"}
.icon-github-alt:before{content:"\f113"}
.icon-folder-close-alt:before{content:"\f114"}
.icon-folder-open-alt:before{content:"\f115"}
.icon-expand-alt:before{content:"\f116"}
.icon-collapse-alt:before{content:"\f117"}
.icon-smile:before{content:"\f118"}
.icon-frown:before{content:"\f119"}
.icon-meh:before{content:"\f11a"}
.icon-gamepad:before{content:"\f11b"}
.icon-keyboard:before{content:"\f11c"}
.icon-flag-alt:before{content:"\f11d"}
.icon-flag-checkered:before{content:"\f11e"}
.icon-terminal:before{content:"\f120"}
.icon-code:before{content:"\f121"}
.icon-reply-all:before{content:"\f122"}
.icon-mail-reply-all:before{content:"\f122"}
.icon-star-half-full:before,.icon-star-half-empty:before{content:"\f123"}
.icon-location-arrow:before{content:"\f124"}
.icon-crop:before{content:"\f125"}
.icon-code-fork:before{content:"\f126"}
.icon-unlink:before{content:"\f127"}
.icon-question:before{content:"\f128"}
.icon-info:before{content:"\f129"}
.icon-exclamation:before{content:"\f12a"}
.icon-superscript:before{content:"\f12b"}
.icon-subscript:before{content:"\f12c"}
.icon-eraser:before{content:"\f12d"}
.icon-puzzle-piece:before{content:"\f12e"}
.icon-microphone:before{content:"\f130"}
.icon-microphone-off:before{content:"\f131"}
.icon-shield:before{content:"\f132"}
.icon-calendar-empty:before{content:"\f133"}
.icon-fire-extinguisher:before{content:"\f134"}
.icon-rocket:before{content:"\f135"}
.icon-maxcdn:before{content:"\f136"}
.icon-chevron-sign-left:before{content:"\f137"}
.icon-chevron-sign-right:before{content:"\f138"}
.icon-chevron-sign-up:before{content:"\f139"}
.icon-chevron-sign-down:before{content:"\f13a"}
.icon-html5:before{content:"\f13b"}
.icon-css3:before{content:"\f13c"}
.icon-anchor:before{content:"\f13d"}
.icon-unlock-alt:before{content:"\f13e"}
.icon-bullseye:before{content:"\f140"}
.icon-ellipsis-horizontal:before{content:"\f141"}
.icon-ellipsis-vertical:before{content:"\f142"}
.icon-rss-sign:before{content:"\f143"}
.icon-play-sign:before{content:"\f144"}
.icon-ticket:before{content:"\f145"}
.icon-minus-sign-alt:before{content:"\f146"}
.icon-check-minus:before{content:"\f147"}
.icon-level-up:before{content:"\f148"}
.icon-level-down:before{content:"\f149"}
.icon-check-sign:before{content:"\f14a"}
.icon-edit-sign:before{content:"\f14b"}
.icon-external-link-sign:before{content:"\f14c"}
.icon-share-sign:before{content:"\f14d"}
.icon-compass:before{content:"\f14e"}
.icon-collapse:before{content:"\f150"}
.icon-collapse-top:before{content:"\f151"}
.icon-expand:before{content:"\f152"}
.icon-euro:before,.icon-eur:before{content:"\f153"}
.icon-gbp:before{content:"\f154"}
.icon-dollar:before,.icon-usd:before{content:"\f155"}
.icon-rupee:before,.icon-inr:before{content:"\f156"}
.icon-yen:before,.icon-jpy:before{content:"\f157"}
.icon-renminbi:before,.icon-cny:before{content:"\f158"}
.icon-won:before,.icon-krw:before{content:"\f159"}
.icon-bitcoin:before,.icon-btc:before{content:"\f15a"}
.icon-file:before{content:"\f15b"}
.icon-file-text:before{content:"\f15c"}
.icon-sort-by-alphabet:before{content:"\f15d"}
.icon-sort-by-alphabet-alt:before{content:"\f15e"}
.icon-sort-by-attributes:before{content:"\f160"}
.icon-sort-by-attributes-alt:before{content:"\f161"}
.icon-sort-by-order:before{content:"\f162"}
.icon-sort-by-order-alt:before{content:"\f163"}
.icon-thumbs-up:before{content:"\f164"}
.icon-thumbs-down:before{content:"\f165"}
.icon-youtube-sign:before{content:"\f166"}
.icon-youtube:before{content:"\f167"}
.icon-xing:before{content:"\f168"}
.icon-xing-sign:before{content:"\f169"}
.icon-youtube-play:before{content:"\f16a"}
.icon-dropbox:before{content:"\f16b"}
.icon-stackexchange:before{content:"\f16c"}
.icon-instagram:before{content:"\f16d"}
.icon-flickr:before{content:"\f16e"}
.icon-adn:before{content:"\f170"}
.icon-bitbucket:before{content:"\f171"}
.icon-bitbucket-sign:before{content:"\f172"}
.icon-tumblr:before{content:"\f173"}
.icon-tumblr-sign:before{content:"\f174"}
.icon-long-arrow-down:before{content:"\f175"}
.icon-long-arrow-up:before{content:"\f176"}
.icon-long-arrow-left:before{content:"\f177"}
.icon-long-arrow-right:before{content:"\f178"}
.icon-apple:before{content:"\f179"}
.icon-windows:before{content:"\f17a"}
.icon-android:before{content:"\f17b"}
.icon-linux:before{content:"\f17c"}
.icon-dribbble:before{content:"\f17d"}
.icon-skype:before{content:"\f17e"}
.icon-foursquare:before{content:"\f180"}
.icon-trello:before{content:"\f181"}
.icon-female:before{content:"\f182"}
.icon-male:before{content:"\f183"}
.icon-gittip:before{content:"\f184"}
.icon-sun:before{content:"\f185"}
.icon-moon:before{content:"\f186"}
.icon-archive:before{content:"\f187"}
.icon-bug:before{content:"\f188"}
.icon-vk:before{content:"\f189"}
.icon-weibo:before{content:"\f18a"}
.icon-renren:before{content:"\f18b"}
code{color:#000}
pre{font-size:inherit;line-height:inherit}
.border-box-sizing{box-sizing:border-box;-moz-box-sizing:border-box;-webkit-box-sizing:border-box}
.corner-all{border-radius:4px}
.hbox{display:-webkit-box;-webkit-box-orient:horizontal;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:horizontal;-moz-box-align:stretch;display:box;box-orient:horizontal;box-align:stretch;display:flex;flex-direction:row;align-items:stretch}
.hbox>*{-webkit-box-flex:0;-moz-box-flex:0;box-flex:0;flex:none}
.vbox{display:-webkit-box;-webkit-box-orient:vertical;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:vertical;-moz-box-align:stretch;display:box;box-orient:vertical;box-align:stretch;display:flex;flex-direction:column;align-items:stretch}
.vbox>*{-webkit-box-flex:0;-moz-box-flex:0;box-flex:0;flex:none}
.hbox.reverse,.vbox.reverse,.reverse{-webkit-box-direction:reverse;-moz-box-direction:reverse;box-direction:reverse;flex-direction:row-reverse}
.hbox.box-flex0,.vbox.box-flex0,.box-flex0{-webkit-box-flex:0;-moz-box-flex:0;box-flex:0;flex:none;width:auto}
.hbox.box-flex1,.vbox.box-flex1,.box-flex1{-webkit-box-flex:1;-moz-box-flex:1;box-flex:1;flex:1}
.hbox.box-flex,.vbox.box-flex,.box-flex{-webkit-box-flex:1;-moz-box-flex:1;box-flex:1;flex:1}
.hbox.box-flex2,.vbox.box-flex2,.box-flex2{-webkit-box-flex:2;-moz-box-flex:2;box-flex:2;flex:2}
.box-group1{-webkit-box-flex-group:1;-moz-box-flex-group:1;box-flex-group:1}
.box-group2{-webkit-box-flex-group:2;-moz-box-flex-group:2;box-flex-group:2}
.hbox.start,.vbox.start,.start{-webkit-box-pack:start;-moz-box-pack:start;box-pack:start;justify-content:flex-start}
.hbox.end,.vbox.end,.end{-webkit-box-pack:end;-moz-box-pack:end;box-pack:end;justify-content:flex-end}
.hbox.center,.vbox.center,.center{-webkit-box-pack:center;-moz-box-pack:center;box-pack:center;justify-content:center}
.hbox.align-start,.vbox.align-start,.align-start{-webkit-box-align:start;-moz-box-align:start;box-align:start;align-items:flex-start}
.hbox.align-end,.vbox.align-end,.align-end{-webkit-box-align:end;-moz-box-align:end;box-align:end;align-items:flex-end}
.hbox.align-center,.vbox.align-center,.align-center{-webkit-box-align:center;-moz-box-align:center;box-align:center;align-items:center}
div.error{margin:2em;text-align:center}
div.error>h1{font-size:500%;line-height:normal}
div.error>p{font-size:200%;line-height:normal}
div.traceback-wrapper{text-align:left;max-width:800px;margin:auto}
body{background-color:#fff;position:absolute;left:0;right:0;top:0;bottom:0;overflow:visible}
div#header{display:none}
#ipython_notebook{padding-left:16px}
#noscript{width:auto;padding-top:16px;padding-bottom:16px;text-align:center;font-size:22px;color:#f00;font-weight:bold}
#ipython_notebook img{font-family:Verdana,"Helvetica Neue",Arial,Helvetica,Geneva,sans-serif;height:24px;text-decoration:none;color:#000}
#site{width:100%;display:none}
.ui-button .ui-button-text{padding:.2em .8em;font-size:77%}
input.ui-button{padding:.3em .9em}
.navbar span{margin-top:3px}
span#login_widget{float:right}
.nav-header{text-transform:none}
.navbar-nobg{background-color:transparent;background-image:none}
#header>span{margin-top:10px}
.modal_stretch{display:-webkit-box;-webkit-box-orient:vertical;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:vertical;-moz-box-align:stretch;display:box;box-orient:vertical;box-align:stretch;display:flex;flex-direction:column;align-items:stretch;height:80%}.modal_stretch .modal-body{max-height:none;flex:1}
@media (min-width:768px){.modal{width:700px;margin-left:-350px}}.center-nav{display:inline-block;margin-bottom:-4px}
.alternate_upload{background-color:none;display:inline}
.alternate_upload.form{padding:0;margin:0}
.alternate_upload input.fileinput{background-color:#f00;position:relative;opacity:0;z-index:2;width:295px;margin-left:163px;cursor:pointer;height:26px}
ul#tabs{margin-bottom:4px}
ul#tabs a{padding-top:4px;padding-bottom:4px}
ul.breadcrumb a:focus,ul.breadcrumb a:hover{text-decoration:none}
ul.breadcrumb i.icon-home{font-size:16px;margin-right:4px}
ul.breadcrumb span{color:#5e5e5e}
.list_toolbar{padding:4px 0 4px 0}
.list_toolbar [class*="span"]{min-height:26px}
.list_header{font-weight:bold}
.list_container{margin-top:4px;margin-bottom:20px;border:1px solid #ababab;border-radius:4px}
.list_container>div{border-bottom:1px solid #ababab}.list_container>div:hover .list-item{background-color:#f00}
.list_container>div:last-child{border:none}
.list_item:hover .list_item{background-color:#ddd}
.list_item a{text-decoration:none}
.list_header>div,.list_item>div{padding-top:4px;padding-bottom:4px;padding-left:7px;padding-right:7px;height:22px;line-height:22px}
.item_name{line-height:22px;height:26px}
.item_icon{font-size:14px;color:#5e5e5e;margin-right:7px}
.item_buttons{line-height:1em}
.toolbar_info{height:26px;line-height:26px}
input.nbname_input,input.engine_num_input{padding-top:3px;padding-bottom:3px;height:14px;line-height:14px;margin:0}
input.engine_num_input{width:60px}
.highlight_text{color:#00f}
#project_name>.breadcrumb{padding:0;margin-bottom:0;background-color:transparent;font-weight:bold}
.folder_icon:before{font-family:FontAwesome;font-weight:normal;font-style:normal;text-decoration:inherit;-webkit-font-smoothing:antialiased;*margin-right:.3em;content:"\f114"}
.notebook_icon:before{font-family:FontAwesome;font-weight:normal;font-style:normal;text-decoration:inherit;-webkit-font-smoothing:antialiased;*margin-right:.3em;content:"\f02d"}
.ansibold{font-weight:bold}
.ansiblack{color:#000}
.ansired{color:#8b0000}
.ansigreen{color:#006400}
.ansiyellow{color:#a52a2a}
.ansiblue{color:#00008b}
.ansipurple{color:#9400d3}
.ansicyan{color:#4682b4}
.ansigray{color:#808080}
.ansibgblack{background-color:#000}
.ansibgred{background-color:#f00}
.ansibggreen{background-color:#008000}
.ansibgyellow{background-color:#ff0}
.ansibgblue{background-color:#00f}
.ansibgpurple{background-color:#f0f}
.ansibgcyan{background-color:#0ff}
.ansibggray{background-color:#808080}
div.cell{border:1px solid transparent;display:-webkit-box;-webkit-box-orient:vertical;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:vertical;-moz-box-align:stretch;display:box;box-orient:vertical;box-align:stretch;display:flex;flex-direction:column;align-items:stretch}div.cell.selected{border-radius:4px;border:thin #ababab solid}
div.cell.edit_mode{border-radius:4px;border:thin #008000 solid}
div.cell{width:100%;padding:5px 5px 5px 0;margin:0;outline:none}
div.prompt{min-width:11ex;padding:.4em;margin:0;font-family:monospace;text-align:right;line-height:1.21429em}
@media (max-width:480px){div.prompt{text-align:left}}div.inner_cell{display:-webkit-box;-webkit-box-orient:vertical;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:vertical;-moz-box-align:stretch;display:box;box-orient:vertical;box-align:stretch;display:flex;flex-direction:column;align-items:stretch;-webkit-box-flex:1;-moz-box-flex:1;box-flex:1;flex:1}
div.input_area{border:1px solid #cfcfcf;border-radius:4px;background:#f7f7f7;line-height:1.21429em}
div.prompt:empty{padding-top:0;padding-bottom:0}
div.input{page-break-inside:avoid;display:-webkit-box;-webkit-box-orient:horizontal;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:horizontal;-moz-box-align:stretch;display:box;box-orient:horizontal;box-align:stretch;display:flex;flex-direction:row;align-items:stretch}
@media (max-width:480px){div.input{display:-webkit-box;-webkit-box-orient:vertical;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:vertical;-moz-box-align:stretch;display:box;box-orient:vertical;box-align:stretch;display:flex;flex-direction:column;align-items:stretch}}div.input_prompt{color:#000080;border-top:1px solid transparent}
div.input_area>div.highlight{margin:.4em;border:none;padding:0;background-color:transparent}
div.input_area>div.highlight>pre{margin:0;border:none;padding:0;background-color:transparent}
.CodeMirror{line-height:1.21429em;height:auto;background:none;}
.CodeMirror-scroll{overflow-y:hidden;overflow-x:auto}
.CodeMirror-lines{padding:.4em}
.CodeMirror-linenumber{padding:0 8px 0 4px}
.CodeMirror-gutters{border-bottom-left-radius:4px;border-top-left-radius:4px}
.CodeMirror pre{padding:0;border:0;border-radius:0;-webkit-border-radius:0;-moz-border-radius:0;border-radius:0}
pre code{display:block;padding:.5em}
.highlight-base,pre code,pre .subst,pre .tag .title,pre .lisp .title,pre .clojure .built_in,pre .nginx .title{color:#000}
.highlight-string,pre .string,pre .constant,pre .parent,pre .tag .value,pre .rules .value,pre .rules .value .number,pre .preprocessor,pre .ruby .symbol,pre .ruby .symbol .string,pre .aggregate,pre .template_tag,pre .django .variable,pre .smalltalk .class,pre .addition,pre .flow,pre .stream,pre .bash .variable,pre .apache .tag,pre .apache .cbracket,pre .tex .command,pre .tex .special,pre .erlang_repl .function_or_atom,pre .markdown .header{color:#ba2121}
.highlight-comment,pre .comment,pre .annotation,pre .template_comment,pre .diff .header,pre .chunk,pre .markdown .blockquote{color:#408080;font-style:italic}
.highlight-number,pre .number,pre .date,pre .regexp,pre .literal,pre .smalltalk .symbol,pre .smalltalk .char,pre .go .constant,pre .change,pre .markdown .bullet,pre .markdown .link_url{color:#080}
pre .label,pre .javadoc,pre .ruby .string,pre .decorator,pre .filter .argument,pre .localvars,pre .array,pre .attr_selector,pre .important,pre .pseudo,pre .pi,pre .doctype,pre .deletion,pre .envvar,pre .shebang,pre .apache .sqbracket,pre .nginx .built_in,pre .tex .formula,pre .erlang_repl .reserved,pre .prompt,pre .markdown .link_label,pre .vhdl .attribute,pre .clojure .attribute,pre .coffeescript .property{color:#88f}
.highlight-keyword,pre .keyword,pre .id,pre .phpdoc,pre .aggregate,pre .css .tag,pre .javadoctag,pre .phpdoc,pre .yardoctag,pre .smalltalk .class,pre .winutils,pre .bash .variable,pre .apache .tag,pre .go .typename,pre .tex .command,pre .markdown .strong,pre .request,pre .status{color:#008000;font-weight:bold}
.highlight-builtin,pre .built_in{color:#008000}
pre .markdown .emphasis{font-style:italic}
pre .nginx .built_in{font-weight:normal}
pre .coffeescript .javascript,pre .javascript .xml,pre .tex .formula,pre .xml .javascript,pre .xml .vbscript,pre .xml .css,pre .xml .cdata{opacity:.5}
.cm-s-ipython span.cm-variable{color:#000}
.cm-s-ipython span.cm-keyword{color:#008000;font-weight:bold}
.cm-s-ipython span.cm-number{color:#080}
.cm-s-ipython span.cm-comment{color:#408080;font-style:italic}
.cm-s-ipython span.cm-string{color:#ba2121}
.cm-s-ipython span.cm-builtin{color:#008000}
.cm-s-ipython span.cm-error{color:#f00}
.cm-s-ipython span.cm-operator{color:#a2f;font-weight:bold}
.cm-s-ipython span.cm-meta{color:#a2f}
.cm-s-ipython span.cm-tab{background:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAMCAYAAAAkuj5RAAAAAXNSR0IArs4c6QAAAGFJREFUSMft1LsRQFAQheHPowAKoACx3IgEKtaEHujDjORSgWTH/ZOdnZOcM/sgk/kFFWY0qV8foQwS4MKBCS3qR6ixBJvElOobYAtivseIE120FaowJPN75GMu8j/LfMwNjh4HUpwg4LUAAAAASUVORK5CYII=);background-position:right;background-repeat:no-repeat}
div.output_wrapper{position:relative;display:-webkit-box;-webkit-box-orient:vertical;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:vertical;-moz-box-align:stretch;display:box;box-orient:vertical;box-align:stretch;display:flex;flex-direction:column;align-items:stretch}
div.output_scroll{height:24em;width:100%;overflow:auto;border-radius:4px;-webkit-box-shadow:inset 0 2px 8px rgba(0,0,0,0.8);-moz-box-shadow:inset 0 2px 8px rgba(0,0,0,0.8);box-shadow:inset 0 2px 8px rgba(0,0,0,0.8);display:block}
div.output_collapsed{margin:0;padding:0;display:-webkit-box;-webkit-box-orient:vertical;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:vertical;-moz-box-align:stretch;display:box;box-orient:vertical;box-align:stretch;display:flex;flex-direction:column;align-items:stretch}
div.out_prompt_overlay{height:100%;padding:0 .4em;position:absolute;border-radius:4px}
div.out_prompt_overlay:hover{-webkit-box-shadow:inset 0 0 1px #000;-moz-box-shadow:inset 0 0 1px #000;box-shadow:inset 0 0 1px #000;background:rgba(240,240,240,0.5)}
div.output_prompt{color:#8b0000}
div.output_area{padding:0;page-break-inside:avoid;display:-webkit-box;-webkit-box-orient:horizontal;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:horizontal;-moz-box-align:stretch;display:box;box-orient:horizontal;box-align:stretch;display:flex;flex-direction:row;align-items:stretch}div.output_area .MathJax_Display{text-align:left !important}
div.output_area .rendered_html table{margin-left:0;margin-right:0}
div.output_area .rendered_html img{margin-left:0;margin-right:0}
.output{display:-webkit-box;-webkit-box-orient:vertical;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:vertical;-moz-box-align:stretch;display:box;box-orient:vertical;box-align:stretch;display:flex;flex-direction:column;align-items:stretch}
@media (max-width:480px){div.output_area{display:-webkit-box;-webkit-box-orient:vertical;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:vertical;-moz-box-align:stretch;display:box;box-orient:vertical;box-align:stretch;display:flex;flex-direction:column;align-items:stretch}}div.output_area pre{margin:0;padding:0;border:0;vertical-align:baseline;color:#000;background-color:transparent;border-radius:0;-webkit-border-radius:0;-moz-border-radius:0;border-radius:0}
div.output_subarea{padding:.4em .4em 0 .4em;-webkit-box-flex:1;-moz-box-flex:1;box-flex:1;flex:1}
div.output_text{text-align:left;color:#000;line-height:1.21429em}
div.output_stderr{background:#fdd;}
div.output_latex{text-align:left}
div.output_javascript:empty{padding:0}
.js-error{color:#8b0000}
div.raw_input_container{font-family:monospace;padding-top:5px}
span.raw_input_prompt{}
input.raw_input{font-family:inherit;font-size:inherit;color:inherit;width:auto;vertical-align:baseline;padding:0 .25em;margin:0 .25em}
input.raw_input:focus{box-shadow:none}
p.p-space{margin-bottom:10px}
.rendered_html{color:#000;}.rendered_html em{font-style:italic}
.rendered_html strong{font-weight:bold}
.rendered_html u{text-decoration:underline}
.rendered_html :link{text-decoration:underline}
.rendered_html :visited{text-decoration:underline}
.rendered_html h1{font-size:185.7%;margin:1.08em 0 0 0;font-weight:bold;line-height:1}
.rendered_html h2{font-size:157.1%;margin:1.27em 0 0 0;font-weight:bold;line-height:1}
.rendered_html h3{font-size:128.6%;margin:1.55em 0 0 0;font-weight:bold;line-height:1}
.rendered_html h4{font-size:100%;margin:2em 0 0 0;font-weight:bold;line-height:1}
.rendered_html h5{font-size:100%;margin:2em 0 0 0;font-weight:bold;line-height:1;font-style:italic}
.rendered_html h6{font-size:100%;margin:2em 0 0 0;font-weight:bold;line-height:1;font-style:italic}
.rendered_html h1:first-child{margin-top:.538em}
.rendered_html h2:first-child{margin-top:.636em}
.rendered_html h3:first-child{margin-top:.777em}
.rendered_html h4:first-child{margin-top:1em}
.rendered_html h5:first-child{margin-top:1em}
.rendered_html h6:first-child{margin-top:1em}
.rendered_html ul{list-style:disc;margin:0 2em}
.rendered_html ul ul{list-style:square;margin:0 2em}
.rendered_html ul ul ul{list-style:circle;margin:0 2em}
.rendered_html ol{list-style:decimal;margin:0 2em}
.rendered_html ol ol{list-style:upper-alpha;margin:0 2em}
.rendered_html ol ol ol{list-style:lower-alpha;margin:0 2em}
.rendered_html ol ol ol ol{list-style:lower-roman;margin:0 2em}
.rendered_html ol ol ol ol ol{list-style:decimal;margin:0 2em}
.rendered_html *+ul{margin-top:1em}
.rendered_html *+ol{margin-top:1em}
.rendered_html hr{color:#000;background-color:#000}
.rendered_html pre{margin:1em 2em}
.rendered_html pre,.rendered_html code{border:0;background-color:#fff;color:#000;font-size:100%;padding:0}
.rendered_html blockquote{margin:1em 2em}
.rendered_html table{margin-left:auto;margin-right:auto;border:1px solid #000;border-collapse:collapse}
.rendered_html tr,.rendered_html th,.rendered_html td{border:1px solid #000;border-collapse:collapse;margin:1em 2em}
.rendered_html td,.rendered_html th{text-align:left;vertical-align:middle;padding:4px}
.rendered_html th{font-weight:bold}
.rendered_html *+table{margin-top:1em}
.rendered_html p{text-align:justify}
.rendered_html *+p{margin-top:1em}
.rendered_html img{display:block;margin-left:auto;margin-right:auto}
.rendered_html *+img{margin-top:1em}
div.text_cell{padding:5px 5px 5px 0;display:-webkit-box;-webkit-box-orient:horizontal;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:horizontal;-moz-box-align:stretch;display:box;box-orient:horizontal;box-align:stretch;display:flex;flex-direction:row;align-items:stretch}
@media (max-width:480px){div.text_cell>div.prompt{display:none}}div.text_cell_render{outline:none;resize:none;width:inherit;border-style:none;padding:.5em .5em .5em .4em;color:#000}
a.anchor-link:link{text-decoration:none;padding:0 20px;visibility:hidden}
h1:hover .anchor-link,h2:hover .anchor-link,h3:hover .anchor-link,h4:hover .anchor-link,h5:hover .anchor-link,h6:hover .anchor-link{visibility:visible}
div.cell.text_cell.rendered{padding:0}
.widget-area{page-break-inside:avoid;display:-webkit-box;-webkit-box-orient:horizontal;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:horizontal;-moz-box-align:stretch;display:box;box-orient:horizontal;box-align:stretch;display:flex;flex-direction:row;align-items:stretch}.widget-area .widget-subarea{padding:.44em .4em .4em 1px;margin-left:6px;box-sizing:border-box;-moz-box-sizing:border-box;-webkit-box-sizing:border-box;display:-webkit-box;-webkit-box-orient:vertical;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:vertical;-moz-box-align:stretch;display:box;box-orient:vertical;box-align:stretch;display:flex;flex-direction:column;align-items:stretch;-webkit-box-flex:2;-moz-box-flex:2;box-flex:2;flex:2;-webkit-box-align:start;-moz-box-align:start;box-align:start;align-items:flex-start}
.widget-hlabel{min-width:10ex;padding-right:8px;padding-top:3px;text-align:right;vertical-align:text-top}
.widget-vlabel{padding-bottom:5px;text-align:center;vertical-align:text-bottom}
.widget-hreadout{padding-left:8px;padding-top:3px;text-align:left;vertical-align:text-top}
.widget-vreadout{padding-top:5px;text-align:center;vertical-align:text-top}
.slide-track{border:1px solid #ccc;background:#fff;border-radius:4px;}
.widget-hslider{padding-left:8px;padding-right:5px;overflow:visible;width:348px;height:5px;max-height:5px;margin-top:11px;margin-bottom:10px;border:1px solid #ccc;background:#fff;border-radius:4px;display:-webkit-box;-webkit-box-orient:horizontal;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:horizontal;-moz-box-align:stretch;display:box;box-orient:horizontal;box-align:stretch;display:flex;flex-direction:row;align-items:stretch}.widget-hslider .ui-slider{border:0 !important;background:none !important;display:-webkit-box;-webkit-box-orient:horizontal;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:horizontal;-moz-box-align:stretch;display:box;box-orient:horizontal;box-align:stretch;display:flex;flex-direction:row;align-items:stretch;-webkit-box-flex:1;-moz-box-flex:1;box-flex:1;flex:1}.widget-hslider .ui-slider .ui-slider-handle{width:14px !important;height:28px !important;margin-top:-8px !important}
.widget-vslider{padding-bottom:8px;overflow:visible;width:5px;max-width:5px;height:250px;margin-left:12px;border:1px solid #ccc;background:#fff;border-radius:4px;display:-webkit-box;-webkit-box-orient:vertical;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:vertical;-moz-box-align:stretch;display:box;box-orient:vertical;box-align:stretch;display:flex;flex-direction:column;align-items:stretch}.widget-vslider .ui-slider{border:0 !important;background:none !important;margin-left:-4px;margin-top:5px;display:-webkit-box;-webkit-box-orient:vertical;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:vertical;-moz-box-align:stretch;display:box;box-orient:vertical;box-align:stretch;display:flex;flex-direction:column;align-items:stretch;-webkit-box-flex:1;-moz-box-flex:1;box-flex:1;flex:1}.widget-vslider .ui-slider .ui-slider-handle{width:28px !important;height:14px !important;margin-left:-9px}
.widget-text{width:350px;margin:0 !important}
.widget-listbox{width:364px;margin-bottom:0}
.widget-numeric-text{width:150px;margin:0 !important}
.widget-progress{width:363px}.widget-progress .bar{-webkit-transition:none;-moz-transition:none;-ms-transition:none;-o-transition:none;transition:none}
.widget-combo-btn{min-width:138px;}
.widget-box{margin:5px;-webkit-box-pack:start;-moz-box-pack:start;box-pack:start;justify-content:flex-start;box-sizing:border-box;-moz-box-sizing:border-box;-webkit-box-sizing:border-box;-webkit-box-align:start;-moz-box-align:start;box-align:start;align-items:flex-start}
.widget-hbox{margin:5px;-webkit-box-pack:start;-moz-box-pack:start;box-pack:start;justify-content:flex-start;box-sizing:border-box;-moz-box-sizing:border-box;-webkit-box-sizing:border-box;-webkit-box-align:start;-moz-box-align:start;box-align:start;align-items:flex-start;display:-webkit-box;-webkit-box-orient:horizontal;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:horizontal;-moz-box-align:stretch;display:box;box-orient:horizontal;box-align:stretch;display:flex;flex-direction:row;align-items:stretch}
.widget-hbox-single{margin:5px;-webkit-box-pack:start;-moz-box-pack:start;box-pack:start;justify-content:flex-start;box-sizing:border-box;-moz-box-sizing:border-box;-webkit-box-sizing:border-box;-webkit-box-align:start;-moz-box-align:start;box-align:start;align-items:flex-start;display:-webkit-box;-webkit-box-orient:horizontal;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:horizontal;-moz-box-align:stretch;display:box;box-orient:horizontal;box-align:stretch;display:flex;flex-direction:row;align-items:stretch;height:30px}
.widget-vbox{margin:5px;-webkit-box-pack:start;-moz-box-pack:start;box-pack:start;justify-content:flex-start;box-sizing:border-box;-moz-box-sizing:border-box;-webkit-box-sizing:border-box;-webkit-box-align:start;-moz-box-align:start;box-align:start;align-items:flex-start;display:-webkit-box;-webkit-box-orient:vertical;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:vertical;-moz-box-align:stretch;display:box;box-orient:vertical;box-align:stretch;display:flex;flex-direction:column;align-items:stretch}
.widget-vbox-single{margin:5px;-webkit-box-pack:start;-moz-box-pack:start;box-pack:start;justify-content:flex-start;box-sizing:border-box;-moz-box-sizing:border-box;-webkit-box-sizing:border-box;-webkit-box-align:start;-moz-box-align:start;box-align:start;align-items:flex-start;display:-webkit-box;-webkit-box-orient:vertical;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:vertical;-moz-box-align:stretch;display:box;box-orient:vertical;box-align:stretch;display:flex;flex-direction:column;align-items:stretch;width:30px}
.widget-modal{overflow:hidden;position:absolute !important;top:0;left:0;margin-left:0 !important}
.widget-modal-body{max-height:none !important}
.widget-container{box-sizing:border-box;-moz-box-sizing:border-box;-webkit-box-sizing:border-box;-webkit-box-align:start;-moz-box-align:start;box-align:start;align-items:flex-start}
.widget-radio-box{display:-webkit-box;-webkit-box-orient:vertical;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:vertical;-moz-box-align:stretch;display:box;box-orient:vertical;box-align:stretch;display:flex;flex-direction:column;align-items:stretch;box-sizing:border-box;-moz-box-sizing:border-box;-webkit-box-sizing:border-box;padding-top:4px}
.docked-widget-modal{overflow:hidden;position:relative !important;top:0 !important;left:0 !important;margin-left:0 !important}
body{background-color:#fff}
body.notebook_app{overflow:hidden}
@media (max-width:767px){body.notebook_app{padding-left:0;padding-right:0}}span#notebook_name{height:1em;line-height:1em;padding:3px;border:none;font-size:146.5%}
div#notebook_panel{margin:0 0 0 0;padding:0;-webkit-box-shadow:0 -1px 10px rgba(0,0,0,0.1);-moz-box-shadow:0 -1px 10px rgba(0,0,0,0.1);box-shadow:0 -1px 10px rgba(0,0,0,0.1)}
div#notebook{font-size:14px;line-height:20px;overflow-y:scroll;overflow-x:auto;width:100%;padding:1em 0 1em 0;margin:0;border-top:1px solid #ababab;outline:none;box-sizing:border-box;-moz-box-sizing:border-box;-webkit-box-sizing:border-box}
div.ui-widget-content{border:1px solid #ababab;outline:none}
pre.dialog{background-color:#f7f7f7;border:1px solid #ddd;border-radius:4px;padding:.4em;padding-left:2em}
p.dialog{padding:.2em}
pre,code,kbd,samp{white-space:pre-wrap}
#fonttest{font-family:monospace}
p{margin-bottom:0}
.end_space{height:200px}
.celltoolbar{border:thin solid #cfcfcf;border-bottom:none;background:#eee;border-radius:3px 3px 0 0;width:100%;-webkit-box-pack:end;height:22px;display:-webkit-box;-webkit-box-orient:horizontal;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:horizontal;-moz-box-align:stretch;display:box;box-orient:horizontal;box-align:stretch;display:flex;flex-direction:row;align-items:stretch;-webkit-box-direction:reverse;-moz-box-direction:reverse;box-direction:reverse;flex-direction:row-reverse}
.ctb_hideshow{display:none;vertical-align:bottom;padding-right:2px}
.celltoolbar>div{padding-top:0}
.ctb_global_show .ctb_show.ctb_hideshow{display:block}
.ctb_global_show .ctb_show+.input_area,.ctb_global_show .ctb_show+div.text_cell_input{border-top-right-radius:0;border-top-left-radius:0}
.celltoolbar .button_container select{margin:10px;margin-top:1px;margin-bottom:0;padding:0;font-size:87%;width:auto;display:inline-block;height:18px;line-height:18px;vertical-align:top}
.celltoolbar label{display:inline-block;height:15px;line-height:15px;vertical-align:top}
.celltoolbar label span{font-size:85%}
.celltoolbar input[type=checkbox]{margin:0;margin-left:4px;margin-right:4px}
.celltoolbar .ui-button{border:none;vertical-align:top;height:20px;min-width:30px}
.completions{position:absolute;z-index:10;overflow:hidden;border:1px solid #ababab;border-radius:4px;-webkit-box-shadow:0 6px 10px -1px #adadad;-moz-box-shadow:0 6px 10px -1px #adadad;box-shadow:0 6px 10px -1px #adadad}
.completions select{background:#fff;outline:none;border:none;padding:0;margin:0;overflow:auto;font-family:monospace;font-size:110%;color:#000;width:auto}
.completions select option.context{color:#0064cd}
#menubar .navbar-inner{min-height:28px;border-top:1px;border-radius:0 0 4px 4px}
#menubar .navbar{margin-bottom:8px}
.nav-wrapper{border-bottom:1px solid #d4d4d4}
#menubar li.dropdown{line-height:12px}
i.menu-icon{padding-top:4px}
ul#help_menu li a{overflow:hidden;padding-right:2.2em}ul#help_menu li a i{margin-right:-1.2em}
#notification_area{z-index:10}
.indicator_area{color:#777;padding:4px 3px;margin:0;width:11px;z-index:10;text-align:center}
#kernel_indicator{margin-right:-16px}
.edit_mode_icon:before{font-family:FontAwesome;font-weight:normal;font-style:normal;text-decoration:inherit;-webkit-font-smoothing:antialiased;*margin-right:.3em;content:"\f040"}
.command_mode_icon:before{font-family:FontAwesome;font-weight:normal;font-style:normal;text-decoration:inherit;-webkit-font-smoothing:antialiased;*margin-right:.3em;content:' '}
.kernel_idle_icon:before{font-family:FontAwesome;font-weight:normal;font-style:normal;text-decoration:inherit;-webkit-font-smoothing:antialiased;*margin-right:.3em;content:"\f10c"}
.kernel_busy_icon:before{font-family:FontAwesome;font-weight:normal;font-style:normal;text-decoration:inherit;-webkit-font-smoothing:antialiased;*margin-right:.3em;content:"\f111"}
.notification_widget{color:#777;padding:1px 12px;margin:2px 4px;z-index:10;border:1px solid #ccc;border-radius:4px;background:rgba(240,240,240,0.5)}.notification_widget.span{padding-right:2px}
div#pager_splitter{height:8px}
#pager-container{position:relative;padding:15px 0}
div#pager{font-size:14px;line-height:20px;overflow:auto;display:none}div#pager pre{line-height:1.21429em;color:#000;background-color:#f7f7f7;padding:.4em}
.quickhelp{display:-webkit-box;-webkit-box-orient:horizontal;-webkit-box-align:stretch;display:-moz-box;-moz-box-orient:horizontal;-moz-box-align:stretch;display:box;box-orient:horizontal;box-align:stretch;display:flex;flex-direction:row;align-items:stretch}
.shortcut_key{display:inline-block;width:20ex;text-align:right;font-family:monospace}
.shortcut_descr{display:inline-block;-webkit-box-flex:1;-moz-box-flex:1;box-flex:1;flex:1}
span#save_widget{padding:0 5px;margin-top:12px}
span#checkpoint_status,span#autosave_status{font-size:small}
@media (max-width:767px){span#save_widget{font-size:small} span#checkpoint_status,span#autosave_status{font-size:x-small}}@media (max-width:767px){span#checkpoint_status,span#autosave_status{display:none}}@media (min-width:768px) and (max-width:979px){span#checkpoint_status{display:none} span#autosave_status{font-size:x-small}}.toolbar{padding:0 10px;margin-top:-5px}.toolbar select,.toolbar label{width:auto;height:26px;vertical-align:middle;margin-right:2px;margin-bottom:0;display:inline;font-size:92%;margin-left:.3em;margin-right:.3em;padding:0;padding-top:3px}
.toolbar .btn{padding:2px 8px}
.toolbar .btn-group{margin-top:0}
.toolbar-inner{border:none !important;-webkit-box-shadow:none !important;-moz-box-shadow:none !important;box-shadow:none !important}
#maintoolbar{margin-bottom:0}
@-moz-keyframes fadeOut{from{opacity:1} to{opacity:0}}@-webkit-keyframes fadeOut{from{opacity:1} to{opacity:0}}@-moz-keyframes fadeIn{from{opacity:0} to{opacity:1}}@-webkit-keyframes fadeIn{from{opacity:0} to{opacity:1}}.bigtooltip{overflow:auto;height:200px;-webkit-transition-property:height;-webkit-transition-duration:500ms;-moz-transition-property:height;-moz-transition-duration:500ms;transition-property:height;transition-duration:500ms}
.smalltooltip{-webkit-transition-property:height;-webkit-transition-duration:500ms;-moz-transition-property:height;-moz-transition-duration:500ms;transition-property:height;transition-duration:500ms;text-overflow:ellipsis;overflow:hidden;height:80px}
.tooltipbuttons{position:absolute;padding-right:15px;top:0;right:0}
.tooltiptext{padding-right:30px}
.ipython_tooltip{max-width:700px;-webkit-animation:fadeOut 400ms;-moz-animation:fadeOut 400ms;animation:fadeOut 400ms;-webkit-animation:fadeIn 400ms;-moz-animation:fadeIn 400ms;animation:fadeIn 400ms;vertical-align:middle;background-color:#f7f7f7;overflow:visible;border:#ababab 1px solid;outline:none;padding:3px;margin:0;padding-left:7px;font-family:monospace;min-height:50px;-moz-box-shadow:0 6px 10px -1px #adadad;-webkit-box-shadow:0 6px 10px -1px #adadad;box-shadow:0 6px 10px -1px #adadad;border-radius:4px;position:absolute;z-index:2}.ipython_tooltip a{float:right}
.ipython_tooltip .tooltiptext pre{border:0;-webkit-border-radius:0;-moz-border-radius:0;border-radius:0;font-size:100%;background-color:#f7f7f7}
.pretooltiparrow{left:0;margin:0;top:-16px;width:40px;height:16px;overflow:hidden;position:absolute}
.pretooltiparrow:before{background-color:#f7f7f7;border:1px #ababab solid;z-index:11;content:"";position:absolute;left:15px;top:10px;width:25px;height:25px;-webkit-transform:rotate(45deg);-moz-transform:rotate(45deg);-ms-transform:rotate(45deg);-o-transform:rotate(45deg)}

    </style>
<style type="text/css">
    .highlight .hll { background-color: #ffffcc }
.highlight  { background: #f8f8f8; }
.highlight .c { color: #408080; font-style: italic } /* Comment */
.highlight .err { border: 1px solid #FF0000 } /* Error */
.highlight .k { color: #008000; font-weight: bold } /* Keyword */
.highlight .o { color: #666666 } /* Operator */
.highlight .cm { color: #408080; font-style: italic } /* Comment.Multiline */
.highlight .cp { color: #BC7A00 } /* Comment.Preproc */
.highlight .c1 { color: #408080; font-style: italic } /* Comment.Single */
.highlight .cs { color: #408080; font-style: italic } /* Comment.Special */
.highlight .gd { color: #A00000 } /* Generic.Deleted */
.highlight .ge { font-style: italic } /* Generic.Emph */
.highlight .gr { color: #FF0000 } /* Generic.Error */
.highlight .gh { color: #000080; font-weight: bold } /* Generic.Heading */
.highlight .gi { color: #00A000 } /* Generic.Inserted */
.highlight .go { color: #888888 } /* Generic.Output */
.highlight .gp { color: #000080; font-weight: bold } /* Generic.Prompt */
.highlight .gs { font-weight: bold } /* Generic.Strong */
.highlight .gu { color: #800080; font-weight: bold } /* Generic.Subheading */
.highlight .gt { color: #0044DD } /* Generic.Traceback */
.highlight .kc { color: #008000; font-weight: bold } /* Keyword.Constant */
.highlight .kd { color: #008000; font-weight: bold } /* Keyword.Declaration */
.highlight .kn { color: #008000; font-weight: bold } /* Keyword.Namespace */
.highlight .kp { color: #008000 } /* Keyword.Pseudo */
.highlight .kr { color: #008000; font-weight: bold } /* Keyword.Reserved */
.highlight .kt { color: #B00040 } /* Keyword.Type */
.highlight .m { color: #666666 } /* Literal.Number */
.highlight .s { color: #BA2121 } /* Literal.String */
.highlight .na { color: #7D9029 } /* Name.Attribute */
.highlight .nb { color: #008000 } /* Name.Builtin */
.highlight .nc { color: #0000FF; font-weight: bold } /* Name.Class */
.highlight .no { color: #880000 } /* Name.Constant */
.highlight .nd { color: #AA22FF } /* Name.Decorator */
.highlight .ni { color: #999999; font-weight: bold } /* Name.Entity */
.highlight .ne { color: #D2413A; font-weight: bold } /* Name.Exception */
.highlight .nf { color: #0000FF } /* Name.Function */
.highlight .nl { color: #A0A000 } /* Name.Label */
.highlight .nn { color: #0000FF; font-weight: bold } /* Name.Namespace */
.highlight .nt { color: #008000; font-weight: bold } /* Name.Tag */
.highlight .nv { color: #19177C } /* Name.Variable */
.highlight .ow { color: #AA22FF; font-weight: bold } /* Operator.Word */
.highlight .w { color: #bbbbbb } /* Text.Whitespace */
.highlight .mb { color: #666666 } /* Literal.Number.Bin */
.highlight .mf { color: #666666 } /* Literal.Number.Float */
.highlight .mh { color: #666666 } /* Literal.Number.Hex */
.highlight .mi { color: #666666 } /* Literal.Number.Integer */
.highlight .mo { color: #666666 } /* Literal.Number.Oct */
.highlight .sb { color: #BA2121 } /* Literal.String.Backtick */
.highlight .sc { color: #BA2121 } /* Literal.String.Char */
.highlight .sd { color: #BA2121; font-style: italic } /* Literal.String.Doc */
.highlight .s2 { color: #BA2121 } /* Literal.String.Double */
.highlight .se { color: #BB6622; font-weight: bold } /* Literal.String.Escape */
.highlight .sh { color: #BA2121 } /* Literal.String.Heredoc */
.highlight .si { color: #BB6688; font-weight: bold } /* Literal.String.Interpol */
.highlight .sx { color: #008000 } /* Literal.String.Other */
.highlight .sr { color: #BB6688 } /* Literal.String.Regex */
.highlight .s1 { color: #BA2121 } /* Literal.String.Single */
.highlight .ss { color: #19177C } /* Literal.String.Symbol */
.highlight .bp { color: #008000 } /* Name.Builtin.Pseudo */
.highlight .vc { color: #19177C } /* Name.Variable.Class */
.highlight .vg { color: #19177C } /* Name.Variable.Global */
.highlight .vi { color: #19177C } /* Name.Variable.Instance */
.highlight .il { color: #666666 } /* Literal.Number.Integer.Long */
    </style>


<style type="text/css">
/* Overrides of notebook CSS for static HTML export */
body {
  overflow: visible;
  padding: 8px;
}

div#notebook {
  overflow: visible;
  border-top: none;
}

@media print {
  div.cell {
    display: block;
    page-break-inside: avoid;
  } 
  div.output_wrapper { 
    display: block;
    page-break-inside: avoid; 
  }
  div.output { 
    display: block;
    page-break-inside: avoid; 
  }
}
</style>

<!-- Custom stylesheet, it must be in the same directory as the html file -->
<link rel="stylesheet" href="custom.css">

<!-- Loading mathjax macro -->
<!-- Load mathjax -->
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_HTML"></script>
    <!-- MathJax configuration -->
    <script type="text/x-mathjax-config">
    MathJax.Hub.Config({
        tex2jax: {
            inlineMath: [ ['$','$'], ["\\(","\\)"] ],
            displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
            processEscapes: true,
            processEnvironments: true
        },
        // Center justify equations in code and markdown cells. Elsewhere
        // we use CSS to left justify single line equations in code cells.
        displayAlign: 'center',
        "HTML-CSS": {
            styles: {'.MathJax_Display': {"margin": 0}},
            linebreaks: { automatic: true }
        }
    });
    </script>
    <!-- End of mathjax configuration -->

</head>
<body>
  <div tabindex="-1" id="notebook" class="border-box-sizing">
    <div class="container" id="notebook-container">

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="What-is-the-most-important-frequency-in-a-signal?">What is the most important frequency in a signal?<a class="anchor-link" href="#What-is-the-most-important-frequency-in-a-signal?">&#182;</a></h1>
</div>
</div>
</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>The ultimate goal of this post is to design a function which takes discretely sampled points, and returns the principal frequency which makes up those points.</p>
<p>Ada is your boss.  Ada hands you a circuit, which contains some capacitors and inductors, and asks you to find out which frequencies the circuit is oscillating at.  Unluckily, you broke your oscilloscope last week, so you are going to have to measure the voltages of the circuit discretely, at many times, and try to infer some pattern from the datapoints.  Fortunately, you know some programming, so you can set up a machine to measure a digital multimeter at a time interval that you specify, and save the data.</p>
<p>What does that mean?  Let&#39;s look at one of the plots that you get from your nifty machine.  (It&#39;s fake, designed to imitate a circuit with two resonance frequencies, combined with some intentional measurement errors.)</p>
</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="Making-some-fake-data">Making some fake data<a class="anchor-link" href="#Making-some-fake-data">&#182;</a></h3>
</div>
</div>
</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[1]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="c">#Some preamble imports.</span>
<span class="o">%</span><span class="k">matplotlib</span> <span class="n">inline</span>
<span class="kn">import</span> <span class="nn">matplotlib.pyplot</span> <span class="kn">as</span> <span class="nn">plt</span>
<span class="kn">import</span> <span class="nn">numpy</span> <span class="kn">as</span> <span class="nn">np</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[21]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mf">10e-3</span><span class="p">,</span><span class="mi">50</span><span class="p">)</span>
<span class="c"># Generate simulated data with simulated noise</span>
<span class="n">Y</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">2e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span> <span class="o">+</span> <span class="mf">0.1</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">3e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span> <span class="o">+</span> <span class="mf">0.05</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">randn</span><span class="p">(</span><span class="mi">50</span><span class="p">)</span>

<span class="c"># plot discrete datapoints</span>
<span class="n">plt</span><span class="o">.</span><span class="n">title</span><span class="p">(</span><span class="s">&quot;Simulated Output of Voltage Measuring Instrument&quot;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">xlabel</span><span class="p">(</span><span class="s">&quot;Time (sec)&quot;</span><span class="p">),</span> <span class="n">plt</span><span class="o">.</span><span class="n">ylabel</span><span class="p">(</span><span class="s">&quot;Voltage&quot;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">scatter</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">Y</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">
    Out[21]:</div>


<div class="output_text output_subarea output_pyout">
<pre>
&lt;matplotlib.collections.PathCollection at 0xb0410c4c&gt;
</pre>
</div>

</div>

<div class="output_area"><div class="prompt"></div>


<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>The above code generates the distribution of dots, and I&#39;m going to go into detail about everything that it does.  Feel free to skip to the next section.</p>
<p>The high level goal of the distribution of dots is for it to primarily represent a single sinewave, but with some other things added in which are obscuring the clear picture.  I&#39;m going to chose a primary frequency, but then add in a secondary frequency, and some random noise.</p>
</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Let&#39;s look at the preamble first.</p>
<pre><code class="language-python">%matplotlib inline
<span class="keyword">import</span> numpy <span class="keyword">as</span> np
<span class="keyword">import</span> matplotlib.pyplot <span class="keyword">as</span> plt
</code></pre>
<p>First, I import <a href="http://www.numpy.org/">numpy</a>, a math package, and <a href="http://matplotlib.org/api/pyplot_api.html">pyplot</a>, a graphing library which can make inline graphs in ipython.  Numpy is aliased to <code>np</code>, and matplotlib is aliased to <code>plt</code>.  Typing <code>np.foo</code> will look in the numpy module for something named foo.  Numpy is useful because it uses handcoded and battle-tested C and Fortran code to do array computations fast.  It has its own datatype, <code>np.ndarray</code>, which is extremely fast and efficient.  It also provides functions like <code>np.sin</code> and <code>np.cos</code> which will operate on every one of the array elements.</p>
</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<pre><code class="language-python">X = np.linspace(<span class="number">0</span>,<span class="number">10e-3</span>,<span class="number">50</span>)
</code></pre>
<p>I want to create some data, which represents the values of a voltometer at different times.  First, I&#39;m going to need an array to represent the times that I want to sample the voltometer at.  I want to find the value of the voltometer 50 times, evenly spaced across an interval of 10 milliseconds.  Numpy&#39;s <a href="http://docs.scipy.org/doc/numpy/reference/generated/numpy.linspace.html">linspace</a> function does just this.  Here&#39;s an example:</p>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[22]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mi">10</span><span class="p">,</span><span class="mi">7</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">
    Out[22]:</div>


<div class="output_text output_subarea output_pyout">
<pre>
array([  0.        ,   1.66666667,   3.33333333,   5.        ,
         6.66666667,   8.33333333,  10.        ])
</pre>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p><code>np.linspace</code> make a one-dimensional array, of 7 evenly spaced points ranging from 0 to 10.  In general, <code>np.linspace(a,b,n)</code> produces an array of $n$ evenly spaced points from $a$ to $b$.</p>
</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>So far, our code generates no output:</p>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[23]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mf">10e-3</span><span class="p">,</span><span class="mi">50</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>We need to make some data in the Y axis.  Let&#39;s start by making a very simple sinusoid and then plotting it.</p>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[25]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mi">10</span><span class="p">,</span><span class="mi">50</span><span class="p">)</span>
<span class="n">Y</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="n">X</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">Y</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">
    Out[25]:</div>


<div class="output_text output_subarea output_pyout">
<pre>
[&lt;matplotlib.lines.Line2D at 0xb03a754c&gt;]
</pre>
</div>

</div>

<div class="output_area"><div class="prompt"></div>


<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
IiIiEkj/D9CWcp+uEtXVAAAAAElFTkSuQmCC
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Remember how I said that numpy has special version of math functions like <code>np.sin</code> and <code>np.cos</code>?  Numpy has those special functions to make applying a function to every element of an array easier.  Below are some other ways to take the sine of every element in X.</p>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[34]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="kn">import</span> <span class="nn">math</span>
<span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mi">10</span><span class="p">,</span><span class="mi">50</span><span class="p">)</span>
<span class="n">Y</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">zeros</span><span class="p">(</span><span class="mi">50</span><span class="p">)</span> <span class="c"># np.zeros(50) returns an array filled with 50 zeros</span>
<span class="k">for</span> <span class="n">i</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="nb">len</span><span class="p">(</span><span class="n">X</span><span class="p">)):</span>
  <span class="n">Y</span><span class="p">[</span><span class="n">i</span><span class="p">]</span> <span class="o">=</span> <span class="n">math</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="n">X</span><span class="p">[</span><span class="n">i</span><span class="p">])</span>
<span class="n">plt</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">Y</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">
    Out[34]:</div>


<div class="output_text output_subarea output_pyout">
<pre>
[&lt;matplotlib.lines.Line2D at 0xb02a69ac&gt;]
</pre>
</div>

</div>

<div class="output_area"><div class="prompt"></div>


<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
IiIiEkj/D9CWcp+uEtXVAAAAAElFTkSuQmCC
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[35]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="kn">import</span> <span class="nn">math</span>
<span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mi">10</span><span class="p">,</span><span class="mi">50</span><span class="p">)</span>
<span class="n">Y</span> <span class="o">=</span> <span class="p">[</span><span class="n">math</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="n">X</span><span class="p">[</span><span class="n">i</span><span class="p">])</span> <span class="k">for</span> <span class="n">i</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mi">50</span><span class="p">)]</span>
<span class="n">plt</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">Y</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">
    Out[35]:</div>


<div class="output_text output_subarea output_pyout">
<pre>
[&lt;matplotlib.lines.Line2D at 0xb0248fcc&gt;]
</pre>
</div>

</div>

<div class="output_area"><div class="prompt"></div>


<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
IiIiEkj/D9CWcp+uEtXVAAAAAElFTkSuQmCC
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[36]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="kn">import</span> <span class="nn">math</span>
<span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mi">10</span><span class="p">,</span><span class="mi">50</span><span class="p">)</span>
<span class="n">Y</span> <span class="o">=</span> <span class="p">[</span><span class="n">math</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="n">x</span><span class="p">)</span> <span class="k">for</span> <span class="n">x</span> <span class="ow">in</span> <span class="n">X</span><span class="p">]</span>
<span class="n">plt</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">Y</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">
    Out[36]:</div>


<div class="output_text output_subarea output_pyout">
<pre>
[&lt;matplotlib.lines.Line2D at 0xb01ed54c&gt;]
</pre>
</div>

</div>

<div class="output_area"><div class="prompt"></div>


<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
IiIiEkj/D9CWcp+uEtXVAAAAAElFTkSuQmCC
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[37]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mi">10</span><span class="p">,</span><span class="mi">50</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="n">X</span><span class="p">))</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">
    Out[37]:</div>


<div class="output_text output_subarea output_pyout">
<pre>
[&lt;matplotlib.lines.Line2D at 0xb01bba0c&gt;]
</pre>
</div>

</div>

<div class="output_area"><div class="prompt"></div>


<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
IiIiEkj/D9CWcp+uEtXVAAAAAElFTkSuQmCC
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Convinced those are the same?  I&#39;m now going to assume that you know that applying a numpy function to an array will apply it to every element of the array.</p>
<p>A not about plotting:  the pyplot function <code>plt.plot</code> plots a continuous line, while the pyplot function <code>plt.scatter</code> plots discrete points.  The plot with discrete points shows you where the function is evaluated.  The <code>plt.plot</code> function just connects the dots.</p>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[38]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mi">10</span><span class="p">,</span><span class="mi">50</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="n">X</span><span class="p">))</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">
    Out[38]:</div>


<div class="output_text output_subarea output_pyout">
<pre>
[&lt;matplotlib.lines.Line2D at 0xb01900ac&gt;]
</pre>
</div>

</div>

<div class="output_area"><div class="prompt"></div>


<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
IiIiEkj/D9CWcp+uEtXVAAAAAElFTkSuQmCC
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[39]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mi">10</span><span class="p">,</span><span class="mi">50</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">scatter</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="n">X</span><span class="p">))</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">
    Out[39]:</div>


<div class="output_text output_subarea output_pyout">
<pre>
&lt;matplotlib.collections.PathCollection at 0xb00fbf6c&gt;
</pre>
</div>

</div>

<div class="output_area"><div class="prompt"></div>


<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
gg==
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Just to keep you awake, we&#39;ve been working on understanding this code, and the output it produces:</p>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[40]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mf">10e-3</span><span class="p">,</span><span class="mi">50</span><span class="p">)</span>
<span class="c"># Generate simulated data with simulated noise</span>
<span class="n">Y</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">2e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span> <span class="o">+</span> <span class="mf">0.1</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">3e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span> <span class="o">+</span> <span class="mf">0.05</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">randn</span><span class="p">(</span><span class="mi">50</span><span class="p">)</span>

<span class="c"># plot discrete datapoints</span>
<span class="n">plt</span><span class="o">.</span><span class="n">title</span><span class="p">(</span><span class="s">&quot;Simulated Output of Voltage Measuring Instrument&quot;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">xlabel</span><span class="p">(</span><span class="s">&quot;Time (sec)&quot;</span><span class="p">),</span> <span class="n">plt</span><span class="o">.</span><span class="n">ylabel</span><span class="p">(</span><span class="s">&quot;Voltage&quot;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">scatter</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">Y</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">
    Out[40]:</div>


<div class="output_text output_subarea output_pyout">
<pre>
&lt;matplotlib.collections.PathCollection at 0xb00dd0ac&gt;
</pre>
</div>

</div>

<div class="output_area"><div class="prompt"></div>


<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
AAAAAElFTkSuQmCC
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>I used a scatter plot instead of a line plot, to emphasize that the datapoints are discretely sampled.</p>
<p>In creating the Y axis, for each point on X, I add three values:</p>
<ul>
<li>Sinusoid 1, which has the frequency which Ada told me to measure.  This is going to be the largest.</li>
<li>Sinusoid 2, a small secondary resonace frequency in our circuit, which will be smaller than Sinusoid 1, but still sizeable.</li>
<li>Random noise, because we don&#39;t have perfect measuring instruments.  Our measurements are pretty good, though, so we can make this component the smallest.</li>
</ul>
<p>Two of them are sinusoids, and one of them is random noise.  I&#39;ll show the two sinusoids seperately, and together.  (Note that I can use both <code>plt.plot</code> and <code>plt.scatter</code> to make dots, and then connect them).</p>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[47]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mf">10e-3</span><span class="p">,</span><span class="mi">50</span><span class="p">)</span>
<span class="n">Y</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">2e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">title</span><span class="p">(</span><span class="s">&quot;Sinusoid 1&quot;</span><span class="p">),</span> <span class="n">plt</span><span class="o">.</span><span class="n">axis</span><span class="p">([</span><span class="mi">0</span><span class="p">,</span><span class="mf">0.012</span><span class="p">,</span><span class="o">-</span><span class="mf">1.2</span><span class="p">,</span><span class="mf">1.2</span><span class="p">])</span>
<span class="n">plt</span><span class="o">.</span><span class="n">scatter</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">Y</span><span class="p">),</span> <span class="n">plt</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">Y</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">
    Out[47]:</div>


<div class="output_text output_subarea output_pyout">
<pre>
(&lt;matplotlib.collections.PathCollection at 0xafe6510c&gt;,
 [&lt;matplotlib.lines.Line2D at 0xafe4582c&gt;])
</pre>
</div>

</div>

<div class="output_area"><div class="prompt"></div>


<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[48]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mf">10e-3</span><span class="p">,</span><span class="mi">50</span><span class="p">)</span>
<span class="n">Y</span> <span class="o">=</span> <span class="mf">0.1</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">3e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">title</span><span class="p">(</span><span class="s">&quot;Sinusoid 2&quot;</span><span class="p">),</span> <span class="n">plt</span><span class="o">.</span><span class="n">axis</span><span class="p">([</span><span class="mi">0</span><span class="p">,</span><span class="mf">0.012</span><span class="p">,</span><span class="o">-</span><span class="mf">1.2</span><span class="p">,</span><span class="mf">1.2</span><span class="p">])</span>
<span class="n">plt</span><span class="o">.</span><span class="n">scatter</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">Y</span><span class="p">),</span> <span class="n">plt</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">Y</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">
    Out[48]:</div>


<div class="output_text output_subarea output_pyout">
<pre>
(&lt;matplotlib.collections.PathCollection at 0xafdba38c&gt;,
 [&lt;matplotlib.lines.Line2D at 0xafe1baac&gt;])
</pre>
</div>

</div>

<div class="output_area"><div class="prompt"></div>


<div class="output_png output_subarea ">
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAYUAAAEKCAYAAAD9xUlFAAAABHNCSVQICAgIfAhkiAAAAAlwSFlz
AAALEgAACxIB0t1+/AAAIABJREFUeJzt3Xd8VGW+BvBnMsn0JJRA6CAuIEU6CCgSGyJS1AUUUBd0
QRHFddW1rKvYsKws9VpQRGBdhOuq4FICeImoIFKzESGUCIYmYIAkJCFlnvvHmRwGJI3MZJL4fD+f
fJg5c877vr9M5jxzKoCIiIiIiIiIiIiIiIiIiIiIiIiIiIiI+IwEEB/C/psAyABgKeL1iQDmV9ho
RC5CWKgHIFJGVwFYB+AkgF8AfA2gq++1DwHcGKJxAcBPACIBsIjXi5oOAD0ArIJR01EAiwDUC+jo
REpBoSBVSRSA/wCYBqAmgIYAngdwJpSDCpAaAN4G0NT3kwFgTkhHJCJSyXUFcKKY10cB+MrvuRfA
fQB2+Zab6ffaRJy7K6eZb/7CL0qjAOwFkA4gBcAI33QLgGcA7APwM4C5MMLqQm1cAuBLXxsrAcxA
6XcfdfYtJ1KhtKUgVUkygAIAHwDoB2NroSQ3wwiT9gCG4ezupeJ25bhhbI30g7HC7wlgm++10QD+
ACAOQHMAHpwbNv7+BWAjgNoAXvQtV1y//q4G8H0p5xUR+c26DMZulVQAeQAWA6jre20Ufr2l0Mvv
+UIAT/geT0TRWwpuGFsWtwFwntf/FwDu93veEkCubzn/Npr4xue//Ico3ZZCexjHFq4sxbwiAaUt
BalqdsL4tt4YQDsADQBMLWb+I36Ps2Cs8EtyGsDtMFb+h2Acx2jle60+gP1+8/4EIBxA7HltNIAR
LNl+0/ajZL8DsAzABADflGJ+kYBSKEhVlgxjn367i1j2NACX3/Pzz/RZCaCvb/pOAO/6ph+CsUVQ
qAmAfBjHF/wdhrF7y7+Ppih+91FTGGcgvQBjq0KkwikUpCppBeDPMM46AoytheEA1pdyeQvOXkOw
DcZ++8YAogE85TdfXQCDYWxV5MEIkALfawsAPAIjGDwAJgH4CMZuI3/7AWyCcXZUBIxTaQcUM7aG
AP4PxvGJWaWsR0TkN60BjOMCBwBk+v59C8bKGTAO5K71m78AxsHgQnNgfAsvNBPGLp5dAP7omz8M
xtZBAoxrIU7AWFlf5lvGAuBvMHYbHQUwD0aoAEZQFLYBGGcfrYVxeulKANN981/IczCCJcPvR2cf
iYiIiIiIiIiIiIiIiIiISOVW1C1+K1yHDh2YmJgY6mGIiFQ1iQA6BqqxSnOdQmJiIkhW25/nnnsu
5GNQbapP9VW/HwAdArkurjShICIioadQEBERk0KhgsTFxYV6CEFTnWsDVF9VV93rC7RKc6AZAH37
x0REpJQsFgsQwHW5thRERMSkUBAREZNCQURETAoFERExKRRERMSkUBAREZNCQURETAoFERExBSIU
3gfwM4CkYuaZDmA3jLv5dQpAnyIiEgSBCIU5APoV83p/AL8D0ALAWBj/0bqIiFRCgQiFrwCcKOb1
QQDm+h5vAFADQGwA+hURkQCriGMKDQGk+j0/AKBRBfQrIiJlFF5B/Zx/s6YL3vlu4sSJ5uO4uDjd
3VBE5DwJCQlISEgIWvuBurNeMwCfA7j8Aq+9DSABwEe+5zsB9IFxcNqf7pIqIlJGVfEuqUsA3O17
3APASfw6EEREpBIIxO6jBTC++cfAOHbwHIAI32vvAFgG4wykPQBOAxgdgD5FRCQI9J/siIhUYVVx
95GIiFQRCgURETEpFERExKRQEBERk0JBRERMCgURETEpFERExKRQEBERk0JBRERMCgURETEpFERE
xKRQEBERk0JBRERMCgURETEpFERExKRQEBERk0JBRERMCgURETEpFERExKRQEBERk0JBRERMCgUR
ETEpFERExKRQEBERk0JBRERMCgURETEpFERExKRQEBERk0JBRERMCgURETEpFERExKRQEBERk0JB
RERMCgURETEpFERExKRQEBERk0JBRERMgQiFfgB2AtgN4IkLvB4H4BSArb6fZwLQp4iIBEF4OZe3
ApgJ4HoABwFsBLAEwI7z5vsSwKBy9iUiIkFW3i2F7gD2ANgHIA/ARwAGX2A+Szn7ERGRClDeUGgI
INXv+QHfNH8E0AtAIoBlANqUs08REQmS8u4+Yinm2QKgMYAsADcB+AxAy3L2KyIiQVDeUDgIY4Vf
qDGMrQV/GX6PlwN4E0AtAGnnNzZx4kTzcVxcHOLi4so5PBGR6iUhIQEJCQlBa7+8+/rDASQDuA7A
IQDfARiOcw80xwI4CmOrojuARQCaXaAtkqXZ8BARkUIWiwUI4HHb8m4p5AN4EEA8jDORZsMIhPt8
r78DYAiAcb55swDcUc4+RUQkSCrTWUHaUhARKaNAbynoimYRETEpFERExKRQEBERk0JBRERMCgUR
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
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Specifying <code>plt.axis</code> lets me control how big the plot appears.  Here, <code>plt.axis([0, 0.012, -1.2, 1.2])</code> sets the x axis to begin at <code>0</code> and end at <code>0.012</code>, and the y axis to begin at <code>-1.2</code> and end at <code>1.2</code>.  Usually, pyplot will automatically scale the data, but I&#39;m manually setting the axis here to highlight the difference in sizes between the two sinewaves.</p>
<p>It should come as no surprise that when added, the larger one dominates:</p>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[62]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mf">10e-3</span><span class="p">,</span><span class="mi">50</span><span class="p">)</span>
<span class="n">Y1</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">2e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span>
<span class="n">Y2</span> <span class="o">=</span> <span class="mf">0.1</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">3e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span>
<span class="n">Y12</span> <span class="o">=</span> <span class="mf">0.1</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">3e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span> <span class="o">+</span> <span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">2e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">title</span><span class="p">(</span><span class="s">&quot;Sinusoid 1 + Sinusoid 2&quot;</span><span class="p">),</span> <span class="n">plt</span><span class="o">.</span><span class="n">axis</span><span class="p">([</span><span class="mi">0</span><span class="p">,</span><span class="mf">0.012</span><span class="p">,</span><span class="o">-</span><span class="mf">1.2</span><span class="p">,</span><span class="mf">1.2</span><span class="p">])</span>
<span class="n">plt</span><span class="o">.</span><span class="n">scatter</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">Y12</span><span class="p">),</span> <span class="n">plt</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">Y12</span><span class="p">,</span><span class="n">label</span><span class="o">=</span><span class="s">&quot;1+2&quot;</span><span class="p">)</span>  <span class="c"># You can specify labels!</span>
<span class="n">plt</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">Y1</span><span class="p">,</span><span class="n">label</span><span class="o">=</span><span class="s">&quot;1&quot;</span><span class="p">),</span> <span class="n">plt</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">Y2</span><span class="p">,</span> <span class="n">label</span><span class="o">=</span><span class="s">&quot;2&quot;</span><span class="p">)</span> <span class="c"># for each graph!</span>
<span class="n">plt</span><span class="o">.</span><span class="n">legend</span><span class="p">()</span> <span class="c">#calling plt.legend() makes a legend!</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">
    Out[62]:</div>


<div class="output_text output_subarea output_pyout">
<pre>
&lt;matplotlib.legend.Legend at 0xaf8fe70c&gt;
</pre>
</div>

</div>

<div class="output_area"><div class="prompt"></div>


<div class="output_png output_subarea ">
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAYUAAAEKCAYAAAD9xUlFAAAABHNCSVQICAgIfAhkiAAAAAlwSFlz
AAALEgAACxIB0t1+/AAAIABJREFUeJzsnXd4VFX6xz8z6Y30QkJCQshMegIoohRhbYAiawMVd2XX
srZVbGtbFHtbGyryc1VEQbGuoqKCaOhYgCSQZCaVJEAqSUhvM+f3x00gQAKTZCZ3ZnI/z3OfJzNz
7znvlNzvOe/7nveAgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgsKQ
sQD4Ucb+I4AGQNXH60uAD4fMmv6jfH4KVo9abgMUrI4pwHagDjgMbAXO6HptNXCRTHYBlABegOjj
9b6eB3ACPgeKACNwrnlNO4q9fn6TgA1I76kS+BQIMat1ClaBIgoKPRkBfAu8BvgCYcDjQJucRpmR
zcB1QDmnvgGeyEJghQnn2fPn5wMsB0Z3HQ2Y9pkoKCjYMGcAtad4fSGwpcdjI/APILfrujd6vLaE
410RkV3ndw9EFgIFQD1QCFzb9bwK+DewH6gAViLdbHtrIwrY1NXGeuB1THN/lALTTDivm+sx7QY4
XD4/gPFd1ynYGcpMQaEnesAAvA/MRBrtno6LkW6GycA8jrlHTjUS90AaTc9EumGdDaR3vfY3pJvw
dGAM4MnxN8uefAT8DvgDT3Zd158ZgLkZTp/fNGCfiecq2BCKKCj0pAHJJy6A/yL5jr8Ggk5xzXNI
I8ZS4Bcgtev5voKZ3RiBJMANaUSb3fX8AuAlpJFuE/AQcDUn/1YjkG6mi4EOpBH4Nyb0OxBMbXO4
fH7JXdfdb8K5CjaGIgoKJ6JDGm2GA4lAKPDqKc4v7/F3M9Io9nQ0AfOBW4BDSH54bddrI4HiHueW
AI5A8AlthCK5XFp6PFeM+VjW1X4t8CaSe6b7cfoprrP3z28ssA64E9hmwvkKNoYiCgqnQo/kk04c
wLVNgHuPxydmqqwHLux6Xoc0sgbpJhfZ47wIoBNpNNyTMiT3TM8+RmM+99FtXe37dv29usfj1FNc
1xN7+/xGI2UgPYH0eSjYIYooKPREC9yDlDUD0mj3GmCHiderOOZ+SEfyO4cD3khujG6CgLlIo+IO
pBugoeu1j4G7kW5snsAzwBokd0lPioE/kLJ7nJDcNpecxj4XwLWXv/vzvk6FPX9+YcDPSPGJt018
Pwo2iCIKCj1pAM4CfgUakW5mmcC9Xa8Ljh9Jnjiq7Pn6BuCTrut/R/JXd7+mRrpxHUTKe58K3Nr1
2ntIGTCbkbJqmoF/9tHntV321gCPIo3KT4W+q71QpEVkTUgj6dNx4vvuC3v+/G5EylZa0vU+G1Cy
jxQUFBQUFBQUFBQUFBQUFBQUFBQUFBQUFBQUFBQUFBQssfpzQKSkpIiMjAy5zVBQUFCwNTIwfe3M
abGalNSMjAyEEHZ7PPbYY7LboLw35f0p78/+DiDFnPdiqxEFBQUFBQX5UURBQUFBQeEoiigMEdOn
T5fbBIthz+8NlPdn69j7+zM3VhNoBkSXf0xBQUFBwURUKhWY8V7uaK6GFBQUFCyBn58ftbWn2tBu
eODr60tNTY3F+1FmCgoKClaNSqVCuTf0/TmYe6agxBQUFBQUFI5i16JQVFTEhg0b2L9/v9ymKCgo
KNgEdisKy5a9TULCRK666lni48/gnXdWyG2SgkKvfPvttwQGjsbJyZXJky+iouLETdIUFIYOu4wp
lJWVMWZMAq2tvwPRQC6urpMoLtYRFHSqPdQVFIYWnU7H+PFTaWn5ApiAo+MSxo3bxW+//Ux5eTn/
+9//EEJw2WWXMXLkSLnNlQUlpiChxBRMpKGhgblzr8HV1Qs/vzBWrvyQwsJi1OrrkQQBQIOzcwSl
paVymqqgcBJbtmxBpboEaedNDzo7n+OPPxrJztYRFzeee+/dwX337SQ+fgIFBQVym6vQgzfeeIMz
zjgDV1dX/va3v/X7+p07d3LBBRfg7+9PUFAQ8+bNo7y83AKW9g+bF4W//e12fvzRgba2Empr13HT
TWVcdlkyra0LkHYjBCjEYDjAmDFj5DRVQeEkAgICUKtzOLaFch2winHjwqir+4mWlg9oafmA+vrb
eeihJ2W0VOFEwsLCWLx4MX//+99PeV5kZCQlJSUnPV9XV8ctt9xCcXExxcXFeHl5DUhczI05ROE9
oALYe4pzlgJ5SNX8xpmhz6Ns2LCBtrbnAF8ghY6O6Vx22Yf8739luLufiavrX1GrG/jssw/x9fU1
Z9cKCoNmzpw5JCd7o1KV4Oj4Em5uiaxY8SspKf8EWo+eZzQmUFZWLZ+hCidx2WWXMXfuXPz9/U95
Xpd75yRmzpzJFVdcgaenJ25ubtx+++1s27bNEqb2C3OIwgpg5ilenw2MBWKAm4G3zNDnUXx8/ICc
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
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>The above curve is not a perfect sinewave.  Let&#39;s make it even less perfect by adding in some noise, randomly.</p>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[55]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mf">10e-3</span><span class="p">,</span><span class="mi">50</span><span class="p">)</span>
<span class="n">Y</span> <span class="o">=</span> <span class="mf">0.1</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">3e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span> <span class="o">+</span> <span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">2e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span> <span class="o">+</span> <span class="mf">0.05</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">randn</span><span class="p">(</span><span class="mi">50</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">title</span><span class="p">(</span><span class="s">&quot;Sinusoid 1 + Sinusoid 2&quot;</span><span class="p">),</span> <span class="n">plt</span><span class="o">.</span><span class="n">axis</span><span class="p">([</span><span class="mi">0</span><span class="p">,</span><span class="mf">0.012</span><span class="p">,</span><span class="o">-</span><span class="mf">1.2</span><span class="p">,</span><span class="mf">1.2</span><span class="p">])</span>
<span class="n">plt</span><span class="o">.</span><span class="n">scatter</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">Y</span><span class="p">),</span> <span class="n">plt</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">Y</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">
    Out[55]:</div>


<div class="output_text output_subarea output_pyout">
<pre>
(&lt;matplotlib.collections.PathCollection at 0xafbdf14c&gt;,
 [&lt;matplotlib.lines.Line2D at 0xafbbfaac&gt;])
</pre>
</div>

</div>

<div class="output_area"><div class="prompt"></div>


<div class="output_png output_subarea ">
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAYUAAAEKCAYAAAD9xUlFAAAABHNCSVQICAgIfAhkiAAAAAlwSFlz
AAALEgAACxIB0t1+/AAAIABJREFUeJztnXd4FNX6xz+7yabRQULvJHRQpIMYBKkCFy9XpVzE/hPF
hr1iuRYQr4r1oihiAQUEBQRRCUWkSosQIBA6REqAJJu+5/fH2YUAKZvszM7M7vk8Tx52Z2fOeWdD
znfO+77nPaBQKBQKhUKhUCgUCoVCoVAoFAqFQqFQKBQKhUKhUCgUCoVCoVAoFH5jFLDUwP7rA2mA
rYjPJwIz/WZN6VHfn8L02I02QGE6egBrgDPAKWA10MH92VdAP4PsAjgIVABEEZ8XdRzAAcwBkgEX
cK22pp0nUL+/LsAy5D39DXwL1NTUOoUpUKKgKEhFYCHwDlAFqAO8CGQbaZSGrARGA8cpfgC8lLHA
Z16cF8jfX2XgI6CB+ycN774ThUJhYToAqcV8PhZYVeC9C7gH2O2+7r0Cn03kYldEQ/f5ngeRscBe
4BywDxjpPm4DngX2AynADORgW1gbjYAV7jZ+BqbinfvjENDTi/M83Ip3A2CwfH8A7d3XKQIMNVNQ
FGQXkA98DvRHPu2WxCDkYNgWuIkL7pHinsTLIZ+m+yMHrK7AFvdntyEH4TigMVCeiwfLgnwNbACq
AS+7ryvNDEBrgun76wkkeHmuwkIoUVAUJA3pExfANKTveAEQXcw1ryOfGA8By4Er3ceLCmZ6cAFt
gEjkE+0O9/FRwBTkk24G8BRwC5f/X62PHEyfA3KRT+A/etFvWfC2zWD5/tq6r3vMi3MVFkOJguJS
EpFPm/WA1kBt4O1izj9e4LUT+RRbEhnAzcD/AUeRfvhm7s9qAQcKnHsQCAVqXNJGbaTLJbPAsQNo
xwfu9lOB95HuGc/7LcVcF+jfX1NgMfAA8LsX5ysshhIFRXHsQvqkW5fh2gwgqsD7SzNVfgb6uo8n
Ip+sQQ5yDQucVx/IQz4NF+QY0j1TsI8GaOc+Guduv4r79VcF3l9ZzHUFCbTvrwEyA+kl5PehCECU
KCgK0gx4BJk1A/JpdwTwh5fX27jgftiC9DvXAyoh3RgeooGhyKfiXOQAmO/+7BvgYeTAVh54FZiF
dJcU5ACwEZnd40C6bW4owb5wIKKQ16W5r+II5O+vDvAbMj7xPy/vR2FBlCgoCpIGdAbWAenIwWwb
MMH9ueDiJ8lLnyoLfr4MmO2+fgPSX+35zI4cuI4g896vAe51fzYdmQGzEplV4wTGF9HnSLe9p4Hn
kU/lxbHL3V5t5CKyDOSTdElcet9FEcjf353IbKWJ7vtMQ2UfKRQKhUKhUCgUCoVCoVAoFAqFQqFQ
KBQKhUKh0GP1Z5lo166d2Lp1q9FmKBQKhdXYivdrZ0rENCmpW7duRQgRsD8vvPCC4Taoe1P3p+4v
8H6AdlqOxaYRBYVCoVAYjxIFhUKhUJxHiYKfiIuLM9oE3QjkewN1f1Yn0O9Pa0wTaAaE2z+mUCgU
Ci+x2Wyg4ViuZgoKhUKhOI8SBYXCBBw8eJCNGzeSlpZmtCmKIEeJgkJhME8++QLNmrWnd++7qF+/
GRs3bjTaJEUQo2IKCoWBrFq1iv79b8XpXA9cAXxH7dpPc+TIHqNNU1gEFVPwkuPHjzN06EiaNGnP
sGGj+fvvv402SaG4jB07dgDXIQUB4J8cO7aP3NxcA61SBDMBKQo5OTl0796XxYvrsW/fxyxcWJMe
PfqpPzSF6WjRogVyQzOn+8jf1KzZFIfDYaBVimAmIEUhISGBEyfyyct7HehIXt5kjh1zkpiYaLRp
CsVF9OzZk/Hj/43NlkRU1D2Ehu5l4MDlRpulCGICUhTCw8PJz0/nwra1+bhcGYSFhRlplkJRKA88
8CIVKrTml1/uIjGxHUuX1ubHH422ShGsaCEK04EUYHsx57wL7EFW87tKgz6LpUWLFnTp0g67PQG5
vW0GHToMIjY2Vu+uFYpSs3gx9O9vp2vXDjRpUp7vvoM77oCkJKMtUwQjWojCZ0D/Yj4fCDQFYoC7
gQ816LNY7HY7U6bMpVy5pgwfPo3+/bdRvvyHmCvZSqGQLFoEgwZdeN+lC0ycKOjWLYUKFRpRsWI0
Tz75PCo7T+EPtBCFVUBqMZ8PAWa4X68DKgM1NOi3WD7+2MGECeX57rv/sWDBNezfb2fuXL17VShK
R3Y2/PYbDBhw8fGcnA84fXoD6enbSUtby9Spi3n33Q+MMVIRVPgjplAHOFTg/WGgrp4dpqbCrFlw
zz3yfVgYfPwxPPggnD2rZ88KRelYuRJatIDq1S8+Pm/eEvLz84HyQGOczmeZO3eJESYqgoxQP/Vz
qd+m0HnwxIkTz7+Oi4src3XDzz6DgQOhZs0Lx3r0kFP0Z56B994rU7MKheZc6jryEB1dBbt9Jy7X
UABstkSio6v62TqFGYmPjyc+Pl639rVysjcEfgTaFPLZR0A8MMv9PhG4FhmcLogmK5rz8yE2Fr76
SvpmC3L6NDRrlkurVv/BZtvMyJFDufPO2zwrAhUKvxMTA7NnQ/v2Fx/fvXs3nTr1JDPzBnJznyMi
4mm2bHlBJUsoLkPrFc3+mCn8ANyPFIUuwBkuFwTNWLwYqlWDzp0v/+z06STS0t5kxYqXgFWsX/88
qalnePzxR/QyR6Eokt27ISMDriokHy82NpaEhI3MnTuXdet2cezYp8TGRvjfSEXQoUVM4RtgDdAM
GTu4HbjH/QOwGNgHJAEfA+M06LNIpk6F8eOhsIf/r7/+htzcKCAa+CdO51e89ZYK3nmLEII33phC
06ZX06pVN+bPn2+0SZZm0SLp5ixqolq3bl0efPBBpk/vy44dEeza5V/7FMGJFjOFEV6cc78G/ZTI
zp2wbRvcdFPhnwshLnEV2VSaXymYPPm/vPTSlzid7wOpjBx5J4sWVaJXr15Gm2ZJFi2C++4r+byI
CLjzTnj/fXj3Xf3tUgQ3AbGiecuWLUyaNIl77/2LsWNzCA8v/LyRI0cQGTkTOWHJITJyNA899H/+
NNXSfPrpLJzOqUA3YBCZmU/wxRffGm2WJUlLg3XroE8f786/91748ks4d05fuxQKy4vCwoUL6dat
L08/fYYVK+oze/bAIjcqiYmJ4ffff2Hw4BWEhmYxceITPPnko3622LpERkYABUelG0lLa44Qgs8+
+4w77riPSZPeJCsryygTLcOyZTIRokIF786vWxeuvx4+/1xXsxQKUy3xLVP2Uf36LTl06F1APnJF
RAxn0qRrGT9+fLHX9egBr7wCak9v71m0aBFDhx4hP78jMIeIiHNUrPhfKlTYzJEjL5GVdT0REb/R
ps1Z1qxZRmiovzKerccdd0CbNvDQQ95fs3o13H47JCaC3fKPcwqtUPspXMLZs6nIGLckO7sZp06d
LvG6Fi1kDELhPc2bD6JcudsYPXoODz6Yw/btD7JpUzrJyb+RlbUAeICsrHns3Hma33//3WhzTYvL
JbPkClufUBzdu0P58rB0qT52KRQQAKLQv39/HI63kBVR1xEZ+Rl9+15f4nVKFErP00/DY485mDnz
P7z99mSaNm1KSEgmYWGTC5xlx26vRmZmpmF2mpWMjAzuvfdhYmJuJy3tGOHhB0t1vc0mM+umTtXJ
QIXCZIiykJ6eLlq1mi1CQqaJK65oIL766muvrlu8WIg+fcrUZVCydq0QdeoIkZFx8XGXyyXat79G
hIWNE5Ak4IyoVq2JSE1NNcZQk+JyucR11w0WERE3C0gWNtsfokaNRuLs2bOlasfpdIkKFZyiT59H
xfjxE0RKSopOFiusAkVUiAgEyvyltG8vxG+/le6a5GQ5yClKxuUS4pprhPjkk8I/P3XqlBg2bLSo
W7eFqFJlnXjmmb/9a6AFOHXqlAgLqyAgR4AQIESFCteJhQsXlqqdRx99WoSGfiLgL+FwjBe1azcV
Z86c0clqhRVAY1GwvPto3z44dAiuuaZ019WvLwvnqRS/kvnhB1kiZOzYwj+vWrUq8+bN5NChHcTH
d2LatOrqe72E0NBQhMgHst1HBJBRqmC8y+XinXf+S17eEKAlubnvcvZsc35UO/IUSlZWFgkJCRw/
ftxoUyyF5UVh7lwYNgxKm+hit0OzZjKTQ3E5GRkZfPrpp0ye/DYPPZTFpEkQElLydW3bQr9+8Oab
+ttoJSpWrMhNN40kPHwCkEpY2J3UqpXDtdde63UbQghcrnygYLmLcuTl5WltruVJSEigXr1mdOs2
nIYNW/Dssy8ZbZKiDJRp6tSpkxA//1y2adfIkUJ8/nnZrg1k0tLSREzMlSIqapAICZkl7PYVYuHC
RV5fn5wsRNWqQhw/rp+NViQvL08MH75M1K27QkyY8GSZ3D633HKbiIwcIOCogF9FpUo1xbFjx3Sw
1to0btxGwHS3qy5FlCvXWCxfvtxos3QB5T66wMGDsHdv2dcaqAykwvn88885dKgRTueP5OffjMsV
xrhxj3l9fcOGMGYMvPyyfjZakZCQEKpX78OECT15883XqFSpUqnbmDHjI+6990pq1JhD/fpO1q5d
Ts2CNeIVuFwukpP/Aka7j0STn9+XhIQEI82yDJYWhXnzYOhQcDjKdr0ShcI5efIU2dktuLAepi5n
zpwqVRvPPCM3Otq7V3PzLM2ffxZeFdVbwsLCmDLlVaZNG0+rVjfQvHlz7YwLEOx2O7VrNwU8mxKd
JSQkXpUd9xJLi8KcOTB8eNmvV6JQOH369CYy8nPgGOAkPPxJrr++b6nauOIK6NdvJy1bLsDhiKRn
z4GcOHFCD3MtQ34+bN8OV17pe1uxsaiqqcUwb95MIiOXERq6nMjI5owZM4Drry95/ZLCwmUujhyR
ZQKOH5fbbZaFnByoWFFu0VlUEb1g5YsvvuSOO+oB7zFwYChffvk/KnhbqAcZ6OvU6QYyM3cBeTgc
T9Op0y5Wrw7eLSV37IAhQyApyfe2cnNl3ST1f7do/vnPHJo2PcDtt7to1qxZyRdYFCtusqML338P
gweXXRBAXtuwIezZA61ba2ZaQDBmzGgeeww2bbqWumXYUXvlypVAXyAcCCc3dxJ//FEel8uFPUgL
92ze7JvrqCAOh0yr3rdPzngVF5OfD/HxYbzzTkyZ/v8GM5b96/TVdeRBuZAK5/hxyMuDOnXKdn21
atWw23cCLveRHZQrVyVoBQFkPOHSbTd9ITZW7t6muJz16+X/XSUIpceSf6EpKbB1qywl7CtKFApn
yxbp+y7r9tU33ngjrVo5KFfuOiCF8PAJfPjhO5raaDW0nCmAEoXi+OknGDDAaCusieVE4dy5c8yc
mc6AAYIIDbasVaJQOFu3Qrt2Zb/e4XCwatUSPvroTjp0OMUjj0xn1ChvNukLTIRQouBPlCiUHcuI
gsvl4q67xnPFFbV4/PGN/PnnS5zToJaCEoXC8cwUfCEsLIzRo0dz++0tOX68oSZ2WZX9+yEqCmrU
0K5NJQqF8/ff8nvp1s1oS6yJZUThf//7hK+/3kBu7jGEuJZ9+44xbpzvu6Y1by7/A+Xna2BkAOHr
TKEgnTvLrSeDGa1nCaDSUovi55/huut8S0IJZiwjCqtWbcDpHAtUBGzk5t7JmjUbfG63fHmZU3/g
gM9NBQyZmZCcrF1WS5s28vs9c0ab9qyIHqJQpw6kp8u0VMUFlOvINywjCrGxDQgPj8dT5sNuj6dR
o/qatK1cSBeTkCCLBWr1pOVwyKybDb5ruGXROvMIZBJATIxMqVZI8vPlTEGJQtmxjCg8+ujDxMQc
Bs5RvvzNVK36Lh9/PEWTtpUoXIwW8YRLCXYXkh4zBVBxhUvZuFHGberVM9oS62IZUShXrhzvvbec
xo3tzJo1hj17ttG0aVNN2laicDFaxhM8dOkCa9dq26ZVOH4csrKgQQPt21aicDE//QT9+xtthbWx
jCgA/Pabg+HDKzBo0CAqV66sWbtKFC5Gj5mCRxRKUckkYPDMEsq65qM4lChcjIon+I6lRGHJEn2e
AjyiEIwD1qW4XLBtm/YzhTp1ICJClmUINvRyHYEShYKcPCk3zerRw2hLrI1lROHkSTlwd++ufdvV
q8tdxVJStG/baiQnQ+XKULWq9m0HqwvJH6KgHmhkgDkuThUI9BXLiMIvv8hfuF65x8qFJNmyRftZ
gocuXYIz2KxH5pGHKlXkDCyYtyEWQpCcnMx336XRv79SR1+xjCjo5TryoERBsnWr9vEED507B99M
4exZOQPVc3+XYHYhZWZm0qvXDbRs2Z3587P53/9GkZ6ebrRZlsYSoiAELF0qN4TXCyUKEj1nCldf
LddAZGXp074Z2bJFLt4LCdGvj2AWhRdffI316yPIyjoIVGPnTjtPPPGC0WZZGkuIwrZtcuVxkyb6
9aFEQaLnTCEqSn7Pmzfr074Z0dN15CGYRWH9+m1kZo5Ebg1jIzt7FBs2bDPaLEtjCVHQe5YA0KyZ
i4SEPI4HsXP29Gn507ixfn0EmwtJzyCzh2AWhdatYwgPX4isdCAIC1tIq1YxRptlaSwhCnrHE06e
PMngwd1JScmmQYPOjBp1Jy6Xq+QLA4xt26BtW9BzH5xgy0DavNk/M4VgLYz3yivPERubiN2+k6io
UTRqtJYpU14x2ixLY3pRSE+XNXPi4vTr4847H2T37s5AFDk5O5k/P5Fp0z7Rr0OTomc8wUOwlLvI
yclh9epN7NnjokULfR8wmjaVpbnz8nTtxpRUrFiRDRtWEhbWjB9+eJht2/6gqh751EGE6UVh+XLo
1EnGFPTizz+3kpt7G3Lv6yiczptZt26Lfh2aFD3jCR5iYuDcucBOoUxJSaFly4706zeZ3NxE+vUb
RHZ2tm79RURArVpSGIKRo0cdVKsWQu/eHQlT9bJ9xvSisGSJ/vGEmJgm2O0/ud+5iIz8mZYtdYxq
mxR/zBTs9sCfLdx776McPNgXp/MbXK7mbNwYxuTJb+naZzDHFXbuhJYtjbYicDC9KCxdqn+Bq08+
eZvo6GlERY3Hbv+T9u1zGD/+fn07NRk5ObJEQJs2+vcV6MHmv/7aRW7ujciZp53MzCFs3pyoa5/B
Lgpa7f2h0EYU+gOJwB7giUI+jwPOApvdP89623BSEjid+g9UjRo1Ys+ercyadTMREVexfPliwoNs
rXxioqziGRWlf1+BHmy+8spWhIV9g8yIySUyci5XX91K1z6DWRR27FCioCW+ikII8B5SGFoCI4DC
fj0rgKvcPyWmBqSkpDBq1F307v0uVapsJCsr00czS6Z8+fIMHtyDGjVC2LdPx5VGJkWPyqhFcfDg
XFauzODqq69n7ty5/unUj7z//mSaNPkDm+0Q4eHX0aNHGI8++pCufQazKCj3kbnoCiwp8P5J909B
4oAfvWhLCCFERkaGqF+/uXA4HhWQIhyOKaJ378HC5XIJf3DjjUJ8841fujIFJ06cEDfdNFZUqfKl
aN16tkhJSdG1v9mzvxVRUQ0EpAn4TURF1RE//vijrn0agdOZKxyOfJGQsNsv/3eTk4WoW1f3bkyH
yyVEpUpC/P230ZYYB57tKDXC15lCHeBQgfeH3ccKIoBuwFZgMXJGUSRr1qwhNbUKubmTgWhyc+9n
9epVnDhxwkdTvaN9e7kKNRjIy8vjmmv68/33FUlNHcTOnefo3r0vOTk5uvU5deoMnM7JQHmgF07n
f3j//Zm69WcUR46EUru2nVatYrDpsZHCJdSrJysJZ2To3pWpOH5cbvdavbrRlgQOvoqCNwr1J1AP
aAdMBeYXd7L8A8ov0LQLIYRf/rAguEQhMTGRw4fPkpv7NlCZ/Pw7OH48h4SEBN36DA93AAVHrgzC
wkJ1688o9u7VtyzLpYSEyP6SkvzXpxlQQWbt8fWv8QhywPdQDzlbKEhagdc/AR8AVYHTlzY2ceJE
8vLycDgOYLc/hMv1FJGRt9OnT3+q++lRwCMKQuizU5aZCAsLw+XKBHKBMCAPITJ1zfV+5pkHWLPm
ZjIzKwPtiYp6gSef/EG3/ozC36IAF+IKeqcVm4lgDDLHx8cTHx9vtBlFEgrsBRoiR5UtXB5oroHM
zQPoBOwvoq3zPrLTp0+LHj3miTp1Fovnn39ZZGdn+9VHV6uWEPv3+7VLQ3C5XKJPnyHC4XhVwH4R
GTlE9Ow5QOTn5+va76pVq8TgwY+KyMhUsW7dOl37MopHHhHijTf8119+fr4YNmyXGDjwd7F69Wr/
dWww48YJ8fbbRlthLGgcU9CCAcAuIAl4yn3sHvcPwH1AAlIw1gBdimjnoht96CEhJk825kseNEiI
efOM6dvfZGdnix491oqWLb8TL7/8qsjKyvJLvy6XEOXKCZGa6pfu/M7QoULMmeOfvlwulxg2bJQI
D58o7Pa1IiqqnnjrrXf907nB9OolxNKlRlthLGgsCmZykLjvTzJ0KNx6K9x4o/8Nef556T56+WX/
920E114Lzz4L11/v3347dICpU6FrV//26w/atIGZM/2T5rt69Wr697+DjIxtQDhwAIejJWfPniQy
MlJ/AwykZk1ZG61evZLPDVTc8VbNxnLTrmhOTta3hHNxXHVV8NT8z8uTMZQOHfzfd8uWgbmHhRD+
jSmcOHGCkJBYpCAA1CckJJJz5875xwCDSE2Vi1vr1jXaksDClKIgBOzbB40aGdN/MGUg7dwpi6lV
qeL/vgN1Y6Njx2QBxwoV/NNfp06dyM9fi1wy5MJm+5xatWoRHR3tHwMMYudOaN488BNC/I0pReHE
CQgPh0qVjOm/fn3IzpZ/3IHO+vWyCq0RBKoo+DvzqE6dOixc+B01a96PzfY1tWun8OuvP/gtjdso
duxQK5n1wJQJ4snJxs0SQD55eFxItWoZZ4c/2LDBWFHYscOYvvXEiHTUuLg4jh1L4oMPBBs32gz9
+/EXao2CPphyprBvn3HxBA/t2wdHXMHImUKTJnI2lql/aSu/YoQoeOjWzcYffxjTt78JxjUK/sCU
omBkkNlDMMQVMjNldVR/FcK7lNBQOXgG2laSRopC69Zw+LDcazvQUYXw9MGUomBkkNnDVVcFvihs
3iyftCIijLMhEOMKe/fKLTKNIDRUZpKtX29M//4iIwNSUowfJwIRU4qCGWYKMTGywFhqqrF26ImR
8QQPgSoKRs0UQK77CHQXUmKi/BsNCb4q97pjSlEww0zBbpdulUCOKxgZT/DQsmVgBZvPnIGsLDAy
GzQYREG5jvTDdKKQmwtHj8q0UKMJ9LiCGUQh0GYKnlmCkdmgXbrI363LZZwNeqMyj/TDdKJw6JBM
A9WxUKfXBHJc4fRpWYu+eXNj7YiNlTPDvDxj7dAKo11HIPcWqF49sMT2UtQaBf0wnSiYwXXkIZDT
UjduhKuvNt4nGxkJtWvLwTQQMIMogJwtBLILSc0U9MN0omCGILOHFi3gwAFITzfaEu0xg+vIQyC5
kMwiCoEcV8jJgf37ZaBZoT2mEwUzzRQcDmjVCrZuNdoS7TGTKARSsFmJgv7s2QMNGshSOArtMaUo
mGWmAIEZbBbCXKKgZgra06aNjM+dOWO0JdqjXEf6YjpRMJP76NSpU6xf/xlPPfU9vXsPJTk52WiT
NOHwYSkMZqlBHyiikJ0tF1SZIXMuNFTGjNatM9oS7VHlLfTFdKJgFveRy+Wid+8hbN9+koyM/sTH
d6Vr196kpaWVfLHJ8cwSzFJEs0ULuRjJ6imUyclSEEJNUmYyUF1Iao2CvphKFM6dk/V4zFAG/vDh
w+zevZf8/AlAJC7Xk2Rm1mZ9ANQPMJPrCGSJ9EqV5AzGyiQlmcN15CHQRCE+Pp6GDVsze/Z2pk6d
wMmTJ402KSAxlSh4Smab4Qk2IiKC/PwswOk+ko/LlRoQ2xuaTRQgMMpomyWe4KFLF+k+svoMDCA5
OZkbbvgXBw68jhCt2bzZzg033GK0WQGJ6UTBLPGE6Oho/vWvfxEV1Q9IJixsEq1a1aST2UbTUpKf
D5s2QceORltyMYEQVzCbKERHQ7Vq0jVndVauXInN1he4AbCRl/c6GzeuIisry2jTAg5TiYLZMo++
+OJjpkwZQ8uWSVx3XVfi4xcRahaHcSkRQvD551/Qo8eDCJFCSoq5RmAlCvoQKC6kKlWqAHsBz7Rn
PyEhDsLMUPogwDCdKJghyOzBbrfzf/93Dw88cD116sQRYWSNaR956613ue++11i7diTp6afo3Pla
9u3bZ7RZ51GioA+BIgoDBgygVatyhIbOw2ZbQlTUdbz55iTsdlMNYQGBqb5RM7mPChIbC7t3G22F
b7z55ns4nV8DnYGWOJ2j+PLLr4w26zyeBWxCGG1J2cjPl6tszfb/N1BEweFwsHLlTzRocA1jx55k
4cLPGT9+nNFmBSSmEgWzzRQ8BIIoCCGAkALvQ8nPN08EMjpaCsKJE0ZbUjaOHJH++6gooy25mOPH
l5GYmMmVVw7h008/M9ocn7DZwjh2rAZvvz2aXr16GW1OwGIqUdi/35yiULs2pKXB2bNGW1J27r//
LiIjbwXygP8RGfk5o0aNMNqs89hs1nYhmdF1tGrVKoYPH43LlcbWrc/ywAOvMW3ap0abVWa2b5fj
Q8WKRlsS2JhKFCpVgnLljLbicmw2WXxrzx6jLSk7zzzzOI888jjh4X8zYMAvrFr1M7GxsUabdRFW
FgWzrVEAmDbtSzIznwaigU44ne/z7rvWnS2sW2e+VOpAxFSpNGacJXiIjZWi0KGD0ZaUDZvNRuvW
I7jhBpgz51ujzSmUli2tKwpmnCk4HKFAZoEjmZbNngMpCl27Gm1F4GOqmYLZgnQFCYS4wubNcotR
sxIevo+ffz7MggULcFlsxZUZReGBB+6mXLkpwDfAcaKi7uX55x802qwys24ddO5stBWBj6lEwewz
BauLwpYt5hWFL7/8mkcfvZnExChGj36ZQYP+ZSlhMKMotGvXjtWrlzF06BYiIsKZN286w4YNM9qs
MnH2rKz62rq10ZYEPqYSBTPPFGJirC0KQph3piCE4O67x5GVNR2oSnr6H6xevY8lS5YYbVqJ7Ny5
k6FDR7GF1JfQAAAgAElEQVRtWwY//zzNneVlHq688krmzHkDl6sKvXr1M9qcMrNhg9we18LeL8ug
RMFLPDEFk/3Ne83x47IGTp06RltyOdnZ2WRnO4FW7iMOoA0pKSkGWlUyBw4coHPnOH74oTv5+WG8
+urHPPPMi0abdRmhoXLfcysXHFSuI/9hKlEws/uoWjW5n7FV8+g9riMzFBu8lIiICFq2vJqQkFeQ
ZQz24HItoavJo4pz5swhO3sYMA5wkJk5iw8++NhoswqlQQO5taxVUaLgP0wlCnXrGm1B8Vg5rmDm
eALA4sXf0qrVUuBtIiJm8/XX02jevLnRZhWLzWYDCk4dXYAJVRe5z4NVRcGzU6ASBf9gKg9dSEjJ
5xiJRxR69DDaktKzZQsMHmy0FUVTr149tm79nffec5GQYGfoUKMtKpmbbrqJF1/sQG7uaIS4gqio
ETz4oDlLL1h5pnDwoJzhmmWnwEDHVDMFs2PlYPOWLTJQZ3aaNLFjojp9xVK3bl02blxF/fohNG++
jDffHMfEic8YbVahWFkUPIvWzOj6DERMNVMQQrin5OYkNhZmzzbaitKTni6DjM2aGW1JyTRqJAsj
WoWYmBhq1Yph8uRupp5BNmhgzf+7oOIJ/sZUM4Xhw8eYLqWvIFaNKWzfLlcLWyGdr2FDmY+en2+0
Jd6TlARNmxptRfFYeaag4gn+xVSi8PPPCSxcuNBoM4qkaVM5AFhoTRVg/iBzQSIiZKbXkSNGW+Id
Z87IfcVr1DDakuKpX1+KrdX+7+bmyvU1Vi0vY0W0EIX+QCKwB3iiiHPedX++FSjSs52X15kDJn6c
KV9eDliHDhltSekw66K1omjc2DoupL175cOCib2eAERGyuqiJl/6cRkJCVLQKlUy2pLgwVdRCAHe
QwpDS2AE0OKScwYCTYEY4G7gwyKNsS+kg8kfCaxYLdVKMwWQcQWrBJut4DryYEUXknId+R9fRaET
kATsB3KBWcClyYRDgBnu1+uAykChk+3XXnuaLl26+GiSvlgtrpCXB3/9BW3bGm2J91gp2KxEQV9U
kNn/+CoKdYCCzpTD7mMlnVPoMrUHHjBnjndBrCYKu3fLTYIqVDDaEu9p3FjNFPRAiYLCG3zNR/E2
VehSj2uh102cOPH867i4OOLi4spklJ7ExsKvvxpthfdYzXUE1psp3Hqr0VZ4R4MG1nqgOXdOipiq
jHox8fHxxMfH69a+r6JwBCi4zrAeciZQ3Dl13ccuo6AomBWrzRSsKApWCjRbaaZQvz4sW2a0Fd5x
8OBBvvjiMDExbXE4yhttjqm49IH5xRe1LcLoq/toIzKA3BAIA24GfrjknB+AMe7XXYAzgMVyIC7Q
qJFcCJaTY7Ql3mFFUahdG06flqmeZiY9Xdb5r13baEu8wyruo/nzF9CixdW88spuEhK+5oEHHjfa
pKDCV1HIA+4HlgI7gNnATuAe9w/AYmAfMiD9MbKkpGUJC5OF+6zwJCuENUXBbpdPtfv3G21J8ezd
K2c1dlOt9ikaK4hCfn4+o0bdhtO5mOzsseTljWL69O9Yt26d0aYFDVqscf3J/VOQS+sH369BP6bB
40Iye9mIY8fkv1Z5ki2IJ9jc4tIEZxNhJdcRQJUqcvHamTNQubLR1hTOmTNnyMtzAR3dR8pht1/N
/v376awizn7BIs845sIqcQUz76FQElYINltNFGw2888WqlatSpUqVZBOB4BE8vJW0a5dOyPNCiqU
KJQBzy5sZseKriMPVkhLtZoogBSFgweNtqJobDYbS5fOp0qVadhsSYSHd+KDDyabfm+NQEKJQhmw
Sgltq5W3KIiaKeiDFTbbadeuHdOnLyUurg6pqSmMHTum5IsUmqFEoQyY3X2UlZXF/PnzWbUqjTp1
rLl/qBIFfTC7+8jD/v0htG4dSWRkpNGmBB1KFMpAvXpw6pRMSTQbaWlpXHVVD0aP/pCUFAdDhlzF
9u3bjTar1HjcR2atpJ6ZCSdPWm83MKuIQlISNGlitBXBiRKFMmC3XyijbTbefvtdkpNjychYAkSQ
lvYcd989wWizSk2VKvJ7Pn3aaEsKZ98+ufeD2beQvRSriMLevUoUjEKJQhmpWfMcCxbs4MyZM0ab
chH79x8lO7sLnsoiQnThyJGjxhpVRswcbLai6wiUKChKRolCKRFCcMcd9xMf/wWvvbaKhg1bsGnT
JqPNOk/v3t2JivoEyALyCA9/k2uv7W60WWXCzHEFq4pCrVqQmgpZWUZbUjR5eTJDqlEjoy0JTpQo
lJJFixYxe/YK8vLuIDv7Hs6e/S///Kd5KqKNGDGC++8fCpzEbm9H9+5n+eCDN402q0yYeV+FPXus
KQp2O9SpY+6Nog4dguhouQufwv8oUSgle/bsITe3F+DJihjC4cPmSUWy2Wzcf//LVK9eh7S0jfz6
6w9UsFLd7AKYuTCeVWcKYH4XknIdGYsShVLSpk0bHI6fgJMA2GwzadrUXDvY/P47dOtmIyrK2ul8
yn2kD1YQBat+t4GAEoVS0qdPH+6/fyRhYU2BNKpV+4jvv59ptFkXsWYNdLdmGOEizOo+ys6WdaUa
NDDakrJhdlFQ6ajGokShDLz++oscOJBIq1ahzJ27jhYmq9r2+++BIQoNG0r/cn6+0ZZcTHKyXBkc
qkU5SQMwuygo95GxKFEoIzVr1qRt20gOHAgz2pSLSE+HxERo395oS3wnIgKuuELuX2EmrOw6AvOX
ulCiYCxKFHzAjOUu1q+X9Y4CJXPDjMFmq4uCmYviCaFEwWiUKPiAGUVhzRro1s1oK7TDjHEFq4tC
vXpw5Ij53HIAKSkQGQmVKhltSfCiRMEHYmLMV0I7UOIJHsyYgWR1UYiIgKpVL2zCZCbULMF4lCj4
gKeEtlmKtrlcsHYtdO1qtCXaodxH+mDWYHMgfLdWR4mCD1SuDOXKmeeJa8cOGZitUcNoS7TDbO6j
3FyZEdWwodGW+IZZRUHNFIxHiYKPmMmFFGjxBDDfTOHAAbnndXi40Zb4hlkzkJQoGI8SBR8xU7A5
0OIJIAfg1FRwOo22RBIo7g2zZiCp1czGo0TBR8wkCoE4U7Db5VPt/v1GWyJJSpKzQ6tjVveRWs1s
PEoUfMQs7qOUFLkTWMuWRluiPWZxIe3du5dly5KpWDHFaFN8xoyicPasLCESHW20JcGNEgUfMctM
4Y8/oEsX+WQdaJgh2PzJJ5/Rpk1XFi/+m//+9zEmT37bWIN8JDV1M0lJOXz00cdkZGQYbQ4gXUeN
G4PNZrQlwU0ADiH+pUkT+RRr9EKgQIwnAOzfv58VK77kxRdncd99E3AaEFw4deoU48c/Qmbm7+Tl
dSYnZxLPP/8KB8z2qO0l33//PQMG9CcnJ59HHllF+/bXmEIYAiVeY3WUKPhIVBRUr2580E6WyzbW
Bq1JTU2lU6c4du2K5NSpPkyffpAbbxztdzuOHj2Kw1EL8AQTahIeHstBo3/pZWTcuMfJzPwOiCQz
cyaHDtXniy++MNoslXlkEpQoaIDRLqSsLNi6FTp1Ms4GPYiPjycrqzku1z+BK8jK+pJff13KuXPn
/GpHo0aNkPtnrHUfWUdu7m6aNWvmVzu0Ii0tFYh1v7ORkxNLamqqkSYBShTMghIFDTA62LxpEzRv
DuXLG2eDHjgcDsAJeJaMZwMuQkJC/GpH+fLlWbBgNhERHxMS8jPlyg1k1qzPibZoRLRv3/6Ehz8K
ZACHCAv7gj59+hhtlnIfmQQlChpg1ExBCMG8efN49dV46te3piujOHr37k2NGmcJC7sbOEl4+JP8
+9+3Ua5cOb/b0qtXL558chp33dWZkyePMHjwDX63QStmzPiQAQMgLGwcYWG7mDHjPTqZYJqpZgrm
wExxfiHMUkSolCxaBFOnwpIl/utTCMEtt9zGokXbcTo/x+H4hCeeqM5LLz3rPyP8QGpqKq+88gbz
58fRsmUFFizoit2gFKuRI6F/fxgzxpDuNScpCXr3NkdqalaWLBuTkQF+nghaHptM19JsLFeioAG7
d8OAAfJJx19s2rSJa68dTkbGDiASOEFYWGOOHz9IlSpV/GeIn/j2W/jqK1iwwDgbrrwSPvkEOnQw
zgYtcbmgYkW5iVHlysbasnMnDB1qjvRuq6G1KCj3kQY0aiTr02dn+6/P06dPExraCCkIANVxOKpw
5swZ/xnhR7p1kyu2jXpuyM+XA5bJdl71CbsdWreGbduMtkTFE8yEEgUNcDjkxiX+XGB11VVXIcQO
YB2Qi93+X6pWjaJevXr+M8KP1K0rN19JSjKm/+RkWX3WgHCGrrRrZw5RUPEE86BEQSNiY/2bgXTF
FVfwyy8/UqHCXuz252jdeg7Lly8i1Kq7yXuBZ7ZgBDt2BGYJkbZtlSgoLkaJgkYYkYHUsWNHmjUb
ycqVr7N16+80CfC/KiNF4a+/AlMU2rWTa1yMRrmPzIMSBY3w7MLmTzIz5RNs+/b+7dco1ExBe9q0
kYJndJkWNVMwD0oUNMLf7iOAP/+UA1VkZMnnBgLt2knfvhGx9EAVhUqVZJkWIwsO5uXJMjGNGhln
g+ICvohCVWAZsBv4GSgqqW0/sA3YDKz3oT9TY4T7aO1a6NzZv30aicMh00HXrfNvvy4XJCYGVuZR
Qdq2NdaFdOCAi+rVBRERxtmguIAvovAkUhRigV/d7wtDAHHAVYDxyyZ1om5duUNYerr/+ly7VpbL
Dia6d/e/C+nAAahWTeb0ByJGBZtzcnL497/vJjb2Bo4cWcUTTzyHVdcqBRK+iMIQYIb79QzgH8Wc
a6ZFcrpgt0ufqD9TJoNRFIyIKwSq68iDUWmpzz33CnPnHsDlmoMQHXjvvUVMm/ap/w1RXIQvolAD
8GxBleJ+XxgC+AXYCNzlQ3+mx5/B5iNHZKA52IJzXbpI95E/A6OBLgpGuY8WL/6NzMyngCggCqfz
IRYtWu5/QxQXUZIoLAO2F/Iz5JLzBBdKWV5Kd6TraABwH3BNWY01O/4MNq9bJwfIYNulqlo1qFMH
EhL812egpqN6aNIETpyQ22H6k9q1a2CzbTn/PjR0C3XqWLPybCBR0kqn64v5LAWoCRwHagF/F3He
Mfe/J4DvkXGFVYWdOHHixPOv4+LiiIuLK8E8cxEbCytW+KevYHQdefC4kNq1809/O3bA3Xf7py8j
CAmBVq1g+3bo0cN//b799it07HgXTucYIiPvpGLFrbzwgkE5xxYiPj6e+Ph43dr35TlzEnAKeAMZ
ZK7M5cHmKCAESAPKIbOUXnT/eymWLYjnYdUqePxxuV+y3vTsCc8/DyYog+93Pv0U4uNh5kz9+xJC
BpgPHoQArDN4nrvvlgX/xo3zb79PP32OtWsPc+utGxg6dCiVja7MZ0HMVBDvdeRMYjdwnfs9QG1g
kft1TeSsYAuySM9CCheEgMBf7qPcXLlGoWNH/fsyI926ye1H/cGhQ1ChQmALAhiXgfTrrxV56qmW
3HrrrUoQTIIvonAa6INMSe0LeJYUHQUGuV/vA650/7QGXvOhP9Nz+nQi6elZPPXUFPbv369bP9u3
Q8OGcuFRMNKsmVzAduxYyef6yo4d0rUS6BgRbD5yRD5EWcxLHPCoFc0asXHjRjp2vIbs7JNMmmSj
bdvO7NYpFSmY4wkg03+7dvWPmy7QM488tG0rg/cul//6nD8fBg2SixIV5kGJgkY89dSrZGS8AtTF
5XqEjIwHePnlN3XpK9hFAfy3XiFYRKFyZahaVZYR8Rfffw/DhvmvP4V3KFHQiNTUc0D98+9drvqc
Pn1Ol76CrbxFYfhLFAI9HbUg/qyYmpoK69dDv37+6U/hPUoUNGLUqKFERT0D7AJyiIx8hZEjh2re
z6lTcPx48AxURbOB9euzGTBgDD/99JMuPQgRPDMF8G+weeFC6NUr8DYtCgSUKGjEQw/dz+OPDyc6
eiAhIYe4995XGDVqhOb9rF8vs46CeXPz9evXc8MNA8nPP8uSJTcxfPidzJ8/X/N+jh6FiAi5YC4Y
8GewWbmOzIsSBY2w2Wy88MLTpKTs5bHHmhAR8S9d+lHxBHjnnWk4nU8D0cANOJ1TefXV9zXvJ5hm
CeC/GkhOJ/z6KwwerH9fitKjREEH/vEPmVmhB0oUID8/HyiYshLmPqYtwZKO6qFpU+maTEvTt59l
y+Dqq4NnBmY1lCjoQMeOMpCm9UI2l0vWPAr2IPO4cWOJjHwZmA84iYoazyOP3Kl5P8E2UyhY7kJP
lOvI3ChR0AG7HYYOhQULtGtz7969TJq0gMjIDKpVM3jvRIPp2bMnP/zwFT17Tic0NI/Jk99m1KiR
mvcTbKIA0KaNYO1ap277GuTlySDzP4ortK8wFCUKOjF0qHYupKVLl9K2bRcmTjzOyZMr6Nt3mC7u
EivRp08fVqz4gT59KlKnjvZZXkIEVzoqwPLly/n662d57LGvqF69PmvXrtW8j5Ur5Wr8evU0b1qh
EUoUdKJXL7lCNCWl5HNLYsyY/8PpnE129j3k5fVj3bqTzJkzx/eGA4DOnfXZnjMlRc74qlfXvm0z
curUKYYMuZmsrBtxue7i1Kn36d9/GBkZGZq0/913c6hevQHXX/8RZ89+zRkjNtpWeIUSBZ0ID4f+
/eHHH31v6/TpY4AnkBBCbm4Hjh496nvDAUCnTjJNVyuEEMyYMZOBA58HEklI0NnBbhJ27NhBSEgT
4Gr3kSG4XFXYt2+fz21v2rSJsWPv5+TJ2bhcd3PgwE5uueUOn9tV6IMSBR3RKgupffvu2O1vIfcx
2k1o6Fy6du3qe8MBQMeOsHGjdjV7pk79gHHjXmHz5qGcOpVN167X6VbDykzUrl2b7OwkLmyLkkJO
zlFq1ChqQ0XvWb58Obm5I4AugJ3c3IeJj1/mc7sKfVCioCMDBkgfqq8pft9/P5MGDU4CvxMW1p63
3nqRLsGel+qmenWZ2rhrlzbtTZ78Pk7nTOQTczsyM29jxgw/bNxgME2aNOGxxx4kKqoDYWHzCA2d
zX/+8xLR0b7vhFa1alUcjp1c2JxxJxUrqnxUs6JEQUcqVZI1epYu9a2d2rVrM3bsOzz8cCcyM89x
zz3ap19aGS1dSDLrxl7gfQgul7U3f/KWl156lvj4uTz1VHkaN76LCRMe0KTdkSNHUq9eBJCOw/EE
kZE38tFHUzRpW6E9ShR0RisX0po1cO21Ydjt6ld2KVoGmx966B4iI8cBecCHREV9wpgxo7Rp3AJ0
7NiR557ry6lTkRw5ok2bERER9O8/h759D/Daa9H8/vsSbrzxRm0aV2iOmbZ9t/x2nIVx9Ci0bi2z
WcpaN97luuAi0WA2H3CsWQMPPCBjC74ihODBB5cyY0ZtevT4Dy+//ATt27f3vWGLMWKE3Or1Dg3i
wWfPQuPGsHkz1K9f8vmK0mGm7TgVXlC7NjRsmM2kSevYsmVLmdrYuVOKghKEwrnqKvkdZWX53pbN
ZqN8+f489FBbFi2aHZSCADBwICxerE1b06bJEtlKEKyBEgWd+emnn0hImMRLL6XQvftg7rtvQqnb
WLNGxiYUhRMZCc2byydRLVi5Enr21KYtq9Kvnyxal5PjWzu5ufDOOzCh9P/tFQahREFHXC4XN988
htzcQeTkDMHpTGDGjPmsXr26VO388YcShZLQKticmQlbtqiig9HREBsLv//uWzvffisL7V19dcnn
KsyBEgUdSU9PJzPTCXhcEJWw2TqRXMo9D9eskXsSK4qmUydtgs3r1kGbNmrzF5AuJF/2LxICpkyB
Rx/VziaF/ihR0JEKFSoQHV0b8OS5p5Kfv5x27dp53capUxeC1Yqi6dxZm5mCch1dwNe4wvLlcuY1
YIB2Nin0R4mCjthsNn76aS7Vqz9HREQc4GTKlFdo27at122sXSufgoN5pzVvaNYMTpyQIuoLK1Yo
UfDQoQP8/TccOFC667Zv384333zDc8+dYcIEWUNKYR3Ur0tn2rZty9GjSSQmzqBVq1q0aVO6hWcq
yOwdISFyEPNltpCTI6/v3l07u6yM3S4DzqVxIU2d+iFduvTlzjs3sGZNDomJL+tnoEIXlCj4gdDQ
UBo0aMCIEXZmzSrdtSrI7D2+Bps3bZJB0cqVtbPJ6pQmrpCamspjjz2J07kGp/MtoBwffPA+u7Sq
QaLwC0oU/MjNN8OcOXKjEW/Iy4MNG9ROa97ia7BZxRMup29fiI+H7OySz01JScHhiAYauY+UIyys
GUe0Whqt8AtKFPxI06ZQt670W3vDtm1ywU+VKvraFSh4gs1lXRivROFyqlWTSQ4rV5Z8bsOGDQkJ
iQSc7iMrycvbQatg2ug6AFCi4GduuQWvXUjKdVQ6ateGiAgoZcYvAPn5MidficLlDBjgXRZSaGgE
MTGriYycTlhYRSpU+Cfz5n2lSflthf9QouBnbrpJblzuzUpRtT6h9JS1ON62bVJUgmWntdLQr18+
c+c6Wbx4MadPny7yvOefh0qVKnLu3H0cP36AM2dS6Nu3rx8tVWiBEgU/U7++TJ/85ZeSz1WZR6Wn
rMFm5ToqnJycHB5++AYOH87g5ptn0bRpG/7666/zn+fm5uJyuViwAL78Er75BkJDbVSpUkVV9LUo
6rdmALfcArNnF3/OsWOyumRsrH9sCgScTifLlk3hvfc2ERPTnmXLvN/dS4lC4UybNo0//xQIUY30
9C84c+Z5br31ftLT0+nX70YiIsoREdGOUaMy+O47NdMKBJQoGMDw4fDDD8VX9fzjD+k6Ug9b3nPr
rfeyevUW8vKuIinpFf7xj5EkJCSUeJ0QUhSuucYPRlqMpKT9ZGbG4RkqhBjLX3/dQd++s4iPb4DL
lUZu7iZyct7kyJF5htqq0AY15BhArVqy3POSJUWfo4LMpWfRogVkZ7+F/G89kLy8kSz1Ytu7xESo
UAHq1dPdRMvRpUsHypWbBZwGBCEhH9GwYR6bN9chJ2cSEA6EkZtbnl9+8SJFSWF6lCgYxM03F52F
dO7cOVauzFNB5lISGVkBOHT+fUjIUcqXL1/idcp1VDQ33XQTd9zRF4ejPhER0bRsOYsVKwbSosXr
wDfuswTh4ZuoV6+mkaYqAhARTOzadUqEhTnFwIG3i3feeU/k5+eL3Nxcccstt4nQ0MoC0kXv3iOE
0+k02lTLMH365yIqqo6AF4Xdvk5UqjRdnDt3rsjzXS6X2LFjh+jf/6T46KMcP1pqPc6cOSOOHj0q
XC6XEEKIDRs2iPLlq4ty5UaI8uWvE7GxVxX7XSv0A9B0y0q1HacBZGRk0Lp1Jw4c+BYhThIV9RSj
R3ekUaN6vPzyTzidC4FwIiJu4q67GvPuu5OMNtkyLF++nCVLlhEVVYePP76XL7+0c911l5+Xl5fH
jTeO5pdfVpOVtYkaNcawfv0n1FM+JK85dOgQv/76K1FRUQwePJjIyEijTQpKtN6O00wYLbh+Y968
eaJCheuEDHEKAZnCZvtUVK++TsDfBY7/Iq688lqjzbUsS5YIUa+eEKdPX/7Zhx9+KKKi4gRkCRDC
bn9JXHfdEP8bqVD4CBrPFFRMwQByc3OBqAJHQoCNtG+/ndDQL/D8jkNCVtCkidrYtqz06wf/+Afc
d9/ln23dmojTOQoZKAWXazg7duz0r4EKhQnxRRT+BfwF5HNha7HC6A8kAnuAJ3zoL2Do3bs3YWFb
sdtfB1YQEXETgwad4uuvh9GgwQwqVLiGihV7Ex39NW+//arR5lqaN96AjRtz6djxA1q27MqYMfdw
7NgZDh8eDQwHcgEIDf2WVq1aGmqrQmEGfPFDNQdcwMfABODPQs4JAXYBfYAjwAZgBFDYI5l7JhQc
7Nu3j/Hjn+LgwSP06tWNSZNeIiIigszMTOLj48nPz6dnz55UrFjRaFMtTWZmJk2ajObYsRnATkJD
12C3D2fQoFpkZDzIqlVzCQ2tTNWqNlavXkrdunWNNlmhKBVaxxS0aGg5RYtCV+AF5GwB4En3v68X
cm5QiYLCP6xevZpBgx7i3LmN7iOC8PDR7NjxMo0aNSIpKYmMjAxatGhBeHi4obYqFGVBa1EI1aqh
IqhDwcRxOAyo3QEUfsPhcOByOZGTWjuQjc32Mw7H69hsNmJiYgy2UKEwFyWJwjKgsBUpTwM/etF+
qR79J06ceP51XFwccXFxpblcobiMq6++mubNo0lIGEFW1gCiombRq1cv5SZSWJb4+Hji4+N1a19v
91EXYCIX3EdPIR/Z3ijkXOU+UuiC0+nktdcms337Hjp3bsejjz6Ew+Ew2iyFQhPMGlN4FNhUyGeh
yEBzb+AosB4VaFYoFArN0FoUfElJHYaMF3QBFgGe7b1ru98D5AH3A0uBHcBsChcEhUKhUJgAMy2N
VjMFhUKhKCVmmikoFAqFIsBQoqBQKBSK8yhRUCgUCsV5lCgoFAqF4jxKFBQKhUJxHiUKCoVCoTiP
EgWFQqFQnEeJgkKhUCjOo0RBoVAoFOdRoqBQKBSK8yhR8BN6lro1mkC+N1D3Z3UC/f60RomCnwjk
/5iBfG+g7s/qBPr9aY0SBYVCoVCcR4mCQqFQKM5jptLZW4B2RhuhUCgUFmMrcKXRRigUCoVCoVAo
FAqFQqFQKBQ+0B9IBPYATxRxzrvuz7cCV3lxbVVgGbAb+BmorK3JpUKP+5uM3L96KzAPqKStyaVC
j/vzMAFwIX+fRqDXvY1H/v4SgDc0tLe06HF/nYD1wGZgA9BRW5NLhS/3Nx1IAbZfcn6gjC1F3Z/h
Y0sIkAQ0BBzIAHKLS84ZCCx2v+4MrPXi2knA4+7XTwCva265d+h1f9dzIRvsdQLv/gDqAUuAZIwR
Bb3urRdyUHG431fX3HLv0Ov+4oF+7tcDgOVaG+4lvtwfwDXIQfTSQTMQxhYo+v5KNbbokZLaCXlj
+4FcYBYw9JJzhgAz3K/XIZW5ZgnXFrxmBvAPHWz3Br3ubxnyCdpzTV09jPcCve4P4C0u/PEZgV73
duhVlFQAAAJVSURBVC/wmvs4wAk9jPcCve7vGBeeLisDR/Qw3gt8uT+AVUBqIe0GwtgCRd9fqcYW
PUShDnCowPvD7mPenFO7mGtrIKdGuP+toZG9pUWv+yvI7Vx4GvA3et3fUPf7bVoaW0r0urcYoCfy
qS0e6KCZxaVDr/t7EpgCHES6Ip7SzuRS4cv9FUcgjC3eUuLYoocoCC/P82aNhK2I9kQp+tEaLe+v
MJ4BcoCvy3i9r+hxf5HA08ALZbxeK/T63YUCVYAuwGPAt6W8Xiv0ur9PgQeA+sDDSN+1EZT1/koz
VlhxbPH2Oq/GllAvGysNR5C+Yw/1kGpW3Dl13ec4CjnumaqmIKdJx4FawN/amVwqtLy/S68di/QZ
9tbI1rKgx/01QfpJtxY4fxNyuuzP36Nev7vDyAAeyECsC6gGnNLEau/R6/46AX3cr+cAn2hkb2kp
6/2V5O6y+tjijTtvLAaOLaHAXuQgEEbJwZIuXAiWFHftJC5E45/EuGCQXvfXH/gLuEIfs71Gr/sr
iFGBZr3u7R7gRffrWKSbxQj0ur8/gWvdr3sjhc8IfLk/Dw0pPNBs9bHFQ0Muvz9TjC0DgF3IoInH
/3iP+8fDe+7PtwLtS7gW5CDyC+ZIG9Pj/vYAB5Bpf5uBD/Qw3Ev0uL+C7MO4lFQ97s0BzET+MW4C
4nSw21v0uL8OyADlFuAPLk6D9De+3N83wFEgG+mXv819PFDGlqLuz0xji0KhUCgUCoVCoVAoFAqF
QqFQKBQKhUKhUCgUCoVCoVAoFAqFQqFQKBQKhUKhUCiCkf8HMmHlkeM4AuwAAAAASUVORK5CYII=
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>There we go!  That looks like it could be the output of a voltage measuring device, on a real circuit, in the real world!</p>
<p>The code again:</p>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[56]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mf">10e-3</span><span class="p">,</span><span class="mi">50</span><span class="p">)</span>
<span class="c"># Generate simulated data with simulated noise</span>
<span class="n">Y</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">2e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span> <span class="o">+</span> <span class="mf">0.1</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">3e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span> <span class="o">+</span> <span class="mf">0.05</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">randn</span><span class="p">(</span><span class="mi">50</span><span class="p">)</span>

<span class="c"># plot discrete datapoints</span>
<span class="n">plt</span><span class="o">.</span><span class="n">title</span><span class="p">(</span><span class="s">&quot;Simulated Output of Voltage Measuring Instrument&quot;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">xlabel</span><span class="p">(</span><span class="s">&quot;Time (sec)&quot;</span><span class="p">),</span> <span class="n">plt</span><span class="o">.</span><span class="n">ylabel</span><span class="p">(</span><span class="s">&quot;Voltage&quot;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">scatter</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">Y</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">
    Out[56]:</div>


<div class="output_text output_subarea output_pyout">
<pre>
&lt;matplotlib.collections.PathCollection at 0xafb35b8c&gt;
</pre>
</div>

</div>

<div class="output_area"><div class="prompt"></div>


<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="Okay,-you-have-the-data.--What-frequency?">Okay, you have the data.  What frequency?<a class="anchor-link" href="#Okay,-you-have-the-data.--What-frequency?">&#182;</a></h3>
</div>
</div>
</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>(I&#39;m going to assume that you know some things about fourier analysis.  If you want to brush up on it, some good resources <a href="http://jeremykun.wordpress.com/2012/04/25/the-fourier-series/">here</a>, <a href="http://jeremykun.wordpress.com/2012/05/27/the-fourier-transform-a-primer/">here</a>, <a href="http://jeremykun.wordpress.com/2012/06/23/the-discrete-fourier-transform/">and here</a>.</p>
</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>The naive way to find the frequency is to look at the data and estimate.  I think that the oscillation has a frequency of 0.003 ms, but I&#39;m lazy and I want a computer to do this for me.</p>
<p>So, I&#39;m going to take the fourier transform of this data.</p>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[77]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mf">10e-3</span><span class="p">,</span><span class="mi">50</span><span class="p">)</span>
<span class="c"># Generate simulated data with simulated noise</span>
<span class="n">Y</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">2e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span> <span class="o">+</span> <span class="mf">0.1</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">3e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span> <span class="o">+</span> <span class="mf">0.05</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">randn</span><span class="p">(</span><span class="mi">50</span><span class="p">)</span>
<span class="n">Yhat</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">fft</span><span class="o">.</span><span class="n">fft</span><span class="p">(</span><span class="n">Y</span><span class="p">)</span>

<span class="c"># plot discrete datapoints</span>
<span class="n">fig</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">figure</span><span class="p">();</span> <span class="n">plt</span><span class="o">.</span><span class="n">title</span><span class="p">(</span><span class="s">&quot;Frequency Distribution (below) vs. Graph (above)&quot;</span><span class="p">)</span>
<span class="n">top</span><span class="p">,</span> <span class="n">bottom</span> <span class="o">=</span> <span class="n">fig</span><span class="o">.</span><span class="n">add_subplot</span><span class="p">(</span><span class="mi">211</span><span class="p">),</span> <span class="n">fig</span><span class="o">.</span><span class="n">add_subplot</span><span class="p">(</span><span class="mi">212</span><span class="p">)</span>
<span class="n">top</span><span class="o">.</span><span class="n">scatter</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">Y</span><span class="p">)</span>
<span class="n">bottom</span><span class="o">.</span><span class="n">scatter</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">fft</span><span class="o">.</span><span class="n">fftfreq</span><span class="p">(</span><span class="mi">50</span><span class="p">,</span><span class="n">d</span><span class="o">=</span><span class="mf">10e-3</span><span class="o">/</span><span class="mi">50</span><span class="p">),</span><span class="n">np</span><span class="o">.</span><span class="n">abs</span><span class="p">(</span><span class="n">Yhat</span><span class="p">))</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">
    Out[77]:</div>


<div class="output_text output_subarea output_pyout">
<pre>
&lt;matplotlib.collections.PathCollection at 0xaf0d9fcc&gt;
</pre>
</div>

</div>

<div class="output_area"><div class="prompt"></div>


<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
AOcHtBpJrw0OAAAAAElFTkSuQmCC
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Let&#39;s get a better look at the frequency distribution by itself, and see what properties it has, as well as the locations of its maxima.</p>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[98]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mf">10e-3</span><span class="p">,</span><span class="mi">50</span><span class="p">)</span>
<span class="c"># Generate simulated data with simulated noise</span>
<span class="n">Y</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">2e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span> <span class="o">+</span> <span class="mf">0.1</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">3e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span> <span class="o">+</span> <span class="mf">0.05</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">randn</span><span class="p">(</span><span class="mi">50</span><span class="p">)</span>
<span class="n">Yhat</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">fft</span><span class="o">.</span><span class="n">fft</span><span class="p">(</span><span class="n">Y</span><span class="p">)</span>

<span class="c"># plot discrete datapoints</span>
<span class="n">freqkey</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">fft</span><span class="o">.</span><span class="n">fftfreq</span><span class="p">(</span><span class="mi">50</span><span class="p">,</span><span class="n">d</span><span class="o">=</span><span class="mf">10e-3</span><span class="o">/</span><span class="mi">50</span><span class="p">)</span>
<span class="n">order</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">argsort</span><span class="p">(</span><span class="nb">abs</span><span class="p">(</span><span class="n">Yhat</span><span class="p">))</span> <span class="c"># orders array indices by entry in Yhat</span>
<span class="k">print</span> <span class="n">order</span><span class="p">[</span><span class="o">-</span><span class="mi">2</span><span class="p">:],</span> <span class="n">Yhat</span><span class="p">[</span><span class="n">order</span><span class="p">[</span><span class="o">-</span><span class="mi">2</span><span class="p">:]]</span> <span class="c"># prints the indices for the two last</span>
<span class="k">print</span> <span class="n">freqkey</span><span class="p">[</span><span class="n">order</span><span class="p">[</span><span class="o">-</span><span class="mi">2</span><span class="p">:]],</span> <span class="n">Yhat</span><span class="p">[</span><span class="n">order</span><span class="p">[</span><span class="o">-</span><span class="mi">2</span><span class="p">:]]</span>
<span class="n">plt</span><span class="o">.</span><span class="n">scatter</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">fft</span><span class="o">.</span><span class="n">fftfreq</span><span class="p">(</span><span class="mi">50</span><span class="p">,</span><span class="n">d</span><span class="o">=</span><span class="mf">10e-3</span><span class="o">/</span><span class="mi">50</span><span class="p">),</span><span class="n">np</span><span class="o">.</span><span class="n">abs</span><span class="p">(</span><span class="n">Yhat</span><span class="p">))</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt"></div>
<div class="output_subarea output_stream output_stdout output_text">
<pre>
[ 3 47] [ 16.32394207-15.18707643j  16.32394207+15.18707643j]
[ 300. -300.] [ 16.32394207-15.18707643j  16.32394207+15.18707643j]

</pre>
</div>
</div>

<div class="output_area"><div class="prompt output_prompt">
    Out[98]:</div>


<div class="output_text output_subarea output_pyout">
<pre>
&lt;matplotlib.collections.PathCollection at 0xae9b18ac&gt;
</pre>
</div>

</div>

<div class="output_area"><div class="prompt"></div>


<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
iIiIiFy+/weEdBpK6FCp+QAAAABJRU5ErkJggg==
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>The interval is <code>10e3</code> seconds long, and contains <code>50</code> sample points.</p>
<p>The datapoint at $+300 Hz$ corresponds to a signal:</p>
<p>$$
e^{-2 \pi i \frac{50 \text{Samples}}{1e3 \text{sec}} (300 Hz) x}
$$</p>
<p>When taken together with the datapoint at -300 Hz, and taking into account their amplitudes, they sum to form a sinewave:</p>
<p>$$
\sin\left(- x * (300 Hz) * \frac{50}{1e-3} \right)
$$</p>
</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>I know that the frequency of the signal is the largest component.  We measure the frequency here:</p>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[99]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mf">10e-3</span><span class="p">,</span><span class="mi">50</span><span class="p">)</span>
<span class="c"># Generate simulated data with simulated noise</span>
<span class="n">Y</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">2e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span> <span class="o">+</span> <span class="mf">0.1</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">3e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span> <span class="o">+</span> <span class="mf">0.05</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">randn</span><span class="p">(</span><span class="mi">50</span><span class="p">)</span>
<span class="n">Yhat</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">fft</span><span class="o">.</span><span class="n">fft</span><span class="p">(</span><span class="n">Y</span><span class="p">)</span>

<span class="c"># plot discrete datapoints</span>
<span class="n">freqkey</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">fft</span><span class="o">.</span><span class="n">fftfreq</span><span class="p">(</span><span class="mi">50</span><span class="p">,</span><span class="n">d</span><span class="o">=</span><span class="mf">10e-3</span><span class="o">/</span><span class="mi">50</span><span class="p">)</span>
<span class="n">order</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">argsort</span><span class="p">(</span><span class="nb">abs</span><span class="p">(</span><span class="n">Yhat</span><span class="p">))</span> <span class="c"># orders array indices by entry in Yhat</span>
<span class="k">print</span> <span class="n">order</span><span class="p">[</span><span class="o">-</span><span class="mi">2</span><span class="p">:],</span> <span class="n">Yhat</span><span class="p">[</span><span class="n">order</span><span class="p">[</span><span class="o">-</span><span class="mi">2</span><span class="p">:]]</span> <span class="c"># prints the indices for the two last</span>
<span class="k">print</span> <span class="n">freqkey</span><span class="p">[</span><span class="n">order</span><span class="p">[</span><span class="o">-</span><span class="mi">2</span><span class="p">:]],</span> <span class="n">Yhat</span><span class="p">[</span><span class="n">order</span><span class="p">[</span><span class="o">-</span><span class="mi">2</span><span class="p">:]]</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt"></div>
<div class="output_subarea output_stream output_stdout output_text">
<pre>
[47  3] [ 16.00326227+14.98746746j  16.00326227-14.98746746j]
[-300.  300.] [ 16.00326227+14.98746746j  16.00326227-14.98746746j]

</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Then we can reconstruct a sinewave using the sum formula above:</p>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[103]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre><span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mf">10e-3</span><span class="p">,</span><span class="mi">50</span><span class="p">)</span>
<span class="c"># Generate simulated data with simulated noise</span>
<span class="n">Y</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">2e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span> <span class="o">+</span> <span class="mf">0.1</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="mf">3e3</span><span class="o">*</span><span class="n">X</span><span class="p">)</span> <span class="o">+</span> <span class="mf">0.05</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">randn</span><span class="p">(</span><span class="mi">50</span><span class="p">)</span>
<span class="n">Yhat</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">fft</span><span class="o">.</span><span class="n">fft</span><span class="p">(</span><span class="n">Y</span><span class="p">)</span>

<span class="c"># plot discrete datapoints</span>
<span class="n">freqkey</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">fft</span><span class="o">.</span><span class="n">fftfreq</span><span class="p">(</span><span class="mi">50</span><span class="p">,</span><span class="n">d</span><span class="o">=</span><span class="mf">10e-3</span><span class="o">/</span><span class="mi">50</span><span class="p">)</span>
<span class="n">order</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">argsort</span><span class="p">(</span><span class="nb">abs</span><span class="p">(</span><span class="n">Yhat</span><span class="p">))</span> <span class="c"># orders array indices by entry in Yhat</span>
<span class="k">print</span> <span class="n">order</span><span class="p">[</span><span class="o">-</span><span class="mi">2</span><span class="p">:],</span> <span class="n">Yhat</span><span class="p">[</span><span class="n">order</span><span class="p">[</span><span class="o">-</span><span class="mi">2</span><span class="p">:]]</span> <span class="c"># prints the indices for the two last</span>
<span class="k">print</span> <span class="n">freqkey</span><span class="p">[</span><span class="n">order</span><span class="p">[</span><span class="o">-</span><span class="mi">2</span><span class="p">:]],</span> <span class="n">Yhat</span><span class="p">[</span><span class="n">order</span><span class="p">[</span><span class="o">-</span><span class="mi">2</span><span class="p">:]]</span>
<span class="n">f</span> <span class="o">=</span> <span class="nb">abs</span><span class="p">(</span><span class="n">freqkey</span><span class="p">[</span><span class="n">order</span><span class="p">[</span><span class="o">-</span><span class="mi">1</span><span class="p">]])</span>

<span class="n">plt</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">X</span><span class="p">,</span><span class="n">np</span><span class="o">.</span><span class="n">sin</span><span class="p">(</span><span class="o">-</span><span class="mi">2</span><span class="o">*</span><span class="n">f</span><span class="o">*</span><span class="p">(</span><span class="mi">50</span><span class="p">)</span><span class="o">/</span><span class="p">(</span><span class="mf">10e-3</span><span class="p">)</span><span class="o">*</span><span class="n">X</span><span class="p">))</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt"></div>
<div class="output_subarea output_stream output_stdout output_text">
<pre>
[ 3 47] [ 16.23540604-15.64995156j  16.23540604+15.64995156j]
[ 300. -300.] [ 16.23540604-15.64995156j  16.23540604+15.64995156j]

</pre>
</div>
</div>

<div class="output_area"><div class="prompt output_prompt">
    Out[103]:</div>


<div class="output_text output_subarea output_pyout">
<pre>
[&lt;matplotlib.lines.Line2D at 0xaf4a362c&gt;]
</pre>
</div>

</div>

<div class="output_area"><div class="prompt"></div>


<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">
In&nbsp;[]:
</div>
<div class="inner_cell">
    <div class="input_area">
<div class="highlight"><pre> 
</pre></div>

</div>
</div>
</div>

</div>
    </div>
  </div>
</body>
</html>
