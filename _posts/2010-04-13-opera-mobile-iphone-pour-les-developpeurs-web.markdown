---
layout: post
title:  "Opéra mobile iPhone, pour les développeurs web"
date:   2010-04-13 16:04:21 +0100
---

J'essaye de regrouper sur cette page les limitations que j'ai pu trouver en testant <a href="http://itunes.apple.com/app/opera-mini-web-browser/id363729560?mt=8">Opéra mini sur l'iPhone</a> sur <a href="http://www.timeofmylife.com" target="_blank">le site</a> dont j'ai la charge (site communautaire très orienté JS). Je ne suis probablement pas le seul à le faire, d'autres comme <a href="http://www.quirksmode.org/mobile/">quirksmode</a> en ont même fait leur métier. Si vous faites des sites pour mobile, ça ne vous intéressera sans doute pas car vous connaissez probablement déjà ces limites. Si vous êtes un <strong>développeur de site/application web pour browser desktop</strong>, et que vous commencez à vouloir prévoir <strong>le même site sur browser touchscreen</strong>, ceci vous intéressera.

Je pars du principe arbitraire que vous êtes dans le cas général où jusqu'ici vous ne vous intéressiez qu'à l'iPhone (70% de PDM en France), que vous vous demandez sérieusement si votre site pourra passer sur l'iPad et que vous vous êtes rendu compte que Opéra mini réprésente <strong>50 millions d'utilisateurs</strong> dans le monde. Et que vous possédez bien sur un iPhone qui du coup pourra vous servir de plateforme de test à pas cher :)

Voici donc les limites par rapport à Safari :
<ul>
  <li>Pas d'event <em>ontouchstart</em>, <em>ontouchmove</em> ou <em>ontouchend</em>, introduits par safari iPhone. Ce n'est certes pas un standard (voir cet excellent <a href="http://www.quirksmode.org/mobile/tableTouch.html">tableau de compatibilité de quirksmode</a>), mais concrètement les librairies JS qui proposent le <strong>drag &amp; drop</strong> (déplacer des objets, faire un slider) aujourd'hui le font sur la base des events <em>onmousedown</em>, <em>onmouseover</em> et <em>onmouseup</em>, qui correspondent quasi littéralement aux events <em>ontouch</em>*. <strong>Sous Opéra il n'y a pas l'un et l'autre est biaisé</strong>, donc pas de drag &amp; drop ou de slider. Dans mon cas, la galerie ne marche pas en slidant avec le doigt, il faut cliquer sur un bouton pour aller à la photo suivante.</li>
  <li>pas d'implémentation de <a href="https://developer.mozilla.org/En/Offline_resources_in_Firefox" target="_blank">manifest.cache</a> (valable sous safari iPhone et Firefox 3.5+), qui permet de déclarer ce dont l'application a besoin en mode offline et qui peut s'utiliser pour avoir un caching hyper agressif, qu'il faut certes apprendre à gérer (on peut se retrouver avec des JS en version 1.2 alors que le site est passé à la version 1.3 par exemple), mais qui permet une rapidité sans pareil, quasi vitale en situation de mobilité</li>
  <li>aucun support des <a href="http://www.whatwg.org/specs/web-apps/current-work/#the-input-element">nouveaux types d'input</a> (url, email, tel ...), là où Safari change le clavier par défaut, ce qui est bien pratique. Bizarrement Opéra Desktop est en avance depuis longtemps sur les autres browsers sur ce point là, mais ils ont du se dire que les développeurs ne l'utilisaient pas assez</li>
</ul>
Les même limites qu'avec Safari (par rapport à un desktop) :
<ul>
  <li>Pas de flash ou de java, rien d'étonnant mais on peut toujours rêver</li>
  <li><strong>l'upload de fichiers n'est pas possible</strong>, comme sur safari, sauf que je n'ai <strong>pas trouvé de moyen de le détecter</strong>, je ne peux donc pas afficher un message alternatif. Pour info, pour le détecter sous Safari je procédais <a href="http://jsfiddle.net/YpP5a/">à ce test</a>. Si vous pensez à une autre manière de tester, je prends ...</li>
  <li>les <em>overflow:scroll</em> ne sont pas gérés (pas de scrollbar ni même de scroll possible). Il faut donc que rien d'essentiel n'en dépende</li>
  <li>il n'y a pas d'équivalent au media query de Mozilla -<em><a href="https://developer.mozilla.org/En/CSS/Media_queries#-moz-touch-enabled">moz-touch-enabled</a><span style="font-style: normal;">, ce qui aurait été bien pratique pour gérer une alternative au point précédent</span></em></li>
  <li><em><span style="font-style: normal;">il ne répond pas au media selector @<strong>media handheld</strong>. Dommage, on peut dire que ce standard est mort</span></em></li>
</ul>
Les bons points similaires :
<ul>
  <li>Il répond au media queries utilisées aujourd'hui pour cibler iPhone Safari : <strong>@media only screen and (max-device-width: 480px) { ../.. }</strong>. Pratique mais ces media selector ne sont <strong>pas viables à long terme</strong> s'ils ne sont utilisés que pour ciber des téléphones plutôt que des capacités !</li>
  <li>canvas est supporté (et marche)</li>
</ul>
Quelques avantages (majeurs) :
<ul></ul>
<ul>
  <li>Opéra a une innovation majeure qui consiste à posséder des serveurs relais qui vont compresser les pages demandées et les streamer (pas d'aller/retour HTTP classiques) vers le browser. C'est complètement adapté aux réseaux 3G et même Edge de mauvaise qualité, et on peut observer des temps de chargement 3 fois plus rapide sur un réseau de qualité moyenne.</li>
  <li>Je n'ai pas de chiffres, mais Opéra <strong>gère son cache de manière "normale"</strong>, par rapport à Safari iPhone qui a un cache avec des <a style="color: blue !important; text-decoration: underline !important; cursor: text !important;" href="http://yuiblog.com/blog/2008/02/06/iphone-cacheability/" target="_blank">limitations</a> qui le rendent pratiquement inutile. En conséquence, si vous avez déjà une politique de caching des assets efficace sur browser desktop, elle s'appliquera très bien sur Opéra (ce qui n'est pas le cas de Safari)</li>
</ul>
Espérons que Fennec saura faire mieux :)
