---
layout: post
title:  "Combien coûte un service tiers gratuit ?"
date:   2016-12-21 16:04:21 +0100
---

Vous auriez tort de croire que la réponse est à moitié dans le titre : n’importe quel script ou service externe que vous rajoutez à votre site va vous coûter quelque chose, qu’il soit gratuit ou pas.

> Cet article et [le suivant sur les pubs]({% post_url 2016-12-21-les-manques-a-gagner-de-la-publicite-pour-les-editeurs %}) sont une transcription d’une conférence donnée à Blend Web Mix 2016. [Cliquez ici si vous êtes plus audio que texte](https://www.youtube.com/watch?v=7tbTm1Jo0gE).


# De quels coûts parle-t-on ?
Ma spécialité étant la performance frontend, j'ai dû m’attaquer au sujet pour des raisons techniques qui m’ont amené à constater que les dégradations en qualité et en performance perçue se traduisaient par un certain nombre de coûts cachés:
<ol>
  <li>des <strong>pertes sèches d’activité</strong></li>
  <li>un manque à gagner dû au ralentissement du chargement ou au gel de l’interface</li>
  <li>une ouverture pour le piratage industriel</li>
  <li>une image de marque entamée.</li>
</ol>
Qui sont les tiers ici ?
<ul>
  <li>Analytics, tracking</li>
  <li>Boutons de partage, réseaux sociaux</li>
  <li>Tests A/B</li>
  <li>Widgets divers : cartes, commentaires, chats…</li>
</ul>
Et étant donnéées leurs spécificités, citons-les à part : <a href="http://braincracking.org/?p=1256">les <strong>publicités</strong>, auxquelles j’ai dédié un article moins technique</a>.

Passons en revue les problèmes et solutions.

<h1>1. Les 20 secondes page blanche</h1>
Dans le plus spectaculaire des cas, vous pourriez avoir un site qui n’affiche rien pendant plusieurs secondes.

[caption id="attachment_1199" align="alignnone" width="1468"]<a href="http://braincracking.org/wp-content/uploads/2016/12/indisponible.png"><img class="size-full wp-image-1199" alt="20 secondes de page blanche, puis affichage du site" src="http://braincracking.org/wp-content/uploads/2016/12/indisponible.png" width="1468" height="578" /></a> Veuillez patienter, nous allons prendre votre temps.[/caption]

Pour rajouter un peu de sel sur la plaie, disons que c’est le grand patron qui découvre cela un samedi après-midi de vacances. On va alors se lancer dans une partie de Cluedo pour savoir qui a tué le site et les premiers suspects sont les administrateurs système.
Heureusement pour eux, ce sont également fréquemment les seuls à être suffisamment bien équipés en monitoring pour démontrer que les serveurs de la boîte vont bien. Dans le meilleur des cas ils savent mener une analyse rapide du réseau, vue d'un navigateur.

[caption id="attachment_1200" align="alignnone" width="1500"]<a href="http://braincracking.org/wp-content/uploads/2016/12/réseau.png"><img class="size-full wp-image-1200" alt="outil chrome dev tools montrant une requête qui ne répond pas" src="http://braincracking.org/wp-content/uploads/2016/12/réseau.png" width="1500" height="253" /></a> la requête coupable est une longue barre grise, sur-lignée en rouge (forcément)[/caption]

Après analyse, c'est un petit script hébergé sur un serveur qui ne répond pas qui bloque la page. On passe donc en revue les suspect suivants :
<ul>
  <li>les développeurs frontend qui n’ont fait que leur devoir en implémentant la solution demandée par le métier,</li>
  <li>les services marketing et commercial qui ont légitimement besoin de cette solution de [adserving | test A/B | widget | tracker]</li>
  <li>le service tiers, qui avait donné comme garantie au moment de l’intégration : "On est sur un CDN".</li>
</ul>
<h2>L’enfer c’est les autres</h2>
Il est très tentant d’incriminer le tiers à la moindre occasion, et parfois c'est justifié, comme cet incident il y a quelques années où certains tunnels de paiement se mettaient à afficher des erreurs de sécurité.

[caption id="attachment_1202" align="alignnone" width="663"]<a href="http://braincracking.org/wp-content/uploads/2016/12/jquery.png"><img class="size-full wp-image-1202" alt="alerte de sécurité SSL de chrome dûe au serveur jquery" src="http://braincracking.org/wp-content/uploads/2016/12/jquery.png" width="663" height="514" /></a> une alerte de sécurité du plus bel effet au moment de payer[/caption]

Les pages touchées étaient celles en HTTPS qui incluaient jQuery depuis le CDN <code>code.jquery.com</code> … dont l’administrateur avait oublié cette après midi là de renouveler le certificat de sécurité.
Mais il y a plein d’autres raisons pour lesquelles un serveur tiers peut ne pas répondre comme :
<ul>
  <li>des attaques mettant à mal votre prestataire ou ses propres prestataires. Dans le cas de <a href="http://dyn.com/blog/dyn-analysis-summary-of-friday-october-21-attack/">l’attaque DDoS contre le DNS Dyn</a>, Twitter n’était plus visible de certains endroits de la planète … tant pis pour les pages incluant les widgets.</li>
  <li>des erreurs réseau de l‘hyper-espace comme la redirection temporaire du domaine Google vers un site de prévention du terrorisme du gouvernement … <a href="http://www.bortzmeyer.org/google-detourne-par-orange.html">Uniquement pour les clients Orange un 17 octobre</a> o_O</li>
  <li>les wifis gratuits publics (hôtels, aéroports…) qui redirigent certaines requêtes pour insérer dans les pages leurs propres régies publicitaires</li>
  <li>les firewalls d'états (Chine en tête) ou les proxys d’entreprise qui ont une opinion tranchée sur l’utilisateur Facebook ou Youtube et qui <a href="https://en.wikipedia.org/wiki/Websites_blocked_in_mainland_China">bloquent les domaines</a> : dommage pour les widgets et analytics</li>
</ul>
Bref : il y a 1001 raisons pour qu'un serveur tiers plante ou ralentisse, il faut donc prévoir son intégration avec cet état de fait.
<h2>Savoir intégrer les tiers</h2>
Généralement l’intégrateur reçoit simplement comme instruction un mail disant :
<blockquote>Insérez ce script dans le <code>&lt;head&gt;</code></blockquote>
J’exagère un peu, parfois on a droit à un document Word formaté proprement (mais qui dit la même chose).
Il arrive aussi que les développeurs décident d’eux même de faire une bêtise :
<ul>
  <li>inclusion de librairies JS / CSS directement depuis leurs CDNs respectifs, comme le préconisent les documentations,</li>
  <li>récupération des fichiers de polices depuis les serveurs Google Fonts ou autre.</li>
</ul>
Dans ce dernier cas, il peut y avoir l’excuse de la licence qui n’autorise pas forcément le rapatriement du fichier. Du coup vous faites subir à vos utilisateurs le comportement par défaut de tous les navigateurs (sauf <abbr title="Internet Explorer et Edge">IE</abbr>) qui n’affichent pas le texte tant que le fichier de police n’est pas présent.
<h3>Je vois pas le problème</h3>
Pour visualiser les problèmes d’intégration, et surtout leurs conséquences pour l’utilisateur, je ne saurais trop vous recommander les outils suivants :
<ul>
  <li>L’indispensable service en ligne <a href="http://www.webpagetest.org/">WebPagetest</a> : déplier "Advanced Settings", onglet "<abbr title="Single Point Of Failure">SPOF</abbr>" et y mettre les noms d’hôtes à faire planter.</li>
  <li>L’extension Chrome <a href="https://chrome.google.com/webstore/detail/spof-o-matic/plikhggfbplemddobondkeogomgoodeg">SPOF-o-Matic</a> qui permet d’alerter, de tester et même de programmer un test comparatif sur WebPagetest.</li>
  <li>En ligne de commande (pour l’intégration continue par exemple), je vous ai fait un petit script par dessus <a href="https://gist.github.com/jpvincent/494117fc2806a5d14806cc96a6354cef">SPOF-check</a></li>
</ul>
[caption id="attachment_1207" align="alignnone" width="663"]<a href="http://braincracking.org/wp-content/uploads/2016/12/spof-o-matic.png"><img class="size-full wp-image-1207" alt="capture de l’interface SPOF-o-Matic" src="http://braincracking.org/wp-content/uploads/2016/12/spof-o-matic.png" width="663" height="365" /></a> l’extension SPOF-o-Matic débusquant les potentiels fauteurs de troubles[/caption]
<h3>Solutions</h3>
Pour ne jamais dépendre d’un tiers, il n’y a qu’environ 2 stratégies :
<ol>
  <li>Les ressources <strong>critiques</strong> sont à rapatrier sur vos propres serveurs. Le CSS bien sûr, les dépendances <abbr title="JavaScript">JS</abbr>, les polices si vous en avez le droit.</li>
  <li>Tout ce qui reste hébergé ailleurs doit passer en asynchrone : trackers, analytics, widgets, <a href="https://www.zachleat.com/web/comprehensive-webfonts/">les polices</a>…</li>
</ol>
Il y a certains cas spéciaux comme l’inclusion des tests A/B qui sont à la fois dynamiques et critiques pour le rendu de la page, étant donné qu’ils essayent de le modifier. Mais si vous demandez poliment une mécanique de "timeout" ou un rapatriement périodique du fichier, vous n’aurez plus de soucis.
<h1>2. Les retards à l’allumage</h1>
Une fois traité le problème impressionnant mais rare de la page blanche dûe à un tiers, reste le problème sournois du ralentissement quotidien, moins visible mais bien plus coûteux. Même avec des inclusions asynchrones on peut se retrouver dans ces situations

[caption id="attachment_1208" align="alignnone" width="916"]<a href="http://braincracking.org/wp-content/uploads/2016/12/ralentissement-fonctionnalite.png"><img class="size-full wp-image-1208" alt="un moteur de recherche de voyages mis en attente" src="http://braincracking.org/wp-content/uploads/2016/12/ralentissement-fonctionnalite.png" width="916" height="742" /></a> Moteur de recherche (95% du business) attendant une régie pub (5% du business)[/caption]

Beaucoup (trop) de fonctionnalités dépendent complètement de JavaScript et de moins en moins de développeurs tentent <a href="http://www.pompage.net/traduction/degradation-elegante-et-amelioration-progressive">l’amélioration progressive</a>. Dans certains cas comme ici un moteur de recherche complexe, il est même irréaliste de vouloir afficher un moteur sans JS, d’où l’idée de mettre une interface de chargement pour patienter. En sachant que ce code métier critique est inclus en haut de page, quand l’exécuter ? Du pire au mieux :
<ol>
  <li>en haut de page ! Bien sur que <strong>NON</strong>, le <abbr title="arbre HTML interprété">DOM</abbr> n’est pas forcément disponible à ce moment-là (ce qui ne se voit pas forcément quand on développe sur son poste en local d’ailleurs :) )</li>
  <li>on fait confiance à StackOverflow et on attend que <code>document.readyState === 'complete'</code> … Cela correspond à l’événement <code>window.load</code>, c’est-à-dire qu’on va attendre patiemment que toutes les images, <code>iframes</code> et même certains scripts asynchrones soient chargés ! L’utilisateur n’attendra pas, lui.</li>
  <li>on attend l’événement <code>document.DOMContentLoaded</code> (en jQuery : <code>$(document).ready(init)</code> ou juste <code>$(init)</code>). Cet événement est retardé par les insertions de scripts même en bas de page, les scripts asynchrones avec l’attribut <code>defer</code> et toute exécution de script, dont le tueur de performances <code>document.write()</code>. La pub utilise toujours cette instruction.</li>
  <li>juste après le code HTML du formulaire : vous avez à la fois la garantie que le DOM est présent, et d’être exécuté avant tous les événements navigateur. C’est ce que je recommande pour du code critique comme ce formulaire.</li>
</ol>
Il y a donc un conflit ouvert entre le tiers inclus et votre code métier, que vous pouvez résoudre en exécutant plus tôt votre code. Si vous persistez à vouloir dépendre de l’événement <code>DOMContentLoaded</code> pour exécuter votre code, il va falloir travailler au corps l’inclusion des scripts des tiers :
<ul>
  <li>s’ils ne sont pas lourds à l’exécution et importants, le plus bas possible dans la source avec l’attribut standard <code>async</code> (IE10 +)</li>
  <li>s’ils sont secondaires, en inclusion asynchrone classique après le chargement de la page (natif <a href="https://gist.github.com/jpvincent/51321638dee74e2dd8f53bada8a47ee5">façon Analytics</a> ou <a href="https://api.jquery.com/jquery.getscript/">$.getScript</a>)</li>
  <li>refuser les scripts dépendant ou utilisant <code>document.write()</code> (d’ailleurs <a href="https://developers.google.com/web/updates/2016/08/removing-document-write">partiellement dépréciée</a> par Chrome).</li>
</ul>
La pub est spéciale : les régies ne peuvent pas se passer de l’instruction <code>document.write()</code> car leurs propres fournisseurs l’utilisent <strong>peut-être</strong>. La solution technique est simplement d’inclure la publicité en <code>iframe</code>, ce qui interdit certains formats invasifs. Désolé de vous le dire mais si votre business dépend de la pub et que vous vous êtes engagé à afficher des pubs qui peuvent déborder de leur cadre, il n’y a pas de solution purement technique ! On va voir <a href="http://braincracking.org/2016/12/21/les-manques-a-gagner-de-la-publicite-pour-les-editeurs/">dans un autre article</a> qu’il y a matière à bien faire, mais que les décisions sont loins d’être techniques.
<h2>Après le chargement</h2>
Vous avez forcément déjà constaté les effets d’interface gelée voire de crash sur mobile, ou vous avez eu l’impression que votre ordinateur portable allait décoller de votre bureau par la seule force de ses ventilateurs. Pour faire le test, je vous conseille de visiter cette page parodique <strong>sans adblock</strong>, voire sur mobile : <a href="http://worldsmostshareablewebsite.greig.cc/">http://worldsmostshareablewebsite.greig.cc/</a>, qui se contente d'inclure plusieurs fournisseurs de boutons de partage. Sur une tablette Nexus 7, l’interface est gelée pendant 20 secondes.
La nouvelle bataille de la performance frontend, est clairement les temps d’exécution abusifs côté client. J’ai déjà eu le cas avec de "simples" inclusions de bouton de partage (Facebook et G+ en tête) mais c’est le cas avec n’importe quel script qui va modifier l’interface. Il y a un principe général à comprendre avec les tiers :
<blockquote>Si c’est rapide à intégrer, c’est que ça va ramer</blockquote>
Les tiers, en particulier s’ils ont une version gratuite, ont 2 besoins antinomiques : simple et passe-partout. Le tutoriel doit paraître simple et consiste généralement en une inclusion de script et à la création d’un peu de HTML. À l’image du bouton Facebook.

[caption id="attachment_1212" align="alignnone" width="540"]<a href="http://braincracking.org/wp-content/uploads/2016/12/facebook.png"><img class="size-full wp-image-1212" alt="code d’inclusion du bouton facebook" src="http://braincracking.org/wp-content/uploads/2016/12/facebook.png" width="540" height="356" /></a> Trop simple pour être performant[/caption]

Remarquez que l’insertion du script est généralement asynchrone comme ici, car les tiers ont fait des efforts pour ne pas bloquer les chargements ces dernières années.
<h3>Détecter le problème</h3>
Maintenant mettez vous à la place du développeur de ce code : comment faire pour savoir où afficher son widget ? La réponse est aussi simple que contre-performante : en s’auto-exécutant dès que possible, et en scannant le DOM régulièrement. Selon la lourdeur initiale de votre site, on peut vite arriver à de jolis blocages dont vous ne comprendrez l’origine qu’avec un profilage en règle. Notez qu’il faut utiliser de vraies machines pour tester correctement tout cela.

[caption id="attachment_1214" align="alignnone" width="1119"]<a href="http://braincracking.org/wp-content/uploads/2016/12/profiler.png"><img class="size-full wp-image-1214" alt="capture d’écran de l’outil de profilage chrome" src="http://braincracking.org/wp-content/uploads/2016/12/profiler.png" width="1119" height="566" /></a> 3 longues secondes d’exécution pour ce seul tiers[/caption]

Le fichier mis en cause ici (<code>post-widget.js</code> surligné en gris) provient effectivement d’un tiers qui inclut des boutons sociaux. Il bloquait l’interface et faisait même régulièrement planter un iPad mini 2. À sa décharge, la page visée était lourde et contenait 150 éléments que le script recherchait et modifiait.
Il n’existait pas d’outil dédié — même payant — permettant de déceler facilement qui était responsable des pics de charge processeur sur une page. J’ai du écrire un petit script du nom de <a href="https://github.com/jpvincent/3rd-party-cpu-abuser">3rd-party-cpu-abuser</a> qui exploite la timeline de Chrome Dev Tools pour mettre des noms sur les responsables :

[caption id="attachment_1215" align="alignnone" width="417"]<a href="http://braincracking.org/wp-content/uploads/2016/12/3rd-party-abuser.png"><img class="size-full wp-image-1215" alt="tableau de statistiques de consommation CPU" src="http://braincracking.org/wp-content/uploads/2016/12/3rd-party-abuser.png" width="417" height="667" /></a> 80% du <abbr title="processeur">CPU</abbr> consommé l’est par les tiers[/caption]

WebPagetest a également récemment introduit une visualisation du CPU répartie par fichier JS : onglet "chrome", case "capture devtools timeline").

Pour débloquer la situation, je me suis contenté comme souvent d’aller lire la documentation pour y chercher 2 optimisations classiques :
<ul>
  <li>comment <strong>ne pas</strong> initialiser le script automatiquement, afin d’éviter l’effet Big Bang du début de construction de page et ne l’exécuter qu’au moment opportun.</li>
  <li>comment indiquer le DOM à modifier, pour éviter les recherches DOM coûteuses</li>
</ul>
Pratiquement tous les fournisseurs de widget proposent maintenant ces options dans leurs APIs. Si ce n’est pas le cas, c’est mauvais signe donc partez chez le concurrent !
<h1>3. L’espionnage industriel</h1>
<h2>Les réseaux sociaux</h2>
<h3>La performance</h3>
Rajouter quelques boutons de partage n’est pas aussi gratuit que ça en a l’air. Il y a bien sûr la performance utilisateur, qui en pâtit.

[caption id="attachment_1218" align="alignnone" width="1500"]<a href="http://braincracking.org/wp-content/uploads/2016/12/buttons.png"><img class="size-full wp-image-1218" alt="comparaison de screenshot montrant le chargement d’une page wordpress avec et sans boutons" src="http://braincracking.org/wp-content/uploads/2016/12/buttons.png" width="1500" height="330" /></a> 4 boutons = ralentissement de l’affichage et du temps de chargement[/caption]

Sur cette <a href="http://wpt.fasterize.com/video/compare.php?tests=161027_KT_15,161027_3X_14">page wordpress simple</a> et optimisée, rajouter 4 boutons natifs multiplie le temps de 1er affichage et de chargement par deux !
<h3>Les taux de transformation</h3>
Mais il faut aussi se demander si ces boutons apportent vraiment quelque chose au site. Par exemple Smashing Mag en 2012 avait constaté qu’en ENLEVANT le bouton Facebook, leurs utilisateurs partageaient plus les articles sur Facebook !

[caption id="attachment_1219" align="alignnone" width="839"]<a href="http://braincracking.org/wp-content/uploads/2016/12/smashing.png"><img class="size-full wp-image-1219" alt="le traffic depuis Facebook a augmenté depuis que nous avons enlevé les boutons FaceBook de nos articles" src="http://braincracking.org/wp-content/uploads/2016/12/smashing.png" width="839" height="465" /></a> Smashing Mag : "le trafic depuis Facebook a augmenté depuis que nous avons enlevé les boutons FaceBook de nos articles" (<a href="https://twitter.com/smashingmag/status/204955763368660992">2012</a>)[/caption]

NB: Entretemps Facebook a sorti un bouton "Partage" plutôt que simplement "Like", et Smashing Mag l’a ré-intégré de manière statique.

Autre exemple : un <a href="https://vwo.com/blog/removing-social-sharing-buttons-from-ecommerce-product-page-increase-conversions/">test A/B fait par un site de e-commerce</a> qui a consisté à <strong>SUPPRIMER ces boutons</strong> de la page produit et leur a permis de constater que cette suppression <strong>AUGMENTAIT les taux de conversion de 12%</strong>. Ils supposent que finalement ces boutons n’étaient qu’une distraction à ce moment-là, et que les chiffres de partage affichés, proches de 0 comme souvent dans le e-commerce, renvoyaient un sentiment négatif. J'y ajouterais que la légère dégradation de performance induite par ces boutons a dû contribuer :)

Bref : comme pour n’importe quelle fonctionnalité, demandez vous si elle ne dessert pas plutôt votre produit, ou s’il n’y a pas une meilleure manière de l’intégrer.
<h3>Nourrir vos concurrents</h3>
La raison d’être secondaire de ces boutons pour Google et Facebook est qu’il leur permet de mieux qualifier leurs propres utilisateurs, en dehors de leurs propres murs. Même lorsque l’utilisateur n’interagit pas avec, leur simple présence signale à G. et FB qu’un de leurs utilisateurs visite une page, leur permettant ainsi d’affiner les profils. Dans le cas de Google, l’utilisateur n’a même pas besoin d’avoir un compte. Ne me lancez pas sur le respect de la vie privée, <a href="https://www.miximum.fr/blog/conf-pw-2017/">d’autres le font mieux que moi</a>, parlons plutôt argent.
À toute fin utile, il faut tout de même rappeler que Google mange 42% du marché de la publicité en France (95% des 47% du <abbr title="Chiffre d’affaire">CA</abbr> de la search).

[caption id="attachment_1220" align="alignnone" width="466"]<a href="http://braincracking.org/wp-content/uploads/2016/12/marché-pub.png"><img class="size-full wp-image-1220" alt="répartition du marché de la publicité en 2014 : 47% pour le search et 31% pour le display" src="http://braincracking.org/wp-content/uploads/2016/12/marché-pub.png" width="466" height="472" /></a> <a href="http://www.iabturkiye.org/sites/default/files/iab_europe_adex_benchmark_2014_report.pdf">Marché de la pub en 2014</a>[/caption]

Pour le marché qui vous intéresse probablement, c’est-à-dire le "display" (affichage de bannières classiques) <a href="https://piacentino.com/jb/2015/publicite-en-ligne-ou-va-largent">on estime</a> que Facebook en capte 30%. Ce que les annonceurs apprécient, outre l’audience, c’est que leur base est hyper qualifiée, et c’est en partie grâce aux sites qui incluent les boutons.
Déjà que ces boîtes s’évitent un maximum d’impôts en France, ce n’est pas la peine de leur faire de cadeau supplémentaire en utilisant leurs scripts tels quels.
<h3>Les solutions</h3>
En admettant que vous ayez déterminé que les boutons de partage étaient vraiment bons pour votre site (typiquement, un site d’articles), il existe des manières d’inclure qui ne nourrissent pas vos concurrents, sont performantes et respectent votre charte graphique :
<ol>
  <li>les boutons statiques, qui sont <a href="https://github.com/bradvin/social-share-urls#google">des liens simples formatés permettant le partage</a> et que vous pouvez mettre par dessus de simples icônes</li>
  <li>si vous avez besoin de fonctionnalités supplémentaires comme des icônes toutes faites, afficher les compteurs ou avoir des analytics, vous pouvez rajouter un peu de JavaScript. Voici une <a href="https://github.com/finderau/share-buttons">solution open-source</a> parmi d’autres</li>
  <li>enfin il existe quantité de services tiers (addThis, shareThis, shareAholic …) mais on retombe sur les travers habituels des tiers : il faut <a href="https://www.xfive.co/blog/social-media-buttons-test-performance-privacy-features/">benchmarker les temps de chargement</a> car ils vont du simple au triple et le business model de certains comme shareAholic est de revendre les données utilisateur</li>
</ol>
<h2>Les tiers des tiers de vos tiers</h2>
<h3>Le problème</h3>
<blockquote>Quand il n’y en a qu’un ça va</blockquote>
S’il est normal de faire confiance à un prestataire direct, la confiance ne peut que s’étioler au fur et à mesure de la chaîne d’appel de leurs partenaires. Or quand vous accordez techniquement les pleins pouvoirs à votre prestataire (une simple insertion de script), vous les accordez également à toute une chaîne de sociétés dont vous n’aviez peut-être jamais entendu parler. Je vais utiliser l’outil <a href="http://requestmap.webperf.tools/">RequestMap</a> pour visualiser un affichage sur la Une du journal Le Monde.

[caption id="attachment_1223" align="alignnone" width="778"]<a href="http://braincracking.org/wp-content/uploads/2016/12/lemonde-niveau1.png"><img class="size-full wp-image-1223" alt="visualisation des requêtes faites vers les 1er tiers" src="http://braincracking.org/wp-content/uploads/2016/12/lemonde-niveau1.png" width="778" height="1108" /></a> Appels vers les tiers de 1er niveau[/caption]

Chaque cercle est un nom de domaine, les flèches sont les appels. Au centre en bleu clair, le domaine principal <code>www.lemonde.fr</code> qui appelle 2 sous-domaines de statiques (<code>s1</code> et <code>s2.lemde.fr</code>). Tous les autres domaines (cercles) appartiennent aux tiers de 1er niveau et correspondent à une <strong>quinzaine de sociétés</strong>. On y trouve 3 solutions d’analytics, une solution de test A/B (Kameleoon), un tracking de réseau social (Po.st), FaceBook, un moteur de recommandation d’articles (Outbrain ici), Cedexis … Ce sont les tiers de 1er niveau : ceux que l’on inclut de manière tout à fait consciente. Les relations avec ces tiers sont régies par un contrat direct ou par l’acceptation des <abbr title="Conditions Générales d’Utilisation">CGU</abbr>, et tant que les principes d’intégration cités plus haut dans cet article sont respectés, je n’ai plus rien à rajouter.

Puis arrive le moment où l’on contacte la régie pub (SmartAd en l’occurrence, à droite sur le graphe précédent, à gauche sur le graphe suivant).

[caption id="attachment_1225" align="alignnone" width="1006"]<a href="http://braincracking.org/wp-content/uploads/2016/12/lemonde-2.png"><img class="size-full wp-image-1225" alt="Tiers de niveau 2, 3 et 4 … Oh la belle rose" src="http://braincracking.org/wp-content/uploads/2016/12/lemonde-2.png" width="1006" height="1109" /></a>Appel des tiers de niveau 2, 3 et 4 … Oh la belle rose[/caption]
Une fois sur son domaine, la régie appelle une <strong>dizaine de sociétés</strong>, principalement des tags de tracking, puis 2 systèmes de mise aux enchères de mon profil utilisateur. C’est la société de <abbr title="Real Time Bidding">RTB</abbr> CasaleMedia qui prend le relai, et qui contacte une <strong>vingtaine d’autres sociétés</strong>. Cela c’est côté client, on ne sait pas combien d’enchérisseur ont accès au profil utilisateur côté serveur. L’enchère a été gagnée par une société de retargeting (Turn) qui elle-même contacte <strong>une vingtaine de sociétés</strong>.
En tout c'est 50 sociétés qui ont été contactées côté client. Si le script du 1er tiers inclus de manière classique, <strong>ces 50 sociétés avec qui vous n’avez aucun contrat ont tout pouvoir sur la page</strong>. Il leur est techniquement possible de la modifier (on a vu des sociétés malhonnêtes remplacer les publicités des autres) ou d’y récupérer des informations : outre les informations de navigation, elles pourraient cibler un site de e-commerce et estimer ses ventes et leur nature par exemple, simplement en scannant le DOM. Ces scénarii sont pratiquement indétectables et quand on voit qu’il arrive que des pirates <a href="https://blog.malwarebytes.com/threat-analysis/2016/01/msn-home-page-drops-more-malware-via-malvertising/">diffusent des virus grâce aux régies publicitaires</a>, il n’est pas irréaliste d’imaginer une société voulant récupérer des informations chez son concurrent.

<h3>Les solutions</h3>
<h4>Se parler !</h4>
Les premières actions ne sont pas techniques du tout. 
D’abord, lister quelles sociétés atterrissent sur vos pages, avec des solutions :
<ul>
  <li>open-source, comme <a href="https://www.mozilla.org/fr/lightbeam/">LightBeam</a> et <a href="https://www.eff.org/fr/privacybadger">privacy-badger</a></li>
  <li>commerciales, comme le populaire <a href="https://www.ghostery.com/">Ghostery</a></li>
</ul>
Ensuite, du côté des contrats signés avec vos partenaires, vérifiez que leurs partenaires à eux vous conviennent et demandez-vous quels engagements de moyens ils ont et pourquoi pas imaginer un système de pénalités. Les régies savent se montrer réactives lorsqu’un de leurs clients leur signale un problème avec un prestataire, mais détecter le problème en question est très compliqué.
Enfin, il faut établir un dialogue entre les équipes IT et les utilisateurs des services tiers (marketing, produit, commerciaux …), en faisant un point régulier "Tiers", avec l’ordre du jour suivant :
<ol>
  <li>l’IT fait l’inventaire des tiers de 1er niveau et remontent éventuellement les problèmes rencontrés</li>
  <li>côté business, on classe par ordre d’importance les tiers … oui il va y avoir du débat !</li>
  <li>décisions de suppression</li>
  <li>décisions quant à l’ordre de chargement</li>
</ol>
On se retrouve à prendre des décisions complexes car comment décider de ce qui est plus important entre un test A/B, les analytics, le player vidéo, les pubs, la carte ou les boutons de partage ? Il faut pourtant que ce soit une décision réfléchie car jusqu’ici, cette décision était de fait laissée à l’intégrateur et au navigateur …

<h4>Côté technique</h4>
Si votre business model vous permet de vous passer des publicités invasives, ce qui est le cas des sites de e-commerce par exemple, alors je ne saurais trop vous recommander la bonne vieille technique de l'inclusion en <code>iframe</code>. Cela gomme les inconvénients de <code>document.write()</code>. L’attribut <code>sandbox</code> vous permet (depuis IE9) d’accorder aux tiers d’accéder à votre page, vos cookies ou le droit d’ouvrir une popup.
<code>&lt;iframe sandbox="allow-scripts" src="carre_pub.html" /&gt;</code> suffit pour les publicités classiques : il autorise JavaScript, mais la sous-page ne peut rien faire sur la page principale. Si vous avez besoin de communiquer, l’API <a href="https://developer.mozilla.org/fr/docs/Web/API/Window/postMessage">postMessage</a> est là pour ça !

Pour aller plus loin, mais je ne l’ai encore jamais vu en production, il y a le standard <a href="https://content-security-policy.com/">Content Security Policy</a> (à partir de Edge) qui permet entre autres de limiter les pouvoirs donnés aux tiers inclus directement en JavaScript. Pour une transition en douceur, vous pouvez passer par une phase d’observation des erreurs côté client (<code>report-uri</code>). Maintenant on ne va pas se mentir : la vraie difficulté, c’est de faire admettre que l’on veut limiter le pouvoir des tiers, pas de les en empêcher !

<h1>Conclusion</h1>
Les tiers posent leurs lots de problèmes mais ils ont pratiquement tous une solution technique, <a href="http://braincracking.org/?p=1256">au contraire des publicités</a>. Du moment que l’on fait attention au moment de l’intégration et que l’on surveille les régressions de performance, on évite la plupart des écueils. Si vous avez plus de quelques services externes et qu’ils peuvent mettre en péril votre business, mettre en place une solution de monitoring dédiée est à envisager rapidement.
