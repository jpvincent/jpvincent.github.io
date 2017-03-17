---
layout: post
title:  "Concours de performances Web : à vous de jouer"
date:   2010-10-27 16:04:21 +0100
---
Hier s'est ouvert le premier (ok, <a href="http://www.fastwebrace.com/">le second</a> dans l'idée) concours international de Performances Web, il durera jusqu'au 30 novembre.
<!--more-->
EDIT : le concours 2010 est maintenant fini, vous pouvez voir les gagnants <a href="http://webperf-contest.com/">ici</a> et <a href="http://braincracking.org/?p=717">l'analyse des techniques employés là</a>

<h2>Comment ça marche ?</h2>
Le principe est simple : tous les développeurs doivent <strong>optimiser la même page</strong>, page prêtée par <a href="http://www.fnac.com/enfants.asp">La Fnac</a>, et mise à disposition sous forme de fichiers statiques que vous pourrez modifier à volonté. On parle donc bien d'un concours de performances frontales, puisque le code backend ne peut pas influer sur le temps d'affichage de la page. Ce qui tombe bien car c'est là où passe 90% du temps d'affichage : sur cette <strong>page très représentative</strong> des sites Web d'aujourd'hui, 1 seconde suffit à générer le HTML, les fichiers statiques sont servis en 50ms chacun, ce qui correspond à des performances honorables côté backend. <strong>Mais le site n'est pas utilisable avant plusieurs secondes </strong>(voir <a href="http://www.webpagetest.org/result/101026_9ANH/">ce graphique</a>).  <a href="http://webperf-contest.com/register-fr.html">Inscrivez vous</a> donc et visez une des 3 catégories (ou toutes, ce qui est théoriquement possible) :
<ul>
  <li>meilleur score YOTTA (sponsor principal fournissant des outils de monitoring de performances). Le score YOTTA est, je cite : <cite lang="en">"an overall performance assessment that takes into account page load time, global reachability, yslow and complexity"</cite>. Récompense : <strong>un iPad</strong></li>
  <li>meilleur temps d'affichage, calculé par l'indispensable <a href="http://www.webpagetest.org">webpagetest.org</a> depuis un IE7 à Paris. Le start render actuel est à 2.5s et n'est pas très utile (voir <a href="http://www.webpagetest.org/results/10/10/26/9ANH/1_screen_render.jpg">ce screenshot</a>), d'autant qu'il est suivi par un freeze du navigateur perceptible à la souris nue, et visible facilement sur <a href="http://www.webpagetest.org/waterfall.png?type=connection&amp;width=930&amp;test=101026_9ANH&amp;run=1&amp;cached=1">ce graphique</a> où l'on voit le CPU plafonner à 100% pendant plusieurs secondes. Le temps onload total est de plus de 7 secondes, je pense que le diviser en 2 avec des fichiers statiques et un peu de malice est largement jouable. Récompense : un <strong>iPod touch</strong></li>
  <li>plus petit nombre de requêtes HTTP et poids total, calculés en additionnant première et seconde impression de la page. La difficulté dans cette catégorie est que les techniques permettant l'un et l'autre sont parfois antinomiques. Récompense : <strong>des livres dédicacés</strong> par <a href="http://phpied.com/">Stoyan Stefanov</a></li>
</ul>
Une fois inscrit, vous recevez un espace FTP et un ZIP contenant les fichiers statiques.
<h1>J'ai une chance ?</h1>
L'idée du concours est bien sur d'apprendre en s'amusant, avec la concurrence comme aiguillon. En passant du temps sur cette page de la FNAC, vous allez pouvoir mettre les mains dans le cambouis, et on sait bien qu'en terme de développement en général, et d'optimisation en particulier, c'est la meilleure école. A mon sens la connaissance basique des performances Web fait maintenant partie intégrante de l'arsenal d'un développeur Web moderne, au même titre que le respect des balises HTML ou du passage des layouts en tableau au tout-CSS il y a plusieurs années.  Accessoirement, c'est l'un des premiers concours avec ce thème et la discipline commence à peine à être maîtrisée. Pour preuve, il doit y avoir moins de 10 professionnels vivant uniquement du consulting sur les performances Web dans le monde, et un seul en France (<a href="http://zeroload.net/">Zerolad</a>, par Vincent Voyer, organisateur principal). Nous sommes donc tous plus ou moins amateurs, peu nombreux et vous avez dès lors de vraies chances de gagner.
<h1>Quelques conseils</h1>
Etant donné que je suis juge, je vais tenter de rester impartial pour ne pas favoriser les français ou mes lecteurs. Cependant je vais vous faciliter la tâche en rappelant ces quelques ressources qui vous donneront une idée de par où commencer. Ces ressources sont publiques et vous serviront même en dehors de ce concours.
<ul>
  <li>la liste de référence toujours valable <a href="http://developer.yahoo.com/performance/rules.html">faite par Yahoo</a>! : Dans le cadre de ce concours, il est clair qu'il vous faut au moins assurer sur la majorité de ces conseils. C'est de toute façon une connaissance et des techniques qui vous serviront sur tous les sites.</li>
  <li>l'autre liste de référence, <a href="http://code.google.com/intl/fr/speed/page-speed/docs/rules_intro.html">faite par Google</a>. Les points se recoupent aussi en général je me contente de citer la liste de Yahoo!. Mais pour ce concours, vous voudrez certainement optimiser plus avant et certains points innovateurs de Google devraient vous aider à gratter des dixièmes de secondes ou quelques kilos</li>
  <li>les archives d'<a href="http://performance.survol.fr/">Eric Daspet</a> (co-organisateur et juge), qui bloguait en français à l'époque où toutes les règles de performances commençaient à sortir</li>
  <li>mes propres archives des <abbr title="Revue de Presse Web">RPW</abbr> taguées "<a href="http://braincracking.org/tag/perfs/">Perfs</a>" qui couvrent elles l'année écoulée et qui contiennent des conseils généraux et aussi quelques techniques extrêmes difficile à sortir en production mais qu'il serait intelligent de déployer dans un concours de performances Web :)</li>
</ul>
Côté outils, pour vous éviter beaucoup de choses fastidieuses, surtout les premières heures :
<ul>
  <li>les classiques <a href="http://developer.yahoo.com/yslow/">ySlow</a> et <a href="http://code.google.com/intl/fr/speed/page-speed/download.html">pageSpeed</a>, pour repérer facilement les problèmes basiques, sur Firefox. Le score ySlow fait d'ailleurs partie du score YOTTA</li>
  <li>le complexe mais très utile <a href="http://ajax.dynatrace.com/pages/download/download.aspx">dynatrace AJAX</a>, pour Internet Explorer. Il permet notamment de comprendre comment IE affiche une page, et peut vous aider à repérer d'autres problèmes invisibles de firebug</li>
  <li>le méconnu <a href="http://sourceforge.net/projects/pagetest/files/">AOL page Test</a> pour IE, qui est en fait le logiciel utilisé par WebPageTest.org . 3 avantages :
<ul>
  <li>Vous verrez votre page comme la verront les juges :)</li>
  <li>Vous ne dépendez pas de Webpagetest.org qui sera forcément saturé de demandes des autres concurrents</li>
  <li>Vous ne révélerez pas votre URL secrète par erreur aux autres concurrents en passant par WebPageTest.org . Il suffit d'oublier de cocher la case "private" une fois ...</li>
</ul>
</li>
  <li>IE7 ! c'est notre étalon, il ne faut donc pas se baser sur des navigateurs plus récents comme IE8 et Firefox. Vous devriez y installer AOL page Test pour voir la page telle que Webpagetest.org la testera. Faîtes attention aux packs d'IE : ils dépannent dans la majorité des cas, mais j'ai déjà noté des différences de comportement (filtres CSS, interaction avec plugins et addons ...) et je ne sais pas si cela influe sur la manière d'afficher une page (nombre de requêtes parallèles par exemple). Utilisez une machine virtuelle pour chaque IE reste un bon conseil.</li>
</ul>
Et enfin des discussions :
<ul>
  <li>le <a href="http://groups.google.fr/group/performance-web">groupe de discussion performances Web</a> français lancé cet été à la suite de la première soirée Webperfs où vous devriez trouver tous ceux qui ressemblent à un expert des performances Web</li>
  <li>un forum ouvert récemment pour l'occasion sur <a href="http://www.developpez.com/redirect/183">developpez.com</a>, et qui restera sûrement après le concours. Nous surveillons ce forum pour répondre à vos questions si vous sentez que vous tentez des optimisations un peu trop extrêmes et que vous n'êtes pas surs que nous les accepterons au final</li>
  <li>et bien sur le twitter officiel du concours : @<a href="http://twitter.com/webperf_contest">webperf_contest</a> qui vous permet de dialoguer directement avec nous</li>
</ul>
Un dernier conseil pour ce concours : remplissez bien votre Gist de toutes les modifications que vous faîtes. Nous nous doutons que les finalistes arriveront dans la même plage de 50-100ms et il serait injuste que les aléas du réseau choisissent un gagnant. Pour les départager, nous allons tout simplement lire les techniques que vous aurez appliquées et voter au plus méritant (originalité, facilité de mise en oeuvre ...)  Voilà, <a href="http://webperf-contest.com/register-fr.html">inscrivez vous</a>, téléchargez et jouez avec la page. C'est le moment d'apprendre et peut être même de gagner des bouquins ou un peu d'électronique :)
