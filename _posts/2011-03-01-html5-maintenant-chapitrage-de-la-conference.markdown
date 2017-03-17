---
layout: post
title:  "HTML5 maintenant, chapitrage de la conférence"
date:   2011-03-01 16:04:21 +0100
---
La vidéo sur la conférence que j‘ai donné à Paris Web 2010 vient de sortir, aussi comme pour la conférence sur <a href="http://braincracking.org/?p=790">les Performances Web</a> je l‘ai chapitré et je vous remets les slides pour que vous vous y retrouviez.

Mes capacités d‘orateur valent ce qu‘elles valent, mais le contenu et le discours restent d‘actualité et devraient permettre je l‘espère à certains développeurs de se décoincer face à HTML5 : les fonctionnalités sont utilisables maintenant en production, pour peu qu‘on sache coder, utiliser des librairies faites pour ça et qu‘on soit suffisamment curieux pour tester tout cela. Les bénéfices sont soit immédiats soit à venir, et il y a certains écueils à connaître.
Le maître mot est : testez ! Dans la plupart des cas vous vous rendrez compte que vous pouvez mettre en production et donc améliorer votre site pour vos utilisateurs.
<!--more-->
<h2>Le chapitrage</h2>
La conférence :
<ul id="playlist">
  <li><a data-start="5">0:05</a> : bonjour</li>
  <li><a data-start="70">1:10</a> : relativisier l'état des specs</li>
  <li>l‘exemple de Web Storage 
  <ul>
    <li><a data-start="180">3:00</a> : 7 implémentations, mais utilisable</li>
    <li><a data-start="270">4:30</a> : librairies d‘accès à Web Storage</li>
    <li><a data-start="340">5:40</a> : Exemples d'utilisation de Web Storage</li>
  </ul>
  </li>
  <li><a data-start="385">6:25</a> : HTML5, c'est gros, et alors ?</li>
  <li>La nouvelle sémantique
  <ul>
    <li><a data-start="430">8:30</a> : le doctype</li>
    <li><a data-start="830">9:20</a> : nouvelles balises (<code>nav, article, header</code>, légendes ...</li>
    <li><a data-start="810">13:30</a> : microdata</li>
    <li><a data-start="975">16:15</a> : nouveaux types de champs de formulaire</li>
  </ul>
  </li>
  <li><a data-start="1135">18:55</a> : les APIs JS
  <ul>
    <li><a data-start="1165">19:25</a> : est on vraiment dans la nouveauté ? Listing des fonctionnalités qui ont déjà un équivalent</li>
    <li><a data-start="1317">21:57</a> : comportement des formulaires, <a href="http://www.alistapart.com/d/forward-thinking-form-validation/enhanced_2.html">démo</a> de librairies sous IE6</li>
    <li><a data-start="1440">24:00</a> : intérêt d'utiliser quand même le standard HTML5 pour les formulaires</li>
  </ul>
  </li>
  <li><a data-start="1625">27:05</a> : Géolocalisation navigateur : <a href="http://jsfiddle.net/braincracking/EbeJQ/">démo</a> et intérêt par rapport à la géolocalisation par IP, alternative</li>
  <li><a data-start="1920">32:00</a> : SVG : Démo sur IE6 grâce à la <a href="http://raphaeljs.com/analytics.html">librairie raphaelJS</a></li>
  <li><a data-start="2060">34:20</a> : La vidéo HTML5 : limites et <a href="http://praegnanz.de/html5video/">librairies</a></li>
  <li><a data-start="2190">36:30</a> : Drag and Drop et sélection multiple : démo IE6 (applet, flash), retour d'implémentation en prod, transcodage en HTML5</li>
  <li><a data-start="2460">41:10</a> : conclusion : partez sur <a href="http://braincracking.org/?p=764">les standards</a>, utilisez <a href="https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-browser-Polyfills">les librairies</a>, jouez et testez</li>
</ul>
Vu mon débit de parole lorsque je suis sous le feu de la rampe j‘ai terminé 10 minutes avant l‘heure prévue. La scéance de question / réponses a donc pu être longue et très intéressante :
<ul>
  <li>44:10 : la remarque qui tue à propos de l‘accessibilité actuelle de HTML5 par Aurélien Levy (j'ai développé ses argument dans ce post sur <a href="http://braincracking.org/?p=860">l'accessibilité  de HTML5</a>). Pour résumer : je maintiens que à part 2 bugs, l‘accessibilité n‘est pas dégradée même pour IE6.</li>
  <li>47:30 : remarque sur la surenchère de librairie JS sur les formulaires chez certaines sociétés</li>
  <li>49:20 : on revient sur ma mauvaise opinion de l‘implémentation de la vidéo HTML5</li>
  <li>51:20 : ARIA sera t il toujours utile ? (oui) </li>
  <li>52:00 : on re-discute de l‘accessibilité de HTML5 entre pros</li>
  <li>53:45 : HTML5 boilerplate</li>
  <li>54:05 : question sur l‘interprétation du doctype HTML5 par IE6 (réponse mal dite : IE n‘a que 2 modes, standard ou quirskmode)</li>
  <li>55:25 : inquiétude sur les failles de sécurité des librairies</li>
</ul>

<h2>La vidéo</h2>
<div id="container-player-html5-now">
    <a href="http://www.dailymotion.com/video/xh9c18">Vous trouverez la vidéo en ligne sur dailymotion</a>
</div>
<script>
var sPlayerId = 'player-html5-now',
  sDailymotionID = 'xh9c18',
  oPlayer,
  playList = document.getElementById('playlist').getElementsByTagName('A');
BC.loadJS('http://ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js', function() {
  var params = { allowScriptAccess: "always" },
    atts = { id: sPlayerId };
  swfobject.embedSWF('http://www.dailymotion.com/swf/'+sDailymotionID+'?enableApi=1&playerapiid='+sPlayerId,
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


<h2>Les slides</h2>
Les slides vus à la conférence sont ceux ci :
<a title="HTML5 c'est maintenant" href="http://www.slideshare.net/jpvincent/html5-now-light">HML5 c‘est maintenant (IE6 inclus)</a>
<iframe src="//www.slideshare.net/slideshow/embed_code/key/kgLbwxqgFCulmE" width="510" height="420" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/jpvincent/html5-now-light" title="Html5 now light" target="_blank">Html5 now light</a> </strong> from <strong><a target="_blank" href="//www.slideshare.net/jpvincent">Jean-Pierre Vincent</a></strong> </div>


Vous trouvez 3 fois plus de détails concernant HTML5 dans <a href="http://braincracking.org/?p=597">une autre série de slides</a>, avec notamment le détail de Audio/Video, un exemple de microdata, le détail du problème des nouveaux éléments et du localStorage et une implémentation de géolocalisation. Bref, à regarder au calme chez soi pour se former.

<h2>Une suite ?</h2>
Ma prochaine conf sur HTML5 sera plus petite, mais complémentaire de celle ci : à la <a href="http://www.kiwiparty.fr/">Kiwi Party</a> organisée par Alsacréations, j‘essaierais de montrer qu‘intégrer HTML5 dans son processus de fabrication de site ne devrait rien changer tant qu'on respecte déjà les bonnes pratiques.
En parallèle mon projet actuel tourne uniquement autour du passage à HTML5, j‘aurais l‘occasion de vous en reparler dès qu‘il pourra être rendu public.
