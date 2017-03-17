---
layout: post
title:  "Concours Webperf 2010 : maîtriser le chargement"
date:   2011-01-10 16:04:21 +0100
---
Ce post est le second d'une série de 3 et est tiré de l'expérience des finalistes du concours de performance Web 2010
<nav>
<ol>
  <li><a href="http://braincracking.org/?p=695">Les bases</a></li>
  <li>Maîtriser le chargement.</li>
  <li><a href="http://braincracking.org/?p=696">Refactoring, enseignements</a>.</li>
</ol>
</nav>
Nous allons analyser les stratégies et techniques gagnantes (ou perdantes parfois) de chargement des dépendances de la page (CSS, JS, images, XHR). C'est là dessus que se sont concentrés les finalistes car il n'existe rien de suffisamment universel pour espérer gagner ce concours, nous avons donc assisté un joli combat de cerveaux.
Ce post sera utile pour les développeurs Web qui pourront être inspirés pour accélérer le rendu de leurs propres pages, et d'un intérêt tout particulier pour ceux qui connaissent déjà cette page.
<!--more-->

<h2>Charger ses <abbr>CSS/JS</abbr></h2>
Toute la difficulté du <abbr>CSS</abbr>, c'est que pendant qu'il est téléchargé, le rendu est bloqué. On pourrait alors tout mettre inline (comme <a href="http://fr.yahoo.com/?p=us">la home de Yahoo</a>!), mais c'est alors sacrifier les bénéfices du caching sur les fichiers statiques externes. Et le cache sur un cybermarchand est extrêmement utile puisqu'on peut s'attendre à ce qu'un utilisateur visite plusieurs pages d'affilé.

Tout est donc question de balance entre l'<span lang="en">inline et l'<span lang="en">external</span>, voici plusieurs stratégies :</span>
<h3>CSS différé</h3>
L'idée est de mettre le minimum de styles inline : un reset <abbr>CSS</abbr>, un peu du <span lang="en">header</span> et un préchargement d'images, et charger en JavaScript le <abbr>CSS</abbr> externe. Malgré un <span lang="en">start render</span> au plus bas (200ms!), ceci déclenche malheureusement un <abbr title="Flash Of Unstyled Content">FOUC</abbr> désagréable le temps que le CSS final arrive.

[caption id="attachment_653" align="alignleft" width="150" caption="Contenu non stylé pendant 1s"]<a href="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/12/delay-css-screenshot.jpg"><img class="size-thumbnail wp-image-653 " title="delay-css-screenshot" src="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/12/delay-css-screenshot-150x150.jpg" alt="Screenshot Flash of Unstyled Content" width="150" height="150" /></a>[/caption]

[caption id="attachment_654" align="alignright" width="150" caption="Après 1s le contenu s&#39;affiche"]<a href="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/12/delay-css-screenshot2.jpg"><img class="size-thumbnail wp-image-654" title="delay-css-screenshot2" src="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/12/delay-css-screenshot2-150x150.jpg" alt="" width="150" height="150" /></a>[/caption]

Ces 2 <span lang="en">screenshots</span> sont extraits de <a href="http://www.webpagetest.org/video/compare.php?tests=101202_a392076e4536d5c6b687b12550aa0a60-r:6-c:0">cette série</a>. Vous pouvez tester cette technique sur vos navigateurs avec <a href="http://stevesouders.com/cuzillion/?c0=hs0hfff0_0_f&amp;c1=bi1hfff2_0_f&amp;c2=bi1hfff2_0_f&amp;c3=bi1hfff2_0_f&amp;c4=bc1dtff5_0_f&amp;t=1291847961219">Cuzillion</a>. Mais par principe un site ne devrait pas dépendre de JavaScript au point de ne pas être stylé si le <abbr>JS</abbr> n'est pas exécuté

Cette technique un peu extrême trouverait cependant un intérêt si le <abbr>CSS</abbr> était parfaitement maîtrisé, auquel cas on pourrait mettre le <abbr>CSS</abbr> utile et visible inline lors de la première visite (bénéfice du <span lang="en">start render</span> rapide), charger le <abbr>CSS</abbr> via <abbr>JS</abbr>, puis marquer l'utilisateur d'un <span lang="en">cookie<span> signalant qu'il a bien chargé la feuille de style pour ne lui servir que l'<abbr>URL</abbr> du fichier (bénéfice du cache).</span></span>
<h3>CSS inline</h3>
Une autre stratégie pour ne pas avoir à télécharger un <abbr>CSS</abbr> qui bloque le rendu est de mettre son contenu <span lang="en">inline</span>. Pour cette page en particulier, certains ont réussi à réduire le poids du <abbr>CSS</abbr> à 4Ko gzipé, alors que le <abbr>HTML</abbr> fait 8Ko gzipé. La taille totale est donc largement acceptable et cela épargne un blocage de 100ms, mais c'est le genre de technique qu'on n'utilise que sur des pages très surveillées et généralement uniques (les <span lang="en">homes</span> de Yahoo! et de google par exemple).
<h3>Mix Inline / Inclusion</h3>
Une technique que je voulais voir en action et qui va au contraire des règles établies : mettre le minium <span lang="en">inline</span> et charger plus loin dans la source le reste des <abbr>CSS</abbr>. Ceci permet en théorie de différer le téléchargement et le <span lang="en">parsing</span> du <abbr>CSS</abbr> et de lancer au plus vite le téléchargement des images qui se trouveraient avant l'inclusion. Le tout sans dépendre de JavaScript.

Mais si <abbr title="Internet Explorer">IE</abbr> commence effectivement le téléchargement des images, il attend tout de même le <abbr>CSS</abbr> avant de commencer le rendu (voir l'effet simplifié sur <a href="http://stevesouders.com/cuzillion/?c0=hs0hfff0_0_f&amp;c1=bi1hfff2_0_f&amp;c2=bi1hfff2_0_f&amp;c3=bi1hfff2_0_f&amp;c4=bc1hfff5_0_f&amp;t=1291920593">Cuzillion</a> ou l'effet sur <a href="http://www.webpagetest.org/video/compare.php?tests=101202_5759f68c99ab05f7b15a8a5358fff14a-r:3-c:0">la page finale</a>, créée par <a href="http://twitter.com/pixelastic">Timothée Carry-Caignon</a>). C'est à confirmer mais le seul cas où cela pourrait marcher serait sur une page longue à calculer <span lang="en">server-side</span>, qui enverrait des bouts de <abbr>HTML</abbr> qui ne contiennent pas encore d'appel <abbr>CSS</abbr>. Mais même là je pense que le <span lang="en">progressive rendering</span> ne serait pas satisfaisant car il y aurait un effet de <span lang="en">freeze</span> au moment où le navigateur découvrirait qu'il y a un <abbr>CSS</abbr> à télécharger, laissant l'utilisateur bloqué sur une page pas encore stylée.

Un autre essai a été <a href="http://entries.webperf-contest.com/4cd948b969dc9/index.html">tenté ici</a> par Thomas ROULET : inclusion <span lang="en">inline</span> d'un <abbr>CSS</abbr> s'occupant du <span lang="en">layout</span> pour que le premier rendu soit satisfaisant. Sans JavaScript, il utilise la balise <code>noscript</code> pour télécharger le <abbr>CSS</abbr>. Voici le rendu :

[caption id="attachment_672" align="alignleft" width="270" caption="Rendu avec le CSS inline"]<a href="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/12/css-inline-rendu1.jpg"><img class="size-medium wp-image-672 " title="css-inline-rendu1" src="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/12/css-inline-rendu1-300x211.jpg" alt="" width="270" height="190" /></a>[/caption]

[caption id="attachment_673" align="alignright" width="270" caption="Rendu avec le CSS final"]<a href="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/12/css-inline-rendu2.jpg"><img class="size-medium wp-image-673 " title="css-inline-rendu2" src="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/12/css-inline-rendu2-300x211.jpg" alt="" width="270" height="190" /></a>[/caption]

Cette technique cumule les avantages, mais pour d'autres raison <a href="http://www.webpagetest.org/video/compare.php?tests=101202_bdfd9de7b571f2309eaf0203d1baea39-r:4-c:0">le temps</a> pendant lequel la page restait avec le <abbr>CSS</abbr> minimal était énorme

<strong>Conclusion</strong> : ne changez rien, il faut faire télécharger le <abbr>CSS</abbr> au plus tôt. Si votre <abbr>CSS</abbr> est vraiment énorme, outre un <span lang="en">refactoring</span>, envisagez le découpage en 1 feuille légère et spécifique à une page que vous placerez <span lang="en">inline</span> et à pré-charger les <abbr>CSS</abbr> des pages suivantes.
<h3>Javascript différé</h3>
Certains ont utilisé des <span lang="en">lazy-loaders</span>, d'autres ont utilisé l'attribut <code>defer</code> (inventé par <abbr>IE</abbr>, standardisé dans <abbr>HTML5</abbr>, supporté par tous) voir <code>async</code>. Voici un petit graphe qui résume les effets de ces attributs, tiré de <a href="http://peter.sh/experiments/asynchronous-and-deferred-javascript-execution-explained/">cet article</a> :

[caption id="attachment_666" align="aligncenter" width="620" caption="Comportement du navigateur avec async et defer"]<a href="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/12/asyn-defer-js.jpg"><img class="size-full wp-image-666 " title="asyn-defer-js" src="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/12/asyn-defer-js.jpg" alt="" width="620" height="101" /></a>[/caption]

Dans les 2 cas cela demandait des modifications de code qui n'ont pas été très lourdes et qui apportent un réel plus à la fluidité du chargement de la page.
<h3>Lazy-loading de JS</h3>
Outre le chargement différé, il était aussi possible de ne charger le <abbr>JS</abbr> qui régit l'<span lang="en">autocomplete</span> du champs de recherche qu'au <code>focus</code> sur ce champs. C'est la technique dite du <span lang="en">lazy-loading</span> qui permet de ne charger les dépendances d'une fonctionnalité que lorsqu'on va en avoir besoin. Voir par exemple la <a href="http://fr.yahoo.com/">home de Yahoo</a>! qui est utilise à fond ce principe.
<h3>Paralléliser</h3>
En performances on commence toujours par là où ça va le moins vite, et là on arrive forcément à <abbr>IE6/7</abbr>. Ce navigateur n'autorise que 2 téléchargement de <abbr>JS/CSS</abbr> en parallèle. La technique généralement pratiquée est de toute façon de n'avoir qu'un seul fichier de chaque type à télécharger, mais Cédric Morin a testé le regroupement puis le <span lang="en">split</span> des <abbr>JS/CSS</abbr> en 2 fichiers pour maximiser l'utilisation de la bande passante. Résultat sur le JS :

[caption id="attachment_670" align="aligncenter" width="396" caption="Téléchargement parallèle des 2 seuls JS de la page"]<a href="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/12/js-parallel.jpg"><img class="size-full wp-image-670" title="js-parallel" src="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/12/js-parallel.jpg" alt="" width="396" height="32" /></a>[/caption]

Ce sont les barres bleues (le téléchargement) qui nous montrent le gain. On doit être ici à 70ms, ce qui n'est pas énorme mais sur un site avec un <abbr>JS</abbr> plus lourd cela peut commencer à être intéressant. Et dans le cadre d'un concours de peformance, l'intérêt est évident :) Par contre, il faut bien faire attention à l'ordre d'exécution des scripts, mais cela peut se ranger dans des fonctions simples ou vous pouvez utiliser des librairies.

Concernant le <abbr>CSS</abbr> par contre, la parallélisation n'amène rien en terme d'accélération du rendu car les temps de téléchargement s'additionnent (à tester sur <abbr>IE7</abbr> avec <a href="http://stevesouders.com/cuzillion/?c0=hc1hfff2_0_f&amp;c1=hc1hfff2_0_f&amp;t=1291988336258">Cuzillion</a>), mieux vaut donc rester sur un fichier unique.
<h2>Techniques de lazy-loading</h2>
Une des grandes techniques de la performance Web est de maîtriser parfaitement le chargement des dépendances, en les chargeant si possible au tout dernier moment, juste avant que l'utilisateur n'en ait besoin. C'est en pratique difficilement applicable les yeux fermés en production car il n'y a rien de générique, hormis <a title="jQuery lazy load" href="http://www.appelsiini.net/projects/lazyload" target="_blank">quelques</a> <a title="YUI image loader" href="http://developer.yahoo.com/yui/imageloader/" target="_blank">librairies</a> qui chargent les images non visibles au moment où l'utilisateur scroll. Comme souvent dans les perfs, on est ici dans du spécifique pour chaque page et c'est au développeur de bien la connaître et de savoir ce qui est important à télécharger durant la première seconde et ce qui peut être remis à plus tard.

Cette technique semble idéale si l'on ne regarde que les outils de mesure automatisés puisqu'il y a beaucoup moins d'objets à télécharger (les outils ne scrollent pas) : le poids initial est réduit et le temps <code>onload</code> raccourci. Ca a donc remonté des concurrents en haut du tableau. Mais dans le cas de la FNAC, il était par exemple non toléré de :
<ul>
  <li>différer le chargement des images produit : dans un magasin, le visuel et le prix sont 2 informations de première importance. Pas de photo ou de prix, pas d'achat. Même si la population sans javascript est de quelques %, il faut au moins que ces gens puissent voir le produit. Sur Internet, les cybermarchands espèrent en plus que des moteurs comme Google Image indexeront leurs images. Notez que pour un autre business qui accepterait de sacrifier ses utilisateurs sans JS et l'indexation (un réseau social par exemple), cette technique très efficace aurait été acceptée</li>
  <li>déclencher des téléchargements au <code>onmousemove</code> : l'accessibilité au clavier en prend un coup</li>
</ul>
Les concurrents de la catégorie meilleur poids ont choisi diverses techniques de chargement différé :
<ul>
  <li>placer dans une balise <code>&lt;noscript&gt;</code> le contenu de la boutique référençant les images. C'est bien vu car les gens sans <abbr title="JavaScript">JS</abbr> et le référencement ne sont pas gênés. Le contenu est ensuite récupéré en <abbr>JS</abbr> et les images téléchargées en fonction de la zone visible. Cela a été fait avec plus ou moins de bonheur par les finalistes, mais regardez <a href="http://entries.webperf-contest.com/4cc80752a91cc/index.st.html">cette page</a> du mythique <a href="http://twitter.com/guslelapin">Cédric Morin</a> avec JavaScript désactivé pour avoir la meilleure version</li>
  <li>la fnac ayant déjà fait le choix original de mettre son <span lang="en">footer</span> en <code>iframe</code>, les concurrents ont simplement retardé ou conditionné son affichage en <abbr>JS</abbr>, certains utilisant <code>&lt;noscript&gt;</code> pour la version sans <abbr>JS</abbr>.</li>
</ul>
<h2>Encodage des images</h2>
La réduction du nombre de requêtes <abbr>HTTP</abbr> étant le nerf de la guerre, cela peut aller jusqu'aux images. La technique répandue aujourd'hui est l'utilisation de <span lang="en">sprites</span>. Il est fortement probable qu'elle soit remplacée demain par la technique d'encodage des images dans le <abbr>CSS</abbr>.
<h3><abbr>MHTML</abbr> et <code>data:uri</code></h3>
Internet Explorer 6 et 7 supportent l'encodage des images en <abbr>MHTML</abbr> tandis que les autres navigateurs supportent l'encodage en <code>base64</code>. Voici <a href="http://s2.webperf-contest.com/4cd42145adb3a/c/ie.css">un exemple de <abbr>CSS</abbr></a> fait par <a href="http://www.christophelaffont.com/">Christophe Laffont</a> ne contenant que des images encodées et <a href="http://s2.webperf-contest.com/4cd42145adb3a/c/noie.css">son équivalent</a> pour les autres navigateurs. Cette technique est largement industrialisable du moment que l'on maîtrise la fabrication de son <abbr>CSS</abbr> final avant mise en production. Plusieurs outils ont été utilisés : il existe même <a href="https://github.com/Schepp/CSS-JS-Booster">une librairie PHP</a> qui s'occupe de concaténer les feuilles de style et de séparer les règles sans images des feuilles contenant les images encodées. Au niveau des outils :
<ul>
  <li>l'encodage en <code>base64</code> peut se faire simplement <a href="http://en.wikipedia.org/wiki/Data_URI_scheme">en <abbr>PHP</abbr></a></li>
  <li><a href="https://github.com/nzakas/cssembed/wiki">CSSEmbed</a> en java</li>
  <li><a href="https://github.com/Schepp/CSS-JS-Booster">CSS-JS-Booster</a> en PHP</li>
  <li>il existe même un <a href="http://duris.ru/">service en ligne</a> (russe)</li>
</ul>
Nous attendions beaucoup du <abbr>MTHML</abbr>, et ce déploiement en situation réelle a révélé une énorme lacune. Ce graphique d'une vue de la page en cache est révélateur :

[caption id="" align="aligncenter" width="558" caption="Le coût CPU du MHTML"]<a href="http://www.webpagetest.org/results/10/12/02/5759f68c99ab05f7b15a8a5358fff14a/6_Cached_waterfall.png"><img src="http://www.webpagetest.org/results/10/12/02/5759f68c99ab05f7b15a8a5358fff14a/6_Cached_waterfall.png" alt="Page load waterfall diagram" width="558" height="112" /></a>[/caption]

Le <abbr>HTML</abbr> et toutes les dépendances viennent du cache, aucun téléchargement sauf le <span lang="en">tracking</span> n'est nécessaire ... mais la barre des téléchargements montre un énorme vide, expliqué par le graphe <abbr title="processeur">CPU</abbr> : il faut <a href="http://www.webpagetest.org/video/compare.php?tests=101202_5759f68c99ab05f7b15a8a5358fff14a-r:6-c:1">une pleine seconde</a> avant de rendre utilement la page !
<h2>Les extrêmes</h2>
Certains concurrents ont tenté des techniques extrêmes qui ont été révélatrices. Parfois disqualifiantes mais très instructives
<h3>1 seule requête <abbr>HTTP</abbr></h3>
Une seule requête <abbr>HTTP</abbr> au lieu des dizaines à l'origine, c'était théoriquement possible, et <a href="http://twitter.com/wixiweb">Arnaud Lemercier</a> l'a osé. La page s'affiche bien et est fonctionnelle sous <span lang="en">Firefox</span>, mais ne marche pas sous <abbr>IE7</abbr>, ce qui était disqualifiant, mais ça nous apprend plusieurs choses :

[caption id="attachment_618" align="aligncenter" width="610" caption="1 query to rule them all"]<a href="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/11/1query-waterfall.png"><img class="size-full wp-image-618    " title="1query-waterfall" src="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/11/1query-waterfall.png" alt="" width="610" height="112" /></a>[/caption]
<ul>
  <li>Le poids total de la page ainsi arrangée était de 500Ko. Et pourtant regardez l'utilisation de la bande passante (graphique vert) : le <strong>navigateur fait des pauses pendant le téléchargement</strong>, mettant finalement 7s au lieu de moins d'une seconde théorique</li>
  <li>le <strong>processeur est occupé à 100%</strong> pendant 6 secondes : sans doute un mix de décompression Gzip sur un fichier qui peine à arriver, de la lecture d'un <abbr>DOM</abbr> extrêmement lourd et d'interprétation des images en base 64</li>
  <li>les premiers pixels ne sont affichés qu'au bout de 4,4 secondes, et on observe le même phénomène, moins gravement, sur Firefox</li>
</ul>
Je cite les autres désavantages de la méthode <span lang="en">inline</span> :
<ul>
  <li>pas de mise en cache : les visiteurs allant sur une seconde page devront re-télécharger la plupart des éléments, ce qui est dommage pour un site comme la FNAC, mais qui n'est pas grave pour certains sites "de destination" comme <a href="http://fr.yahoo.com/?p=us">la homepage de Yahoo</a>!</li>
  <li>la technique des <code>data:uri</code> est incompatible avec <abbr>IE6-7</abbr>, il faut donc rajouter une technique équivalente appellée <abbr>MHTML</abbr>. Et donc doubler le poids des images encodées ou faire de la détection de navigateur</li>
</ul>
En résumé : mettre inline <abbr>CSS</abbr>, <abbr>JS</abbr> et surtout images encodées en <code>base64</code> est extrêmement consommateur de ressources, quel que soit le navigateur au point <strong>d'empirer la situation du <span lang="en">start render</span></strong>. Il serait intéressant de savoir si c'est la construction d'un <abbr>DOM</abbr> fait de 500k de texte ou si c'est le décodage des images qui plombe à ce point les performances de rendu. Si quelqu'un a du temps pour nous tester cela, le code source de cette page extrême est ici : <a href="http://entries.webperf-contest.com/4cd9d06f380f8/">http://entries.webperf-contest.com/4cd9d06f380f8/</a>
