---
layout: post
title:  "Retour sur le Concours Webperf 2010"
date:   2011-01-10 16:04:21 +0100
---
Durant tout novembre 2010 a eu lieu le premier <a href="http://webperf-contest.com/index-fr.html">concours international de performance Web</a>. Je vous renvoie vers <a href="http://braincracking.org/?p=607">ce post d'introduction</a> si vous n'en aviez pas entendu parler. Le principe était relativement simple : afficher une page le plus rapidement possible. Pour cela la FNAC nous a gentiment prêté son nom, et une page cible.

<!--more-->

Nous avons reproduit cette page via des fichiers statiques et les concurrents ont pu modifier les sources sur un serveur commun. Résultats ? la page de base mettait <strong>4-5 secondes avant de commencer à afficher quelque chose d'utile</strong> et <strong>plus de 15s pour récupérer complètement la page</strong>. Et on parle bien de <strong><a href="http://www.webpagetest.org/result/101203_0c152afe6dab2cb3efbd674e32bcb6da/4/details/">la version statique</a></strong> : pas de script côté serveur, pas de requête SQL, bref aucun ralentissement de côté là.

<figure>
  <video controls width="610"  autobuffer preload=”auto”>
  <source src="http://vid.ly/1l4c0t?content=video" />
  <a href="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2011/01/filmstrip.png"><img class="size-large wp-image-704" title="filmstrip" src="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2011/01/filmstrip-1024x82.png" alt="" width="614" height="49" /></a>
</video>
<figcaption>Comparaison du rendu progressif entre la page statique originale et les pages des 3 gagnants</figcaption>
</figure>

<h2>Mais comment font ils ?</h2>
Afin de contribuer à répandre la bonne parole sur les performances Web, et pour nous aider à les juger, chaque candidat avait la responsabilité de noter les modifications qu'il avait entrepris. Il y en a de toute beauté, mais il faut fouiller pour les trouver : il y en a 250 que <a href="http://webperf-contest.com/leaderboard-fr.html">vous retrouverez ici</a> dans la colonne Gist. Vous pouvez classer les concurrents par temps de rendu, nombre de requête, poids total et score YOTTAA pour voir ceux des finalistes

Il y a aussi un petit nombre de posts de blogs des participants que je vous conseille de lire :
<ul>
  <li><a href="http://www.yterium.net/Ma-participation-au-Webperf">l'énorme post</a> du suractif Cédric Morin, à relire plusieurs fois tant certaines techniques sont géniales. On y apprend aussi que le projet SPIP va bénéficier de cette expérience, qu'il en a profité pour lancer JQI, un lazy-loader de jQuery et qu'il contribue à CSSTidy</li>
  <li>un <a href="http://entries.webperf-contest.com/4cd41a2537c1a/gist.html">retour tout aussi détaillé</a> de <a href="http://twitter.com/pixelastic">Timothée Carry-Caignon</a> sur son travail, avec notamment un bug de &lt;noscript&gt; et pas mal de travail sur JS</li>
  <li>un <a lang="en" href="http://tips.freedev.com.ar/en/wpo/faster-sprite-download/">court article</a> d'un développeur espagnol spécialisé dans la performance Web, qui dit tout le bien qu'il y a à précharger ses sprites en JS dans le head avant d'y faire référence dans le CSS . Pas sur que l'idée soit géniale.</li>
</ul>
<strong>Ex-concurrent ? : contribuez vous aussi</strong> : si vous avez le sentiment d'avoir essayé une technique qui vaut qu'on parle d'elle car elle est <strong>peu connue mais efficace</strong> ou au contraire <strong>connue mais décevante</strong>, bloguez dessus ou <a href="http://braincracking.org/a-propos/">écrivez moi</a> ! Ca fera peut être l'objet d'un post complémentaire
<h2>Je n'ai pas la journée ...</h2>
Moi non plus, donc j'ai pris plusieurs soirées : faisant partie du jury, j'ai du passer du temps à analyser le travail de la quinzaine de finalistes (ceux qui avaient le meilleur score calculé automatiquement). En passant j'avais préparé ces articles, que je n'ai mis en forme que récemment, et divisé en une série de 3 posts qui résument globalement les techniques utilisées :
<nav><ol>
  <li><a href="http://braincracking.org/?p=695">Les bases</a>. Configurer Apache, réduire le poids</li>
  <li><a href="http://braincracking.org/?p=617">Maîtriser le chargement</a>. L'essentiel de l'effort pendant le concours : charger images, JS et CSS, lazy-loading, defer, inline, parallélisation, MHTML et data:uri</li>
  <li><a href="http://braincracking.org/?p=696">Refactoring, enseignements</a>. Les bonnes pratiques de Développement Web ont eu le dernier mot sur les perfs ! Egalement le résumé des <strong>enseignements techniques</strong> du concours</li>
</ol></nav>
Ce n'est qu'une vue partielle et ça ne sera jamais que mon opinion mais devant la somme de travail et le nombre d'avis divergents potentiels, je ne peux que vous demander de réagir en commentaire, par mail ou par blog, afin que la communauté en bénéficie
<h2>Bilan sur le concours lui même</h2>
0 retour presse, preuve que la performance Web est loin de l'oeil de la presse informatique généraliste. Mais nous avons tout de même eu 250 inscrits dont 55 ont réellement participé. Aux deux tiers français de par l'influence d'Eric Daspet et <a href="http://www.alsacreations.com/actu/lire/1147-concours-international-de-performance-web.html">Alsacreations</a>, ce chiffre est tout de même largement supérieur à nos attentes (honnêtement j'aurai déjà été heureux avec une vingtaine de français...). J'interprète ce bon résultat comme le signe que les développeurs Web commencent à envisager avec autant de sérieux les techniques de performance Web que l'accessibilité ou le bon usage des CSS.
<ul>
  <li>la charge de travail était énorme : ceux qui ont été finalistes ont consacré plus de 30h sur cette page ! Certains ont abandonné après avoir vu le nombre de fichiers et la taille du <abbr>HTML</abbr>, et d'autres en <a href="http://razorfast.com/2010/12/02/why-i-didnt-try-to-win-webperf-contest-2010/">en cours de route</a>. Pour être honnête nous ne nous attendions pas à ce volume de travail. Rassurez vous, nous avons passé autant de temps à l'organiser puis à le juger.</li>
  <li>malgré cela, je maintiens que cette page était parfaite car complètement représentative des pages du Web d'aujourd'hui : un long historique, des fonctionnalités qui s'accumulent, des compétences variées qui travaillent le même code avec différents niveaux de qualité ... Les poids, le nombre de requêtes et les temps de chargement sont dans les moyennes mondiales</li>
  <li>beaucoup de concurrents, c'est beaucoup d'essais différents, et donc beaucoup de validation / invalidation de techniques. Je pense sérieusement que c'est bon pour les connaissances globales sur les performances Web</li>
  <li>même les concurrents qui ont abandonné très tôt ont dit avoir appris des choses, ce qui était le but premier. Ne serait ce que de mettre les mains dans le cambouis et se poser des questions qu'on n'a pas le temps ou l'envie de se poser au travail, c'est ça qui fait de vous autre chose qu'une <abbr title="pisseur de code">dactylo-codeuse</abbr></li>
  <li>du code open-source a été amélioré ou créé, grâce à l'infatiguable Cédric Morin : Spip, <a lang="en" href="https://github.com/Cerdic/CSSTidy">CSSTidy</a>, <a lang="en" href="https://github.com/Cerdic/jQl">jQueryLoader</a></li>
  <li>nous avons testé les limites de <abbr>YOTTAA</abbr> (un bug trouvé et corrigé) et de WebpageTest.org</li>
</ul>
Vous retrouverez un bilan plus technique dans <a href="http://braincracking.org/?p=696">le dernier post</a>.

Bravo à l'organisateur principal <a href="http://zeroload.net/" title="consultant performance web">Zeroload</a> qui a eu l'idée et qui a consacré le plus de temps à ce projet. Bravo également à tous les concurrents que je n'ai pas cité dans les différents posts, ça n'était pas facile mais vous comme les autres en retireront une meilleure connaissance. Merci à mes collègues juges <a href="http://eric.daspet.name/">Eric Daspet</a> et <a href="http://www.phpied.com/category/performance/">Stoyan Stefanov</a> et enfin Merci à nos sponsors <a href="http://www.yottaa.com/" title="web performance tools ">YOTTA</a>, <a href="http://www.catchpoint.com/">Catchpoint System</a> et <a href="http://www.alwaysdata.com/" title="hébergement">Always Data</a> d'avoir fourni hébergement et lots
