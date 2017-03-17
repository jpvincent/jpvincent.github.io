---
layout: post
title:  "Migrer un site de production vers un markup HTML5"
date:   2010-02-08 16:04:21 +0100
---
A force de <a href="http://braincracking.org/blog/category/rpw/">couvrir l'actualité du développement web</a> où l'on parle d'HTML5 à peu près tous les jours, on finit par être frustré des démos faites sur des pages qui n'ont pas grand chose à voir avec <strong>les contraintes que l'on a en production</strong> : compatibilité IE6, contenus lourds, sites complètement dynamiques et édités par plusieurs dizaines de mains dont certaines non techniques.

En tombant <a href="http://inspectelement.com/tutorials/code-a-backwards-compatible-one-page-portfolio-with-html5-and-css3/">ici</a> et <a href="http://www.alsacreations.com/article/lire/947-osez-creer-site-html5-css3.html">là</a> sur des exemples de migration de layout (sur des pages de portfolio) vers HTML5, je me suis rendu compte qu'une certaine partie de HTML5 était déjà utilisable en production de manière assez peu coûteuse. Voici donc ce que j'ai du faire pour <strong>passer un site de production en HTML5</strong>.

L'objectif est donc de rajouter toute la sémantique HTML5 utile possible, rôles ARIA inclus, sans rien casser du design ou de la compatibilité existante.
<h1>CSS et JS</h1>
<h2>librairie CSS</h2>
Premier problème : j'utilise un système de CSS qui m'a permis au tout début du site de proposer facilement des variations de layout, et qui sera bien pratique lorsque le site se proposera en marque blanche : il s'agit de <a href="http://developer.yahoo.com/yui/grids/">YUI grids</a>. Couplé à leur CSS reset et au font.css, il permet d'obtenir un <strong>rendu uniforme des éléments de base sur tous les browsers</strong>. Hors de question donc de s'en passer, mais elle faisait référence exclusivement à des éléments de type DIV, alors que je m'apprête à en supprimer quelques unes. J'ai donc du la fixer, même si je n'aime pas modifier des librairies, car cela pose des <strong>problèmes de maintenance</strong> par la suite. Je leur ai <a href="http://yuilibrary.com/projects/yui2/ticket/2528762">ouvert un ticket</a> en espérant faire évoluer la librairie.
<h2>CSS Reset</h2>
Une fois ma librairie fixée, je me suis rendu compte que <strong>tous les browsers affichent par défaut inline les élément inconnus</strong>. Ce qui veut dire par exemple qu'une &lt;DIV&gt; qui doit devenir un &lt;HEADER&gt; ne sera plus en <em>display:block;</em>.
<ul>
  <li>j'ai trop de modifications CSS à faire pour rattraper cette règle cas par cas</li>
  <li>il n'est pas précisé dans les specs un valeur display par défaut (j'imagine que cela viendra plus tard ?).</li>
  <li>les anciens browsers ne se mettront jamais à jour et il n'est pas sur que les nouveaux s'entendent sur des valeurs par défaut</li>
  <li>je ne veux pas me contenter de noyer les DIVs existantes dans les nouvelles balises, mais bien remplacer ce qui peut l'être</li>
  <li>j'ai déjà un fichier reset.css qui me garantit le même affichage de tout le HTML sur tous les browsers</li>
</ul>
J'ai donc décidé d'étendre l'idée du reset.css aux nouvelles balises HTML5, en lisant ce pour quoi chacune était faite et en leur imposant une valeur de display par défaut en conséquence :
<pre><code>/* some of the HTML5 tags that should display as blocks by default */
article,
aside,
audio,
canvas,
datagrid,
datalist,
details,
dialog,
figure,
footer,
header,
menu,
nav,
section,
video {
display: block;
}
/* some of the HTML5 tags that should display as inline text by default */
abbr,
eventsource,
mark,
meter,
time,
progress,
output,
bb {
display:inline;
}</code></pre>
Que pensez vous des choix faits entre <em>block</em> et <em>inline</em> ?

NB : D'après certains, <strong>du point de vue des performances</strong>, c'est une mauvaise pratique que de targeter en css des éléments HTML, il vaudrait mieux privilégier les classes et IDs. Personnellement je n'ai jamais pu constater de différence, et tous les fichiers de "CSS reset" du monde sont bien obligés d'en passer par là. Connaissez vous des cas où cette notation peut ralentir ? Y-a-t il une autre solution ?
<h2>Fixer IE</h2>
IE (de 6 à 8) ne sait pas styler des éléments qu'il ne connaît pas. Je vais donc utiliser <a href="http://remysharp.com/2009/01/07/html5-enabling-script/">la même technique que d'autres</a> pour <strong>hacker IE</strong> : <strong>créer tous les types de tag HTML5 au moins une fois</strong>. Voici <a href="http://timeofmylife.com/assets/js/html5tagcreate.js">le JS final</a>, fortement inspiré de <a href="http://remysharp.com/2009/01/07/html5-enabling-script/">celui-ci</a>, optimisations paranoïaques en plus.
<pre><code>// inspired from http://remysharp.com/downloads/html5.js
// creates all know HTML5 tags once, for IE to be able to style them
// For discussion and comments, see: http://remysharp.com/2009/01/07/html5-enabling-script/
(function(){
if(!/*@cc_on!@*/0)
return;
var e = 'abbr,article,aside,audio,bb,canvas,datagrid,datalist,details,dialog,eventsource,figure,footer,header,hgroup,mark,menu,meter,nav,output,progress,section,time,video'
.split(','),
create = document.createElement;
for(var i=0, iTotal = e.length; i&lt;iTotal;i++) {
create(e[i]);
}
})();</code></pre>
Je vois beaucoup de démos utilisant les <strong>commentaires conditionnels IE</strong> pour inclure ces lignes de JS qui ne servent qu'à IE. Du point de vue des performances, il faut limiter au maximum le nombre d'objets externes à une page, en particulier celui des <strong>fichiers JS qui bloquent le rendu de la page</strong> tant qu'ils ne sont pas téléchargés et interprétés. IE étant justement le browser qui a le plus besoin de ce genre d'optimisation, <strong>j'inclus ce fichier dans le JS principal du site</strong>, où il va d'ailleurs rejoindre d'autres hacks IE.

Question maintenance c'est un peu embêtant car on se retrouve avec 2 fichiers (css + js) comportant la même liste d'objets à mettre à jour en cas d'addition/suppression de tags HTML5. Rien d'ingérable cependant.
<h1>HTML</h1>
<h2>HEAD</h2>
N'étant pas un utilisateur des validateurs W3C et encore moins de XHTML, les pages ont toujours été définies en HTML 4 transitionel :
<code>&lt;!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"   "http://www.w3.org/TR/html4/loose.dtd"&gt;</code>
On passe donc à la nouvelle balise
<pre><code>&lt;!DOCTYPE html&gt;</code></pre>
et c'est tout ... j'ai lancé mes 3 machines virtuelles avec des installations propres de <strong>IE6, IE7 et IE8</strong> en cherchant partout un bug CSS ou même JS qui signalerait qu'on est passé en <em>quirksmode</em>, et RIEN, le <strong>site s'affiche normalement</strong>.

Développeurs, avez vous constaté des différences d'affichage en passant à HTML5 en partant d'un autre doctype que HTML4 transitional ?
<h2>Formulaires</h2>
HTML5 introduit une <strong>dizaine de nouveaux types de champs</strong> (<em>email</em>, <em>url</em>, <em>datetime</em> ...). Il faut utiliser Opéra, qui va jusqu'à afficher un <strong>widget calendrier sur un champs datetime</strong>, pour comprendre ce que les browsers pourront en faire dans les années à venir. Safari iPhone lui adapte le clavier virtuel au type de champs. Le bonus, c'est que <strong>de IE6 au dernier Chrome</strong>, les input de type inconnus sont reconnus comme des &lt;input type="text"&gt; ! C'est dont à la fois sans risque et avec un certain intérêt que l'on peut les introduire dès maintenant
<pre><code>&lt;input type="email" name="loginemail" id="loginemail" placeholder="votremail@votrefai.com" /&gt;</code></pre>
HTML5 introduit également l'attribut "<em>placeholder</em>" qui contient le texte par défaut à afficher lorsqu'il n'y a pas de focus sur le champs. Si cela vous fait penser à un code JS que vous avez probablement déjà implémenté une bonne dizaine de fois, c'est normal, et vous <strong>pourrez le réutiliser en fallback</strong>. <a href="http://timeofmylife.com/assets/js/defaultvalue.class.js">Voici un exemple de code</a>
<h2>Rôles ARIA et nouvelle sémantique</h2>
Dans les templates qui définissent mes quelques layouts, j'ai remplacé certaines &lt;<em>DIV</em>&gt; par un markup qui a plus de sens : &lt;<em>HEADER</em>&gt;, &lt;<em>FOOTER</em>&gt;, &lt;<em>NAV</em>&gt; et &lt;<em>SECTION</em>&gt;. Les 2 premières parlent d'elles même, les <a href="http://dev.w3.org/html5/spec/semantics.html#sections">specs</a> ne disent pas si elles doivent être unique ou pas, la logique suggérant que oui. &lt;<em>NAV</em>&gt; correspond aux différents éléments de navigation et &lt;<em>SECTION</em>&gt; quant à elle n'a pas plus de signification que &lt;<em>DIV</em>&gt;, l'utilisation que j'en fais est donc tout à fait arbitraire : <strong>je garde &lt;<em>DIV</em>&gt; pour le positionnement</strong>, et &lt;<strong><em>SECTION</em>&gt; pour les endroits qui entourent directement le contenu</strong>, et auxquelles je vais rajouter un role ARIA.

Il existe d'autres balises qui seront surtout utiles aux sites de contenu comme <strong>&lt;</strong><em><strong>ARTICLE</strong></em><strong>&gt; et &lt;</strong><em><strong>ASIDE</strong></em><strong>&gt; </strong>(contenu relatif), mais dont je n'ai pas l'utilité sur un <a href="http://www.timeofmylife.com?utm_source=jpv">réseau social qui permet de stocker son contenu numérique</a>. D'autres balises sont beaucoup plus spécifiques : &lt;<em>HGROUP&gt;</em> permet de faire une table de matières ou &lt;<em>ADRESS</em>&gt; qui contient un moyen de contact relatif à l'article ou à la page entière. J'utilise cette dernière pour <strong>baliser le lien vers "nous contacter"</strong> en bas de page, car <a href="http://dev.w3.org/html5/spec/semantics.html#the-address-element">la spec</a> laisse supposer que le navigateur devrait mettre en avant cette information... Là encore, c'est de la sémantique, donc il y a une grande part d'interprétation : au final ce sont les browsers et surtout les moteurs de recherche qui dicteront les balises utilisées et leurs usages
<h1>Bénéfices</h1>
Sémantiquement le <strong>markup devient très lisible</strong>, cela va donc bénéficier aux lecteurs d'écran pour la partie ARIA. Concernant les balises<strong> HTML5, à ma connaissance personne ne les prend en compte actuellement</strong>. Mais gageons que <strong>dans un futur proche</strong> les moteurs de recherche, les lecteurs d'écran ou tout autre robot/browser vont implémenter de quoi exploiter le markup HTML5 car après tout on leur mâche le travail, et <strong>Yahoo/Google ont déjà fait l'effort pour les microformats</strong> alors même que ceux ci ne sont pas des standards W3C.

Vu l'orientation sémantique des balises HTML5, on peut imaginer que les <strong>premiers sites de news à se lancer auront un avantage concurrentiel</strong> sur leurs concurrents, c'est moins évident pour les sites qui dépendent peu du référencement.

Cela permet accessoirement de commencer à <strong>se lancer de manière soft</strong> dans cette évolution du web qui sera incontournable dans un futur proche. A partir de ces manipulations de base, tout nouveau développement pourra être pensé avec les nouvelles balises et l'ajout de ARIA. <strong>L'équipe et le site seront ainsi prêts pour le futur</strong>.
<h1>Coût</h1>
Cela m'a pris <strong>une journée pleine</strong> (sur 3 jours), mais pour vous aider à quantifier l'effort chez vous voici quelques variables à prendre en compte :
<ul>
  <li>J'ai fait ces modifs sur un <a href="http://www.timeofmylife.com?utm_source=jpv">site de taille moyenne</a> (150 modules) mais avec très peu de modèles de pages différents (1 seul header, 1 seul footer, <strong>4 layouts</strong>)</li>
  <li>Le HTML a moins de 2 ans et la base de code était saine (oui je me jette des fleurs). Il n'y a eu que 6 dévelopeurs différents qui ont édité HTML et CSS.</li>
  <li>J'avais une <strong>connaissance très théoriques</strong> des rôles ARIA et de la nouvelle sémantique HTML5, j'ai donc passé beaucoup de temps en recherches dans les specs W3C et sur Google pour comprendre. Et pour tout ce qui touche à la sémantique, on peut passer des heures à tergiverser sur l'idée qu'en avaient les concepteurs.</li>
  <li>Je n'ai touché qu'au <strong>plus facile : le markup des layouts</strong>. Cela prendrait 1 semaine pour passer en revue les 150 modules et mettre à jour le markup de tous les widgets du site intelligemment (barre d'upload, listings, références aux dates ...)</li>
  <li>Rajoutez encore une journée pour jouer TOUS les scénarios de tests de tout le site sur la petite dizaine de browsers supportés : IE6 (hé oui), IE7, IE8, FF3.5, FF3.6, Chrome Mac et Chrome PC. Safari Mac et Opéra en bonus</li>
</ul>
<h1>Conclusion</h1>
Passer au markup HTML5 :
<ul>
  <li>le risque de ne plus être compatible tout browser est maîtrisé</li>
  <li>la transition est facile si l'équipe maîtrise bien le markup des layouts du site</li>
  <li><strong>pas de bénéfice immédiat connu en terme de référencement/accessibilité</strong></li>
  <li>tout futur projet incluant une nouvelle balise HTML5 bénéficiera de ce travail déjà accompli (<strong>et il y en aura</strong>)</li>
</ul>
