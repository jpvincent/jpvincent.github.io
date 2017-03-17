---
layout: post
title:  "Concours de performances Web : à vous de jouer"
date:   2011-01-10 16:04:21 +0100
---
Ce post est le premier d'une série de 3 et est tiré de l'expérience des finalistes du concours de performance Web 2010
<nav>
<ol>
  <li>Les bases</li>
  <li><a href="http://braincracking.org/?p=617">Maîtriser le chargement</a>.</li>
  <li><a href="http://braincracking.org/?p=696">Refactoring, enseignements</a>.</li>
</ol>
</nav>
Il traite rapidement des actions classiques en Performances Web qui sont les plus faciles à mettre en place tout en offrant un retour sur investissement souvent suffisant. Il devrait être utile aux novices mais ceux qui connaissent déjà les bases devraient aller voir les 2 articles suivants.
<!--more-->

<h2>La configuration d'Apache</h2>
Les premières actions pour améliorer à peu de frais et de manière impactante les performances Web passent par la configuration de son serveur Web. Si vous voulez un exemple complet, regardez <a href="http://entries.webperf-contest.com/4ce63091aafd1/sources/htaccess.txt">ce .htaccess</a>, fait par <a href="http://twitter.com/omarnetfr">Omar Haddouchi</a>, qui a fait par ailleurs un <a href="https://gist.github.com/22fbd663646947a3d3ba">boulot impressionant</a>. Il y a principalement 3 actions :
<h3>Gérer le cache navigateur</h3>
Il s'agit de demander au navigateur de garder chez lui autant que possible les fichiers statiques. Non les techniques à base de <a href="http://en.wikipedia.org/wiki/HTTP_ETag">ETag</a> et de réponse 304 sont quasiment inutiles car une requête <abbr>HTTP</abbr> est tout de même faite pour vérifier la fraîcheur du fichier. Et sur des fichiers de page Web, généralement entre 1 et 200Ko, l'aller-retour au serveur seul est plus long que le téléchargement. Voici un exemple de configuration :
<pre><code>&lt;IfModule mod_expires.c&gt;
  ExpiresActive On
  ExpiresDefault "access plus 1 month"
  ExpiresByType image/jpeg "access plus 1 month"
# ../..
  ExpiresByType text/javascript "access plus 1 month"
&lt;/IfModule&gt;
</code></pre>
Un mois ça n'est pas encore assez agressif, la valeur généralement <a href="http://developer.yahoo.com/performance/rules.html#expires">recommandée</a> est de 10 ans :) Par contre cela demande une très bonne gestion (dynamique donc) des URLs qui mènent aux fichiers statiques, afin de faire recharger les <abbr>JS/CSS/</abbr>images que vous mettez à jour.
<h3>La compression <abbr>Gzip</abbr></h3>
Compresser à la volée les fichiers non binaires (<abbr>HTML, CSS, JS</abbr>) vous épargne d'envoyer 80% de leurs poids sur votre réseau, et aide vos utilisateurs à petit débit (à l'étranger, sur mobile, un <a href="http://royal.pingdom.com/2010/11/12/real-connection-speeds-for-internet-users-across-the-world/">petit pourcentage</a> de français). Compresser du binaire est généralement inutile et consommateur de ressources serveur, d'où cette configuration :
<pre><code>&lt;IfModule mod_gzip.c&gt;
  mod_gzip_on       yes
  mod_gzip_dechunk  yes
  mod_gzip_item_include file      .(html|txt|css|js|json)$
  mod_gzip_item_exclude file      .(jpg|png|gif|ico)$
&lt;/IfModule&gt;
</code></pre>
Vous pouvez aller encore plus loin avec gzip et ne pas faire faire la compression par Apache à chaque requête mais plutôt en générant des fichiers gzipés (au build, ou de manière dynamique) de vos HTML (si la page est statique) et CSS/JS et en servant cette version si le navigateur le supporte. Voici un exemple de configuration :
<pre><code>
# déclaration des types des fichiers gzipés
AddType "application/javascript" .jsgz
AddType "text/css" .cssgz
AddType "text/html" .htmlgz
# ajout de l'entête gzip pour ces fichiers
AddEncoding gzip .jsgz
AddEncoding gzip .cssgz
AddEncoding gzip .htmlgz
</code></pre>
Cet exemple est pris du <code>.htaccess</code> de <a href="ftp://ftp.alwaysdata.com/entries/4ccbf6fa4c539/.htaccess">Etienne Guilluy</a>. Dans le cadre du concours, l'intérêt était limité, mais en production l'avantage est évident car il permet d'épargner du CPU et donc de mieux résister aux charges.
<h3>Supprimer les <span lang="en">cookies</span> inutiles</h3>
Un <span lang="en">cookie</span> fait entre 0.5 et 4ko. Une page moyenne déclenche une cinquantaine de requêtes (115 sur cette page de la FNAC ...). Si vous n'y prenez pas garde vous allez donc demander à votre utilisateur d'uploader chez vous entre 25Ko et 200Ko (le double donc pour la FNAC) alors que le cookie n'est généralement utile que pour la partie dynamique qui génère le <abbr>HTML</abbr>, et que l'upload des <abbr>ADSL</abbr> beaucoup plus précieux que le download.

La technique consiste à mettre ses fichiers statiques sur un autre nom de domaine ou sous-domaine et à ne pas activer les cookies. Pour ce concours, nous fournissions des sous-domaines, mais il fallait penser à remplacer la définition du cookie sur le domaine faite en JS dans le marqueur pour en bénéficier complètement.
<h2>Le poids des images</h2>
et le choc des mots... Mais je m'égare :) L'avantage de ce concours, c'est que certains ont pu revisiter les images de la FNAC qui comme sur beaucoup de sites sont fabriquées à la chaîne, sans optimisation particulière. Plusieurs concurrents ont donc réduit le poids des images avec les techniques suivantes :
<ul>
  <li>réduction de la palette de couleurs pour les <abbr>PNG</abbr> 8 bits</li>
  <li><span lang="en">indexed colors</span> pour les images de moins de 256 couleurs</li>
</ul>
<h2>Optimisation du CSS</h2>
<h3>Automagique</h3>
La technique générale a été de générer une version "de production" par un outil, en voici plusieurs :
<ul>
  <li><a lang="en" href="http://csstidy.sourceforge.net/">CSS Tidy</a> pour le compresser</li>
  <li>l'<span lang="en">addon</span> Firefox <a lang="en" href="https://addons.mozilla.org/en-US/firefox/addon/10704/">css-usage</a> pour ne récupérer que les <abbr>CSS</abbr> utilisés</li>
</ul>
Il n'y a pas vraiment d'outil meilleur que l'autre en terme de performance pure (poids final et rendu). Prenez donc celui qui s'adapte le mieux à votre environnement de mise en production (vous en avez un n'est ce pas ?) et vérifiez bien que tous vos hacks restent compris.

Si dans le cadre du concours il était intelligent de réduire le <abbr>CSS</abbr> en supprimant les lignes inutilisées dans cette page, en environnement de production, c'est forcément plus délicat
<h3>A la main</h3>
Dans les recommandations Google se trouve depuis le début un conseil sur <a href="http://code.google.com/intl/fr/speed/page-speed/docs/rendering.html#UseEfficientCSSSelectors">les sélecteurs CSS</a> qui permettent d'accélérer le rendu. Théoriquement c'est évident. Mais on va se parler franchement : je n'ai jamais constaté ou lu que cela permettait de gagner quoi que ce soit de significatif ( &gt; 100ms).
