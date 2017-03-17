---
layout: post
title:  "Passer son blog Wordpress à la sémantique HTML5 et ARIA"
date:   2010-08-27 16:36:57 +0100
---

<abbr>HTML5</abbr> introduit de nouveaux éléments qui sont parfaits pour ajouter de la sémantique à un blog ou un journal. <abbr>ARIA</abbr> fait de même concernant l'accessibilité aussi étant donné la facilité permise par <span lang="en">Wordpress</span> pour modifier son <span lang="en">markup</span>, il serait dommage de se priver, même si vous êtes un débutant <span lang="en">Wordpress</span> comme moi.  Nous allons donc voir :
<ul>
  <li>l'utilisation des nouvelles balises <code>&lt;article&gt;, &lt;time&gt;, &lt;nav&gt; ...</code></li>
  <li>certains rôles <abbr>ARIA</abbr></li>
  <li>les améliorations des formulaires</li>
  <li>les ajouts <abbr>JS</abbr>/<abbr>CSS</abbr> à apporter</li>
  <li>les fichiers à modifier dans <span lang="en">wordpress</span></li>
</ul>
Le tout bien sur compatible sur tous les navigateurs, IE6 inclus.

Même si vous n'êtes pas un utilisateur Wordpress, les remarques sur la sémantique HTML5 et les rôles ARIA restent valables quel que soit le site.
<!--more-->
<h2>Travail préparatoire</h2>
Tout passage à <abbr>HTML5</abbr> doit passer par 2 ajouts à votre site, afin d'avoir un comportement normal des nouveaux éléments, que les anciens navigateurs ne pouvaient bien sur pas connaître. Je les reprend de mon précédent article sur <a href="http://braincracking.org/blog/2010/02/08/migrer-un-site-de-production-vers-un-markup-html5/">le passage à <abbr>HTML5</abbr></a>, la situation n'ayant pas évolué depuis.
<h3 lang="en">JavaScript</h3>
<abbr title="Internet Explorer" lang="en">IE</abbr> ne sait pas styler les éléments inconnus. Heureusement il existe une solution <abbr title="Javascript" lang="en">JS</abbr> reconnue appelée <a lang="en" href="http://remysharp.com/2009/01/07/html5-enabling-script/">HTML5 shim</a> pour contourner ce problème, qui consiste à créer dynamiquement au moins une fois ce nouvel élément. Ceci marche avec <abbr title="Internet Explorer 6">IE6</abbr> et supérieur. Notez que ce code doit être <strong>exécuté obligatoirement dans <code>&lt;HEAD&gt;</code></strong>. Plusieurs choix alors :
<ul>
  <li>pour des gains de performances, vous avez enlevé les inclusions de fichiers <abbr title="JavaScript">JS</abbr> du <code>&lt;HEAD&gt;</code>. Il vous faut alors éditer <em><strong>header.php</strong></em> pour rajouter ce script inline, avant <code>&lt;HEAD&gt;</code></li>
  <li>vous ou votre thème inclue déjà <span lang="en">modernizr</span>, ce script est déjà inclus dedans. Si <span lang="en">modernizr</span> est dans le <code>&lt;HEAD&gt;</code> vous n'avez rien à ajouter</li>
  <li>vous avez un fichier javascript global inclus dans le <code>&lt;HEAD&gt;</code>, c'est dans ce fichier qu'il faut rajouter ces lignes.</li>
</ul>
<pre><code>// inspiré de http://code.google.com/p/html5shim/source/browse/trunk/html5.js
// crée tous les tags HTML5 au moins une fois pour que IE sache les styler
// Commentaires et discussions, voir http://remysharp.com/2009/01/07/html5-enabling-script/
(function(){
  if(!/*@cc_on!@*/0)
    return;
  var e = 'abbr,article,aside,audio,bb,canvas,datagrid,datalist,details,dialog,eventsource,figure,footer,header,hgroup,mark,menu,meter,nav,output,progress,section,time,video'
    .split(',');
  for(var i=0, iTotal = e.length; i&lt;iTotal;i++) {
    document.createElement(e[i]);
  }
})();</code></pre>
<h3>CSS</h3>
Tous les éléments inconnus pour les navigateurs sont de type <code>display:inline</code> par défaut, ce qui peut rapidement casser un <span lang="en">design</span> : si vous passez d'une <code>&lt;DIV&gt;</code> à un <code>&lt;ARTICLE&gt;</code>, vous allez être obligé de rétablir <code>display:block</code> pour chaque élément. Il est préférable de s'inspirer de la <strong>stratégie des <span lang="en">CSS reset</span></strong> et de <strong>définir un comportement par défaut</strong> à toutes les balises <abbr>HTML5</abbr> possibles. Notez que l'on définit également les balises qui devraient être <span lang="en">inline</span>, ceci afin de prévenir le cas où un navigateur déciderait de mettre une des ces balises en <code>display:block</code>.  Dans le <strong>fichier style.css</strong>, après le <span lang="en">CSS reset</span> :
<pre><code>/* tags HTML5 qui se comportent comme des blocs */
article, aside, audio, canvas, datagrid, datalist, details, dialog, figure, footer, header, menu, nav, section, video { display: block; }
/* tags de type en ligne */
abbr, eventsource, mark, meter, time, progress, output, bb { display:inline; }</code></pre>
<h3 lang="en">doctype</h3>
Nous allons maintenant remplacer votre doctype par le désormais célèbre nouveau <span lang="en">doctype <abbr>HTML5</abbr></span>.  dans <strong><em>header.php</em></strong>, première ligne : <code>&lt;!DOCTYPE html&gt;</code>. Vérifiez que votre blog s'affiche toujours comme avant sur tous vos navigateurs cible, je n'ai jamais entendu que ce nouveau <span lang="en">doctype</span> posait problème. Vous êtes officiellement passé en <abbr>HTML5</abbr> :)
<h2>Les articles</h2>
<h3>La sémantique en théorie</h3>
Il y a 4 nouvelles balises toutes trouvées (en plus du <code>&lt;Hx&gt;</code>) qui vont enrichir sémantiquement nos articles. Ce qui change avec <abbr>HTML5</abbr>, c'est que non seulement la balise a un sens, mais ce sens peut varier en fonction des éléments parents :
<ul>
  <li><code>&lt;ARTICLE&gt;</code> : surpris ? Allons un peu plus loin : la <a href="http://dev.w3.org/html5/spec/Overview.html#the-article-element">définition du W3C</a> dit qu'elle représente une section isolable du site, qui peut s'autosuffire en dehors du contexte de la page (dans un lecteur RSS par exemple). Cette balise peut aussi être enfant d'un autre <code>&lt;ARTICLE&gt;</code>, comme dans l'interprétation qu'en a fait <a lang="en" href="http://html5doctor.com/the-article-element/">HTML5 Doctor</a>, auquel cas elle pourrait représenter les commentaires relatifs à un article. Nous n'allons pas le faire ici car ce point est parfois discuté et cela demande beaucoup plus de modifications dans wordpress car les styles de l'articles et des commentaires n'ont pas été prévus pour s'imbriquer.</li>
  <li><code>&lt;HEADER&gt;</code> : cette balise n'est pas exclusivement réservée à l'en-tête du site ! A l'intérieur d'un <code>&lt;ARTICLE&gt;</code>, elle sert à repérer les <strong>titres et méta-données de l'article</strong> comme la date de publication et le nom de l'auteur. Dans Wordpress, cela correspond exactement aux éléments avec les classes <code>.entry-title, .entry-header, .author, .published</code> et <code>.comment-count</code></li>
  <li><code>&lt;FOOTER&gt;</code> : elle non plus n'est pas réservée au pied de page du site. Placée dans un <code>&lt;ARTICLE&gt;</code>, elle marque les informations relatives à l'article mais non indispensables, telles que tags et catégories. Dans Wordpress, cela correspond aux classes <code>.entry-footer, .entry-categories</code> et <code>.entry-tags</code></li>
  <li><code>&lt;TIME&gt;</code> : elle sert à indiquer une date. Rajoutez y l'attribut <code>pubdate</code> pour signifier au parseur qu'il s'agit de la <strong>date de publication de l'<code>&lt;ARTICLE&gt;</code> parent</strong>. L'attribut <code>datetime</code> doit contenir la date à un <a href="http://dev.w3.org/html5/spec/text-level-semantics.html#the-time-element">format standardisé</a> qui peut varier de "2010-08-20" à "2010-08-20T20:00+09:00". PHP à la rescousse : <a href="http://fr.php.net/manual/fr/class.datetime.php#datetime.constants.types">il y a une constante</a> pour ce format qui s'appelle <strong><code>DATE_W3C</code></strong> et que vous pouvez passer à la fonction wordpress d'affichage de la date.</li>
</ul>
<h3>Le code final :</h3>
Voici le code original simplifié d'une installation Wordpress 3 avec le thème par défaut :
<pre><code>&lt;div class="hentry" id="post-777"&gt;
  &lt;h2 class="entry-title"&gt;Titre + lien&lt;/h2&gt;
  &lt;div class="entry-meta"&gt;
    &lt;span class="meta-prep meta-prep-author"&gt;Publié le&lt;/span&gt;
    &lt;span class="entry-date"&gt;Vendredi 20 août 2010&lt;/span&gt;
    &lt;span class="author vcard"&gt;Auteur&lt;/span&gt;
  &lt;/div&gt;&lt;!-- .entry-meta --&gt;
  &lt;div class="entry-content"&gt;
    corps de l article
  &lt;/div&gt;&lt;!-- .entry-content --&gt;
  &lt;div class="entry-utility entry-footer"&gt;
    catégories, tags, commentaires ...
  &lt;/div&gt;&lt;!-- .entry-utility --&gt;
&lt;/div&gt;&lt;!-- .hentry --&gt;</code></pre>
notez les très pratiques commentaires qui marquent les balises de fermeture. Nous allons donc :
<ul>
  <li><strong>remplacer</strong> la <code>DIV.<em>hentry</em></code> par une balise <code>ARTICLE</code>, en conservant les classes</li>
  <li><strong>remplacer</strong> la <code>DIV.<em>entry-utility</em></code> (ou <code>.<em>entry-footer</em></code> dans mon thème) par une balise <code>FOOTER</code>, en conservant les classes</li>
  <li><strong>entourer</strong> le &lt;<code>H2&gt;</code> et la <code>DIV.entry-meta</code> par une balise <code>HEADER</code></li>
  <li><strong>remplacer</strong> le <code>&lt;H2&gt;</code> par un <code>&lt;H1&gt;</code> : en <abbr>HTML5</abbr>, les <code>Hx</code> sont relatifs à l'élément parent pour lequel cela a un sens : <code>&lt;ARTICLE&gt;</code>, <code>&lt;SECTION&gt;</code> ou <code>BODY</code>. Contrairement à certaines recommandations <abbr title="Search Engine Optimization" lang="en">SEO</abbr>, il est donc possible d'avoir plusieurs H1 par page</li>
  <li>entourer ou <strong>remplacer</strong> le <code>SPAN.entry-date</code> par ce code : <code>&lt;time pubdate datetime="&lt;?php the_time( DATE_W3C ); ?&gt;"&gt; ... &lt;/time&gt;</code></li>
</ul>
pour obtenir ce markup :
<pre><code>&lt;<strong>article</strong> class="hentry" id="post-777"&gt;
  &lt;<strong>header</strong>&gt;
    &lt;<strong>h1</strong> class="entry-title"&gt;Titre + lien&lt;/<strong>h1</strong>&gt;
    &lt;div class="entry-meta"&gt;
      &lt;span class="meta-prep meta-prep-author"&gt;Publié le&lt;/span&gt;
      &lt;<strong>time pubdate datetime</strong>="2010-08-20T11:34:47+00:00"
          class="entry-date"&gt;
        Vendredi 20 août 2010
      &lt;/<strong>time</strong>&gt;
      &lt;span class="author vcard"&gt;Auteur&lt;/span&gt;
    &lt;/div&gt;&lt;!-- .entry-meta --&gt;
  &lt;/<strong>header</strong>&gt;
  &lt;div class="entry-content"&gt;
    corps de l article
  &lt;/div&gt;&lt;!-- .entry-content --&gt;
  &lt;<strong>footer</strong> class="entry-utility entry-footer"&gt;
    catégories, tags, commentaires ...
  &lt;/<strong>footer</strong>&gt;&lt;!-- .entry-utility --&gt;
&lt;/<strong>article</strong>&gt;&lt;!-- .hentry --&gt;</code></pre>
<h3>Que modifier ?</h3>
Il va falloir apporter ces modifications à beaucoup d'endroits, voici une liste non exhaustive (selon votre thème) :
<ul>
  <li>archive.php</li>
  <li>single.php</li>
  <li>page.php</li>
  <li>index.php</li>
  <li>category.php</li>
  <li>author.php</li>
  <li>tag.php</li>
  <li>search.php (à la différence que <code>&lt;ARTICLE&gt;</code> ne doit pas remplacer le <code>&lt;LI&gt;</code> mais être son premier et unique fils)</li>
  <li>date.php</li>
</ul>
Je déteste dupliquer le code, donc si vous êtes bons en Wordpress vous devriez peut être inclure un template unique d'article dans tous ces fichiers, car c'est quasiment le même markup à chaque fois. La partie difficile étant bien sur de coder le "quasiment" :)  Voilà, <strong>vous êtes passé à la sémantique HTML5 sur la partie importante du blog</strong> que sont les articles. Logiquement Google ou d'autres programmes devraient utiliser un jour ce standard, au même titre que les <span lang="en">microformats</span>, <span lang="en">microdata</span> ou <abbr>RDFa</abbr> pour comprendre plus facilement votre site et donc proposer des fonctionalités spécifiques à vos lecteurs (un affichage spécifique dans les résultats Google, une meilleure indexation, les navigateurs pourraient mettre en avant l'article ...).
<h2><span lang="en"><abbr>HTML5</abbr> Forms</span> et rôles ARIA dans votre layout</h2>
Pour le reste du site, nous allons ajouter plus de sémantique. Notez que les rôles ARIA sont déjà correctement définis dans le thème par défaut, mais ce n'est probablement pas celui que vous utilisez, donc je vais les répéter.
<h3>Formulaires</h3>
<span lang="en">Forms 2</span> a été renommé en <span lang="en"><abbr>HTML5</abbr></span> Forms et continue de définir de nouveaux types de champs ainsi que d'ajouter du comportemental sans JavaScript. Par défaut il n'y a pas de JavaScript qui régisse les formulaires de Wordpress (commentaires et recherche), on ne perd donc pas de fonctionnalité pour les anciens navigateurs : on en rajoute pour ceux qui supportent <span lang="en"><abbr>HTML5</abbr></span> Forms :) Si vous voulez ajouter facilement ces fonctionnalités pour pour d'autres navigateur sans taper de code <abbr title="javascript">JS</abbr>, vous pouvez partir sur <a href="http://code.google.com/p/webforms2/">la librairie <span lang="en">WebForms2</span></a>
<h4>Dans comments.php :</h4>
<ul>
  <li>la <code>&lt;DIV&gt;</code> avec l'<abbr title="identifiant">id</abbr> <code>#respond</code> devient une <code>&lt;SECTION&gt;</code> : comme une <code>&lt;DIV&gt;</code>, la <code>&lt;SECTION&gt;</code> n'a pas de valeur sémantique particulière, sinon qu'elle contient un titre, ce qui est ici le cas avec un <code>&lt;H3&gt;</code> comme enfant direct. Ce <code>&lt;H3&gt;</code> est d'ailleurs promu au rang de <code>&lt;H1&gt;</code> car c'est le titre le plus important de cette <code>&lt;SECTION&gt;</code></li>
  <li>dans le formulaire de commentaire, on <strong>modifie le type de 2 champs</strong> : celui de l'email et du site Web pour les passer très logiquement en <code>type="email"</code> et <code>type="url"</code>. Cela ne change rien pour la plupart des navigateurs mais outre la lisibilité du code, le seul bénéfice connu actuel est le changement de clavier sur <abbr title="Apple Mobile Operating System">iOS</abbr> ... Mais dans les mois qui viennent, d'autres clients pourraient l'utiliser pour une meilleure autocomplétion des champs ou pour signaler un type de données invalides. En attendant cela ne coûte rien</li>
  <li>rajouter l'attribut <code>required</code> ainsi que <code>aria-required="true"</code> aux champs obligatoires (au minimum l'email et le champs de commentaires). En code <span lang="en">Wordpress</span> cela donne :</li>
</ul>
<pre><code> &lt;input name="email" id="email" <strong>type="email"</strong> value="&lt;?php echo $comment_author_email; ?&gt;" tabindex="2" &lt;?php if ( $req ) echo "<strong>aria-required='true' required</strong>"; ?&gt; /&gt;</code></pre>

Les navigateurs supportant <code>required</code> (<abbr title="Firefox 4">FF4</abbr>, Opéra 9+, <abbr title="Chrome, Safari, Safaris mobiles">Webkits</abbr>) ne laisseront pas l'utilisateur valider le formulaire tant qu'il n'a pas rempli <strong>correctement</strong> les champs. Voir cette démo <a href="http://devfiles.myopera.com/articles/67/example.html">faite par Opéra</a>. 

Vous pouvez ensuite styler en CSS grâce aux pseudo-sélecteurs les champs non ou mal remplis :
<pre><code>input:invalid,
input:invalid + label {
  background-color : red;
}</code></pre>
Votre code final généré pour la zone de saisie de commentaires devrait ressembler à ceci :
<pre><code>&lt;<strong>section</strong> id="respond"&gt;
  &lt;h3 id="leave-a-reply"&gt;Laisser un commentaire&lt;/h3&gt;
  &lt;!--BEGIN #comment-form--&gt;
  &lt;form action="..."&gt;
    &lt;!--BEGIN #form-section-url--&gt;
    &lt;div class="form-section" id="form-section-url"&gt;
      &lt;input <strong>type="url" required="required" aria-required="true"</strong> value="http://braincracking.org/" id="url" name="url"&gt;
      &lt;label class="required" for="url"&gt;Site Web&lt;/label&gt;
    &lt;!--END #form-section-url--&gt;
    &lt;/div&gt;
  ../..
  &lt;!--END #comment-form--&gt;
  &lt;/form&gt;
&lt;!--END #respond--&gt;
&lt;/<strong>section</strong>&gt; </code></pre>
Notez que l'on pourrait passer de même tous les widgets avec <code>&lt;SECTION&gt;</code> et leur titre en <code>&lt;H1&gt;</code>.

<strong>Dans <strong>searchform.php</strong> :</strong>

Si votre thème n'a peut-être pas ce fichier ou équivalent, sachez que le formulaire de recherche par défaut se trouve dans <strong><em>general-template.php</em></strong>, dans la fonction <code>get_search_form</code>.  Nous allons rajouter ici 3 choses :
<ul>
  <li>le rôle <abbr>ARIA</abbr> sur le formulaire lui même : <code>&lt;form role="search" ...&gt;</code></li>
  <li>le type <span lang="en">"search"</span> sur le champs de formulaire : <code>&lt;input type="search" /&gt;</code>. Notez que sous les webkits sur Mac OS, votre champs apparaît stylé comme <span lang="en">Spotlight</span> : arrondi avec une croix d'annulation</li>
  <li>un <span lang="en">"placeholder"</span>, grand classique du Web : un texte par défaut disparaît lorsque l'on clique sur le champs et ré-apparaît si rien n'a été écrit : <code>&lt;input placeholder="Rechercher" /&gt;</code></li>
</ul>
ce qui se devrait se traduire par ce code final :
<pre><code>&lt;form <strong>role="search" </strong>action="http://braincracking.org/blog" method="get" class="searchform"&gt;
  &lt;input <strong>type="search" placeholder</strong>="Rechercher" name="s" class="search" /&gt;
  &lt;button type="submit" class="search-btn"&gt;Rechercher&lt;/button&gt;
&lt;!--END #searchform--&gt;
&lt;/form&gt; </code></pre>
<h3 lang="en">Layout</h3>
<h4>sidebar.php</h4>
Ceci affiche la ou les colonnes latérales, qui est donc un contenu sans forcément un rapport direct avec le contenu principal du site. <abbr>ARIA</abbr> et <abbr>HTML5</abbr> ont tout prévu pour cela : <code>role="complementary"</code> et balise <code>&lt;ASIDE&gt;</code>. Vous devez donc modifier une (ou plusieurs selon le thème) <code>&lt;DIV&gt;</code> parentes pour obtenir ceci :
<pre><code>
&lt;<strong>aside role="complementary"</strong> id="..." class="..."&gt;
  widget et contenus relatifs
&lt;/<strong>aside</strong>&gt;
</code></pre>
<h4>header.php</h4>
Nous avons déjà modifié ce fichier pour y rajouter le <span lang="en">doctype</span>, notre <abbr title="Javascript">JS</abbr> et le <abbr>CSS</abbr> afin de préparer le blog à <abbr>HTML5</abbr>. Passons maintenant aux ajouts sémantiques :
<ul>
  <li>la balise <code>&lt;HEADER&gt;</code>, pour marquer cette zone</li>
  <li>la balise <code>&lt;NAV&gt;</code> pour indiquer une zone de liens. Selon votre thème, cela peut être la liste des catégories ou des pages à part. Nous lui ajouterons le rôle <abbr>ARIA</abbr> <code>navigation</code> qui lui correspond exactement</li>
  <li>le rôle <abbr>ARIA</abbr> <code>banner</code> qui signale titre et logo</li>
  <li>si votre formulaire de recherche n'est pas déjà modifié, c'est le moment de rajouter <code>role="search"</code> sur le formulaire et <code>type="search" placeholder="Rechercher"</code> dans le champs de recherche</li>
</ul>
Les résultats varient grandement selon le thème, mais voici un exemple simplifié à partir du code du thème par défaut Wordpress 3 :
<pre><code>&lt;<strong>header</strong> class="header"&gt;
  &lt;<strong>nav</strong> id="access" <strong>role="navigation"</strong>&gt;
    &lt;ul class="nav"&gt;
      &lt;li class="current_page_item"&gt;&lt;a href="http://braincracking.org/blog/" title="Accueil"&gt;Accueil&lt;/a&gt;&lt;/li&gt;
      &lt;li class="page_item page-item-2"&gt;&lt;a href="http://braincracking.org/blog/a-propos/" title="A propos"&gt;A propos&lt;/a&gt;&lt;/li&gt;
    &lt;/ul&gt;
  &lt;/<strong>nav</strong>&gt;
  &lt;!-- END #access --&gt;
  &lt;div id="branding" <strong>role="banner"</strong>&gt;
    &lt;h1 id="blog_header"&gt;&lt;a href="http://braincracking.org/blog"&gt;BrainCracking&lt;/a&gt;&lt;/h1&gt;
  &lt;/div&gt; &lt;!-- #branding --&gt;
&lt;/<strong>header</strong>&gt;&lt;!-- #header --&gt;
</code></pre>
<h4>footer.php</h4>
Là aussi, sans grande surprise nous allons utiliser la balise <code>&lt;FOOTER&gt;</code> ainsi que le <code>role=contentinfo</code> qui sont tous deux faits pour baliser une zone contenant copyrights, liens vers mentions légales et autres textes <strong>relatifs au site, mais pas forcément au contenu de cette page</strong>. <strong>Attention</strong>, chez <span lang="en">Wordpress</span> cela ne correspond pas exactement à la <code>DIV id="footer"</code> du template par défaut, celle ci contenant également d'autres widgets.
Concrètement :
<pre><code>&lt;!--BEGIN #footer--&gt;
&lt;div id="footer"&gt;
  &lt;div id="footer-widget-area"&gt;
    code généré par les widgets
  &lt;/div&gt;&lt;!-- End #footer-widget-area --&gt;
  &lt;<strong>footer</strong> id="copyright" <strong>role="contentinfo"</strong>&gt;© 2009-2010
    &lt;nav role="navigation"&gt;
      Liens vers mentions légales, vie privée, qui somme nous ...
    &lt;/nav&gt;
  &lt;/<strong>footer</strong>&gt;
&lt;!--END #footer--&gt;
&lt;/div&gt;</code></pre>
Notez que pour ceux qui font des liens, on peut tout à fait rajouter la balise <code>&lt;NAV&gt;</code>.
<h2>Conclusion</h2>
Avec ce tutoriel et 2 à 3 heures de travail, vous pouvez passez assez facilement à la sémantique <abbr>HTML5</abbr> sur votre blog dès aujourd'hui. En fait en le codant, il devient même évident que les spécifications <strong><abbr>HTML5</abbr> et <abbr>ARIA</abbr> on été influencées par la sémantique <span lang="en">Wordpress</span></strong> tant la correspondance est facile.

Du côté de <abbr>HTML5</abbr>, il est probable que sur ce thème par défaut, <span lang="en">Wordpress</span> ait préféré faire l'impasse pour ne pas dépendre de Javascript ou de spécifications qui ne sont pas totalement fixées. Je pense personnellement que ces 2 limites ne sont pas un problème :
<ul>
  <li>les éléments que j'ai cité ici ont tous ont été largement discutés, et il est peu probable que le <abbr>W3C</abbr> revienne dessus. Si c'est cependant le cas les modifications à faire ne sont pas complexes</li>
  <li> j'estime la population avec <abbr title="Internet Explorer">IE</abbr> et javascript désactivé comme négligeable, au pire ils auront un design cassé mais sur un blog le contenu principal (l'article) restera exploitable.</li>
</ul>
Les rôles <abbr>ARIA</abbr> sont déjà inclus dans le thème par défaut de <span lang="en">Wordpress</span> 3, mais nombre de thèmes ont oublié de les répliquer. En créant ce blog, je cherchais des thèmes exploitant déjà la sémantique <abbr>HTML5</abbr>, mais ils sont peu nombreux donc le choix en graphismes est limité, et à l'heure d'écrire cet article aucun n'exploitait autant <abbr>ARIA</abbr> et <abbr>HTML5</abbr>.

Références, outils, remerciements :
<ul>
  <li>2 thèmes Wordpress tout faits (mais incomplets) : <a href="http://wordpress.org/extend/themes/toolbox">Toolbox</a> et <a href="http://wordpress.org/extend/themes/justcss">JustCSS</a></li>
  <li>les <a href="http://dev.w3.org/html5/spec/Overview.html#the-nav-element">specifications HTML5</a> et <a href="http://www.w3.org/TR/wai-aria/">ARIA</a></li>
  <li>la <a href="http://devfiles.myopera.com/articles/67/example.html">démo d'Opéra</a> sur les formulaires HTML5</li>
  <li><a href="http://html5doctor.com/article-archive/">HTML5 Doctor</a> pour l'ensemble de leur oeuvre d'explication des specifications</li>
  <li>Merci à <a href="http://blog.johanbleuzen.fr/">Johan Bleuzen</a> d'avoir relu et validé la partie Wordpress</li>
  <li>les validateurs <a href="http://validator.w3.org/check?uri=http%3A%2F%2Fbraincracking.org%2Fblog%2F">W3C</a> et <a href="http://html5.validator.nu/?doc=http://braincracking.org/blog&amp;showsource=yes">WHATWG</a> que passe maintenant <a href="http://braincracking.org">braincracking.org</a></li>
  <li>le plugin Wave pour <a href="http://wave.webaim.org/?lang=en">vérifier ARIA</a> sur son site</li>
  <li>le <a href="http://code.google.com/p/h5o/">bookmarklet h5o</a> pour analyser une page HTML5 selon l'algorithme W3C</li>
</ul>
