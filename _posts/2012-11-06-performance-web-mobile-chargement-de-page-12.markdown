---
layout: post
title:  "Performance web mobile, chargement de page 1/2"
date:   2012-11-06 16:04:21 +0100
---
<em>Cet article a été initialement publié sur <a title="le train de 13h37" href="http://letrainde13h37.fr/14/performance-web-mobile-chargement-de-page-1-sur-2/">le train de 13h37</a>, magazine du développement web de grande qualité, qui a le double mérite de <a href="http://letrainde13h37.fr/rejoignez-nous/">rémunérer</a> ses auteurs et de proposer les articles avec une licence <a href="http://creativecommons.org/licenses/by-nc-sa/2.0/fr/">CC BY-NC-SA</a>.</em>
<h1>Introduction</h1>
Vos utilisateurs viennent chez vous sur mobile : <a href="http://gs.statcounter.com/#mobile_vs_desktop-FR-monthly-201208-201210">5 % selon StatsCounter</a> ; <a href="http://www.atinternet.fr/documents/systemes-dexploitation-pres-de-7-des-visites-europeennes-pour-ios/">10 % selon ATInternet</a> ; 9 % d’après ce que je vois chez mes clients dont le site n’a PAS été pensé pour du mobile. Évidemment cette part est en augmentation constante, a déjà largement dépassé IE7, va dépasser IE8, et il est raisonnable de penser que, ces statistiques étant récoltées via JavaScript, certaines tentatives d’accès ne soient même pas comptabilisées ! Que vous le vouliez ou non, que votre site ait été fait ou pas pour cet usage, <strong>vos utilisateurs essaient de venir sur votre site dans des conditions de réseau qui ne vous arrangent pas forcément</strong>, mais qui vont vous poser quelques défis intéressants.
<h1>De quels mobiles parle-t-on ?</h1>
Plus on rentre dans les détails des navigateurs, des systèmes d’exploitation et des capacités matérielles, et plus il semble illusoire de définir ce qu’est vraiment un mobile. Restons pragmatiques et, dans le cadre de cet article sur la performance, précisons ce que nous allons considérer comme notre cible :
<ul>
  <li>capacité du navigateur à afficher des sites non spécifiquement prévus pour mobiles ;</li>
  <li>navigation "tactile" ;</li>
  <li>matériel sous-dimensionné par rapport à la machine de bureau moyenne ;</li>
  <li>qualité de réseau fluctuante.</li>
</ul>
Ce dernier critère peut prêter à discussion, tant on observe d’utilisateurs mobiles accéder aux sites depuis le confort du WiFi de la maison : par exemple, la moitié des visiteurs dits "mobile" sont en fait sur iPad, ce qui implique (le plus souvent) le WiFi.

Mais dans le cadre de cet article nous allons surtout nous intéresser aux "problèmes supplémentaires à résoudre pour le chargement des pages" : les temps de chargement depuis une tablette / mobile sur WiFi sont largement comparables aux temps de chargement sur certains navigateurs de bureau.
<h1>Comment tester ?</h1>
Tout développement commence par la mise dans de bonnes conditions pour tester ; la mesure a lieu ensuite dans un chantier performance web. Le banc de test idéal pour le mobile est simplement impossible à monter et force est de constater que les éditeurs de sites, de l’indépendant aux sites avec des millions de pages vues, se contentent généralement de l’iPhone de l’intégrateur et éventuellement de l’Android-qui-s’appelle-revient du collègue.

Si l’on regarde les parts de marché en France, cela correspond effectivement à une réalité : nous avons Safari pour iOS partagé entre les différents iPhone et iPad, et le navigateur par défaut d’Android distribué avec les version 2.x. En queue de peloton et souvent ignorés, à tort pour les valeurs d’interopérabilité du Web, viennent les Opera (mini ou mobile) pourtant ultradominants dans d’autres pays, Firefox mobile, ainsi que différentes variantes de Webkit sur les OS mobiles survivants (Blackberry, SymbianOS, LG…), ainsi que IE Windows.

Ah, et ne comptez pas sur les émulateurs : ils sont déjà à peine acceptables pour tester les fonctionnalités, alors question performance ils sont tout simplement disqualifiés d’office ! Entre l’utilisation du processeur de la machine hôte, une connexion filaire sans défaut et les approximations inhérentes à tout émulateur, vous n’aurez jamais quoi que ce soit de réaliste dans votre quête de la fluidité.

La solution minimale du pauvre, c’est de conserver ses mobiles et d’attendre qu’ils vieillissent ou d’acheter des occasions datant d’un an ou deux, sans mettre à jour le système d’exploitation. Pour simuler un réseau 3G, on pourra utiliser des proxys via WiFi qui dégradent volontairement la connexion. Il y a une grosse dizaine de solutions, généralement limitées à un seul OS (souvent Mac). La plus compatible et reconnue est le proxy payant <a href="http://www.charlesproxy.com/">Charles</a> mais la gratuité de <a href="http://www.jopa.fr/index.php/2010/07/17/limiter-la-bande-passante-avec-trickle/">Trickle</a> (Linux), <a href="http://osxdaily.com/2011/08/10/simulate-internet-connectivity-bandwidth-speeds-network-link-conditioner/">Network Link Conditioner</a> (OSX 10.6) ou <a href="http://slowyapp.com/">Slowy App</a> (OSX 10.5) peut vous séduire.

Le principe est simple : ces logiciels contiennent de quoi dégrader artificiellement une connexion (débit, latence, perte de paquets). Vous configurez votre téléphone pour utiliser votre machine comme proxy via WiFi, et voilà ! En prime, vous pouvez analyser les trames réseau qui passent. C’est moins cher que de vraies cartes SIM et la constance de la dégradation permet de faire des tests répétés, là où la qualité d’un vrai réseau data peut varier d’une minute à l’autre.

Pour être honnête, les techniques décrites dans cet article ont surtout été testées sur le navigateur Android 2.3 et Safari iOS 6, avec des vérifications fonctionnelles sur Firefox et Opera mobile sur Android, les deux absents notoires étant le Safari de Blackberry et Internet Explorer.

Signalons également que nous ignorons délibérément un bon milliard d'habitants sur cette planète qui se connectent au Net avec des mobiles pré-smartphones : techniquement difficiles à adresser sérieusement, ils sont également une minorité en France à posséder ces anciens mobiles et avoir souscrit un abonnement 3G.
<h1>Chargez !</h1>
<h2>L’importance du réseau</h2>
Croyez-le ou non, <strong>les débits théoriques en 3G et technologies au-delà peuvent être plus rapides que les débits constatés des ADSL</strong>. Mais comme finalement assez peu de gens font du Web depuis les laboratoires de tests des constructeurs, il va falloir composer avec la réalité physique : une couverture nuageuse un peu forte, un déplacement un peu rapide, <a href="http://www.clubic.com/smartphone/iphone/actualite-349254-iphone-4-reponse-steve-jobs-probleme-reception.html">un doigt mal placé</a>, un mur un peu épais, ou des gens autour de vous également équipés de mobiles (incroyable en 2012), et vous vous retrouvez rapidement avec des débits ridicules, des pertes de paquets énormes et le sentiment global qu’on vous a un peu survendu la "meilleure couverture réseau de France".

Lorsque le réseau marche, l’utilisateur français moyen a <strong>en moyenne 2,50 Mb/s</strong> (<a href="http://letrainde13h37.fr/23/debit-latence-france-bien-calibrer-tests-performance/">sources</a>) ce qui est plus rapide que 25% des ADSL de France. Par contre, la latence ou ping est en moyenne à 200 ms soit le double de la moyenne en ADSL.

Depuis <a href="http://www.belshe.com/2010/05/24/more-bandwidth-doesnt-matter-much/">toujours</a>, la latence est l’ennemi numéro 1 de nos pages Web. Ceci est du à <a href="http://www.slideshare.net/edaspet/soire-webperf-du-29-nov-2010-latence-et-cdn">la nature même de HTTP</a> et signifie que pour un débit moyen seulement deux fois inférieur, le temps de chargement sera en fait 3 à 6 fois plus long sur mobile ! Pire, la perte de paquets est assez conséquente et inhérente à la technologie radio utilisée pour communiquer entre le mobile et l’antenne de l’opérateur. Et cette perte de paquets arrive plus fréquemment pour les petites requêtes… Exactement la manière dont sont composées la majorité de nos pages Web actuelles !

Ajoutez à cela qu’accrocher et maintenir le réseau data est une des opérations les plus coûteuses en terme de batterie : passez en mode avion sur votre smartphone, utilisez-le presque comme d’habitude (lectures, jeux…) et vous verrez qu’il peut tenir une seconde journée sans recharge. Et souvenez-vous des anciens portables sans data qui tenaient une semaine…

Bref, c’en est au point que les constructeurs optimisent comme ils peuvent le matériel, par exemple en mettant en veille la radio après un certain temps d’inutilisation. On se retrouve donc dans des situations où, le temps de lire cet article, l’antenne s’est déconnectée et il vous faudra plusieurs secondes pour que la requête suivante aboutisse !

Malgré cela, les utilisateurs ne sont pas beaucoup plus patients que sur un ordinateur de bureau : après 5 à 10 secondes de page blanche, une majorité d’utilisateurs a déjà rebroussé chemin, et autant vous dire qu’ils n’apparaissent même pas sur les statistiques collectées en JavaScript. Comment faire pour vaincre l’hémorragie de visiteurs et proposer des services de meilleure qualité perçue ?

Eh bien il va nous falloir revisiter tout l’arsenal des classiques de la Performance Web.
<h2>Remettre en cause les classiques</h2>
Lorsque le pape de la performance, Steve Souders, a émis chez Yahoo! <a href="http://developer.yahoo.com/performance/rules.html">les premières recommandations de performance Web</a>, c’était en faisant du reverse-engineering sur les moteurs IE6-IE7 et Firefox 2. C’était en 2005 et, avec l’abandon progressif du support de ces navigateurs antédiluviens (à l’échelle du Web) d’un côté et la banalisation de l’ADSL de l’autre, certaines règles ou astuces sont devenues moins importantes.

L’apparition de la navigation sur mobile en a même rendu certaines dangereuses !
<h3>Le domain-sharding</h3>
Citons par exemple le domain-sharding, qui consiste à répartir ses fichiers statiques sur plusieurs sous-domaines différents. Étonnamment populaire, à l’origine cette technique permettait de passer outre la limitation à 2 files de téléchargement par domaine et de bénéficier de toute la largeur des bandes passantes modernes. Mais à partir de IE8, sorti en 2009, cette technique devient inutile car le nombre de téléchargements simultanés passe de 2 à généralement 6 : les navigateurs se sont adaptés à la banalisation des gros débits et tentent d’en tirer parti.

Utilisons le service <a href="http://stevesouders.com/cuzillion/">Cuzillon</a> qui va nous permettre de <a href="http://stevesouders.com/cuzillion/?c0=bi1hfff0_0_f&amp;c1=bi1hfff0_0_f&amp;c2=bi1hfff0_0_f&amp;c3=bi1hfff0_0_f&amp;c4=bi1hfff0_0_f&amp;c5=bi1hfff0_0_f&amp;c6=bi2hfff0_0_f&amp;c7=bi2hfff0_0_f&amp;c8=bi2hfff0_0_f&amp;c9=bi2hfff0_0_f&amp;c10=bi2hfff0_0_f&amp;c11=bi2hfff0_0_f&amp;c12=bi3hfff0_0_f&amp;c13=bi3hfff0_0_f&amp;c14=bi3hfff0_0_f&amp;c15=bi3hfff0_0_f&amp;c16=bi3hfff0_0_f&amp;c17=bi3hfff0_0_f&amp;c18=bi4hfff0_0_f&amp;c19=bi4hfff0_0_f&amp;c20=bi4hfff0_0_f&amp;c21=bi4hfff0_0_f&amp;c22=bi4hfff0_0_f&amp;c23=bi4hfff0_0_f&amp;t=1344287686190">simuler une page avec 24 images réparties sur 4 domaines</a> et <a href="http://stevesouders.com/cuzillion/?c0=bi1hfff0_0_f&amp;c1=bi1hfff0_0_f&amp;c2=bi1hfff0_0_f&amp;c3=bi1hfff0_0_f&amp;c4=bi1hfff0_0_f&amp;c5=bi1hfff0_0_f&amp;c6=bi1hfff0_0_f&amp;c7=bi1hfff0_0_f&amp;c8=bi1hfff0_0_f&amp;c9=bi1hfff0_0_f&amp;c10=bi1hfff0_0_f&amp;c11=bi1hfff0_0_f&amp;c12=bi1hfff0_0_f&amp;c13=bi1hfff0_0_f&amp;c14=bi1hfff0_0_f&amp;c15=bi1hfff0_0_f&amp;c16=bi1hfff0_0_f&amp;c17=bi1hfff0_0_f&amp;c18=bi1hfff0_0_f&amp;c19=bi1hfff0_0_f&amp;c20=bi1hfff0_0_f&amp;c21=bi1hfff0_0_f&amp;c22=bi1hfff0_0_f&amp;c23=bi1hfff0_0_f&amp;t=1344289103427">une autre page avec 24 images sans domain-sharding</a>. Le service <a href="http://www.webpagetest.org/">WebPageTest</a> nous permet de voir qu’il y avait effectivement une amélioration sur le temps de chargement sur IE7 :

[caption id="attachment_1158" align="alignnone" width="1251"]<a href="http://braincracking.org/wp-content/uploads/2012/11/ie7-sharding.png"><img class="size-full wp-image-1158" title="ie7-sharding" src="http://braincracking.org/wp-content/uploads/2012/11/ie7-sharding.png" alt="" width="1251" height="286" /></a> WebPagetest – Test de chargement d’une page sous IE7 avec (en bas) et sans sharding (en haut). <a href="http://www.webpagetest.org/video/compare.php?tests=120806_f7a9cc3a9bae3fff71e1ac524842bca3%2C120806_379be6337c90ce0cb2273ad4d88a709e&amp;thumbSize=200&amp;ival=500&amp;end=all">Voir la source</a>[/caption]

Le test sur les mêmes URLs sur IE8 nous montre que l’on ne gagne plus grand chose en premier rendu (les petites images chargées) :

[caption id="attachment_1159" align="alignnone" width="1058"]<a href="http://braincracking.org/wp-content/uploads/2012/11/ie8-sharding.png"><img class="size-full wp-image-1159" title="ie8-sharding" src="http://braincracking.org/wp-content/uploads/2012/11/ie8-sharding.png" alt="" width="1058" height="278" /></a> WebPagetest – Test de chargement d’une page sous IE8 avec (en bas) et sans sharding (en haut). <a href="http://www.webpagetest.org/video/compare.php?tests=120806_c2e00161edfe350d8d77969b44361ccc,120806_a0162c681568ff998a5cb40f6c03b8cd">Voir la source</a>[/caption]

On ne gagne plus rien car la parallélisation des téléchargements est assurée de base par le navigateur. On a en fait rajouté deux mini-problèmes en rajoutant un temps de résolution DNS supplémentaire par domaine, et en faisant concourrir plusieurs images pour la même bande passante. Je dis "mini-problème" en supposant qu’on ne ciblait que les navigateurs de bureau (c’est mal) où ces problèmes sur des vrais sites (75-150 requêtes, 10-15 domaines) passent finalement inaperçus, mais vous ciblez les mobiles, pas vrai ?

Sur mobile, les navigateurs ouvrent également plusieurs connexions par domaine et font même du multiplexing HTTP (plusieurs requêtes HTTP dans le même tuyau TCP) pour minimiser les problèmes liés à la latence. Vous prenez alors le risque de couper court à ces optimisations. Résultat ?

[caption id="attachment_1160" align="alignnone" width="807"]<a href="http://braincracking.org/wp-content/uploads/2012/11/ios-sharding.png"><img class="size-full wp-image-1160" title="ios-sharding" src="http://braincracking.org/wp-content/uploads/2012/11/ios-sharding.png" alt="" width="807" height="495" /></a> WebPagetest – Test de chargement d’une page sous iOS avec (en bas) et sans sharding (en haut). <a href="http://www.webpagetest.org/video/compare.php?tests=120806_K6_W44,120806_EZ_W36">Voir la source</a>[/caption]

<strong>L’affichage a été ralenti</strong> ! En forçant encore plus de domaines, vous demandez à plus d’objets de se partager la même bande passante, que vous devez considérer a priori comme rare sur mobile. Donc vous ralentissez l’affichage des premières images alors même que les suivantes ne sont pas immédiatement utiles. Ici, le temps de chargement total est en plus augmenté car le multiplexing n’a pas pu être utilisé, ce qui peut être le cas sur des sites avec un ratio ressources / domaine faible.
<h3>Les scripts en bas de page</h3>
Le temps avant le premier rendu (l’affichage des premiers pixels du site) est un peu le but ultime de la performance Web, car il y a une très forte corrélation entre ce temps et une augmentation des taux de transformation ( + rapide = + d’argent ).

Dans ce cadre, une autre technique populaire consiste à déplacer toutes les balises script en bas de page. Cette technique marche effectivement bien sur les anciens navigateurs et continue à faire ses preuves dans une moindre mesure sur les nouveaux malgré leurs progrès dans la prioritisation des téléchargements.

Regardons <a href="http://www.webpagetest.org/video/compare.php?tests=120806_K3_WTX,120806_PB_WT9">l’effet attendu avec IE8</a> sur une page simple où le téléchargement du JavaScript est retardé côté serveur.

[caption id="attachment_1161" align="alignnone" width="2076"]<a href="http://braincracking.org/wp-content/uploads/2012/11/desktop-scriptsbottom.png"><img class="size-full wp-image-1161" title="desktop-scriptsbottom" src="http://braincracking.org/wp-content/uploads/2012/11/desktop-scriptsbottom.png" alt="" width="2076" height="298" /></a> Bureau : tests de comparaison de chargement d’une page avec les scripts en bas et en haut de page. <a href="http://www.webpagetest.org/video/compare.php?tests=120806_K3_WTX,120806_PB_WT9">voir la source</a>[/caption]

En haut, la page améliorée par déplacement des balises script ; on voit que le contenu (une simple phrase) s’affiche rapidement, alors qu’en bas la page blanche de la mort est de rigueur.

Passons sur mobile, si vous le voulez bien, et <a href="http://www.webpagetest.org/video/compare.php?tests=120806_A9_WQE,120806_BA_WQ9">testons Safari iOS et Android Browser 2.3.</a>

[caption id="attachment_1162" align="alignnone" width="1794"]<a href="http://braincracking.org/wp-content/uploads/2012/11/mobiles-scriptsbottom.png"><img class="size-full wp-image-1162" title="mobiles-scriptsbottom" src="http://braincracking.org/wp-content/uploads/2012/11/mobiles-scriptsbottom.png" alt="" width="1794" height="509" /></a> Mobiles Android et iOS : tests de comparaison de chargement d’une page avec les scripts en bas de page. <a href="http://www.webpagetest.org/video/compare.php?tests=120806_A9_WQE,120806_BA_WQ9">Voir la source</a>[/caption]

Android se comporte comme nos navigateurs de bureau et affiche le contenu dès qu’il l’a. Safari iOS préfère attendre le script avant d’afficher quoi que ce soit, sans doute par peur du FOUC (<em>Flash Of Unscripted Content</em>).

On peut débattre sur le fait qu’ergonomiquement il vaut mieux attendre une page bien faite (complètement scriptée) plutôt que d’afficher rapidement du contenu pas finalisé, mais <strong>les taux de tranformation montrent que l’affichage rapide rapporte plus</strong>.
Et de toute façon, en tant qu’auteur de la page, c’est à vous de juger si elle peut être vue momentanément sans scripting ou pas : si vous utilisez des méthodes de développement par couche (HTML, puis CSS, puis JS), ça devrait être bon. Si vous voulez de la performance, il va donc falloir faire évoluer votre technique d’inclusion de script pour passer à une version asynchrone.

Le chargement asynchrone à la base est bête comme chou :
<pre><code>var oNode = document.createElement('script'); 
oNode.type = 'text/javascript'; 
oNode.src = 'http://domaine/monscript.js'; 
document.getElementsByTagName('head')[0].appendChild(oNode); </code></pre>
Vous utilisez le DOM pour créer dynamiquement de nouveaux noeuds script qui vont charger des fichiers JS. Cette technique est notamment utilisée par les Google Analytics, boutons G+ et autres Like Facebook pour s’insérer dans vos sites sans introduire de dépendance forte vis-à-vis de leurs serveurs.
Autrement dit, lorsque leurs serveurs sont en rade ou bloqués par un proxy, le navigateur va tout de même afficher votre page (trop aimable). Utiliser cela avec ses propres scripts est globalement une bonne idée, et <strong>devient vital sur mobile</strong>. Vous aurez probablement besoin d’aller plus loin avec le chargement dynamique de scripts (exécuter du code lorsque le fichier est arrivé, gestion des dépendances et de l’ordre de chargement, concaténation de fichiers à la volée…), aussi pensez à utiliser des frameworks solides comme <a href="http://requirejs.org/">requireJS</a>, <a href="http://yepnopejs.com/">YepNope.js</a> (inclus avec Modernizr), <a href="http://headjs.com/">HeadJS</a>, <a href="http://labjs.com/">LabJS</a>… Chacun a sa philosophie.
Et puisque je vous tiens, voici le mien : <a href="https://github.com/jpvincent/LazyLoadLight">LazyLoadLight</a>, qui se veut léger et facile :)

Voir <a href="http://braincracking.org/?p=1164">la suite de cet article</a> qui parle des chemins à explorer pour des pages toujours plus rapides sur mobile.
