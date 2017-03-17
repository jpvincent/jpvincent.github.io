---
layout: post
title:  "Conférence performance à Paris Web 2010"
date:   2011-02-03 16:04:21 +0100
---
J'avais un <a href="http://www.paris-web.fr/2010/programme/performance-web.php">atelier sur les performances</a> pour le cycle de conférence Paris Web 2010, qui est maintenant sorti en vidéo. L'atelier s'est finalement transformé en une conférence improvisée d'1h30, par manque de candidats pour tester leurs sites, et de par la configuration de la pièce (amphi au lieu de salle de cours). Malheureusement le côté improvisé se ressent un peu car je ne m'étais pas chronométré pour réussir à synthétiser chaque sujet. 
Chaque sujet est donc discuté en profondeur, mais je n'ai pu traiter que les bases. Voici le chapitrage de cette conférence :
<!--more-->
<ul id="playlist">
  <li><a data-start="0">0 à 10:30</a> : relation chiffre d'affaire et temps d'affichage
  <li><a data-start="630">10:30</a> : référencement et webperf
  <li><a data-start="750">12:30</a> : quelles sont les mesures à prendre en compte ?</li>
  <ul>
    <li>comparaison affichage 01net / evene.fr
    <li>utilisation webpagetest.org
  </ul>
  <li><a data-start="1280">21:20</a> : contre-performance du protocole HTTP, parallélisation et latence
  <li><a data-start="1460">24:20</a> les JS/CSS (outil cuzillion), quels navigateurs tester ?
  <li><a data-start="2038">33:58</a> : outils pour JS/CSS
  <li><a data-start="2500">41:40</a> : organisation du code JS (bottom, loader)
  <li><a data-start="2890">48:10</a> : minifier le HTML ?
  <li><a data-start="2999">49:59</a> : checklists des recomandation webperf
  <li><a data-start="3030">50:30</a> : sprites ou encodage des images ?
  <li><a data-start="3330">55:30</a> : les images en PNG
  <li><a data-start="3420">57:00</a> : gzip
  <li><a data-start="3540">59:00</a> : interlude
  <li>Outils</li>
  <ul>
    <li><a data-start="3630">1:00:30</a> : firebug, ySlow, pageSpeed et cuzillion.org
    <li><a data-start="3780">1:03:50</a> : démo (plantée) de AOL pageTest et fallback sur webpagetest.org
    <li><a data-start="4050">1:07:30</a> : aparté sur les cookies
    <li><a data-start="4290">1:11:30</a> : Speed tracer
    <li><a data-start="4460">1:14:20</a> : dynatrace AJAX
    <li><a data-start="4500">1:15:00</a> : mot clé debugger
    <li><a data-start="4590">1:16:30</a> : IE8 debug tools (démo plantée)
  </ul>
  <li>Questions</li>
  <ul>
    <li><a data-start="4770">1:19:30</a> : question sur les commentaires conditionnels
    <li><a data-start="4950">1:22:30</a> : remarque sur l'importance du server-side ?
    <li><a data-start="5070">1:24:30</a> : remarque sur le PNG et l'encodage de JS
  </ul>
</ul>

<div id="container-player-atelier-perf">
    <a href="http://www.dailymotion.com/video/xguqi5">Vous trouverez la vidéo en ligne sur dailymotion</a>
</div>
<script>
var sPlayerId = 'player-atelier-perf',
  oPlayer,
  playList = document.getElementById('playlist').getElementsByTagName('A');
BC.loadJS('http://ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js', function() {
  var params = { allowScriptAccess: "always" },
    atts = { id: sPlayerId };
  swfobject.embedSWF('http://www.dailymotion.com/swf/xguqi5?enableApi=1&playerapiid='+sPlayerId,
                       'container-'+sPlayerId, "600", "400", "9", null, null, params, atts);
});
var onTimeClick = function() {
  oPlayer.seekTo( this.getAttribute('data-start') );
};
function onDailymotionPlayerReady() {
  oPlayer = document.getElementById(sPlayerId);
  for(var i=0; i < playList.length; i++) {
    playList[i].onclick = onTimeClick;
  }
};
</script>
<p>Je remercie en tout cas l'organisation sans faille de Paris Web, le caméramen qui a bien filmé les zones intéressantes de l'écran projeté et le monteur de la vidéo !
</p>

<!--
<iframe frameborder="0" width="560" height="448" src="http://www.dailymotion.com/embed/video/xguqi5?width=560&theme=eggplant&foreground=%23CFCFCF&highlight=%23834596&background=%23000000&start=&animatedTitle=&iframe=1&additionalInfos=0&autoPlay=0&hideInfos=0"></iframe><br /><b><a href="http://www.dailymotion.com/video/xguqi5_atelier-performance-web-jean-pierre-vincent_tech">[Atelier] Performance Web (Jean-Pierre Vincent)</a></b>
-->
