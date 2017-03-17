---
layout: post
title:  "HTML5 et le Web ouvert, pour les aigris et les cyniques"
date:   2011-01-29 16:04:21 +0100
---
<img src="http://www.w3.org/html/logo/downloads/HTML5_Logo_128.png" style="float:right" alt="" />
L'actualité récente autour de <abbr>HTML5</abbr> a fait réagir beaucoup de développeurs, ce qui à tout le moins permet d'avoir un instantané des opinions de la communauté Web. Elle vont de l'inutilité d‘avoir un <a href="http://www.w3.org/html/logo/">logo</a> aux affirmations du type “<abbr>HTML5</abbr> ne me concerne pas car je supporte <abbr>IE6</abbr>”. Ceux qui participent au débat sont soit enthousiastes soit <strong>aigris</strong> par la réalité de leur travail. Ceux qu‘on ne voit pas parler sont les <strong>blasés</strong>. Ce post d‘opinion, plutôt inhabituel sur ce blog technique, s‘adresse à ces 2 dernières catégories : <strong>pourquoi soutenir ou même s‘intéresser à l‘extension des technologies du Web en général, et au mouvement HTML5 en particulier</strong> ?
<!--more-->
<h2>Un peu de machiavélisme</h2>
Mettons de côté les principes philosophiques d'un web ouvert : soit vous y adhérez déjà par principe, soit ça vous en touche une sans faire bouger l'autre auquel cas il est bon de se rappeler ce que le Web ouvert peut vous apporter personnellement.
<h3>Ca n'appartient à personne</h3>
On peut remercier Apple pour nous avoir démontré avec Objective-C et les applications iOS tous les désavantages liés aux langages et processus de distributions fermés. C'est un succès commercial et les utilisateurs sont heureux, alors pourquoi est ce que les développeurs joueraient les rabats-joie ?
<ul>
  <li>Outils et langage imposés, parfois payants</li>
  <li>Une autorité arbitraire peut décider unilatéralement que votre application ne sera pas visible ou doit fermer</li>
  <li>La même autorité peut décider que votre business model ne lui convient pas (voir le cas de <a href="http://www.igeneration.fr/ipad/presse-sur-ios-apple-serre-la-vis-30482">la presse belge</a>)</li>
  <li>Si la communauté est petite, la formation sera chère. Les infos pour développer sur iOS sont monnaie courante, mais il est difficile de trouver des infos pour les autres plate-formes</li>
</ul>
<h3>Le multi-plateforme arrive</h3>
Ca m'ennuie de le reconnaître, mais le Web à partir du bureau devient l'exception plutôt que la règle, en particulier lorsque l'on regarde les statistiques mondiales. Depuis plusieurs années déjà il y a plus d'internautes sur mobile que sur bureau, et dans les pays en voie de développement c'est même l'unique usage du Web. Certes pour ces pays ça n'est pas du smartphone mais ça viendra. Dans l'occident, l'usage du Web depuis différents terminaux est maintenant courant et se fait selon la fonctionalité et les conditions pratiques : le mobile pendant les pauses, les éventuels trajets ou le soir au lit pour vérifier son facebook et ses mails, le PC au bureau et à la maison pour du e-commerce par exemple.

Hors dans l'ère pré-iPhone il fallait savoir coder en java ou pire dans des langages propriétaires pour faire des applications. Apple a imposé l'Objective-C, Android et d'autres sont restés sur java. Je n'ai rien contre java, mais pourquoi j'apprendrais un nouveau langage si j'ai la possibilité de capitaliser sur ce que je connais déjà ? Et quelles sociétés <a href="http://www.pagesjaunes.fr/plusdeservices/groupepagesjaunes/surmonmobile.do">hormis les pages jaunes</a> peuvent se payer de construire et maintenir la même applications sur une dizaine de plate-formes mobiles différentes ?

Il est déjà possible de construire des applications Web qui tournent très bien sur la majorité des webkits mobiles, et aujourd'hui ces sites-applications peuvent se passer d'une connexion au net (Offline, Web Storage), peuvent parfois avoir accès aux capacités matérielles du mobile (la géolocalisation) et peuvent être “installés”.

Le W3C est en train <a href="http://www.w3.org/2009/dap/#history">de spécifier</a> de quoi accéder à beaucoup plus de matériel (SMS, contacts, caméra, fichiers locaux ...), avec le concours de Nokia, Ericson ou Vodafone. Il y a donc un espoir pour qu'une application Web ait les mêmes possibilités qu'une application native. Pousser dans le sens des standards du Web, c'est donc aussi se donner la possibilité de pouvoir changer de branche ou d'utiliser la même équipe de développeurs pour faire du Web desktop, mobile et de l'applicatif mobile.

<h3>La concurrence, c'est bon, mangez en</h3>
Pourquoi les technos du Web devraient elles s‘attaquer au mobile voire au software ? Il existe déjà de très bons langages avec de très bons frameworks et de très bons développeurs dans ces domaines. Sauf que le manque de concurrence dans la technologie est très dangereuse, même si elle entraîne des difficultés pour les développeurs. Souvenez vous comme c‘était pratique pour les développeurs Web de voir <abbr title="Internet Explorer 6">IE6</abbr> gagner la première <span lang="en">Browser War</span>. En ces temps heureux pendant 1 ou 2 ans, beaucoup ont eu le sentiment qu'ils pouvaient ne tester qu'un seul navigateur et ignorer les autres (Safari Mac, Lynx ...). Et puis Firefox est arrivé, a grandi et a forcé Microsoft à s'engager, négocier puis bientôt respecter les standards du Web. Le projet Webkit a survécu et a facilité l'arrivée de Chrome et des Webkits sur mobiles. Opéra a testé un business model et a fourni de bonnes idées à tout le monde.

Bref la concurrence entre navigateurs a fait que l'expérience utilisateur en général s'est améliorée. La situation pour les développeurs Web s‘est compliquée d‘une certaine manière, puisqu‘il y a une bonne dizaine de <a title="Tester avec fiabilité les navigateurs" href="http://braincracking.org/?p=614">navigateurs à tester</a>. Cette complexité est compensée par la maturité grandissante des développeurs et des librairies. Que se serait il passé si Microsoft était resté à 90% de part de marché ces 10 dernières années ? L‘accès aux technos du Web serait sans doute plus complexe (essayez de lire <a title="documentation sur dropEffect" href="http://msdn.microsoft.com/en-us/library/ms533741(VS.85).aspx">la MSDN</a>), voire payante comme l‘est <a title="inscription développeur Apple" href="http://developer.apple.com/programs/which-program/">le développement iOS</a>.

Même en regardant le processus d‘écriture des specs, le fait d‘avoir 2 organisations a déjà eu des effets positifs, et on espère que l'émulation va continuer. Après le schisme de 2004, le WHATWG (originellement Apple, Mozilla et Opéra) avait travaillé sur ces spécifications :
<ul>
  <li>WebForms 2, qui améliore l'interface des formulaires natifs</li>
  <li>Web Storage, pensé comme un super cookie</li>
  <li>Web Socket et Server Sent Event qui permettent une communication plus efficace entre le client et le serveur</li>
</ul>
En résumé : ils se sont concentrés sur ce qui pouvait être utile aux développeurs d'application Web dans l'immédiat et se sont lancés dans le même temps dans l'implémentation dans les navigateurs. On n'est peut être pas dans la révolution technologique (les formulaires sont émulables en Javascript, le reste avec Flash), mais cette marche forcée du HTML vers les “applications Web” est très positive, car elle nous donne plus de pouvoir, et certaine API comme “Web Workers” sont très innovantes.

<h2>Un peu de pragmatisme</h2>
En attendant le mobile, voyons déjà ce qu'on peut faire des nouveautés sur navigateur desktop, avec notamment les apports de HTML5. Ce qui compte pour un développeur au moment de faire ses choix techniques, c‘est l‘état d‘implémentation de chaque API JavaScript par les navigateurs ou, pour la partie sémantique, de savoir quels sont les <span lang="en">user-agent</span> qui savent exploiter le nouveau balisage.

<h3>Sémantique </h3>
Côté sémantique, la politique des lecteurs d‘écran est probablement la même que celle du moteur Google : s‘adapter à ce que font la majorité des sites (<a href="http://www.google.com/support/forum/p/Webmasters/thread?tid=2d4592cbb613e42c&hl=en">source</a> : un community manager de Google). Si les webmasters se mettent à utiliser <code>nav</code> ou <code>article</code>, ils suivront. Les navigateurs seront sans doute un peu plus pro-actifs pour proposer des fonctionalités tirant parti de l‘analyse des balises (un <a href="http://www.apple.com/fr/safari/whats-new.html#reader">lecteur d‘article</a> comme Safari par exemple). Dans les 2 cas, suivre les balises standards (et éviter d‘en inventer) c‘est se donner une <strong>longueur d‘avance sur les concurrents</strong> pour un coût de développement négligeable.
<h3>les APIs</h3>
Côté APIs (Web apps selon le <abbr>WHATWG</abbr>) il ne faut pas prendre <abbr>HTML5</abbr> comme un bloc mais partir de la fonctionnalité souhaitée et regarder quelles sont les <abbr>APIs</abbr> qui peuvent vous être utiles. La plupart du temps, la fonctionnalité est simulable en JavaScript ou en Flash, et un <a href="https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-browser-Polyfills">certain nombre</a> de petites librairies (ou polyfills, si l‘usage <a href="http://remysharp.com/2010/10/08/what-is-a-polyfill/">du terme</a> prend) permettent d‘y accéder sans trop de problèmes. L‘intérêt fonctionnel est de proposer une meilleure expérience à la moitié de vos utilisateurs qui ont des navigateurs au niveau, et une fonctionnalité qui marche pour les autres. 
Au niveau du code, les spécifications n‘étant pas réservées aux implémentation navigateur, l‘API est généralement également suivie par ces <span lang="en">polyfills</span>. Lors d‘un changement de projet, de librairie, ou lorsque vous passerez à l‘utilisation exclusive de la fonctionnalité en natif, vous retrouverez les mêmes signature de fonction et la même structure de code.

J‘ai fait une conférence entière à Paris Web 2010 sur ce sujet, je vous invite à regarder <a href="http://braincracking.org/?p=597">les slides</a> pour vous forger une opinion sur le déploiement possible en production.

<h2>Mais que faire John ?</h2>
Au niveau individuel, pour aider à tout cela, c‘est autant une affaire d‘expertise technique que de savoir vendre en interne ces technologies. Formez vous, faîtes votre veille techno, participez aux réunions produit ou faîtes des démos en interne utilisant vos nouvelles connaissances : ça égaie le travail et vous donnera l‘impression salutaire d‘être un peu plus qu‘un cracheur de code.
Un engagement un peu plus fort pourrait aussi consister à signaler des bugs aux navigateurs, à ceux qui font des librairies open-source ou des outils vous permettant de transposer vos compétences Web sur mobile ou desktop.
Enfin vous pouvez également contribuer au code open source ou participer aux discussions W3C ou WHATWG, ils sont réellement dans l‘attente des avis des développeurs Web pour progresser!

Au niveau de la communauté, je pense qu‘une initiative du W3 comme ce logo reste positive, tant que la discussion avec la communauté est ouverte (le logo ne sera officiel que début 2011) et que le message n‘est pas brouillé (une phrase heureusement retirée incluait CSS3, WOFF et SVG dans HTML5). Beaucoup de développeurs seront heureux d‘avoir une bannière à laquelle se rallier et tant pis pour ceux qui accusent le W3C de vouloir faire du marketing. Demandez à la fondation Mozilla si le marketing sert ou dessert la cause du libre.
A propos de brouiller le message, je ne pense pas que la décision du WHATWG d‘abandonner HTML5 et de parler de “living standard“ et de “web application” soit la bonne. Concernant le terme HTML5, on pourra se limiter à la spec du W3C, préciser quelles APIs on y inclue ou bien en parler à tort et à travers : comme “AJAX”, tout est fonction de votre interlocuteur.

<h2>Conclusion</h2>
Pourquoi pousser dans le sens du Web ouvert notamment avec HTML5 et les APIs associées ?
<ul>
  <li>pour jouer le jeu de la concurrence et placer le tryptique Javascript/HTML/CSS  au backend, dans des applications de bureau ou sur mobile
  <li>pour les sociétés du Web, la possibilité de ré-utiliser les compétences et les outils en interne pour attaquer d'autres domaines
  <li>pour les développeurs, plus de possibilités de carrière, plus de nouveautés, bref que du bonheur
  <li>l‘accès à de nouvelles fonctionnalité est plus facile, ce qui permet d‘avoir une longueur d‘avance sur les concurrents
  <li>les standards même en construction facilitent la réutilisabilité et la maintenance du code
</ul>
