---
layout: post
title:  Interactive Jupyter notebook for coordinates transformation
categories: [python]
---

## Description
<br>
Sometimes you need to calculate a set of bounding boxes around different parts of the world using a specific coordinate projection system. There are a number of tools that allow drawing boxes interactively in geographical coordinates (lat, lon) using a web page, such as [geojson.io](http://geojson.io). However, I don't know an easy way of converting or defining bounding boxes in other geographical projections. Probably GIS systems provide the right functionality to do this easily, but this is also about experimenting with interactive Jupyter notebooks and create this functionality.
<br>
The following notebook demonstrates how to draw boxes in a map interactively, which can then be converted into any coordinate reference system. This page contains a rendered copy of the notebook but to test this you'll need to run a Jupyter notebook session. A copy of this notebook can be downloaded from [here](https://gist.github.com/prl900/44ae91df0168ba0c5f7bf72da084a7f8).
<br>
Interactive widgets in Jupyter notebooks allow us to make our code interactive. I'm sure this notebook can be extended and improved to make it more intuitive and interactive. Let us know if you find a way of improving this or something interesting.
<br>

<html>
<head><meta charset="utf-8" />

<title>interactive_maps</title>

<script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.1.10/require.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.0.3/jquery.min.js"></script>



<style type="text/css">
    /*!
*
* Twitter Bootstrap
*
*/
/*!
 * Bootstrap v3.3.7 (http://getbootstrap.com)
 * Copyright 2011-2016 Twitter, Inc.
 * Licensed under MIT (https://github.com/twbs/bootstrap/blob/master/LICENSE)
 */
/*! normalize.css v3.0.3 | MIT License | github.com/necolas/normalize.css */
html {
  font-family: sans-serif;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
}
body {
  margin: 0;
}
article,
aside,
details,
figcaption,
figure,
footer,
header,
hgroup,
main,
menu,
nav,
section,
summary {
  display: block;
}
audio,
canvas,
progress,
video {
  display: inline-block;
  vertical-align: baseline;
}
audio:not([controls]) {
  display: none;
  height: 0;
}
[hidden],
template {
  display: none;
}
a {
  background-color: transparent;
}
a:active,
a:hover {
  outline: 0;
}
abbr[title] {
  border-bottom: 1px dotted;
}
b,
strong {
  font-weight: bold;
}
dfn {
  font-style: italic;
}
h1 {
  font-size: 2em;
  margin: 0.67em 0;
}
mark {
  background: #ff0;
  color: #000;
}
small {
  font-size: 80%;
}
sub,
sup {
  font-size: 75%;
  line-height: 0;
  position: relative;
  vertical-align: baseline;
}
sup {
  top: -0.5em;
}
sub {
  bottom: -0.25em;
}
img {
  border: 0;
}
svg:not(:root) {
  overflow: hidden;
}
figure {
  margin: 1em 40px;
}
hr {
  box-sizing: content-box;
  height: 0;
}
pre {
  overflow: auto;
}
code,
kbd,
pre,
samp {
  font-family: monospace, monospace;
  font-size: 1em;
}
button,
input,
optgroup,
select,
textarea {
  color: inherit;
  font: inherit;
  margin: 0;
}
button {
  overflow: visible;
}
button,
select {
  text-transform: none;
}
button,
html input[type="button"],
input[type="reset"],
input[type="submit"] {
  -webkit-appearance: button;
  cursor: pointer;
}
button[disabled],
html input[disabled] {
  cursor: default;
}
button::-moz-focus-inner,
input::-moz-focus-inner {
  border: 0;
  padding: 0;
}
input {
  line-height: normal;
}
input[type="checkbox"],
input[type="radio"] {
  box-sizing: border-box;
  padding: 0;
}
input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  height: auto;
}
input[type="search"] {
  -webkit-appearance: textfield;
  box-sizing: content-box;
}
input[type="search"]::-webkit-search-cancel-button,
input[type="search"]::-webkit-search-decoration {
  -webkit-appearance: none;
}
fieldset {
  border: 1px solid #c0c0c0;
  margin: 0 2px;
  padding: 0.35em 0.625em 0.75em;
}
legend {
  border: 0;
  padding: 0;
}
textarea {
  overflow: auto;
}
optgroup {
  font-weight: bold;
}
table {
  border-collapse: collapse;
  border-spacing: 0;
}
td,
th {
  padding: 0;
}
/*! Source: https://github.com/h5bp/html5-boilerplate/blob/master/src/css/main.css */
@media print {
  *,
  *:before,
  *:after {
    background: transparent !important;
    box-shadow: none !important;
    text-shadow: none !important;
  }
  a,
  a:visited {
    text-decoration: underline;
  }
  a[href]:after {
    content: " (" attr(href) ")";
  }
  abbr[title]:after {
    content: " (" attr(title) ")";
  }
  a[href^="#"]:after,
  a[href^="javascript:"]:after {
    content: "";
  }
  pre,
  blockquote {
    border: 1px solid #999;
    page-break-inside: avoid;
  }
  thead {
    display: table-header-group;
  }
  tr,
  img {
    page-break-inside: avoid;
  }
  img {
    max-width: 100% !important;
  }
  p,
  h2,
  h3 {
    orphans: 3;
    widows: 3;
  }
  h2,
  h3 {
    page-break-after: avoid;
  }
  .navbar {
    display: none;
  }
  .btn > .caret,
  .dropup > .btn > .caret {
    border-top-color: #000 !important;
  }
  .label {
    border: 1px solid #000;
  }
  .table {
    border-collapse: collapse !important;
  }
  .table td,
  .table th {
    background-color: #fff !important;
  }
  .table-bordered th,
  .table-bordered td {
    border: 1px solid #ddd !important;
  }
}
@font-face {
  font-family: 'Glyphicons Halflings';
  src: url('../components/bootstrap/fonts/glyphicons-halflings-regular.eot');
  src: url('../components/bootstrap/fonts/glyphicons-halflings-regular.eot?#iefix') format('embedded-opentype'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.woff2') format('woff2'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.woff') format('woff'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.ttf') format('truetype'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.svg#glyphicons_halflingsregular') format('svg');
}
.glyphicon {
  position: relative;
  top: 1px;
  display: inline-block;
  font-family: 'Glyphicons Halflings';
  font-style: normal;
  font-weight: normal;
  line-height: 1;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.glyphicon-asterisk:before {
  content: "\002a";
}
.glyphicon-plus:before {
  content: "\002b";
}
.glyphicon-euro:before,
.glyphicon-eur:before {
  content: "\20ac";
}
.glyphicon-minus:before {
  content: "\2212";
}
.glyphicon-cloud:before {
  content: "\2601";
}
.glyphicon-envelope:before {
  content: "\2709";
}
.glyphicon-pencil:before {
  content: "\270f";
}
.glyphicon-glass:before {
  content: "\e001";
}
.glyphicon-music:before {
  content: "\e002";
}
.glyphicon-search:before {
  content: "\e003";
}
.glyphicon-heart:before {
  content: "\e005";
}
.glyphicon-star:before {
  content: "\e006";
}
.glyphicon-star-empty:before {
  content: "\e007";
}
.glyphicon-user:before {
  content: "\e008";
}
.glyphicon-film:before {
  content: "\e009";
}
.glyphicon-th-large:before {
  content: "\e010";
}
.glyphicon-th:before {
  content: "\e011";
}
.glyphicon-th-list:before {
  content: "\e012";
}
.glyphicon-ok:before {
  content: "\e013";
}
.glyphicon-remove:before {
  content: "\e014";
}
.glyphicon-zoom-in:before {
  content: "\e015";
}
.glyphicon-zoom-out:before {
  content: "\e016";
}
.glyphicon-off:before {
  content: "\e017";
}
.glyphicon-signal:before {
  content: "\e018";
}
.glyphicon-cog:before {
  content: "\e019";
}
.glyphicon-trash:before {
  content: "\e020";
}
.glyphicon-home:before {
  content: "\e021";
}
.glyphicon-file:before {
  content: "\e022";
}
.glyphicon-time:before {
  content: "\e023";
}
.glyphicon-road:before {
  content: "\e024";
}
.glyphicon-download-alt:before {
  content: "\e025";
}
.glyphicon-download:before {
  content: "\e026";
}
.glyphicon-upload:before {
  content: "\e027";
}
.glyphicon-inbox:before {
  content: "\e028";
}
.glyphicon-play-circle:before {
  content: "\e029";
}
.glyphicon-repeat:before {
  content: "\e030";
}
.glyphicon-refresh:before {
  content: "\e031";
}
.glyphicon-list-alt:before {
  content: "\e032";
}
.glyphicon-lock:before {
  content: "\e033";
}
.glyphicon-flag:before {
  content: "\e034";
}
.glyphicon-headphones:before {
  content: "\e035";
}
.glyphicon-volume-off:before {
  content: "\e036";
}
.glyphicon-volume-down:before {
  content: "\e037";
}
.glyphicon-volume-up:before {
  content: "\e038";
}
.glyphicon-qrcode:before {
  content: "\e039";
}
.glyphicon-barcode:before {
  content: "\e040";
}
.glyphicon-tag:before {
  content: "\e041";
}
.glyphicon-tags:before {
  content: "\e042";
}
.glyphicon-book:before {
  content: "\e043";
}
.glyphicon-bookmark:before {
  content: "\e044";
}
.glyphicon-print:before {
  content: "\e045";
}
.glyphicon-camera:before {
  content: "\e046";
}
.glyphicon-font:before {
  content: "\e047";
}
.glyphicon-bold:before {
  content: "\e048";
}
.glyphicon-italic:before {
  content: "\e049";
}
.glyphicon-text-height:before {
  content: "\e050";
}
.glyphicon-text-width:before {
  content: "\e051";
}
.glyphicon-align-left:before {
  content: "\e052";
}
.glyphicon-align-center:before {
  content: "\e053";
}
.glyphicon-align-right:before {
  content: "\e054";
}
.glyphicon-align-justify:before {
  content: "\e055";
}
.glyphicon-list:before {
  content: "\e056";
}
.glyphicon-indent-left:before {
  content: "\e057";
}
.glyphicon-indent-right:before {
  content: "\e058";
}
.glyphicon-facetime-video:before {
  content: "\e059";
}
.glyphicon-picture:before {
  content: "\e060";
}
.glyphicon-map-marker:before {
  content: "\e062";
}
.glyphicon-adjust:before {
  content: "\e063";
}
.glyphicon-tint:before {
  content: "\e064";
}
.glyphicon-edit:before {
  content: "\e065";
}
.glyphicon-share:before {
  content: "\e066";
}
.glyphicon-check:before {
  content: "\e067";
}
.glyphicon-move:before {
  content: "\e068";
}
.glyphicon-step-backward:before {
  content: "\e069";
}
.glyphicon-fast-backward:before {
  content: "\e070";
}
.glyphicon-backward:before {
  content: "\e071";
}
.glyphicon-play:before {
  content: "\e072";
}
.glyphicon-pause:before {
  content: "\e073";
}
.glyphicon-stop:before {
  content: "\e074";
}
.glyphicon-forward:before {
  content: "\e075";
}
.glyphicon-fast-forward:before {
  content: "\e076";
}
.glyphicon-step-forward:before {
  content: "\e077";
}
.glyphicon-eject:before {
  content: "\e078";
}
.glyphicon-chevron-left:before {
  content: "\e079";
}
.glyphicon-chevron-right:before {
  content: "\e080";
}
.glyphicon-plus-sign:before {
  content: "\e081";
}
.glyphicon-minus-sign:before {
  content: "\e082";
}
.glyphicon-remove-sign:before {
  content: "\e083";
}
.glyphicon-ok-sign:before {
  content: "\e084";
}
.glyphicon-question-sign:before {
  content: "\e085";
}
.glyphicon-info-sign:before {
  content: "\e086";
}
.glyphicon-screenshot:before {
  content: "\e087";
}
.glyphicon-remove-circle:before {
  content: "\e088";
}
.glyphicon-ok-circle:before {
  content: "\e089";
}
.glyphicon-ban-circle:before {
  content: "\e090";
}
.glyphicon-arrow-left:before {
  content: "\e091";
}
.glyphicon-arrow-right:before {
  content: "\e092";
}
.glyphicon-arrow-up:before {
  content: "\e093";
}
.glyphicon-arrow-down:before {
  content: "\e094";
}
.glyphicon-share-alt:before {
  content: "\e095";
}
.glyphicon-resize-full:before {
  content: "\e096";
}
.glyphicon-resize-small:before {
  content: "\e097";
}
.glyphicon-exclamation-sign:before {
  content: "\e101";
}
.glyphicon-gift:before {
  content: "\e102";
}
.glyphicon-leaf:before {
  content: "\e103";
}
.glyphicon-fire:before {
  content: "\e104";
}
.glyphicon-eye-open:before {
  content: "\e105";
}
.glyphicon-eye-close:before {
  content: "\e106";
}
.glyphicon-warning-sign:before {
  content: "\e107";
}
.glyphicon-plane:before {
  content: "\e108";
}
.glyphicon-calendar:before {
  content: "\e109";
}
.glyphicon-random:before {
  content: "\e110";
}
.glyphicon-comment:before {
  content: "\e111";
}
.glyphicon-magnet:before {
  content: "\e112";
}
.glyphicon-chevron-up:before {
  content: "\e113";
}
.glyphicon-chevron-down:before {
  content: "\e114";
}
.glyphicon-retweet:before {
  content: "\e115";
}
.glyphicon-shopping-cart:before {
  content: "\e116";
}
.glyphicon-folder-close:before {
  content: "\e117";
}
.glyphicon-folder-open:before {
  content: "\e118";
}
.glyphicon-resize-vertical:before {
  content: "\e119";
}
.glyphicon-resize-horizontal:before {
  content: "\e120";
}
.glyphicon-hdd:before {
  content: "\e121";
}
.glyphicon-bullhorn:before {
  content: "\e122";
}
.glyphicon-bell:before {
  content: "\e123";
}
.glyphicon-certificate:before {
  content: "\e124";
}
.glyphicon-thumbs-up:before {
  content: "\e125";
}
.glyphicon-thumbs-down:before {
  content: "\e126";
}
.glyphicon-hand-right:before {
  content: "\e127";
}
.glyphicon-hand-left:before {
  content: "\e128";
}
.glyphicon-hand-up:before {
  content: "\e129";
}
.glyphicon-hand-down:before {
  content: "\e130";
}
.glyphicon-circle-arrow-right:before {
  content: "\e131";
}
.glyphicon-circle-arrow-left:before {
  content: "\e132";
}
.glyphicon-circle-arrow-up:before {
  content: "\e133";
}
.glyphicon-circle-arrow-down:before {
  content: "\e134";
}
.glyphicon-globe:before {
  content: "\e135";
}
.glyphicon-wrench:before {
  content: "\e136";
}
.glyphicon-tasks:before {
  content: "\e137";
}
.glyphicon-filter:before {
  content: "\e138";
}
.glyphicon-briefcase:before {
  content: "\e139";
}
.glyphicon-fullscreen:before {
  content: "\e140";
}
.glyphicon-dashboard:before {
  content: "\e141";
}
.glyphicon-paperclip:before {
  content: "\e142";
}
.glyphicon-heart-empty:before {
  content: "\e143";
}
.glyphicon-link:before {
  content: "\e144";
}
.glyphicon-phone:before {
  content: "\e145";
}
.glyphicon-pushpin:before {
  content: "\e146";
}
.glyphicon-usd:before {
  content: "\e148";
}
.glyphicon-gbp:before {
  content: "\e149";
}
.glyphicon-sort:before {
  content: "\e150";
}
.glyphicon-sort-by-alphabet:before {
  content: "\e151";
}
.glyphicon-sort-by-alphabet-alt:before {
  content: "\e152";
}
.glyphicon-sort-by-order:before {
  content: "\e153";
}
.glyphicon-sort-by-order-alt:before {
  content: "\e154";
}
.glyphicon-sort-by-attributes:before {
  content: "\e155";
}
.glyphicon-sort-by-attributes-alt:before {
  content: "\e156";
}
.glyphicon-unchecked:before {
  content: "\e157";
}
.glyphicon-expand:before {
  content: "\e158";
}
.glyphicon-collapse-down:before {
  content: "\e159";
}
.glyphicon-collapse-up:before {
  content: "\e160";
}
.glyphicon-log-in:before {
  content: "\e161";
}
.glyphicon-flash:before {
  content: "\e162";
}
.glyphicon-log-out:before {
  content: "\e163";
}
.glyphicon-new-window:before {
  content: "\e164";
}
.glyphicon-record:before {
  content: "\e165";
}
.glyphicon-save:before {
  content: "\e166";
}
.glyphicon-open:before {
  content: "\e167";
}
.glyphicon-saved:before {
  content: "\e168";
}
.glyphicon-import:before {
  content: "\e169";
}
.glyphicon-export:before {
  content: "\e170";
}
.glyphicon-send:before {
  content: "\e171";
}
.glyphicon-floppy-disk:before {
  content: "\e172";
}
.glyphicon-floppy-saved:before {
  content: "\e173";
}
.glyphicon-floppy-remove:before {
  content: "\e174";
}
.glyphicon-floppy-save:before {
  content: "\e175";
}
.glyphicon-floppy-open:before {
  content: "\e176";
}
.glyphicon-credit-card:before {
  content: "\e177";
}
.glyphicon-transfer:before {
  content: "\e178";
}
.glyphicon-cutlery:before {
  content: "\e179";
}
.glyphicon-header:before {
  content: "\e180";
}
.glyphicon-compressed:before {
  content: "\e181";
}
.glyphicon-earphone:before {
  content: "\e182";
}
.glyphicon-phone-alt:before {
  content: "\e183";
}
.glyphicon-tower:before {
  content: "\e184";
}
.glyphicon-stats:before {
  content: "\e185";
}
.glyphicon-sd-video:before {
  content: "\e186";
}
.glyphicon-hd-video:before {
  content: "\e187";
}
.glyphicon-subtitles:before {
  content: "\e188";
}
.glyphicon-sound-stereo:before {
  content: "\e189";
}
.glyphicon-sound-dolby:before {
  content: "\e190";
}
.glyphicon-sound-5-1:before {
  content: "\e191";
}
.glyphicon-sound-6-1:before {
  content: "\e192";
}
.glyphicon-sound-7-1:before {
  content: "\e193";
}
.glyphicon-copyright-mark:before {
  content: "\e194";
}
.glyphicon-registration-mark:before {
  content: "\e195";
}
.glyphicon-cloud-download:before {
  content: "\e197";
}
.glyphicon-cloud-upload:before {
  content: "\e198";
}
.glyphicon-tree-conifer:before {
  content: "\e199";
}
.glyphicon-tree-deciduous:before {
  content: "\e200";
}
.glyphicon-cd:before {
  content: "\e201";
}
.glyphicon-save-file:before {
  content: "\e202";
}
.glyphicon-open-file:before {
  content: "\e203";
}
.glyphicon-level-up:before {
  content: "\e204";
}
.glyphicon-copy:before {
  content: "\e205";
}
.glyphicon-paste:before {
  content: "\e206";
}
.glyphicon-alert:before {
  content: "\e209";
}
.glyphicon-equalizer:before {
  content: "\e210";
}
.glyphicon-king:before {
  content: "\e211";
}
.glyphicon-queen:before {
  content: "\e212";
}
.glyphicon-pawn:before {
  content: "\e213";
}
.glyphicon-bishop:before {
  content: "\e214";
}
.glyphicon-knight:before {
  content: "\e215";
}
.glyphicon-baby-formula:before {
  content: "\e216";
}
.glyphicon-tent:before {
  content: "\26fa";
}
.glyphicon-blackboard:before {
  content: "\e218";
}
.glyphicon-bed:before {
  content: "\e219";
}
.glyphicon-apple:before {
  content: "\f8ff";
}
.glyphicon-erase:before {
  content: "\e221";
}
.glyphicon-hourglass:before {
  content: "\231b";
}
.glyphicon-lamp:before {
  content: "\e223";
}
.glyphicon-duplicate:before {
  content: "\e224";
}
.glyphicon-piggy-bank:before {
  content: "\e225";
}
.glyphicon-scissors:before {
  content: "\e226";
}
.glyphicon-bitcoin:before {
  content: "\e227";
}
.glyphicon-btc:before {
  content: "\e227";
}
.glyphicon-xbt:before {
  content: "\e227";
}
.glyphicon-yen:before {
  content: "\00a5";
}
.glyphicon-jpy:before {
  content: "\00a5";
}
.glyphicon-ruble:before {
  content: "\20bd";
}
.glyphicon-rub:before {
  content: "\20bd";
}
.glyphicon-scale:before {
  content: "\e230";
}
.glyphicon-ice-lolly:before {
  content: "\e231";
}
.glyphicon-ice-lolly-tasted:before {
  content: "\e232";
}
.glyphicon-education:before {
  content: "\e233";
}
.glyphicon-option-horizontal:before {
  content: "\e234";
}
.glyphicon-option-vertical:before {
  content: "\e235";
}
.glyphicon-menu-hamburger:before {
  content: "\e236";
}
.glyphicon-modal-window:before {
  content: "\e237";
}
.glyphicon-oil:before {
  content: "\e238";
}
.glyphicon-grain:before {
  content: "\e239";
}
.glyphicon-sunglasses:before {
  content: "\e240";
}
.glyphicon-text-size:before {
  content: "\e241";
}
.glyphicon-text-color:before {
  content: "\e242";
}
.glyphicon-text-background:before {
  content: "\e243";
}
.glyphicon-object-align-top:before {
  content: "\e244";
}
.glyphicon-object-align-bottom:before {
  content: "\e245";
}
.glyphicon-object-align-horizontal:before {
  content: "\e246";
}
.glyphicon-object-align-left:before {
  content: "\e247";
}
.glyphicon-object-align-vertical:before {
  content: "\e248";
}
.glyphicon-object-align-right:before {
  content: "\e249";
}
.glyphicon-triangle-right:before {
  content: "\e250";
}
.glyphicon-triangle-left:before {
  content: "\e251";
}
.glyphicon-triangle-bottom:before {
  content: "\e252";
}
.glyphicon-triangle-top:before {
  content: "\e253";
}
.glyphicon-console:before {
  content: "\e254";
}
.glyphicon-superscript:before {
  content: "\e255";
}
.glyphicon-subscript:before {
  content: "\e256";
}
.glyphicon-menu-left:before {
  content: "\e257";
}
.glyphicon-menu-right:before {
  content: "\e258";
}
.glyphicon-menu-down:before {
  content: "\e259";
}
.glyphicon-menu-up:before {
  content: "\e260";
}
* {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
*:before,
*:after {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
html {
  font-size: 10px;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}
body {
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-size: 13px;
  line-height: 1.42857143;
  color: #000;
  background-color: #fff;
}
input,
button,
select,
textarea {
  font-family: inherit;
  font-size: inherit;
  line-height: inherit;
}
a {
  color: #337ab7;
  text-decoration: none;
}
a:hover,
a:focus {
  color: #23527c;
  text-decoration: underline;
}
a:focus {
  outline: 5px auto -webkit-focus-ring-color;
  outline-offset: -2px;
}
figure {
  margin: 0;
}
img {
  vertical-align: middle;
}
.img-responsive,
.thumbnail > img,
.thumbnail a > img,
.carousel-inner > .item > img,
.carousel-inner > .item > a > img {
  display: block;
  max-width: 100%;
  height: auto;
}
.img-rounded {
  border-radius: 3px;
}
.img-thumbnail {
  padding: 4px;
  line-height: 1.42857143;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 2px;
  -webkit-transition: all 0.2s ease-in-out;
  -o-transition: all 0.2s ease-in-out;
  transition: all 0.2s ease-in-out;
  display: inline-block;
  max-width: 100%;
  height: auto;
}
.img-circle {
  border-radius: 50%;
}
hr {
  margin-top: 18px;
  margin-bottom: 18px;
  border: 0;
  border-top: 1px solid #eeeeee;
}
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  padding: 0;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
}
.sr-only-focusable:active,
.sr-only-focusable:focus {
  position: static;
  width: auto;
  height: auto;
  margin: 0;
  overflow: visible;
  clip: auto;
}
[role="button"] {
  cursor: pointer;
}
h1,
h2,
h3,
h4,
h5,
h6,
.h1,
.h2,
.h3,
.h4,
.h5,
.h6 {
  font-family: inherit;
  font-weight: 500;
  line-height: 1.1;
  color: inherit;
}
h1 small,
h2 small,
h3 small,
h4 small,
h5 small,
h6 small,
.h1 small,
.h2 small,
.h3 small,
.h4 small,
.h5 small,
.h6 small,
h1 .small,
h2 .small,
h3 .small,
h4 .small,
h5 .small,
h6 .small,
.h1 .small,
.h2 .small,
.h3 .small,
.h4 .small,
.h5 .small,
.h6 .small {
  font-weight: normal;
  line-height: 1;
  color: #777777;
}
h1,
.h1,
h2,
.h2,
h3,
.h3 {
  margin-top: 18px;
  margin-bottom: 9px;
}
h1 small,
.h1 small,
h2 small,
.h2 small,
h3 small,
.h3 small,
h1 .small,
.h1 .small,
h2 .small,
.h2 .small,
h3 .small,
.h3 .small {
  font-size: 65%;
}
h4,
.h4,
h5,
.h5,
h6,
.h6 {
  margin-top: 9px;
  margin-bottom: 9px;
}
h4 small,
.h4 small,
h5 small,
.h5 small,
h6 small,
.h6 small,
h4 .small,
.h4 .small,
h5 .small,
.h5 .small,
h6 .small,
.h6 .small {
  font-size: 75%;
}
h1,
.h1 {
  font-size: 33px;
}
h2,
.h2 {
  font-size: 27px;
}
h3,
.h3 {
  font-size: 23px;
}
h4,
.h4 {
  font-size: 17px;
}
h5,
.h5 {
  font-size: 13px;
}
h6,
.h6 {
  font-size: 12px;
}
p {
  margin: 0 0 9px;
}
.lead {
  margin-bottom: 18px;
  font-size: 14px;
  font-weight: 300;
  line-height: 1.4;
}
@media (min-width: 768px) {
  .lead {
    font-size: 19.5px;
  }
}
small,
.small {
  font-size: 92%;
}
mark,
.mark {
  background-color: #fcf8e3;
  padding: .2em;
}
.text-left {
  text-align: left;
}
.text-right {
  text-align: right;
}
.text-center {
  text-align: center;
}
.text-justify {
  text-align: justify;
}
.text-nowrap {
  white-space: nowrap;
}
.text-lowercase {
  text-transform: lowercase;
}
.text-uppercase {
  text-transform: uppercase;
}
.text-capitalize {
  text-transform: capitalize;
}
.text-muted {
  color: #777777;
}
.text-primary {
  color: #337ab7;
}
a.text-primary:hover,
a.text-primary:focus {
  color: #286090;
}
.text-success {
  color: #3c763d;
}
a.text-success:hover,
a.text-success:focus {
  color: #2b542c;
}
.text-info {
  color: #31708f;
}
a.text-info:hover,
a.text-info:focus {
  color: #245269;
}
.text-warning {
  color: #8a6d3b;
}
a.text-warning:hover,
a.text-warning:focus {
  color: #66512c;
}
.text-danger {
  color: #a94442;
}
a.text-danger:hover,
a.text-danger:focus {
  color: #843534;
}
.bg-primary {
  color: #fff;
  background-color: #337ab7;
}
a.bg-primary:hover,
a.bg-primary:focus {
  background-color: #286090;
}
.bg-success {
  background-color: #dff0d8;
}
a.bg-success:hover,
a.bg-success:focus {
  background-color: #c1e2b3;
}
.bg-info {
  background-color: #d9edf7;
}
a.bg-info:hover,
a.bg-info:focus {
  background-color: #afd9ee;
}
.bg-warning {
  background-color: #fcf8e3;
}
a.bg-warning:hover,
a.bg-warning:focus {
  background-color: #f7ecb5;
}
.bg-danger {
  background-color: #f2dede;
}
a.bg-danger:hover,
a.bg-danger:focus {
  background-color: #e4b9b9;
}
.page-header {
  padding-bottom: 8px;
  margin: 36px 0 18px;
  border-bottom: 1px solid #eeeeee;
}
ul,
ol {
  margin-top: 0;
  margin-bottom: 9px;
}
ul ul,
ol ul,
ul ol,
ol ol {
  margin-bottom: 0;
}
.list-unstyled {
  padding-left: 0;
  list-style: none;
}
.list-inline {
  padding-left: 0;
  list-style: none;
  margin-left: -5px;
}
.list-inline > li {
  display: inline-block;
  padding-left: 5px;
  padding-right: 5px;
}
dl {
  margin-top: 0;
  margin-bottom: 18px;
}
dt,
dd {
  line-height: 1.42857143;
}
dt {
  font-weight: bold;
}
dd {
  margin-left: 0;
}
@media (min-width: 541px) {
  .dl-horizontal dt {
    float: left;
    width: 160px;
    clear: left;
    text-align: right;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .dl-horizontal dd {
    margin-left: 180px;
  }
}
abbr[title],
abbr[data-original-title] {
  cursor: help;
  border-bottom: 1px dotted #777777;
}
.initialism {
  font-size: 90%;
  text-transform: uppercase;
}
blockquote {
  padding: 9px 18px;
  margin: 0 0 18px;
  font-size: inherit;
  border-left: 5px solid #eeeeee;
}
blockquote p:last-child,
blockquote ul:last-child,
blockquote ol:last-child {
  margin-bottom: 0;
}
blockquote footer,
blockquote small,
blockquote .small {
  display: block;
  font-size: 80%;
  line-height: 1.42857143;
  color: #777777;
}
blockquote footer:before,
blockquote small:before,
blockquote .small:before {
  content: '\2014 \00A0';
}
.blockquote-reverse,
blockquote.pull-right {
  padding-right: 15px;
  padding-left: 0;
  border-right: 5px solid #eeeeee;
  border-left: 0;
  text-align: right;
}
.blockquote-reverse footer:before,
blockquote.pull-right footer:before,
.blockquote-reverse small:before,
blockquote.pull-right small:before,
.blockquote-reverse .small:before,
blockquote.pull-right .small:before {
  content: '';
}
.blockquote-reverse footer:after,
blockquote.pull-right footer:after,
.blockquote-reverse small:after,
blockquote.pull-right small:after,
.blockquote-reverse .small:after,
blockquote.pull-right .small:after {
  content: '\00A0 \2014';
}
address {
  margin-bottom: 18px;
  font-style: normal;
  line-height: 1.42857143;
}
code,
kbd,
pre,
samp {
  font-family: monospace;
}
code {
  padding: 2px 4px;
  font-size: 90%;
  color: #c7254e;
  background-color: #f9f2f4;
  border-radius: 2px;
}
kbd {
  padding: 2px 4px;
  font-size: 90%;
  color: #888;
  background-color: transparent;
  border-radius: 1px;
  box-shadow: inset 0 -1px 0 rgba(0, 0, 0, 0.25);
}
kbd kbd {
  padding: 0;
  font-size: 100%;
  font-weight: bold;
  box-shadow: none;
}
pre {
  display: block;
  padding: 8.5px;
  margin: 0 0 9px;
  font-size: 12px;
  line-height: 1.42857143;
  word-break: break-all;
  word-wrap: break-word;
  color: #333333;
  background-color: #f5f5f5;
  border: 1px solid #ccc;
  border-radius: 2px;
}
pre code {
  padding: 0;
  font-size: inherit;
  color: inherit;
  white-space: pre-wrap;
  background-color: transparent;
  border-radius: 0;
}
.pre-scrollable {
  max-height: 340px;
  overflow-y: scroll;
}
.container {
  margin-right: auto;
  margin-left: auto;
  padding-left: 0px;
  padding-right: 0px;
}
@media (min-width: 768px) {
  .container {
    width: 768px;
  }
}
@media (min-width: 992px) {
  .container {
    width: 940px;
  }
}
@media (min-width: 1200px) {
  .container {
    width: 1140px;
  }
}
.container-fluid {
  margin-right: auto;
  margin-left: auto;
  padding-left: 0px;
  padding-right: 0px;
}
.row {
  margin-left: 0px;
  margin-right: 0px;
}
.col-xs-1, .col-sm-1, .col-md-1, .col-lg-1, .col-xs-2, .col-sm-2, .col-md-2, .col-lg-2, .col-xs-3, .col-sm-3, .col-md-3, .col-lg-3, .col-xs-4, .col-sm-4, .col-md-4, .col-lg-4, .col-xs-5, .col-sm-5, .col-md-5, .col-lg-5, .col-xs-6, .col-sm-6, .col-md-6, .col-lg-6, .col-xs-7, .col-sm-7, .col-md-7, .col-lg-7, .col-xs-8, .col-sm-8, .col-md-8, .col-lg-8, .col-xs-9, .col-sm-9, .col-md-9, .col-lg-9, .col-xs-10, .col-sm-10, .col-md-10, .col-lg-10, .col-xs-11, .col-sm-11, .col-md-11, .col-lg-11, .col-xs-12, .col-sm-12, .col-md-12, .col-lg-12 {
  position: relative;
  min-height: 1px;
  padding-left: 0px;
  padding-right: 0px;
}
.col-xs-1, .col-xs-2, .col-xs-3, .col-xs-4, .col-xs-5, .col-xs-6, .col-xs-7, .col-xs-8, .col-xs-9, .col-xs-10, .col-xs-11, .col-xs-12 {
  float: left;
}
.col-xs-12 {
  width: 100%;
}
.col-xs-11 {
  width: 91.66666667%;
}
.col-xs-10 {
  width: 83.33333333%;
}
.col-xs-9 {
  width: 75%;
}
.col-xs-8 {
  width: 66.66666667%;
}
.col-xs-7 {
  width: 58.33333333%;
}
.col-xs-6 {
  width: 50%;
}
.col-xs-5 {
  width: 41.66666667%;
}
.col-xs-4 {
  width: 33.33333333%;
}
.col-xs-3 {
  width: 25%;
}
.col-xs-2 {
  width: 16.66666667%;
}
.col-xs-1 {
  width: 8.33333333%;
}
.col-xs-pull-12 {
  right: 100%;
}
.col-xs-pull-11 {
  right: 91.66666667%;
}
.col-xs-pull-10 {
  right: 83.33333333%;
}
.col-xs-pull-9 {
  right: 75%;
}
.col-xs-pull-8 {
  right: 66.66666667%;
}
.col-xs-pull-7 {
  right: 58.33333333%;
}
.col-xs-pull-6 {
  right: 50%;
}
.col-xs-pull-5 {
  right: 41.66666667%;
}
.col-xs-pull-4 {
  right: 33.33333333%;
}
.col-xs-pull-3 {
  right: 25%;
}
.col-xs-pull-2 {
  right: 16.66666667%;
}
.col-xs-pull-1 {
  right: 8.33333333%;
}
.col-xs-pull-0 {
  right: auto;
}
.col-xs-push-12 {
  left: 100%;
}
.col-xs-push-11 {
  left: 91.66666667%;
}
.col-xs-push-10 {
  left: 83.33333333%;
}
.col-xs-push-9 {
  left: 75%;
}
.col-xs-push-8 {
  left: 66.66666667%;
}
.col-xs-push-7 {
  left: 58.33333333%;
}
.col-xs-push-6 {
  left: 50%;
}
.col-xs-push-5 {
  left: 41.66666667%;
}
.col-xs-push-4 {
  left: 33.33333333%;
}
.col-xs-push-3 {
  left: 25%;
}
.col-xs-push-2 {
  left: 16.66666667%;
}
.col-xs-push-1 {
  left: 8.33333333%;
}
.col-xs-push-0 {
  left: auto;
}
.col-xs-offset-12 {
  margin-left: 100%;
}
.col-xs-offset-11 {
  margin-left: 91.66666667%;
}
.col-xs-offset-10 {
  margin-left: 83.33333333%;
}
.col-xs-offset-9 {
  margin-left: 75%;
}
.col-xs-offset-8 {
  margin-left: 66.66666667%;
}
.col-xs-offset-7 {
  margin-left: 58.33333333%;
}
.col-xs-offset-6 {
  margin-left: 50%;
}
.col-xs-offset-5 {
  margin-left: 41.66666667%;
}
.col-xs-offset-4 {
  margin-left: 33.33333333%;
}
.col-xs-offset-3 {
  margin-left: 25%;
}
.col-xs-offset-2 {
  margin-left: 16.66666667%;
}
.col-xs-offset-1 {
  margin-left: 8.33333333%;
}
.col-xs-offset-0 {
  margin-left: 0%;
}
@media (min-width: 768px) {
  .col-sm-1, .col-sm-2, .col-sm-3, .col-sm-4, .col-sm-5, .col-sm-6, .col-sm-7, .col-sm-8, .col-sm-9, .col-sm-10, .col-sm-11, .col-sm-12 {
    float: left;
  }
  .col-sm-12 {
    width: 100%;
  }
  .col-sm-11 {
    width: 91.66666667%;
  }
  .col-sm-10 {
    width: 83.33333333%;
  }
  .col-sm-9 {
    width: 75%;
  }
  .col-sm-8 {
    width: 66.66666667%;
  }
  .col-sm-7 {
    width: 58.33333333%;
  }
  .col-sm-6 {
    width: 50%;
  }
  .col-sm-5 {
    width: 41.66666667%;
  }
  .col-sm-4 {
    width: 33.33333333%;
  }
  .col-sm-3 {
    width: 25%;
  }
  .col-sm-2 {
    width: 16.66666667%;
  }
  .col-sm-1 {
    width: 8.33333333%;
  }
  .col-sm-pull-12 {
    right: 100%;
  }
  .col-sm-pull-11 {
    right: 91.66666667%;
  }
  .col-sm-pull-10 {
    right: 83.33333333%;
  }
  .col-sm-pull-9 {
    right: 75%;
  }
  .col-sm-pull-8 {
    right: 66.66666667%;
  }
  .col-sm-pull-7 {
    right: 58.33333333%;
  }
  .col-sm-pull-6 {
    right: 50%;
  }
  .col-sm-pull-5 {
    right: 41.66666667%;
  }
  .col-sm-pull-4 {
    right: 33.33333333%;
  }
  .col-sm-pull-3 {
    right: 25%;
  }
  .col-sm-pull-2 {
    right: 16.66666667%;
  }
  .col-sm-pull-1 {
    right: 8.33333333%;
  }
  .col-sm-pull-0 {
    right: auto;
  }
  .col-sm-push-12 {
    left: 100%;
  }
  .col-sm-push-11 {
    left: 91.66666667%;
  }
  .col-sm-push-10 {
    left: 83.33333333%;
  }
  .col-sm-push-9 {
    left: 75%;
  }
  .col-sm-push-8 {
    left: 66.66666667%;
  }
  .col-sm-push-7 {
    left: 58.33333333%;
  }
  .col-sm-push-6 {
    left: 50%;
  }
  .col-sm-push-5 {
    left: 41.66666667%;
  }
  .col-sm-push-4 {
    left: 33.33333333%;
  }
  .col-sm-push-3 {
    left: 25%;
  }
  .col-sm-push-2 {
    left: 16.66666667%;
  }
  .col-sm-push-1 {
    left: 8.33333333%;
  }
  .col-sm-push-0 {
    left: auto;
  }
  .col-sm-offset-12 {
    margin-left: 100%;
  }
  .col-sm-offset-11 {
    margin-left: 91.66666667%;
  }
  .col-sm-offset-10 {
    margin-left: 83.33333333%;
  }
  .col-sm-offset-9 {
    margin-left: 75%;
  }
  .col-sm-offset-8 {
    margin-left: 66.66666667%;
  }
  .col-sm-offset-7 {
    margin-left: 58.33333333%;
  }
  .col-sm-offset-6 {
    margin-left: 50%;
  }
  .col-sm-offset-5 {
    margin-left: 41.66666667%;
  }
  .col-sm-offset-4 {
    margin-left: 33.33333333%;
  }
  .col-sm-offset-3 {
    margin-left: 25%;
  }
  .col-sm-offset-2 {
    margin-left: 16.66666667%;
  }
  .col-sm-offset-1 {
    margin-left: 8.33333333%;
  }
  .col-sm-offset-0 {
    margin-left: 0%;
  }
}
@media (min-width: 992px) {
  .col-md-1, .col-md-2, .col-md-3, .col-md-4, .col-md-5, .col-md-6, .col-md-7, .col-md-8, .col-md-9, .col-md-10, .col-md-11, .col-md-12 {
    float: left;
  }
  .col-md-12 {
    width: 100%;
  }
  .col-md-11 {
    width: 91.66666667%;
  }
  .col-md-10 {
    width: 83.33333333%;
  }
  .col-md-9 {
    width: 75%;
  }
  .col-md-8 {
    width: 66.66666667%;
  }
  .col-md-7 {
    width: 58.33333333%;
  }
  .col-md-6 {
    width: 50%;
  }
  .col-md-5 {
    width: 41.66666667%;
  }
  .col-md-4 {
    width: 33.33333333%;
  }
  .col-md-3 {
    width: 25%;
  }
  .col-md-2 {
    width: 16.66666667%;
  }
  .col-md-1 {
    width: 8.33333333%;
  }
  .col-md-pull-12 {
    right: 100%;
  }
  .col-md-pull-11 {
    right: 91.66666667%;
  }
  .col-md-pull-10 {
    right: 83.33333333%;
  }
  .col-md-pull-9 {
    right: 75%;
  }
  .col-md-pull-8 {
    right: 66.66666667%;
  }
  .col-md-pull-7 {
    right: 58.33333333%;
  }
  .col-md-pull-6 {
    right: 50%;
  }
  .col-md-pull-5 {
    right: 41.66666667%;
  }
  .col-md-pull-4 {
    right: 33.33333333%;
  }
  .col-md-pull-3 {
    right: 25%;
  }
  .col-md-pull-2 {
    right: 16.66666667%;
  }
  .col-md-pull-1 {
    right: 8.33333333%;
  }
  .col-md-pull-0 {
    right: auto;
  }
  .col-md-push-12 {
    left: 100%;
  }
  .col-md-push-11 {
    left: 91.66666667%;
  }
  .col-md-push-10 {
    left: 83.33333333%;
  }
  .col-md-push-9 {
    left: 75%;
  }
  .col-md-push-8 {
    left: 66.66666667%;
  }
  .col-md-push-7 {
    left: 58.33333333%;
  }
  .col-md-push-6 {
    left: 50%;
  }
  .col-md-push-5 {
    left: 41.66666667%;
  }
  .col-md-push-4 {
    left: 33.33333333%;
  }
  .col-md-push-3 {
    left: 25%;
  }
  .col-md-push-2 {
    left: 16.66666667%;
  }
  .col-md-push-1 {
    left: 8.33333333%;
  }
  .col-md-push-0 {
    left: auto;
  }
  .col-md-offset-12 {
    margin-left: 100%;
  }
  .col-md-offset-11 {
    margin-left: 91.66666667%;
  }
  .col-md-offset-10 {
    margin-left: 83.33333333%;
  }
  .col-md-offset-9 {
    margin-left: 75%;
  }
  .col-md-offset-8 {
    margin-left: 66.66666667%;
  }
  .col-md-offset-7 {
    margin-left: 58.33333333%;
  }
  .col-md-offset-6 {
    margin-left: 50%;
  }
  .col-md-offset-5 {
    margin-left: 41.66666667%;
  }
  .col-md-offset-4 {
    margin-left: 33.33333333%;
  }
  .col-md-offset-3 {
    margin-left: 25%;
  }
  .col-md-offset-2 {
    margin-left: 16.66666667%;
  }
  .col-md-offset-1 {
    margin-left: 8.33333333%;
  }
  .col-md-offset-0 {
    margin-left: 0%;
  }
}
@media (min-width: 1200px) {
  .col-lg-1, .col-lg-2, .col-lg-3, .col-lg-4, .col-lg-5, .col-lg-6, .col-lg-7, .col-lg-8, .col-lg-9, .col-lg-10, .col-lg-11, .col-lg-12 {
    float: left;
  }
  .col-lg-12 {
    width: 100%;
  }
  .col-lg-11 {
    width: 91.66666667%;
  }
  .col-lg-10 {
    width: 83.33333333%;
  }
  .col-lg-9 {
    width: 75%;
  }
  .col-lg-8 {
    width: 66.66666667%;
  }
  .col-lg-7 {
    width: 58.33333333%;
  }
  .col-lg-6 {
    width: 50%;
  }
  .col-lg-5 {
    width: 41.66666667%;
  }
  .col-lg-4 {
    width: 33.33333333%;
  }
  .col-lg-3 {
    width: 25%;
  }
  .col-lg-2 {
    width: 16.66666667%;
  }
  .col-lg-1 {
    width: 8.33333333%;
  }
  .col-lg-pull-12 {
    right: 100%;
  }
  .col-lg-pull-11 {
    right: 91.66666667%;
  }
  .col-lg-pull-10 {
    right: 83.33333333%;
  }
  .col-lg-pull-9 {
    right: 75%;
  }
  .col-lg-pull-8 {
    right: 66.66666667%;
  }
  .col-lg-pull-7 {
    right: 58.33333333%;
  }
  .col-lg-pull-6 {
    right: 50%;
  }
  .col-lg-pull-5 {
    right: 41.66666667%;
  }
  .col-lg-pull-4 {
    right: 33.33333333%;
  }
  .col-lg-pull-3 {
    right: 25%;
  }
  .col-lg-pull-2 {
    right: 16.66666667%;
  }
  .col-lg-pull-1 {
    right: 8.33333333%;
  }
  .col-lg-pull-0 {
    right: auto;
  }
  .col-lg-push-12 {
    left: 100%;
  }
  .col-lg-push-11 {
    left: 91.66666667%;
  }
  .col-lg-push-10 {
    left: 83.33333333%;
  }
  .col-lg-push-9 {
    left: 75%;
  }
  .col-lg-push-8 {
    left: 66.66666667%;
  }
  .col-lg-push-7 {
    left: 58.33333333%;
  }
  .col-lg-push-6 {
    left: 50%;
  }
  .col-lg-push-5 {
    left: 41.66666667%;
  }
  .col-lg-push-4 {
    left: 33.33333333%;
  }
  .col-lg-push-3 {
    left: 25%;
  }
  .col-lg-push-2 {
    left: 16.66666667%;
  }
  .col-lg-push-1 {
    left: 8.33333333%;
  }
  .col-lg-push-0 {
    left: auto;
  }
  .col-lg-offset-12 {
    margin-left: 100%;
  }
  .col-lg-offset-11 {
    margin-left: 91.66666667%;
  }
  .col-lg-offset-10 {
    margin-left: 83.33333333%;
  }
  .col-lg-offset-9 {
    margin-left: 75%;
  }
  .col-lg-offset-8 {
    margin-left: 66.66666667%;
  }
  .col-lg-offset-7 {
    margin-left: 58.33333333%;
  }
  .col-lg-offset-6 {
    margin-left: 50%;
  }
  .col-lg-offset-5 {
    margin-left: 41.66666667%;
  }
  .col-lg-offset-4 {
    margin-left: 33.33333333%;
  }
  .col-lg-offset-3 {
    margin-left: 25%;
  }
  .col-lg-offset-2 {
    margin-left: 16.66666667%;
  }
  .col-lg-offset-1 {
    margin-left: 8.33333333%;
  }
  .col-lg-offset-0 {
    margin-left: 0%;
  }
}
table {
  background-color: transparent;
}
caption {
  padding-top: 8px;
  padding-bottom: 8px;
  color: #777777;
  text-align: left;
}
th {
  text-align: left;
}
.table {
  width: 100%;
  max-width: 100%;
  margin-bottom: 18px;
}
.table > thead > tr > th,
.table > tbody > tr > th,
.table > tfoot > tr > th,
.table > thead > tr > td,
.table > tbody > tr > td,
.table > tfoot > tr > td {
  padding: 8px;
  line-height: 1.42857143;
  vertical-align: top;
  border-top: 1px solid #ddd;
}
.table > thead > tr > th {
  vertical-align: bottom;
  border-bottom: 2px solid #ddd;
}
.table > caption + thead > tr:first-child > th,
.table > colgroup + thead > tr:first-child > th,
.table > thead:first-child > tr:first-child > th,
.table > caption + thead > tr:first-child > td,
.table > colgroup + thead > tr:first-child > td,
.table > thead:first-child > tr:first-child > td {
  border-top: 0;
}
.table > tbody + tbody {
  border-top: 2px solid #ddd;
}
.table .table {
  background-color: #fff;
}
.table-condensed > thead > tr > th,
.table-condensed > tbody > tr > th,
.table-condensed > tfoot > tr > th,
.table-condensed > thead > tr > td,
.table-condensed > tbody > tr > td,
.table-condensed > tfoot > tr > td {
  padding: 5px;
}
.table-bordered {
  border: 1px solid #ddd;
}
.table-bordered > thead > tr > th,
.table-bordered > tbody > tr > th,
.table-bordered > tfoot > tr > th,
.table-bordered > thead > tr > td,
.table-bordered > tbody > tr > td,
.table-bordered > tfoot > tr > td {
  border: 1px solid #ddd;
}
.table-bordered > thead > tr > th,
.table-bordered > thead > tr > td {
  border-bottom-width: 2px;
}
.table-striped > tbody > tr:nth-of-type(odd) {
  background-color: #f9f9f9;
}
.table-hover > tbody > tr:hover {
  background-color: #f5f5f5;
}
table col[class*="col-"] {
  position: static;
  float: none;
  display: table-column;
}
table td[class*="col-"],
table th[class*="col-"] {
  position: static;
  float: none;
  display: table-cell;
}
.table > thead > tr > td.active,
.table > tbody > tr > td.active,
.table > tfoot > tr > td.active,
.table > thead > tr > th.active,
.table > tbody > tr > th.active,
.table > tfoot > tr > th.active,
.table > thead > tr.active > td,
.table > tbody > tr.active > td,
.table > tfoot > tr.active > td,
.table > thead > tr.active > th,
.table > tbody > tr.active > th,
.table > tfoot > tr.active > th {
  background-color: #f5f5f5;
}
.table-hover > tbody > tr > td.active:hover,
.table-hover > tbody > tr > th.active:hover,
.table-hover > tbody > tr.active:hover > td,
.table-hover > tbody > tr:hover > .active,
.table-hover > tbody > tr.active:hover > th {
  background-color: #e8e8e8;
}
.table > thead > tr > td.success,
.table > tbody > tr > td.success,
.table > tfoot > tr > td.success,
.table > thead > tr > th.success,
.table > tbody > tr > th.success,
.table > tfoot > tr > th.success,
.table > thead > tr.success > td,
.table > tbody > tr.success > td,
.table > tfoot > tr.success > td,
.table > thead > tr.success > th,
.table > tbody > tr.success > th,
.table > tfoot > tr.success > th {
  background-color: #dff0d8;
}
.table-hover > tbody > tr > td.success:hover,
.table-hover > tbody > tr > th.success:hover,
.table-hover > tbody > tr.success:hover > td,
.table-hover > tbody > tr:hover > .success,
.table-hover > tbody > tr.success:hover > th {
  background-color: #d0e9c6;
}
.table > thead > tr > td.info,
.table > tbody > tr > td.info,
.table > tfoot > tr > td.info,
.table > thead > tr > th.info,
.table > tbody > tr > th.info,
.table > tfoot > tr > th.info,
.table > thead > tr.info > td,
.table > tbody > tr.info > td,
.table > tfoot > tr.info > td,
.table > thead > tr.info > th,
.table > tbody > tr.info > th,
.table > tfoot > tr.info > th {
  background-color: #d9edf7;
}
.table-hover > tbody > tr > td.info:hover,
.table-hover > tbody > tr > th.info:hover,
.table-hover > tbody > tr.info:hover > td,
.table-hover > tbody > tr:hover > .info,
.table-hover > tbody > tr.info:hover > th {
  background-color: #c4e3f3;
}
.table > thead > tr > td.warning,
.table > tbody > tr > td.warning,
.table > tfoot > tr > td.warning,
.table > thead > tr > th.warning,
.table > tbody > tr > th.warning,
.table > tfoot > tr > th.warning,
.table > thead > tr.warning > td,
.table > tbody > tr.warning > td,
.table > tfoot > tr.warning > td,
.table > thead > tr.warning > th,
.table > tbody > tr.warning > th,
.table > tfoot > tr.warning > th {
  background-color: #fcf8e3;
}
.table-hover > tbody > tr > td.warning:hover,
.table-hover > tbody > tr > th.warning:hover,
.table-hover > tbody > tr.warning:hover > td,
.table-hover > tbody > tr:hover > .warning,
.table-hover > tbody > tr.warning:hover > th {
  background-color: #faf2cc;
}
.table > thead > tr > td.danger,
.table > tbody > tr > td.danger,
.table > tfoot > tr > td.danger,
.table > thead > tr > th.danger,
.table > tbody > tr > th.danger,
.table > tfoot > tr > th.danger,
.table > thead > tr.danger > td,
.table > tbody > tr.danger > td,
.table > tfoot > tr.danger > td,
.table > thead > tr.danger > th,
.table > tbody > tr.danger > th,
.table > tfoot > tr.danger > th {
  background-color: #f2dede;
}
.table-hover > tbody > tr > td.danger:hover,
.table-hover > tbody > tr > th.danger:hover,
.table-hover > tbody > tr.danger:hover > td,
.table-hover > tbody > tr:hover > .danger,
.table-hover > tbody > tr.danger:hover > th {
  background-color: #ebcccc;
}
.table-responsive {
  overflow-x: auto;
  min-height: 0.01%;
}
@media screen and (max-width: 767px) {
  .table-responsive {
    width: 100%;
    margin-bottom: 13.5px;
    overflow-y: hidden;
    -ms-overflow-style: -ms-autohiding-scrollbar;
    border: 1px solid #ddd;
  }
  .table-responsive > .table {
    margin-bottom: 0;
  }
  .table-responsive > .table > thead > tr > th,
  .table-responsive > .table > tbody > tr > th,
  .table-responsive > .table > tfoot > tr > th,
  .table-responsive > .table > thead > tr > td,
  .table-responsive > .table > tbody > tr > td,
  .table-responsive > .table > tfoot > tr > td {
    white-space: nowrap;
  }
  .table-responsive > .table-bordered {
    border: 0;
  }
  .table-responsive > .table-bordered > thead > tr > th:first-child,
  .table-responsive > .table-bordered > tbody > tr > th:first-child,
  .table-responsive > .table-bordered > tfoot > tr > th:first-child,
  .table-responsive > .table-bordered > thead > tr > td:first-child,
  .table-responsive > .table-bordered > tbody > tr > td:first-child,
  .table-responsive > .table-bordered > tfoot > tr > td:first-child {
    border-left: 0;
  }
  .table-responsive > .table-bordered > thead > tr > th:last-child,
  .table-responsive > .table-bordered > tbody > tr > th:last-child,
  .table-responsive > .table-bordered > tfoot > tr > th:last-child,
  .table-responsive > .table-bordered > thead > tr > td:last-child,
  .table-responsive > .table-bordered > tbody > tr > td:last-child,
  .table-responsive > .table-bordered > tfoot > tr > td:last-child {
    border-right: 0;
  }
  .table-responsive > .table-bordered > tbody > tr:last-child > th,
  .table-responsive > .table-bordered > tfoot > tr:last-child > th,
  .table-responsive > .table-bordered > tbody > tr:last-child > td,
  .table-responsive > .table-bordered > tfoot > tr:last-child > td {
    border-bottom: 0;
  }
}
fieldset {
  padding: 0;
  margin: 0;
  border: 0;
  min-width: 0;
}
legend {
  display: block;
  width: 100%;
  padding: 0;
  margin-bottom: 18px;
  font-size: 19.5px;
  line-height: inherit;
  color: #333333;
  border: 0;
  border-bottom: 1px solid #e5e5e5;
}
label {
  display: inline-block;
  max-width: 100%;
  margin-bottom: 5px;
  font-weight: bold;
}
input[type="search"] {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
input[type="radio"],
input[type="checkbox"] {
  margin: 4px 0 0;
  margin-top: 1px \9;
  line-height: normal;
}
input[type="file"] {
  display: block;
}
input[type="range"] {
  display: block;
  width: 100%;
}
select[multiple],
select[size] {
  height: auto;
}
input[type="file"]:focus,
input[type="radio"]:focus,
input[type="checkbox"]:focus {
  outline: 5px auto -webkit-focus-ring-color;
  outline-offset: -2px;
}
output {
  display: block;
  padding-top: 7px;
  font-size: 13px;
  line-height: 1.42857143;
  color: #555555;
}
.form-control {
  display: block;
  width: 100%;
  height: 32px;
  padding: 6px 12px;
  font-size: 13px;
  line-height: 1.42857143;
  color: #555555;
  background-color: #fff;
  background-image: none;
  border: 1px solid #ccc;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  -webkit-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  -o-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
}
.form-control:focus {
  border-color: #66afe9;
  outline: 0;
  -webkit-box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
  box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
}
.form-control::-moz-placeholder {
  color: #999;
  opacity: 1;
}
.form-control:-ms-input-placeholder {
  color: #999;
}
.form-control::-webkit-input-placeholder {
  color: #999;
}
.form-control::-ms-expand {
  border: 0;
  background-color: transparent;
}
.form-control[disabled],
.form-control[readonly],
fieldset[disabled] .form-control {
  background-color: #eeeeee;
  opacity: 1;
}
.form-control[disabled],
fieldset[disabled] .form-control {
  cursor: not-allowed;
}
textarea.form-control {
  height: auto;
}
input[type="search"] {
  -webkit-appearance: none;
}
@media screen and (-webkit-min-device-pixel-ratio: 0) {
  input[type="date"].form-control,
  input[type="time"].form-control,
  input[type="datetime-local"].form-control,
  input[type="month"].form-control {
    line-height: 32px;
  }
  input[type="date"].input-sm,
  input[type="time"].input-sm,
  input[type="datetime-local"].input-sm,
  input[type="month"].input-sm,
  .input-group-sm input[type="date"],
  .input-group-sm input[type="time"],
  .input-group-sm input[type="datetime-local"],
  .input-group-sm input[type="month"] {
    line-height: 30px;
  }
  input[type="date"].input-lg,
  input[type="time"].input-lg,
  input[type="datetime-local"].input-lg,
  input[type="month"].input-lg,
  .input-group-lg input[type="date"],
  .input-group-lg input[type="time"],
  .input-group-lg input[type="datetime-local"],
  .input-group-lg input[type="month"] {
    line-height: 45px;
  }
}
.form-group {
  margin-bottom: 15px;
}
.radio,
.checkbox {
  position: relative;
  display: block;
  margin-top: 10px;
  margin-bottom: 10px;
}
.radio label,
.checkbox label {
  min-height: 18px;
  padding-left: 20px;
  margin-bottom: 0;
  font-weight: normal;
  cursor: pointer;
}
.radio input[type="radio"],
.radio-inline input[type="radio"],
.checkbox input[type="checkbox"],
.checkbox-inline input[type="checkbox"] {
  position: absolute;
  margin-left: -20px;
  margin-top: 4px \9;
}
.radio + .radio,
.checkbox + .checkbox {
  margin-top: -5px;
}
.radio-inline,
.checkbox-inline {
  position: relative;
  display: inline-block;
  padding-left: 20px;
  margin-bottom: 0;
  vertical-align: middle;
  font-weight: normal;
  cursor: pointer;
}
.radio-inline + .radio-inline,
.checkbox-inline + .checkbox-inline {
  margin-top: 0;
  margin-left: 10px;
}
input[type="radio"][disabled],
input[type="checkbox"][disabled],
input[type="radio"].disabled,
input[type="checkbox"].disabled,
fieldset[disabled] input[type="radio"],
fieldset[disabled] input[type="checkbox"] {
  cursor: not-allowed;
}
.radio-inline.disabled,
.checkbox-inline.disabled,
fieldset[disabled] .radio-inline,
fieldset[disabled] .checkbox-inline {
  cursor: not-allowed;
}
.radio.disabled label,
.checkbox.disabled label,
fieldset[disabled] .radio label,
fieldset[disabled] .checkbox label {
  cursor: not-allowed;
}
.form-control-static {
  padding-top: 7px;
  padding-bottom: 7px;
  margin-bottom: 0;
  min-height: 31px;
}
.form-control-static.input-lg,
.form-control-static.input-sm {
  padding-left: 0;
  padding-right: 0;
}
.input-sm {
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
select.input-sm {
  height: 30px;
  line-height: 30px;
}
textarea.input-sm,
select[multiple].input-sm {
  height: auto;
}
.form-group-sm .form-control {
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
.form-group-sm select.form-control {
  height: 30px;
  line-height: 30px;
}
.form-group-sm textarea.form-control,
.form-group-sm select[multiple].form-control {
  height: auto;
}
.form-group-sm .form-control-static {
  height: 30px;
  min-height: 30px;
  padding: 6px 10px;
  font-size: 12px;
  line-height: 1.5;
}
.input-lg {
  height: 45px;
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
select.input-lg {
  height: 45px;
  line-height: 45px;
}
textarea.input-lg,
select[multiple].input-lg {
  height: auto;
}
.form-group-lg .form-control {
  height: 45px;
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
.form-group-lg select.form-control {
  height: 45px;
  line-height: 45px;
}
.form-group-lg textarea.form-control,
.form-group-lg select[multiple].form-control {
  height: auto;
}
.form-group-lg .form-control-static {
  height: 45px;
  min-height: 35px;
  padding: 11px 16px;
  font-size: 17px;
  line-height: 1.3333333;
}
.has-feedback {
  position: relative;
}
.has-feedback .form-control {
  padding-right: 40px;
}
.form-control-feedback {
  position: absolute;
  top: 0;
  right: 0;
  z-index: 2;
  display: block;
  width: 32px;
  height: 32px;
  line-height: 32px;
  text-align: center;
  pointer-events: none;
}
.input-lg + .form-control-feedback,
.input-group-lg + .form-control-feedback,
.form-group-lg .form-control + .form-control-feedback {
  width: 45px;
  height: 45px;
  line-height: 45px;
}
.input-sm + .form-control-feedback,
.input-group-sm + .form-control-feedback,
.form-group-sm .form-control + .form-control-feedback {
  width: 30px;
  height: 30px;
  line-height: 30px;
}
.has-success .help-block,
.has-success .control-label,
.has-success .radio,
.has-success .checkbox,
.has-success .radio-inline,
.has-success .checkbox-inline,
.has-success.radio label,
.has-success.checkbox label,
.has-success.radio-inline label,
.has-success.checkbox-inline label {
  color: #3c763d;
}
.has-success .form-control {
  border-color: #3c763d;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
}
.has-success .form-control:focus {
  border-color: #2b542c;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #67b168;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #67b168;
}
.has-success .input-group-addon {
  color: #3c763d;
  border-color: #3c763d;
  background-color: #dff0d8;
}
.has-success .form-control-feedback {
  color: #3c763d;
}
.has-warning .help-block,
.has-warning .control-label,
.has-warning .radio,
.has-warning .checkbox,
.has-warning .radio-inline,
.has-warning .checkbox-inline,
.has-warning.radio label,
.has-warning.checkbox label,
.has-warning.radio-inline label,
.has-warning.checkbox-inline label {
  color: #8a6d3b;
}
.has-warning .form-control {
  border-color: #8a6d3b;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
}
.has-warning .form-control:focus {
  border-color: #66512c;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #c0a16b;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #c0a16b;
}
.has-warning .input-group-addon {
  color: #8a6d3b;
  border-color: #8a6d3b;
  background-color: #fcf8e3;
}
.has-warning .form-control-feedback {
  color: #8a6d3b;
}
.has-error .help-block,
.has-error .control-label,
.has-error .radio,
.has-error .checkbox,
.has-error .radio-inline,
.has-error .checkbox-inline,
.has-error.radio label,
.has-error.checkbox label,
.has-error.radio-inline label,
.has-error.checkbox-inline label {
  color: #a94442;
}
.has-error .form-control {
  border-color: #a94442;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
}
.has-error .form-control:focus {
  border-color: #843534;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #ce8483;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #ce8483;
}
.has-error .input-group-addon {
  color: #a94442;
  border-color: #a94442;
  background-color: #f2dede;
}
.has-error .form-control-feedback {
  color: #a94442;
}
.has-feedback label ~ .form-control-feedback {
  top: 23px;
}
.has-feedback label.sr-only ~ .form-control-feedback {
  top: 0;
}
.help-block {
  display: block;
  margin-top: 5px;
  margin-bottom: 10px;
  color: #404040;
}
@media (min-width: 768px) {
  .form-inline .form-group {
    display: inline-block;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .form-inline .form-control {
    display: inline-block;
    width: auto;
    vertical-align: middle;
  }
  .form-inline .form-control-static {
    display: inline-block;
  }
  .form-inline .input-group {
    display: inline-table;
    vertical-align: middle;
  }
  .form-inline .input-group .input-group-addon,
  .form-inline .input-group .input-group-btn,
  .form-inline .input-group .form-control {
    width: auto;
  }
  .form-inline .input-group > .form-control {
    width: 100%;
  }
  .form-inline .control-label {
    margin-bottom: 0;
    vertical-align: middle;
  }
  .form-inline .radio,
  .form-inline .checkbox {
    display: inline-block;
    margin-top: 0;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .form-inline .radio label,
  .form-inline .checkbox label {
    padding-left: 0;
  }
  .form-inline .radio input[type="radio"],
  .form-inline .checkbox input[type="checkbox"] {
    position: relative;
    margin-left: 0;
  }
  .form-inline .has-feedback .form-control-feedback {
    top: 0;
  }
}
.form-horizontal .radio,
.form-horizontal .checkbox,
.form-horizontal .radio-inline,
.form-horizontal .checkbox-inline {
  margin-top: 0;
  margin-bottom: 0;
  padding-top: 7px;
}
.form-horizontal .radio,
.form-horizontal .checkbox {
  min-height: 25px;
}
.form-horizontal .form-group {
  margin-left: 0px;
  margin-right: 0px;
}
@media (min-width: 768px) {
  .form-horizontal .control-label {
    text-align: right;
    margin-bottom: 0;
    padding-top: 7px;
  }
}
.form-horizontal .has-feedback .form-control-feedback {
  right: 0px;
}
@media (min-width: 768px) {
  .form-horizontal .form-group-lg .control-label {
    padding-top: 11px;
    font-size: 17px;
  }
}
@media (min-width: 768px) {
  .form-horizontal .form-group-sm .control-label {
    padding-top: 6px;
    font-size: 12px;
  }
}
.btn {
  display: inline-block;
  margin-bottom: 0;
  font-weight: normal;
  text-align: center;
  vertical-align: middle;
  touch-action: manipulation;
  cursor: pointer;
  background-image: none;
  border: 1px solid transparent;
  white-space: nowrap;
  padding: 6px 12px;
  font-size: 13px;
  line-height: 1.42857143;
  border-radius: 2px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
.btn:focus,
.btn:active:focus,
.btn.active:focus,
.btn.focus,
.btn:active.focus,
.btn.active.focus {
  outline: 5px auto -webkit-focus-ring-color;
  outline-offset: -2px;
}
.btn:hover,
.btn:focus,
.btn.focus {
  color: #333;
  text-decoration: none;
}
.btn:active,
.btn.active {
  outline: 0;
  background-image: none;
  -webkit-box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
  box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
}
.btn.disabled,
.btn[disabled],
fieldset[disabled] .btn {
  cursor: not-allowed;
  opacity: 0.65;
  filter: alpha(opacity=65);
  -webkit-box-shadow: none;
  box-shadow: none;
}
a.btn.disabled,
fieldset[disabled] a.btn {
  pointer-events: none;
}
.btn-default {
  color: #333;
  background-color: #fff;
  border-color: #ccc;
}
.btn-default:focus,
.btn-default.focus {
  color: #333;
  background-color: #e6e6e6;
  border-color: #8c8c8c;
}
.btn-default:hover {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.btn-default:active,
.btn-default.active,
.open > .dropdown-toggle.btn-default {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.btn-default:active:hover,
.btn-default.active:hover,
.open > .dropdown-toggle.btn-default:hover,
.btn-default:active:focus,
.btn-default.active:focus,
.open > .dropdown-toggle.btn-default:focus,
.btn-default:active.focus,
.btn-default.active.focus,
.open > .dropdown-toggle.btn-default.focus {
  color: #333;
  background-color: #d4d4d4;
  border-color: #8c8c8c;
}
.btn-default:active,
.btn-default.active,
.open > .dropdown-toggle.btn-default {
  background-image: none;
}
.btn-default.disabled:hover,
.btn-default[disabled]:hover,
fieldset[disabled] .btn-default:hover,
.btn-default.disabled:focus,
.btn-default[disabled]:focus,
fieldset[disabled] .btn-default:focus,
.btn-default.disabled.focus,
.btn-default[disabled].focus,
fieldset[disabled] .btn-default.focus {
  background-color: #fff;
  border-color: #ccc;
}
.btn-default .badge {
  color: #fff;
  background-color: #333;
}
.btn-primary {
  color: #fff;
  background-color: #337ab7;
  border-color: #2e6da4;
}
.btn-primary:focus,
.btn-primary.focus {
  color: #fff;
  background-color: #286090;
  border-color: #122b40;
}
.btn-primary:hover {
  color: #fff;
  background-color: #286090;
  border-color: #204d74;
}
.btn-primary:active,
.btn-primary.active,
.open > .dropdown-toggle.btn-primary {
  color: #fff;
  background-color: #286090;
  border-color: #204d74;
}
.btn-primary:active:hover,
.btn-primary.active:hover,
.open > .dropdown-toggle.btn-primary:hover,
.btn-primary:active:focus,
.btn-primary.active:focus,
.open > .dropdown-toggle.btn-primary:focus,
.btn-primary:active.focus,
.btn-primary.active.focus,
.open > .dropdown-toggle.btn-primary.focus {
  color: #fff;
  background-color: #204d74;
  border-color: #122b40;
}
.btn-primary:active,
.btn-primary.active,
.open > .dropdown-toggle.btn-primary {
  background-image: none;
}
.btn-primary.disabled:hover,
.btn-primary[disabled]:hover,
fieldset[disabled] .btn-primary:hover,
.btn-primary.disabled:focus,
.btn-primary[disabled]:focus,
fieldset[disabled] .btn-primary:focus,
.btn-primary.disabled.focus,
.btn-primary[disabled].focus,
fieldset[disabled] .btn-primary.focus {
  background-color: #337ab7;
  border-color: #2e6da4;
}
.btn-primary .badge {
  color: #337ab7;
  background-color: #fff;
}
.btn-success {
  color: #fff;
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.btn-success:focus,
.btn-success.focus {
  color: #fff;
  background-color: #449d44;
  border-color: #255625;
}
.btn-success:hover {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.btn-success:active,
.btn-success.active,
.open > .dropdown-toggle.btn-success {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.btn-success:active:hover,
.btn-success.active:hover,
.open > .dropdown-toggle.btn-success:hover,
.btn-success:active:focus,
.btn-success.active:focus,
.open > .dropdown-toggle.btn-success:focus,
.btn-success:active.focus,
.btn-success.active.focus,
.open > .dropdown-toggle.btn-success.focus {
  color: #fff;
  background-color: #398439;
  border-color: #255625;
}
.btn-success:active,
.btn-success.active,
.open > .dropdown-toggle.btn-success {
  background-image: none;
}
.btn-success.disabled:hover,
.btn-success[disabled]:hover,
fieldset[disabled] .btn-success:hover,
.btn-success.disabled:focus,
.btn-success[disabled]:focus,
fieldset[disabled] .btn-success:focus,
.btn-success.disabled.focus,
.btn-success[disabled].focus,
fieldset[disabled] .btn-success.focus {
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.btn-success .badge {
  color: #5cb85c;
  background-color: #fff;
}
.btn-info {
  color: #fff;
  background-color: #5bc0de;
  border-color: #46b8da;
}
.btn-info:focus,
.btn-info.focus {
  color: #fff;
  background-color: #31b0d5;
  border-color: #1b6d85;
}
.btn-info:hover {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.btn-info:active,
.btn-info.active,
.open > .dropdown-toggle.btn-info {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.btn-info:active:hover,
.btn-info.active:hover,
.open > .dropdown-toggle.btn-info:hover,
.btn-info:active:focus,
.btn-info.active:focus,
.open > .dropdown-toggle.btn-info:focus,
.btn-info:active.focus,
.btn-info.active.focus,
.open > .dropdown-toggle.btn-info.focus {
  color: #fff;
  background-color: #269abc;
  border-color: #1b6d85;
}
.btn-info:active,
.btn-info.active,
.open > .dropdown-toggle.btn-info {
  background-image: none;
}
.btn-info.disabled:hover,
.btn-info[disabled]:hover,
fieldset[disabled] .btn-info:hover,
.btn-info.disabled:focus,
.btn-info[disabled]:focus,
fieldset[disabled] .btn-info:focus,
.btn-info.disabled.focus,
.btn-info[disabled].focus,
fieldset[disabled] .btn-info.focus {
  background-color: #5bc0de;
  border-color: #46b8da;
}
.btn-info .badge {
  color: #5bc0de;
  background-color: #fff;
}
.btn-warning {
  color: #fff;
  background-color: #f0ad4e;
  border-color: #eea236;
}
.btn-warning:focus,
.btn-warning.focus {
  color: #fff;
  background-color: #ec971f;
  border-color: #985f0d;
}
.btn-warning:hover {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.btn-warning:active,
.btn-warning.active,
.open > .dropdown-toggle.btn-warning {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.btn-warning:active:hover,
.btn-warning.active:hover,
.open > .dropdown-toggle.btn-warning:hover,
.btn-warning:active:focus,
.btn-warning.active:focus,
.open > .dropdown-toggle.btn-warning:focus,
.btn-warning:active.focus,
.btn-warning.active.focus,
.open > .dropdown-toggle.btn-warning.focus {
  color: #fff;
  background-color: #d58512;
  border-color: #985f0d;
}
.btn-warning:active,
.btn-warning.active,
.open > .dropdown-toggle.btn-warning {
  background-image: none;
}
.btn-warning.disabled:hover,
.btn-warning[disabled]:hover,
fieldset[disabled] .btn-warning:hover,
.btn-warning.disabled:focus,
.btn-warning[disabled]:focus,
fieldset[disabled] .btn-warning:focus,
.btn-warning.disabled.focus,
.btn-warning[disabled].focus,
fieldset[disabled] .btn-warning.focus {
  background-color: #f0ad4e;
  border-color: #eea236;
}
.btn-warning .badge {
  color: #f0ad4e;
  background-color: #fff;
}
.btn-danger {
  color: #fff;
  background-color: #d9534f;
  border-color: #d43f3a;
}
.btn-danger:focus,
.btn-danger.focus {
  color: #fff;
  background-color: #c9302c;
  border-color: #761c19;
}
.btn-danger:hover {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.btn-danger:active,
.btn-danger.active,
.open > .dropdown-toggle.btn-danger {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.btn-danger:active:hover,
.btn-danger.active:hover,
.open > .dropdown-toggle.btn-danger:hover,
.btn-danger:active:focus,
.btn-danger.active:focus,
.open > .dropdown-toggle.btn-danger:focus,
.btn-danger:active.focus,
.btn-danger.active.focus,
.open > .dropdown-toggle.btn-danger.focus {
  color: #fff;
  background-color: #ac2925;
  border-color: #761c19;
}
.btn-danger:active,
.btn-danger.active,
.open > .dropdown-toggle.btn-danger {
  background-image: none;
}
.btn-danger.disabled:hover,
.btn-danger[disabled]:hover,
fieldset[disabled] .btn-danger:hover,
.btn-danger.disabled:focus,
.btn-danger[disabled]:focus,
fieldset[disabled] .btn-danger:focus,
.btn-danger.disabled.focus,
.btn-danger[disabled].focus,
fieldset[disabled] .btn-danger.focus {
  background-color: #d9534f;
  border-color: #d43f3a;
}
.btn-danger .badge {
  color: #d9534f;
  background-color: #fff;
}
.btn-link {
  color: #337ab7;
  font-weight: normal;
  border-radius: 0;
}
.btn-link,
.btn-link:active,
.btn-link.active,
.btn-link[disabled],
fieldset[disabled] .btn-link {
  background-color: transparent;
  -webkit-box-shadow: none;
  box-shadow: none;
}
.btn-link,
.btn-link:hover,
.btn-link:focus,
.btn-link:active {
  border-color: transparent;
}
.btn-link:hover,
.btn-link:focus {
  color: #23527c;
  text-decoration: underline;
  background-color: transparent;
}
.btn-link[disabled]:hover,
fieldset[disabled] .btn-link:hover,
.btn-link[disabled]:focus,
fieldset[disabled] .btn-link:focus {
  color: #777777;
  text-decoration: none;
}
.btn-lg,
.btn-group-lg > .btn {
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
.btn-sm,
.btn-group-sm > .btn {
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
.btn-xs,
.btn-group-xs > .btn {
  padding: 1px 5px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
.btn-block {
  display: block;
  width: 100%;
}
.btn-block + .btn-block {
  margin-top: 5px;
}
input[type="submit"].btn-block,
input[type="reset"].btn-block,
input[type="button"].btn-block {
  width: 100%;
}
.fade {
  opacity: 0;
  -webkit-transition: opacity 0.15s linear;
  -o-transition: opacity 0.15s linear;
  transition: opacity 0.15s linear;
}
.fade.in {
  opacity: 1;
}
.collapse {
  display: none;
}
.collapse.in {
  display: block;
}
tr.collapse.in {
  display: table-row;
}
tbody.collapse.in {
  display: table-row-group;
}
.collapsing {
  position: relative;
  height: 0;
  overflow: hidden;
  -webkit-transition-property: height, visibility;
  transition-property: height, visibility;
  -webkit-transition-duration: 0.35s;
  transition-duration: 0.35s;
  -webkit-transition-timing-function: ease;
  transition-timing-function: ease;
}
.caret {
  display: inline-block;
  width: 0;
  height: 0;
  margin-left: 2px;
  vertical-align: middle;
  border-top: 4px dashed;
  border-top: 4px solid \9;
  border-right: 4px solid transparent;
  border-left: 4px solid transparent;
}
.dropup,
.dropdown {
  position: relative;
}
.dropdown-toggle:focus {
  outline: 0;
}
.dropdown-menu {
  position: absolute;
  top: 100%;
  left: 0;
  z-index: 1000;
  display: none;
  float: left;
  min-width: 160px;
  padding: 5px 0;
  margin: 2px 0 0;
  list-style: none;
  font-size: 13px;
  text-align: left;
  background-color: #fff;
  border: 1px solid #ccc;
  border: 1px solid rgba(0, 0, 0, 0.15);
  border-radius: 2px;
  -webkit-box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175);
  background-clip: padding-box;
}
.dropdown-menu.pull-right {
  right: 0;
  left: auto;
}
.dropdown-menu .divider {
  height: 1px;
  margin: 8px 0;
  overflow: hidden;
  background-color: #e5e5e5;
}
.dropdown-menu > li > a {
  display: block;
  padding: 3px 20px;
  clear: both;
  font-weight: normal;
  line-height: 1.42857143;
  color: #333333;
  white-space: nowrap;
}
.dropdown-menu > li > a:hover,
.dropdown-menu > li > a:focus {
  text-decoration: none;
  color: #262626;
  background-color: #f5f5f5;
}
.dropdown-menu > .active > a,
.dropdown-menu > .active > a:hover,
.dropdown-menu > .active > a:focus {
  color: #fff;
  text-decoration: none;
  outline: 0;
  background-color: #337ab7;
}
.dropdown-menu > .disabled > a,
.dropdown-menu > .disabled > a:hover,
.dropdown-menu > .disabled > a:focus {
  color: #777777;
}
.dropdown-menu > .disabled > a:hover,
.dropdown-menu > .disabled > a:focus {
  text-decoration: none;
  background-color: transparent;
  background-image: none;
  filter: progid:DXImageTransform.Microsoft.gradient(enabled = false);
  cursor: not-allowed;
}
.open > .dropdown-menu {
  display: block;
}
.open > a {
  outline: 0;
}
.dropdown-menu-right {
  left: auto;
  right: 0;
}
.dropdown-menu-left {
  left: 0;
  right: auto;
}
.dropdown-header {
  display: block;
  padding: 3px 20px;
  font-size: 12px;
  line-height: 1.42857143;
  color: #777777;
  white-space: nowrap;
}
.dropdown-backdrop {
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
  z-index: 990;
}
.pull-right > .dropdown-menu {
  right: 0;
  left: auto;
}
.dropup .caret,
.navbar-fixed-bottom .dropdown .caret {
  border-top: 0;
  border-bottom: 4px dashed;
  border-bottom: 4px solid \9;
  content: "";
}
.dropup .dropdown-menu,
.navbar-fixed-bottom .dropdown .dropdown-menu {
  top: auto;
  bottom: 100%;
  margin-bottom: 2px;
}
@media (min-width: 541px) {
  .navbar-right .dropdown-menu {
    left: auto;
    right: 0;
  }
  .navbar-right .dropdown-menu-left {
    left: 0;
    right: auto;
  }
}
.btn-group,
.btn-group-vertical {
  position: relative;
  display: inline-block;
  vertical-align: middle;
}
.btn-group > .btn,
.btn-group-vertical > .btn {
  position: relative;
  float: left;
}
.btn-group > .btn:hover,
.btn-group-vertical > .btn:hover,
.btn-group > .btn:focus,
.btn-group-vertical > .btn:focus,
.btn-group > .btn:active,
.btn-group-vertical > .btn:active,
.btn-group > .btn.active,
.btn-group-vertical > .btn.active {
  z-index: 2;
}
.btn-group .btn + .btn,
.btn-group .btn + .btn-group,
.btn-group .btn-group + .btn,
.btn-group .btn-group + .btn-group {
  margin-left: -1px;
}
.btn-toolbar {
  margin-left: -5px;
}
.btn-toolbar .btn,
.btn-toolbar .btn-group,
.btn-toolbar .input-group {
  float: left;
}
.btn-toolbar > .btn,
.btn-toolbar > .btn-group,
.btn-toolbar > .input-group {
  margin-left: 5px;
}
.btn-group > .btn:not(:first-child):not(:last-child):not(.dropdown-toggle) {
  border-radius: 0;
}
.btn-group > .btn:first-child {
  margin-left: 0;
}
.btn-group > .btn:first-child:not(:last-child):not(.dropdown-toggle) {
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
}
.btn-group > .btn:last-child:not(:first-child),
.btn-group > .dropdown-toggle:not(:first-child) {
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}
.btn-group > .btn-group {
  float: left;
}
.btn-group > .btn-group:not(:first-child):not(:last-child) > .btn {
  border-radius: 0;
}
.btn-group > .btn-group:first-child:not(:last-child) > .btn:last-child,
.btn-group > .btn-group:first-child:not(:last-child) > .dropdown-toggle {
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
}
.btn-group > .btn-group:last-child:not(:first-child) > .btn:first-child {
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}
.btn-group .dropdown-toggle:active,
.btn-group.open .dropdown-toggle {
  outline: 0;
}
.btn-group > .btn + .dropdown-toggle {
  padding-left: 8px;
  padding-right: 8px;
}
.btn-group > .btn-lg + .dropdown-toggle {
  padding-left: 12px;
  padding-right: 12px;
}
.btn-group.open .dropdown-toggle {
  -webkit-box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
  box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
}
.btn-group.open .dropdown-toggle.btn-link {
  -webkit-box-shadow: none;
  box-shadow: none;
}
.btn .caret {
  margin-left: 0;
}
.btn-lg .caret {
  border-width: 5px 5px 0;
  border-bottom-width: 0;
}
.dropup .btn-lg .caret {
  border-width: 0 5px 5px;
}
.btn-group-vertical > .btn,
.btn-group-vertical > .btn-group,
.btn-group-vertical > .btn-group > .btn {
  display: block;
  float: none;
  width: 100%;
  max-width: 100%;
}
.btn-group-vertical > .btn-group > .btn {
  float: none;
}
.btn-group-vertical > .btn + .btn,
.btn-group-vertical > .btn + .btn-group,
.btn-group-vertical > .btn-group + .btn,
.btn-group-vertical > .btn-group + .btn-group {
  margin-top: -1px;
  margin-left: 0;
}
.btn-group-vertical > .btn:not(:first-child):not(:last-child) {
  border-radius: 0;
}
.btn-group-vertical > .btn:first-child:not(:last-child) {
  border-top-right-radius: 2px;
  border-top-left-radius: 2px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.btn-group-vertical > .btn:last-child:not(:first-child) {
  border-top-right-radius: 0;
  border-top-left-radius: 0;
  border-bottom-right-radius: 2px;
  border-bottom-left-radius: 2px;
}
.btn-group-vertical > .btn-group:not(:first-child):not(:last-child) > .btn {
  border-radius: 0;
}
.btn-group-vertical > .btn-group:first-child:not(:last-child) > .btn:last-child,
.btn-group-vertical > .btn-group:first-child:not(:last-child) > .dropdown-toggle {
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.btn-group-vertical > .btn-group:last-child:not(:first-child) > .btn:first-child {
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.btn-group-justified {
  display: table;
  width: 100%;
  table-layout: fixed;
  border-collapse: separate;
}
.btn-group-justified > .btn,
.btn-group-justified > .btn-group {
  float: none;
  display: table-cell;
  width: 1%;
}
.btn-group-justified > .btn-group .btn {
  width: 100%;
}
.btn-group-justified > .btn-group .dropdown-menu {
  left: auto;
}
[data-toggle="buttons"] > .btn input[type="radio"],
[data-toggle="buttons"] > .btn-group > .btn input[type="radio"],
[data-toggle="buttons"] > .btn input[type="checkbox"],
[data-toggle="buttons"] > .btn-group > .btn input[type="checkbox"] {
  position: absolute;
  clip: rect(0, 0, 0, 0);
  pointer-events: none;
}
.input-group {
  position: relative;
  display: table;
  border-collapse: separate;
}
.input-group[class*="col-"] {
  float: none;
  padding-left: 0;
  padding-right: 0;
}
.input-group .form-control {
  position: relative;
  z-index: 2;
  float: left;
  width: 100%;
  margin-bottom: 0;
}
.input-group .form-control:focus {
  z-index: 3;
}
.input-group-lg > .form-control,
.input-group-lg > .input-group-addon,
.input-group-lg > .input-group-btn > .btn {
  height: 45px;
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
select.input-group-lg > .form-control,
select.input-group-lg > .input-group-addon,
select.input-group-lg > .input-group-btn > .btn {
  height: 45px;
  line-height: 45px;
}
textarea.input-group-lg > .form-control,
textarea.input-group-lg > .input-group-addon,
textarea.input-group-lg > .input-group-btn > .btn,
select[multiple].input-group-lg > .form-control,
select[multiple].input-group-lg > .input-group-addon,
select[multiple].input-group-lg > .input-group-btn > .btn {
  height: auto;
}
.input-group-sm > .form-control,
.input-group-sm > .input-group-addon,
.input-group-sm > .input-group-btn > .btn {
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
select.input-group-sm > .form-control,
select.input-group-sm > .input-group-addon,
select.input-group-sm > .input-group-btn > .btn {
  height: 30px;
  line-height: 30px;
}
textarea.input-group-sm > .form-control,
textarea.input-group-sm > .input-group-addon,
textarea.input-group-sm > .input-group-btn > .btn,
select[multiple].input-group-sm > .form-control,
select[multiple].input-group-sm > .input-group-addon,
select[multiple].input-group-sm > .input-group-btn > .btn {
  height: auto;
}
.input-group-addon,
.input-group-btn,
.input-group .form-control {
  display: table-cell;
}
.input-group-addon:not(:first-child):not(:last-child),
.input-group-btn:not(:first-child):not(:last-child),
.input-group .form-control:not(:first-child):not(:last-child) {
  border-radius: 0;
}
.input-group-addon,
.input-group-btn {
  width: 1%;
  white-space: nowrap;
  vertical-align: middle;
}
.input-group-addon {
  padding: 6px 12px;
  font-size: 13px;
  font-weight: normal;
  line-height: 1;
  color: #555555;
  text-align: center;
  background-color: #eeeeee;
  border: 1px solid #ccc;
  border-radius: 2px;
}
.input-group-addon.input-sm {
  padding: 5px 10px;
  font-size: 12px;
  border-radius: 1px;
}
.input-group-addon.input-lg {
  padding: 10px 16px;
  font-size: 17px;
  border-radius: 3px;
}
.input-group-addon input[type="radio"],
.input-group-addon input[type="checkbox"] {
  margin-top: 0;
}
.input-group .form-control:first-child,
.input-group-addon:first-child,
.input-group-btn:first-child > .btn,
.input-group-btn:first-child > .btn-group > .btn,
.input-group-btn:first-child > .dropdown-toggle,
.input-group-btn:last-child > .btn:not(:last-child):not(.dropdown-toggle),
.input-group-btn:last-child > .btn-group:not(:last-child) > .btn {
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
}
.input-group-addon:first-child {
  border-right: 0;
}
.input-group .form-control:last-child,
.input-group-addon:last-child,
.input-group-btn:last-child > .btn,
.input-group-btn:last-child > .btn-group > .btn,
.input-group-btn:last-child > .dropdown-toggle,
.input-group-btn:first-child > .btn:not(:first-child),
.input-group-btn:first-child > .btn-group:not(:first-child) > .btn {
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}
.input-group-addon:last-child {
  border-left: 0;
}
.input-group-btn {
  position: relative;
  font-size: 0;
  white-space: nowrap;
}
.input-group-btn > .btn {
  position: relative;
}
.input-group-btn > .btn + .btn {
  margin-left: -1px;
}
.input-group-btn > .btn:hover,
.input-group-btn > .btn:focus,
.input-group-btn > .btn:active {
  z-index: 2;
}
.input-group-btn:first-child > .btn,
.input-group-btn:first-child > .btn-group {
  margin-right: -1px;
}
.input-group-btn:last-child > .btn,
.input-group-btn:last-child > .btn-group {
  z-index: 2;
  margin-left: -1px;
}
.nav {
  margin-bottom: 0;
  padding-left: 0;
  list-style: none;
}
.nav > li {
  position: relative;
  display: block;
}
.nav > li > a {
  position: relative;
  display: block;
  padding: 10px 15px;
}
.nav > li > a:hover,
.nav > li > a:focus {
  text-decoration: none;
  background-color: #eeeeee;
}
.nav > li.disabled > a {
  color: #777777;
}
.nav > li.disabled > a:hover,
.nav > li.disabled > a:focus {
  color: #777777;
  text-decoration: none;
  background-color: transparent;
  cursor: not-allowed;
}
.nav .open > a,
.nav .open > a:hover,
.nav .open > a:focus {
  background-color: #eeeeee;
  border-color: #337ab7;
}
.nav .nav-divider {
  height: 1px;
  margin: 8px 0;
  overflow: hidden;
  background-color: #e5e5e5;
}
.nav > li > a > img {
  max-width: none;
}
.nav-tabs {
  border-bottom: 1px solid #ddd;
}
.nav-tabs > li {
  float: left;
  margin-bottom: -1px;
}
.nav-tabs > li > a {
  margin-right: 2px;
  line-height: 1.42857143;
  border: 1px solid transparent;
  border-radius: 2px 2px 0 0;
}
.nav-tabs > li > a:hover {
  border-color: #eeeeee #eeeeee #ddd;
}
.nav-tabs > li.active > a,
.nav-tabs > li.active > a:hover,
.nav-tabs > li.active > a:focus {
  color: #555555;
  background-color: #fff;
  border: 1px solid #ddd;
  border-bottom-color: transparent;
  cursor: default;
}
.nav-tabs.nav-justified {
  width: 100%;
  border-bottom: 0;
}
.nav-tabs.nav-justified > li {
  float: none;
}
.nav-tabs.nav-justified > li > a {
  text-align: center;
  margin-bottom: 5px;
}
.nav-tabs.nav-justified > .dropdown .dropdown-menu {
  top: auto;
  left: auto;
}
@media (min-width: 768px) {
  .nav-tabs.nav-justified > li {
    display: table-cell;
    width: 1%;
  }
  .nav-tabs.nav-justified > li > a {
    margin-bottom: 0;
  }
}
.nav-tabs.nav-justified > li > a {
  margin-right: 0;
  border-radius: 2px;
}
.nav-tabs.nav-justified > .active > a,
.nav-tabs.nav-justified > .active > a:hover,
.nav-tabs.nav-justified > .active > a:focus {
  border: 1px solid #ddd;
}
@media (min-width: 768px) {
  .nav-tabs.nav-justified > li > a {
    border-bottom: 1px solid #ddd;
    border-radius: 2px 2px 0 0;
  }
  .nav-tabs.nav-justified > .active > a,
  .nav-tabs.nav-justified > .active > a:hover,
  .nav-tabs.nav-justified > .active > a:focus {
    border-bottom-color: #fff;
  }
}
.nav-pills > li {
  float: left;
}
.nav-pills > li > a {
  border-radius: 2px;
}
.nav-pills > li + li {
  margin-left: 2px;
}
.nav-pills > li.active > a,
.nav-pills > li.active > a:hover,
.nav-pills > li.active > a:focus {
  color: #fff;
  background-color: #337ab7;
}
.nav-stacked > li {
  float: none;
}
.nav-stacked > li + li {
  margin-top: 2px;
  margin-left: 0;
}
.nav-justified {
  width: 100%;
}
.nav-justified > li {
  float: none;
}
.nav-justified > li > a {
  text-align: center;
  margin-bottom: 5px;
}
.nav-justified > .dropdown .dropdown-menu {
  top: auto;
  left: auto;
}
@media (min-width: 768px) {
  .nav-justified > li {
    display: table-cell;
    width: 1%;
  }
  .nav-justified > li > a {
    margin-bottom: 0;
  }
}
.nav-tabs-justified {
  border-bottom: 0;
}
.nav-tabs-justified > li > a {
  margin-right: 0;
  border-radius: 2px;
}
.nav-tabs-justified > .active > a,
.nav-tabs-justified > .active > a:hover,
.nav-tabs-justified > .active > a:focus {
  border: 1px solid #ddd;
}
@media (min-width: 768px) {
  .nav-tabs-justified > li > a {
    border-bottom: 1px solid #ddd;
    border-radius: 2px 2px 0 0;
  }
  .nav-tabs-justified > .active > a,
  .nav-tabs-justified > .active > a:hover,
  .nav-tabs-justified > .active > a:focus {
    border-bottom-color: #fff;
  }
}
.tab-content > .tab-pane {
  display: none;
}
.tab-content > .active {
  display: block;
}
.nav-tabs .dropdown-menu {
  margin-top: -1px;
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.navbar {
  position: relative;
  min-height: 30px;
  margin-bottom: 18px;
  border: 1px solid transparent;
}
@media (min-width: 541px) {
  .navbar {
    border-radius: 2px;
  }
}
@media (min-width: 541px) {
  .navbar-header {
    float: left;
  }
}
.navbar-collapse {
  overflow-x: visible;
  padding-right: 0px;
  padding-left: 0px;
  border-top: 1px solid transparent;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.1);
  -webkit-overflow-scrolling: touch;
}
.navbar-collapse.in {
  overflow-y: auto;
}
@media (min-width: 541px) {
  .navbar-collapse {
    width: auto;
    border-top: 0;
    box-shadow: none;
  }
  .navbar-collapse.collapse {
    display: block !important;
    height: auto !important;
    padding-bottom: 0;
    overflow: visible !important;
  }
  .navbar-collapse.in {
    overflow-y: visible;
  }
  .navbar-fixed-top .navbar-collapse,
  .navbar-static-top .navbar-collapse,
  .navbar-fixed-bottom .navbar-collapse {
    padding-left: 0;
    padding-right: 0;
  }
}
.navbar-fixed-top .navbar-collapse,
.navbar-fixed-bottom .navbar-collapse {
  max-height: 340px;
}
@media (max-device-width: 540px) and (orientation: landscape) {
  .navbar-fixed-top .navbar-collapse,
  .navbar-fixed-bottom .navbar-collapse {
    max-height: 200px;
  }
}
.container > .navbar-header,
.container-fluid > .navbar-header,
.container > .navbar-collapse,
.container-fluid > .navbar-collapse {
  margin-right: 0px;
  margin-left: 0px;
}
@media (min-width: 541px) {
  .container > .navbar-header,
  .container-fluid > .navbar-header,
  .container > .navbar-collapse,
  .container-fluid > .navbar-collapse {
    margin-right: 0;
    margin-left: 0;
  }
}
.navbar-static-top {
  z-index: 1000;
  border-width: 0 0 1px;
}
@media (min-width: 541px) {
  .navbar-static-top {
    border-radius: 0;
  }
}
.navbar-fixed-top,
.navbar-fixed-bottom {
  position: fixed;
  right: 0;
  left: 0;
  z-index: 1030;
}
@media (min-width: 541px) {
  .navbar-fixed-top,
  .navbar-fixed-bottom {
    border-radius: 0;
  }
}
.navbar-fixed-top {
  top: 0;
  border-width: 0 0 1px;
}
.navbar-fixed-bottom {
  bottom: 0;
  margin-bottom: 0;
  border-width: 1px 0 0;
}
.navbar-brand {
  float: left;
  padding: 6px 0px;
  font-size: 17px;
  line-height: 18px;
  height: 30px;
}
.navbar-brand:hover,
.navbar-brand:focus {
  text-decoration: none;
}
.navbar-brand > img {
  display: block;
}
@media (min-width: 541px) {
  .navbar > .container .navbar-brand,
  .navbar > .container-fluid .navbar-brand {
    margin-left: 0px;
  }
}
.navbar-toggle {
  position: relative;
  float: right;
  margin-right: 0px;
  padding: 9px 10px;
  margin-top: -2px;
  margin-bottom: -2px;
  background-color: transparent;
  background-image: none;
  border: 1px solid transparent;
  border-radius: 2px;
}
.navbar-toggle:focus {
  outline: 0;
}
.navbar-toggle .icon-bar {
  display: block;
  width: 22px;
  height: 2px;
  border-radius: 1px;
}
.navbar-toggle .icon-bar + .icon-bar {
  margin-top: 4px;
}
@media (min-width: 541px) {
  .navbar-toggle {
    display: none;
  }
}
.navbar-nav {
  margin: 3px 0px;
}
.navbar-nav > li > a {
  padding-top: 10px;
  padding-bottom: 10px;
  line-height: 18px;
}
@media (max-width: 540px) {
  .navbar-nav .open .dropdown-menu {
    position: static;
    float: none;
    width: auto;
    margin-top: 0;
    background-color: transparent;
    border: 0;
    box-shadow: none;
  }
  .navbar-nav .open .dropdown-menu > li > a,
  .navbar-nav .open .dropdown-menu .dropdown-header {
    padding: 5px 15px 5px 25px;
  }
  .navbar-nav .open .dropdown-menu > li > a {
    line-height: 18px;
  }
  .navbar-nav .open .dropdown-menu > li > a:hover,
  .navbar-nav .open .dropdown-menu > li > a:focus {
    background-image: none;
  }
}
@media (min-width: 541px) {
  .navbar-nav {
    float: left;
    margin: 0;
  }
  .navbar-nav > li {
    float: left;
  }
  .navbar-nav > li > a {
    padding-top: 6px;
    padding-bottom: 6px;
  }
}
.navbar-form {
  margin-left: 0px;
  margin-right: 0px;
  padding: 10px 0px;
  border-top: 1px solid transparent;
  border-bottom: 1px solid transparent;
  -webkit-box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.1), 0 1px 0 rgba(255, 255, 255, 0.1);
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.1), 0 1px 0 rgba(255, 255, 255, 0.1);
  margin-top: -1px;
  margin-bottom: -1px;
}
@media (min-width: 768px) {
  .navbar-form .form-group {
    display: inline-block;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .navbar-form .form-control {
    display: inline-block;
    width: auto;
    vertical-align: middle;
  }
  .navbar-form .form-control-static {
    display: inline-block;
  }
  .navbar-form .input-group {
    display: inline-table;
    vertical-align: middle;
  }
  .navbar-form .input-group .input-group-addon,
  .navbar-form .input-group .input-group-btn,
  .navbar-form .input-group .form-control {
    width: auto;
  }
  .navbar-form .input-group > .form-control {
    width: 100%;
  }
  .navbar-form .control-label {
    margin-bottom: 0;
    vertical-align: middle;
  }
  .navbar-form .radio,
  .navbar-form .checkbox {
    display: inline-block;
    margin-top: 0;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .navbar-form .radio label,
  .navbar-form .checkbox label {
    padding-left: 0;
  }
  .navbar-form .radio input[type="radio"],
  .navbar-form .checkbox input[type="checkbox"] {
    position: relative;
    margin-left: 0;
  }
  .navbar-form .has-feedback .form-control-feedback {
    top: 0;
  }
}
@media (max-width: 540px) {
  .navbar-form .form-group {
    margin-bottom: 5px;
  }
  .navbar-form .form-group:last-child {
    margin-bottom: 0;
  }
}
@media (min-width: 541px) {
  .navbar-form {
    width: auto;
    border: 0;
    margin-left: 0;
    margin-right: 0;
    padding-top: 0;
    padding-bottom: 0;
    -webkit-box-shadow: none;
    box-shadow: none;
  }
}
.navbar-nav > li > .dropdown-menu {
  margin-top: 0;
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.navbar-fixed-bottom .navbar-nav > li > .dropdown-menu {
  margin-bottom: 0;
  border-top-right-radius: 2px;
  border-top-left-radius: 2px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.navbar-btn {
  margin-top: -1px;
  margin-bottom: -1px;
}
.navbar-btn.btn-sm {
  margin-top: 0px;
  margin-bottom: 0px;
}
.navbar-btn.btn-xs {
  margin-top: 4px;
  margin-bottom: 4px;
}
.navbar-text {
  margin-top: 6px;
  margin-bottom: 6px;
}
@media (min-width: 541px) {
  .navbar-text {
    float: left;
    margin-left: 0px;
    margin-right: 0px;
  }
}
@media (min-width: 541px) {
  .navbar-left {
    float: left !important;
    float: left;
  }
  .navbar-right {
    float: right !important;
    float: right;
    margin-right: 0px;
  }
  .navbar-right ~ .navbar-right {
    margin-right: 0;
  }
}
.navbar-default {
  background-color: #f8f8f8;
  border-color: #e7e7e7;
}
.navbar-default .navbar-brand {
  color: #777;
}
.navbar-default .navbar-brand:hover,
.navbar-default .navbar-brand:focus {
  color: #5e5e5e;
  background-color: transparent;
}
.navbar-default .navbar-text {
  color: #777;
}
.navbar-default .navbar-nav > li > a {
  color: #777;
}
.navbar-default .navbar-nav > li > a:hover,
.navbar-default .navbar-nav > li > a:focus {
  color: #333;
  background-color: transparent;
}
.navbar-default .navbar-nav > .active > a,
.navbar-default .navbar-nav > .active > a:hover,
.navbar-default .navbar-nav > .active > a:focus {
  color: #555;
  background-color: #e7e7e7;
}
.navbar-default .navbar-nav > .disabled > a,
.navbar-default .navbar-nav > .disabled > a:hover,
.navbar-default .navbar-nav > .disabled > a:focus {
  color: #ccc;
  background-color: transparent;
}
.navbar-default .navbar-toggle {
  border-color: #ddd;
}
.navbar-default .navbar-toggle:hover,
.navbar-default .navbar-toggle:focus {
  background-color: #ddd;
}
.navbar-default .navbar-toggle .icon-bar {
  background-color: #888;
}
.navbar-default .navbar-collapse,
.navbar-default .navbar-form {
  border-color: #e7e7e7;
}
.navbar-default .navbar-nav > .open > a,
.navbar-default .navbar-nav > .open > a:hover,
.navbar-default .navbar-nav > .open > a:focus {
  background-color: #e7e7e7;
  color: #555;
}
@media (max-width: 540px) {
  .navbar-default .navbar-nav .open .dropdown-menu > li > a {
    color: #777;
  }
  .navbar-default .navbar-nav .open .dropdown-menu > li > a:hover,
  .navbar-default .navbar-nav .open .dropdown-menu > li > a:focus {
    color: #333;
    background-color: transparent;
  }
  .navbar-default .navbar-nav .open .dropdown-menu > .active > a,
  .navbar-default .navbar-nav .open .dropdown-menu > .active > a:hover,
  .navbar-default .navbar-nav .open .dropdown-menu > .active > a:focus {
    color: #555;
    background-color: #e7e7e7;
  }
  .navbar-default .navbar-nav .open .dropdown-menu > .disabled > a,
  .navbar-default .navbar-nav .open .dropdown-menu > .disabled > a:hover,
  .navbar-default .navbar-nav .open .dropdown-menu > .disabled > a:focus {
    color: #ccc;
    background-color: transparent;
  }
}
.navbar-default .navbar-link {
  color: #777;
}
.navbar-default .navbar-link:hover {
  color: #333;
}
.navbar-default .btn-link {
  color: #777;
}
.navbar-default .btn-link:hover,
.navbar-default .btn-link:focus {
  color: #333;
}
.navbar-default .btn-link[disabled]:hover,
fieldset[disabled] .navbar-default .btn-link:hover,
.navbar-default .btn-link[disabled]:focus,
fieldset[disabled] .navbar-default .btn-link:focus {
  color: #ccc;
}
.navbar-inverse {
  background-color: #222;
  border-color: #080808;
}
.navbar-inverse .navbar-brand {
  color: #9d9d9d;
}
.navbar-inverse .navbar-brand:hover,
.navbar-inverse .navbar-brand:focus {
  color: #fff;
  background-color: transparent;
}
.navbar-inverse .navbar-text {
  color: #9d9d9d;
}
.navbar-inverse .navbar-nav > li > a {
  color: #9d9d9d;
}
.navbar-inverse .navbar-nav > li > a:hover,
.navbar-inverse .navbar-nav > li > a:focus {
  color: #fff;
  background-color: transparent;
}
.navbar-inverse .navbar-nav > .active > a,
.navbar-inverse .navbar-nav > .active > a:hover,
.navbar-inverse .navbar-nav > .active > a:focus {
  color: #fff;
  background-color: #080808;
}
.navbar-inverse .navbar-nav > .disabled > a,
.navbar-inverse .navbar-nav > .disabled > a:hover,
.navbar-inverse .navbar-nav > .disabled > a:focus {
  color: #444;
  background-color: transparent;
}
.navbar-inverse .navbar-toggle {
  border-color: #333;
}
.navbar-inverse .navbar-toggle:hover,
.navbar-inverse .navbar-toggle:focus {
  background-color: #333;
}
.navbar-inverse .navbar-toggle .icon-bar {
  background-color: #fff;
}
.navbar-inverse .navbar-collapse,
.navbar-inverse .navbar-form {
  border-color: #101010;
}
.navbar-inverse .navbar-nav > .open > a,
.navbar-inverse .navbar-nav > .open > a:hover,
.navbar-inverse .navbar-nav > .open > a:focus {
  background-color: #080808;
  color: #fff;
}
@media (max-width: 540px) {
  .navbar-inverse .navbar-nav .open .dropdown-menu > .dropdown-header {
    border-color: #080808;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu .divider {
    background-color: #080808;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > li > a {
    color: #9d9d9d;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > li > a:hover,
  .navbar-inverse .navbar-nav .open .dropdown-menu > li > a:focus {
    color: #fff;
    background-color: transparent;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > .active > a,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .active > a:hover,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .active > a:focus {
    color: #fff;
    background-color: #080808;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > .disabled > a,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .disabled > a:hover,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .disabled > a:focus {
    color: #444;
    background-color: transparent;
  }
}
.navbar-inverse .navbar-link {
  color: #9d9d9d;
}
.navbar-inverse .navbar-link:hover {
  color: #fff;
}
.navbar-inverse .btn-link {
  color: #9d9d9d;
}
.navbar-inverse .btn-link:hover,
.navbar-inverse .btn-link:focus {
  color: #fff;
}
.navbar-inverse .btn-link[disabled]:hover,
fieldset[disabled] .navbar-inverse .btn-link:hover,
.navbar-inverse .btn-link[disabled]:focus,
fieldset[disabled] .navbar-inverse .btn-link:focus {
  color: #444;
}
.breadcrumb {
  padding: 8px 15px;
  margin-bottom: 18px;
  list-style: none;
  background-color: #f5f5f5;
  border-radius: 2px;
}
.breadcrumb > li {
  display: inline-block;
}
.breadcrumb > li + li:before {
  content: "/\00a0";
  padding: 0 5px;
  color: #5e5e5e;
}
.breadcrumb > .active {
  color: #777777;
}
.pagination {
  display: inline-block;
  padding-left: 0;
  margin: 18px 0;
  border-radius: 2px;
}
.pagination > li {
  display: inline;
}
.pagination > li > a,
.pagination > li > span {
  position: relative;
  float: left;
  padding: 6px 12px;
  line-height: 1.42857143;
  text-decoration: none;
  color: #337ab7;
  background-color: #fff;
  border: 1px solid #ddd;
  margin-left: -1px;
}
.pagination > li:first-child > a,
.pagination > li:first-child > span {
  margin-left: 0;
  border-bottom-left-radius: 2px;
  border-top-left-radius: 2px;
}
.pagination > li:last-child > a,
.pagination > li:last-child > span {
  border-bottom-right-radius: 2px;
  border-top-right-radius: 2px;
}
.pagination > li > a:hover,
.pagination > li > span:hover,
.pagination > li > a:focus,
.pagination > li > span:focus {
  z-index: 2;
  color: #23527c;
  background-color: #eeeeee;
  border-color: #ddd;
}
.pagination > .active > a,
.pagination > .active > span,
.pagination > .active > a:hover,
.pagination > .active > span:hover,
.pagination > .active > a:focus,
.pagination > .active > span:focus {
  z-index: 3;
  color: #fff;
  background-color: #337ab7;
  border-color: #337ab7;
  cursor: default;
}
.pagination > .disabled > span,
.pagination > .disabled > span:hover,
.pagination > .disabled > span:focus,
.pagination > .disabled > a,
.pagination > .disabled > a:hover,
.pagination > .disabled > a:focus {
  color: #777777;
  background-color: #fff;
  border-color: #ddd;
  cursor: not-allowed;
}
.pagination-lg > li > a,
.pagination-lg > li > span {
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
}
.pagination-lg > li:first-child > a,
.pagination-lg > li:first-child > span {
  border-bottom-left-radius: 3px;
  border-top-left-radius: 3px;
}
.pagination-lg > li:last-child > a,
.pagination-lg > li:last-child > span {
  border-bottom-right-radius: 3px;
  border-top-right-radius: 3px;
}
.pagination-sm > li > a,
.pagination-sm > li > span {
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
}
.pagination-sm > li:first-child > a,
.pagination-sm > li:first-child > span {
  border-bottom-left-radius: 1px;
  border-top-left-radius: 1px;
}
.pagination-sm > li:last-child > a,
.pagination-sm > li:last-child > span {
  border-bottom-right-radius: 1px;
  border-top-right-radius: 1px;
}
.pager {
  padding-left: 0;
  margin: 18px 0;
  list-style: none;
  text-align: center;
}
.pager li {
  display: inline;
}
.pager li > a,
.pager li > span {
  display: inline-block;
  padding: 5px 14px;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 15px;
}
.pager li > a:hover,
.pager li > a:focus {
  text-decoration: none;
  background-color: #eeeeee;
}
.pager .next > a,
.pager .next > span {
  float: right;
}
.pager .previous > a,
.pager .previous > span {
  float: left;
}
.pager .disabled > a,
.pager .disabled > a:hover,
.pager .disabled > a:focus,
.pager .disabled > span {
  color: #777777;
  background-color: #fff;
  cursor: not-allowed;
}
.label {
  display: inline;
  padding: .2em .6em .3em;
  font-size: 75%;
  font-weight: bold;
  line-height: 1;
  color: #fff;
  text-align: center;
  white-space: nowrap;
  vertical-align: baseline;
  border-radius: .25em;
}
a.label:hover,
a.label:focus {
  color: #fff;
  text-decoration: none;
  cursor: pointer;
}
.label:empty {
  display: none;
}
.btn .label {
  position: relative;
  top: -1px;
}
.label-default {
  background-color: #777777;
}
.label-default[href]:hover,
.label-default[href]:focus {
  background-color: #5e5e5e;
}
.label-primary {
  background-color: #337ab7;
}
.label-primary[href]:hover,
.label-primary[href]:focus {
  background-color: #286090;
}
.label-success {
  background-color: #5cb85c;
}
.label-success[href]:hover,
.label-success[href]:focus {
  background-color: #449d44;
}
.label-info {
  background-color: #5bc0de;
}
.label-info[href]:hover,
.label-info[href]:focus {
  background-color: #31b0d5;
}
.label-warning {
  background-color: #f0ad4e;
}
.label-warning[href]:hover,
.label-warning[href]:focus {
  background-color: #ec971f;
}
.label-danger {
  background-color: #d9534f;
}
.label-danger[href]:hover,
.label-danger[href]:focus {
  background-color: #c9302c;
}
.badge {
  display: inline-block;
  min-width: 10px;
  padding: 3px 7px;
  font-size: 12px;
  font-weight: bold;
  color: #fff;
  line-height: 1;
  vertical-align: middle;
  white-space: nowrap;
  text-align: center;
  background-color: #777777;
  border-radius: 10px;
}
.badge:empty {
  display: none;
}
.btn .badge {
  position: relative;
  top: -1px;
}
.btn-xs .badge,
.btn-group-xs > .btn .badge {
  top: 0;
  padding: 1px 5px;
}
a.badge:hover,
a.badge:focus {
  color: #fff;
  text-decoration: none;
  cursor: pointer;
}
.list-group-item.active > .badge,
.nav-pills > .active > a > .badge {
  color: #337ab7;
  background-color: #fff;
}
.list-group-item > .badge {
  float: right;
}
.list-group-item > .badge + .badge {
  margin-right: 5px;
}
.nav-pills > li > a > .badge {
  margin-left: 3px;
}
.jumbotron {
  padding-top: 30px;
  padding-bottom: 30px;
  margin-bottom: 30px;
  color: inherit;
  background-color: #eeeeee;
}
.jumbotron h1,
.jumbotron .h1 {
  color: inherit;
}
.jumbotron p {
  margin-bottom: 15px;
  font-size: 20px;
  font-weight: 200;
}
.jumbotron > hr {
  border-top-color: #d5d5d5;
}
.container .jumbotron,
.container-fluid .jumbotron {
  border-radius: 3px;
  padding-left: 0px;
  padding-right: 0px;
}
.jumbotron .container {
  max-width: 100%;
}
@media screen and (min-width: 768px) {
  .jumbotron {
    padding-top: 48px;
    padding-bottom: 48px;
  }
  .container .jumbotron,
  .container-fluid .jumbotron {
    padding-left: 60px;
    padding-right: 60px;
  }
  .jumbotron h1,
  .jumbotron .h1 {
    font-size: 59px;
  }
}
.thumbnail {
  display: block;
  padding: 4px;
  margin-bottom: 18px;
  line-height: 1.42857143;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 2px;
  -webkit-transition: border 0.2s ease-in-out;
  -o-transition: border 0.2s ease-in-out;
  transition: border 0.2s ease-in-out;
}
.thumbnail > img,
.thumbnail a > img {
  margin-left: auto;
  margin-right: auto;
}
a.thumbnail:hover,
a.thumbnail:focus,
a.thumbnail.active {
  border-color: #337ab7;
}
.thumbnail .caption {
  padding: 9px;
  color: #000;
}
.alert {
  padding: 15px;
  margin-bottom: 18px;
  border: 1px solid transparent;
  border-radius: 2px;
}
.alert h4 {
  margin-top: 0;
  color: inherit;
}
.alert .alert-link {
  font-weight: bold;
}
.alert > p,
.alert > ul {
  margin-bottom: 0;
}
.alert > p + p {
  margin-top: 5px;
}
.alert-dismissable,
.alert-dismissible {
  padding-right: 35px;
}
.alert-dismissable .close,
.alert-dismissible .close {
  position: relative;
  top: -2px;
  right: -21px;
  color: inherit;
}
.alert-success {
  background-color: #dff0d8;
  border-color: #d6e9c6;
  color: #3c763d;
}
.alert-success hr {
  border-top-color: #c9e2b3;
}
.alert-success .alert-link {
  color: #2b542c;
}
.alert-info {
  background-color: #d9edf7;
  border-color: #bce8f1;
  color: #31708f;
}
.alert-info hr {
  border-top-color: #a6e1ec;
}
.alert-info .alert-link {
  color: #245269;
}
.alert-warning {
  background-color: #fcf8e3;
  border-color: #faebcc;
  color: #8a6d3b;
}
.alert-warning hr {
  border-top-color: #f7e1b5;
}
.alert-warning .alert-link {
  color: #66512c;
}
.alert-danger {
  background-color: #f2dede;
  border-color: #ebccd1;
  color: #a94442;
}
.alert-danger hr {
  border-top-color: #e4b9c0;
}
.alert-danger .alert-link {
  color: #843534;
}
@-webkit-keyframes progress-bar-stripes {
  from {
    background-position: 40px 0;
  }
  to {
    background-position: 0 0;
  }
}
@keyframes progress-bar-stripes {
  from {
    background-position: 40px 0;
  }
  to {
    background-position: 0 0;
  }
}
.progress {
  overflow: hidden;
  height: 18px;
  margin-bottom: 18px;
  background-color: #f5f5f5;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.1);
  box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.1);
}
.progress-bar {
  float: left;
  width: 0%;
  height: 100%;
  font-size: 12px;
  line-height: 18px;
  color: #fff;
  text-align: center;
  background-color: #337ab7;
  -webkit-box-shadow: inset 0 -1px 0 rgba(0, 0, 0, 0.15);
  box-shadow: inset 0 -1px 0 rgba(0, 0, 0, 0.15);
  -webkit-transition: width 0.6s ease;
  -o-transition: width 0.6s ease;
  transition: width 0.6s ease;
}
.progress-striped .progress-bar,
.progress-bar-striped {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-size: 40px 40px;
}
.progress.active .progress-bar,
.progress-bar.active {
  -webkit-animation: progress-bar-stripes 2s linear infinite;
  -o-animation: progress-bar-stripes 2s linear infinite;
  animation: progress-bar-stripes 2s linear infinite;
}
.progress-bar-success {
  background-color: #5cb85c;
}
.progress-striped .progress-bar-success {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.progress-bar-info {
  background-color: #5bc0de;
}
.progress-striped .progress-bar-info {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.progress-bar-warning {
  background-color: #f0ad4e;
}
.progress-striped .progress-bar-warning {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.progress-bar-danger {
  background-color: #d9534f;
}
.progress-striped .progress-bar-danger {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.media {
  margin-top: 15px;
}
.media:first-child {
  margin-top: 0;
}
.media,
.media-body {
  zoom: 1;
  overflow: hidden;
}
.media-body {
  width: 10000px;
}
.media-object {
  display: block;
}
.media-object.img-thumbnail {
  max-width: none;
}
.media-right,
.media > .pull-right {
  padding-left: 10px;
}
.media-left,
.media > .pull-left {
  padding-right: 10px;
}
.media-left,
.media-right,
.media-body {
  display: table-cell;
  vertical-align: top;
}
.media-middle {
  vertical-align: middle;
}
.media-bottom {
  vertical-align: bottom;
}
.media-heading {
  margin-top: 0;
  margin-bottom: 5px;
}
.media-list {
  padding-left: 0;
  list-style: none;
}
.list-group {
  margin-bottom: 20px;
  padding-left: 0;
}
.list-group-item {
  position: relative;
  display: block;
  padding: 10px 15px;
  margin-bottom: -1px;
  background-color: #fff;
  border: 1px solid #ddd;
}
.list-group-item:first-child {
  border-top-right-radius: 2px;
  border-top-left-radius: 2px;
}
.list-group-item:last-child {
  margin-bottom: 0;
  border-bottom-right-radius: 2px;
  border-bottom-left-radius: 2px;
}
a.list-group-item,
button.list-group-item {
  color: #555;
}
a.list-group-item .list-group-item-heading,
button.list-group-item .list-group-item-heading {
  color: #333;
}
a.list-group-item:hover,
button.list-group-item:hover,
a.list-group-item:focus,
button.list-group-item:focus {
  text-decoration: none;
  color: #555;
  background-color: #f5f5f5;
}
button.list-group-item {
  width: 100%;
  text-align: left;
}
.list-group-item.disabled,
.list-group-item.disabled:hover,
.list-group-item.disabled:focus {
  background-color: #eeeeee;
  color: #777777;
  cursor: not-allowed;
}
.list-group-item.disabled .list-group-item-heading,
.list-group-item.disabled:hover .list-group-item-heading,
.list-group-item.disabled:focus .list-group-item-heading {
  color: inherit;
}
.list-group-item.disabled .list-group-item-text,
.list-group-item.disabled:hover .list-group-item-text,
.list-group-item.disabled:focus .list-group-item-text {
  color: #777777;
}
.list-group-item.active,
.list-group-item.active:hover,
.list-group-item.active:focus {
  z-index: 2;
  color: #fff;
  background-color: #337ab7;
  border-color: #337ab7;
}
.list-group-item.active .list-group-item-heading,
.list-group-item.active:hover .list-group-item-heading,
.list-group-item.active:focus .list-group-item-heading,
.list-group-item.active .list-group-item-heading > small,
.list-group-item.active:hover .list-group-item-heading > small,
.list-group-item.active:focus .list-group-item-heading > small,
.list-group-item.active .list-group-item-heading > .small,
.list-group-item.active:hover .list-group-item-heading > .small,
.list-group-item.active:focus .list-group-item-heading > .small {
  color: inherit;
}
.list-group-item.active .list-group-item-text,
.list-group-item.active:hover .list-group-item-text,
.list-group-item.active:focus .list-group-item-text {
  color: #c7ddef;
}
.list-group-item-success {
  color: #3c763d;
  background-color: #dff0d8;
}
a.list-group-item-success,
button.list-group-item-success {
  color: #3c763d;
}
a.list-group-item-success .list-group-item-heading,
button.list-group-item-success .list-group-item-heading {
  color: inherit;
}
a.list-group-item-success:hover,
button.list-group-item-success:hover,
a.list-group-item-success:focus,
button.list-group-item-success:focus {
  color: #3c763d;
  background-color: #d0e9c6;
}
a.list-group-item-success.active,
button.list-group-item-success.active,
a.list-group-item-success.active:hover,
button.list-group-item-success.active:hover,
a.list-group-item-success.active:focus,
button.list-group-item-success.active:focus {
  color: #fff;
  background-color: #3c763d;
  border-color: #3c763d;
}
.list-group-item-info {
  color: #31708f;
  background-color: #d9edf7;
}
a.list-group-item-info,
button.list-group-item-info {
  color: #31708f;
}
a.list-group-item-info .list-group-item-heading,
button.list-group-item-info .list-group-item-heading {
  color: inherit;
}
a.list-group-item-info:hover,
button.list-group-item-info:hover,
a.list-group-item-info:focus,
button.list-group-item-info:focus {
  color: #31708f;
  background-color: #c4e3f3;
}
a.list-group-item-info.active,
button.list-group-item-info.active,
a.list-group-item-info.active:hover,
button.list-group-item-info.active:hover,
a.list-group-item-info.active:focus,
button.list-group-item-info.active:focus {
  color: #fff;
  background-color: #31708f;
  border-color: #31708f;
}
.list-group-item-warning {
  color: #8a6d3b;
  background-color: #fcf8e3;
}
a.list-group-item-warning,
button.list-group-item-warning {
  color: #8a6d3b;
}
a.list-group-item-warning .list-group-item-heading,
button.list-group-item-warning .list-group-item-heading {
  color: inherit;
}
a.list-group-item-warning:hover,
button.list-group-item-warning:hover,
a.list-group-item-warning:focus,
button.list-group-item-warning:focus {
  color: #8a6d3b;
  background-color: #faf2cc;
}
a.list-group-item-warning.active,
button.list-group-item-warning.active,
a.list-group-item-warning.active:hover,
button.list-group-item-warning.active:hover,
a.list-group-item-warning.active:focus,
button.list-group-item-warning.active:focus {
  color: #fff;
  background-color: #8a6d3b;
  border-color: #8a6d3b;
}
.list-group-item-danger {
  color: #a94442;
  background-color: #f2dede;
}
a.list-group-item-danger,
button.list-group-item-danger {
  color: #a94442;
}
a.list-group-item-danger .list-group-item-heading,
button.list-group-item-danger .list-group-item-heading {
  color: inherit;
}
a.list-group-item-danger:hover,
button.list-group-item-danger:hover,
a.list-group-item-danger:focus,
button.list-group-item-danger:focus {
  color: #a94442;
  background-color: #ebcccc;
}
a.list-group-item-danger.active,
button.list-group-item-danger.active,
a.list-group-item-danger.active:hover,
button.list-group-item-danger.active:hover,
a.list-group-item-danger.active:focus,
button.list-group-item-danger.active:focus {
  color: #fff;
  background-color: #a94442;
  border-color: #a94442;
}
.list-group-item-heading {
  margin-top: 0;
  margin-bottom: 5px;
}
.list-group-item-text {
  margin-bottom: 0;
  line-height: 1.3;
}
.panel {
  margin-bottom: 18px;
  background-color: #fff;
  border: 1px solid transparent;
  border-radius: 2px;
  -webkit-box-shadow: 0 1px 1px rgba(0, 0, 0, 0.05);
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.05);
}
.panel-body {
  padding: 15px;
}
.panel-heading {
  padding: 10px 15px;
  border-bottom: 1px solid transparent;
  border-top-right-radius: 1px;
  border-top-left-radius: 1px;
}
.panel-heading > .dropdown .dropdown-toggle {
  color: inherit;
}
.panel-title {
  margin-top: 0;
  margin-bottom: 0;
  font-size: 15px;
  color: inherit;
}
.panel-title > a,
.panel-title > small,
.panel-title > .small,
.panel-title > small > a,
.panel-title > .small > a {
  color: inherit;
}
.panel-footer {
  padding: 10px 15px;
  background-color: #f5f5f5;
  border-top: 1px solid #ddd;
  border-bottom-right-radius: 1px;
  border-bottom-left-radius: 1px;
}
.panel > .list-group,
.panel > .panel-collapse > .list-group {
  margin-bottom: 0;
}
.panel > .list-group .list-group-item,
.panel > .panel-collapse > .list-group .list-group-item {
  border-width: 1px 0;
  border-radius: 0;
}
.panel > .list-group:first-child .list-group-item:first-child,
.panel > .panel-collapse > .list-group:first-child .list-group-item:first-child {
  border-top: 0;
  border-top-right-radius: 1px;
  border-top-left-radius: 1px;
}
.panel > .list-group:last-child .list-group-item:last-child,
.panel > .panel-collapse > .list-group:last-child .list-group-item:last-child {
  border-bottom: 0;
  border-bottom-right-radius: 1px;
  border-bottom-left-radius: 1px;
}
.panel > .panel-heading + .panel-collapse > .list-group .list-group-item:first-child {
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.panel-heading + .list-group .list-group-item:first-child {
  border-top-width: 0;
}
.list-group + .panel-footer {
  border-top-width: 0;
}
.panel > .table,
.panel > .table-responsive > .table,
.panel > .panel-collapse > .table {
  margin-bottom: 0;
}
.panel > .table caption,
.panel > .table-responsive > .table caption,
.panel > .panel-collapse > .table caption {
  padding-left: 15px;
  padding-right: 15px;
}
.panel > .table:first-child,
.panel > .table-responsive:first-child > .table:first-child {
  border-top-right-radius: 1px;
  border-top-left-radius: 1px;
}
.panel > .table:first-child > thead:first-child > tr:first-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child,
.panel > .table:first-child > tbody:first-child > tr:first-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child {
  border-top-left-radius: 1px;
  border-top-right-radius: 1px;
}
.panel > .table:first-child > thead:first-child > tr:first-child td:first-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child td:first-child,
.panel > .table:first-child > tbody:first-child > tr:first-child td:first-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child td:first-child,
.panel > .table:first-child > thead:first-child > tr:first-child th:first-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child th:first-child,
.panel > .table:first-child > tbody:first-child > tr:first-child th:first-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child th:first-child {
  border-top-left-radius: 1px;
}
.panel > .table:first-child > thead:first-child > tr:first-child td:last-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child td:last-child,
.panel > .table:first-child > tbody:first-child > tr:first-child td:last-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child td:last-child,
.panel > .table:first-child > thead:first-child > tr:first-child th:last-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child th:last-child,
.panel > .table:first-child > tbody:first-child > tr:first-child th:last-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child th:last-child {
  border-top-right-radius: 1px;
}
.panel > .table:last-child,
.panel > .table-responsive:last-child > .table:last-child {
  border-bottom-right-radius: 1px;
  border-bottom-left-radius: 1px;
}
.panel > .table:last-child > tbody:last-child > tr:last-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child {
  border-bottom-left-radius: 1px;
  border-bottom-right-radius: 1px;
}
.panel > .table:last-child > tbody:last-child > tr:last-child td:first-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child td:first-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child td:first-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child td:first-child,
.panel > .table:last-child > tbody:last-child > tr:last-child th:first-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child th:first-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child th:first-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child th:first-child {
  border-bottom-left-radius: 1px;
}
.panel > .table:last-child > tbody:last-child > tr:last-child td:last-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child td:last-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child td:last-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child td:last-child,
.panel > .table:last-child > tbody:last-child > tr:last-child th:last-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child th:last-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child th:last-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child th:last-child {
  border-bottom-right-radius: 1px;
}
.panel > .panel-body + .table,
.panel > .panel-body + .table-responsive,
.panel > .table + .panel-body,
.panel > .table-responsive + .panel-body {
  border-top: 1px solid #ddd;
}
.panel > .table > tbody:first-child > tr:first-child th,
.panel > .table > tbody:first-child > tr:first-child td {
  border-top: 0;
}
.panel > .table-bordered,
.panel > .table-responsive > .table-bordered {
  border: 0;
}
.panel > .table-bordered > thead > tr > th:first-child,
.panel > .table-responsive > .table-bordered > thead > tr > th:first-child,
.panel > .table-bordered > tbody > tr > th:first-child,
.panel > .table-responsive > .table-bordered > tbody > tr > th:first-child,
.panel > .table-bordered > tfoot > tr > th:first-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > th:first-child,
.panel > .table-bordered > thead > tr > td:first-child,
.panel > .table-responsive > .table-bordered > thead > tr > td:first-child,
.panel > .table-bordered > tbody > tr > td:first-child,
.panel > .table-responsive > .table-bordered > tbody > tr > td:first-child,
.panel > .table-bordered > tfoot > tr > td:first-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > td:first-child {
  border-left: 0;
}
.panel > .table-bordered > thead > tr > th:last-child,
.panel > .table-responsive > .table-bordered > thead > tr > th:last-child,
.panel > .table-bordered > tbody > tr > th:last-child,
.panel > .table-responsive > .table-bordered > tbody > tr > th:last-child,
.panel > .table-bordered > tfoot > tr > th:last-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > th:last-child,
.panel > .table-bordered > thead > tr > td:last-child,
.panel > .table-responsive > .table-bordered > thead > tr > td:last-child,
.panel > .table-bordered > tbody > tr > td:last-child,
.panel > .table-responsive > .table-bordered > tbody > tr > td:last-child,
.panel > .table-bordered > tfoot > tr > td:last-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > td:last-child {
  border-right: 0;
}
.panel > .table-bordered > thead > tr:first-child > td,
.panel > .table-responsive > .table-bordered > thead > tr:first-child > td,
.panel > .table-bordered > tbody > tr:first-child > td,
.panel > .table-responsive > .table-bordered > tbody > tr:first-child > td,
.panel > .table-bordered > thead > tr:first-child > th,
.panel > .table-responsive > .table-bordered > thead > tr:first-child > th,
.panel > .table-bordered > tbody > tr:first-child > th,
.panel > .table-responsive > .table-bordered > tbody > tr:first-child > th {
  border-bottom: 0;
}
.panel > .table-bordered > tbody > tr:last-child > td,
.panel > .table-responsive > .table-bordered > tbody > tr:last-child > td,
.panel > .table-bordered > tfoot > tr:last-child > td,
.panel > .table-responsive > .table-bordered > tfoot > tr:last-child > td,
.panel > .table-bordered > tbody > tr:last-child > th,
.panel > .table-responsive > .table-bordered > tbody > tr:last-child > th,
.panel > .table-bordered > tfoot > tr:last-child > th,
.panel > .table-responsive > .table-bordered > tfoot > tr:last-child > th {
  border-bottom: 0;
}
.panel > .table-responsive {
  border: 0;
  margin-bottom: 0;
}
.panel-group {
  margin-bottom: 18px;
}
.panel-group .panel {
  margin-bottom: 0;
  border-radius: 2px;
}
.panel-group .panel + .panel {
  margin-top: 5px;
}
.panel-group .panel-heading {
  border-bottom: 0;
}
.panel-group .panel-heading + .panel-collapse > .panel-body,
.panel-group .panel-heading + .panel-collapse > .list-group {
  border-top: 1px solid #ddd;
}
.panel-group .panel-footer {
  border-top: 0;
}
.panel-group .panel-footer + .panel-collapse .panel-body {
  border-bottom: 1px solid #ddd;
}
.panel-default {
  border-color: #ddd;
}
.panel-default > .panel-heading {
  color: #333333;
  background-color: #f5f5f5;
  border-color: #ddd;
}
.panel-default > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #ddd;
}
.panel-default > .panel-heading .badge {
  color: #f5f5f5;
  background-color: #333333;
}
.panel-default > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #ddd;
}
.panel-primary {
  border-color: #337ab7;
}
.panel-primary > .panel-heading {
  color: #fff;
  background-color: #337ab7;
  border-color: #337ab7;
}
.panel-primary > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #337ab7;
}
.panel-primary > .panel-heading .badge {
  color: #337ab7;
  background-color: #fff;
}
.panel-primary > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #337ab7;
}
.panel-success {
  border-color: #d6e9c6;
}
.panel-success > .panel-heading {
  color: #3c763d;
  background-color: #dff0d8;
  border-color: #d6e9c6;
}
.panel-success > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #d6e9c6;
}
.panel-success > .panel-heading .badge {
  color: #dff0d8;
  background-color: #3c763d;
}
.panel-success > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #d6e9c6;
}
.panel-info {
  border-color: #bce8f1;
}
.panel-info > .panel-heading {
  color: #31708f;
  background-color: #d9edf7;
  border-color: #bce8f1;
}
.panel-info > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #bce8f1;
}
.panel-info > .panel-heading .badge {
  color: #d9edf7;
  background-color: #31708f;
}
.panel-info > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #bce8f1;
}
.panel-warning {
  border-color: #faebcc;
}
.panel-warning > .panel-heading {
  color: #8a6d3b;
  background-color: #fcf8e3;
  border-color: #faebcc;
}
.panel-warning > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #faebcc;
}
.panel-warning > .panel-heading .badge {
  color: #fcf8e3;
  background-color: #8a6d3b;
}
.panel-warning > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #faebcc;
}
.panel-danger {
  border-color: #ebccd1;
}
.panel-danger > .panel-heading {
  color: #a94442;
  background-color: #f2dede;
  border-color: #ebccd1;
}
.panel-danger > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #ebccd1;
}
.panel-danger > .panel-heading .badge {
  color: #f2dede;
  background-color: #a94442;
}
.panel-danger > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #ebccd1;
}
.embed-responsive {
  position: relative;
  display: block;
  height: 0;
  padding: 0;
  overflow: hidden;
}
.embed-responsive .embed-responsive-item,
.embed-responsive iframe,
.embed-responsive embed,
.embed-responsive object,
.embed-responsive video {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  height: 100%;
  width: 100%;
  border: 0;
}
.embed-responsive-16by9 {
  padding-bottom: 56.25%;
}
.embed-responsive-4by3 {
  padding-bottom: 75%;
}
.well {
  min-height: 20px;
  padding: 19px;
  margin-bottom: 20px;
  background-color: #f5f5f5;
  border: 1px solid #e3e3e3;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.05);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.05);
}
.well blockquote {
  border-color: #ddd;
  border-color: rgba(0, 0, 0, 0.15);
}
.well-lg {
  padding: 24px;
  border-radius: 3px;
}
.well-sm {
  padding: 9px;
  border-radius: 1px;
}
.close {
  float: right;
  font-size: 19.5px;
  font-weight: bold;
  line-height: 1;
  color: #000;
  text-shadow: 0 1px 0 #fff;
  opacity: 0.2;
  filter: alpha(opacity=20);
}
.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
  cursor: pointer;
  opacity: 0.5;
  filter: alpha(opacity=50);
}
button.close {
  padding: 0;
  cursor: pointer;
  background: transparent;
  border: 0;
  -webkit-appearance: none;
}
.modal-open {
  overflow: hidden;
}
.modal {
  display: none;
  overflow: hidden;
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 1050;
  -webkit-overflow-scrolling: touch;
  outline: 0;
}
.modal.fade .modal-dialog {
  -webkit-transform: translate(0, -25%);
  -ms-transform: translate(0, -25%);
  -o-transform: translate(0, -25%);
  transform: translate(0, -25%);
  -webkit-transition: -webkit-transform 0.3s ease-out;
  -moz-transition: -moz-transform 0.3s ease-out;
  -o-transition: -o-transform 0.3s ease-out;
  transition: transform 0.3s ease-out;
}
.modal.in .modal-dialog {
  -webkit-transform: translate(0, 0);
  -ms-transform: translate(0, 0);
  -o-transform: translate(0, 0);
  transform: translate(0, 0);
}
.modal-open .modal {
  overflow-x: hidden;
  overflow-y: auto;
}
.modal-dialog {
  position: relative;
  width: auto;
  margin: 10px;
}
.modal-content {
  position: relative;
  background-color: #fff;
  border: 1px solid #999;
  border: 1px solid rgba(0, 0, 0, 0.2);
  border-radius: 3px;
  -webkit-box-shadow: 0 3px 9px rgba(0, 0, 0, 0.5);
  box-shadow: 0 3px 9px rgba(0, 0, 0, 0.5);
  background-clip: padding-box;
  outline: 0;
}
.modal-backdrop {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 1040;
  background-color: #000;
}
.modal-backdrop.fade {
  opacity: 0;
  filter: alpha(opacity=0);
}
.modal-backdrop.in {
  opacity: 0.5;
  filter: alpha(opacity=50);
}
.modal-header {
  padding: 15px;
  border-bottom: 1px solid #e5e5e5;
}
.modal-header .close {
  margin-top: -2px;
}
.modal-title {
  margin: 0;
  line-height: 1.42857143;
}
.modal-body {
  position: relative;
  padding: 15px;
}
.modal-footer {
  padding: 15px;
  text-align: right;
  border-top: 1px solid #e5e5e5;
}
.modal-footer .btn + .btn {
  margin-left: 5px;
  margin-bottom: 0;
}
.modal-footer .btn-group .btn + .btn {
  margin-left: -1px;
}
.modal-footer .btn-block + .btn-block {
  margin-left: 0;
}
.modal-scrollbar-measure {
  position: absolute;
  top: -9999px;
  width: 50px;
  height: 50px;
  overflow: scroll;
}
@media (min-width: 768px) {
  .modal-dialog {
    width: 600px;
    margin: 30px auto;
  }
  .modal-content {
    -webkit-box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
  }
  .modal-sm {
    width: 300px;
  }
}
@media (min-width: 992px) {
  .modal-lg {
    width: 900px;
  }
}
.tooltip {
  position: absolute;
  z-index: 1070;
  display: block;
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-style: normal;
  font-weight: normal;
  letter-spacing: normal;
  line-break: auto;
  line-height: 1.42857143;
  text-align: left;
  text-align: start;
  text-decoration: none;
  text-shadow: none;
  text-transform: none;
  white-space: normal;
  word-break: normal;
  word-spacing: normal;
  word-wrap: normal;
  font-size: 12px;
  opacity: 0;
  filter: alpha(opacity=0);
}
.tooltip.in {
  opacity: 0.9;
  filter: alpha(opacity=90);
}
.tooltip.top {
  margin-top: -3px;
  padding: 5px 0;
}
.tooltip.right {
  margin-left: 3px;
  padding: 0 5px;
}
.tooltip.bottom {
  margin-top: 3px;
  padding: 5px 0;
}
.tooltip.left {
  margin-left: -3px;
  padding: 0 5px;
}
.tooltip-inner {
  max-width: 200px;
  padding: 3px 8px;
  color: #fff;
  text-align: center;
  background-color: #000;
  border-radius: 2px;
}
.tooltip-arrow {
  position: absolute;
  width: 0;
  height: 0;
  border-color: transparent;
  border-style: solid;
}
.tooltip.top .tooltip-arrow {
  bottom: 0;
  left: 50%;
  margin-left: -5px;
  border-width: 5px 5px 0;
  border-top-color: #000;
}
.tooltip.top-left .tooltip-arrow {
  bottom: 0;
  right: 5px;
  margin-bottom: -5px;
  border-width: 5px 5px 0;
  border-top-color: #000;
}
.tooltip.top-right .tooltip-arrow {
  bottom: 0;
  left: 5px;
  margin-bottom: -5px;
  border-width: 5px 5px 0;
  border-top-color: #000;
}
.tooltip.right .tooltip-arrow {
  top: 50%;
  left: 0;
  margin-top: -5px;
  border-width: 5px 5px 5px 0;
  border-right-color: #000;
}
.tooltip.left .tooltip-arrow {
  top: 50%;
  right: 0;
  margin-top: -5px;
  border-width: 5px 0 5px 5px;
  border-left-color: #000;
}
.tooltip.bottom .tooltip-arrow {
  top: 0;
  left: 50%;
  margin-left: -5px;
  border-width: 0 5px 5px;
  border-bottom-color: #000;
}
.tooltip.bottom-left .tooltip-arrow {
  top: 0;
  right: 5px;
  margin-top: -5px;
  border-width: 0 5px 5px;
  border-bottom-color: #000;
}
.tooltip.bottom-right .tooltip-arrow {
  top: 0;
  left: 5px;
  margin-top: -5px;
  border-width: 0 5px 5px;
  border-bottom-color: #000;
}
.popover {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 1060;
  display: none;
  max-width: 276px;
  padding: 1px;
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-style: normal;
  font-weight: normal;
  letter-spacing: normal;
  line-break: auto;
  line-height: 1.42857143;
  text-align: left;
  text-align: start;
  text-decoration: none;
  text-shadow: none;
  text-transform: none;
  white-space: normal;
  word-break: normal;
  word-spacing: normal;
  word-wrap: normal;
  font-size: 13px;
  background-color: #fff;
  background-clip: padding-box;
  border: 1px solid #ccc;
  border: 1px solid rgba(0, 0, 0, 0.2);
  border-radius: 3px;
  -webkit-box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
}
.popover.top {
  margin-top: -10px;
}
.popover.right {
  margin-left: 10px;
}
.popover.bottom {
  margin-top: 10px;
}
.popover.left {
  margin-left: -10px;
}
.popover-title {
  margin: 0;
  padding: 8px 14px;
  font-size: 13px;
  background-color: #f7f7f7;
  border-bottom: 1px solid #ebebeb;
  border-radius: 2px 2px 0 0;
}
.popover-content {
  padding: 9px 14px;
}
.popover > .arrow,
.popover > .arrow:after {
  position: absolute;
  display: block;
  width: 0;
  height: 0;
  border-color: transparent;
  border-style: solid;
}
.popover > .arrow {
  border-width: 11px;
}
.popover > .arrow:after {
  border-width: 10px;
  content: "";
}
.popover.top > .arrow {
  left: 50%;
  margin-left: -11px;
  border-bottom-width: 0;
  border-top-color: #999999;
  border-top-color: rgba(0, 0, 0, 0.25);
  bottom: -11px;
}
.popover.top > .arrow:after {
  content: " ";
  bottom: 1px;
  margin-left: -10px;
  border-bottom-width: 0;
  border-top-color: #fff;
}
.popover.right > .arrow {
  top: 50%;
  left: -11px;
  margin-top: -11px;
  border-left-width: 0;
  border-right-color: #999999;
  border-right-color: rgba(0, 0, 0, 0.25);
}
.popover.right > .arrow:after {
  content: " ";
  left: 1px;
  bottom: -10px;
  border-left-width: 0;
  border-right-color: #fff;
}
.popover.bottom > .arrow {
  left: 50%;
  margin-left: -11px;
  border-top-width: 0;
  border-bottom-color: #999999;
  border-bottom-color: rgba(0, 0, 0, 0.25);
  top: -11px;
}
.popover.bottom > .arrow:after {
  content: " ";
  top: 1px;
  margin-left: -10px;
  border-top-width: 0;
  border-bottom-color: #fff;
}
.popover.left > .arrow {
  top: 50%;
  right: -11px;
  margin-top: -11px;
  border-right-width: 0;
  border-left-color: #999999;
  border-left-color: rgba(0, 0, 0, 0.25);
}
.popover.left > .arrow:after {
  content: " ";
  right: 1px;
  border-right-width: 0;
  border-left-color: #fff;
  bottom: -10px;
}
.carousel {
  position: relative;
}
.carousel-inner {
  position: relative;
  overflow: hidden;
  width: 100%;
}
.carousel-inner > .item {
  display: none;
  position: relative;
  -webkit-transition: 0.6s ease-in-out left;
  -o-transition: 0.6s ease-in-out left;
  transition: 0.6s ease-in-out left;
}
.carousel-inner > .item > img,
.carousel-inner > .item > a > img {
  line-height: 1;
}
@media all and (transform-3d), (-webkit-transform-3d) {
  .carousel-inner > .item {
    -webkit-transition: -webkit-transform 0.6s ease-in-out;
    -moz-transition: -moz-transform 0.6s ease-in-out;
    -o-transition: -o-transform 0.6s ease-in-out;
    transition: transform 0.6s ease-in-out;
    -webkit-backface-visibility: hidden;
    -moz-backface-visibility: hidden;
    backface-visibility: hidden;
    -webkit-perspective: 1000px;
    -moz-perspective: 1000px;
    perspective: 1000px;
  }
  .carousel-inner > .item.next,
  .carousel-inner > .item.active.right {
    -webkit-transform: translate3d(100%, 0, 0);
    transform: translate3d(100%, 0, 0);
    left: 0;
  }
  .carousel-inner > .item.prev,
  .carousel-inner > .item.active.left {
    -webkit-transform: translate3d(-100%, 0, 0);
    transform: translate3d(-100%, 0, 0);
    left: 0;
  }
  .carousel-inner > .item.next.left,
  .carousel-inner > .item.prev.right,
  .carousel-inner > .item.active {
    -webkit-transform: translate3d(0, 0, 0);
    transform: translate3d(0, 0, 0);
    left: 0;
  }
}
.carousel-inner > .active,
.carousel-inner > .next,
.carousel-inner > .prev {
  display: block;
}
.carousel-inner > .active {
  left: 0;
}
.carousel-inner > .next,
.carousel-inner > .prev {
  position: absolute;
  top: 0;
  width: 100%;
}
.carousel-inner > .next {
  left: 100%;
}
.carousel-inner > .prev {
  left: -100%;
}
.carousel-inner > .next.left,
.carousel-inner > .prev.right {
  left: 0;
}
.carousel-inner > .active.left {
  left: -100%;
}
.carousel-inner > .active.right {
  left: 100%;
}
.carousel-control {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  width: 15%;
  opacity: 0.5;
  filter: alpha(opacity=50);
  font-size: 20px;
  color: #fff;
  text-align: center;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.6);
  background-color: rgba(0, 0, 0, 0);
}
.carousel-control.left {
  background-image: -webkit-linear-gradient(left, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.0001) 100%);
  background-image: -o-linear-gradient(left, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.0001) 100%);
  background-image: linear-gradient(to right, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.0001) 100%);
  background-repeat: repeat-x;
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#80000000', endColorstr='#00000000', GradientType=1);
}
.carousel-control.right {
  left: auto;
  right: 0;
  background-image: -webkit-linear-gradient(left, rgba(0, 0, 0, 0.0001) 0%, rgba(0, 0, 0, 0.5) 100%);
  background-image: -o-linear-gradient(left, rgba(0, 0, 0, 0.0001) 0%, rgba(0, 0, 0, 0.5) 100%);
  background-image: linear-gradient(to right, rgba(0, 0, 0, 0.0001) 0%, rgba(0, 0, 0, 0.5) 100%);
  background-repeat: repeat-x;
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#00000000', endColorstr='#80000000', GradientType=1);
}
.carousel-control:hover,
.carousel-control:focus {
  outline: 0;
  color: #fff;
  text-decoration: none;
  opacity: 0.9;
  filter: alpha(opacity=90);
}
.carousel-control .icon-prev,
.carousel-control .icon-next,
.carousel-control .glyphicon-chevron-left,
.carousel-control .glyphicon-chevron-right {
  position: absolute;
  top: 50%;
  margin-top: -10px;
  z-index: 5;
  display: inline-block;
}
.carousel-control .icon-prev,
.carousel-control .glyphicon-chevron-left {
  left: 50%;
  margin-left: -10px;
}
.carousel-control .icon-next,
.carousel-control .glyphicon-chevron-right {
  right: 50%;
  margin-right: -10px;
}
.carousel-control .icon-prev,
.carousel-control .icon-next {
  width: 20px;
  height: 20px;
  line-height: 1;
  font-family: serif;
}
.carousel-control .icon-prev:before {
  content: '\2039';
}
.carousel-control .icon-next:before {
  content: '\203a';
}
.carousel-indicators {
  position: absolute;
  bottom: 10px;
  left: 50%;
  z-index: 15;
  width: 60%;
  margin-left: -30%;
  padding-left: 0;
  list-style: none;
  text-align: center;
}
.carousel-indicators li {
  display: inline-block;
  width: 10px;
  height: 10px;
  margin: 1px;
  text-indent: -999px;
  border: 1px solid #fff;
  border-radius: 10px;
  cursor: pointer;
  background-color: #000 \9;
  background-color: rgba(0, 0, 0, 0);
}
.carousel-indicators .active {
  margin: 0;
  width: 12px;
  height: 12px;
  background-color: #fff;
}
.carousel-caption {
  position: absolute;
  left: 15%;
  right: 15%;
  bottom: 20px;
  z-index: 10;
  padding-top: 20px;
  padding-bottom: 20px;
  color: #fff;
  text-align: center;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.6);
}
.carousel-caption .btn {
  text-shadow: none;
}
@media screen and (min-width: 768px) {
  .carousel-control .glyphicon-chevron-left,
  .carousel-control .glyphicon-chevron-right,
  .carousel-control .icon-prev,
  .carousel-control .icon-next {
    width: 30px;
    height: 30px;
    margin-top: -10px;
    font-size: 30px;
  }
  .carousel-control .glyphicon-chevron-left,
  .carousel-control .icon-prev {
    margin-left: -10px;
  }
  .carousel-control .glyphicon-chevron-right,
  .carousel-control .icon-next {
    margin-right: -10px;
  }
  .carousel-caption {
    left: 20%;
    right: 20%;
    padding-bottom: 30px;
  }
  .carousel-indicators {
    bottom: 20px;
  }
}
.clearfix:before,
.clearfix:after,
.dl-horizontal dd:before,
.dl-horizontal dd:after,
.container:before,
.container:after,
.container-fluid:before,
.container-fluid:after,
.row:before,
.row:after,
.form-horizontal .form-group:before,
.form-horizontal .form-group:after,
.btn-toolbar:before,
.btn-toolbar:after,
.btn-group-vertical > .btn-group:before,
.btn-group-vertical > .btn-group:after,
.nav:before,
.nav:after,
.navbar:before,
.navbar:after,
.navbar-header:before,
.navbar-header:after,
.navbar-collapse:before,
.navbar-collapse:after,
.pager:before,
.pager:after,
.panel-body:before,
.panel-body:after,
.modal-header:before,
.modal-header:after,
.modal-footer:before,
.modal-footer:after,
.item_buttons:before,
.item_buttons:after {
  content: " ";
  display: table;
}
.clearfix:after,
.dl-horizontal dd:after,
.container:after,
.container-fluid:after,
.row:after,
.form-horizontal .form-group:after,
.btn-toolbar:after,
.btn-group-vertical > .btn-group:after,
.nav:after,
.navbar:after,
.navbar-header:after,
.navbar-collapse:after,
.pager:after,
.panel-body:after,
.modal-header:after,
.modal-footer:after,
.item_buttons:after {
  clear: both;
}
.center-block {
  display: block;
  margin-left: auto;
  margin-right: auto;
}
.pull-right {
  float: right !important;
}
.pull-left {
  float: left !important;
}
.hide {
  display: none !important;
}
.show {
  display: block !important;
}
.invisible {
  visibility: hidden;
}
.text-hide {
  font: 0/0 a;
  color: transparent;
  text-shadow: none;
  background-color: transparent;
  border: 0;
}
.hidden {
  display: none !important;
}
.affix {
  position: fixed;
}
@-ms-viewport {
  width: device-width;
}
.visible-xs,
.visible-sm,
.visible-md,
.visible-lg {
  display: none !important;
}
.visible-xs-block,
.visible-xs-inline,
.visible-xs-inline-block,
.visible-sm-block,
.visible-sm-inline,
.visible-sm-inline-block,
.visible-md-block,
.visible-md-inline,
.visible-md-inline-block,
.visible-lg-block,
.visible-lg-inline,
.visible-lg-inline-block {
  display: none !important;
}
@media (max-width: 767px) {
  .visible-xs {
    display: block !important;
  }
  table.visible-xs {
    display: table !important;
  }
  tr.visible-xs {
    display: table-row !important;
  }
  th.visible-xs,
  td.visible-xs {
    display: table-cell !important;
  }
}
@media (max-width: 767px) {
  .visible-xs-block {
    display: block !important;
  }
}
@media (max-width: 767px) {
  .visible-xs-inline {
    display: inline !important;
  }
}
@media (max-width: 767px) {
  .visible-xs-inline-block {
    display: inline-block !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm {
    display: block !important;
  }
  table.visible-sm {
    display: table !important;
  }
  tr.visible-sm {
    display: table-row !important;
  }
  th.visible-sm,
  td.visible-sm {
    display: table-cell !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm-block {
    display: block !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm-inline {
    display: inline !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm-inline-block {
    display: inline-block !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md {
    display: block !important;
  }
  table.visible-md {
    display: table !important;
  }
  tr.visible-md {
    display: table-row !important;
  }
  th.visible-md,
  td.visible-md {
    display: table-cell !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md-block {
    display: block !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md-inline {
    display: inline !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md-inline-block {
    display: inline-block !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg {
    display: block !important;
  }
  table.visible-lg {
    display: table !important;
  }
  tr.visible-lg {
    display: table-row !important;
  }
  th.visible-lg,
  td.visible-lg {
    display: table-cell !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg-block {
    display: block !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg-inline {
    display: inline !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg-inline-block {
    display: inline-block !important;
  }
}
@media (max-width: 767px) {
  .hidden-xs {
    display: none !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .hidden-sm {
    display: none !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .hidden-md {
    display: none !important;
  }
}
@media (min-width: 1200px) {
  .hidden-lg {
    display: none !important;
  }
}
.visible-print {
  display: none !important;
}
@media print {
  .visible-print {
    display: block !important;
  }
  table.visible-print {
    display: table !important;
  }
  tr.visible-print {
    display: table-row !important;
  }
  th.visible-print,
  td.visible-print {
    display: table-cell !important;
  }
}
.visible-print-block {
  display: none !important;
}
@media print {
  .visible-print-block {
    display: block !important;
  }
}
.visible-print-inline {
  display: none !important;
}
@media print {
  .visible-print-inline {
    display: inline !important;
  }
}
.visible-print-inline-block {
  display: none !important;
}
@media print {
  .visible-print-inline-block {
    display: inline-block !important;
  }
}
@media print {
  .hidden-print {
    display: none !important;
  }
}
/*!
*
* Font Awesome
*
*/
/*!
 *  Font Awesome 4.7.0 by @davegandy - http://fontawesome.io - @fontawesome
 *  License - http://fontawesome.io/license (Font: SIL OFL 1.1, CSS: MIT License)
 */
/* FONT PATH
 * -------------------------- */
@font-face {
  font-family: 'FontAwesome';
  src: url('../components/font-awesome/fonts/fontawesome-webfont.eot?v=4.7.0');
  src: url('../components/font-awesome/fonts/fontawesome-webfont.eot?#iefix&v=4.7.0') format('embedded-opentype'), url('../components/font-awesome/fonts/fontawesome-webfont.woff2?v=4.7.0') format('woff2'), url('../components/font-awesome/fonts/fontawesome-webfont.woff?v=4.7.0') format('woff'), url('../components/font-awesome/fonts/fontawesome-webfont.ttf?v=4.7.0') format('truetype'), url('../components/font-awesome/fonts/fontawesome-webfont.svg?v=4.7.0#fontawesomeregular') format('svg');
  font-weight: normal;
  font-style: normal;
}
.fa {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
/* makes the font 33% larger relative to the icon container */
.fa-lg {
  font-size: 1.33333333em;
  line-height: 0.75em;
  vertical-align: -15%;
}
.fa-2x {
  font-size: 2em;
}
.fa-3x {
  font-size: 3em;
}
.fa-4x {
  font-size: 4em;
}
.fa-5x {
  font-size: 5em;
}
.fa-fw {
  width: 1.28571429em;
  text-align: center;
}
.fa-ul {
  padding-left: 0;
  margin-left: 2.14285714em;
  list-style-type: none;
}
.fa-ul > li {
  position: relative;
}
.fa-li {
  position: absolute;
  left: -2.14285714em;
  width: 2.14285714em;
  top: 0.14285714em;
  text-align: center;
}
.fa-li.fa-lg {
  left: -1.85714286em;
}
.fa-border {
  padding: .2em .25em .15em;
  border: solid 0.08em #eee;
  border-radius: .1em;
}
.fa-pull-left {
  float: left;
}
.fa-pull-right {
  float: right;
}
.fa.fa-pull-left {
  margin-right: .3em;
}
.fa.fa-pull-right {
  margin-left: .3em;
}
/* Deprecated as of 4.4.0 */
.pull-right {
  float: right;
}
.pull-left {
  float: left;
}
.fa.pull-left {
  margin-right: .3em;
}
.fa.pull-right {
  margin-left: .3em;
}
.fa-spin {
  -webkit-animation: fa-spin 2s infinite linear;
  animation: fa-spin 2s infinite linear;
}
.fa-pulse {
  -webkit-animation: fa-spin 1s infinite steps(8);
  animation: fa-spin 1s infinite steps(8);
}
@-webkit-keyframes fa-spin {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(359deg);
    transform: rotate(359deg);
  }
}
@keyframes fa-spin {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(359deg);
    transform: rotate(359deg);
  }
}
.fa-rotate-90 {
  -ms-filter: "progid:DXImageTransform.Microsoft.BasicImage(rotation=1)";
  -webkit-transform: rotate(90deg);
  -ms-transform: rotate(90deg);
  transform: rotate(90deg);
}
.fa-rotate-180 {
  -ms-filter: "progid:DXImageTransform.Microsoft.BasicImage(rotation=2)";
  -webkit-transform: rotate(180deg);
  -ms-transform: rotate(180deg);
  transform: rotate(180deg);
}
.fa-rotate-270 {
  -ms-filter: "progid:DXImageTransform.Microsoft.BasicImage(rotation=3)";
  -webkit-transform: rotate(270deg);
  -ms-transform: rotate(270deg);
  transform: rotate(270deg);
}
.fa-flip-horizontal {
  -ms-filter: "progid:DXImageTransform.Microsoft.BasicImage(rotation=0, mirror=1)";
  -webkit-transform: scale(-1, 1);
  -ms-transform: scale(-1, 1);
  transform: scale(-1, 1);
}
.fa-flip-vertical {
  -ms-filter: "progid:DXImageTransform.Microsoft.BasicImage(rotation=2, mirror=1)";
  -webkit-transform: scale(1, -1);
  -ms-transform: scale(1, -1);
  transform: scale(1, -1);
}
:root .fa-rotate-90,
:root .fa-rotate-180,
:root .fa-rotate-270,
:root .fa-flip-horizontal,
:root .fa-flip-vertical {
  filter: none;
}
.fa-stack {
  position: relative;
  display: inline-block;
  width: 2em;
  height: 2em;
  line-height: 2em;
  vertical-align: middle;
}
.fa-stack-1x,
.fa-stack-2x {
  position: absolute;
  left: 0;
  width: 100%;
  text-align: center;
}
.fa-stack-1x {
  line-height: inherit;
}
.fa-stack-2x {
  font-size: 2em;
}
.fa-inverse {
  color: #fff;
}
/* Font Awesome uses the Unicode Private Use Area (PUA) to ensure screen
   readers do not read off random characters that represent icons */
.fa-glass:before {
  content: "\f000";
}
.fa-music:before {
  content: "\f001";
}
.fa-search:before {
  content: "\f002";
}
.fa-envelope-o:before {
  content: "\f003";
}
.fa-heart:before {
  content: "\f004";
}
.fa-star:before {
  content: "\f005";
}
.fa-star-o:before {
  content: "\f006";
}
.fa-user:before {
  content: "\f007";
}
.fa-film:before {
  content: "\f008";
}
.fa-th-large:before {
  content: "\f009";
}
.fa-th:before {
  content: "\f00a";
}
.fa-th-list:before {
  content: "\f00b";
}
.fa-check:before {
  content: "\f00c";
}
.fa-remove:before,
.fa-close:before,
.fa-times:before {
  content: "\f00d";
}
.fa-search-plus:before {
  content: "\f00e";
}
.fa-search-minus:before {
  content: "\f010";
}
.fa-power-off:before {
  content: "\f011";
}
.fa-signal:before {
  content: "\f012";
}
.fa-gear:before,
.fa-cog:before {
  content: "\f013";
}
.fa-trash-o:before {
  content: "\f014";
}
.fa-home:before {
  content: "\f015";
}
.fa-file-o:before {
  content: "\f016";
}
.fa-clock-o:before {
  content: "\f017";
}
.fa-road:before {
  content: "\f018";
}
.fa-download:before {
  content: "\f019";
}
.fa-arrow-circle-o-down:before {
  content: "\f01a";
}
.fa-arrow-circle-o-up:before {
  content: "\f01b";
}
.fa-inbox:before {
  content: "\f01c";
}
.fa-play-circle-o:before {
  content: "\f01d";
}
.fa-rotate-right:before,
.fa-repeat:before {
  content: "\f01e";
}
.fa-refresh:before {
  content: "\f021";
}
.fa-list-alt:before {
  content: "\f022";
}
.fa-lock:before {
  content: "\f023";
}
.fa-flag:before {
  content: "\f024";
}
.fa-headphones:before {
  content: "\f025";
}
.fa-volume-off:before {
  content: "\f026";
}
.fa-volume-down:before {
  content: "\f027";
}
.fa-volume-up:before {
  content: "\f028";
}
.fa-qrcode:before {
  content: "\f029";
}
.fa-barcode:before {
  content: "\f02a";
}
.fa-tag:before {
  content: "\f02b";
}
.fa-tags:before {
  content: "\f02c";
}
.fa-book:before {
  content: "\f02d";
}
.fa-bookmark:before {
  content: "\f02e";
}
.fa-print:before {
  content: "\f02f";
}
.fa-camera:before {
  content: "\f030";
}
.fa-font:before {
  content: "\f031";
}
.fa-bold:before {
  content: "\f032";
}
.fa-italic:before {
  content: "\f033";
}
.fa-text-height:before {
  content: "\f034";
}
.fa-text-width:before {
  content: "\f035";
}
.fa-align-left:before {
  content: "\f036";
}
.fa-align-center:before {
  content: "\f037";
}
.fa-align-right:before {
  content: "\f038";
}
.fa-align-justify:before {
  content: "\f039";
}
.fa-list:before {
  content: "\f03a";
}
.fa-dedent:before,
.fa-outdent:before {
  content: "\f03b";
}
.fa-indent:before {
  content: "\f03c";
}
.fa-video-camera:before {
  content: "\f03d";
}
.fa-photo:before,
.fa-image:before,
.fa-picture-o:before {
  content: "\f03e";
}
.fa-pencil:before {
  content: "\f040";
}
.fa-map-marker:before {
  content: "\f041";
}
.fa-adjust:before {
  content: "\f042";
}
.fa-tint:before {
  content: "\f043";
}
.fa-edit:before,
.fa-pencil-square-o:before {
  content: "\f044";
}
.fa-share-square-o:before {
  content: "\f045";
}
.fa-check-square-o:before {
  content: "\f046";
}
.fa-arrows:before {
  content: "\f047";
}
.fa-step-backward:before {
  content: "\f048";
}
.fa-fast-backward:before {
  content: "\f049";
}
.fa-backward:before {
  content: "\f04a";
}
.fa-play:before {
  content: "\f04b";
}
.fa-pause:before {
  content: "\f04c";
}
.fa-stop:before {
  content: "\f04d";
}
.fa-forward:before {
  content: "\f04e";
}
.fa-fast-forward:before {
  content: "\f050";
}
.fa-step-forward:before {
  content: "\f051";
}
.fa-eject:before {
  content: "\f052";
}
.fa-chevron-left:before {
  content: "\f053";
}
.fa-chevron-right:before {
  content: "\f054";
}
.fa-plus-circle:before {
  content: "\f055";
}
.fa-minus-circle:before {
  content: "\f056";
}
.fa-times-circle:before {
  content: "\f057";
}
.fa-check-circle:before {
  content: "\f058";
}
.fa-question-circle:before {
  content: "\f059";
}
.fa-info-circle:before {
  content: "\f05a";
}
.fa-crosshairs:before {
  content: "\f05b";
}
.fa-times-circle-o:before {
  content: "\f05c";
}
.fa-check-circle-o:before {
  content: "\f05d";
}
.fa-ban:before {
  content: "\f05e";
}
.fa-arrow-left:before {
  content: "\f060";
}
.fa-arrow-right:before {
  content: "\f061";
}
.fa-arrow-up:before {
  content: "\f062";
}
.fa-arrow-down:before {
  content: "\f063";
}
.fa-mail-forward:before,
.fa-share:before {
  content: "\f064";
}
.fa-expand:before {
  content: "\f065";
}
.fa-compress:before {
  content: "\f066";
}
.fa-plus:before {
  content: "\f067";
}
.fa-minus:before {
  content: "\f068";
}
.fa-asterisk:before {
  content: "\f069";
}
.fa-exclamation-circle:before {
  content: "\f06a";
}
.fa-gift:before {
  content: "\f06b";
}
.fa-leaf:before {
  content: "\f06c";
}
.fa-fire:before {
  content: "\f06d";
}
.fa-eye:before {
  content: "\f06e";
}
.fa-eye-slash:before {
  content: "\f070";
}
.fa-warning:before,
.fa-exclamation-triangle:before {
  content: "\f071";
}
.fa-plane:before {
  content: "\f072";
}
.fa-calendar:before {
  content: "\f073";
}
.fa-random:before {
  content: "\f074";
}
.fa-comment:before {
  content: "\f075";
}
.fa-magnet:before {
  content: "\f076";
}
.fa-chevron-up:before {
  content: "\f077";
}
.fa-chevron-down:before {
  content: "\f078";
}
.fa-retweet:before {
  content: "\f079";
}
.fa-shopping-cart:before {
  content: "\f07a";
}
.fa-folder:before {
  content: "\f07b";
}
.fa-folder-open:before {
  content: "\f07c";
}
.fa-arrows-v:before {
  content: "\f07d";
}
.fa-arrows-h:before {
  content: "\f07e";
}
.fa-bar-chart-o:before,
.fa-bar-chart:before {
  content: "\f080";
}
.fa-twitter-square:before {
  content: "\f081";
}
.fa-facebook-square:before {
  content: "\f082";
}
.fa-camera-retro:before {
  content: "\f083";
}
.fa-key:before {
  content: "\f084";
}
.fa-gears:before,
.fa-cogs:before {
  content: "\f085";
}
.fa-comments:before {
  content: "\f086";
}
.fa-thumbs-o-up:before {
  content: "\f087";
}
.fa-thumbs-o-down:before {
  content: "\f088";
}
.fa-star-half:before {
  content: "\f089";
}
.fa-heart-o:before {
  content: "\f08a";
}
.fa-sign-out:before {
  content: "\f08b";
}
.fa-linkedin-square:before {
  content: "\f08c";
}
.fa-thumb-tack:before {
  content: "\f08d";
}
.fa-external-link:before {
  content: "\f08e";
}
.fa-sign-in:before {
  content: "\f090";
}
.fa-trophy:before {
  content: "\f091";
}
.fa-github-square:before {
  content: "\f092";
}
.fa-upload:before {
  content: "\f093";
}
.fa-lemon-o:before {
  content: "\f094";
}
.fa-phone:before {
  content: "\f095";
}
.fa-square-o:before {
  content: "\f096";
}
.fa-bookmark-o:before {
  content: "\f097";
}
.fa-phone-square:before {
  content: "\f098";
}
.fa-twitter:before {
  content: "\f099";
}
.fa-facebook-f:before,
.fa-facebook:before {
  content: "\f09a";
}
.fa-github:before {
  content: "\f09b";
}
.fa-unlock:before {
  content: "\f09c";
}
.fa-credit-card:before {
  content: "\f09d";
}
.fa-feed:before,
.fa-rss:before {
  content: "\f09e";
}
.fa-hdd-o:before {
  content: "\f0a0";
}
.fa-bullhorn:before {
  content: "\f0a1";
}
.fa-bell:before {
  content: "\f0f3";
}
.fa-certificate:before {
  content: "\f0a3";
}
.fa-hand-o-right:before {
  content: "\f0a4";
}
.fa-hand-o-left:before {
  content: "\f0a5";
}
.fa-hand-o-up:before {
  content: "\f0a6";
}
.fa-hand-o-down:before {
  content: "\f0a7";
}
.fa-arrow-circle-left:before {
  content: "\f0a8";
}
.fa-arrow-circle-right:before {
  content: "\f0a9";
}
.fa-arrow-circle-up:before {
  content: "\f0aa";
}
.fa-arrow-circle-down:before {
  content: "\f0ab";
}
.fa-globe:before {
  content: "\f0ac";
}
.fa-wrench:before {
  content: "\f0ad";
}
.fa-tasks:before {
  content: "\f0ae";
}
.fa-filter:before {
  content: "\f0b0";
}
.fa-briefcase:before {
  content: "\f0b1";
}
.fa-arrows-alt:before {
  content: "\f0b2";
}
.fa-group:before,
.fa-users:before {
  content: "\f0c0";
}
.fa-chain:before,
.fa-link:before {
  content: "\f0c1";
}
.fa-cloud:before {
  content: "\f0c2";
}
.fa-flask:before {
  content: "\f0c3";
}
.fa-cut:before,
.fa-scissors:before {
  content: "\f0c4";
}
.fa-copy:before,
.fa-files-o:before {
  content: "\f0c5";
}
.fa-paperclip:before {
  content: "\f0c6";
}
.fa-save:before,
.fa-floppy-o:before {
  content: "\f0c7";
}
.fa-square:before {
  content: "\f0c8";
}
.fa-navicon:before,
.fa-reorder:before,
.fa-bars:before {
  content: "\f0c9";
}
.fa-list-ul:before {
  content: "\f0ca";
}
.fa-list-ol:before {
  content: "\f0cb";
}
.fa-strikethrough:before {
  content: "\f0cc";
}
.fa-underline:before {
  content: "\f0cd";
}
.fa-table:before {
  content: "\f0ce";
}
.fa-magic:before {
  content: "\f0d0";
}
.fa-truck:before {
  content: "\f0d1";
}
.fa-pinterest:before {
  content: "\f0d2";
}
.fa-pinterest-square:before {
  content: "\f0d3";
}
.fa-google-plus-square:before {
  content: "\f0d4";
}
.fa-google-plus:before {
  content: "\f0d5";
}
.fa-money:before {
  content: "\f0d6";
}
.fa-caret-down:before {
  content: "\f0d7";
}
.fa-caret-up:before {
  content: "\f0d8";
}
.fa-caret-left:before {
  content: "\f0d9";
}
.fa-caret-right:before {
  content: "\f0da";
}
.fa-columns:before {
  content: "\f0db";
}
.fa-unsorted:before,
.fa-sort:before {
  content: "\f0dc";
}
.fa-sort-down:before,
.fa-sort-desc:before {
  content: "\f0dd";
}
.fa-sort-up:before,
.fa-sort-asc:before {
  content: "\f0de";
}
.fa-envelope:before {
  content: "\f0e0";
}
.fa-linkedin:before {
  content: "\f0e1";
}
.fa-rotate-left:before,
.fa-undo:before {
  content: "\f0e2";
}
.fa-legal:before,
.fa-gavel:before {
  content: "\f0e3";
}
.fa-dashboard:before,
.fa-tachometer:before {
  content: "\f0e4";
}
.fa-comment-o:before {
  content: "\f0e5";
}
.fa-comments-o:before {
  content: "\f0e6";
}
.fa-flash:before,
.fa-bolt:before {
  content: "\f0e7";
}
.fa-sitemap:before {
  content: "\f0e8";
}
.fa-umbrella:before {
  content: "\f0e9";
}
.fa-paste:before,
.fa-clipboard:before {
  content: "\f0ea";
}
.fa-lightbulb-o:before {
  content: "\f0eb";
}
.fa-exchange:before {
  content: "\f0ec";
}
.fa-cloud-download:before {
  content: "\f0ed";
}
.fa-cloud-upload:before {
  content: "\f0ee";
}
.fa-user-md:before {
  content: "\f0f0";
}
.fa-stethoscope:before {
  content: "\f0f1";
}
.fa-suitcase:before {
  content: "\f0f2";
}
.fa-bell-o:before {
  content: "\f0a2";
}
.fa-coffee:before {
  content: "\f0f4";
}
.fa-cutlery:before {
  content: "\f0f5";
}
.fa-file-text-o:before {
  content: "\f0f6";
}
.fa-building-o:before {
  content: "\f0f7";
}
.fa-hospital-o:before {
  content: "\f0f8";
}
.fa-ambulance:before {
  content: "\f0f9";
}
.fa-medkit:before {
  content: "\f0fa";
}
.fa-fighter-jet:before {
  content: "\f0fb";
}
.fa-beer:before {
  content: "\f0fc";
}
.fa-h-square:before {
  content: "\f0fd";
}
.fa-plus-square:before {
  content: "\f0fe";
}
.fa-angle-double-left:before {
  content: "\f100";
}
.fa-angle-double-right:before {
  content: "\f101";
}
.fa-angle-double-up:before {
  content: "\f102";
}
.fa-angle-double-down:before {
  content: "\f103";
}
.fa-angle-left:before {
  content: "\f104";
}
.fa-angle-right:before {
  content: "\f105";
}
.fa-angle-up:before {
  content: "\f106";
}
.fa-angle-down:before {
  content: "\f107";
}
.fa-desktop:before {
  content: "\f108";
}
.fa-laptop:before {
  content: "\f109";
}
.fa-tablet:before {
  content: "\f10a";
}
.fa-mobile-phone:before,
.fa-mobile:before {
  content: "\f10b";
}
.fa-circle-o:before {
  content: "\f10c";
}
.fa-quote-left:before {
  content: "\f10d";
}
.fa-quote-right:before {
  content: "\f10e";
}
.fa-spinner:before {
  content: "\f110";
}
.fa-circle:before {
  content: "\f111";
}
.fa-mail-reply:before,
.fa-reply:before {
  content: "\f112";
}
.fa-github-alt:before {
  content: "\f113";
}
.fa-folder-o:before {
  content: "\f114";
}
.fa-folder-open-o:before {
  content: "\f115";
}
.fa-smile-o:before {
  content: "\f118";
}
.fa-frown-o:before {
  content: "\f119";
}
.fa-meh-o:before {
  content: "\f11a";
}
.fa-gamepad:before {
  content: "\f11b";
}
.fa-keyboard-o:before {
  content: "\f11c";
}
.fa-flag-o:before {
  content: "\f11d";
}
.fa-flag-checkered:before {
  content: "\f11e";
}
.fa-terminal:before {
  content: "\f120";
}
.fa-code:before {
  content: "\f121";
}
.fa-mail-reply-all:before,
.fa-reply-all:before {
  content: "\f122";
}
.fa-star-half-empty:before,
.fa-star-half-full:before,
.fa-star-half-o:before {
  content: "\f123";
}
.fa-location-arrow:before {
  content: "\f124";
}
.fa-crop:before {
  content: "\f125";
}
.fa-code-fork:before {
  content: "\f126";
}
.fa-unlink:before,
.fa-chain-broken:before {
  content: "\f127";
}
.fa-question:before {
  content: "\f128";
}
.fa-info:before {
  content: "\f129";
}
.fa-exclamation:before {
  content: "\f12a";
}
.fa-superscript:before {
  content: "\f12b";
}
.fa-subscript:before {
  content: "\f12c";
}
.fa-eraser:before {
  content: "\f12d";
}
.fa-puzzle-piece:before {
  content: "\f12e";
}
.fa-microphone:before {
  content: "\f130";
}
.fa-microphone-slash:before {
  content: "\f131";
}
.fa-shield:before {
  content: "\f132";
}
.fa-calendar-o:before {
  content: "\f133";
}
.fa-fire-extinguisher:before {
  content: "\f134";
}
.fa-rocket:before {
  content: "\f135";
}
.fa-maxcdn:before {
  content: "\f136";
}
.fa-chevron-circle-left:before {
  content: "\f137";
}
.fa-chevron-circle-right:before {
  content: "\f138";
}
.fa-chevron-circle-up:before {
  content: "\f139";
}
.fa-chevron-circle-down:before {
  content: "\f13a";
}
.fa-html5:before {
  content: "\f13b";
}
.fa-css3:before {
  content: "\f13c";
}
.fa-anchor:before {
  content: "\f13d";
}
.fa-unlock-alt:before {
  content: "\f13e";
}
.fa-bullseye:before {
  content: "\f140";
}
.fa-ellipsis-h:before {
  content: "\f141";
}
.fa-ellipsis-v:before {
  content: "\f142";
}
.fa-rss-square:before {
  content: "\f143";
}
.fa-play-circle:before {
  content: "\f144";
}
.fa-ticket:before {
  content: "\f145";
}
.fa-minus-square:before {
  content: "\f146";
}
.fa-minus-square-o:before {
  content: "\f147";
}
.fa-level-up:before {
  content: "\f148";
}
.fa-level-down:before {
  content: "\f149";
}
.fa-check-square:before {
  content: "\f14a";
}
.fa-pencil-square:before {
  content: "\f14b";
}
.fa-external-link-square:before {
  content: "\f14c";
}
.fa-share-square:before {
  content: "\f14d";
}
.fa-compass:before {
  content: "\f14e";
}
.fa-toggle-down:before,
.fa-caret-square-o-down:before {
  content: "\f150";
}
.fa-toggle-up:before,
.fa-caret-square-o-up:before {
  content: "\f151";
}
.fa-toggle-right:before,
.fa-caret-square-o-right:before {
  content: "\f152";
}
.fa-euro:before,
.fa-eur:before {
  content: "\f153";
}
.fa-gbp:before {
  content: "\f154";
}
.fa-dollar:before,
.fa-usd:before {
  content: "\f155";
}
.fa-rupee:before,
.fa-inr:before {
  content: "\f156";
}
.fa-cny:before,
.fa-rmb:before,
.fa-yen:before,
.fa-jpy:before {
  content: "\f157";
}
.fa-ruble:before,
.fa-rouble:before,
.fa-rub:before {
  content: "\f158";
}
.fa-won:before,
.fa-krw:before {
  content: "\f159";
}
.fa-bitcoin:before,
.fa-btc:before {
  content: "\f15a";
}
.fa-file:before {
  content: "\f15b";
}
.fa-file-text:before {
  content: "\f15c";
}
.fa-sort-alpha-asc:before {
  content: "\f15d";
}
.fa-sort-alpha-desc:before {
  content: "\f15e";
}
.fa-sort-amount-asc:before {
  content: "\f160";
}
.fa-sort-amount-desc:before {
  content: "\f161";
}
.fa-sort-numeric-asc:before {
  content: "\f162";
}
.fa-sort-numeric-desc:before {
  content: "\f163";
}
.fa-thumbs-up:before {
  content: "\f164";
}
.fa-thumbs-down:before {
  content: "\f165";
}
.fa-youtube-square:before {
  content: "\f166";
}
.fa-youtube:before {
  content: "\f167";
}
.fa-xing:before {
  content: "\f168";
}
.fa-xing-square:before {
  content: "\f169";
}
.fa-youtube-play:before {
  content: "\f16a";
}
.fa-dropbox:before {
  content: "\f16b";
}
.fa-stack-overflow:before {
  content: "\f16c";
}
.fa-instagram:before {
  content: "\f16d";
}
.fa-flickr:before {
  content: "\f16e";
}
.fa-adn:before {
  content: "\f170";
}
.fa-bitbucket:before {
  content: "\f171";
}
.fa-bitbucket-square:before {
  content: "\f172";
}
.fa-tumblr:before {
  content: "\f173";
}
.fa-tumblr-square:before {
  content: "\f174";
}
.fa-long-arrow-down:before {
  content: "\f175";
}
.fa-long-arrow-up:before {
  content: "\f176";
}
.fa-long-arrow-left:before {
  content: "\f177";
}
.fa-long-arrow-right:before {
  content: "\f178";
}
.fa-apple:before {
  content: "\f179";
}
.fa-windows:before {
  content: "\f17a";
}
.fa-android:before {
  content: "\f17b";
}
.fa-linux:before {
  content: "\f17c";
}
.fa-dribbble:before {
  content: "\f17d";
}
.fa-skype:before {
  content: "\f17e";
}
.fa-foursquare:before {
  content: "\f180";
}
.fa-trello:before {
  content: "\f181";
}
.fa-female:before {
  content: "\f182";
}
.fa-male:before {
  content: "\f183";
}
.fa-gittip:before,
.fa-gratipay:before {
  content: "\f184";
}
.fa-sun-o:before {
  content: "\f185";
}
.fa-moon-o:before {
  content: "\f186";
}
.fa-archive:before {
  content: "\f187";
}
.fa-bug:before {
  content: "\f188";
}
.fa-vk:before {
  content: "\f189";
}
.fa-weibo:before {
  content: "\f18a";
}
.fa-renren:before {
  content: "\f18b";
}
.fa-pagelines:before {
  content: "\f18c";
}
.fa-stack-exchange:before {
  content: "\f18d";
}
.fa-arrow-circle-o-right:before {
  content: "\f18e";
}
.fa-arrow-circle-o-left:before {
  content: "\f190";
}
.fa-toggle-left:before,
.fa-caret-square-o-left:before {
  content: "\f191";
}
.fa-dot-circle-o:before {
  content: "\f192";
}
.fa-wheelchair:before {
  content: "\f193";
}
.fa-vimeo-square:before {
  content: "\f194";
}
.fa-turkish-lira:before,
.fa-try:before {
  content: "\f195";
}
.fa-plus-square-o:before {
  content: "\f196";
}
.fa-space-shuttle:before {
  content: "\f197";
}
.fa-slack:before {
  content: "\f198";
}
.fa-envelope-square:before {
  content: "\f199";
}
.fa-wordpress:before {
  content: "\f19a";
}
.fa-openid:before {
  content: "\f19b";
}
.fa-institution:before,
.fa-bank:before,
.fa-university:before {
  content: "\f19c";
}
.fa-mortar-board:before,
.fa-graduation-cap:before {
  content: "\f19d";
}
.fa-yahoo:before {
  content: "\f19e";
}
.fa-google:before {
  content: "\f1a0";
}
.fa-reddit:before {
  content: "\f1a1";
}
.fa-reddit-square:before {
  content: "\f1a2";
}
.fa-stumbleupon-circle:before {
  content: "\f1a3";
}
.fa-stumbleupon:before {
  content: "\f1a4";
}
.fa-delicious:before {
  content: "\f1a5";
}
.fa-digg:before {
  content: "\f1a6";
}
.fa-pied-piper-pp:before {
  content: "\f1a7";
}
.fa-pied-piper-alt:before {
  content: "\f1a8";
}
.fa-drupal:before {
  content: "\f1a9";
}
.fa-joomla:before {
  content: "\f1aa";
}
.fa-language:before {
  content: "\f1ab";
}
.fa-fax:before {
  content: "\f1ac";
}
.fa-building:before {
  content: "\f1ad";
}
.fa-child:before {
  content: "\f1ae";
}
.fa-paw:before {
  content: "\f1b0";
}
.fa-spoon:before {
  content: "\f1b1";
}
.fa-cube:before {
  content: "\f1b2";
}
.fa-cubes:before {
  content: "\f1b3";
}
.fa-behance:before {
  content: "\f1b4";
}
.fa-behance-square:before {
  content: "\f1b5";
}
.fa-steam:before {
  content: "\f1b6";
}
.fa-steam-square:before {
  content: "\f1b7";
}
.fa-recycle:before {
  content: "\f1b8";
}
.fa-automobile:before,
.fa-car:before {
  content: "\f1b9";
}
.fa-cab:before,
.fa-taxi:before {
  content: "\f1ba";
}
.fa-tree:before {
  content: "\f1bb";
}
.fa-spotify:before {
  content: "\f1bc";
}
.fa-deviantart:before {
  content: "\f1bd";
}
.fa-soundcloud:before {
  content: "\f1be";
}
.fa-database:before {
  content: "\f1c0";
}
.fa-file-pdf-o:before {
  content: "\f1c1";
}
.fa-file-word-o:before {
  content: "\f1c2";
}
.fa-file-excel-o:before {
  content: "\f1c3";
}
.fa-file-powerpoint-o:before {
  content: "\f1c4";
}
.fa-file-photo-o:before,
.fa-file-picture-o:before,
.fa-file-image-o:before {
  content: "\f1c5";
}
.fa-file-zip-o:before,
.fa-file-archive-o:before {
  content: "\f1c6";
}
.fa-file-sound-o:before,
.fa-file-audio-o:before {
  content: "\f1c7";
}
.fa-file-movie-o:before,
.fa-file-video-o:before {
  content: "\f1c8";
}
.fa-file-code-o:before {
  content: "\f1c9";
}
.fa-vine:before {
  content: "\f1ca";
}
.fa-codepen:before {
  content: "\f1cb";
}
.fa-jsfiddle:before {
  content: "\f1cc";
}
.fa-life-bouy:before,
.fa-life-buoy:before,
.fa-life-saver:before,
.fa-support:before,
.fa-life-ring:before {
  content: "\f1cd";
}
.fa-circle-o-notch:before {
  content: "\f1ce";
}
.fa-ra:before,
.fa-resistance:before,
.fa-rebel:before {
  content: "\f1d0";
}
.fa-ge:before,
.fa-empire:before {
  content: "\f1d1";
}
.fa-git-square:before {
  content: "\f1d2";
}
.fa-git:before {
  content: "\f1d3";
}
.fa-y-combinator-square:before,
.fa-yc-square:before,
.fa-hacker-news:before {
  content: "\f1d4";
}
.fa-tencent-weibo:before {
  content: "\f1d5";
}
.fa-qq:before {
  content: "\f1d6";
}
.fa-wechat:before,
.fa-weixin:before {
  content: "\f1d7";
}
.fa-send:before,
.fa-paper-plane:before {
  content: "\f1d8";
}
.fa-send-o:before,
.fa-paper-plane-o:before {
  content: "\f1d9";
}
.fa-history:before {
  content: "\f1da";
}
.fa-circle-thin:before {
  content: "\f1db";
}
.fa-header:before {
  content: "\f1dc";
}
.fa-paragraph:before {
  content: "\f1dd";
}
.fa-sliders:before {
  content: "\f1de";
}
.fa-share-alt:before {
  content: "\f1e0";
}
.fa-share-alt-square:before {
  content: "\f1e1";
}
.fa-bomb:before {
  content: "\f1e2";
}
.fa-soccer-ball-o:before,
.fa-futbol-o:before {
  content: "\f1e3";
}
.fa-tty:before {
  content: "\f1e4";
}
.fa-binoculars:before {
  content: "\f1e5";
}
.fa-plug:before {
  content: "\f1e6";
}
.fa-slideshare:before {
  content: "\f1e7";
}
.fa-twitch:before {
  content: "\f1e8";
}
.fa-yelp:before {
  content: "\f1e9";
}
.fa-newspaper-o:before {
  content: "\f1ea";
}
.fa-wifi:before {
  content: "\f1eb";
}
.fa-calculator:before {
  content: "\f1ec";
}
.fa-paypal:before {
  content: "\f1ed";
}
.fa-google-wallet:before {
  content: "\f1ee";
}
.fa-cc-visa:before {
  content: "\f1f0";
}
.fa-cc-mastercard:before {
  content: "\f1f1";
}
.fa-cc-discover:before {
  content: "\f1f2";
}
.fa-cc-amex:before {
  content: "\f1f3";
}
.fa-cc-paypal:before {
  content: "\f1f4";
}
.fa-cc-stripe:before {
  content: "\f1f5";
}
.fa-bell-slash:before {
  content: "\f1f6";
}
.fa-bell-slash-o:before {
  content: "\f1f7";
}
.fa-trash:before {
  content: "\f1f8";
}
.fa-copyright:before {
  content: "\f1f9";
}
.fa-at:before {
  content: "\f1fa";
}
.fa-eyedropper:before {
  content: "\f1fb";
}
.fa-paint-brush:before {
  content: "\f1fc";
}
.fa-birthday-cake:before {
  content: "\f1fd";
}
.fa-area-chart:before {
  content: "\f1fe";
}
.fa-pie-chart:before {
  content: "\f200";
}
.fa-line-chart:before {
  content: "\f201";
}
.fa-lastfm:before {
  content: "\f202";
}
.fa-lastfm-square:before {
  content: "\f203";
}
.fa-toggle-off:before {
  content: "\f204";
}
.fa-toggle-on:before {
  content: "\f205";
}
.fa-bicycle:before {
  content: "\f206";
}
.fa-bus:before {
  content: "\f207";
}
.fa-ioxhost:before {
  content: "\f208";
}
.fa-angellist:before {
  content: "\f209";
}
.fa-cc:before {
  content: "\f20a";
}
.fa-shekel:before,
.fa-sheqel:before,
.fa-ils:before {
  content: "\f20b";
}
.fa-meanpath:before {
  content: "\f20c";
}
.fa-buysellads:before {
  content: "\f20d";
}
.fa-connectdevelop:before {
  content: "\f20e";
}
.fa-dashcube:before {
  content: "\f210";
}
.fa-forumbee:before {
  content: "\f211";
}
.fa-leanpub:before {
  content: "\f212";
}
.fa-sellsy:before {
  content: "\f213";
}
.fa-shirtsinbulk:before {
  content: "\f214";
}
.fa-simplybuilt:before {
  content: "\f215";
}
.fa-skyatlas:before {
  content: "\f216";
}
.fa-cart-plus:before {
  content: "\f217";
}
.fa-cart-arrow-down:before {
  content: "\f218";
}
.fa-diamond:before {
  content: "\f219";
}
.fa-ship:before {
  content: "\f21a";
}
.fa-user-secret:before {
  content: "\f21b";
}
.fa-motorcycle:before {
  content: "\f21c";
}
.fa-street-view:before {
  content: "\f21d";
}
.fa-heartbeat:before {
  content: "\f21e";
}
.fa-venus:before {
  content: "\f221";
}
.fa-mars:before {
  content: "\f222";
}
.fa-mercury:before {
  content: "\f223";
}
.fa-intersex:before,
.fa-transgender:before {
  content: "\f224";
}
.fa-transgender-alt:before {
  content: "\f225";
}
.fa-venus-double:before {
  content: "\f226";
}
.fa-mars-double:before {
  content: "\f227";
}
.fa-venus-mars:before {
  content: "\f228";
}
.fa-mars-stroke:before {
  content: "\f229";
}
.fa-mars-stroke-v:before {
  content: "\f22a";
}
.fa-mars-stroke-h:before {
  content: "\f22b";
}
.fa-neuter:before {
  content: "\f22c";
}
.fa-genderless:before {
  content: "\f22d";
}
.fa-facebook-official:before {
  content: "\f230";
}
.fa-pinterest-p:before {
  content: "\f231";
}
.fa-whatsapp:before {
  content: "\f232";
}
.fa-server:before {
  content: "\f233";
}
.fa-user-plus:before {
  content: "\f234";
}
.fa-user-times:before {
  content: "\f235";
}
.fa-hotel:before,
.fa-bed:before {
  content: "\f236";
}
.fa-viacoin:before {
  content: "\f237";
}
.fa-train:before {
  content: "\f238";
}
.fa-subway:before {
  content: "\f239";
}
.fa-medium:before {
  content: "\f23a";
}
.fa-yc:before,
.fa-y-combinator:before {
  content: "\f23b";
}
.fa-optin-monster:before {
  content: "\f23c";
}
.fa-opencart:before {
  content: "\f23d";
}
.fa-expeditedssl:before {
  content: "\f23e";
}
.fa-battery-4:before,
.fa-battery:before,
.fa-battery-full:before {
  content: "\f240";
}
.fa-battery-3:before,
.fa-battery-three-quarters:before {
  content: "\f241";
}
.fa-battery-2:before,
.fa-battery-half:before {
  content: "\f242";
}
.fa-battery-1:before,
.fa-battery-quarter:before {
  content: "\f243";
}
.fa-battery-0:before,
.fa-battery-empty:before {
  content: "\f244";
}
.fa-mouse-pointer:before {
  content: "\f245";
}
.fa-i-cursor:before {
  content: "\f246";
}
.fa-object-group:before {
  content: "\f247";
}
.fa-object-ungroup:before {
  content: "\f248";
}
.fa-sticky-note:before {
  content: "\f249";
}
.fa-sticky-note-o:before {
  content: "\f24a";
}
.fa-cc-jcb:before {
  content: "\f24b";
}
.fa-cc-diners-club:before {
  content: "\f24c";
}
.fa-clone:before {
  content: "\f24d";
}
.fa-balance-scale:before {
  content: "\f24e";
}
.fa-hourglass-o:before {
  content: "\f250";
}
.fa-hourglass-1:before,
.fa-hourglass-start:before {
  content: "\f251";
}
.fa-hourglass-2:before,
.fa-hourglass-half:before {
  content: "\f252";
}
.fa-hourglass-3:before,
.fa-hourglass-end:before {
  content: "\f253";
}
.fa-hourglass:before {
  content: "\f254";
}
.fa-hand-grab-o:before,
.fa-hand-rock-o:before {
  content: "\f255";
}
.fa-hand-stop-o:before,
.fa-hand-paper-o:before {
  content: "\f256";
}
.fa-hand-scissors-o:before {
  content: "\f257";
}
.fa-hand-lizard-o:before {
  content: "\f258";
}
.fa-hand-spock-o:before {
  content: "\f259";
}
.fa-hand-pointer-o:before {
  content: "\f25a";
}
.fa-hand-peace-o:before {
  content: "\f25b";
}
.fa-trademark:before {
  content: "\f25c";
}
.fa-registered:before {
  content: "\f25d";
}
.fa-creative-commons:before {
  content: "\f25e";
}
.fa-gg:before {
  content: "\f260";
}
.fa-gg-circle:before {
  content: "\f261";
}
.fa-tripadvisor:before {
  content: "\f262";
}
.fa-odnoklassniki:before {
  content: "\f263";
}
.fa-odnoklassniki-square:before {
  content: "\f264";
}
.fa-get-pocket:before {
  content: "\f265";
}
.fa-wikipedia-w:before {
  content: "\f266";
}
.fa-safari:before {
  content: "\f267";
}
.fa-chrome:before {
  content: "\f268";
}
.fa-firefox:before {
  content: "\f269";
}
.fa-opera:before {
  content: "\f26a";
}
.fa-internet-explorer:before {
  content: "\f26b";
}
.fa-tv:before,
.fa-television:before {
  content: "\f26c";
}
.fa-contao:before {
  content: "\f26d";
}
.fa-500px:before {
  content: "\f26e";
}
.fa-amazon:before {
  content: "\f270";
}
.fa-calendar-plus-o:before {
  content: "\f271";
}
.fa-calendar-minus-o:before {
  content: "\f272";
}
.fa-calendar-times-o:before {
  content: "\f273";
}
.fa-calendar-check-o:before {
  content: "\f274";
}
.fa-industry:before {
  content: "\f275";
}
.fa-map-pin:before {
  content: "\f276";
}
.fa-map-signs:before {
  content: "\f277";
}
.fa-map-o:before {
  content: "\f278";
}
.fa-map:before {
  content: "\f279";
}
.fa-commenting:before {
  content: "\f27a";
}
.fa-commenting-o:before {
  content: "\f27b";
}
.fa-houzz:before {
  content: "\f27c";
}
.fa-vimeo:before {
  content: "\f27d";
}
.fa-black-tie:before {
  content: "\f27e";
}
.fa-fonticons:before {
  content: "\f280";
}
.fa-reddit-alien:before {
  content: "\f281";
}
.fa-edge:before {
  content: "\f282";
}
.fa-credit-card-alt:before {
  content: "\f283";
}
.fa-codiepie:before {
  content: "\f284";
}
.fa-modx:before {
  content: "\f285";
}
.fa-fort-awesome:before {
  content: "\f286";
}
.fa-usb:before {
  content: "\f287";
}
.fa-product-hunt:before {
  content: "\f288";
}
.fa-mixcloud:before {
  content: "\f289";
}
.fa-scribd:before {
  content: "\f28a";
}
.fa-pause-circle:before {
  content: "\f28b";
}
.fa-pause-circle-o:before {
  content: "\f28c";
}
.fa-stop-circle:before {
  content: "\f28d";
}
.fa-stop-circle-o:before {
  content: "\f28e";
}
.fa-shopping-bag:before {
  content: "\f290";
}
.fa-shopping-basket:before {
  content: "\f291";
}
.fa-hashtag:before {
  content: "\f292";
}
.fa-bluetooth:before {
  content: "\f293";
}
.fa-bluetooth-b:before {
  content: "\f294";
}
.fa-percent:before {
  content: "\f295";
}
.fa-gitlab:before {
  content: "\f296";
}
.fa-wpbeginner:before {
  content: "\f297";
}
.fa-wpforms:before {
  content: "\f298";
}
.fa-envira:before {
  content: "\f299";
}
.fa-universal-access:before {
  content: "\f29a";
}
.fa-wheelchair-alt:before {
  content: "\f29b";
}
.fa-question-circle-o:before {
  content: "\f29c";
}
.fa-blind:before {
  content: "\f29d";
}
.fa-audio-description:before {
  content: "\f29e";
}
.fa-volume-control-phone:before {
  content: "\f2a0";
}
.fa-braille:before {
  content: "\f2a1";
}
.fa-assistive-listening-systems:before {
  content: "\f2a2";
}
.fa-asl-interpreting:before,
.fa-american-sign-language-interpreting:before {
  content: "\f2a3";
}
.fa-deafness:before,
.fa-hard-of-hearing:before,
.fa-deaf:before {
  content: "\f2a4";
}
.fa-glide:before {
  content: "\f2a5";
}
.fa-glide-g:before {
  content: "\f2a6";
}
.fa-signing:before,
.fa-sign-language:before {
  content: "\f2a7";
}
.fa-low-vision:before {
  content: "\f2a8";
}
.fa-viadeo:before {
  content: "\f2a9";
}
.fa-viadeo-square:before {
  content: "\f2aa";
}
.fa-snapchat:before {
  content: "\f2ab";
}
.fa-snapchat-ghost:before {
  content: "\f2ac";
}
.fa-snapchat-square:before {
  content: "\f2ad";
}
.fa-pied-piper:before {
  content: "\f2ae";
}
.fa-first-order:before {
  content: "\f2b0";
}
.fa-yoast:before {
  content: "\f2b1";
}
.fa-themeisle:before {
  content: "\f2b2";
}
.fa-google-plus-circle:before,
.fa-google-plus-official:before {
  content: "\f2b3";
}
.fa-fa:before,
.fa-font-awesome:before {
  content: "\f2b4";
}
.fa-handshake-o:before {
  content: "\f2b5";
}
.fa-envelope-open:before {
  content: "\f2b6";
}
.fa-envelope-open-o:before {
  content: "\f2b7";
}
.fa-linode:before {
  content: "\f2b8";
}
.fa-address-book:before {
  content: "\f2b9";
}
.fa-address-book-o:before {
  content: "\f2ba";
}
.fa-vcard:before,
.fa-address-card:before {
  content: "\f2bb";
}
.fa-vcard-o:before,
.fa-address-card-o:before {
  content: "\f2bc";
}
.fa-user-circle:before {
  content: "\f2bd";
}
.fa-user-circle-o:before {
  content: "\f2be";
}
.fa-user-o:before {
  content: "\f2c0";
}
.fa-id-badge:before {
  content: "\f2c1";
}
.fa-drivers-license:before,
.fa-id-card:before {
  content: "\f2c2";
}
.fa-drivers-license-o:before,
.fa-id-card-o:before {
  content: "\f2c3";
}
.fa-quora:before {
  content: "\f2c4";
}
.fa-free-code-camp:before {
  content: "\f2c5";
}
.fa-telegram:before {
  content: "\f2c6";
}
.fa-thermometer-4:before,
.fa-thermometer:before,
.fa-thermometer-full:before {
  content: "\f2c7";
}
.fa-thermometer-3:before,
.fa-thermometer-three-quarters:before {
  content: "\f2c8";
}
.fa-thermometer-2:before,
.fa-thermometer-half:before {
  content: "\f2c9";
}
.fa-thermometer-1:before,
.fa-thermometer-quarter:before {
  content: "\f2ca";
}
.fa-thermometer-0:before,
.fa-thermometer-empty:before {
  content: "\f2cb";
}
.fa-shower:before {
  content: "\f2cc";
}
.fa-bathtub:before,
.fa-s15:before,
.fa-bath:before {
  content: "\f2cd";
}
.fa-podcast:before {
  content: "\f2ce";
}
.fa-window-maximize:before {
  content: "\f2d0";
}
.fa-window-minimize:before {
  content: "\f2d1";
}
.fa-window-restore:before {
  content: "\f2d2";
}
.fa-times-rectangle:before,
.fa-window-close:before {
  content: "\f2d3";
}
.fa-times-rectangle-o:before,
.fa-window-close-o:before {
  content: "\f2d4";
}
.fa-bandcamp:before {
  content: "\f2d5";
}
.fa-grav:before {
  content: "\f2d6";
}
.fa-etsy:before {
  content: "\f2d7";
}
.fa-imdb:before {
  content: "\f2d8";
}
.fa-ravelry:before {
  content: "\f2d9";
}
.fa-eercast:before {
  content: "\f2da";
}
.fa-microchip:before {
  content: "\f2db";
}
.fa-snowflake-o:before {
  content: "\f2dc";
}
.fa-superpowers:before {
  content: "\f2dd";
}
.fa-wpexplorer:before {
  content: "\f2de";
}
.fa-meetup:before {
  content: "\f2e0";
}
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
}
.sr-only-focusable:active,
.sr-only-focusable:focus {
  position: static;
  width: auto;
  height: auto;
  margin: 0;
  overflow: visible;
  clip: auto;
}
.sr-only-focusable:active,
.sr-only-focusable:focus {
  position: static;
  width: auto;
  height: auto;
  margin: 0;
  overflow: visible;
  clip: auto;
}
/*!
*
* IPython base
*
*/
.modal.fade .modal-dialog {
  -webkit-transform: translate(0, 0);
  -ms-transform: translate(0, 0);
  -o-transform: translate(0, 0);
  transform: translate(0, 0);
}
code {
  color: #000;
}
pre {
  font-size: inherit;
  line-height: inherit;
}
label {
  font-weight: normal;
}
/* Make the page background atleast 100% the height of the view port */
/* Make the page itself atleast 70% the height of the view port */
.border-box-sizing {
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
.corner-all {
  border-radius: 2px;
}
.no-padding {
  padding: 0px;
}
/* Flexible box model classes */
/* Taken from Alex Russell http://infrequently.org/2009/08/css-3-progress/ */
/* This file is a compatability layer.  It allows the usage of flexible box 
model layouts accross multiple browsers, including older browsers.  The newest,
universal implementation of the flexible box model is used when available (see
`Modern browsers` comments below).  Browsers that are known to implement this 
new spec completely include:

    Firefox 28.0+
    Chrome 29.0+
    Internet Explorer 11+ 
    Opera 17.0+

Browsers not listed, including Safari, are supported via the styling under the
`Old browsers` comments below.
*/
.hbox {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
.hbox > * {
  /* Old browsers */
  -webkit-box-flex: 0;
  -moz-box-flex: 0;
  box-flex: 0;
  /* Modern browsers */
  flex: none;
}
.vbox {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
}
.vbox > * {
  /* Old browsers */
  -webkit-box-flex: 0;
  -moz-box-flex: 0;
  box-flex: 0;
  /* Modern browsers */
  flex: none;
}
.hbox.reverse,
.vbox.reverse,
.reverse {
  /* Old browsers */
  -webkit-box-direction: reverse;
  -moz-box-direction: reverse;
  box-direction: reverse;
  /* Modern browsers */
  flex-direction: row-reverse;
}
.hbox.box-flex0,
.vbox.box-flex0,
.box-flex0 {
  /* Old browsers */
  -webkit-box-flex: 0;
  -moz-box-flex: 0;
  box-flex: 0;
  /* Modern browsers */
  flex: none;
  width: auto;
}
.hbox.box-flex1,
.vbox.box-flex1,
.box-flex1 {
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
.hbox.box-flex,
.vbox.box-flex,
.box-flex {
  /* Old browsers */
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
.hbox.box-flex2,
.vbox.box-flex2,
.box-flex2 {
  /* Old browsers */
  -webkit-box-flex: 2;
  -moz-box-flex: 2;
  box-flex: 2;
  /* Modern browsers */
  flex: 2;
}
.box-group1 {
  /*  Deprecated */
  -webkit-box-flex-group: 1;
  -moz-box-flex-group: 1;
  box-flex-group: 1;
}
.box-group2 {
  /* Deprecated */
  -webkit-box-flex-group: 2;
  -moz-box-flex-group: 2;
  box-flex-group: 2;
}
.hbox.start,
.vbox.start,
.start {
  /* Old browsers */
  -webkit-box-pack: start;
  -moz-box-pack: start;
  box-pack: start;
  /* Modern browsers */
  justify-content: flex-start;
}
.hbox.end,
.vbox.end,
.end {
  /* Old browsers */
  -webkit-box-pack: end;
  -moz-box-pack: end;
  box-pack: end;
  /* Modern browsers */
  justify-content: flex-end;
}
.hbox.center,
.vbox.center,
.center {
  /* Old browsers */
  -webkit-box-pack: center;
  -moz-box-pack: center;
  box-pack: center;
  /* Modern browsers */
  justify-content: center;
}
.hbox.baseline,
.vbox.baseline,
.baseline {
  /* Old browsers */
  -webkit-box-pack: baseline;
  -moz-box-pack: baseline;
  box-pack: baseline;
  /* Modern browsers */
  justify-content: baseline;
}
.hbox.stretch,
.vbox.stretch,
.stretch {
  /* Old browsers */
  -webkit-box-pack: stretch;
  -moz-box-pack: stretch;
  box-pack: stretch;
  /* Modern browsers */
  justify-content: stretch;
}
.hbox.align-start,
.vbox.align-start,
.align-start {
  /* Old browsers */
  -webkit-box-align: start;
  -moz-box-align: start;
  box-align: start;
  /* Modern browsers */
  align-items: flex-start;
}
.hbox.align-end,
.vbox.align-end,
.align-end {
  /* Old browsers */
  -webkit-box-align: end;
  -moz-box-align: end;
  box-align: end;
  /* Modern browsers */
  align-items: flex-end;
}
.hbox.align-center,
.vbox.align-center,
.align-center {
  /* Old browsers */
  -webkit-box-align: center;
  -moz-box-align: center;
  box-align: center;
  /* Modern browsers */
  align-items: center;
}
.hbox.align-baseline,
.vbox.align-baseline,
.align-baseline {
  /* Old browsers */
  -webkit-box-align: baseline;
  -moz-box-align: baseline;
  box-align: baseline;
  /* Modern browsers */
  align-items: baseline;
}
.hbox.align-stretch,
.vbox.align-stretch,
.align-stretch {
  /* Old browsers */
  -webkit-box-align: stretch;
  -moz-box-align: stretch;
  box-align: stretch;
  /* Modern browsers */
  align-items: stretch;
}
div.error {
  margin: 2em;
  text-align: center;
}
div.error > h1 {
  font-size: 500%;
  line-height: normal;
}
div.error > p {
  font-size: 200%;
  line-height: normal;
}
div.traceback-wrapper {
  text-align: left;
  max-width: 800px;
  margin: auto;
}
div.traceback-wrapper pre.traceback {
  max-height: 600px;
  overflow: auto;
}
/**
 * Primary styles
 *
 * Author: Jupyter Development Team
 */
body {
  background-color: #fff;
  /* This makes sure that the body covers the entire window and needs to
       be in a different element than the display: box in wrapper below */
  position: absolute;
  left: 0px;
  right: 0px;
  top: 0px;
  bottom: 0px;
  overflow: visible;
}
body > #header {
  /* Initially hidden to prevent FLOUC */
  display: none;
  background-color: #fff;
  /* Display over codemirror */
  position: relative;
  z-index: 100;
}
body > #header #header-container {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  padding: 5px;
  padding-bottom: 5px;
  padding-top: 5px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
body > #header .header-bar {
  width: 100%;
  height: 1px;
  background: #e7e7e7;
  margin-bottom: -1px;
}
@media print {
  body > #header {
    display: none !important;
  }
}
#header-spacer {
  width: 100%;
  visibility: hidden;
}
@media print {
  #header-spacer {
    display: none;
  }
}
#ipython_notebook {
  padding-left: 0px;
  padding-top: 1px;
  padding-bottom: 1px;
}
[dir="rtl"] #ipython_notebook {
  margin-right: 10px;
  margin-left: 0;
}
[dir="rtl"] #ipython_notebook.pull-left {
  float: right !important;
  float: right;
}
.flex-spacer {
  flex: 1;
}
#noscript {
  width: auto;
  padding-top: 16px;
  padding-bottom: 16px;
  text-align: center;
  font-size: 22px;
  color: red;
  font-weight: bold;
}
#ipython_notebook img {
  height: 28px;
}
#site {
  width: 100%;
  display: none;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  overflow: auto;
}
@media print {
  #site {
    height: auto !important;
  }
}
/* Smaller buttons */
.ui-button .ui-button-text {
  padding: 0.2em 0.8em;
  font-size: 77%;
}
input.ui-button {
  padding: 0.3em 0.9em;
}
span#kernel_logo_widget {
  margin: 0 10px;
}
span#login_widget {
  float: right;
}
[dir="rtl"] span#login_widget {
  float: left;
}
span#login_widget > .button,
#logout {
  color: #333;
  background-color: #fff;
  border-color: #ccc;
}
span#login_widget > .button:focus,
#logout:focus,
span#login_widget > .button.focus,
#logout.focus {
  color: #333;
  background-color: #e6e6e6;
  border-color: #8c8c8c;
}
span#login_widget > .button:hover,
#logout:hover {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
span#login_widget > .button:active,
#logout:active,
span#login_widget > .button.active,
#logout.active,
.open > .dropdown-togglespan#login_widget > .button,
.open > .dropdown-toggle#logout {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
span#login_widget > .button:active:hover,
#logout:active:hover,
span#login_widget > .button.active:hover,
#logout.active:hover,
.open > .dropdown-togglespan#login_widget > .button:hover,
.open > .dropdown-toggle#logout:hover,
span#login_widget > .button:active:focus,
#logout:active:focus,
span#login_widget > .button.active:focus,
#logout.active:focus,
.open > .dropdown-togglespan#login_widget > .button:focus,
.open > .dropdown-toggle#logout:focus,
span#login_widget > .button:active.focus,
#logout:active.focus,
span#login_widget > .button.active.focus,
#logout.active.focus,
.open > .dropdown-togglespan#login_widget > .button.focus,
.open > .dropdown-toggle#logout.focus {
  color: #333;
  background-color: #d4d4d4;
  border-color: #8c8c8c;
}
span#login_widget > .button:active,
#logout:active,
span#login_widget > .button.active,
#logout.active,
.open > .dropdown-togglespan#login_widget > .button,
.open > .dropdown-toggle#logout {
  background-image: none;
}
span#login_widget > .button.disabled:hover,
#logout.disabled:hover,
span#login_widget > .button[disabled]:hover,
#logout[disabled]:hover,
fieldset[disabled] span#login_widget > .button:hover,
fieldset[disabled] #logout:hover,
span#login_widget > .button.disabled:focus,
#logout.disabled:focus,
span#login_widget > .button[disabled]:focus,
#logout[disabled]:focus,
fieldset[disabled] span#login_widget > .button:focus,
fieldset[disabled] #logout:focus,
span#login_widget > .button.disabled.focus,
#logout.disabled.focus,
span#login_widget > .button[disabled].focus,
#logout[disabled].focus,
fieldset[disabled] span#login_widget > .button.focus,
fieldset[disabled] #logout.focus {
  background-color: #fff;
  border-color: #ccc;
}
span#login_widget > .button .badge,
#logout .badge {
  color: #fff;
  background-color: #333;
}
.nav-header {
  text-transform: none;
}
#header > span {
  margin-top: 10px;
}
.modal_stretch .modal-dialog {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  min-height: 80vh;
}
.modal_stretch .modal-dialog .modal-body {
  max-height: calc(100vh - 200px);
  overflow: auto;
  flex: 1;
}
.modal-header {
  cursor: move;
}
@media (min-width: 768px) {
  .modal .modal-dialog {
    width: 700px;
  }
}
@media (min-width: 768px) {
  select.form-control {
    margin-left: 12px;
    margin-right: 12px;
  }
}
/*!
*
* IPython auth
*
*/
.center-nav {
  display: inline-block;
  margin-bottom: -4px;
}
[dir="rtl"] .center-nav form.pull-left {
  float: right !important;
  float: right;
}
[dir="rtl"] .center-nav .navbar-text {
  float: right;
}
[dir="rtl"] .navbar-inner {
  text-align: right;
}
[dir="rtl"] div.text-left {
  text-align: right;
}
/*!
*
* IPython tree view
*
*/
/* We need an invisible input field on top of the sentense*/
/* "Drag file onto the list ..." */
.alternate_upload {
  background-color: none;
  display: inline;
}
.alternate_upload.form {
  padding: 0;
  margin: 0;
}
.alternate_upload input.fileinput {
  position: absolute;
  display: block;
  width: 100%;
  height: 100%;
  overflow: hidden;
  cursor: pointer;
  opacity: 0;
  z-index: 2;
}
.alternate_upload .btn-xs > input.fileinput {
  margin: -1px -5px;
}
.alternate_upload .btn-upload {
  position: relative;
  height: 22px;
}
::-webkit-file-upload-button {
  cursor: pointer;
}
/**
 * Primary styles
 *
 * Author: Jupyter Development Team
 */
ul#tabs {
  margin-bottom: 4px;
}
ul#tabs a {
  padding-top: 6px;
  padding-bottom: 4px;
}
[dir="rtl"] ul#tabs.nav-tabs > li {
  float: right;
}
[dir="rtl"] ul#tabs.nav.nav-tabs {
  padding-right: 0;
}
ul.breadcrumb a:focus,
ul.breadcrumb a:hover {
  text-decoration: none;
}
ul.breadcrumb i.icon-home {
  font-size: 16px;
  margin-right: 4px;
}
ul.breadcrumb span {
  color: #5e5e5e;
}
.list_toolbar {
  padding: 4px 0 4px 0;
  vertical-align: middle;
}
.list_toolbar .tree-buttons {
  padding-top: 1px;
}
[dir="rtl"] .list_toolbar .tree-buttons .pull-right {
  float: left !important;
  float: left;
}
[dir="rtl"] .list_toolbar .col-sm-4,
[dir="rtl"] .list_toolbar .col-sm-8 {
  float: right;
}
.dynamic-buttons {
  padding-top: 3px;
  display: inline-block;
}
.list_toolbar [class*="span"] {
  min-height: 24px;
}
.list_header {
  font-weight: bold;
  background-color: #EEE;
}
.list_placeholder {
  font-weight: bold;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 7px;
  padding-right: 7px;
}
.list_container {
  margin-top: 4px;
  margin-bottom: 20px;
  border: 1px solid #ddd;
  border-radius: 2px;
}
.list_container > div {
  border-bottom: 1px solid #ddd;
}
.list_container > div:hover .list-item {
  background-color: red;
}
.list_container > div:last-child {
  border: none;
}
.list_item:hover .list_item {
  background-color: #ddd;
}
.list_item a {
  text-decoration: none;
}
.list_item:hover {
  background-color: #fafafa;
}
.list_header > div,
.list_item > div {
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 7px;
  padding-right: 7px;
  line-height: 22px;
}
.list_header > div input,
.list_item > div input {
  margin-right: 7px;
  margin-left: 14px;
  vertical-align: text-bottom;
  line-height: 22px;
  position: relative;
  top: -1px;
}
.list_header > div .item_link,
.list_item > div .item_link {
  margin-left: -1px;
  vertical-align: baseline;
  line-height: 22px;
}
[dir="rtl"] .list_item > div input {
  margin-right: 0;
}
.new-file input[type=checkbox] {
  visibility: hidden;
}
.item_name {
  line-height: 22px;
  height: 24px;
}
.item_icon {
  font-size: 14px;
  color: #5e5e5e;
  margin-right: 7px;
  margin-left: 7px;
  line-height: 22px;
  vertical-align: baseline;
}
.item_modified {
  margin-right: 7px;
  margin-left: 7px;
}
[dir="rtl"] .item_modified.pull-right {
  float: left !important;
  float: left;
}
.item_buttons {
  line-height: 1em;
  margin-left: -5px;
}
.item_buttons .btn,
.item_buttons .btn-group,
.item_buttons .input-group {
  float: left;
}
.item_buttons > .btn,
.item_buttons > .btn-group,
.item_buttons > .input-group {
  margin-left: 5px;
}
.item_buttons .btn {
  min-width: 13ex;
}
.item_buttons .running-indicator {
  padding-top: 4px;
  color: #5cb85c;
}
.item_buttons .kernel-name {
  padding-top: 4px;
  color: #5bc0de;
  margin-right: 7px;
  float: left;
}
[dir="rtl"] .item_buttons.pull-right {
  float: left !important;
  float: left;
}
[dir="rtl"] .item_buttons .kernel-name {
  margin-left: 7px;
  float: right;
}
.toolbar_info {
  height: 24px;
  line-height: 24px;
}
.list_item input:not([type=checkbox]) {
  padding-top: 3px;
  padding-bottom: 3px;
  height: 22px;
  line-height: 14px;
  margin: 0px;
}
.highlight_text {
  color: blue;
}
#project_name {
  display: inline-block;
  padding-left: 7px;
  margin-left: -2px;
}
#project_name > .breadcrumb {
  padding: 0px;
  margin-bottom: 0px;
  background-color: transparent;
  font-weight: bold;
}
.sort_button {
  display: inline-block;
  padding-left: 7px;
}
[dir="rtl"] .sort_button.pull-right {
  float: left !important;
  float: left;
}
#tree-selector {
  padding-right: 0px;
}
#button-select-all {
  min-width: 50px;
}
[dir="rtl"] #button-select-all.btn {
  float: right ;
}
#select-all {
  margin-left: 7px;
  margin-right: 2px;
  margin-top: 2px;
  height: 16px;
}
[dir="rtl"] #select-all.pull-left {
  float: right !important;
  float: right;
}
.menu_icon {
  margin-right: 2px;
}
.tab-content .row {
  margin-left: 0px;
  margin-right: 0px;
}
.folder_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f114";
}
.folder_icon:before.fa-pull-left {
  margin-right: .3em;
}
.folder_icon:before.fa-pull-right {
  margin-left: .3em;
}
.folder_icon:before.pull-left {
  margin-right: .3em;
}
.folder_icon:before.pull-right {
  margin-left: .3em;
}
.notebook_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f02d";
  position: relative;
  top: -1px;
}
.notebook_icon:before.fa-pull-left {
  margin-right: .3em;
}
.notebook_icon:before.fa-pull-right {
  margin-left: .3em;
}
.notebook_icon:before.pull-left {
  margin-right: .3em;
}
.notebook_icon:before.pull-right {
  margin-left: .3em;
}
.running_notebook_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f02d";
  position: relative;
  top: -1px;
  color: #5cb85c;
}
.running_notebook_icon:before.fa-pull-left {
  margin-right: .3em;
}
.running_notebook_icon:before.fa-pull-right {
  margin-left: .3em;
}
.running_notebook_icon:before.pull-left {
  margin-right: .3em;
}
.running_notebook_icon:before.pull-right {
  margin-left: .3em;
}
.file_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f016";
  position: relative;
  top: -2px;
}
.file_icon:before.fa-pull-left {
  margin-right: .3em;
}
.file_icon:before.fa-pull-right {
  margin-left: .3em;
}
.file_icon:before.pull-left {
  margin-right: .3em;
}
.file_icon:before.pull-right {
  margin-left: .3em;
}
#notebook_toolbar .pull-right {
  padding-top: 0px;
  margin-right: -1px;
}
ul#new-menu {
  left: auto;
  right: 0;
}
#new-menu .dropdown-header {
  font-size: 10px;
  border-bottom: 1px solid #e5e5e5;
  padding: 0 0 3px;
  margin: -3px 20px 0;
}
.kernel-menu-icon {
  padding-right: 12px;
  width: 24px;
  content: "\f096";
}
.kernel-menu-icon:before {
  content: "\f096";
}
.kernel-menu-icon-current:before {
  content: "\f00c";
}
#tab_content {
  padding-top: 20px;
}
#running .panel-group .panel {
  margin-top: 3px;
  margin-bottom: 1em;
}
#running .panel-group .panel .panel-heading {
  background-color: #EEE;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 7px;
  padding-right: 7px;
  line-height: 22px;
}
#running .panel-group .panel .panel-heading a:focus,
#running .panel-group .panel .panel-heading a:hover {
  text-decoration: none;
}
#running .panel-group .panel .panel-body {
  padding: 0px;
}
#running .panel-group .panel .panel-body .list_container {
  margin-top: 0px;
  margin-bottom: 0px;
  border: 0px;
  border-radius: 0px;
}
#running .panel-group .panel .panel-body .list_container .list_item {
  border-bottom: 1px solid #ddd;
}
#running .panel-group .panel .panel-body .list_container .list_item:last-child {
  border-bottom: 0px;
}
.delete-button {
  display: none;
}
.duplicate-button {
  display: none;
}
.rename-button {
  display: none;
}
.move-button {
  display: none;
}
.download-button {
  display: none;
}
.shutdown-button {
  display: none;
}
.dynamic-instructions {
  display: inline-block;
  padding-top: 4px;
}
/*!
*
* IPython text editor webapp
*
*/
.selected-keymap i.fa {
  padding: 0px 5px;
}
.selected-keymap i.fa:before {
  content: "\f00c";
}
#mode-menu {
  overflow: auto;
  max-height: 20em;
}
.edit_app #header {
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
}
.edit_app #menubar .navbar {
  /* Use a negative 1 bottom margin, so the border overlaps the border of the
    header */
  margin-bottom: -1px;
}
.dirty-indicator {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 20px;
}
.dirty-indicator.fa-pull-left {
  margin-right: .3em;
}
.dirty-indicator.fa-pull-right {
  margin-left: .3em;
}
.dirty-indicator.pull-left {
  margin-right: .3em;
}
.dirty-indicator.pull-right {
  margin-left: .3em;
}
.dirty-indicator-dirty {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 20px;
}
.dirty-indicator-dirty.fa-pull-left {
  margin-right: .3em;
}
.dirty-indicator-dirty.fa-pull-right {
  margin-left: .3em;
}
.dirty-indicator-dirty.pull-left {
  margin-right: .3em;
}
.dirty-indicator-dirty.pull-right {
  margin-left: .3em;
}
.dirty-indicator-clean {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 20px;
}
.dirty-indicator-clean.fa-pull-left {
  margin-right: .3em;
}
.dirty-indicator-clean.fa-pull-right {
  margin-left: .3em;
}
.dirty-indicator-clean.pull-left {
  margin-right: .3em;
}
.dirty-indicator-clean.pull-right {
  margin-left: .3em;
}
.dirty-indicator-clean:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f00c";
}
.dirty-indicator-clean:before.fa-pull-left {
  margin-right: .3em;
}
.dirty-indicator-clean:before.fa-pull-right {
  margin-left: .3em;
}
.dirty-indicator-clean:before.pull-left {
  margin-right: .3em;
}
.dirty-indicator-clean:before.pull-right {
  margin-left: .3em;
}
#filename {
  font-size: 16pt;
  display: table;
  padding: 0px 5px;
}
#current-mode {
  padding-left: 5px;
  padding-right: 5px;
}
#texteditor-backdrop {
  padding-top: 20px;
  padding-bottom: 20px;
}
@media not print {
  #texteditor-backdrop {
    background-color: #EEE;
  }
}
@media print {
  #texteditor-backdrop #texteditor-container .CodeMirror-gutter,
  #texteditor-backdrop #texteditor-container .CodeMirror-gutters {
    background-color: #fff;
  }
}
@media not print {
  #texteditor-backdrop #texteditor-container .CodeMirror-gutter,
  #texteditor-backdrop #texteditor-container .CodeMirror-gutters {
    background-color: #fff;
  }
}
@media not print {
  #texteditor-backdrop #texteditor-container {
    padding: 0px;
    background-color: #fff;
    -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
    box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  }
}
.CodeMirror-dialog {
  background-color: #fff;
}
/*!
*
* IPython notebook
*
*/
/* CSS font colors for translated ANSI escape sequences */
/* The color values are a mix of
   http://www.xcolors.net/dl/baskerville-ivorylight and
   http://www.xcolors.net/dl/euphrasia */
.ansi-black-fg {
  color: #3E424D;
}
.ansi-black-bg {
  background-color: #3E424D;
}
.ansi-black-intense-fg {
  color: #282C36;
}
.ansi-black-intense-bg {
  background-color: #282C36;
}
.ansi-red-fg {
  color: #E75C58;
}
.ansi-red-bg {
  background-color: #E75C58;
}
.ansi-red-intense-fg {
  color: #B22B31;
}
.ansi-red-intense-bg {
  background-color: #B22B31;
}
.ansi-green-fg {
  color: #00A250;
}
.ansi-green-bg {
  background-color: #00A250;
}
.ansi-green-intense-fg {
  color: #007427;
}
.ansi-green-intense-bg {
  background-color: #007427;
}
.ansi-yellow-fg {
  color: #DDB62B;
}
.ansi-yellow-bg {
  background-color: #DDB62B;
}
.ansi-yellow-intense-fg {
  color: #B27D12;
}
.ansi-yellow-intense-bg {
  background-color: #B27D12;
}
.ansi-blue-fg {
  color: #208FFB;
}
.ansi-blue-bg {
  background-color: #208FFB;
}
.ansi-blue-intense-fg {
  color: #0065CA;
}
.ansi-blue-intense-bg {
  background-color: #0065CA;
}
.ansi-magenta-fg {
  color: #D160C4;
}
.ansi-magenta-bg {
  background-color: #D160C4;
}
.ansi-magenta-intense-fg {
  color: #A03196;
}
.ansi-magenta-intense-bg {
  background-color: #A03196;
}
.ansi-cyan-fg {
  color: #60C6C8;
}
.ansi-cyan-bg {
  background-color: #60C6C8;
}
.ansi-cyan-intense-fg {
  color: #258F8F;
}
.ansi-cyan-intense-bg {
  background-color: #258F8F;
}
.ansi-white-fg {
  color: #C5C1B4;
}
.ansi-white-bg {
  background-color: #C5C1B4;
}
.ansi-white-intense-fg {
  color: #A1A6B2;
}
.ansi-white-intense-bg {
  background-color: #A1A6B2;
}
.ansi-default-inverse-fg {
  color: #FFFFFF;
}
.ansi-default-inverse-bg {
  background-color: #000000;
}
.ansi-bold {
  font-weight: bold;
}
.ansi-underline {
  text-decoration: underline;
}
/* The following styles are deprecated an will be removed in a future version */
.ansibold {
  font-weight: bold;
}
.ansi-inverse {
  outline: 0.5px dotted;
}
/* use dark versions for foreground, to improve visibility */
.ansiblack {
  color: black;
}
.ansired {
  color: darkred;
}
.ansigreen {
  color: darkgreen;
}
.ansiyellow {
  color: #c4a000;
}
.ansiblue {
  color: darkblue;
}
.ansipurple {
  color: darkviolet;
}
.ansicyan {
  color: steelblue;
}
.ansigray {
  color: gray;
}
/* and light for background, for the same reason */
.ansibgblack {
  background-color: black;
}
.ansibgred {
  background-color: red;
}
.ansibggreen {
  background-color: green;
}
.ansibgyellow {
  background-color: yellow;
}
.ansibgblue {
  background-color: blue;
}
.ansibgpurple {
  background-color: magenta;
}
.ansibgcyan {
  background-color: cyan;
}
.ansibggray {
  background-color: gray;
}
div.cell {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  border-radius: 2px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  border-width: 1px;
  border-style: solid;
  border-color: transparent;
  width: 100%;
  padding: 5px;
  /* This acts as a spacer between cells, that is outside the border */
  margin: 0px;
  outline: none;
  position: relative;
  overflow: visible;
}
div.cell:before {
  position: absolute;
  display: block;
  top: -1px;
  left: -1px;
  width: 5px;
  height: calc(100% +  2px);
  content: '';
  background: transparent;
}
div.cell.jupyter-soft-selected {
  border-left-color: #E3F2FD;
  border-left-width: 1px;
  padding-left: 5px;
  border-right-color: #E3F2FD;
  border-right-width: 1px;
  background: #E3F2FD;
}
@media print {
  div.cell.jupyter-soft-selected {
    border-color: transparent;
  }
}
div.cell.selected,
div.cell.selected.jupyter-soft-selected {
  border-color: #ababab;
}
div.cell.selected:before,
div.cell.selected.jupyter-soft-selected:before {
  position: absolute;
  display: block;
  top: -1px;
  left: -1px;
  width: 5px;
  height: calc(100% +  2px);
  content: '';
  background: #42A5F5;
}
@media print {
  div.cell.selected,
  div.cell.selected.jupyter-soft-selected {
    border-color: transparent;
  }
}
.edit_mode div.cell.selected {
  border-color: #66BB6A;
}
.edit_mode div.cell.selected:before {
  position: absolute;
  display: block;
  top: -1px;
  left: -1px;
  width: 5px;
  height: calc(100% +  2px);
  content: '';
  background: #66BB6A;
}
@media print {
  .edit_mode div.cell.selected {
    border-color: transparent;
  }
}
.prompt {
  /* This needs to be wide enough for 3 digit prompt numbers: In[100]: */
  min-width: 14ex;
  /* This padding is tuned to match the padding on the CodeMirror editor. */
  padding: 0.4em;
  margin: 0px;
  font-family: monospace;
  text-align: right;
  /* This has to match that of the the CodeMirror class line-height below */
  line-height: 1.21429em;
  /* Don't highlight prompt number selection */
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  /* Use default cursor */
  cursor: default;
}
@media (max-width: 540px) {
  .prompt {
    text-align: left;
  }
}
div.inner_cell {
  min-width: 0;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
/* input_area and input_prompt must match in top border and margin for alignment */
div.input_area {
  border: 1px solid #cfcfcf;
  border-radius: 2px;
  background: #f7f7f7;
  line-height: 1.21429em;
}
/* This is needed so that empty prompt areas can collapse to zero height when there
   is no content in the output_subarea and the prompt. The main purpose of this is
   to make sure that empty JavaScript output_subareas have no height. */
div.prompt:empty {
  padding-top: 0;
  padding-bottom: 0;
}
div.unrecognized_cell {
  padding: 5px 5px 5px 0px;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
div.unrecognized_cell .inner_cell {
  border-radius: 2px;
  padding: 5px;
  font-weight: bold;
  color: red;
  border: 1px solid #cfcfcf;
  background: #eaeaea;
}
div.unrecognized_cell .inner_cell a {
  color: inherit;
  text-decoration: none;
}
div.unrecognized_cell .inner_cell a:hover {
  color: inherit;
  text-decoration: none;
}
@media (max-width: 540px) {
  div.unrecognized_cell > div.prompt {
    display: none;
  }
}
div.code_cell {
  /* avoid page breaking on code cells when printing */
}
@media print {
  div.code_cell {
    page-break-inside: avoid;
  }
}
/* any special styling for code cells that are currently running goes here */
div.input {
  page-break-inside: avoid;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
@media (max-width: 540px) {
  div.input {
    /* Old browsers */
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-box-align: stretch;
    display: -moz-box;
    -moz-box-orient: vertical;
    -moz-box-align: stretch;
    display: box;
    box-orient: vertical;
    box-align: stretch;
    /* Modern browsers */
    display: flex;
    flex-direction: column;
    align-items: stretch;
  }
}
/* input_area and input_prompt must match in top border and margin for alignment */
div.input_prompt {
  color: #303F9F;
  border-top: 1px solid transparent;
}
div.input_area > div.highlight {
  margin: 0.4em;
  border: none;
  padding: 0px;
  background-color: transparent;
}
div.input_area > div.highlight > pre {
  margin: 0px;
  border: none;
  padding: 0px;
  background-color: transparent;
}
/* The following gets added to the <head> if it is detected that the user has a
 * monospace font with inconsistent normal/bold/italic height.  See
 * notebookmain.js.  Such fonts will have keywords vertically offset with
 * respect to the rest of the text.  The user should select a better font.
 * See: https://github.com/ipython/ipython/issues/1503
 *
 * .CodeMirror span {
 *      vertical-align: bottom;
 * }
 */
.CodeMirror {
  line-height: 1.21429em;
  /* Changed from 1em to our global default */
  font-size: 14px;
  height: auto;
  /* Changed to auto to autogrow */
  background: none;
  /* Changed from white to allow our bg to show through */
}
.CodeMirror-scroll {
  /*  The CodeMirror docs are a bit fuzzy on if overflow-y should be hidden or visible.*/
  /*  We have found that if it is visible, vertical scrollbars appear with font size changes.*/
  overflow-y: hidden;
  overflow-x: auto;
}
.CodeMirror-lines {
  /* In CM2, this used to be 0.4em, but in CM3 it went to 4px. We need the em value because */
  /* we have set a different line-height and want this to scale with that. */
  /* Note that this should set vertical padding only, since CodeMirror assumes
       that horizontal padding will be set on CodeMirror pre */
  padding: 0.4em 0;
}
.CodeMirror-linenumber {
  padding: 0 8px 0 4px;
}
.CodeMirror-gutters {
  border-bottom-left-radius: 2px;
  border-top-left-radius: 2px;
}
.CodeMirror pre {
  /* In CM3 this went to 4px from 0 in CM2. This sets horizontal padding only,
    use .CodeMirror-lines for vertical */
  padding: 0 0.4em;
  border: 0;
  border-radius: 0;
}
.CodeMirror-cursor {
  border-left: 1.4px solid black;
}
@media screen and (min-width: 2138px) and (max-width: 4319px) {
  .CodeMirror-cursor {
    border-left: 2px solid black;
  }
}
@media screen and (min-width: 4320px) {
  .CodeMirror-cursor {
    border-left: 4px solid black;
  }
}
/*

Original style from softwaremaniacs.org (c) Ivan Sagalaev <Maniac@SoftwareManiacs.Org>
Adapted from GitHub theme

*/
.highlight-base {
  color: #000;
}
.highlight-variable {
  color: #000;
}
.highlight-variable-2 {
  color: #1a1a1a;
}
.highlight-variable-3 {
  color: #333333;
}
.highlight-string {
  color: #BA2121;
}
.highlight-comment {
  color: #408080;
  font-style: italic;
}
.highlight-number {
  color: #080;
}
.highlight-atom {
  color: #88F;
}
.highlight-keyword {
  color: #008000;
  font-weight: bold;
}
.highlight-builtin {
  color: #008000;
}
.highlight-error {
  color: #f00;
}
.highlight-operator {
  color: #AA22FF;
  font-weight: bold;
}
.highlight-meta {
  color: #AA22FF;
}
/* previously not defined, copying from default codemirror */
.highlight-def {
  color: #00f;
}
.highlight-string-2 {
  color: #f50;
}
.highlight-qualifier {
  color: #555;
}
.highlight-bracket {
  color: #997;
}
.highlight-tag {
  color: #170;
}
.highlight-attribute {
  color: #00c;
}
.highlight-header {
  color: blue;
}
.highlight-quote {
  color: #090;
}
.highlight-link {
  color: #00c;
}
/* apply the same style to codemirror */
.cm-s-ipython span.cm-keyword {
  color: #008000;
  font-weight: bold;
}
.cm-s-ipython span.cm-atom {
  color: #88F;
}
.cm-s-ipython span.cm-number {
  color: #080;
}
.cm-s-ipython span.cm-def {
  color: #00f;
}
.cm-s-ipython span.cm-variable {
  color: #000;
}
.cm-s-ipython span.cm-operator {
  color: #AA22FF;
  font-weight: bold;
}
.cm-s-ipython span.cm-variable-2 {
  color: #1a1a1a;
}
.cm-s-ipython span.cm-variable-3 {
  color: #333333;
}
.cm-s-ipython span.cm-comment {
  color: #408080;
  font-style: italic;
}
.cm-s-ipython span.cm-string {
  color: #BA2121;
}
.cm-s-ipython span.cm-string-2 {
  color: #f50;
}
.cm-s-ipython span.cm-meta {
  color: #AA22FF;
}
.cm-s-ipython span.cm-qualifier {
  color: #555;
}
.cm-s-ipython span.cm-builtin {
  color: #008000;
}
.cm-s-ipython span.cm-bracket {
  color: #997;
}
.cm-s-ipython span.cm-tag {
  color: #170;
}
.cm-s-ipython span.cm-attribute {
  color: #00c;
}
.cm-s-ipython span.cm-header {
  color: blue;
}
.cm-s-ipython span.cm-quote {
  color: #090;
}
.cm-s-ipython span.cm-link {
  color: #00c;
}
.cm-s-ipython span.cm-error {
  color: #f00;
}
.cm-s-ipython span.cm-tab {
  background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAMCAYAAAAkuj5RAAAAAXNSR0IArs4c6QAAAGFJREFUSMft1LsRQFAQheHPowAKoACx3IgEKtaEHujDjORSgWTH/ZOdnZOcM/sgk/kFFWY0qV8foQwS4MKBCS3qR6ixBJvElOobYAtivseIE120FaowJPN75GMu8j/LfMwNjh4HUpwg4LUAAAAASUVORK5CYII=);
  background-position: right;
  background-repeat: no-repeat;
}
div.output_wrapper {
  /* this position must be relative to enable descendents to be absolute within it */
  position: relative;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  z-index: 1;
}
/* class for the output area when it should be height-limited */
div.output_scroll {
  /* ideally, this would be max-height, but FF barfs all over that */
  height: 24em;
  /* FF needs this *and the wrapper* to specify full width, or it will shrinkwrap */
  width: 100%;
  overflow: auto;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 2px 8px rgba(0, 0, 0, 0.8);
  box-shadow: inset 0 2px 8px rgba(0, 0, 0, 0.8);
  display: block;
}
/* output div while it is collapsed */
div.output_collapsed {
  margin: 0px;
  padding: 0px;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
}
div.out_prompt_overlay {
  height: 100%;
  padding: 0px 0.4em;
  position: absolute;
  border-radius: 2px;
}
div.out_prompt_overlay:hover {
  /* use inner shadow to get border that is computed the same on WebKit/FF */
  -webkit-box-shadow: inset 0 0 1px #000;
  box-shadow: inset 0 0 1px #000;
  background: rgba(240, 240, 240, 0.5);
}
div.output_prompt {
  color: #D84315;
}
/* This class is the outer container of all output sections. */
div.output_area {
  padding: 0px;
  page-break-inside: avoid;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
div.output_area .MathJax_Display {
  text-align: left !important;
}
div.output_area .rendered_html table {
  margin-left: 0;
  margin-right: 0;
}
div.output_area .rendered_html img {
  margin-left: 0;
  margin-right: 0;
}
div.output_area img,
div.output_area svg {
  max-width: 100%;
  height: auto;
}
div.output_area img.unconfined,
div.output_area svg.unconfined {
  max-width: none;
}
div.output_area .mglyph > img {
  max-width: none;
}
/* This is needed to protect the pre formating from global settings such
   as that of bootstrap */
.output {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
}
@media (max-width: 540px) {
  div.output_area {
    /* Old browsers */
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-box-align: stretch;
    display: -moz-box;
    -moz-box-orient: vertical;
    -moz-box-align: stretch;
    display: box;
    box-orient: vertical;
    box-align: stretch;
    /* Modern browsers */
    display: flex;
    flex-direction: column;
    align-items: stretch;
  }
}
div.output_area pre {
  margin: 0;
  padding: 1px 0 1px 0;
  border: 0;
  vertical-align: baseline;
  color: black;
  background-color: transparent;
  border-radius: 0;
}
/* This class is for the output subarea inside the output_area and after
   the prompt div. */
div.output_subarea {
  overflow-x: auto;
  padding: 0.4em;
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
  max-width: calc(100% - 14ex);
}
div.output_scroll div.output_subarea {
  overflow-x: visible;
}
/* The rest of the output_* classes are for special styling of the different
   output types */
/* all text output has this class: */
div.output_text {
  text-align: left;
  color: #000;
  /* This has to match that of the the CodeMirror class line-height below */
  line-height: 1.21429em;
}
/* stdout/stderr are 'text' as well as 'stream', but execute_result/error are *not* streams */
div.output_stderr {
  background: #fdd;
  /* very light red background for stderr */
}
div.output_latex {
  text-align: left;
}
/* Empty output_javascript divs should have no height */
div.output_javascript:empty {
  padding: 0;
}
.js-error {
  color: darkred;
}
/* raw_input styles */
div.raw_input_container {
  line-height: 1.21429em;
  padding-top: 5px;
}
pre.raw_input_prompt {
  /* nothing needed here. */
}
input.raw_input {
  font-family: monospace;
  font-size: inherit;
  color: inherit;
  width: auto;
  /* make sure input baseline aligns with prompt */
  vertical-align: baseline;
  /* padding + margin = 0.5em between prompt and cursor */
  padding: 0em 0.25em;
  margin: 0em 0.25em;
}
input.raw_input:focus {
  box-shadow: none;
}
p.p-space {
  margin-bottom: 10px;
}
div.output_unrecognized {
  padding: 5px;
  font-weight: bold;
  color: red;
}
div.output_unrecognized a {
  color: inherit;
  text-decoration: none;
}
div.output_unrecognized a:hover {
  color: inherit;
  text-decoration: none;
}
.rendered_html {
  color: #000;
  /* any extras will just be numbers: */
}
.rendered_html em {
  font-style: italic;
}
.rendered_html strong {
  font-weight: bold;
}
.rendered_html u {
  text-decoration: underline;
}
.rendered_html :link {
  text-decoration: underline;
}
.rendered_html :visited {
  text-decoration: underline;
}
.rendered_html h1 {
  font-size: 185.7%;
  margin: 1.08em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h2 {
  font-size: 157.1%;
  margin: 1.27em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h3 {
  font-size: 128.6%;
  margin: 1.55em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h4 {
  font-size: 100%;
  margin: 2em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h5 {
  font-size: 100%;
  margin: 2em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
  font-style: italic;
}
.rendered_html h6 {
  font-size: 100%;
  margin: 2em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
  font-style: italic;
}
.rendered_html h1:first-child {
  margin-top: 0.538em;
}
.rendered_html h2:first-child {
  margin-top: 0.636em;
}
.rendered_html h3:first-child {
  margin-top: 0.777em;
}
.rendered_html h4:first-child {
  margin-top: 1em;
}
.rendered_html h5:first-child {
  margin-top: 1em;
}
.rendered_html h6:first-child {
  margin-top: 1em;
}
.rendered_html ul:not(.list-inline),
.rendered_html ol:not(.list-inline) {
  padding-left: 2em;
}
.rendered_html ul {
  list-style: disc;
}
.rendered_html ul ul {
  list-style: square;
  margin-top: 0;
}
.rendered_html ul ul ul {
  list-style: circle;
}
.rendered_html ol {
  list-style: decimal;
}
.rendered_html ol ol {
  list-style: upper-alpha;
  margin-top: 0;
}
.rendered_html ol ol ol {
  list-style: lower-alpha;
}
.rendered_html ol ol ol ol {
  list-style: lower-roman;
}
.rendered_html ol ol ol ol ol {
  list-style: decimal;
}
.rendered_html * + ul {
  margin-top: 1em;
}
.rendered_html * + ol {
  margin-top: 1em;
}
.rendered_html hr {
  color: black;
  background-color: black;
}
.rendered_html pre {
  margin: 1em 2em;
  padding: 0px;
  background-color: #fff;
}
.rendered_html code {
  background-color: #eff0f1;
}
.rendered_html p code {
  padding: 1px 5px;
}
.rendered_html pre code {
  background-color: #fff;
}
.rendered_html pre,
.rendered_html code {
  border: 0;
  color: #000;
  font-size: 100%;
}
.rendered_html blockquote {
  margin: 1em 2em;
}
.rendered_html table {
  margin-left: auto;
  margin-right: auto;
  border: none;
  border-collapse: collapse;
  border-spacing: 0;
  color: black;
  font-size: 12px;
  table-layout: fixed;
}
.rendered_html thead {
  border-bottom: 1px solid black;
  vertical-align: bottom;
}
.rendered_html tr,
.rendered_html th,
.rendered_html td {
  text-align: right;
  vertical-align: middle;
  padding: 0.5em 0.5em;
  line-height: normal;
  white-space: normal;
  max-width: none;
  border: none;
}
.rendered_html th {
  font-weight: bold;
}
.rendered_html tbody tr:nth-child(odd) {
  background: #f5f5f5;
}
.rendered_html tbody tr:hover {
  background: rgba(66, 165, 245, 0.2);
}
.rendered_html * + table {
  margin-top: 1em;
}
.rendered_html p {
  text-align: left;
}
.rendered_html * + p {
  margin-top: 1em;
}
.rendered_html img {
  display: block;
  margin-left: auto;
  margin-right: auto;
}
.rendered_html * + img {
  margin-top: 1em;
}
.rendered_html img,
.rendered_html svg {
  max-width: 100%;
  height: auto;
}
.rendered_html img.unconfined,
.rendered_html svg.unconfined {
  max-width: none;
}
.rendered_html .alert {
  margin-bottom: initial;
}
.rendered_html * + .alert {
  margin-top: 1em;
}
[dir="rtl"] .rendered_html p {
  text-align: right;
}
div.text_cell {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
@media (max-width: 540px) {
  div.text_cell > div.prompt {
    display: none;
  }
}
div.text_cell_render {
  /*font-family: "Helvetica Neue", Arial, Helvetica, Geneva, sans-serif;*/
  outline: none;
  resize: none;
  width: inherit;
  border-style: none;
  padding: 0.5em 0.5em 0.5em 0.4em;
  color: #000;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
a.anchor-link:link {
  text-decoration: none;
  padding: 0px 20px;
  visibility: hidden;
}
h1:hover .anchor-link,
h2:hover .anchor-link,
h3:hover .anchor-link,
h4:hover .anchor-link,
h5:hover .anchor-link,
h6:hover .anchor-link {
  visibility: visible;
}
.text_cell.rendered .input_area {
  display: none;
}
.text_cell.rendered .rendered_html {
  overflow-x: auto;
  overflow-y: hidden;
}
.text_cell.rendered .rendered_html tr,
.text_cell.rendered .rendered_html th,
.text_cell.rendered .rendered_html td {
  max-width: none;
}
.text_cell.unrendered .text_cell_render {
  display: none;
}
.text_cell .dropzone .input_area {
  border: 2px dashed #bababa;
  margin: -1px;
}
.cm-header-1,
.cm-header-2,
.cm-header-3,
.cm-header-4,
.cm-header-5,
.cm-header-6 {
  font-weight: bold;
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
}
.cm-header-1 {
  font-size: 185.7%;
}
.cm-header-2 {
  font-size: 157.1%;
}
.cm-header-3 {
  font-size: 128.6%;
}
.cm-header-4 {
  font-size: 110%;
}
.cm-header-5 {
  font-size: 100%;
  font-style: italic;
}
.cm-header-6 {
  font-size: 100%;
  font-style: italic;
}
/*!
*
* IPython notebook webapp
*
*/
@media (max-width: 767px) {
  .notebook_app {
    padding-left: 0px;
    padding-right: 0px;
  }
}
#ipython-main-app {
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  height: 100%;
}
div#notebook_panel {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  height: 100%;
}
div#notebook {
  font-size: 14px;
  line-height: 20px;
  overflow-y: hidden;
  overflow-x: auto;
  width: 100%;
  /* This spaces the page away from the edge of the notebook area */
  padding-top: 20px;
  margin: 0px;
  outline: none;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  min-height: 100%;
}
@media not print {
  #notebook-container {
    padding: 15px;
    background-color: #fff;
    min-height: 0;
    -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
    box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  }
}
@media print {
  #notebook-container {
    width: 100%;
  }
}
div.ui-widget-content {
  border: 1px solid #ababab;
  outline: none;
}
pre.dialog {
  background-color: #f7f7f7;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0.4em;
  padding-left: 2em;
}
p.dialog {
  padding: 0.2em;
}
/* Word-wrap output correctly.  This is the CSS3 spelling, though Firefox seems
   to not honor it correctly.  Webkit browsers (Chrome, rekonq, Safari) do.
 */
pre,
code,
kbd,
samp {
  white-space: pre-wrap;
}
#fonttest {
  font-family: monospace;
}
p {
  margin-bottom: 0;
}
.end_space {
  min-height: 100px;
  transition: height .2s ease;
}
.notebook_app > #header {
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
}
@media not print {
  .notebook_app {
    background-color: #EEE;
  }
}
kbd {
  border-style: solid;
  border-width: 1px;
  box-shadow: none;
  margin: 2px;
  padding-left: 2px;
  padding-right: 2px;
  padding-top: 1px;
  padding-bottom: 1px;
}
.jupyter-keybindings {
  padding: 1px;
  line-height: 24px;
  border-bottom: 1px solid gray;
}
.jupyter-keybindings input {
  margin: 0;
  padding: 0;
  border: none;
}
.jupyter-keybindings i {
  padding: 6px;
}
.well code {
  background-color: #ffffff;
  border-color: #ababab;
  border-width: 1px;
  border-style: solid;
  padding: 2px;
  padding-top: 1px;
  padding-bottom: 1px;
}
/* CSS for the cell toolbar */
.celltoolbar {
  border: thin solid #CFCFCF;
  border-bottom: none;
  background: #EEE;
  border-radius: 2px 2px 0px 0px;
  width: 100%;
  height: 29px;
  padding-right: 4px;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
  /* Old browsers */
  -webkit-box-pack: end;
  -moz-box-pack: end;
  box-pack: end;
  /* Modern browsers */
  justify-content: flex-end;
  display: -webkit-flex;
}
@media print {
  .celltoolbar {
    display: none;
  }
}
.ctb_hideshow {
  display: none;
  vertical-align: bottom;
}
/* ctb_show is added to the ctb_hideshow div to show the cell toolbar.
   Cell toolbars are only shown when the ctb_global_show class is also set.
*/
.ctb_global_show .ctb_show.ctb_hideshow {
  display: block;
}
.ctb_global_show .ctb_show + .input_area,
.ctb_global_show .ctb_show + div.text_cell_input,
.ctb_global_show .ctb_show ~ div.text_cell_render {
  border-top-right-radius: 0px;
  border-top-left-radius: 0px;
}
.ctb_global_show .ctb_show ~ div.text_cell_render {
  border: 1px solid #cfcfcf;
}
.celltoolbar {
  font-size: 87%;
  padding-top: 3px;
}
.celltoolbar select {
  display: block;
  width: 100%;
  height: 32px;
  padding: 6px 12px;
  font-size: 13px;
  line-height: 1.42857143;
  color: #555555;
  background-color: #fff;
  background-image: none;
  border: 1px solid #ccc;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  -webkit-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  -o-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
  width: inherit;
  font-size: inherit;
  height: 22px;
  padding: 0px;
  display: inline-block;
}
.celltoolbar select:focus {
  border-color: #66afe9;
  outline: 0;
  -webkit-box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
  box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
}
.celltoolbar select::-moz-placeholder {
  color: #999;
  opacity: 1;
}
.celltoolbar select:-ms-input-placeholder {
  color: #999;
}
.celltoolbar select::-webkit-input-placeholder {
  color: #999;
}
.celltoolbar select::-ms-expand {
  border: 0;
  background-color: transparent;
}
.celltoolbar select[disabled],
.celltoolbar select[readonly],
fieldset[disabled] .celltoolbar select {
  background-color: #eeeeee;
  opacity: 1;
}
.celltoolbar select[disabled],
fieldset[disabled] .celltoolbar select {
  cursor: not-allowed;
}
textarea.celltoolbar select {
  height: auto;
}
select.celltoolbar select {
  height: 30px;
  line-height: 30px;
}
textarea.celltoolbar select,
select[multiple].celltoolbar select {
  height: auto;
}
.celltoolbar label {
  margin-left: 5px;
  margin-right: 5px;
}
.tags_button_container {
  width: 100%;
  display: flex;
}
.tag-container {
  display: flex;
  flex-direction: row;
  flex-grow: 1;
  overflow: hidden;
  position: relative;
}
.tag-container > * {
  margin: 0 4px;
}
.remove-tag-btn {
  margin-left: 4px;
}
.tags-input {
  display: flex;
}
.cell-tag:last-child:after {
  content: "";
  position: absolute;
  right: 0;
  width: 40px;
  height: 100%;
  /* Fade to background color of cell toolbar */
  background: linear-gradient(to right, rgba(0, 0, 0, 0), #EEE);
}
.tags-input > * {
  margin-left: 4px;
}
.cell-tag,
.tags-input input,
.tags-input button {
  display: block;
  width: 100%;
  height: 32px;
  padding: 6px 12px;
  font-size: 13px;
  line-height: 1.42857143;
  color: #555555;
  background-color: #fff;
  background-image: none;
  border: 1px solid #ccc;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  -webkit-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  -o-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
  box-shadow: none;
  width: inherit;
  font-size: inherit;
  height: 22px;
  line-height: 22px;
  padding: 0px 4px;
  display: inline-block;
}
.cell-tag:focus,
.tags-input input:focus,
.tags-input button:focus {
  border-color: #66afe9;
  outline: 0;
  -webkit-box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
  box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
}
.cell-tag::-moz-placeholder,
.tags-input input::-moz-placeholder,
.tags-input button::-moz-placeholder {
  color: #999;
  opacity: 1;
}
.cell-tag:-ms-input-placeholder,
.tags-input input:-ms-input-placeholder,
.tags-input button:-ms-input-placeholder {
  color: #999;
}
.cell-tag::-webkit-input-placeholder,
.tags-input input::-webkit-input-placeholder,
.tags-input button::-webkit-input-placeholder {
  color: #999;
}
.cell-tag::-ms-expand,
.tags-input input::-ms-expand,
.tags-input button::-ms-expand {
  border: 0;
  background-color: transparent;
}
.cell-tag[disabled],
.tags-input input[disabled],
.tags-input button[disabled],
.cell-tag[readonly],
.tags-input input[readonly],
.tags-input button[readonly],
fieldset[disabled] .cell-tag,
fieldset[disabled] .tags-input input,
fieldset[disabled] .tags-input button {
  background-color: #eeeeee;
  opacity: 1;
}
.cell-tag[disabled],
.tags-input input[disabled],
.tags-input button[disabled],
fieldset[disabled] .cell-tag,
fieldset[disabled] .tags-input input,
fieldset[disabled] .tags-input button {
  cursor: not-allowed;
}
textarea.cell-tag,
textarea.tags-input input,
textarea.tags-input button {
  height: auto;
}
select.cell-tag,
select.tags-input input,
select.tags-input button {
  height: 30px;
  line-height: 30px;
}
textarea.cell-tag,
textarea.tags-input input,
textarea.tags-input button,
select[multiple].cell-tag,
select[multiple].tags-input input,
select[multiple].tags-input button {
  height: auto;
}
.cell-tag,
.tags-input button {
  padding: 0px 4px;
}
.cell-tag {
  background-color: #fff;
  white-space: nowrap;
}
.tags-input input[type=text]:focus {
  outline: none;
  box-shadow: none;
  border-color: #ccc;
}
.completions {
  position: absolute;
  z-index: 110;
  overflow: hidden;
  border: 1px solid #ababab;
  border-radius: 2px;
  -webkit-box-shadow: 0px 6px 10px -1px #adadad;
  box-shadow: 0px 6px 10px -1px #adadad;
  line-height: 1;
}
.completions select {
  background: white;
  outline: none;
  border: none;
  padding: 0px;
  margin: 0px;
  overflow: auto;
  font-family: monospace;
  font-size: 110%;
  color: #000;
  width: auto;
}
.completions select option.context {
  color: #286090;
}
#kernel_logo_widget .current_kernel_logo {
  display: none;
  margin-top: -1px;
  margin-bottom: -1px;
  width: 32px;
  height: 32px;
}
[dir="rtl"] #kernel_logo_widget {
  float: left !important;
  float: left;
}
.modal .modal-body .move-path {
  display: flex;
  flex-direction: row;
  justify-content: space;
  align-items: center;
}
.modal .modal-body .move-path .server-root {
  padding-right: 20px;
}
.modal .modal-body .move-path .path-input {
  flex: 1;
}
#menubar {
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  margin-top: 1px;
}
#menubar .navbar {
  border-top: 1px;
  border-radius: 0px 0px 2px 2px;
  margin-bottom: 0px;
}
#menubar .navbar-toggle {
  float: left;
  padding-top: 7px;
  padding-bottom: 7px;
  border: none;
}
#menubar .navbar-collapse {
  clear: left;
}
[dir="rtl"] #menubar .navbar-toggle {
  float: right;
}
[dir="rtl"] #menubar .navbar-collapse {
  clear: right;
}
[dir="rtl"] #menubar .navbar-nav {
  float: right;
}
[dir="rtl"] #menubar .nav {
  padding-right: 0px;
}
[dir="rtl"] #menubar .navbar-nav > li {
  float: right;
}
[dir="rtl"] #menubar .navbar-right {
  float: left !important;
}
[dir="rtl"] ul.dropdown-menu {
  text-align: right;
  left: auto;
}
[dir="rtl"] ul#new-menu.dropdown-menu {
  right: auto;
  left: 0;
}
.nav-wrapper {
  border-bottom: 1px solid #e7e7e7;
}
i.menu-icon {
  padding-top: 4px;
}
[dir="rtl"] i.menu-icon.pull-right {
  float: left !important;
  float: left;
}
ul#help_menu li a {
  overflow: hidden;
  padding-right: 2.2em;
}
ul#help_menu li a i {
  margin-right: -1.2em;
}
[dir="rtl"] ul#help_menu li a {
  padding-left: 2.2em;
}
[dir="rtl"] ul#help_menu li a i {
  margin-right: 0;
  margin-left: -1.2em;
}
[dir="rtl"] ul#help_menu li a i.pull-right {
  float: left !important;
  float: left;
}
.dropdown-submenu {
  position: relative;
}
.dropdown-submenu > .dropdown-menu {
  top: 0;
  left: 100%;
  margin-top: -6px;
  margin-left: -1px;
}
[dir="rtl"] .dropdown-submenu > .dropdown-menu {
  right: 100%;
  margin-right: -1px;
}
.dropdown-submenu:hover > .dropdown-menu {
  display: block;
}
.dropdown-submenu > a:after {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: block;
  content: "\f0da";
  float: right;
  color: #333333;
  margin-top: 2px;
  margin-right: -10px;
}
.dropdown-submenu > a:after.fa-pull-left {
  margin-right: .3em;
}
.dropdown-submenu > a:after.fa-pull-right {
  margin-left: .3em;
}
.dropdown-submenu > a:after.pull-left {
  margin-right: .3em;
}
.dropdown-submenu > a:after.pull-right {
  margin-left: .3em;
}
[dir="rtl"] .dropdown-submenu > a:after {
  float: left;
  content: "\f0d9";
  margin-right: 0;
  margin-left: -10px;
}
.dropdown-submenu:hover > a:after {
  color: #262626;
}
.dropdown-submenu.pull-left {
  float: none;
}
.dropdown-submenu.pull-left > .dropdown-menu {
  left: -100%;
  margin-left: 10px;
}
#notification_area {
  float: right !important;
  float: right;
  z-index: 10;
}
[dir="rtl"] #notification_area {
  float: left !important;
  float: left;
}
.indicator_area {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
}
[dir="rtl"] .indicator_area {
  float: left !important;
  float: left;
}
#kernel_indicator {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
  border-left: 1px solid;
}
#kernel_indicator .kernel_indicator_name {
  padding-left: 5px;
  padding-right: 5px;
}
[dir="rtl"] #kernel_indicator {
  float: left !important;
  float: left;
  border-left: 0;
  border-right: 1px solid;
}
#modal_indicator {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
}
[dir="rtl"] #modal_indicator {
  float: left !important;
  float: left;
}
#readonly-indicator {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
  margin-top: 2px;
  margin-bottom: 0px;
  margin-left: 0px;
  margin-right: 0px;
  display: none;
}
.modal_indicator:before {
  width: 1.28571429em;
  text-align: center;
}
.edit_mode .modal_indicator:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f040";
}
.edit_mode .modal_indicator:before.fa-pull-left {
  margin-right: .3em;
}
.edit_mode .modal_indicator:before.fa-pull-right {
  margin-left: .3em;
}
.edit_mode .modal_indicator:before.pull-left {
  margin-right: .3em;
}
.edit_mode .modal_indicator:before.pull-right {
  margin-left: .3em;
}
.command_mode .modal_indicator:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: ' ';
}
.command_mode .modal_indicator:before.fa-pull-left {
  margin-right: .3em;
}
.command_mode .modal_indicator:before.fa-pull-right {
  margin-left: .3em;
}
.command_mode .modal_indicator:before.pull-left {
  margin-right: .3em;
}
.command_mode .modal_indicator:before.pull-right {
  margin-left: .3em;
}
.kernel_idle_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f10c";
}
.kernel_idle_icon:before.fa-pull-left {
  margin-right: .3em;
}
.kernel_idle_icon:before.fa-pull-right {
  margin-left: .3em;
}
.kernel_idle_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_idle_icon:before.pull-right {
  margin-left: .3em;
}
.kernel_busy_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f111";
}
.kernel_busy_icon:before.fa-pull-left {
  margin-right: .3em;
}
.kernel_busy_icon:before.fa-pull-right {
  margin-left: .3em;
}
.kernel_busy_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_busy_icon:before.pull-right {
  margin-left: .3em;
}
.kernel_dead_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f1e2";
}
.kernel_dead_icon:before.fa-pull-left {
  margin-right: .3em;
}
.kernel_dead_icon:before.fa-pull-right {
  margin-left: .3em;
}
.kernel_dead_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_dead_icon:before.pull-right {
  margin-left: .3em;
}
.kernel_disconnected_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f127";
}
.kernel_disconnected_icon:before.fa-pull-left {
  margin-right: .3em;
}
.kernel_disconnected_icon:before.fa-pull-right {
  margin-left: .3em;
}
.kernel_disconnected_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_disconnected_icon:before.pull-right {
  margin-left: .3em;
}
.notification_widget {
  color: #777;
  z-index: 10;
  background: rgba(240, 240, 240, 0.5);
  margin-right: 4px;
  color: #333;
  background-color: #fff;
  border-color: #ccc;
}
.notification_widget:focus,
.notification_widget.focus {
  color: #333;
  background-color: #e6e6e6;
  border-color: #8c8c8c;
}
.notification_widget:hover {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.notification_widget:active,
.notification_widget.active,
.open > .dropdown-toggle.notification_widget {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.notification_widget:active:hover,
.notification_widget.active:hover,
.open > .dropdown-toggle.notification_widget:hover,
.notification_widget:active:focus,
.notification_widget.active:focus,
.open > .dropdown-toggle.notification_widget:focus,
.notification_widget:active.focus,
.notification_widget.active.focus,
.open > .dropdown-toggle.notification_widget.focus {
  color: #333;
  background-color: #d4d4d4;
  border-color: #8c8c8c;
}
.notification_widget:active,
.notification_widget.active,
.open > .dropdown-toggle.notification_widget {
  background-image: none;
}
.notification_widget.disabled:hover,
.notification_widget[disabled]:hover,
fieldset[disabled] .notification_widget:hover,
.notification_widget.disabled:focus,
.notification_widget[disabled]:focus,
fieldset[disabled] .notification_widget:focus,
.notification_widget.disabled.focus,
.notification_widget[disabled].focus,
fieldset[disabled] .notification_widget.focus {
  background-color: #fff;
  border-color: #ccc;
}
.notification_widget .badge {
  color: #fff;
  background-color: #333;
}
.notification_widget.warning {
  color: #fff;
  background-color: #f0ad4e;
  border-color: #eea236;
}
.notification_widget.warning:focus,
.notification_widget.warning.focus {
  color: #fff;
  background-color: #ec971f;
  border-color: #985f0d;
}
.notification_widget.warning:hover {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.notification_widget.warning:active,
.notification_widget.warning.active,
.open > .dropdown-toggle.notification_widget.warning {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.notification_widget.warning:active:hover,
.notification_widget.warning.active:hover,
.open > .dropdown-toggle.notification_widget.warning:hover,
.notification_widget.warning:active:focus,
.notification_widget.warning.active:focus,
.open > .dropdown-toggle.notification_widget.warning:focus,
.notification_widget.warning:active.focus,
.notification_widget.warning.active.focus,
.open > .dropdown-toggle.notification_widget.warning.focus {
  color: #fff;
  background-color: #d58512;
  border-color: #985f0d;
}
.notification_widget.warning:active,
.notification_widget.warning.active,
.open > .dropdown-toggle.notification_widget.warning {
  background-image: none;
}
.notification_widget.warning.disabled:hover,
.notification_widget.warning[disabled]:hover,
fieldset[disabled] .notification_widget.warning:hover,
.notification_widget.warning.disabled:focus,
.notification_widget.warning[disabled]:focus,
fieldset[disabled] .notification_widget.warning:focus,
.notification_widget.warning.disabled.focus,
.notification_widget.warning[disabled].focus,
fieldset[disabled] .notification_widget.warning.focus {
  background-color: #f0ad4e;
  border-color: #eea236;
}
.notification_widget.warning .badge {
  color: #f0ad4e;
  background-color: #fff;
}
.notification_widget.success {
  color: #fff;
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.notification_widget.success:focus,
.notification_widget.success.focus {
  color: #fff;
  background-color: #449d44;
  border-color: #255625;
}
.notification_widget.success:hover {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.notification_widget.success:active,
.notification_widget.success.active,
.open > .dropdown-toggle.notification_widget.success {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.notification_widget.success:active:hover,
.notification_widget.success.active:hover,
.open > .dropdown-toggle.notification_widget.success:hover,
.notification_widget.success:active:focus,
.notification_widget.success.active:focus,
.open > .dropdown-toggle.notification_widget.success:focus,
.notification_widget.success:active.focus,
.notification_widget.success.active.focus,
.open > .dropdown-toggle.notification_widget.success.focus {
  color: #fff;
  background-color: #398439;
  border-color: #255625;
}
.notification_widget.success:active,
.notification_widget.success.active,
.open > .dropdown-toggle.notification_widget.success {
  background-image: none;
}
.notification_widget.success.disabled:hover,
.notification_widget.success[disabled]:hover,
fieldset[disabled] .notification_widget.success:hover,
.notification_widget.success.disabled:focus,
.notification_widget.success[disabled]:focus,
fieldset[disabled] .notification_widget.success:focus,
.notification_widget.success.disabled.focus,
.notification_widget.success[disabled].focus,
fieldset[disabled] .notification_widget.success.focus {
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.notification_widget.success .badge {
  color: #5cb85c;
  background-color: #fff;
}
.notification_widget.info {
  color: #fff;
  background-color: #5bc0de;
  border-color: #46b8da;
}
.notification_widget.info:focus,
.notification_widget.info.focus {
  color: #fff;
  background-color: #31b0d5;
  border-color: #1b6d85;
}
.notification_widget.info:hover {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.notification_widget.info:active,
.notification_widget.info.active,
.open > .dropdown-toggle.notification_widget.info {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.notification_widget.info:active:hover,
.notification_widget.info.active:hover,
.open > .dropdown-toggle.notification_widget.info:hover,
.notification_widget.info:active:focus,
.notification_widget.info.active:focus,
.open > .dropdown-toggle.notification_widget.info:focus,
.notification_widget.info:active.focus,
.notification_widget.info.active.focus,
.open > .dropdown-toggle.notification_widget.info.focus {
  color: #fff;
  background-color: #269abc;
  border-color: #1b6d85;
}
.notification_widget.info:active,
.notification_widget.info.active,
.open > .dropdown-toggle.notification_widget.info {
  background-image: none;
}
.notification_widget.info.disabled:hover,
.notification_widget.info[disabled]:hover,
fieldset[disabled] .notification_widget.info:hover,
.notification_widget.info.disabled:focus,
.notification_widget.info[disabled]:focus,
fieldset[disabled] .notification_widget.info:focus,
.notification_widget.info.disabled.focus,
.notification_widget.info[disabled].focus,
fieldset[disabled] .notification_widget.info.focus {
  background-color: #5bc0de;
  border-color: #46b8da;
}
.notification_widget.info .badge {
  color: #5bc0de;
  background-color: #fff;
}
.notification_widget.danger {
  color: #fff;
  background-color: #d9534f;
  border-color: #d43f3a;
}
.notification_widget.danger:focus,
.notification_widget.danger.focus {
  color: #fff;
  background-color: #c9302c;
  border-color: #761c19;
}
.notification_widget.danger:hover {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.notification_widget.danger:active,
.notification_widget.danger.active,
.open > .dropdown-toggle.notification_widget.danger {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.notification_widget.danger:active:hover,
.notification_widget.danger.active:hover,
.open > .dropdown-toggle.notification_widget.danger:hover,
.notification_widget.danger:active:focus,
.notification_widget.danger.active:focus,
.open > .dropdown-toggle.notification_widget.danger:focus,
.notification_widget.danger:active.focus,
.notification_widget.danger.active.focus,
.open > .dropdown-toggle.notification_widget.danger.focus {
  color: #fff;
  background-color: #ac2925;
  border-color: #761c19;
}
.notification_widget.danger:active,
.notification_widget.danger.active,
.open > .dropdown-toggle.notification_widget.danger {
  background-image: none;
}
.notification_widget.danger.disabled:hover,
.notification_widget.danger[disabled]:hover,
fieldset[disabled] .notification_widget.danger:hover,
.notification_widget.danger.disabled:focus,
.notification_widget.danger[disabled]:focus,
fieldset[disabled] .notification_widget.danger:focus,
.notification_widget.danger.disabled.focus,
.notification_widget.danger[disabled].focus,
fieldset[disabled] .notification_widget.danger.focus {
  background-color: #d9534f;
  border-color: #d43f3a;
}
.notification_widget.danger .badge {
  color: #d9534f;
  background-color: #fff;
}
div#pager {
  background-color: #fff;
  font-size: 14px;
  line-height: 20px;
  overflow: hidden;
  display: none;
  position: fixed;
  bottom: 0px;
  width: 100%;
  max-height: 50%;
  padding-top: 8px;
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  /* Display over codemirror */
  z-index: 100;
  /* Hack which prevents jquery ui resizable from changing top. */
  top: auto !important;
}
div#pager pre {
  line-height: 1.21429em;
  color: #000;
  background-color: #f7f7f7;
  padding: 0.4em;
}
div#pager #pager-button-area {
  position: absolute;
  top: 8px;
  right: 20px;
}
div#pager #pager-contents {
  position: relative;
  overflow: auto;
  width: 100%;
  height: 100%;
}
div#pager #pager-contents #pager-container {
  position: relative;
  padding: 15px 0px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
div#pager .ui-resizable-handle {
  top: 0px;
  height: 8px;
  background: #f7f7f7;
  border-top: 1px solid #cfcfcf;
  border-bottom: 1px solid #cfcfcf;
  /* This injects handle bars (a short, wide = symbol) for 
        the resize handle. */
}
div#pager .ui-resizable-handle::after {
  content: '';
  top: 2px;
  left: 50%;
  height: 3px;
  width: 30px;
  margin-left: -15px;
  position: absolute;
  border-top: 1px solid #cfcfcf;
}
.quickhelp {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
  line-height: 1.8em;
}
.shortcut_key {
  display: inline-block;
  width: 21ex;
  text-align: right;
  font-family: monospace;
}
.shortcut_descr {
  display: inline-block;
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
span.save_widget {
  height: 30px;
  margin-top: 4px;
  display: flex;
  justify-content: flex-start;
  align-items: baseline;
  width: 50%;
  flex: 1;
}
span.save_widget span.filename {
  height: 100%;
  line-height: 1em;
  margin-left: 16px;
  border: none;
  font-size: 146.5%;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
  border-radius: 2px;
}
span.save_widget span.filename:hover {
  background-color: #e6e6e6;
}
[dir="rtl"] span.save_widget.pull-left {
  float: right !important;
  float: right;
}
[dir="rtl"] span.save_widget span.filename {
  margin-left: 0;
  margin-right: 16px;
}
span.checkpoint_status,
span.autosave_status {
  font-size: small;
  white-space: nowrap;
  padding: 0 5px;
}
@media (max-width: 767px) {
  span.save_widget {
    font-size: small;
    padding: 0 0 0 5px;
  }
  span.checkpoint_status,
  span.autosave_status {
    display: none;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  span.checkpoint_status {
    display: none;
  }
  span.autosave_status {
    font-size: x-small;
  }
}
.toolbar {
  padding: 0px;
  margin-left: -5px;
  margin-top: 2px;
  margin-bottom: 5px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
.toolbar select,
.toolbar label {
  width: auto;
  vertical-align: middle;
  margin-right: 2px;
  margin-bottom: 0px;
  display: inline;
  font-size: 92%;
  margin-left: 0.3em;
  margin-right: 0.3em;
  padding: 0px;
  padding-top: 3px;
}
.toolbar .btn {
  padding: 2px 8px;
}
.toolbar .btn-group {
  margin-top: 0px;
  margin-left: 5px;
}
.toolbar-btn-label {
  margin-left: 6px;
}
#maintoolbar {
  margin-bottom: -3px;
  margin-top: -8px;
  border: 0px;
  min-height: 27px;
  margin-left: 0px;
  padding-top: 11px;
  padding-bottom: 3px;
}
#maintoolbar .navbar-text {
  float: none;
  vertical-align: middle;
  text-align: right;
  margin-left: 5px;
  margin-right: 0px;
  margin-top: 0px;
}
.select-xs {
  height: 24px;
}
[dir="rtl"] .btn-group > .btn,
.btn-group-vertical > .btn {
  float: right;
}
.pulse,
.dropdown-menu > li > a.pulse,
li.pulse > a.dropdown-toggle,
li.pulse.open > a.dropdown-toggle {
  background-color: #F37626;
  color: white;
}
/**
 * Primary styles
 *
 * Author: Jupyter Development Team
 */
/** WARNING IF YOU ARE EDITTING THIS FILE, if this is a .css file, It has a lot
 * of chance of beeing generated from the ../less/[samename].less file, you can
 * try to get back the less file by reverting somme commit in history
 **/
/*
 * We'll try to get something pretty, so we
 * have some strange css to have the scroll bar on
 * the left with fix button on the top right of the tooltip
 */
@-moz-keyframes fadeOut {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
@-webkit-keyframes fadeOut {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
@-moz-keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
@-webkit-keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
/*properties of tooltip after "expand"*/
.bigtooltip {
  overflow: auto;
  height: 200px;
  -webkit-transition-property: height;
  -webkit-transition-duration: 500ms;
  -moz-transition-property: height;
  -moz-transition-duration: 500ms;
  transition-property: height;
  transition-duration: 500ms;
}
/*properties of tooltip before "expand"*/
.smalltooltip {
  -webkit-transition-property: height;
  -webkit-transition-duration: 500ms;
  -moz-transition-property: height;
  -moz-transition-duration: 500ms;
  transition-property: height;
  transition-duration: 500ms;
  text-overflow: ellipsis;
  overflow: hidden;
  height: 80px;
}
.tooltipbuttons {
  position: absolute;
  padding-right: 15px;
  top: 0px;
  right: 0px;
}
.tooltiptext {
  /*avoid the button to overlap on some docstring*/
  padding-right: 30px;
}
.ipython_tooltip {
  max-width: 700px;
  /*fade-in animation when inserted*/
  -webkit-animation: fadeOut 400ms;
  -moz-animation: fadeOut 400ms;
  animation: fadeOut 400ms;
  -webkit-animation: fadeIn 400ms;
  -moz-animation: fadeIn 400ms;
  animation: fadeIn 400ms;
  vertical-align: middle;
  background-color: #f7f7f7;
  overflow: visible;
  border: #ababab 1px solid;
  outline: none;
  padding: 3px;
  margin: 0px;
  padding-left: 7px;
  font-family: monospace;
  min-height: 50px;
  -moz-box-shadow: 0px 6px 10px -1px #adadad;
  -webkit-box-shadow: 0px 6px 10px -1px #adadad;
  box-shadow: 0px 6px 10px -1px #adadad;
  border-radius: 2px;
  position: absolute;
  z-index: 1000;
}
.ipython_tooltip a {
  float: right;
}
.ipython_tooltip .tooltiptext pre {
  border: 0;
  border-radius: 0;
  font-size: 100%;
  background-color: #f7f7f7;
}
.pretooltiparrow {
  left: 0px;
  margin: 0px;
  top: -16px;
  width: 40px;
  height: 16px;
  overflow: hidden;
  position: absolute;
}
.pretooltiparrow:before {
  background-color: #f7f7f7;
  border: 1px #ababab solid;
  z-index: 11;
  content: "";
  position: absolute;
  left: 15px;
  top: 10px;
  width: 25px;
  height: 25px;
  -webkit-transform: rotate(45deg);
  -moz-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  -o-transform: rotate(45deg);
}
ul.typeahead-list i {
  margin-left: -10px;
  width: 18px;
}
[dir="rtl"] ul.typeahead-list i {
  margin-left: 0;
  margin-right: -10px;
}
ul.typeahead-list {
  max-height: 80vh;
  overflow: auto;
}
ul.typeahead-list > li > a {
  /** Firefox bug **/
  /* see https://github.com/jupyter/notebook/issues/559 */
  white-space: normal;
}
ul.typeahead-list  > li > a.pull-right {
  float: left !important;
  float: left;
}
[dir="rtl"] .typeahead-list {
  text-align: right;
}
.cmd-palette .modal-body {
  padding: 7px;
}
.cmd-palette form {
  background: white;
}
.cmd-palette input {
  outline: none;
}
.no-shortcut {
  min-width: 20px;
  color: transparent;
}
[dir="rtl"] .no-shortcut.pull-right {
  float: left !important;
  float: left;
}
[dir="rtl"] .command-shortcut.pull-right {
  float: left !important;
  float: left;
}
.command-shortcut:before {
  content: "(command mode)";
  padding-right: 3px;
  color: #777777;
}
.edit-shortcut:before {
  content: "(edit)";
  padding-right: 3px;
  color: #777777;
}
[dir="rtl"] .edit-shortcut.pull-right {
  float: left !important;
  float: left;
}
#find-and-replace #replace-preview .match,
#find-and-replace #replace-preview .insert {
  background-color: #BBDEFB;
  border-color: #90CAF9;
  border-style: solid;
  border-width: 1px;
  border-radius: 0px;
}
[dir="ltr"] #find-and-replace .input-group-btn + .form-control {
  border-left: none;
}
[dir="rtl"] #find-and-replace .input-group-btn + .form-control {
  border-right: none;
}
#find-and-replace #replace-preview .replace .match {
  background-color: #FFCDD2;
  border-color: #EF9A9A;
  border-radius: 0px;
}
#find-and-replace #replace-preview .replace .insert {
  background-color: #C8E6C9;
  border-color: #A5D6A7;
  border-radius: 0px;
}
#find-and-replace #replace-preview {
  max-height: 60vh;
  overflow: auto;
}
#find-and-replace #replace-preview pre {
  padding: 5px 10px;
}
.terminal-app {
  background: #EEE;
}
.terminal-app #header {
  background: #fff;
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
}
.terminal-app .terminal {
  width: 100%;
  float: left;
  font-family: monospace;
  color: white;
  background: black;
  padding: 0.4em;
  border-radius: 2px;
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.4);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.4);
}
.terminal-app .terminal,
.terminal-app .terminal dummy-screen {
  line-height: 1em;
  font-size: 14px;
}
.terminal-app .terminal .xterm-rows {
  padding: 10px;
}
.terminal-app .terminal-cursor {
  color: black;
  background: white;
}
.terminal-app #terminado-container {
  margin-top: 20px;
}
/*# sourceMappingURL=style.min.css.map */
    </style>
<style type="text/css">
    .highlight .hll { background-color: #ffffcc }
.highlight  { background: #f8f8f8; }
.highlight .c { color: #408080; font-style: italic } /* Comment */
.highlight .err { border: 1px solid #FF0000 } /* Error */
.highlight .k { color: #008000; font-weight: bold } /* Keyword */
.highlight .o { color: #666666 } /* Operator */
.highlight .ch { color: #408080; font-style: italic } /* Comment.Hashbang */
.highlight .cm { color: #408080; font-style: italic } /* Comment.Multiline */
.highlight .cp { color: #BC7A00 } /* Comment.Preproc */
.highlight .cpf { color: #408080; font-style: italic } /* Comment.PreprocFile */
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
.highlight .sa { color: #BA2121 } /* Literal.String.Affix */
.highlight .sb { color: #BA2121 } /* Literal.String.Backtick */
.highlight .sc { color: #BA2121 } /* Literal.String.Char */
.highlight .dl { color: #BA2121 } /* Literal.String.Delimiter */
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
.highlight .fm { color: #0000FF } /* Name.Function.Magic */
.highlight .vc { color: #19177C } /* Name.Variable.Class */
.highlight .vg { color: #19177C } /* Name.Variable.Global */
.highlight .vi { color: #19177C } /* Name.Variable.Instance */
.highlight .vm { color: #19177C } /* Name.Variable.Magic */
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
}@media print {
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
    <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/latest.js?config=TeX-AMS_HTML"></script>
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
    <!-- End of mathjax configuration --></head>
<body>
  <div tabindex="-1" id="notebook" class="border-box-sizing">
    <div class="container" id="notebook-container">

<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Graphical-interactive-coordinate-transformation-app">Graphical interactive coordinate transformation app<a class="anchor-link" href="#Graphical-interactive-coordinate-transformation-app">&#182;</a></h1><h3 id="This-first-cell-defines-the-map-where-will-draw-the-boxes-with-the-coordinates-we-want-to-reproject.">This first cell defines the map where will draw the boxes with the coordinates we want to reproject.<a class="anchor-link" href="#This-first-cell-defines-the-map-where-will-draw-the-boxes-with-the-coordinates-we-want-to-reproject.">&#182;</a></h3>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[15]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">from</span> <span class="nn">ipyleaflet</span> <span class="kn">import</span> <span class="n">Map</span><span class="p">,</span> <span class="n">basemaps</span><span class="p">,</span> <span class="n">basemap_to_tiles</span><span class="p">,</span> <span class="n">DrawControl</span>
<span class="kn">import</span> <span class="nn">geojson</span>

<span class="k">def</span> <span class="nf">bbox</span><span class="p">(</span><span class="n">coord_list</span><span class="p">):</span>
    <span class="n">box</span> <span class="o">=</span> <span class="p">[]</span>
    <span class="k">for</span> <span class="n">i</span> <span class="ow">in</span> <span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mi">1</span><span class="p">):</span>
        <span class="n">res</span> <span class="o">=</span> <span class="nb">sorted</span><span class="p">(</span><span class="n">coord_list</span><span class="p">,</span> <span class="n">key</span><span class="o">=</span><span class="k">lambda</span> <span class="n">x</span><span class="p">:</span><span class="n">x</span><span class="p">[</span><span class="n">i</span><span class="p">])</span>
        <span class="n">box</span><span class="o">.</span><span class="n">append</span><span class="p">((</span><span class="n">res</span><span class="p">[</span><span class="mi">0</span><span class="p">][</span><span class="n">i</span><span class="p">],</span><span class="n">res</span><span class="p">[</span><span class="o">-</span><span class="mi">1</span><span class="p">][</span><span class="n">i</span><span class="p">]))</span>
 
    <span class="k">return</span> <span class="n">box</span><span class="p">[</span><span class="mi">0</span><span class="p">][</span><span class="mi">0</span><span class="p">],</span> <span class="n">box</span><span class="p">[</span><span class="mi">1</span><span class="p">][</span><span class="mi">0</span><span class="p">],</span> <span class="n">box</span><span class="p">[</span><span class="mi">0</span><span class="p">][</span><span class="mi">1</span><span class="p">],</span> <span class="n">box</span><span class="p">[</span><span class="mi">1</span><span class="p">][</span><span class="mi">1</span><span class="p">]</span>
    

<span class="n">watercolor</span> <span class="o">=</span> <span class="n">basemap_to_tiles</span><span class="p">(</span><span class="n">basemaps</span><span class="o">.</span><span class="n">Stamen</span><span class="o">.</span><span class="n">Watercolor</span><span class="p">)</span>

<span class="n">m</span> <span class="o">=</span> <span class="n">Map</span><span class="p">(</span><span class="n">layers</span><span class="o">=</span><span class="p">(</span><span class="n">watercolor</span><span class="p">,</span> <span class="p">),</span> <span class="n">center</span><span class="o">=</span><span class="p">(</span><span class="o">-</span><span class="mi">25</span><span class="p">,</span> <span class="mi">140</span><span class="p">),</span> <span class="n">zoom</span><span class="o">=</span><span class="mi">4</span><span class="p">)</span>

<span class="n">draw_control</span> <span class="o">=</span> <span class="n">DrawControl</span><span class="p">(</span><span class="n">circle</span><span class="o">=</span><span class="p">{},</span> <span class="n">circleMarker</span><span class="o">=</span><span class="p">{},</span> <span class="n">circlemarker</span><span class="o">=</span><span class="p">{},</span> 
                           <span class="n">CircleMarker</span><span class="o">=</span><span class="p">{},</span> <span class="n">polyline</span><span class="o">=</span><span class="p">{},</span> <span class="n">marker</span><span class="o">=</span><span class="p">{},</span> <span class="n">polygon</span><span class="o">=</span><span class="p">{},</span>
                          <span class="n">rectangle</span> <span class="o">=</span> <span class="p">{</span><span class="s2">&quot;shapeOptions&quot;</span><span class="p">:</span> <span class="p">{</span><span class="s2">&quot;color&quot;</span><span class="p">:</span> <span class="s2">&quot;#00005d&quot;</span><span class="p">,</span><span class="s2">&quot;fillOpacity&quot;</span><span class="p">:</span> <span class="mf">0.0</span><span class="p">}})</span>


<span class="k">def</span> <span class="nf">handle_draw</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">action</span><span class="p">,</span> <span class="n">geo_json</span><span class="p">):</span>
    <span class="c1">#self.clear()</span>
    <span class="n">lon_min</span><span class="o">.</span><span class="n">value</span><span class="p">,</span><span class="n">lat_min</span><span class="o">.</span><span class="n">value</span><span class="p">,</span><span class="n">lon_max</span><span class="o">.</span><span class="n">value</span><span class="p">,</span><span class="n">lat_max</span><span class="o">.</span><span class="n">value</span><span class="o">=</span><span class="n">bbox</span><span class="p">(</span><span class="nb">list</span><span class="p">(</span><span class="n">geojson</span><span class="o">.</span><span class="n">utils</span><span class="o">.</span><span class="n">coords</span><span class="p">(</span><span class="n">geo_json</span><span class="p">[</span><span class="s1">&#39;geometry&#39;</span><span class="p">])))</span>
    <span class="n">x_min</span><span class="o">.</span><span class="n">value</span><span class="p">,</span><span class="n">y_min</span><span class="o">.</span><span class="n">value</span><span class="o">=</span><span class="n">transform</span><span class="p">(</span><span class="n">lon_min</span><span class="o">.</span><span class="n">value</span><span class="p">,</span><span class="n">lat_min</span><span class="o">.</span><span class="n">value</span><span class="p">)</span>
    <span class="n">x_max</span><span class="o">.</span><span class="n">value</span><span class="p">,</span><span class="n">y_max</span><span class="o">.</span><span class="n">value</span><span class="o">=</span><span class="n">transform</span><span class="p">(</span><span class="n">lon_max</span><span class="o">.</span><span class="n">value</span><span class="p">,</span><span class="n">lat_max</span><span class="o">.</span><span class="n">value</span><span class="p">)</span>
    <span class="nb">print</span><span class="p">(</span><span class="sa">f</span><span class="s2">&quot;Width: {x_max.value-x_min.value}&quot;</span><span class="p">)</span>
    <span class="nb">print</span><span class="p">(</span><span class="sa">f</span><span class="s2">&quot;Height: {y_max.value-y_min.value}&quot;</span><span class="p">)</span>
    
<span class="n">draw_control</span><span class="o">.</span><span class="n">on_draw</span><span class="p">(</span><span class="n">handle_draw</span><span class="p">)</span>

<span class="n">m</span><span class="o">.</span><span class="n">add_control</span><span class="p">(</span><span class="n">draw_control</span><span class="p">)</span>

<span class="n">m</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt"></div>





 
 
<div id="0624fc40-d798-4f18-af78-fd5d2357bb3d"></div>
<div class="output_subarea output_widget_view ">
<script type="text/javascript">
var element = $('#0624fc40-d798-4f18-af78-fd5d2357bb3d');
</script>
<script type="application/vnd.jupyter.widget-view+json">
{"model_id": "09721cf958e14fa2bf7d39583625dbcf", "version_major": 2, "version_minor": 0}
</script>
</div>

</div>

<div class="output_area">

    <div class="prompt"></div>


<div class="output_subarea output_stream output_stdout output_text">
<pre>Width: 969138.0354123353
Height: 1025906.5802111514
Width: 1067710.6478760948
Height: 1026234.6256653685
Width: 1035883.6942156783
Height: 1012169.0844018743
Width: 1018550.5840057939
Height: 1027538.6821468929
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="Let's-define-the-output-CRS-using-its-EPSG-code">Let's define the output CRS using its EPSG code<a class="anchor-link" href="#Let's-define-the-output-CRS-using-its-EPSG-code">&#182;</a></h3>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[9]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">from</span> <span class="nn">__future__</span> <span class="kn">import</span> <span class="n">print_function</span>
<span class="kn">from</span> <span class="nn">ipywidgets</span> <span class="kn">import</span> <span class="n">interact</span><span class="p">,</span> <span class="n">interactive</span><span class="p">,</span> <span class="n">fixed</span><span class="p">,</span> <span class="n">interact_manual</span>
<span class="kn">import</span> <span class="nn">ipywidgets</span> <span class="k">as</span> <span class="nn">widgets</span>

<span class="n">in_crs</span> <span class="o">=</span> <span class="n">widgets</span><span class="o">.</span><span class="n">IntText</span><span class="p">(</span><span class="n">value</span><span class="o">=</span><span class="mi">4326</span><span class="p">,</span><span class="n">description</span><span class="o">=</span><span class="s1">&#39;Input EPSG:&#39;</span><span class="p">,</span><span class="n">disabled</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
<span class="n">display</span><span class="p">(</span><span class="n">in_crs</span><span class="p">)</span>
<span class="n">out_crs</span> <span class="o">=</span> <span class="n">widgets</span><span class="o">.</span><span class="n">IntText</span><span class="p">(</span><span class="n">value</span><span class="o">=</span><span class="mi">3577</span><span class="p">,</span><span class="n">description</span><span class="o">=</span><span class="s1">&#39;Output EPSG:&#39;</span><span class="p">,</span><span class="n">disabled</span><span class="o">=</span><span class="kc">False</span><span class="p">)</span>
<span class="n">display</span><span class="p">(</span><span class="n">out_crs</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt"></div>





 
 
<div id="a25a69e1-4978-41df-9961-24dcfbc3af00"></div>
<div class="output_subarea output_widget_view ">
<script type="text/javascript">
var element = $('#a25a69e1-4978-41df-9961-24dcfbc3af00');
</script>
<script type="application/vnd.jupyter.widget-view+json">
{"model_id": "e8ad8de78717439fa1102521081c6be4", "version_major": 2, "version_minor": 0}
</script>
</div>

</div>

<div class="output_area">

    <div class="prompt"></div>





 
 
<div id="edb64568-ca7e-402b-b79a-a3dbada16b33"></div>
<div class="output_subarea output_widget_view ">
<script type="text/javascript">
var element = $('#edb64568-ca7e-402b-b79a-a3dbada16b33');
</script>
<script type="application/vnd.jupyter.widget-view+json">
{"model_id": "22af92d4715b447f8a39b5e817993a94", "version_major": 2, "version_minor": 0}
</script>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="These-boxes-will-contain-the-corners-of-the-drawn-in-geographical-coordinates">These boxes will contain the corners of the drawn in geographical coordinates<a class="anchor-link" href="#These-boxes-will-contain-the-corners-of-the-drawn-in-geographical-coordinates">&#182;</a></h3>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[10]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">lon_min</span> <span class="o">=</span> <span class="n">widgets</span><span class="o">.</span><span class="n">BoundedFloatText</span><span class="p">(</span><span class="n">value</span><span class="o">=</span><span class="mf">0.0</span><span class="p">,</span><span class="nb">min</span><span class="o">=-</span><span class="mf">380.0</span><span class="p">,</span><span class="nb">max</span><span class="o">=</span><span class="mf">380.0</span><span class="p">,</span><span class="n">description</span><span class="o">=</span><span class="s1">&#39;Min Longitude:&#39;</span><span class="p">)</span>
<span class="n">display</span><span class="p">(</span><span class="n">lon_min</span><span class="p">)</span>
<span class="n">lat_min</span> <span class="o">=</span> <span class="n">widgets</span><span class="o">.</span><span class="n">BoundedFloatText</span><span class="p">(</span><span class="n">value</span><span class="o">=</span><span class="mf">0.0</span><span class="p">,</span><span class="nb">min</span><span class="o">=-</span><span class="mf">90.0</span><span class="p">,</span><span class="nb">max</span><span class="o">=</span><span class="mf">90.0</span><span class="p">,</span><span class="n">description</span><span class="o">=</span><span class="s1">&#39;Min Latitude:&#39;</span><span class="p">)</span>
<span class="n">display</span><span class="p">(</span><span class="n">lat_min</span><span class="p">)</span>
<span class="n">lon_max</span> <span class="o">=</span> <span class="n">widgets</span><span class="o">.</span><span class="n">BoundedFloatText</span><span class="p">(</span><span class="n">value</span><span class="o">=</span><span class="mf">0.0</span><span class="p">,</span><span class="nb">min</span><span class="o">=-</span><span class="mf">380.0</span><span class="p">,</span><span class="nb">max</span><span class="o">=</span><span class="mf">380.0</span><span class="p">,</span><span class="n">description</span><span class="o">=</span><span class="s1">&#39;Max Longitude:&#39;</span><span class="p">)</span>
<span class="n">display</span><span class="p">(</span><span class="n">lon_max</span><span class="p">)</span>
<span class="n">lat_max</span> <span class="o">=</span> <span class="n">widgets</span><span class="o">.</span><span class="n">BoundedFloatText</span><span class="p">(</span><span class="n">value</span><span class="o">=</span><span class="mf">0.0</span><span class="p">,</span><span class="nb">min</span><span class="o">=-</span><span class="mf">90.0</span><span class="p">,</span><span class="nb">max</span><span class="o">=</span><span class="mf">90.0</span><span class="p">,</span><span class="n">description</span><span class="o">=</span><span class="s1">&#39;Max Latitude:&#39;</span><span class="p">)</span>
<span class="n">display</span><span class="p">(</span><span class="n">lat_max</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt"></div>





 
 
<div id="c24f6fed-061a-441e-a915-6cd1a94b69b7"></div>
<div class="output_subarea output_widget_view ">
<script type="text/javascript">
var element = $('#c24f6fed-061a-441e-a915-6cd1a94b69b7');
</script>
<script type="application/vnd.jupyter.widget-view+json">
{"model_id": "44bc88f7560e43a0ba44fc0fd77b9718", "version_major": 2, "version_minor": 0}
</script>
</div>

</div>

<div class="output_area">

    <div class="prompt"></div>





 
 
<div id="fec1ad64-21b1-473a-8af6-28dfada1e56c"></div>
<div class="output_subarea output_widget_view ">
<script type="text/javascript">
var element = $('#fec1ad64-21b1-473a-8af6-28dfada1e56c');
</script>
<script type="application/vnd.jupyter.widget-view+json">
{"model_id": "504859767e364c31ab54ed2b7cb2ad66", "version_major": 2, "version_minor": 0}
</script>
</div>

</div>

<div class="output_area">

    <div class="prompt"></div>





 
 
<div id="7c2d0f39-e8db-4bcf-85a5-8fa7b9708974"></div>
<div class="output_subarea output_widget_view ">
<script type="text/javascript">
var element = $('#7c2d0f39-e8db-4bcf-85a5-8fa7b9708974');
</script>
<script type="application/vnd.jupyter.widget-view+json">
{"model_id": "8afdb925455e4eff87a3681b796b83af", "version_major": 2, "version_minor": 0}
</script>
</div>

</div>

<div class="output_area">

    <div class="prompt"></div>





 
 
<div id="363472f7-72f7-433a-909d-93c62f0de677"></div>
<div class="output_subarea output_widget_view ">
<script type="text/javascript">
var element = $('#363472f7-72f7-433a-909d-93c62f0de677');
</script>
<script type="application/vnd.jupyter.widget-view+json">
{"model_id": "1c243b4df49d4e0caac52e7d7ac3030d", "version_major": 2, "version_minor": 0}
</script>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="Finally-we-declare-the-boxes-that-will-contain-the-projected-coordinates-in-the-chosen-projection">Finally we declare the boxes that will contain the projected coordinates in the chosen projection<a class="anchor-link" href="#Finally-we-declare-the-boxes-that-will-contain-the-projected-coordinates-in-the-chosen-projection">&#182;</a></h3>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[11]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">from</span> <span class="nn">pyproj</span> <span class="kn">import</span> <span class="n">Proj</span><span class="p">,</span> <span class="n">transform</span>
<span class="kn">from</span> <span class="nn">pyproj</span> <span class="kn">import</span> <span class="n">Transformer</span>

<span class="n">transformer</span> <span class="o">=</span> <span class="n">Transformer</span><span class="o">.</span><span class="n">from_crs</span><span class="p">(</span><span class="sa">f</span><span class="s2">&quot;EPSG:</span><span class="si">{in_crs.value}</span><span class="s2">&quot;</span><span class="p">,</span> <span class="sa">f</span><span class="s2">&quot;EPSG:</span><span class="si">{out_crs.value}</span><span class="s2">&quot;</span><span class="p">,</span> <span class="n">always_xy</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>

<span class="k">def</span> <span class="nf">transform</span><span class="p">(</span><span class="n">x</span><span class="p">,</span><span class="n">y</span><span class="p">):</span>
    <span class="k">return</span> <span class="n">transformer</span><span class="o">.</span><span class="n">transform</span><span class="p">(</span><span class="n">x</span><span class="p">,</span><span class="n">y</span><span class="p">)</span>

<span class="n">x_min</span> <span class="o">=</span> <span class="n">widgets</span><span class="o">.</span><span class="n">FloatText</span><span class="p">(</span><span class="n">value</span><span class="o">=</span><span class="mf">0.0</span><span class="p">,</span><span class="n">description</span><span class="o">=</span><span class="s1">&#39;Min X:&#39;</span><span class="p">)</span>
<span class="n">display</span><span class="p">(</span><span class="n">x_min</span><span class="p">)</span>
<span class="n">y_min</span> <span class="o">=</span> <span class="n">widgets</span><span class="o">.</span><span class="n">FloatText</span><span class="p">(</span><span class="n">value</span><span class="o">=</span><span class="mf">0.0</span><span class="p">,</span><span class="n">description</span><span class="o">=</span><span class="s1">&#39;Min Y:&#39;</span><span class="p">)</span>
<span class="n">display</span><span class="p">(</span><span class="n">y_min</span><span class="p">)</span>
<span class="n">x_max</span> <span class="o">=</span> <span class="n">widgets</span><span class="o">.</span><span class="n">FloatText</span><span class="p">(</span><span class="n">value</span><span class="o">=</span><span class="mf">0.0</span><span class="p">,</span><span class="n">description</span><span class="o">=</span><span class="s1">&#39;Max X:&#39;</span><span class="p">)</span>
<span class="n">display</span><span class="p">(</span><span class="n">x_max</span><span class="p">)</span>
<span class="n">y_max</span> <span class="o">=</span> <span class="n">widgets</span><span class="o">.</span><span class="n">FloatText</span><span class="p">(</span><span class="n">value</span><span class="o">=</span><span class="mf">0.0</span><span class="p">,</span><span class="n">description</span><span class="o">=</span><span class="s1">&#39;Max Y:&#39;</span><span class="p">)</span>
<span class="n">display</span><span class="p">(</span><span class="n">y_max</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt"></div>





 
 
<div id="212030e7-8133-4c69-a150-7d8e80ff054c"></div>
<div class="output_subarea output_widget_view ">
<script type="text/javascript">
var element = $('#212030e7-8133-4c69-a150-7d8e80ff054c');
</script>
<script type="application/vnd.jupyter.widget-view+json">
{"model_id": "2a0f9415f6104050a8a87b76233eafce", "version_major": 2, "version_minor": 0}
</script>
</div>

</div>

<div class="output_area">

    <div class="prompt"></div>





 
 
<div id="45fb8070-baca-47c9-9aed-d17e74bbd0bc"></div>
<div class="output_subarea output_widget_view ">
<script type="text/javascript">
var element = $('#45fb8070-baca-47c9-9aed-d17e74bbd0bc');
</script>
<script type="application/vnd.jupyter.widget-view+json">
{"model_id": "4545b80ac12948ba84e52c0299c8fc95", "version_major": 2, "version_minor": 0}
</script>
</div>

</div>

<div class="output_area">

    <div class="prompt"></div>





 
 
<div id="80b78a82-cd9b-4900-9063-aa0cb8d355bc"></div>
<div class="output_subarea output_widget_view ">
<script type="text/javascript">
var element = $('#80b78a82-cd9b-4900-9063-aa0cb8d355bc');
</script>
<script type="application/vnd.jupyter.widget-view+json">
{"model_id": "33d05c1a35a84b4fb39f0f6969ba9354", "version_major": 2, "version_minor": 0}
</script>
</div>

</div>

<div class="output_area">

    <div class="prompt"></div>





 
 
<div id="3a4182e4-bb53-4fed-b459-5fd2c66dda29"></div>
<div class="output_subarea output_widget_view ">
<script type="text/javascript">
var element = $('#3a4182e4-bb53-4fed-b459-5fd2c66dda29');
</script>
<script type="application/vnd.jupyter.widget-view+json">
{"model_id": "581eb89a110f447c90e289795f9a5635", "version_major": 2, "version_minor": 0}
</script>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="Now-go-back-to-the-interactive-map-and-draw-a-rectangle-to-see-the-values-in-these-cells-updated.">Now go back to the interactive map and draw a rectangle to see the values in these cells updated.<a class="anchor-link" href="#Now-go-back-to-the-interactive-map-and-draw-a-rectangle-to-see-the-values-in-these-cells-updated.">&#182;</a></h3>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span> 
</pre></div>

    </div>
</div>
</div>

</div>
    </div>
  </div>
</body>


<head>


<!-- Load require.js. Delete this if your page already loads require.js -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.3.4/require.min.js" integrity="sha256-Ae2Vz/4ePdIu6ZyI/5ZGsYnb+m0JlOmKPjt6XZ9JJkA=" crossorigin="anonymous"></script>
<script src="https://unpkg.com/@jupyter-widgets/html-manager@*/dist/embed-amd.js" crossorigin="anonymous"></script>
<script type="application/vnd.jupyter.widget-state+json">
{
    "version_major": 2,
    "version_minor": 0,
    "state": {
        "b73e72c14bc24f12b914da794a3fb0df": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "1699746878ea4994ada1b73d06394e31": {
            "model_name": "VBoxModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "_dom_classes": [
                    "widget-interact"
                ],
                "children": [
                    "IPY_MODEL_731a4d80871e42a58b89a3c7a8e6e429",
                    "IPY_MODEL_065dda93bf74412695b67ef0c460ff03"
                ],
                "layout": "IPY_MODEL_b73e72c14bc24f12b914da794a3fb0df"
            }
        },
        "75e4b46c23d14c8b88b44bf5b2f613d6": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "5200e36ff486484589a29ae5c93354cb": {
            "model_name": "SliderStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "731a4d80871e42a58b89a3c7a8e6e429": {
            "model_name": "IntSliderModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "x",
                "layout": "IPY_MODEL_75e4b46c23d14c8b88b44bf5b2f613d6",
                "max": 30,
                "min": -10,
                "style": "IPY_MODEL_5200e36ff486484589a29ae5c93354cb",
                "value": 10
            }
        },
        "26c899ce1e0744339006dfaeb4d97636": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "065dda93bf74412695b67ef0c460ff03": {
            "model_name": "OutputModel",
            "model_module": "@jupyter-widgets/output",
            "model_module_version": "1.0.0",
            "state": {
                "layout": "IPY_MODEL_26c899ce1e0744339006dfaeb4d97636",
                "outputs": [
                    {
                        "output_type": "display_data",
                        "data": {
                            "text/plain": "100"
                        },
                        "metadata": {}
                    }
                ]
            }
        },
        "0bee97195a3b469ebd941fc2f2beb42a": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "5ded732b72d34d7a9b7f37f9cd5a7e32": {
            "model_name": "VBoxModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "_dom_classes": [
                    "widget-interact"
                ],
                "children": [
                    "IPY_MODEL_f360e33da41247bfb91599f60e1a3ab1",
                    "IPY_MODEL_835738f45d97443f83a015ed3fa1bd4d"
                ],
                "layout": "IPY_MODEL_0bee97195a3b469ebd941fc2f2beb42a"
            }
        },
        "122d938bfef3405ab6fe2f6d9e8a55f6": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "059f3cde596344c6821878c1bcf5fdfe": {
            "model_name": "SliderStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "f360e33da41247bfb91599f60e1a3ab1": {
            "model_name": "IntSliderModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "x",
                "layout": "IPY_MODEL_122d938bfef3405ab6fe2f6d9e8a55f6",
                "max": 30,
                "min": -10,
                "style": "IPY_MODEL_059f3cde596344c6821878c1bcf5fdfe",
                "value": -10
            }
        },
        "52a0853e85fb443a95b80d0d97225183": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "835738f45d97443f83a015ed3fa1bd4d": {
            "model_name": "OutputModel",
            "model_module": "@jupyter-widgets/output",
            "model_module_version": "1.0.0",
            "state": {
                "layout": "IPY_MODEL_52a0853e85fb443a95b80d0d97225183",
                "outputs": [
                    {
                        "output_type": "display_data",
                        "data": {
                            "text/plain": "100"
                        },
                        "metadata": {}
                    }
                ]
            }
        },
        "3f56517c5bf048df89020fa08bba384d": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "1fca28dff8ff401c9ecf3faa0153f4fd": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "4f647bd023c248ddba4cad0233411bb9": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "9219a273a384421da4315a9e7424cd33": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "f3b6b87b4aa9497ba55b7e5149795940": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "2e05b37e99a1419181e7fb6cc8ce7c8c": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_24c41d913ef146ff901ac71fc1786202",
                    "IPY_MODEL_3d9891304034443e83adf277a7c4c610",
                    "IPY_MODEL_275fe24861ba42bcaef9744e21d85f48"
                ],
                "default_style": "IPY_MODEL_1fca28dff8ff401c9ecf3faa0153f4fd",
                "dragging_style": "IPY_MODEL_4f647bd023c248ddba4cad0233411bb9",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_3f56517c5bf048df89020fa08bba384d"
                ],
                "layout": "IPY_MODEL_9219a273a384421da4315a9e7424cd33",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_1fca28dff8ff401c9ecf3faa0153f4fd",
                "west": 180,
                "zoom": 5
            }
        },
        "24c41d913ef146ff901ac71fc1786202": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "3d9891304034443e83adf277a7c4c610": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "275fe24861ba42bcaef9744e21d85f48": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "fillColor": "#fca45d",
                        "color": "#fca45d",
                        "fillOpacity": 1
                    }
                }
            }
        },
        "83945737427546179a62bb83719efc6e": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "c9552a865cb649458090cb4cedd822e8": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "c4bb343c996441688a1ffd9d88c8dee6": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "2112c81cda804a47a5a09c96a8697056": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "317140ce40d24c85b9a4af32c117c1a8": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "e8f3b9a82de34f25b346845c12f802cb": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_a6f90b60d9d74c0aaa19e9bf218469b9",
                    "IPY_MODEL_53631f783b2b49a88e1d74b3ad4705d7",
                    "IPY_MODEL_a99ff20c75ef442cbd35de15b5448ccc"
                ],
                "default_style": "IPY_MODEL_c9552a865cb649458090cb4cedd822e8",
                "dragging_style": "IPY_MODEL_c4bb343c996441688a1ffd9d88c8dee6",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_83945737427546179a62bb83719efc6e"
                ],
                "layout": "IPY_MODEL_2112c81cda804a47a5a09c96a8697056",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_c9552a865cb649458090cb4cedd822e8",
                "west": 180,
                "zoom": 5
            }
        },
        "a6f90b60d9d74c0aaa19e9bf218469b9": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "53631f783b2b49a88e1d74b3ad4705d7": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "a99ff20c75ef442cbd35de15b5448ccc": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "color": "#fca45d",
                        "fillOpacity": 1
                    }
                }
            }
        },
        "0b495c72301642a6a59ace1ef52a6704": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "27e12c8a8db745efab5e07bcb9fb4c70": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "0f7dce797cc84f7c8c2bf78872082aa8": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "b4cf81b786ee4108825fe9906d97c948": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "c97434bf05864c99b30e2bf2050b6fc2": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "d07707960ba54a668b24b476584dd9d5": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_93e681799d7947b29f864baa245748d6",
                    "IPY_MODEL_60fdc40ee62243d38c2d4d38110f1448",
                    "IPY_MODEL_797420ac25b9451f86a70838978f9f0d"
                ],
                "default_style": "IPY_MODEL_27e12c8a8db745efab5e07bcb9fb4c70",
                "dragging_style": "IPY_MODEL_0f7dce797cc84f7c8c2bf78872082aa8",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_0b495c72301642a6a59ace1ef52a6704"
                ],
                "layout": "IPY_MODEL_b4cf81b786ee4108825fe9906d97c948",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_27e12c8a8db745efab5e07bcb9fb4c70",
                "west": 180,
                "zoom": 5
            }
        },
        "93e681799d7947b29f864baa245748d6": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "60fdc40ee62243d38c2d4d38110f1448": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "797420ac25b9451f86a70838978f9f0d": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "color": "#fca45d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "39f0fc426b9c4297838a88d57ea8c927": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "2a5e538148f54fd5ad242a9b777ce822": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "5f1d856ad3284b66b29987f775516206": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "26cdcaca4bc54866ba0d01c9da8054d2": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "32d9f7f2983546bfa0f9f8d25fa8ad47": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "836c1a274c4b40848bdd6e5363a9a335": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    46.49839225859763,
                    363.86718750000006
                ],
                "controls": [
                    "IPY_MODEL_61aad090c17a45a898adac38484b5de0",
                    "IPY_MODEL_e2fd8de542df4b09809f0ef587384130",
                    "IPY_MODEL_af4df3821b704b408762fad79fbb6e9f"
                ],
                "default_style": "IPY_MODEL_2a5e538148f54fd5ad242a9b777ce822",
                "dragging_style": "IPY_MODEL_5f1d856ad3284b66b29987f775516206",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_39f0fc426b9c4297838a88d57ea8c927"
                ],
                "layout": "IPY_MODEL_26cdcaca4bc54866ba0d01c9da8054d2",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_2a5e538148f54fd5ad242a9b777ce822",
                "west": 180,
                "zoom": 4
            }
        },
        "61aad090c17a45a898adac38484b5de0": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "e2fd8de542df4b09809f0ef587384130": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "af4df3821b704b408762fad79fbb6e9f": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "d7746f88243d4c459f26ce93119ffc9b": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "f2d0a7b3b06b422f9a50a73f4fbfa544": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "9a45a0328cfc4effa1874ebbf6689c44": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "216799f81cff430890992ffc09b7cdd2": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "009b7946c3c349bdafb59a82e9a5bf01": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "1c60bd72939c48d49253af6a1150839f": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_06d64a1210914a3fb8d27ecbb51900da",
                    "IPY_MODEL_815ad1c0264b4d9fb5b563b5ea5917f0",
                    "IPY_MODEL_a3e8242b35f84342827aaab4d96e1e3f"
                ],
                "default_style": "IPY_MODEL_f2d0a7b3b06b422f9a50a73f4fbfa544",
                "dragging_style": "IPY_MODEL_9a45a0328cfc4effa1874ebbf6689c44",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_d7746f88243d4c459f26ce93119ffc9b"
                ],
                "layout": "IPY_MODEL_216799f81cff430890992ffc09b7cdd2",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_f2d0a7b3b06b422f9a50a73f4fbfa544",
                "west": 180,
                "zoom": 5
            }
        },
        "06d64a1210914a3fb8d27ecbb51900da": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "815ad1c0264b4d9fb5b563b5ea5917f0": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "a3e8242b35f84342827aaab4d96e1e3f": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "37de1e5d93d64ab492ce7b9cefb707db": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "ac17736db77641108fe4adcef2e7d001": {
            "model_name": "VBoxModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "_dom_classes": [
                    "widget-interact"
                ],
                "children": [
                    "IPY_MODEL_e61f8136e04b46a2b5fb3b6f9dd599ea",
                    "IPY_MODEL_ec2d1628f60a4679982bbce1b63841f6"
                ],
                "layout": "IPY_MODEL_37de1e5d93d64ab492ce7b9cefb707db"
            }
        },
        "7a26924556e4471c9111eac79a0030ba": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "b6e80843f59e4002993b125a38e42f72": {
            "model_name": "SliderStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "e61f8136e04b46a2b5fb3b6f9dd599ea": {
            "model_name": "IntSliderModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "x",
                "layout": "IPY_MODEL_7a26924556e4471c9111eac79a0030ba",
                "max": 30,
                "min": -10,
                "style": "IPY_MODEL_b6e80843f59e4002993b125a38e42f72",
                "value": 10
            }
        },
        "e5dcffbe562d4c67b78177726f1cd95c": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "ec2d1628f60a4679982bbce1b63841f6": {
            "model_name": "OutputModel",
            "model_module": "@jupyter-widgets/output",
            "model_module_version": "1.0.0",
            "state": {
                "layout": "IPY_MODEL_e5dcffbe562d4c67b78177726f1cd95c",
                "outputs": [
                    {
                        "output_type": "display_data",
                        "data": {
                            "text/plain": "100"
                        },
                        "metadata": {}
                    }
                ]
            }
        },
        "032ee9d35e6d435fa88cc03e57e8ea3a": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "f4a936cd765247a49c59fc82c584bf6c": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "2fddb181ef644708870f44c3e53f1adf": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "7edf090973d842ea8b9d1c0a35dfa7c2": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "2b8055d8f0ff497d9ac425f139ff815c": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "62ba8ad6c8144a94a7797115fe01531f": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_5c22c953463f4c92a48ba4fc3c1a4456",
                    "IPY_MODEL_03318001037741d893478a0aadfdd1e9",
                    "IPY_MODEL_0c0795c1af3f4511a744b6ee31c5a4f2"
                ],
                "default_style": "IPY_MODEL_f4a936cd765247a49c59fc82c584bf6c",
                "dragging_style": "IPY_MODEL_2fddb181ef644708870f44c3e53f1adf",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_032ee9d35e6d435fa88cc03e57e8ea3a"
                ],
                "layout": "IPY_MODEL_7edf090973d842ea8b9d1c0a35dfa7c2",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_f4a936cd765247a49c59fc82c584bf6c",
                "west": 180,
                "zoom": 5
            }
        },
        "5c22c953463f4c92a48ba4fc3c1a4456": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "03318001037741d893478a0aadfdd1e9": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "0c0795c1af3f4511a744b6ee31c5a4f2": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "38e7fc7950c841868d311030471c733d": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "a87bfc896e7c4830b00dda3b2b9e89c7": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "33ef2527b6214897ae9d3e39c88725aa": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "32bdbb9a26854fa990fb255a96fb3a04": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "c2adffc25d9e4a61abe080eb05286b37": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "dbdd03347c744f648c9c4cc46a420e77": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_5b7aed076b3448228b832c9fe0469e0b",
                    "IPY_MODEL_819c7ae3e2e04651a8e74fdb3333536d",
                    "IPY_MODEL_2516feabb92d4d329a2a1771ea8fe6c6"
                ],
                "default_style": "IPY_MODEL_a87bfc896e7c4830b00dda3b2b9e89c7",
                "dragging_style": "IPY_MODEL_33ef2527b6214897ae9d3e39c88725aa",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_38e7fc7950c841868d311030471c733d"
                ],
                "layout": "IPY_MODEL_32bdbb9a26854fa990fb255a96fb3a04",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_a87bfc896e7c4830b00dda3b2b9e89c7",
                "west": 180,
                "zoom": 5
            }
        },
        "5b7aed076b3448228b832c9fe0469e0b": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "819c7ae3e2e04651a8e74fdb3333536d": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "2516feabb92d4d329a2a1771ea8fe6c6": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "7c7aad093e5b48568b1d96228a979cbe": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "6b9573eca79342e29d48034cb1699301": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "b27302b7a7d64a7b84afb21ba224df21": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "e5a768893fdb423d81b983734e064188": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "d2ed436b8558412cb69bb7385cfcbde3": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "a5285ad634524e3e93b0e89767a69970": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_3ab872ce8b144367a14d71cc9c66a492",
                    "IPY_MODEL_58032eb878cb4092a1939a91733cb372",
                    "IPY_MODEL_e95c44b388884b8cbe1a0765d87667e4"
                ],
                "default_style": "IPY_MODEL_6b9573eca79342e29d48034cb1699301",
                "dragging_style": "IPY_MODEL_b27302b7a7d64a7b84afb21ba224df21",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_7c7aad093e5b48568b1d96228a979cbe"
                ],
                "layout": "IPY_MODEL_e5a768893fdb423d81b983734e064188",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_6b9573eca79342e29d48034cb1699301",
                "west": 180,
                "zoom": 5
            }
        },
        "3ab872ce8b144367a14d71cc9c66a492": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "58032eb878cb4092a1939a91733cb372": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "e95c44b388884b8cbe1a0765d87667e4": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "1af913143fc64b969a4123adb16d8d90": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "2ae35897d4754d60b01ecb70b3a3007b": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "7c8898aae9a648829da9873033568a1d": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "326eeed77f74474f9881415c9a2240e7": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "e84edb49d1574d389acb1456381cd0db": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "cb12cfbbb98a4e36b9232ae551d297c4": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50,
                    354
                ],
                "controls": [
                    "IPY_MODEL_6d97199a9f6e42138c3713378e185c74",
                    "IPY_MODEL_dd28426e60d74ae4be2ef88adcc8a025",
                    "IPY_MODEL_a7c0077244eb40c9800d76da88011086"
                ],
                "default_style": "IPY_MODEL_2ae35897d4754d60b01ecb70b3a3007b",
                "dragging_style": "IPY_MODEL_7c8898aae9a648829da9873033568a1d",
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_1af913143fc64b969a4123adb16d8d90"
                ],
                "layout": "IPY_MODEL_326eeed77f74474f9881415c9a2240e7",
                "modisdate": "yesterday",
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "style": "IPY_MODEL_e84edb49d1574d389acb1456381cd0db",
                "zoom": 5
            }
        },
        "6d97199a9f6e42138c3713378e185c74": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "dd28426e60d74ae4be2ef88adcc8a025": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "a7c0077244eb40c9800d76da88011086": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "d0a15783c7f54ab29a7881853d90b265": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "492fa82aea77495b94472a2490ee861f": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "f68f2ab0f7c045a9a3da50000fabb84e": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "f5930fafcb294ab194bc563905554557": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "2cac4767d2e44a148a99d686b3cb7533": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "848ec307d2b74880935bcdc57d69843e": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_12f5a8fb61a64ea0ba3c8232d276c5b1",
                    "IPY_MODEL_cd4f343cc36a4635a58c9caf42ae17fd",
                    "IPY_MODEL_9c62f5b1875d488198e1d98ae1edbf07"
                ],
                "default_style": "IPY_MODEL_492fa82aea77495b94472a2490ee861f",
                "dragging_style": "IPY_MODEL_f68f2ab0f7c045a9a3da50000fabb84e",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_d0a15783c7f54ab29a7881853d90b265"
                ],
                "layout": "IPY_MODEL_f5930fafcb294ab194bc563905554557",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_492fa82aea77495b94472a2490ee861f",
                "west": 180,
                "zoom": 5
            }
        },
        "12f5a8fb61a64ea0ba3c8232d276c5b1": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "cd4f343cc36a4635a58c9caf42ae17fd": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "9c62f5b1875d488198e1d98ae1edbf07": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "circlemarker": {
                    "shapeOptions": {}
                },
                "options": [
                    "position"
                ],
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "e38de60f2ce743b2b2468ff7f38a56f8": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "85f6bff6623344c3840d8f4e8ae3c0e7": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "b1defe0afd9b49baab2623a366d638ca": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "17912998afed444cac170797fd3d63da": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "64e61f06a9b54546bd7259437b2582b6": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "fef33ab96bb14ce7af6d7346da5dc3aa": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_4a6ee792006f4c52860af002a1b2b3ef",
                    "IPY_MODEL_d8557b1fdcf84ff0ab3194709b48839f",
                    "IPY_MODEL_c4f41bae3b67473195a8655fcdd209df"
                ],
                "default_style": "IPY_MODEL_85f6bff6623344c3840d8f4e8ae3c0e7",
                "dragging_style": "IPY_MODEL_b1defe0afd9b49baab2623a366d638ca",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_e38de60f2ce743b2b2468ff7f38a56f8"
                ],
                "layout": "IPY_MODEL_17912998afed444cac170797fd3d63da",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_85f6bff6623344c3840d8f4e8ae3c0e7",
                "west": 180,
                "zoom": 5
            }
        },
        "4a6ee792006f4c52860af002a1b2b3ef": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "d8557b1fdcf84ff0ab3194709b48839f": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "c4f41bae3b67473195a8655fcdd209df": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "f107bc31830c4f2e877607e6e4308855": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "8bf250efe5864445a14e65ca259d7ec3": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "28ff54fade2147eaac45d3624ad84dd9": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "f630f9e35eaf4866985f0d4028928bed": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "5971ce3301a24a498ca01d5bf8b6ea74": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "dcad6c8162d54c70b7d4373a60374bfb": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_096fb244ba924e908f77a0e9c216caa0",
                    "IPY_MODEL_31d442ed61f44dd1b7406351b38dbaa5",
                    "IPY_MODEL_41ee398b80764e838874ac447640f297"
                ],
                "default_style": "IPY_MODEL_8bf250efe5864445a14e65ca259d7ec3",
                "dragging_style": "IPY_MODEL_28ff54fade2147eaac45d3624ad84dd9",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_f107bc31830c4f2e877607e6e4308855"
                ],
                "layout": "IPY_MODEL_f630f9e35eaf4866985f0d4028928bed",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_8bf250efe5864445a14e65ca259d7ec3",
                "west": 180,
                "zoom": 5
            }
        },
        "096fb244ba924e908f77a0e9c216caa0": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "31d442ed61f44dd1b7406351b38dbaa5": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "41ee398b80764e838874ac447640f297": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "01f5525b78d84dfeac88453a688aa4a5": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "62906d7646c549489cceab93ed24588e": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "5277fb443c874b6cb0538b1278829709": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "721886dc6cf54cf79d87ce38da036e38": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "eb73d2ece9904833bc8396e8f3100bb7": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "71d36257a1e342abb951013f86816c7a": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    49.41097319969587,
                    370.63476562500006
                ],
                "controls": [
                    "IPY_MODEL_e57083b765584d7e956157fc368af9f2",
                    "IPY_MODEL_6f6f7d28121e439b8cc530b5fbc3d8ec",
                    "IPY_MODEL_b81cf00751674431b6463e56f671fe9a"
                ],
                "default_style": "IPY_MODEL_62906d7646c549489cceab93ed24588e",
                "dragging_style": "IPY_MODEL_5277fb443c874b6cb0538b1278829709",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_01f5525b78d84dfeac88453a688aa4a5"
                ],
                "layout": "IPY_MODEL_721886dc6cf54cf79d87ce38da036e38",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_62906d7646c549489cceab93ed24588e",
                "west": 180,
                "zoom": 5
            }
        },
        "e57083b765584d7e956157fc368af9f2": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "6f6f7d28121e439b8cc530b5fbc3d8ec": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "b81cf00751674431b6463e56f671fe9a": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "3c00f48b3dc64bd484f02ec9d8e31812": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "8892f08428574a7fb947d136e195144e": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "6cca69f867cd4be6b94dee019f4a319a": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "b8303fc0c52240a394811546c3b31ceb": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "76d61b5313a8403883fe6b4afc52b411": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "12300cecb1044a3f97d852bd3c52a2a8": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_af932b29de7b488b81f394eaf140b17d",
                    "IPY_MODEL_6fb08a9a05a545efa107f9f1dc4d9a6b",
                    "IPY_MODEL_8b80d5aed1094d8dbd64c50d3df3ba31"
                ],
                "default_style": "IPY_MODEL_8892f08428574a7fb947d136e195144e",
                "dragging_style": "IPY_MODEL_6cca69f867cd4be6b94dee019f4a319a",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_3c00f48b3dc64bd484f02ec9d8e31812"
                ],
                "layout": "IPY_MODEL_b8303fc0c52240a394811546c3b31ceb",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_8892f08428574a7fb947d136e195144e",
                "west": 180,
                "zoom": 5
            }
        },
        "af932b29de7b488b81f394eaf140b17d": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "6fb08a9a05a545efa107f9f1dc4d9a6b": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "8b80d5aed1094d8dbd64c50d3df3ba31": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "808b2910b88b4d8891204e721c8ff4e7": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "021b360921834389bf8bf597b0e8d80f": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "3a81c4015ef84b08bcca1d33bbf34dc2": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "98dc7636d491486aa6517f19e9736f84": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "e71c28082a7f4635ab64b573271e7cfc": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "0cbd0e69006f484f9cac6876d21a6675": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_b7c5aef690a84b0db1b25529566e4be7",
                    "IPY_MODEL_cff50147b8c34e79a6ec884532442e02",
                    "IPY_MODEL_d89c415e388e46249c6a38e9a2fcdde0"
                ],
                "default_style": "IPY_MODEL_021b360921834389bf8bf597b0e8d80f",
                "dragging_style": "IPY_MODEL_3a81c4015ef84b08bcca1d33bbf34dc2",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_808b2910b88b4d8891204e721c8ff4e7"
                ],
                "layout": "IPY_MODEL_98dc7636d491486aa6517f19e9736f84",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_021b360921834389bf8bf597b0e8d80f",
                "west": 180,
                "zoom": 5
            }
        },
        "b7c5aef690a84b0db1b25529566e4be7": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "cff50147b8c34e79a6ec884532442e02": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "d89c415e388e46249c6a38e9a2fcdde0": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "fb04bc4cf6fb4d1bb1a4bb444c8275ac": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "4636e4b117d3458bb657ca599eabbc38": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "e6738af4c22f4ff081aa12851f9b110d": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "6360a10de3e941b681a5dce56d47ddb8": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "96f8a65400d245c392bf3a5648b5e50e": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "94fc2d80afd2431a8bc9203762682bde": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_974d435660094f50bc0d355036928b1d",
                    "IPY_MODEL_cce26a27b54b46a3a85badcfbd40d8dc",
                    "IPY_MODEL_c0f1d485e4f346c0b833899119ddaf79"
                ],
                "default_style": "IPY_MODEL_4636e4b117d3458bb657ca599eabbc38",
                "dragging_style": "IPY_MODEL_e6738af4c22f4ff081aa12851f9b110d",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_fb04bc4cf6fb4d1bb1a4bb444c8275ac"
                ],
                "layout": "IPY_MODEL_6360a10de3e941b681a5dce56d47ddb8",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_4636e4b117d3458bb657ca599eabbc38",
                "west": 180,
                "zoom": 5
            }
        },
        "974d435660094f50bc0d355036928b1d": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "cce26a27b54b46a3a85badcfbd40d8dc": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "c0f1d485e4f346c0b833899119ddaf79": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "aa98dadf4f5e479cbb00edbadf30b46d": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "431283bd3a934caabad25f975d655efe": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "1479dd91462a45d8bc5d16be24cf903c": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "973f0e2058c443e89630b8c433f2b938": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "43b6fb811fee4f19b082de5c5ff4f5bc": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "78aef8ecf9af40c9a3be8bc554993b85": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_c4e9290236814b2a85364ec91a83306e",
                    "IPY_MODEL_1617f4b84a514493971809a7aae25164",
                    "IPY_MODEL_d07ef52136bc413eaa5e237621b51674"
                ],
                "default_style": "IPY_MODEL_431283bd3a934caabad25f975d655efe",
                "dragging_style": "IPY_MODEL_1479dd91462a45d8bc5d16be24cf903c",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_aa98dadf4f5e479cbb00edbadf30b46d"
                ],
                "layout": "IPY_MODEL_973f0e2058c443e89630b8c433f2b938",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_431283bd3a934caabad25f975d655efe",
                "west": 180,
                "zoom": 5
            }
        },
        "c4e9290236814b2a85364ec91a83306e": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "1617f4b84a514493971809a7aae25164": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "d07ef52136bc413eaa5e237621b51674": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "5657c7b0c9d343eca620095cf7829996": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "8851a289d56241e8b2694386f2a5819b": {
            "model_name": "SliderStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "44bd883ae8424d51a496b3e43d23696c": {
            "model_name": "IntSliderModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "layout": "IPY_MODEL_5657c7b0c9d343eca620095cf7829996",
                "style": "IPY_MODEL_8851a289d56241e8b2694386f2a5819b",
                "value": 100
            }
        },
        "dffc0a738b9f46209ff4ab3582023305": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "1cefe3f8fb01454599602f5998090c5f": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "e45c325318b746f3a76579d8341e3ad7": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "layout": "IPY_MODEL_dffc0a738b9f46209ff4ab3582023305",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_1cefe3f8fb01454599602f5998090c5f"
            }
        },
        "9e032c2e98524f78b76189272ed6d69b": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "4203de9bd50e4050a13ef564aa957897": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "3662e2bc87ea415ab7583d0b514fbb5d": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "layout": "IPY_MODEL_9e032c2e98524f78b76189272ed6d69b",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_4203de9bd50e4050a13ef564aa957897"
            }
        },
        "46ba14ed6cad4743b01aae8e2e85f4d7": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "6500697f4c124f5785f05fb7cd65ac8b": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "88e5ae25887b40268504aae30523d585": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Text:",
                "layout": "IPY_MODEL_46ba14ed6cad4743b01aae8e2e85f4d7",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_6500697f4c124f5785f05fb7cd65ac8b"
            }
        },
        "3f09c3df8650421eb406c42f0381cd6e": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "cede58650d9c427887239e4aaa8a55ef": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "7f2bda649e3c43f3a2793bc711d30001": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Text:",
                "layout": "IPY_MODEL_3f09c3df8650421eb406c42f0381cd6e",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_cede58650d9c427887239e4aaa8a55ef"
            }
        },
        "c89264599e9547d892759770917dda86": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "cc459c987c6c49aa9ef9fd568b5f8d0a": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "c6aa72aef68c48f9a2b947f3f4e5416f": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Latitude:",
                "layout": "IPY_MODEL_c89264599e9547d892759770917dda86",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_cc459c987c6c49aa9ef9fd568b5f8d0a"
            }
        },
        "e11ce3ffd5a843429be857a61aeb6afe": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "4f0cb61035534ad8a8ae362c20b2e326": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "2ce4ad18dc6a462393241f63204e8f95": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Latitude:",
                "layout": "IPY_MODEL_e11ce3ffd5a843429be857a61aeb6afe",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_4f0cb61035534ad8a8ae362c20b2e326"
            }
        },
        "46bfeb55472c448bb56034b36d0eb7cb": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "0fc9dadb7c66472e85eec14ad67fc941": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "6daa72f9f417486db11fbb913776e471": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Longitude:",
                "layout": "IPY_MODEL_46bfeb55472c448bb56034b36d0eb7cb",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_0fc9dadb7c66472e85eec14ad67fc941"
            }
        },
        "d0198a94e5d6488cabd5a932f37414ac": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "9ddf7519ab124bce89da60da1d51053d": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "a3865615f15c48f2909137b3ee1b3d0c": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Longitude:",
                "layout": "IPY_MODEL_d0198a94e5d6488cabd5a932f37414ac",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_9ddf7519ab124bce89da60da1d51053d"
            }
        },
        "a46337e6362a41f9a4c89010e025bfbe": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "24ffdcefd44b4c8f80db2138632070f0": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "125073f4caf946cf9c85019850c4be63": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Latitude:",
                "layout": "IPY_MODEL_a46337e6362a41f9a4c89010e025bfbe",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_24ffdcefd44b4c8f80db2138632070f0",
                "value": 48.216438
            }
        },
        "409bfdc70b484160bcb42e04420d6292": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "8556410db64e4218a873ec32b678a822": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "672bf5df3c8c4ad59bf6e9b97997d535": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Latitude:",
                "layout": "IPY_MODEL_409bfdc70b484160bcb42e04420d6292",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_8556410db64e4218a873ec32b678a822",
                "value": 53.948812
            }
        },
        "881cf53d767b419d997f14f94cf32da5": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "2a6ceb0abd4e44fa936324bada750ff4": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "59871567ffce4af88b7f2c165064d5bb": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Longitude:",
                "layout": "IPY_MODEL_881cf53d767b419d997f14f94cf32da5",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_2a6ceb0abd4e44fa936324bada750ff4",
                "value": -10.410882000000015
            }
        },
        "7b167374466545399c1e901bd48c72ec": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "01398fcb1f4040d5ac7290ea558657ea": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "0436720905d3408ab9a37e7f86af5f9f": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Longitude:",
                "layout": "IPY_MODEL_7b167374466545399c1e901bd48c72ec",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_01398fcb1f4040d5ac7290ea558657ea",
                "value": 4.357767000000024
            }
        },
        "48452a2724f14d09a92a85caaabd91bd": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "166c35d193bf45028d525f9547d34b02": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "8e7870d4deb346f5b50a083747d47538": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "e26716501ed74f20967b0f5a8aaccf2e": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "7e655529509b4db3856cb1877f1bcadc": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "be80cd172a784a0186b7cd472ad610eb": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_ab92b88cd6174de48e0936c91f682db3",
                    "IPY_MODEL_19a2bbb3919a4c4e8a9147a15fe4835c",
                    "IPY_MODEL_819ded1cc22b404ebc3d812a7614b6be"
                ],
                "default_style": "IPY_MODEL_166c35d193bf45028d525f9547d34b02",
                "dragging_style": "IPY_MODEL_8e7870d4deb346f5b50a083747d47538",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_48452a2724f14d09a92a85caaabd91bd"
                ],
                "layout": "IPY_MODEL_e26716501ed74f20967b0f5a8aaccf2e",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_166c35d193bf45028d525f9547d34b02",
                "west": 180,
                "zoom": 5
            }
        },
        "ab92b88cd6174de48e0936c91f682db3": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "19a2bbb3919a4c4e8a9147a15fe4835c": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "819ded1cc22b404ebc3d812a7614b6be": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "3fde860a07294bc19bcea6e391d0f9b4": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "3f8c9279b54440c88b3a9b5d843e110f": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "36704e30ef7d4be494a6e2d364be28b4": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "c862ca305a8941b8b3e32f896a78af4a": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "f7aa1ba948294d59a74a3b200dd2573c": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "8f0a35f808774b879a524657f18c3233": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_aed9602c9a414cb9b697c6727e1791a1",
                    "IPY_MODEL_af52e28e96c94a3cb229123664af288f",
                    "IPY_MODEL_8df2ee89f1e54f7ab0672f9197fdf697"
                ],
                "default_style": "IPY_MODEL_3f8c9279b54440c88b3a9b5d843e110f",
                "dragging_style": "IPY_MODEL_36704e30ef7d4be494a6e2d364be28b4",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_3fde860a07294bc19bcea6e391d0f9b4"
                ],
                "layout": "IPY_MODEL_c862ca305a8941b8b3e32f896a78af4a",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_3f8c9279b54440c88b3a9b5d843e110f",
                "west": 180,
                "zoom": 5
            }
        },
        "aed9602c9a414cb9b697c6727e1791a1": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "af52e28e96c94a3cb229123664af288f": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "8df2ee89f1e54f7ab0672f9197fdf697": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "ee47164b3c434a849c39fe7ea3665246": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "f237836fa5e645e9a9805b7c743fac8f": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "1d288eb9fa51484b894e81d0118d5d58": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "layout": "IPY_MODEL_ee47164b3c434a849c39fe7ea3665246",
                "step": 1,
                "style": "IPY_MODEL_f237836fa5e645e9a9805b7c743fac8f",
                "value": 4326
            }
        },
        "2a701099d38348dfac4981a35d17dbef": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "e272f43c1eb24a36b86e717aa60765d7": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "2b161606391f4a8980ced936769ea70a": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_2a701099d38348dfac4981a35d17dbef",
                "step": 1,
                "style": "IPY_MODEL_e272f43c1eb24a36b86e717aa60765d7",
                "value": 3577
            }
        },
        "4b6ececc93b04d92b419added340196d": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "b566f82739284b3e8719da8037aaa411": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "46e0e905c03e44b48e23d00775e0d96a": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Longitude:",
                "layout": "IPY_MODEL_4b6ececc93b04d92b419added340196d",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_b566f82739284b3e8719da8037aaa411"
            }
        },
        "220e2bc2d03d476883fafbc4f5e208d3": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "3dd8d0ee555340c1a9b48863b361947e": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "651970ad47f541b4af1b504f84265143": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Latitude:",
                "layout": "IPY_MODEL_220e2bc2d03d476883fafbc4f5e208d3",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_3dd8d0ee555340c1a9b48863b361947e"
            }
        },
        "9e6d4092e7bd412cb38faa0c53eecb64": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "d2409c4a3d234d1d9c69bd553a705457": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "10e36083654c4f71bca4fe099a5c20fc": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Longitude:",
                "layout": "IPY_MODEL_9e6d4092e7bd412cb38faa0c53eecb64",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_d2409c4a3d234d1d9c69bd553a705457"
            }
        },
        "b8bc62e1f8c54232ac0be1a6d75698cd": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "600b370f52cb46aa83b85a8a0e4d1832": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "05579ed7dc2b4ec998ce97c7c789f334": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Latitude:",
                "layout": "IPY_MODEL_b8bc62e1f8c54232ac0be1a6d75698cd",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_600b370f52cb46aa83b85a8a0e4d1832"
            }
        },
        "050250b958cf45b6b58e83881c9d3444": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "37208ecf55644e21bedb4388f7e98d86": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "7bc6fb3398994e758180b3347f3e72be": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min X:",
                "layout": "IPY_MODEL_050250b958cf45b6b58e83881c9d3444",
                "step": null,
                "style": "IPY_MODEL_37208ecf55644e21bedb4388f7e98d86"
            }
        },
        "87f90fcd7f7d4a21877d149628c5e32a": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "ddc82c8680494effae514aabda4d700d": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "db19ed345baa46f985cacf7639c5a8a1": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Y:",
                "layout": "IPY_MODEL_87f90fcd7f7d4a21877d149628c5e32a",
                "step": null,
                "style": "IPY_MODEL_ddc82c8680494effae514aabda4d700d"
            }
        },
        "d0555037400e4051ac17d47c7b3e7e8e": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "7f13d1b1f64b46b59cf0c86fc1b57348": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "209e2692e5c0496ea6ef0b4cab0bf2f3": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max X:",
                "layout": "IPY_MODEL_d0555037400e4051ac17d47c7b3e7e8e",
                "step": null,
                "style": "IPY_MODEL_7f13d1b1f64b46b59cf0c86fc1b57348"
            }
        },
        "cbfe294d7b1e40a29513b9d1a7f77044": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "d36b1ddd977541ce8f4b248a2f9df521": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "ae810560c65b4532a19826b6ade646cc": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Y:",
                "layout": "IPY_MODEL_cbfe294d7b1e40a29513b9d1a7f77044",
                "step": null,
                "style": "IPY_MODEL_d36b1ddd977541ce8f4b248a2f9df521"
            }
        },
        "e88c8beca1c94d18b019cb114e3babf1": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "19960ebaac1c46e6a76823f407b8014d": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "08399fc8e869441a82fca06c91b73910": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "layout": "IPY_MODEL_e88c8beca1c94d18b019cb114e3babf1",
                "step": 1,
                "style": "IPY_MODEL_19960ebaac1c46e6a76823f407b8014d",
                "value": 4326
            }
        },
        "d9a38a4d40fe47c889cb4336e3d72a36": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "be70c2910f5f445290a68a877533de20": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "df6e3aaac22a4f468f101380e0fa3a80": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_d9a38a4d40fe47c889cb4336e3d72a36",
                "step": 1,
                "style": "IPY_MODEL_be70c2910f5f445290a68a877533de20",
                "value": 3577
            }
        },
        "b6fd657f679b46efa5f5738f24a879b8": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "92e7117268ac41b9970f0e0e68979d05": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "cd013e6bda57401bb1599c101f529adf": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min X:",
                "layout": "IPY_MODEL_b6fd657f679b46efa5f5738f24a879b8",
                "step": null,
                "style": "IPY_MODEL_92e7117268ac41b9970f0e0e68979d05",
                "value": 27775.674821281424
            }
        },
        "25e81d185b6d42609d4fa3734696d640": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "234dca2a8ead4c7fb80cafc65d1b8fd2": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "0c6c34fb7fe94fa39ea087f2226fb751": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Y:",
                "layout": "IPY_MODEL_25e81d185b6d42609d4fa3734696d640",
                "step": null,
                "style": "IPY_MODEL_234dca2a8ead4c7fb80cafc65d1b8fd2",
                "value": -2566853.945113243
            }
        },
        "f95000dedfee4e3182499d986e5e371c": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "897388468d654bc08b7ac4a9aaacf79c": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "bd6e0446d2d84301a5d21fb850fa70c9": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max X:",
                "layout": "IPY_MODEL_f95000dedfee4e3182499d986e5e371c",
                "step": null,
                "style": "IPY_MODEL_897388468d654bc08b7ac4a9aaacf79c",
                "value": 1389570.1046725435
            }
        },
        "388c5693bb134995826db837caeee147": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "a67f857a2b284da3a3fcf0248e0d1650": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "4ba55c9fb3bf41a6bad11191b63c065c": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Y:",
                "layout": "IPY_MODEL_388c5693bb134995826db837caeee147",
                "step": null,
                "style": "IPY_MODEL_a67f857a2b284da3a3fcf0248e0d1650",
                "value": -1299293.844582205
            }
        },
        "ec545c916701490f8d2b2b7a3d0ed865": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "c6af5abbe769491998399d444926a577": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "963ea571d1844865a8d3ed09a198007f": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Longitude:",
                "layout": "IPY_MODEL_ec545c916701490f8d2b2b7a3d0ed865",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_c6af5abbe769491998399d444926a577",
                "value": 132.275391
            }
        },
        "ebd337a1fb5644508b575397fc83abd7": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "95dccdb8fed44c56a2b0893243d502ba": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "fa6d95b4c2f740618382fcc1a8a85569": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Latitude:",
                "layout": "IPY_MODEL_ebd337a1fb5644508b575397fc83abd7",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_95dccdb8fed44c56a2b0893243d502ba",
                "value": -23.787858
            }
        },
        "0895cf872b534fe5a0223993bd975a6c": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "92103010bd8c49daae82ee56d6186cc0": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "cc94d3c3ad454b3ca437f6bff4b7dbeb": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Longitude:",
                "layout": "IPY_MODEL_0895cf872b534fe5a0223993bd975a6c",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_92103010bd8c49daae82ee56d6186cc0",
                "value": 144.50336399999998
            }
        },
        "55e1eab079fd47e090d9702b27355049": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "5cd17dee862f4a0d984b09a3c5a19cac": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "68c75dddf0324ba0b4ee211cca2d9dc6": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Latitude:",
                "layout": "IPY_MODEL_55e1eab079fd47e090d9702b27355049",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_5cd17dee862f4a0d984b09a3c5a19cac",
                "value": -11.695273
            }
        },
        "5c851c1e172c4863a2326ab12e2b25f5": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "81d9c2e9e6e147478c7918307d6733ef": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "b6dc38440af74b7fa96b6a0f0f4af5ed": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "11c5497f7d534cb9be6e7d9f1af21c7c": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "5ceef636d92344e3ae571c5f8606f733": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "ee3293f8e2444cdb8c475665120b41fb": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -24.766784522874442,
                    500.09765625000006
                ],
                "controls": [
                    "IPY_MODEL_81599faf121a4f46bd54b7bb96178605",
                    "IPY_MODEL_40805f26af55438988e20d57e7f908fb",
                    "IPY_MODEL_314b29a04c194565a24a7e0aa2b1135d"
                ],
                "default_style": "IPY_MODEL_81d9c2e9e6e147478c7918307d6733ef",
                "dragging_style": "IPY_MODEL_b6dc38440af74b7fa96b6a0f0f4af5ed",
                "east": 500.09765625000006,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_5c851c1e172c4863a2326ab12e2b25f5"
                ],
                "layout": "IPY_MODEL_11c5497f7d534cb9be6e7d9f1af21c7c",
                "modisdate": "yesterday",
                "north": -24.766784522874442,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": -24.766784522874442,
                "style": "IPY_MODEL_81d9c2e9e6e147478c7918307d6733ef",
                "west": 180,
                "zoom": 4
            }
        },
        "81599faf121a4f46bd54b7bb96178605": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "40805f26af55438988e20d57e7f908fb": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "314b29a04c194565a24a7e0aa2b1135d": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "261923f191a342d38cca8c467ba2ce6a": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "5ee19975b4c341659885a957544a8904": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "fee3d903cda845379463448e41b5589e": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "disabled": true,
                "layout": "IPY_MODEL_261923f191a342d38cca8c467ba2ce6a",
                "step": 1,
                "style": "IPY_MODEL_5ee19975b4c341659885a957544a8904",
                "value": 4326
            }
        },
        "f6ab17e03273448eab2696a410ed4f3a": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "0dce51e502e1488d8b26ef0a5bbbc8aa": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "d8c45605093b49c3903fb741bc2ca7bf": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_f6ab17e03273448eab2696a410ed4f3a",
                "step": 1,
                "style": "IPY_MODEL_0dce51e502e1488d8b26ef0a5bbbc8aa",
                "value": 3577
            }
        },
        "f066f3d93b6f4ab0b371382db5b8bd25": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "2911ea35cdb14b0c8269f152684b30ee": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "1f225ccc8a4d4680934312d7ddfa31cb": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "disabled": true,
                "layout": "IPY_MODEL_f066f3d93b6f4ab0b371382db5b8bd25",
                "step": 1,
                "style": "IPY_MODEL_2911ea35cdb14b0c8269f152684b30ee",
                "value": 4326
            }
        },
        "f160933d66744598ba8df9f7500e3d9b": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "68c3f3f33c654647a57e55b78609c962": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "5f5f7fe9f7144553ac61e02a7b777e50": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_f160933d66744598ba8df9f7500e3d9b",
                "step": 1,
                "style": "IPY_MODEL_68c3f3f33c654647a57e55b78609c962",
                "value": 3577
            }
        },
        "0a3d2f1e55d6443c84b2e90769be8cb1": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "aee4865f696c45998691677e1a0f257f": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "1dec1b965e9d45ffb679245a81e83057": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "38ff8dcea9684f0fad2c7e95291353d5": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "f4c6ad4d4a674e128e94e3f31ffec248": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "84aad67908df44849aab4150a4502b1e": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_3e1e801917f844b587f4c55848d6fd64",
                    "IPY_MODEL_33651349c89c489facbf8d65cccbc66f",
                    "IPY_MODEL_b2f973f795a34b26a7546754d0715f04"
                ],
                "default_style": "IPY_MODEL_aee4865f696c45998691677e1a0f257f",
                "dragging_style": "IPY_MODEL_1dec1b965e9d45ffb679245a81e83057",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_0a3d2f1e55d6443c84b2e90769be8cb1"
                ],
                "layout": "IPY_MODEL_38ff8dcea9684f0fad2c7e95291353d5",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_aee4865f696c45998691677e1a0f257f",
                "west": 180,
                "zoom": 5
            }
        },
        "3e1e801917f844b587f4c55848d6fd64": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "33651349c89c489facbf8d65cccbc66f": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "b2f973f795a34b26a7546754d0715f04": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "7bb1b8639baf499d82510d8a8bc661e4": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "e41eed47397e4e249d2e97c5e6343e7d": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "472a455b9e09423a9a12610bd38766e5": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min X:",
                "layout": "IPY_MODEL_7bb1b8639baf499d82510d8a8bc661e4",
                "step": null,
                "style": "IPY_MODEL_e41eed47397e4e249d2e97c5e6343e7d"
            }
        },
        "01725535bbc8456cbc7311af8c6865ed": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "2630e0e52934457097930809bf8edcfc": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "6626f740a0404eb7b6f7e324c90ebfa0": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Y:",
                "layout": "IPY_MODEL_01725535bbc8456cbc7311af8c6865ed",
                "step": null,
                "style": "IPY_MODEL_2630e0e52934457097930809bf8edcfc"
            }
        },
        "1cd42e8c519840a487f0e5308988cc36": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "d65e6c8487c544df89752af794774310": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "d7ed4f00a8ef4519b8f532e03043b7d6": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max X:",
                "layout": "IPY_MODEL_1cd42e8c519840a487f0e5308988cc36",
                "step": null,
                "style": "IPY_MODEL_d65e6c8487c544df89752af794774310"
            }
        },
        "88dd5e04502048acb3963851dcdb10e4": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "142f79c9e3cc4aa0ae2f4bfa0a9d435f": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "e21a2976fd664d9dad13e152fe3312d9": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Y:",
                "layout": "IPY_MODEL_88dd5e04502048acb3963851dcdb10e4",
                "step": null,
                "style": "IPY_MODEL_142f79c9e3cc4aa0ae2f4bfa0a9d435f"
            }
        },
        "c8d9ff0b1fc849bd8eaeca5a2fa33edd": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "f996bfb677cc4b6c80083a5e2cce7128": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "a79afd6af4a04ee494d4686449899486": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "43b05b55f58847e59efaf88803598659": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "265182d26ade4d59818cdfeaa782487f": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "7dd94bce12f14cd8a09f1f37433af852": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    354.02343750000006
                ],
                "controls": [
                    "IPY_MODEL_6a140df9da964d198f5169bf51011d82",
                    "IPY_MODEL_633fe9e092d74faebc91b087c56bd761",
                    "IPY_MODEL_366458ed58b243b884a44c3ad55802ac"
                ],
                "default_style": "IPY_MODEL_f996bfb677cc4b6c80083a5e2cce7128",
                "dragging_style": "IPY_MODEL_a79afd6af4a04ee494d4686449899486",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_c8d9ff0b1fc849bd8eaeca5a2fa33edd"
                ],
                "layout": "IPY_MODEL_43b05b55f58847e59efaf88803598659",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_f996bfb677cc4b6c80083a5e2cce7128",
                "west": 180,
                "zoom": 5
            }
        },
        "6a140df9da964d198f5169bf51011d82": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "633fe9e092d74faebc91b087c56bd761": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "366458ed58b243b884a44c3ad55802ac": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "7aab53b501074744bfa87381a84ade82": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "d718a4d3186944b5b4bdda741407ec68": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "a1b074d2723f4ffbbd2c579e38b4dd5d": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "disabled": true,
                "layout": "IPY_MODEL_7aab53b501074744bfa87381a84ade82",
                "step": 1,
                "style": "IPY_MODEL_d718a4d3186944b5b4bdda741407ec68",
                "value": 4326
            }
        },
        "45707e0a32fc472cac244754adc11b6a": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "7ed98257403c47d2ba1465acb4675522": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "ad1e203c1ce64ae181f350e86ae89ccf": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_45707e0a32fc472cac244754adc11b6a",
                "step": 1,
                "style": "IPY_MODEL_7ed98257403c47d2ba1465acb4675522",
                "value": 3577
            }
        },
        "155f8489cd184145a2d68b4d644c557a": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "1ed5507d549845b991183ba0b2f2b85d": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "4f80367a43ef429d96be3af9c520a91d": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Longitude:",
                "layout": "IPY_MODEL_155f8489cd184145a2d68b4d644c557a",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_1ed5507d549845b991183ba0b2f2b85d",
                "value": -10.542806999999982
            }
        },
        "260d6d1126b54e9da02bdb1f610e0bb0": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "eae04647ca3e491aac0be5beffc9f9c1": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "3430839cd059432f9aa36e350dc65b2b": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Latitude:",
                "layout": "IPY_MODEL_260d6d1126b54e9da02bdb1f610e0bb0",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_eae04647ca3e491aac0be5beffc9f9c1",
                "value": 46.397147
            }
        },
        "4ec01ee760ef40cdb84419411fcdc83d": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "539cd1cb94764163b236a9208aca6823": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "bdbd4ef7c1204a4dbe15570e28a64058": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Longitude:",
                "layout": "IPY_MODEL_4ec01ee760ef40cdb84419411fcdc83d",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_539cd1cb94764163b236a9208aca6823",
                "value": 5.148829999999975
            }
        },
        "c948e32fd3684e729e4eb4281db464c4": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "1f1681d142f34269a59f7b2794351839": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "5d04ee59a05445b2b911a4ab1daa004a": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Latitude:",
                "layout": "IPY_MODEL_c948e32fd3684e729e4eb4281db464c4",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_1f1681d142f34269a59f7b2794351839",
                "value": 52.873274
            }
        },
        "472e7200df79447bae4fcebb3501d9dd": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "09f710312ae84f669d36d9237224d06f": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "4e9e60f0574443a4a3452789ea1c5638": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min X:",
                "layout": "IPY_MODEL_472e7200df79447bae4fcebb3501d9dd",
                "step": null,
                "style": "IPY_MODEL_09f710312ae84f669d36d9237224d06f",
                "value": -17267828.476042427
            }
        },
        "60c50afa8cf74d0ea9222f756074e2aa": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "30d36233f6c842a6a4e5d7362173024c": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "53b482b250db44f0b996caab604c7da0": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Y:",
                "layout": "IPY_MODEL_60c50afa8cf74d0ea9222f756074e2aa",
                "step": null,
                "style": "IPY_MODEL_30d36233f6c842a6a4e5d7362173024c",
                "value": -7002995.638395221
            }
        },
        "6d8f9cd6072f45fe952bf2f1a7d7bede": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "717b2d22d9284b13aa2072280b55c5e2": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "2942984f1bf642b4ae3991f0eeab5bd3": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max X:",
                "layout": "IPY_MODEL_6d8f9cd6072f45fe952bf2f1a7d7bede",
                "step": null,
                "style": "IPY_MODEL_717b2d22d9284b13aa2072280b55c5e2",
                "value": -16389494.2723409
            }
        },
        "cd16c7c8cad94e4db8572f27f43a3c74": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "3fcfc40f997a455fbe69f1165e5b2020": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "e793962200b7486b866653f6523e0cc1": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Y:",
                "layout": "IPY_MODEL_cd16c7c8cad94e4db8572f27f43a3c74",
                "step": null,
                "style": "IPY_MODEL_3fcfc40f997a455fbe69f1165e5b2020",
                "value": -4763941.026321793
            }
        },
        "97e4f990c24b424285a9d878992a61a6": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "975fef8220294327b50f7bc43c108dcb": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "ebcda031178b4d629c03447a15f312bc": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "58e8015fc8394d94acf4e31061e9ad46": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "0a1201cc28394576bad1015156937228": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "2030d83147684f118fdaea084a7cce42": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    50.00773901463687,
                    140.00976562500003
                ],
                "controls": [
                    "IPY_MODEL_424e0a7abb794651be909289cb974ccd",
                    "IPY_MODEL_17a312fcbe8e4a97ae1b4d5dda7c2195",
                    "IPY_MODEL_4bfb0e6da919487796c9c9542d65dbc4"
                ],
                "default_style": "IPY_MODEL_975fef8220294327b50f7bc43c108dcb",
                "dragging_style": "IPY_MODEL_ebcda031178b4d629c03447a15f312bc",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_97e4f990c24b424285a9d878992a61a6"
                ],
                "layout": "IPY_MODEL_58e8015fc8394d94acf4e31061e9ad46",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_975fef8220294327b50f7bc43c108dcb",
                "west": 180,
                "zoom": 5
            }
        },
        "424e0a7abb794651be909289cb974ccd": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "17a312fcbe8e4a97ae1b4d5dda7c2195": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "4bfb0e6da919487796c9c9542d65dbc4": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "d0c6db7013d74110b56e88f53e5058cc": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "fce03a9c426248ad9c7251f89f0b16c8": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "b74373166450478f86fbb483a7e3c031": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "a41d4ca0fac641a5b689bc21097d5d37": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "dc11f1e3396e4153954ab8cfdbffaed5": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "a235895b3679473abeab838978d42ea2": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -40.01078714046552,
                    140.00976562500003
                ],
                "controls": [
                    "IPY_MODEL_6f4732ac842f45c1bfef0b4862923b55",
                    "IPY_MODEL_28fefee7f6444a048487684acdbe5266",
                    "IPY_MODEL_4b06a64c4c5b415da3e219c62bb52506"
                ],
                "default_style": "IPY_MODEL_fce03a9c426248ad9c7251f89f0b16c8",
                "dragging_style": "IPY_MODEL_b74373166450478f86fbb483a7e3c031",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_d0c6db7013d74110b56e88f53e5058cc"
                ],
                "layout": "IPY_MODEL_a41d4ca0fac641a5b689bc21097d5d37",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_fce03a9c426248ad9c7251f89f0b16c8",
                "west": 180,
                "zoom": 5
            }
        },
        "6f4732ac842f45c1bfef0b4862923b55": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "28fefee7f6444a048487684acdbe5266": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "4b06a64c4c5b415da3e219c62bb52506": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "f7f973d7231a469692c8d440a5649b97": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "0a0c0f0b39a84ca9829e8579ad9544bc": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "0d3dda9acf7248538adbb1f6ee7e2f7d": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "df5aa905dffe47a484fa508d6e018e19": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "a9d605c8d2474dc7b024ba725345b06b": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "61a3464f086d4de1b108a6d8b81490c1": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -39.97712009843963,
                    140.00976562500003
                ],
                "controls": [
                    "IPY_MODEL_ecd66bfbc617498e8d5329bbe608fe6f",
                    "IPY_MODEL_af68a8c15512483bb9bd13668707f868",
                    "IPY_MODEL_98f11d40a7164a2c823b945ab23feea2"
                ],
                "default_style": "IPY_MODEL_0a0c0f0b39a84ca9829e8579ad9544bc",
                "dragging_style": "IPY_MODEL_0d3dda9acf7248538adbb1f6ee7e2f7d",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_f7f973d7231a469692c8d440a5649b97"
                ],
                "layout": "IPY_MODEL_df5aa905dffe47a484fa508d6e018e19",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_0a0c0f0b39a84ca9829e8579ad9544bc",
                "west": 180,
                "zoom": 4
            }
        },
        "ecd66bfbc617498e8d5329bbe608fe6f": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "af68a8c15512483bb9bd13668707f868": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "98f11d40a7164a2c823b945ab23feea2": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "8c11e49242fa46dc9cbad26ea3ab5f2e": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "fe1994a585714555936aa294db6e306e": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "9dc23811ce484c15acc9a1434fc78bcc": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "816af7fab4c7449dbb451ce97e421540": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "d9da2a56a7e64ae6af7ba4d1467b5f34": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "ea615c3c1acb4b8d82f4ba331f356b34": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -9.96885060854611,
                    140.00976562500003
                ],
                "controls": [
                    "IPY_MODEL_7b4d8552971b421baccd4bd32dfce3fc",
                    "IPY_MODEL_19b8eae4c4fa404e93c76e6ed40c55c5",
                    "IPY_MODEL_61d53b4a27dc482d8533062a4b33cb50"
                ],
                "default_style": "IPY_MODEL_fe1994a585714555936aa294db6e306e",
                "dragging_style": "IPY_MODEL_9dc23811ce484c15acc9a1434fc78bcc",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_8c11e49242fa46dc9cbad26ea3ab5f2e"
                ],
                "layout": "IPY_MODEL_816af7fab4c7449dbb451ce97e421540",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_fe1994a585714555936aa294db6e306e",
                "west": 180,
                "zoom": 4
            }
        },
        "7b4d8552971b421baccd4bd32dfce3fc": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "19b8eae4c4fa404e93c76e6ed40c55c5": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "61d53b4a27dc482d8533062a4b33cb50": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "c4740848d0254a3099b5ab6b46e999ce": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "19aeb5f1946d4a8f91c455139d6cef12": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "d3551c8c0e5243a5b2f745bd1673d894": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "a830f2a89cdd468f93bb406232afdda9": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "b1b53f56afa140578bb63c0da6130736": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "7d10b7585bc246189d9592267c9aaf05": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -19.973348786110602,
                    140.00976562500003
                ],
                "controls": [
                    "IPY_MODEL_fe17c7d3032444078dad33e0fdb2d412",
                    "IPY_MODEL_0f11d26c29e349e8b58a821db6a78b8f",
                    "IPY_MODEL_9132ac1e4e194784adf09ed3ff390d38"
                ],
                "default_style": "IPY_MODEL_19aeb5f1946d4a8f91c455139d6cef12",
                "dragging_style": "IPY_MODEL_d3551c8c0e5243a5b2f745bd1673d894",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_c4740848d0254a3099b5ab6b46e999ce"
                ],
                "layout": "IPY_MODEL_a830f2a89cdd468f93bb406232afdda9",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_19aeb5f1946d4a8f91c455139d6cef12",
                "west": 180,
                "zoom": 4
            }
        },
        "fe17c7d3032444078dad33e0fdb2d412": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "0f11d26c29e349e8b58a821db6a78b8f": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "9132ac1e4e194784adf09ed3ff390d38": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "38dc8e59c6c548a19e7337f2313cdddd": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "b5778702e3474ab8a05f370e4ea9ef26": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "ec67abfe8c1a441693c8d389b73b071e": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "440c97a4f5b84dcdac0a88575356da19": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "edd90281f40b46a5b8933726d81b3425": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "b33a5d8982be4e7a8947d1b9642ded02": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -26.194876675795218,
                    132.45117187500003
                ],
                "controls": [
                    "IPY_MODEL_d2fdfd034b93418cba77a44ad3611395",
                    "IPY_MODEL_3d9570f0a44543e6bf195730a258a11e",
                    "IPY_MODEL_2c2d81a72e8e4daaa7ab8ffb998ffc13"
                ],
                "default_style": "IPY_MODEL_b5778702e3474ab8a05f370e4ea9ef26",
                "dragging_style": "IPY_MODEL_ec67abfe8c1a441693c8d389b73b071e",
                "east": -180,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_38dc8e59c6c548a19e7337f2313cdddd"
                ],
                "layout": "IPY_MODEL_440c97a4f5b84dcdac0a88575356da19",
                "modisdate": "yesterday",
                "north": -90,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": 90,
                "style": "IPY_MODEL_b5778702e3474ab8a05f370e4ea9ef26",
                "west": 180,
                "zoom": 4
            }
        },
        "d2fdfd034b93418cba77a44ad3611395": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "3d9570f0a44543e6bf195730a258a11e": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "2c2d81a72e8e4daaa7ab8ffb998ffc13": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "84ce240b6e984f17948df96606f58014": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "d4fb2c63a0d94154b56dbe4085858888": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "59d05510a23c4b3681c817f98ad11ea1": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "62101a8774b3402c8078e3d422108608": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "1ea47434fe984f07a847d309748d3f57": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "9eab3ea26c084f3ab28bf2d04830d45f": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -25,
                    140
                ],
                "controls": [
                    "IPY_MODEL_71dbce1598da47968385b42f8e59bc87",
                    "IPY_MODEL_1c37e465bf0641b68b6c66aef3040a69",
                    "IPY_MODEL_ef6aff72275c4623aad3c4352444b9c3"
                ],
                "default_style": "IPY_MODEL_d4fb2c63a0d94154b56dbe4085858888",
                "dragging_style": "IPY_MODEL_59d05510a23c4b3681c817f98ad11ea1",
                "east": 180.87890625000003,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_84ce240b6e984f17948df96606f58014"
                ],
                "layout": "IPY_MODEL_62101a8774b3402c8078e3d422108608",
                "modisdate": "yesterday",
                "north": -8.233237111274553,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": -39.77476948529546,
                "style": "IPY_MODEL_1ea47434fe984f07a847d309748d3f57",
                "west": 99.05273437500001,
                "zoom": 4
            }
        },
        "71dbce1598da47968385b42f8e59bc87": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "1c37e465bf0641b68b6c66aef3040a69": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "ef6aff72275c4623aad3c4352444b9c3": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "9064c60c7f34432db7165ec4bbffa5c1": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "f5c804a2088348b398b3f4d022fcb02f": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "f7dce4c53abf43c29ae1762837e1935c": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "disabled": true,
                "layout": "IPY_MODEL_9064c60c7f34432db7165ec4bbffa5c1",
                "step": 1,
                "style": "IPY_MODEL_f5c804a2088348b398b3f4d022fcb02f",
                "value": 4326
            }
        },
        "f3f13d49a37a4b5a96e74789a5fbade0": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "1b346266bfe740d4bedf76ebc8c6adae": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "d8a94d929c7c4bdb915de569d8213699": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_f3f13d49a37a4b5a96e74789a5fbade0",
                "step": 1,
                "style": "IPY_MODEL_1b346266bfe740d4bedf76ebc8c6adae",
                "value": 3577
            }
        },
        "6a0161c6bc59417195afdedb9126c7c9": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "6059ba0f86754b59baeb2af698ad26ea": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "a1d00adc8e084293bde5fd3681144e62": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Longitude:",
                "layout": "IPY_MODEL_6a0161c6bc59417195afdedb9126c7c9",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_6059ba0f86754b59baeb2af698ad26ea"
            }
        },
        "364f2e952b304ac38779cc9b0885d1f4": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "bacdbc5129f04c189abd2293058b0e50": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "edd5153f9ff74a478eb8658f54694620": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Latitude:",
                "layout": "IPY_MODEL_364f2e952b304ac38779cc9b0885d1f4",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_bacdbc5129f04c189abd2293058b0e50"
            }
        },
        "551c37609d704af9940708857e1db15e": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "f717c3e04043432eb68da96fbe800320": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "953b2ccabdd4476599a8f19e926597d2": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Longitude:",
                "layout": "IPY_MODEL_551c37609d704af9940708857e1db15e",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_f717c3e04043432eb68da96fbe800320"
            }
        },
        "e91391e0fb9241988b578cb28a5fa823": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "10be216bb43f405a839cf3f9c8d23f25": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "aafd3293645145e395ad52b897735393": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Latitude:",
                "layout": "IPY_MODEL_e91391e0fb9241988b578cb28a5fa823",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_10be216bb43f405a839cf3f9c8d23f25"
            }
        },
        "3044e24c9afa4addbca68ba477dfa356": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "455378e126134c33aa4b86dd7365ba56": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "c3eb1c074eb6480cafb4cf658eb15fc3": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min X:",
                "layout": "IPY_MODEL_3044e24c9afa4addbca68ba477dfa356",
                "step": null,
                "style": "IPY_MODEL_455378e126134c33aa4b86dd7365ba56"
            }
        },
        "8df8e886ec7b421e92158776e843016f": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "223c65b352e3446fab59c9081e9b7a0c": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "be3be6d8c18e45209c46f24d899d95c4": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Y:",
                "layout": "IPY_MODEL_8df8e886ec7b421e92158776e843016f",
                "step": null,
                "style": "IPY_MODEL_223c65b352e3446fab59c9081e9b7a0c"
            }
        },
        "020ae4ee2c544693ab3c6db024476844": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "f44441e79ad94c8884975dfb2c875fe0": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "bc32bee6364c4044a35a28120201472d": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max X:",
                "layout": "IPY_MODEL_020ae4ee2c544693ab3c6db024476844",
                "step": null,
                "style": "IPY_MODEL_f44441e79ad94c8884975dfb2c875fe0"
            }
        },
        "e9262c2309e94d1aa8f06c6a730031c7": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "c249a3a1fed7416a821917d7438f7a0d": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "efa4be2e950c41c8a4aa36a887451e73": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Y:",
                "layout": "IPY_MODEL_e9262c2309e94d1aa8f06c6a730031c7",
                "step": null,
                "style": "IPY_MODEL_c249a3a1fed7416a821917d7438f7a0d"
            }
        },
        "7dcf06b563524912ac642859e493d946": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "232ba9c8d7be421c8a593fcdcccb3b88": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "d49e1043955a408fa59b38d4dc398925": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "disabled": true,
                "layout": "IPY_MODEL_7dcf06b563524912ac642859e493d946",
                "step": 1,
                "style": "IPY_MODEL_232ba9c8d7be421c8a593fcdcccb3b88",
                "value": 4326
            }
        },
        "76e5ad566f074199b32d68b6701ce1e2": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "d8c2b7481bfe489c85b50b0db327bc10": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "58ca9126997b4d43a4ac9acbc8d8a983": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_76e5ad566f074199b32d68b6701ce1e2",
                "step": 1,
                "style": "IPY_MODEL_d8c2b7481bfe489c85b50b0db327bc10",
                "value": 3577
            }
        },
        "d40cf5f6db3f47948166a445a4755ced": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "bc7ac25322984033aec9ed26b4346adc": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "ebca7c7eb1de456b92db9c7d663dc437": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Longitude:",
                "layout": "IPY_MODEL_d40cf5f6db3f47948166a445a4755ced",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_bc7ac25322984033aec9ed26b4346adc"
            }
        },
        "3b3858e25d9c4fa699956337b0245588": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "ea7c918bb0d14557bfea12e31aec4ba8": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "f0a539e657fc41e5823939f59efd1492": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Latitude:",
                "layout": "IPY_MODEL_3b3858e25d9c4fa699956337b0245588",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_ea7c918bb0d14557bfea12e31aec4ba8"
            }
        },
        "800de99b22494399b33858a4473c390d": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "46f86da99c5f4513a2de6f8e7ea2cea5": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "5327caf87620499586cf67edb33d27b2": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Longitude:",
                "layout": "IPY_MODEL_800de99b22494399b33858a4473c390d",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_46f86da99c5f4513a2de6f8e7ea2cea5"
            }
        },
        "0dcceb93755b4347b1684809cd396775": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "93d8c1a46eaa4275a09c667407c965c0": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "2b4d5bdcf9984d6ca8feb6900862f752": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Latitude:",
                "layout": "IPY_MODEL_0dcceb93755b4347b1684809cd396775",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_93d8c1a46eaa4275a09c667407c965c0"
            }
        },
        "67235fb7b4ff4ac9a0908be0474427ec": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "1fe1fc1d57f64b02816c52df9671cafd": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "9a1dcbdec4834ccba8df85660637c12b": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min X:",
                "layout": "IPY_MODEL_67235fb7b4ff4ac9a0908be0474427ec",
                "step": null,
                "style": "IPY_MODEL_1fe1fc1d57f64b02816c52df9671cafd"
            }
        },
        "b0c14babd83d45038a54b95217d89dad": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "02f7fb8d81684ae69943614ea2a26582": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "67a4acc69a894b11913b2d0b611b5b94": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Y:",
                "layout": "IPY_MODEL_b0c14babd83d45038a54b95217d89dad",
                "step": null,
                "style": "IPY_MODEL_02f7fb8d81684ae69943614ea2a26582"
            }
        },
        "3aa2c2da91284013961e4b84094f595c": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "49442203069f44b083a344f85186ab4f": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "12de78fa58f94cf8969d920f7f472a97": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max X:",
                "layout": "IPY_MODEL_3aa2c2da91284013961e4b84094f595c",
                "step": null,
                "style": "IPY_MODEL_49442203069f44b083a344f85186ab4f"
            }
        },
        "23161bb26b2d4bcb9feb8c7d8c6378e6": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "8f0ce130c22d4e1da66ac0e10f14bfcf": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "6f35f62fac0d4bb3a5067aab241422e6": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Y:",
                "layout": "IPY_MODEL_23161bb26b2d4bcb9feb8c7d8c6378e6",
                "step": null,
                "style": "IPY_MODEL_8f0ce130c22d4e1da66ac0e10f14bfcf"
            }
        },
        "a0bd036a6a8d4bf88ee2534b2d108d75": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "553bc62094a143ecb684139cdc4a4ba8": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "19b7ce1f69d84a0e958ad72fa7da0696": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "disabled": true,
                "layout": "IPY_MODEL_a0bd036a6a8d4bf88ee2534b2d108d75",
                "step": 1,
                "style": "IPY_MODEL_553bc62094a143ecb684139cdc4a4ba8",
                "value": 4326
            }
        },
        "86fe9122284e43a3abdd0cb5aa178fea": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "47125a16671b4f60a3bb2e2f8975d71e": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "da8e49e84b0c4034978b7a90dde2eda2": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_86fe9122284e43a3abdd0cb5aa178fea",
                "step": 1,
                "style": "IPY_MODEL_47125a16671b4f60a3bb2e2f8975d71e",
                "value": 3577
            }
        },
        "e4d985600f5e4b208d20e3908883e6e2": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "85267498c7134ca598e74345e802c46b": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "74970054367f4b539e367e8e64c1913f": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "0e09e7beba134cadac3809093e751202": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "dd3295c27dc34039bacf7be6364865f8": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "e19f976054bb45d18e84d9638566fa02": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -25,
                    140
                ],
                "controls": [
                    "IPY_MODEL_4f37342c0537482a90783bae5fb0afc5",
                    "IPY_MODEL_c0a6a11668214098944760666d7f9892",
                    "IPY_MODEL_8ca08b044e104f09bacb94e2ff6fef4b"
                ],
                "default_style": "IPY_MODEL_85267498c7134ca598e74345e802c46b",
                "dragging_style": "IPY_MODEL_74970054367f4b539e367e8e64c1913f",
                "east": 180.87890625000003,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_e4d985600f5e4b208d20e3908883e6e2"
                ],
                "layout": "IPY_MODEL_0e09e7beba134cadac3809093e751202",
                "modisdate": "yesterday",
                "north": -8.233237111274553,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": -39.77476948529546,
                "style": "IPY_MODEL_dd3295c27dc34039bacf7be6364865f8",
                "west": 99.05273437500001,
                "zoom": 4
            }
        },
        "4f37342c0537482a90783bae5fb0afc5": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "c0a6a11668214098944760666d7f9892": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "8ca08b044e104f09bacb94e2ff6fef4b": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "d832e805a9aa4185b01cf3be213b6f20": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "dbd182f125e04b00ad9a7d8672ed49cd": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "8fb87e3b85974a589144dd76ab056db1": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "disabled": true,
                "layout": "IPY_MODEL_d832e805a9aa4185b01cf3be213b6f20",
                "step": 1,
                "style": "IPY_MODEL_dbd182f125e04b00ad9a7d8672ed49cd",
                "value": 4326
            }
        },
        "29773be5eaa244879e9d4dc315215278": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "7c94c5081cb94255b21e1665a712bb4e": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "984be4ab07f84675b85a7f88a74af715": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_29773be5eaa244879e9d4dc315215278",
                "step": 1,
                "style": "IPY_MODEL_7c94c5081cb94255b21e1665a712bb4e",
                "value": 3577
            }
        },
        "eaef2c8d984b423e8917470edb80b810": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "69a3cd0c55c4488899af19242bef6d9b": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "346291d9b6be4f9389dc53001c9a327d": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Longitude:",
                "layout": "IPY_MODEL_eaef2c8d984b423e8917470edb80b810",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_69a3cd0c55c4488899af19242bef6d9b",
                "value": -180
            }
        },
        "cb81f7ac5c484a3e8c57a86a71c0bc32": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "bba5f6f94abf4edf8dd85b622133124e": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "69de1d5c7cf14cbfa09807c0ceac1b0d": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Latitude:",
                "layout": "IPY_MODEL_cb81f7ac5c484a3e8c57a86a71c0bc32",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_bba5f6f94abf4edf8dd85b622133124e",
                "value": -27.215556
            }
        },
        "f571e8351b2f470a804875fc9db55e1f": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "6e8c1d0d215144f7be6da57e268f28b2": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "4b780d53f7dc42b4adc862c073314660": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Longitude:",
                "layout": "IPY_MODEL_f571e8351b2f470a804875fc9db55e1f",
                "max": 180,
                "min": -180,
                "step": null,
                "style": "IPY_MODEL_6e8c1d0d215144f7be6da57e268f28b2",
                "value": -180
            }
        },
        "7d648943fca54582964864ea58c9773f": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "1d1276c4c2004206a256f31dd67a5a17": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "ccb3de83c93a42c694cdc190ffe5561c": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Latitude:",
                "layout": "IPY_MODEL_7d648943fca54582964864ea58c9773f",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_1d1276c4c2004206a256f31dd67a5a17",
                "value": -12.726084
            }
        },
        "4ff14acfc6ed4283815b509e0e56571e": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "1f0a948efe5d488db02f75c11bcecd4b": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "a3edb906e88241e89d0b79bc8d3d2dd8": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min X:",
                "layout": "IPY_MODEL_4ff14acfc6ed4283815b509e0e56571e",
                "step": null,
                "style": "IPY_MODEL_1f0a948efe5d488db02f75c11bcecd4b",
                "value": -1960193.377608321
            }
        },
        "4027932af4b04cb4b5a0cf3bfaac8eb8": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "b6ba5d98131944058556ba6c96c74217": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "2f31bc688d904a0eafda6493a271a50b": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Y:",
                "layout": "IPY_MODEL_4027932af4b04cb4b5a0cf3bfaac8eb8",
                "step": null,
                "style": "IPY_MODEL_b6ba5d98131944058556ba6c96c74217",
                "value": -3105638.000965851
            }
        },
        "c99dfea126a94fed9e49b8b74f218aaf": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "802da3ad156647c3abe092f0aa89b422": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "69704a5761f143d488d3b32773559bac": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max X:",
                "layout": "IPY_MODEL_c99dfea126a94fed9e49b8b74f218aaf",
                "step": null,
                "style": "IPY_MODEL_802da3ad156647c3abe092f0aa89b422",
                "value": 1078792.165974399
            }
        },
        "28153246420b4035bb4ee6288232c08f": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "02b8438e14fe4e67b79154164d80eb80": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "f087b8dd63fb41a1aa7f573850dffc27": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Y:",
                "layout": "IPY_MODEL_28153246420b4035bb4ee6288232c08f",
                "step": null,
                "style": "IPY_MODEL_02b8438e14fe4e67b79154164d80eb80",
                "value": -1384459.1297494501
            }
        },
        "c4eb74dad1034765820ea25ee4ceaffe": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "31d53d728bd843579cbe828bc1a96115": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "8beae486cde9491ca2fe8ab003930ac1": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "7418b904f0fd4a27987ae601435cf020": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "a2e01da2848842e7913ee100f8d0f5be": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "e5693608164e4bf8abe27c249fe9c0a9": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -25,
                    140
                ],
                "controls": [
                    "IPY_MODEL_fcbcd739637b47bd822fa9991c586f16",
                    "IPY_MODEL_d538e578144544d1a9325d1192bd9210",
                    "IPY_MODEL_5e7751536048436f8c84af49bfea1656"
                ],
                "default_style": "IPY_MODEL_31d53d728bd843579cbe828bc1a96115",
                "dragging_style": "IPY_MODEL_8beae486cde9491ca2fe8ab003930ac1",
                "east": 180.87890625000003,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_c4eb74dad1034765820ea25ee4ceaffe"
                ],
                "layout": "IPY_MODEL_7418b904f0fd4a27987ae601435cf020",
                "modisdate": "yesterday",
                "north": -8.233237111274553,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": -39.77476948529546,
                "style": "IPY_MODEL_a2e01da2848842e7913ee100f8d0f5be",
                "west": 99.05273437500001,
                "zoom": 4
            }
        },
        "fcbcd739637b47bd822fa9991c586f16": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "d538e578144544d1a9325d1192bd9210": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "5e7751536048436f8c84af49bfea1656": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "9ba681f9b16e4f2db953881f7b76d1a2": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "f5a5b09aa318453896af1bf7353795f4": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "9e394142e2d3472c8b2b0c0cfbd35f14": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Longitude:",
                "layout": "IPY_MODEL_9ba681f9b16e4f2db953881f7b76d1a2",
                "max": 380,
                "min": -380,
                "step": null,
                "style": "IPY_MODEL_f5a5b09aa318453896af1bf7353795f4",
                "value": -248.115234
            }
        },
        "c9a072418040454b9d35fdd81b2926bd": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "f1a5a42ae451446b99b384cbaceceea0": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "9c0622dd56954f7482080803d1ea882d": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Latitude:",
                "layout": "IPY_MODEL_c9a072418040454b9d35fdd81b2926bd",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_f1a5a42ae451446b99b384cbaceceea0",
                "value": -27.215556
            }
        },
        "1c8df6e6b7644d2a8aff257c217cc18c": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "53c220420c1a43e8acc24f6ff6140f31": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "c852091683c84680ae926c2ca14a1a64": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Longitude:",
                "layout": "IPY_MODEL_1c8df6e6b7644d2a8aff257c217cc18c",
                "max": 380,
                "min": -380,
                "step": null,
                "style": "IPY_MODEL_53c220420c1a43e8acc24f6ff6140f31",
                "value": -218.22209
            }
        },
        "67c0c47b340e4ebf8facfc924c0707ac": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "9dcf908aa867452ca0ecc4cc25e84ca9": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "978303c9fef448d78b62171052e7ec0b": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Latitude:",
                "layout": "IPY_MODEL_67c0c47b340e4ebf8facfc924c0707ac",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_9dcf908aa867452ca0ecc4cc25e84ca9",
                "value": -12.726084
            }
        },
        "956522d5abc54f4ca74ea3c1e7967a9e": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "b9c668ba3e3f4bbf909b9fc7f3b8821d": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "00d05f82ac174587bfe3434334a5ac08": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "31ec3dc1de2245e9ab033a4ea2cfeaa7": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "11ac8784006c4af0bd4aa96dd628284e": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "f5ee38aa4346408abbf037cee531f359": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -25,
                    140
                ],
                "controls": [
                    "IPY_MODEL_40a7bb33e5df4fba82ad238ee62fef4f",
                    "IPY_MODEL_ec0f0e0c43e9471089f0f62b008e2805",
                    "IPY_MODEL_1e23c747e6a344598217f63170982832"
                ],
                "default_style": "IPY_MODEL_b9c668ba3e3f4bbf909b9fc7f3b8821d",
                "dragging_style": "IPY_MODEL_00d05f82ac174587bfe3434334a5ac08",
                "east": 180.87890625000003,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_956522d5abc54f4ca74ea3c1e7967a9e"
                ],
                "layout": "IPY_MODEL_31ec3dc1de2245e9ab033a4ea2cfeaa7",
                "modisdate": "yesterday",
                "north": -8.233237111274553,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": -39.77476948529546,
                "style": "IPY_MODEL_11ac8784006c4af0bd4aa96dd628284e",
                "west": 99.05273437500001,
                "zoom": 4
            }
        },
        "40a7bb33e5df4fba82ad238ee62fef4f": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "ec0f0e0c43e9471089f0f62b008e2805": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "1e23c747e6a344598217f63170982832": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "03437a4d17a540f49ac9b171ca254bb5": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "57edeb1844da48b2a644b1c6b742c3cd": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "e8ad8de78717439fa1102521081c6be4": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Input EPSG:",
                "disabled": true,
                "layout": "IPY_MODEL_03437a4d17a540f49ac9b171ca254bb5",
                "step": 1,
                "style": "IPY_MODEL_57edeb1844da48b2a644b1c6b742c3cd",
                "value": 4326
            }
        },
        "86f65e04d7254115a6543e164a8b4ea5": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "9ca1d85211034644aa97b4abec5ace2f": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "22af92d4715b447f8a39b5e817993a94": {
            "model_name": "IntTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Output EPSG:",
                "layout": "IPY_MODEL_86f65e04d7254115a6543e164a8b4ea5",
                "step": 1,
                "style": "IPY_MODEL_9ca1d85211034644aa97b4abec5ace2f",
                "value": 3577
            }
        },
        "36399af04c0841dfba5074af706b6ca3": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "4e7efa5a9a2c449e95f33c1432e890fd": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "44bc88f7560e43a0ba44fc0fd77b9718": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Longitude:",
                "layout": "IPY_MODEL_36399af04c0841dfba5074af706b6ca3",
                "max": 380,
                "min": -380,
                "step": null,
                "style": "IPY_MODEL_4e7efa5a9a2c449e95f33c1432e890fd",
                "value": 112.324219
            }
        },
        "25eb1a4d339045f897addec4311980a3": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "38f2638746fc465e990e78746d26e4d3": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "504859767e364c31ab54ed2b7cb2ad66": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Latitude:",
                "layout": "IPY_MODEL_25eb1a4d339045f897addec4311980a3",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_38f2638746fc465e990e78746d26e4d3",
                "value": -32.424022
            }
        },
        "2d843b8147304981adde7feeb6fd38a0": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "75a2c5e9f474460983e0068c3ccda479": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "8afdb925455e4eff87a3681b796b83af": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Longitude:",
                "layout": "IPY_MODEL_2d843b8147304981adde7feeb6fd38a0",
                "max": 380,
                "min": -380,
                "step": null,
                "style": "IPY_MODEL_75a2c5e9f474460983e0068c3ccda479",
                "value": 123.932519
            }
        },
        "d1c93ca93baf4cdfb1658a939985dd9e": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "e69ab3d8cada42df8503acb1ce5e4f46": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "1c243b4df49d4e0caac52e7d7ac3030d": {
            "model_name": "BoundedFloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Latitude:",
                "layout": "IPY_MODEL_d1c93ca93baf4cdfb1658a939985dd9e",
                "max": 90,
                "min": -90,
                "step": null,
                "style": "IPY_MODEL_e69ab3d8cada42df8503acb1ce5e4f46",
                "value": -24.287027
            }
        },
        "bc056ad12f6f4bd3bb17eabe969d9f5f": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "4276cdc6658148d1bf9273787187b11b": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "2a0f9415f6104050a8a87b76233eafce": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min X:",
                "layout": "IPY_MODEL_bc056ad12f6f4bd3bb17eabe969d9f5f",
                "step": null,
                "style": "IPY_MODEL_4276cdc6658148d1bf9273787187b11b",
                "value": -1828162.5787569045
            }
        },
        "4f2567dd381441d9a0ee3725c1a63098": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "f5954dd98e924842ab3765f1e8f31e06": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "4545b80ac12948ba84e52c0299c8fc95": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Min Y:",
                "layout": "IPY_MODEL_4f2567dd381441d9a0ee3725c1a63098",
                "step": null,
                "style": "IPY_MODEL_f5954dd98e924842ab3765f1e8f31e06",
                "value": -3675815.2556721447
            }
        },
        "bcfb9c2fc7d84060addac0b4beea8e73": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "bab4fde6b2044b0cabd79a8df88bf72c": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "33d05c1a35a84b4fb39f0f6969ba9354": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max X:",
                "layout": "IPY_MODEL_bcfb9c2fc7d84060addac0b4beea8e73",
                "step": null,
                "style": "IPY_MODEL_bab4fde6b2044b0cabd79a8df88bf72c",
                "value": -809611.9947511106
            }
        },
        "b60b0d3f164d4b72b26fdf7d57e57e3b": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "c7f40ec9a79f495392814c0e8ed3b4bc": {
            "model_name": "DescriptionStyleModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description_width": ""
            }
        },
        "581eb89a110f447c90e289795f9a5635": {
            "model_name": "FloatTextModel",
            "model_module": "@jupyter-widgets/controls",
            "model_module_version": "1.5.0",
            "state": {
                "description": "Max Y:",
                "layout": "IPY_MODEL_b60b0d3f164d4b72b26fdf7d57e57e3b",
                "step": null,
                "style": "IPY_MODEL_c7f40ec9a79f495392814c0e8ed3b4bc",
                "value": -2648276.573525252
            }
        },
        "e6b5de8b92ae4c24a979d0f9909da702": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "5592b50a15ae43668bd007b93baf89f3": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "941d3d134b7047cbb0437f9d1e05050c": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "2fbc7be23fc24cb9a03bc14a50862f43": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "3439094fc5c04210ad33bb07d9071d97": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "322616123b424b49850893b1a84cc6ff": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -25,
                    140
                ],
                "controls": [
                    "IPY_MODEL_61f2a9f62bd846ccabf70d4bcc0652f0",
                    "IPY_MODEL_9ff3e2f7135342208fc60b7cff37555a",
                    "IPY_MODEL_e84d840fb9c14eca9825558b47fc6ee4"
                ],
                "default_style": "IPY_MODEL_5592b50a15ae43668bd007b93baf89f3",
                "dragging_style": "IPY_MODEL_941d3d134b7047cbb0437f9d1e05050c",
                "east": 180.87890625000003,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_e6b5de8b92ae4c24a979d0f9909da702"
                ],
                "layout": "IPY_MODEL_2fbc7be23fc24cb9a03bc14a50862f43",
                "modisdate": "yesterday",
                "north": -8.233237111274553,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": -39.77476948529546,
                "style": "IPY_MODEL_3439094fc5c04210ad33bb07d9071d97",
                "west": 99.05273437500001,
                "zoom": 4
            }
        },
        "61f2a9f62bd846ccabf70d4bcc0652f0": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "9ff3e2f7135342208fc60b7cff37555a": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "e84d840fb9c14eca9825558b47fc6ee4": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "fb5371b9c85c4f8fbf298d7b59721faa": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "d559e111b1144ddfa3ec710331699416": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "75bdb6cbe5e74d9fb711062c67a6b90b": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "ab01872c3f5c4d378bb49b65efbfb456": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "48c1b5dbdb594da0b45fa2dcf38f879b": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "cdeeedf695e7454aac0d0aa71907a71c": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -25,
                    140
                ],
                "controls": [
                    "IPY_MODEL_988655bf47ce470b9048f3d43c3ad16d",
                    "IPY_MODEL_b0f7b275f23443f58065abeade55218a",
                    "IPY_MODEL_e2c2d31002d9427db92dbed062df32c2"
                ],
                "default_style": "IPY_MODEL_d559e111b1144ddfa3ec710331699416",
                "dragging_style": "IPY_MODEL_75bdb6cbe5e74d9fb711062c67a6b90b",
                "east": 180.87890625000003,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_fb5371b9c85c4f8fbf298d7b59721faa"
                ],
                "layout": "IPY_MODEL_ab01872c3f5c4d378bb49b65efbfb456",
                "modisdate": "yesterday",
                "north": -8.233237111274553,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": -39.77476948529546,
                "style": "IPY_MODEL_48c1b5dbdb594da0b45fa2dcf38f879b",
                "west": 99.05273437500001,
                "zoom": 4
            }
        },
        "988655bf47ce470b9048f3d43c3ad16d": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "b0f7b275f23443f58065abeade55218a": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "e2c2d31002d9427db92dbed062df32c2": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "e00c7bd29af544959af7eaa93b8cb988": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "bfefbf53ac5d4c609f6b3c1711820656": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "1f75f09bb3594a3a8b2606fc2cfe32bc": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "3a2600aa8a074c6dad1f2e06fcc039ab": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "2ca8a6c5e2c94ed1a8d694e7fd8de419": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "95d0e211e8a749cb8e8f2b62a1e4fe3a": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -25,
                    140
                ],
                "controls": [
                    "IPY_MODEL_57b0b1c00e344f8e83a2e445ceae7d0c",
                    "IPY_MODEL_f7e7bfad6b0b4979aba3227a3aa21c81",
                    "IPY_MODEL_e0ef98ca22c5472086dcf04328495c5b"
                ],
                "default_style": "IPY_MODEL_bfefbf53ac5d4c609f6b3c1711820656",
                "dragging_style": "IPY_MODEL_1f75f09bb3594a3a8b2606fc2cfe32bc",
                "east": 180.87890625000003,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_e00c7bd29af544959af7eaa93b8cb988"
                ],
                "layout": "IPY_MODEL_3a2600aa8a074c6dad1f2e06fcc039ab",
                "modisdate": "yesterday",
                "north": -8.233237111274553,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": -39.77476948529546,
                "style": "IPY_MODEL_2ca8a6c5e2c94ed1a8d694e7fd8de419",
                "west": 99.05273437500001,
                "zoom": 4
            }
        },
        "57b0b1c00e344f8e83a2e445ceae7d0c": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "f7e7bfad6b0b4979aba3227a3aa21c81": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "e0ef98ca22c5472086dcf04328495c5b": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        },
        "0b9563672cdf45c293acfdfbfb909b30": {
            "model_name": "LeafletTileLayerModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "attribution": "Map tiles by <a href=\"http://stamen.com/\">Stamen Design</a>, under <a href=\"http://creativecommons.org/licenses/by/3.0\">CC BY 3.0</a>. Data by <a href=\"http://openstreetmap.org/\">OpenStreetMap</a>, under <a href=\"http://creativecommons.org/licenses/by-sa/3.0\">CC BY SA</a>.",
                "max_native_zoom": 18,
                "min_native_zoom": 0,
                "min_zoom": 1,
                "name": "Stamen.Watercolor",
                "no_wrap": false,
                "options": [
                    "attribution",
                    "detect_retina",
                    "max_native_zoom",
                    "max_zoom",
                    "min_native_zoom",
                    "min_zoom",
                    "no_wrap",
                    "tile_size",
                    "tms"
                ],
                "url": "https://stamen-tiles-a.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png"
            }
        },
        "b949ee766f4d48cd9ee4df1ffcd4a657": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "19323b8ef6fe4523b824933bf8cf9a5f": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "cursor": "move"
            }
        },
        "99260fb87bd14731828ef5703f973d6b": {
            "model_name": "LayoutModel",
            "model_module": "@jupyter-widgets/base",
            "model_module_version": "1.2.0",
            "state": {}
        },
        "023a276341f94d9284deb4936d89cb75": {
            "model_name": "LeafletMapStyleModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1"
            }
        },
        "09721cf958e14fa2bf7d39583625dbcf": {
            "model_name": "LeafletMapModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "center": [
                    -25,
                    140
                ],
                "controls": [
                    "IPY_MODEL_62383fd9361b41f5b86acc14b730ca42",
                    "IPY_MODEL_bfed7373eb5b44a7a173cd0a90feebf2",
                    "IPY_MODEL_b00b01c87f3145728c836443e63ee9ed"
                ],
                "default_style": "IPY_MODEL_b949ee766f4d48cd9ee4df1ffcd4a657",
                "dragging_style": "IPY_MODEL_19323b8ef6fe4523b824933bf8cf9a5f",
                "east": 180.87890625000003,
                "fullscreen": false,
                "interpolation": "bilinear",
                "layers": [
                    "IPY_MODEL_0b9563672cdf45c293acfdfbfb909b30"
                ],
                "layout": "IPY_MODEL_99260fb87bd14731828ef5703f973d6b",
                "modisdate": "yesterday",
                "north": -8.233237111274553,
                "options": [
                    "bounce_at_zoom_limits",
                    "box_zoom",
                    "center",
                    "close_popup_on_click",
                    "double_click_zoom",
                    "dragging",
                    "fullscreen",
                    "inertia",
                    "inertia_deceleration",
                    "inertia_max_speed",
                    "interpolation",
                    "keyboard",
                    "keyboard_pan_offset",
                    "keyboard_zoom_offset",
                    "max_zoom",
                    "min_zoom",
                    "scroll_wheel_zoom",
                    "tap",
                    "tap_tolerance",
                    "touch_zoom",
                    "world_copy_jump",
                    "zoom",
                    "zoom_animation_threshold",
                    "zoom_start"
                ],
                "south": -39.77476948529546,
                "style": "IPY_MODEL_023a276341f94d9284deb4936d89cb75",
                "west": 99.05273437500001,
                "zoom": 4
            }
        },
        "62383fd9361b41f5b86acc14b730ca42": {
            "model_name": "LeafletZoomControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "zoom_in_text",
                    "zoom_in_title",
                    "zoom_out_text",
                    "zoom_out_title"
                ]
            }
        },
        "bfed7373eb5b44a7a173cd0a90feebf2": {
            "model_name": "LeafletAttributionControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position",
                    "prefix"
                ],
                "position": "bottomright",
                "prefix": "Leaflet"
            }
        },
        "b00b01c87f3145728c836443e63ee9ed": {
            "model_name": "LeafletDrawControlModel",
            "model_module": "jupyter-leaflet",
            "model_module_version": "^0.12.1",
            "state": {
                "_model_module_version": "^0.12.1",
                "_view_module_version": "^0.12.1",
                "options": [
                    "position"
                ],
                "polygon": {},
                "polyline": {},
                "rectangle": {
                    "shapeOptions": {
                        "color": "#00005d",
                        "fillOpacity": 0
                    }
                }
            }
        }
    }
}
</script>
</head>
</html>

