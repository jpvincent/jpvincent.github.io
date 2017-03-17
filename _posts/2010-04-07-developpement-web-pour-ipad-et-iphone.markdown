---
layout: post
title:  "Opéra mobile iPhone, pour les développeurs web"
date:   2010-04-07 16:04:21 +0100
---
Je regroupe ici une collection de liens traitant des différentes techniques et particuliarités lorsque l'on veut que son site Web marche également sur l'iPad et l'iPhone. Les 2 principales causes de problèmes sont l'absence de support de Flash et le fait d'être sur un touchscreen.

Ce post n'est pas fixe et d'autres liens viendront s'ajouter au fur et à mesure de mes <a href="http://jpv.typepad.com/blog/revue-de-presse/">revues de presse</a>. N'hésitez pas à <strong>m'en proposer de nouveaux dans les commentaires</strong>. Je privilégie les guides pratiques
<ul>
  <li>performances Web et réactivité d'une application Web pour iPad : de <a href="http://mir.aculo.us/2010/06/04/making-an-ipad-html5-app-making-it-really-fast/">bons conseils</a> comme éviter les images, les -webkit-gradient, opacité, text-shadow et box-shadow, et utiliser -webkit-transform avec parcimonie</li>
  <li><a href="http://www.ravelrumba.com/blog/ipad-http-debugging/">observer les requêtes (HTTP et autres)</a> faites par l'iPad / iPhone, via le wifi, un proxy et Charles. En bonus, on peut même savoir quelles sont les requêtes faites par les applications commerciales.</li>
  <li>l'iPad et le <a href="http://blog.typekit.com/2010/04/05/experimenting-with-web-fonts-on-the-ipad/">support des fonts</a>. Vous aurez probablement à supporter l'iPad dans les 6 mois à venir, aussi si vous comptiez sur son support CSS3/HTML5 pour faire quelque chose de moderne avec les fonts (au détriment des IEs...), il semble que celui ci soit (encore une fois) assez <strong>spécifique, voire bugué</strong>.</li>
  <li><a href="http://matthewjamestaylor.com/blog/ipad-layout-with-landscape-portrait-modes">librairie CSS</a> pour faire un layout qui s'adapte en fonction de l'orientation portrait/paysage</li>
  <li>les conseil d'Apple pour <a href="http://developer.apple.com/safari/library/technotes/tn2010/tn2262.html#">adapter son site à l'iPad</a>. Ne pas compter sur les plugins (flash donc ...), :<em>hover</em> et <em>mouseover</em> inutiles, <em>position:fixed</em> différent. Rien de bien neuf donc, mais ça fait toujours du bien à lire.</li>
  <li>appliquer des <a href="http://mislav.uniqpath.com/2010/04/targeted-css/">règles CSS</a> pour l'iPhone et l'iPad, en fonction de l'orientation et de la taille de l'écran. On y apprend également comment désactiver le menu contextuel.</li>
  <li><a href="http://davidwalsh.name/detect-ipad">détecter l'iPad</a> (PHP, JS)</li>
  <li>détecter les <a href="http://www.cloudfour.com/ipad-orientation-css/">dimensions et l'orientation</a> avec les media queries</li>
  <li>donner à certains éléments (comme des boutons ou des liens d'actions) toujours <a href="hack pour iOS safari pour donner à certains éléments toujours la même taille quel que soit le zoom : http://bit.ly/bgYx33">les mêmes dimensions</a>, quel que soit le niveau de zoom</li>
  <li>les <a href="http://www.yuiblog.com/blog/2010/06/28/mobile-browser-cache-limits/">limites du cache de Safari</a>. Sur iPad on est 25ko max (dézipé) par composant. Pour iPhone 4, 100ko max par composant, 2Mo max par page.</li>
  <li>librairie JS pour <a href="http://cubiq.org/iscroll">émuler l'</a><em><a href="http://cubiq.org/iscroll">overflow:scroll</a></em> pour les safari iOS : par défaut safari n'affiche pas de scrolbar, le contenu qui déborde n'est donc pas accessible.</li>
</ul>
