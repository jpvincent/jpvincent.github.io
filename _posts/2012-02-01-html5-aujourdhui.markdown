---
layout: post
title:  "HTML5, aujourd’hui"
date:   2012-02-01 16:04:21 +0100
---
L'article suivant est la retranscription de l'article que j'ai écrit pour <a href="http://phpsolmag.org/">le magazine PHP Solutions</a>, dans le numéro hors série de 2011 consacré à HTML5. Le monde compliqué du droit d'auteur veut que ce texte ne m'appartienne pas, c'est donc avec leur permission express que j'en fait bénéficier mes lecteurs (oui toi). Toute reproduction est donc interdite.

Cet article se veut une introduction générale à HTML5, notamment pour les développeurs PHP qui sont généralement les mieux placés pour prendre au sérieux ce qu'il se passe au frontend. Si vous avez lu <a href="http://livre-html5.com/">mon livre sur HTML5</a> ou que vous suivez déjà régulièrement l'actualité, je doute que vous y appreniez grand chose. Il explique :
<ul>
  <li>l'origine de la spécification</li>
  <li>comment les sites d'aujourd'hui peuvent en bénéficier</li>
  <li>le futur des applications Web</li>
  <li>les rares synergies entre HTML5 et PHP</li>
</ul>
<h2>Historique</h2>
<h3>Une définition variable</h3>
Le terme même « HTML5 » couvre plusieurs définitions, et nous allons voir que le W3C a surpris tout le monde dans son choix final. Avant que le terme HTML5 n'apparaisse, HTML 4.01 était stabilisé et le W3C planchait sur XHTML 2 car pour eux la syntaxe stricte de XML allait permettre à tous les navigateurs d'être enfin d'accord sur les règles de <em>parsing</em>des pages.
<!--more-->
[caption id="attachment_924" align="aligncenter" width="101" caption="Le logo volontairement énigmatique du WHATWG"]<img class="size-full wp-image-924" title="logo_whatwg" src="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2012/01/html5_intro_figure_01.png" alt="" width="101" height="101" />[/caption]

Nous sommes en 2004 et des représentants d'Apple, Opéra et Mozilla, pensant que XHTML 2 est une fausse bonne idée, décident de former un groupe dissident, dénommé non sans ironie WHATWG (What Working Group?). Ils travaillent de leur côté à leur vision du Web : celle d'une plateforme de développement universelle qui pourrait battre les applications natives, aux langages généralement propriétaires et fermés, en étant exécutable sur tous les systèmes et toutes les machines. Leur première spécification est « WebForms 2 », une évolution majeur des formulaires HTML que nous allons aborder plus loin. Vont venir s'ajouter beaucoup de petites spécifications regroupées sous le terme « Web Application 1 ».

En 2006 le terme HTML5 apparaît enfin : c'est la version approuvée par le W3C des spécifications travaillées par le WHATWG. Le Web a eu peur un instant que le W3C soit tellement discrédité par cette affaire que ses spécifications, déjà mal comprises des développeurs Web par la faute des implémentations non conformes de certains navigateurs, ne fassent plus autorité. Désormais cette réconciliation donne un nouveau rythme de travail : les navigateurs (hors Microsoft) testent des implémentations de fonctionnalité à toute vitesse, s'accordent sur les APIs finales et les formalisent, pendant que le W3C garde son rôle de gardien des clés et continue son travail de proposition de spécifications.

En 2009, le W3C accuse le coup du rejet par la communauté des développeurs Web de XHTML 2 et l'abandonne : l'inconvénient majeur de la syntaxe XML était qu'elle ne serait jamais rétrocompatible avec les navigateurs déjà en circulation. A cette époque déjà on a du mal à savoir ce que désigne vraiment HTML5 : des spécifications d'API sont réparties entre trois spécifications (« Web Application », « HTML5 » par le WHATWG et « HTML5 » par le W3C), et certaines devenues trop grosses ou pratiquement finalisées sont carrément sorties de HTML5 pour vivre par elle même.

[caption id="attachment_925" align="aligncenter" width="256" caption="Le logo officiel de HTML5"]<img class="size-full wp-image-925" title="logo_html5" src="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2012/01/html5_intro_figure_02.png" alt="" width="256" height="256" />[/caption]

En 2011 le terme HTML5 est clairement un terme générique : le W3C déclare officiellement qu'il désigne désormais plus une idée de l'évolution du Web qu'une spécification en particulier, et lui donne par la même occasion un <strong>logo</strong>. Le WHATWG de soncôté bannit ce terme pour renommer ses spécifications « living standard », ce que l'on pourrait traduire par : « ne cherchez pas la stabilité chez nous, allez plutôt voir le W3C pour ça ».

&nbsp;

Le HTML5 dont nous allons parler ici désigne trois choses :
<ul>
  <li>l'évolution de HTML 4, avec une syntaxe assouplie, une nouvelle sémantique et l'introduction des <strong>microdata</strong>,</li>
  <li>de nouvelles interactions     utilisateur, avec les nouveaux formulaires, la géolocalisation et     le multimédia</li>
  <li>l'arrivée des applications Web    avec des API JavaScript comme <strong>Websocket</strong> pour la    communication temps réel, la gestion du hors ligne, le stockage     d'informations côté client et <strong>XMLHTTPRequest 2</strong></li>
</ul>
Nous avons choisi ces sujets car ce sont que vous pourrez utiliser en production le plus immédiatement, en assurant une compatibilité avec tous les navigateurs depuis Internet Explorer 6.
<h3>Quand HTML5 sera-t-il prêt ?</h3>
Réponse courte : maintenant, et depuis plusieurs années déjà. Lisez la réponse longue pour comprendre de quoi HTML5 est fait.
<h4>2022 ?</h4>
A l'époque où le WHATWG commença à travailler sur la spécification en 2004, Ian Hickson, l'éditeur de la spécification, avait annoncé une date qui est restée longtemps dans la mémoire des journalistes : 2022. Il faut bien comprendre qu'il faisait référence aux critères du W3C : des spécifications complètes, finalisées, acceptées, et surtout avec au moins deux implémentations complètement interopérables. Cela semble raisonnable à première vue, mais concrètement ce niveau de qualité repousse très loin les dates de finalisation. Par exemple après 10 ans d'existence et d'utilisation en production, CSS 2.1 vient à peine d'atteindre le niveau final des spécifications : <em>« Recomendation »</em>. Et malgré cela il n'existe pas encore deux navigateurs implémentant <strong>toutes</strong> les fonctionnalités, et sûrement pas de manière identique. HTML 4.01 lui par contre a bien atteint le niveau <em>« Recomendation »</em> il y a 10 ans, et même si il n'y a pas de test unitaire pour le prouver on peut considérer que plusieurs navigateurs interprètent le même code de la même manière. Mais cela ne change pourtant rien au quotidien des développeurs Web qui doivent assurer le support sur les navigateurs qui ont ignoré ou mal interprété les spécifications.
<h4>2011 ?</h4>
La mauvaise nouvelle, c'est que l'on vient de voir avec CSS2.1 et HTML 4.01 que l'état d'une spécification du W3C ne signifiait pas grand chose pour le développeur web. La bonne nouvelle, c'est que pour HTML5 non plus l'état de la spécification n'a pas beaucoup d'importance ! Ce qui compte vraiment c'est :
<ul>
  <li>la stabilité des implémentations  navigateur,</li>
  <li>la possibilité d'émuler la  fonctionnalité sur les anciens navigateurs,</li>
  <li>à défaut, l'acceptation d'une   page non améliorée pour les anciens navigateurs</li>
</ul>
Autre grande nouvelle : HTML5 n'est pas fait d'un bloc. La spécification se découpe en de nombreuses fonctionnalités séparées, n'ayant parfois pas de lien entre elles, et chaque navigateur avance sur ses sujets favoris. Concrètement il y a aujourd'hui une bonne dizaine de nouvelles fonctionnalités qui sont à la fois suffisamment stables, suffisamment répandues et en même temps simulables sur les anciens navigateurs, ce sont celles que nous allons couvrir rapidement aujourd'hui et qui nous font dire que HTML5 est déjà là.
<h4>1999 ?</h4>
Poussons le raisonnement jusqu'au bout : en découpant HTML5 en spécifications séparées, on arrive à des conclusions étranges ; Internet Explorer 5.5, sorti en 1999 est compatible avec certaines fonctionnalités HTML5 comme le <em>drag and drop</em>. Évidemment cela s'est fait dans l'autre sens, c'est à dire qu'un des premiers travaux du WHATWG a été de coucher sur le papier des fonctionnalités comme le <em>drag and drop</em> qui n'avaient jamais été écrites. Cet exemple est là pour illustrer que la question du « quand » n'a pas vraiment de sens.

<strong>Conclusion</strong> : des fonctionnalités majeures associées à HTML5 sont disponibles dès maintenant pour des environnements de production, y compris avec la contrainte du support d'anciens navigateurs.
<h3>La philosophie</h3>
HTML5 est né dans le conflit et avec la volonté d'être pragmatique pour une adoption rapide. Cela lui confère un certain nombre de principes :
<ul>
  <li><strong>Un assouplissement des règles</strong>.     En réaction aux travaux sur XHTML 2 qui impliquait la     syntaxe stricte de XML dans les pages HTML, HTML5 est clairement    permissif et enlève les attributs inutiles, les <code>doctype</code>s     compliqués, enlève l'obligation d'utiliser des <em title="&quot;">quotes </em> et autorise les variations de casse.</li>
  <li><strong>L'unification des règles de     <em>parsing</em></strong>. Les navigateurs respectant HTML5 implémentent le     même algorithme d'interprétation du texte HTML pour construire le     DOM, c'est à dire la représentation finale du document accessible     en CSS et en JavaScript.</li>
  <li><strong>La rétro-compatibilité</strong>. Au     contraire de ce sur quoi s'engageait le W3C à l'époque, les     nouvelles fonctionnalités sont introduites avec des syntaxes    permettant au parc de navigateurs actuels (en partant de Internet     Explorer 6) de ne pas « casser » les pages.</li>
  <li><strong>Formalisation de l'existant</strong>. Le    WHATWG a commencé par écrire les spécifications de    fonctionnalités jamais documentées auparavant et pourtant     implémentées au prix de longues heures de <em>reverse     engineering </em> par tous les navigateurs. Cela va de    fonctionnalités complètes comme le <em>drag     and drop </em>à des attributs DOM comme <code>.innerHTML</code>.</li>
</ul>
On le voit, HTML5 a été pensé avec beaucoup de pragmatisme pour une adoption immédiate. Cela est du à la volonté des fabricants de navigateurs de faire avancer le Web. Une fois ces bases saines établies, ils ont maintenant les coudées franches pour rattraper les applications natives en terme de fonctionnalités.
<h2>Après HTML4</h2>
Avant l'animation 2D, la vidéo ou des piles de JavaScript pour faire du logiciel, HTML5, c'est d'abord du code HTML ! La sémantique est revue, la syntaxe évolue et l'accessibilité est prise en compte.
<h3>Nouvelle syntaxe</h3>
La modification la plus visible est bien sur l'introduction du nouveau <code>doctype</code> :
<pre><code>&lt;!doctype html&gt;</code></pre>
Cela simplifie les choses et on est même en droit de se demander à quoi servaient les <code>doctype</code>s 4.01 <em>strict </em>ou <em>transitional</em>. Pour la petite histoire, Internet Explorer 6 n'utilisait cette instruction que pour savoir dans quel mode de rendu CSS il fallait se mettre (<em>quirksmode</em> ou mode standard).

Autre simplification : la déclaration de l'encodage de la page.
<pre><code>&lt;meta charset="utf-8"&gt;</code></pre>
Certains attributs comme <code>type</code> dont l'absence est universellement bien interprétée peuvent maintenant disparaître :
<pre><code>&lt;script&gt; alert('Iceberg droit devant'); &lt;/script&gt;
&lt;style&gt; #titanic { float: none; } &lt;/style&gt;
</code></pre>
<h3>nouvelle sémantique</h3>
Plusieurs balises disparaissent carrément car elles ont été accusées de mélanger le fond et la forme. Citons celles qui étaient le plus couramment utilisé : <code>font</code>, <code>center</code>, <code>strike</code>, <code>u</code>, <code>big</code>, <code>frame</code>, <code>frameset</code> ou encore <code>acronym</code> et <code>applet </code>ainsi qu'une petite dizaine d'autres.

Les attributs suivants sont également frappés d'obsolescence :

<code>longdesc</code> et <code>name</code> sur la balise <code>img</code>, <code>scope</code> sur la balise <code>td</code>, <code>classid</code> sur la balise <code>object</code>, <code>target</code> sur la balise <code>link</code> ainsi qu'une dizaine d'autres.

Ont été remplacés pour laisser la place à CSS :

<code>align</code>, <code>bgcolor</code>, <code>background</code>, <code>border</code>, <code>cellpadding</code> et <code>cellspacing</code> sur <code>table</code>, <code>height</code> sur <code>td</code> et <code>th</code>, <code>marginheight</code> et <code>scrolling</code> sur <code>iframe</code> ainsi qu'une vingtaine d'autres.

D'autres changent subtilement de valeur sémantique. Il n'y aurait pas assez de place dans ces colonnes pour ré-expliquer le sens de chacune d'entre elles, aussi prenons la plus célèbre : <code>strong</code>. Avant, elle était censé désigner un mot qui devait se dire avec emphase (sur lequel on mettrait l'accent à l'oral). Cette fonction étant maintenant dévolue à la balise <code>em</code>, <code>strong</code> veut maintenant dire « mot important », ce qui colle avec l'usage effectif qui en était fait aujourd'hui, notamment en référencement.

Enfin et surtout, de nouvelles balises apparaissent, les plus importantes étant les balises dite <strong>de section </strong>: <code>section</code>, <code>article</code>, <code>aside</code>, <code>header</code>, <code>footer</code>. Elle sont utiles pour préciser un peu les zones de votre site que vous entouriez auparavant d'une <code>div</code>, faute de mieux. Vous pouvez utiliser le chemin de décision suivant pour déterminer la nature de votre balise :
<ul>
  <li>le contenu est il indépendant du  site et peut il être exporté sans ce contexte ? Par exemple un  article de blog ou une fiche produit. Dans ce cas là, la balise   <code>article</code> est toute  indiquée</li>
  <li>le contenu mérite au moins un   titre et regroupe des informations sur la même thématique ? La  balise section est faite pour ça et vous permet de marquer par exemple des  onglets, des modules de news, une zone de commentaires …</li>
  <li><code>aside</code>,   <code>footer</code> et <code>header</code> peuvent être utilisées pour marquer des zones particulières dans   les sections que nous venons de voir : un groupe de titres pour   <code>header</code>, des  métadonnées (<em>copyright</em>, auteur, référence…) pour le <code>footer</code> et des informations complémentaires pour <code>aside</code> (autres articles, produits similaires...)</li>
  <li>La <code>div</code> reste d'actualité pour tous les besoins non couverts</li>
</ul>
Citons également:
<ul>
  <li>la balise <code>nav</code> qui marque les liens de navigation de la page (et non d'une <code>section</code>,  <code>footer</code> étant plus  indiqué pour cela),</li>
  <li><code>time</code> qui permet de marquer de manière formelle une date (exemple PHP :   <code>&lt;time datetime="&lt;?=   date( DATE_W3C); ?&gt;"&gt;Maintenant !&lt;/time&gt;</code>).</li>
  <li><code>figure</code> et <code>figcaption</code> pour les illustrations et leur description (regardez le code source de cet article)</li>
</ul>
On le voit, ces nouvelles balises peuvent être mises à profit principalement pour les sites à contenu éditorial (journaux, réseaux sociaux...) voire les sites de e-commerce. Le problème que vous rencontrerez sera que dans les anciennes version d'Internet Explorer vous ne pourrez pas appliquer de style à ces éléments (mettre un <code>display:block</code> sur les balises sectionnantes par exemple). Pour cela il suffit d'un court fix Javascript que vous pourrez trouver sur le net en tapant <em>HTML5 shiv</em>.
<h3>microdata</h3>
Microdata, présenté plus en détails ailleurs dans ce numéro (NDR : le numéro en question étant payant, je vous propose d'aller directement sur <a href="http://schema.org/docs/gs.html#microdata_why">le site édité par Google</a>), se propose de remplacer RDFa et microformat. Si vous ne connaissez pas ces projets, il s'agit simplement de marquer sa page d'une manière beaucoup plus précise et libre que ce que la spécification HTML5 peut prévoir. Si par exemple votre site liste des événements, des recettes de cuisine, des personnes ou des avis vous allez pouvoir indiquer très précisément dans votre page où se trouvent les informations intéressantes. Concrètement vous pouvez définir vos propres vocabulaires ou vous rendre sur <a href="http://schema.org">http://schema.org</a> (maintenu par Google, Bing et Yahoo!) et choisir le vocabulaire adapté à ce que vous voulez décrire. Puis à l'aide de 3 attributs (<code>itemscope</code>, <code>itemtype</code> et <code>itemprop</code>), vous marquez les éléments importants. Par exemple nous allons marquer précisément une page présentant un profil. On commence par marquer la zone avec <code>itemscope</code> :
<pre><code>&lt;div itemscope&gt;../..
&lt;/div&gt;</code></pre>
On déclare ensuite le vocabulaire auquel on va se référer grâce à <code>itemtype</code> et une <abbr>URL</abbr>. Ici on va prendre le format <code>hCard</code>, probablement le plus répandu des formats. Notez que l'url pointe sur le site microformats : microformat et microdata sont compatibles car définir un vocabulaire ne consiste jamais qu'à lister des clés et leurs valeurs possibles.

<pre><code>&lt;div itemscope itemtype="http://microformats.org/profile/hcard"&gt;
../..
&lt;/div&gt;
</code></pre>
Enfin avec <code>itemprop</code> vous marquez chaque information de votre markup pour indiquer où se trouve la valeur de chaque partie du vocabulaire.
<figure>
<pre><code>&lt;div itemscope itemtype="http://microformats.org/profile/hcard"&gt;
Vous pouvez contacter &lt;span itemprop="given-name"&gt;JP Vincent&lt;/span&gt; 
pour toute question HTML5 sur 
&lt;a itemprop="email" href="mailto:jp@braincracking.org"&gt;ce mail&lt;/a&gt;
../..
&lt;/div&gt;
</code></pre>
<figcaption>marquage d'une page de contact avec le standard microdata et un vocabulaire microformat</figcaption>
</figure>

Un programme tel que ceux de Google ou certains plugins navigateur vont lire le markup et associer le nom avec le mail.&nbsp;

<h3>ARIA</h3>
ARIA est une spécification axée sur l'accessibilité et permet de proposer des interfaces riches  qui restent accessibles aux utilisateurs de technologie d'assistance. A son niveau le plus basique vous pouvez au moins indiquer les zones principales grâce à l'attribut <code>role</code>: les liens de navigations avec <code>role="navigation"</code>, le contenu principal avec <code>role="main"</code>, les mentions légales avec <code>role="contentinfo"</code>, le titre ou le logo avec <code>role="banner"</code>&hellip;

A un niveau un peu plus évolué, vous pouvez marquer des interactions plus complexes et néanmoins répandues comme des barres de progression (<code>role="progressbar"</code>), des curseurs (<code>role="scrollbar"</code>, <code>role="slider"</code>), des menus, des boîtes de dialogue (<code>role="dialog"</code>). C'est très utile aux technologies d'assistance pour comprendre des zones de l'écran qui sont plus dynamiques que jamais : imaginez que vous ayez un chat en ligne en Web, comme dans l'exemple suivant.

<figure>
<pre><code>&lt;ol&gt;
  &lt;li&gt;KevinDu75: Kikoo ^__^
&lt;/ol&gt;
</code></pre>
<figcaption>Markup d'une chatroom</figcaption>
</figure>
Sans indication particulière, un lecteur d'écran va lire une seule fois ce code, alors qu'il y a sûrement une routine JavaScript qui met à jour régulièrement cette zone. Avec ARIA, il suffit de rajouter le rôle correspondant (d'après la spécification, c'est <code>log</code>) à cette fonctionnalité classique, et les lecteurs se mettront à lire automatiquement tout dernier élément ajouté au DOM

<figure>
<pre><code>&lt;ol role="log"&gt;
  &lt;li&gt;KevinDu75: Kikoo ^__^
  &lt;li&gt;DarkBlackWarrior666 : LOL !!!!!
&lt;/ol&gt;
</code></pre>
<figcaption>Marquage ARIA de la zone de chat</figcaption>
</figure>

La bonne nouvelle c'est que certaines librairies JavaScript y compris certains plugins jQuery rajoutent automatiquement des attributs ARIA lorsqu'ils vous proposent des widgets un peu compliqués. C'est à vous de faire attention à ce que l'option soit bien proposée pour garantir une meilleure accessibilité de vos interfaces.
<h2>Interaction utilisateur</h2>
<h3>Les nouveaux formulaires</h3>
Les formulaires HTML ont été largement dépoussiérés.
Auparavant on avait surtout des   champs de type texte ou mot de passe. HTML5 définit une série de  nouveaux types qui se rencontrent couramment sur les formulaires :  <code>url</code>, <code>mail</code>, <code>search</code>, <code>date</code>, <code>number</code>&helip; Quel est l'intérêt ? Il offre aux navigateurs et  aux technologies d'assistance des points d'attache pour améliorer   l'expérience utilisateur : changement de clavier virtuel sur les  mobiles pour faciliter la saisie, apparition d'interfaces   spécialisées pour sélectionner facilement des dates ou des  chiffres …
[caption id="attachment_926" align="aligncenter" width="238" caption="les adaptations des navigateurs aux champs date et tel"]<img class="size-full" src="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2012/01/html5_intro_figure_04.png" alt="" /><img class="size-full wp-image-926" title="type_date" src="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2012/01/html5_intro_figure_03.png" alt="" width="238" height="208" />[/caption]
La  validation  des données a été standardisée. Jusqu'ici les développeurs empilaient une bonne   couche de Javascript par dessus chaque formulaire. Ici simplement   avec du HTML vous pouvez demander au navigateur d'exécuter un   certain nombre de règles de validation et d'afficher des  indications à l'utilisateur pour remplir sans se tromper son  formulaire.
La simplification de certains   comportements jusqu'ici scriptés : mettre le <em>caret </em>automatiquement sur certains formulaires avec l'attribut  <code>autofocus</code>, mettre un   texte de suggestion par défaut dans les champs texte qui apparaît   ou disparaît avec <code>placeholder</code>.

Pour le développeur Web coincé entre PHP et JavaScript il y a ici une vraie opportunité : la validation des données doit bien sur rester côté serveur, et il y a maintenant une manière très simple de réutiliser le même code entre le client et le serveur. Prenons une classe de validation en PHP simpliste que l'on voudrait utiliser, comme ci-après.

<figure>
<pre><code>&lt;?php
class validation {
  // d'abord on met public l'expression rationnelle pour l'utiliser plus tard
  public static $regMail="^[w-]+(?:.[w-]+)*@(?:[w-]+.)+[a-zA-Z]{2,7}$";
  public static $regNickname="#^[a-zA-Z0-9àáâãäåèéêëìíîïòóôõöùúûüýýÿçñ _-.']{2,50}$#i";
  // fonctions appellées par le serveur
  static function isValidMail( $email ) {
    return preg_match("/".self::$regMail."/",$email));
  }
  static function isValidNickname( $nickname ) {
    return preg_match("/".self::$regNickname ."/",$nickname));
  }
}
</pre></code>
<figcaption>une classe statique PHP de validation des données</figcaption>
</figure>
On voit dans le listing 4 que l'on a isolé les expressions régulières utilisées pour valider le formulaire dans des variables statiques, et nous allons en fait les réutiliser dans le HTML. En HTML5 nous avons l'attribut <code>pattern</code> pour donner des expressions régulières au navigateur qu'il va exécuter pour vérifier l'entrée utilisateur, comme ci-après.

<figure>
<pre><code>&lt;form&gt;
  &lt;label &gt;Votre mail : &lt;input type="email" pattern="&lt;?= validation:: $regMail ?&gt;" required /&gt;
  &lt;/label&gt;
  &lt;label &gt;Votre surnom : &lt;input type="text" pattern="&lt;?= validation:: $regNickname ?"&gt; required /&gt;
  &lt;/label&gt;
&lt;/form&gt;
</pre></code>
<figcaption>Transmission des expressions régulières de validation au navigateur</figcaption>
</figure>

[caption id="attachment_928" align="aligncenter" width="234" caption="interface de validation de formulaire, sous Chrome"]<a href="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2012/01/html5_intro_figure_05.png"><img class="size-full wp-image-928" title="html5_intro_figure_05" src="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2012/01/html5_intro_figure_05.png" alt="" width="234" height="93" /></a>[/caption]

Sans avoir tapé de CSS ou de javascript, certains navigateurs afficheront l'équivalent de la figure 5 à l'utilisateur. Vous trouverez quelques librairies Javascript qui permettent d'émuler la même chose sur tous les navigateurs.
<h3>vidéo native</h3>
Les balises <code>audio</code> et <code>video</code> ont été calquées sur le modèle de la balise <code>img</code> : permettre très simplement d'insérer du multimédia dans une  page Web. La promesse est bien sur de se passer de Flash pour lire de la vidéo, et d'offrir à vos utilisateurs plus de fluidité (pas de démarrage de Flash ou d'un autre plugin), une meilleure utilisation du CPU sur les environnements que Flash n'adressait pas (Mac OS, Linux, les mobiles) et une interface par défaut qu'ils connaissent déjà. Pour le développeur, pouvoir utiliser JavaScript et CSS pour contrôler une vidéo est très naturel et promet donc de belles intégrations.

Tempérons cela par plusieurs points :
<ul>
  <li>les implémentations actuelles   des navigateurs sont encore un cran en dessous de ce que permet   Flash. Notoirement mettre une interface en CSS en vrai plein écran  n'est encore possible sur aucun navigateur, il faut pour cela   attendre une autre spécification : la <em>Fullscreen API</em>, en cours de  test chez Firefox et Chrome</li>
  <li>la guerre des codecs continue de  faire rage : le standard de l'industrie vidéo est h.264 et ils  n'ont aucune envie d'adapter tous les outils de leur chaîne de  production pour le Web. De l'autre côté WebM, pourtant offert à   la communauté par Google, ne peut être décodé que sur Firefox,  Chrome et Opéra et on attend depuis un an qu'Adobe fasse de même  pour que les sites Web n'aient pas à stocker en deux encodages  leurs vidéos.</li>
  <li>le W3C n'a pas l'intention  d'adresser le marché particulier du <em>streaming </em>payant ou des  vidéos protégées, car c'est contraire à la philosophie  d'ouverture du Web. Cela signifie que pour ces marchés Flash  restera très longtemps la seule technologie utilisable.</li>
</ul>
En bref, les éléments multimédia sont déjà indispensables sur des environnements où Flash n'est pas présent, et vous pouvez déjà essayer de l'intégrer en complément de Flash sur vos sites Web, mais il est peu probable que ce soit votre seul lecteur.
<h3>Géolocalisation</h3>
La géolocalisation permet à une page Web de savoir (après autorisation de l'utilisateur) où celui-ci se trouve. Au contraire de la géolocalisation par IP qui peut au mieux vous donner le nom de la ville de votre point d'entrée sur le Net (en ADSL, cela peut être une erreur de 5 kms), les implémentations navigateur se basent sur des données matérielles plus précises :
<ul>
  <li>sur bureau, l'adresse MAC de la   carte Wifi est envoyée sur les systèmes de Google (pour Firefox,  Opéra et Chrome) ou de Microsoft, qui ont scanné à grande échelle   les zones les plus densément peuplées, et qui y ont associé des   coordonnées GPS. En gros, si la « streetview » de   Google Maps ou de Bing est disponible pour votre quartier, vous   serez repéré précisément (de l'ordre de la centaine de mètres)</li>
  <li>sur mobile, c'est le GPS embarqué   qui est directement interrogé. A défaut de GPS, c'est la  triangulation par antenne relai.</li>
</ul>
Dans tous les cas si aucune donnée n'est disponible, c'est au final l'adresse IP qui est utilisée, avec l'imprécision que l'on sait.

Voici un exemple d'implémentation qui essaye d'utiliser l'API du navigateur, mais qui à défaut va utiliser les solutions traditionnelles de détection par IP :

<figure>
<pre><code>// test du support navigateur
if( navigator.geolocation ) {
  // appel asynchrone à l'API
  navigator.geolocation.getCurrentPosition( function( position ) {
  // l'objet position un sous-objet contient toutes les informations
    console.log( position.coords.latitude,  position.coords.longitude );
  });
} else {
  // retour à une solution moins précise
  var oXHR = new XMLHTTPRequest();
  // appel d'un script PHP qui va récupérer l'IP et trouver la ville
  oXHR.open('GET', '/getlocation.php');
  // on peut supposer que le script répond avec des coordonnées
  oXHR.onload = function( e ) {
    console.log( e.currentTarget.responseText );
  };
  oXHR.send();
}
</pre></code>
<figcaption>Géolocalisation native, avec méthode de secours traditionnelle</figcaption>
</figure>


Les solutions de correspondance IP / ville sont généralement payantes, même si il en existe des gratuites. Elle se présentent sous la forme de script et de base de données à installer chez soi et à exploiter en PHP. Dans l'un ou l'autre cas vous n'êtes bien sur absolument pas certain de la véracité des données. La géolocalisation des navigateurs permet à tout le moins d'avoir une solution IP gratuite, et dans la plupart des cas d'obtenir des informations beaucoup plus précises.

L'API prévoit aussi de demander des coordonnées plus précises, de pouvoir surveiller les déplacements et de fournir des données de vitesse, d'altitude, d'orientation par rapport au nord. Certaines implémentations peuvent également y rajouter la ville ou l'adresse !

Le fait que géolocalisation existe, et marche depuis déjà deux ans est majeur pour les technologies du Web, car c'est la première fois qu'une page Web peut interagir avec le matériel de l'utilisateur, privilège réservé jusqu'ici aux applications natives.
<h2>L'ère des applications Web</h2>
<h3>Websocket et Server Sent Events</h3>
WebSocket implémente ce que les développeurs Web devaient jusque là simuler : de la communication temps réel bi-directionnelle entre le client et le serveur. Le protocole HTTP montre en effet ses limites lorsqu'il s'agit d'attendre des informations venant du serveur ou d'écrire un jeu vidéo avec un flux d'informations continu client / serveur. Chaque envoi d'information est encapsulé par parfois plusieurs ko de headers tandis qu'avec websocket il ne s'agit plus que de quelques octect superflus. De plus la connection reste ouverte en permanence ce qui réduit très fortement la latence, chose indispensable pour des applications aussi réactives que des jeux. Il y a un article dans ce numéro qui trait plus en profondeur de WebSocket

Le problème de WebSocket pour le développeur PHP, c'est que cela remet en cause beaucoup de choses côté serveur : les serveurs WebSocket d'aujourd'hui sont écrits dans des langages tels que JavaScript (sur serveur Node.js) qui sont taillés pour exécuter du code de manière asynchrone et qui supportent en théorie des milliers de connexions ouvertes simultanément. Il existe bien une implémentation WebSocket en PHP, mais on ne sait pas vraiment quelle charge elle peut tenir en production, l'empreinte mémoire et la consommation CPU d'un processus PHP étant généralement assez gourmande : après tout pour tenir un socket ouvert, il n'y a pas d'autre moyen qu'une boucle infinie entrecoupée de <code>sleep()</code> et PHP n'a pas été pensé dans ce sens !

A côté de WebSocket, il y a la spécification « Server Sent Events », qui elle ne permet d'avoir que la communication du serveur au client, ce qui peut suffire à la plupart des applications : surveiller un flux twitter, un cours boursier, avoir un dashboard etc … Pour envoyer des informations au serveur, il reste bien sur <code>XMLHTTPRequest</code>, qui est un peu plus lent que WebSocket et qui crée un nouveau thread sur le serveur. En fait SSE remplace efficacement et formalise la technique dite « forever iframe » où PHP et JavaScript gardaient la connexion ouverte via une <code>iframe</code> où du HTML était régulièrement injecté, même lorsqu'aucun événement ne devait être envoyé.

L'avantage de Server Sent Events, c'est qu'elle permet de réutiliser pratiquement le même code côté PHP, avec quelques ajouts que nous allons commenter, tout en optimisant le trafic réseau et en formalisant les échanges de données. Regardons d'abord comment on faisait en PHP pour maintenir la connexion ouverte par une iframe :

<figure>
<pre><code>&lt;?php
// désactivation des caches navigateurs et proxys
header('Cache-Control: no-cache');
// boucle infinie
while(true) {
  // méthode métier de récupération des données
  $data = my_get_data();
  // encodage en JSON pour interprétation directe en JavaScript
  $data = json_encode( $data );
  // on fait exécuter une fonction JavaScript qui contient nos données
  print '&lt;script&gt; fonction_rappel('. $data .'); &lt;/script&gt;';
  // même si les données sont vides, la connexion est maintenue car du HTML est envoyé
  flush();
  // on laisse respirer le CPU avant la prochaine itération
  sleep(1);
}
</pre></code>
<figcaption>technique de <em>forever iframe</em>, côté PHP</figcaption>
</figure>


Voici maintenant les lignes que nous devrions ajouter pour renvoyer des informations à un navigateur supportant SSE :


<figure>
<pre><code>&lt;?php
// détection du support par le navigateur
<strong>$hasSSE = ($_SERVER['HTTP_ACCEPT'] === 'text/event-stream');</strong>
// désactivation des caches navigateurs et proxys
header('Cache-Control: no-cache');
// on informe le navigateur qu'on va lui répondre au format SSE
if( $hasSSE === true)
  <strong>header('Content-Type: text/event-stream');</strong>
// boucle infinie
while(true) {
  // méthode métier de récupération des données
  $data = my_get_data();
  // encodage en JSON pour interprétation directe en JavaScript
  $data = json_encode( $data );
  if( $hasSSE === true)
    // le standard prévoit que les données arrivent après la chaîne data:
    <strong>print 'data: '. $data;</strong>
  else
    // on fait exécuter une fonction JavaScript qui contient nos données
    print '&lt;script&gt; fonction_rappel('. $data .'); &lt;/script&gt;';

  // le standard sépare les données sur plusieurs lignes par un double retour chariot
  <strong>print PHP_EOL.PHP_EOL;</strong>
  flush();
  sleep(1);
}
</pre></code>
<figcaption>Ajout du code pour supporter Server Sent Event</figcaption>
</figure>

On le voit, on envoie simplement un entête supplémentaire, puis chaque ligne de donnée est préfixée par <span style="text-decoration: underline;">data:</span> (avec ou sans espace), et les blocs de données sont séparées par un <strong>double retour chariot</strong>.

Côté client il suffit d'instancier l'objet <code>EventSource</code> avec l'URL du serveur et d'écouter les événements, comme ici :

<figure>
<pre><code>// on se branche sur le PHP
var connexion = new EventSource('/get_data.php');
// programmation événementielle
connexion.onmessage = function( e ) {
  // la propriété data de l'événement contient directement la chaîne JSON qu'on a créé en PHP
  console.log( e.data );
};
</pre></code>
<figcaption>Utilisation côté client de Server Sent Event</figcaption>
</figure>

L'API va beaucoup plus loin et en plus de la reconnexion automatique gère également :
<ul>
  <li>les tickets, généralement un  marqueur de temps ou un numéro de ligne qui peut servir de repère   à PHP lorsque le navigateur se reconnecte,</li>
  <li>des événements nommés, qui  permettent de créer des catégories de dialogue,</li>
  <li>des événements comme les  erreurs, l'ouverture et la fermeture de la connexion</li>
</ul>
Il existe d'hors et déjà des librairies JavaScript permettant de simuler l'API Server Sent Events sur tous les navigateurs, et donc de réutiliser votre code PHP.
<h3>Offline</h3>
De plus en plus d'utilisateurs sont en situation de mobilité lorsqu'ils consultent des sites Web, qu'ils soient sur smartphone ou qu'ils utilisent un laptop dans un café avec une connexion Wifi défectueuse ou inexistante. Le grand avantage qu'ont les applications dites natives, c'est que l'interface se lance immédiatement, qu'il y ait du réseau ou pas. Hors certaines applications Web comme les jeux <em>casual</em> n'ont parfois pas du tout besoin de connexion, tandis que d'autres comme les webmails n'ont besoin d'une connexion que pour aller chercher de nouvelles données.

La gestion de l'offline vise à mettre les applications Web au même niveau que les applications natives en :
<ul>
  <li>ne faisant télécharger qu'une fois pour   toute l'interface et le cœur applicatif,</li>
  <li>donnant de  quoi gérer les versions de l'application,</li>
  <li>permettant de réagir et détecter les situations de déconnexion.</li>
</ul>
Concrètement vous allez référencer un fichier <code>.manifest</code> dans votre HTML :
<pre><code>&lt;!doctype html&gt;
&lt;html manifest="mapage.manifest"&gt;
…
</code></pre>
Puis vous allez servir ce fichier (avec un entête type mime "<code>text/manifest</code>") qui référence plusieurs choses :
<ul>
  <li>les dépendance (CSS, JS,  images..) à garder pour une utilisation <em>offline</em> (section   <code>CACHE</code>) ,</li>
  <li>les URLs qui doivent absolument   accéder au réseau (section <code>NETWORK</code>),</li>
  <li>les pages par défaut lorsque le   réseau ne répond pas (section <code>FALLBACK</code>)</li>
</ul>
L'exemple suivant serait une application Web qui n'aurait que deux dépendances, qui irait chercher le reste sur le réseau, et qui affiche la page <code>offline.html</code> pour toute autre requête sans réseau :

<figure>
<pre><code>CACHE MANIFEST
# v1.0

CACHE
css/styles.css
js/main.js

FALLBACK:
/ offline.html

NETWORK:
*
</pre></code>
<figcaption>fichier <code>offline.manifest</code></figcaption>
</figure>

La déclaration est simple, c'est dans la programmation métier de votre application que les choses se compliquent. Par défaut les navigateurs vont donc afficher la version qu'ils ont en local, puis aller vérifier si le manifeste de cache a changé ne serait ce que d'un octet (c'est pour cela qu'on met généralement en commentaire le numéro de version de l'application). Si c'est le cas, il va télécharger de manière transparente toutes les ressources, qu'elles aient changé ou non, et envoyer un événement Javascript pour dire qu'il a mis à jour le cœur applicatif. Il appartient au développeur de gérer ces états de transition entre deux versions de la même application. Il peut être aidé en cela par une série d'événement JavaScript et de fonctions situées dans l'objet global <code>applicationCache</code>, et qui lui donneront l'état de la mise à jour et un contrôle sur celle ci.

Notons par ailleurs que l'on a accès à la propriété <code>window.navigator.onLine</code> (valeur booléenne), censée donner l'état de la connexion, et aux événements <code>ononline </code>et <code>onoffline</code>, qui avertissent du changement d'état. Malheureusement seul Internet Explorer, et encore à partir de la version 8, implémente de manière utile cette propriété et ces événements.
<h3>XHR 2</h3>
<code>XMLHTTPRequest</code> a été la base technique à ce que l'on a appelé le « Web 2.0 » et permet de faire des requêtes asynchrones au serveur pour récupérer ou envoyer des informations. Dans HTML5, la version 2 déjà implémentée dans la plupart des navigateurs donne plus de contrôle et d'informations sur les requêtes :
<ul>
  <li>indications de progression : si   la taille totale de votre envoi de fichier ou de votre  téléchargement est connue, vous avez accès à l’évènement <code>progress</code>, et aux  propriétés <code>total</code> et  <code>loaded</code>. Vous pouvez  maintenant afficher à votre utilisateur de vraies barres de   progression.</li>
  <li>création de formulaires à la  volée : l'API <code>FormData</code> permet de créer ou de modifier des formulaires à la volée et  le navigateur les formate en mode <code>POST</code>.  Ceci permet de gérer facilement l'envoi de fichiers envoyés en  <em>drag and drop</em> depuis le bureau ou en sélection multiple, le  tout sans rechargement de page</li>
</ul>
En bref, là aussi cette API modernise les applications Web en faisant évoluer un des outils principaux des développeurs.
<h3>Local Storage</h3>
Que l'on gère un site Web classique avec des formulaires sur plusieurs pages ou une application Web lourde type google Docs, on a besoin de stocker les informations que l'utilisateur génère, sans forcément passer par le réseau pour stocker ces informations sur le serveur. On pouvait jusqu'ici utiliser les cookies, mais on était limité en taille (4ko tout compris) et on pouvait dégrader sérieusement les performances : sur la plupart des sites les cookies sont envoyés avec chaque requête HTTP et une page Web moyenne est composée de 80 requêtes HTTP, soit un trafic réseau énorme et surtout inutile. La solution préconisée par PHP est d'ailleurs de ne stocker dans le cookie qu'un court identifiant de session, pour retrouver côté serveur les données.

« Web Storage » permet soit de faire tampon, soit d'éviter carrément le trafic réseau ou la gestion de session en offrant aux développeurs un espace pour stocker au moins 5 Mo de données. Utilisez l'API <code>sessionStorage</code> pour un stockage temporaire (vidé à la fermeture de la page) ou <code>localStorage</code> pour un stockage permanent. Dans les deux cas vous pouvez stocker des paires clé/valeur très simplement :

<figure>
<pre><code>localStorage.setItem('locale', 'fr_FR');
console.log( localStorage.getItem('locale') );
localStorage.removeItem( 'locale');
</pre></code>
<figcaption>définir, récupérer et supprimer une valeur côté client</figcaption>
</figure>

Seules les chaînes de caractère sont acceptées, donc si vous avez besoin de stocker des données plus structurées, utilisez simplement JSON. Si vous avez besoin d'utiliser cette fonctionnalité sur tous les navigateurs, sachez que vous pouvez utiliser Flash ou des techniques propriétaires de chez Microsoft pour y stocker jusqu'à 100ko de données. Il existe déjà plusieurs petites librairies qui font cette abstraction pour vous.
<h2>Le futur</h2>
Les navigateurs veulent clairement concurrencer les applications dites natives, voici ce sur quoi ils planchent.

<strong>La 3D </strong>: avec WebGL, qui permet donc de faire des jeux ou des environnements immersifs le tout en 3D. L'interaction avec le DOM permet même de réutiliser d'autres composants : on peut par exemple utiliser en texture un élément <code>&lt;video&gt;</code> ou le contenu d'une <code>&lt;div&gt;</code> sur des éléments de décor. Certaines sociétés éditrices de jeux vidéos console ou PC travaillent déjà sur cette techno pour trouver de nouveaux canaux de distribution et toucher plus de monde, en pariant sur une plus large diffusion des navigateurs et sur l'accélération matérielle généralisée d'ici 2 à 3 ans.

<strong>Le multithread </strong>: la spécification <code>Web Workers</code> permet d'exécuter du javascript dans un processus différent du navigateur, et même d'exploiter un second processeur. Cela permet d'avoir des applications avec des calculs lourds telles de la manipulation de photo ou de vidéo, des jeux vidéo ou des applications scientifiques.

<strong>L'accès au matériel</strong> : la géolocalisation permet déjà d'accéder au GPS des mobiles, <code>getUserMedia</code> permet d'accéder à la caméra, et <code>DeviceOrientation</code> pointe sur les informations du gyroscope. Il y a déjà des implémentations sur Firefox, Chrome et certains mobiles.

<strong>Le peer to peer </strong>: des expérimentations ont été menées et permettent de faire dialoguer directement deux navigateurs de deux machines différentes sans passer par un serveur pour maintenir la connexion. Oui le prochain Skype pourrait être écrit en JavaScript.

<strong>L'accès au bureau </strong>: plein écran, notifications, contacts … il y a plusieurs expérimentations autour de ces fonctionnalités qui vont permettre aux pages, pardon aux applications Web de s'intégrer complètement à votre OS, que vous soyez sur mobile ou sur PC classique.

Et je n'ai listé ici que les API qui ont au moins une implémentation, fusse-t-elle expérimentale. L'idée vous l'aurez compris est de donner les pleins pouvoirs aux « sites » Web qui sont aujourd'hui les interfaces privilégiées par le grand public. Cela ne se fait pas sans soulever des questions sur la sécurité, mais le W3C veille et chaque API est prévue pour passer par une demande de permission à l'utilisateur.

<h2>Conclusion</h2>
HTML5 et les navigateurs apportent toutes ces nouvelles capacités qui rivalisent avec les applications natives. Avec la puissance de diffusion du Web qui est de fait multi-plateforme, HTML5 termine le travail commencé par PHP et Linux car les logiciels de demain pourront être écrits avec des langages non propriétaires. C'est une grande victoire pour le monde du libre.

Références :
<h3>Sur Internet</h3>
<ul>
  <li><em>http://braincracking.org/</em> – [FR] le site de l'auteur : veille technologique sur HTML5,   Javascript et les performances Web,</li>
  <li><em>http://livre-html5.com/</em> – [FR] le site du livre HTML5, avec les démos,</li>
  <li><a href="http://www.html5-css3.fr/"><em>http://html5-css3.fr/</em></a> – [FR] formation et tutoriaux à HTML5 et CSS3,</li>
  <li><a href="http://alsacreations.com/"><em>http://alsacreations.com/</em></a> – [FR] tutoriaux, forums et actualité des technos frontend,</li>
  <li><em><a href="http://html5doctor.com/">http://html5doctor.com/</a> </em>– [EN] site d'information sur la sémantique de HTML5,</li>
  <li>http://www.html5rocks.com/<em> </em>– [EN] maintenu par Google : tutoriaux et présentation  générale avec démos de HTML5 au sens large.</li>
</ul>
