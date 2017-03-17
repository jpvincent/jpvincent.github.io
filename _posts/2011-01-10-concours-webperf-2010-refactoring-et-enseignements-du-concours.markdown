---
layout: post
title:  "Concours Webperf 2010 : refactoring et enseignements du concours"
date:   2011-01-10 16:04:21 +0100
---
Ce post est le dernier d'une série de 3 et est tiré de l'expérience des finalistes du concours de performance Web 2010
<nav>
<ol>
  <li><a href="http://braincracking.org/?p=695">Les bases</a></li>
  <li><a href="http://braincracking.org/?p=617">Maîtriser le chargement</a>.</li>
  <li>Refactoring, enseignements.</li>
</ol>
</nav>
Tout développeur Web sera content de voir que les bonnes pratiques de codage aident réellement les performances de rendu, en plus de leur intérêt propre. J'ai également résumé les enseignements que j'ai tiré de ce concours

<!--more-->

<h2><abbr>HTML</abbr>, <span lang="en">markup</span> et <abbr>DOM</abbr></h2>
Quand on me dit qu'on peut accélérer le rendu d'une page en modifiant le <abbr>HTML</abbr>, je commençais généralement à rigoler doucement : comme pour le <abbr>CSS</abbr>, je n'ai vu aucune démonstration du phénomène
Mais à bien y repenser je me souviens des grandes années du Web où j'étais passé de <span lang="en">layouts</span> à base de <code>table</code>s imbriquées à un mix de <code>div</code>s et de règles <abbr>CSS</abbr> externalisées : non seulement le poids des pages était réduit (et à cette époque pré-<abbr>ADSL<abbr>, c'était encore important) mais sur les machines de l'époque on pouvait sentir la différence de rapidité d'affichage.
Et aujourd'hui on retombe parfois sur des sites dont la maintenance n'est pas évidente, l'équipe sous pression et où chaque ajout au design s'accompagne d'une pléthore de div et de règles CSS ultra spécifiques. Même sur nos machines modernes, avec IE7 le rendu peut être ralenti sans que l'on s'en rende vraiment compte.</abbr></abbr>

Force est de constater que les concurrents qui ont simplifié à la main le HTML (réduction de 30% à 50% des 1900 éléments du <abbr>DOM</abbr>) ont constaté un réel gain de vitesse de rendu. Je n'ai aucun graphe pour le prouver (imaginez la difficulté pour mesurer cela), juste les ressentis concordants de plusieurs concurrents et un constat sur leurs pages finales.

L'ordre d'affichage a aussi son importance : sur une page statique comme ici, ça n'était pas visible, mais un site dynamique comme la FNAC pourrait très bien envoyer son contenu par blocs en <a lang="en" href="http://fr2.php.net/flush">flushant</a> dès que possible des blocs de <abbr>HTML</abbr>. Certains comme Cédric Morin ont donc travaillé le <abbr>CSS</abbr> pour pouvoir mettre en premier dans la source les éléments importants comme la colonne centrale.
<h2>Produire du bon code !</h2>
Certains concurrents comme la jusqu'au-boutiste <a href="http://twitter.com/theasta">Alexandrine Boissière</a> sont partis sur un <span lang="en">refactoring<span> complet du code, du <abbr>HTML</abbr> au <abbr>CSS</abbr> en passant par JavaScript ! Même si ça ne ressort pas forcément bien sur les graphes (par rapport aux autres finalistes), le rendu au <a href="http://www.webpagetest.org/video/compare.php?tests=101202_9729ae2f68ec0a91009a612e2e60ea7d-r:1-c:0&amp;thumbSize=200&amp;ival=100">premier affichage</a> est parfait (on voit en moins d'une seconde le contenu principal), et la sensation de fluidité au <span lang="en">scroll</span> ou à l'utilisation est saisissante : attendez le chargement et scrollez sur la <a href="http://entries.webperf-contest.com/base-fnac-ftw/index.html">page originale</a> et sur <a href="http://entries.webperf-contest.com/4cd5940647d03/index.html">cette page</a>. Ce n'est pas le genre de donnée qui se calcule, sauf peut être en regardant les courbes <abbr>CPU</abbr>, mais un robot ne peut pas le calculer.</span></span>

<abbr title="Internet Explorer 7">IE7</abbr> avait visiblement du mal à afficher un arbre <abbr>DOM</abbr> aussi gros (213Ko bruts pour 2000 éléments) et avec autant d'erreurs : plus de <a href="http://validator.w3.org/check?uri=http://entries.webperf-contest.com/base-fnac-ftw/index.html&amp;charset=(detect+automatically)&amp;doctype=Inline&amp;group=0">2300 erreurs</a> repérées par le validateur pour du <code>HTML4.01 Transitional</code>, c'est rare. On peut donc imaginer que le parser de <abbr>IE</abbr> souffrait pour afficher la page. Certains finalistes ont :
<ul>
  <li>diminué de moitié le nombre de noeuds du <abbr>DOM</abbr>, ce qui influence clairement sur la fluidité finale</li>
  <li>nettoyé le <abbr>HTML</abbr> des erreurs qui ont peut être une influence sur le <span lang="en">parser</span> ( <abbr>IDs</abbr> non conformes,</li>
  <li>supprimé les tableaux : <abbr>IE</abbr> a toujours eu du mal avec l'affichage de tableaux imbriqués</li>
  <li>nettoyé les appels <abbr>JS</abbr> <span lang="en">inline</span></li>
  <li>corrigé des erreurs de <abbr>HTML</abbr> comme des <abbr>URLs</abbr> malformées (!)</li>
  <li>enlevé les paramètres de session des <abbr>URLs</abbr>, pour les rajouter en <abbr>JS</abbr> dynamiquement</li>
</ul>
Au niveau <abbr>CSS</abbr> :
<ul>
  <li>mise en place d'un <code>reset.css</code>, ce qui permet de supprimer beaucoup de déclarations</li>
  <li>suppression des <code>filter()</code> <abbr>IE</abbr>, consommateurs de ressources. Je note au passage que cela augure du mauvais pour <abbr>CSS3</abbr> : il ne faudra pas abuser des opacités, ombrages et autres tranformations ou transitions</li>
  <li>séparation des <abbr>CSS</abbr> texte des <abbr>CSS</abbr> avec images encodées</li>
  <li>nettoyage des règles <abbr>CSS</abbr> inutiles sur cette page, soit 75% de la feuille. Ce nettoyage est largement discutable, car on a affaire à une feuille pour tout un site. Cependant 3/4 de règles non utilisées, ça vaut le coup à mon avis de soulager le <span lang="en">parser</span> en créant un <abbr>CSS</abbr> par type de page, généré automatiquement en concaténant une feuille de style par module par exemple.</li>
  <li>utilisation de <abbr>CSS3</abbr> et de dégradation gracieuse pour les bords arrondis ou les effets de reflets. C'est un choix que nous cautionnions, même s'il faut bien avouer que peu d'équipes Web arrivent à échapper au diktat du <span lang="en">pixel-perfect</span></li>
  <li>utilisation de <code>:before</code> pour certains éléments répétitifs (<abbr title="Internet Explorer 6">IE6</abbr> n'était pas à prendre en compte)</li>
  <li>dégradés gérés avec un seul <abbr>PNG</abbr> transparent, la couleur étant définie en <abbr>CSS</abbr></li>
</ul>
Au niveau JavaScript il y avait pas mal de ré-écriture de code à faire, même si j'estimais personnellement cela hors-sujet du concours. Certains candidats ont pourtant gagné en fluidité grâce à cela. Je les cite en vrac :
<ul>
  <li>remplacement du widget d'<span lang="en">autocomplete</span></li>
  <li>moins de manipulations de <abbr>DOM</abbr></li>
  <li><abbr title="+">plus</abbr> de cache sur les variables</li>
  <li>optimisation des sélecteurs <span lang="en">jQuery </span></li>
  <li>accès par <abbr>ID</abbr> et moins par sélecteurs (opinion personnelle : <span lang="en">jQuery</span> habitue mal les développeurs)</li>
  <li>suppression du <code>document.write()</code></li>
  <li>remplacer les modifications de style sur des éléments par des changements de classe</li>
  <li>mutualiser les <abbr title="XMLHttpRequest, dit AJAX">XHR</abbr> qui récupéraient les infos de stock</li>
</ul>
<h2>Les enseignements du concours sur la performance Web</h2>
<ul>
  <li><abbr>MHTML</abbr> est très consommateur de <abbr>CPU</abbr> et met à mal <abbr>IE7</abbr>. A utiliser avec précaution et lui préférer les sprites si nécessaire</li>
  <li>la taille du <abbr>DOM</abbr> et sa complexité jouent réellement sur la vitesse et la fluidité d'affichage</li>
  <li>diviser par 10 le temps de rendu et améliorer la fluidité d'une page comme La Fnac a maintenant un prix : une semaine à temps complet pour un développeur dans les conditions d'une page statique. A vue de nez, pour industrialiser cela, il faut peut être décupler ce temps ?</li>
  <li>les optimisations de base sont déjà très efficaces et faciles à faire, mais dans ce cas ne suffisent pas : il faut peut être envisager un refactoring</li>
  <li>les bonnes pratiques du développement Web et l'organisation de son code HTML/CSS aident réellement à garder de bonnes performances</li>
  <li>les meilleurs concurrents étaient également les mieux organisés : versioning, tests automatisés des fonctionalités, processus de mise en ligne... Ils étaient également les plus clairs dans leurs notes ce qui je pense n'est pas un hasard</li>
  <li>il y a des consultants en performance qui ont participé mais les finalistes et les vainqueurs ne disent pas être des pros de la performance Web. Juste beaucoup de travail, des bonnes pratiques et de l'auto-formation avant et pendant le concours</li>
  <li>dans les techniques utilisées, il n'y a eu que du connu, malgré la cinquantaine de cerveaux qui se sont réellement penchés sur la question. Cela signifie peut être que l'on a fait le tour des techniques Webperf, et que le combat sera maintenant de les industrialiser et d'en répandre l'usage.</li>
  <li>en France on n'a pas de pétrole, mais on a de bons <abbr title="Développeur Web">Webdevs</abbr>. Les 2 tiers des participants étant français, on peut se dire qu'il est statistiquement normal d'avoir 3 gagnants français. Mais si on regarde la quinzaine de finalistes en haut <a href="http://webperf-contest.com/leaderboard-fr.html">du tableau</a>, il y a sur-représentation.</li>
</ul>
