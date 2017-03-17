---
layout: post
title:  "Le web mobile et la performance"
date:   2014-12-12 16:04:21 +0100
---
Cet article a été rédigé initialement pour 24 jours de web : http://www.24joursdeweb.fr/2014/le-web-mobile-et-la-performance/

Tout site sortant aujourd’hui doit avoir été pensé avec des contraintes mobiles, et utiliser des techniques d’amélioration progressive pour délivrer rapidement les fonctionnalités sur toutes plateformes.

En attendant cette conclusion, nous commencerons par essayer de connaître notre utilisateur, puis nous verrons les tactiques et stratégies propres au mobile.
<h2>Où est le problème ?</h2>
La <abbr title="Téléphonie 4e génération">4G</abbr> a débarqué, des mobiles à 8 processeurs se vendent, et la plupart des gens utilisent leur mobile depuis chez eux : en Wifi ! À priori le métier de spécialiste performance web serait donc mort et vous me trouverez en train de parler <abbr title="Hypertext markup language, langage de marquage hypertexte">HTML</abbr>5 à mes chèvres dans le Larzac, merci au revoir. À moins que…
<h3>L’utilisation</h3>
La « situation de mobilité » est-elle un mythe ? L’expérience et une <a href="http://services.google.com/fh/files/misc/omp-2013-fr-local.pdf">étude Google</a>montrent clairement que les ¾ des gens utilisent majoritairement leur mobile depuis le confort de leur canapé / lit / lunette, derrière le wifi familial. 15% de ces mêmes Français ont même la fibre.

Le temps passé dans une situation confortable est majoritaire mais cela est justement dû à <strong>la réalité de la situation de mobilité : cela doit être très rapide</strong>. Dans un magasin, les gens vérifient les prix des concurrents sur le Web : si votre page ne s’affiche pas en plus de quelques secondes, vous avez perdu la guerre des prix dès la première bataille (afficher le prix) ! Dans la rue, l’utilisateur va vouloir connaître les horaires de cinéma ou les restaurants du coin : si c’est votre coeur de métier vous n’aurez toutes vos chances qu’en affichant rapidement l’information demandée. Dans les situations d’attente, les gens dégainent les téléphones pour consulter réseaux sociaux et mails : si votre page a la chance d’être dans leur fil d’information, il va falloir afficher la promesse très rapidement (un article sur les félins à travers les âges, une vidéo de chats, le lolcat du jour…). Enfin la préparation d’achat se fait fréquemment sur mobile (plus discret dans l’open space, à portée de main lors des pauses clope voire au feu rouge), même si la finalisation de la commande se fait encore majoritairement sur l’ordinateur du foyer voire sur tablette.

Tout cela ne compte que pour quelques secondes dans les statistiques mais les utilisateurs se souviendront de votre marque de manière très positive si vous avez résolu leur problème rapidement.
<h3>La connexion</h3>
Les données de cette année montrent clairement une amélioration des débits <abbr title="Téléphonie de 3e génération">3G</abbr> (6 <abbr title="Megabytes">Mb</abbr>/<abbr title="seconde">s</abbr> en moyenne selon Akamai) et la <abbr>4G</abbr> à la française envoie du poney au galop (21-32 <abbr>Mb</abbr>/<abbr>s</abbr> <strong>constatés</strong> selon <a href="http://www.degrouptest.com/publications/14/Barometre-Internet-mobiles/4">Degrouptest</a>, pointes de 34 <abbr>Mb</abbr>/<abbr>s</abbr> selon <a href="http://www.akamai.com/dl/whitepapers/akamai-soti-a4-q214.pdf">Akamai</a>).

Mais si vous êtes utilisateur régulier du mobile, vous devinez la vérité derrière ces excellentes moyennes : même avec une puce et un abonnement 4G vous ne chargez parfois pas du tout les pages et vous esquissez un sourire triomphant lorsqu’un site web se charge aussi bien qu’au bureau. C’est parce que l’ennemi numéro 1 du chargement de page web reste la <strong>latence réseau</strong>, et même sur une <abbr>4G</abbr> de bonne qualité les latences constatés par Degrouptest sont entre 50 et 100 <abbr title="millisecondes">ms</abbr>.

Concrètement :
<ul>
  <li>ligne du haut : une <abbr>4G</abbr> de cadre parisien avec <strong>20 <abbr>Mb</abbr>/<abbr>s</abbr> de débit</strong> mais 200 <abbr>ms</abbr> de latence (les murs de Paris sont épais),</li>
  <li>ligne du bas : l’<abbr title="haut débit asymétrique">ADSL</abbr> de la mamie du Cantal avec <strong>2 <abbr>Mb</abbr>/<abbr>s</abbr></strong> et 20 <abbr>ms</abbr> de latence (oui son <abbr title="nœud de raccordement local au réseau">DSLAM</abbr> est proche).</li>
</ul>
<a href="http://media.24joursdeweb.fr/2014/12/debit-vs-latence.jpg"><img alt="La mamie du Cantal gagne (et avec un ping pareil, elle frag aussi de l’ado à tout va). " src="http://media.24joursdeweb.fr/2014/12/debit-vs-latence.jpg" width="715" height="290" /></a>

La mamie du Cantal gagne
(et avec un ping pareil, elle frag aussi de l’ado à tout va).

L’affichage via la connexion à 2 <abbr>Mb</abbr>/<abbr>s</abbr> commence 1 seconde plus tôt, <strong>le formulaire est utilisable 2 secondes plus tôt</strong> et le site au complet se charge 5 secondes plus vite.

Ajoutez à cela que les réseaux <abbr>3G</abbr> des grandes villes sont saturées, et qu’en fonction de l’endroit, de l’heure de la journée, du jour de la semaine et de la position de votre doigt sur le mobile, les performances ne sont pas du tout les mêmes. Dans certains cas pas un octet ne passe.

Tout ça pour dire que les grands principes de la <strong>limitation du nombre de requêtes</strong> d’abord, puis <strong>du poids du contenu</strong> restent valables.
<h3>L’utilisateur</h3>
Tout le monde n’investit pas dans un bidule avec 8 processeurs et 3 <abbr title="Giga-octets">Go</abbr> de <abbr title="mémoire vive">RAM</abbr>, par contre les utilisateurs mobiles sont clairement habitués à ce que les interfaces soient fluides et réactives, et même à ce qu’on leur affiche des données en l’absence de réseau. Ajoutez à cela le fait que l’on sert fréquemment des sites mobiles minimalistes et vous comprendrez que l’utilisateur a bien du mal à accepter que les sites qu’il consulte mettent du temps à apparaître, ou laggent lorsqu’il scrolle.

Du coup un tombereau d’études utilisateur ou d’analyse statistiques démontrent ce que l’on pressent déjà : le temps coûte cher.

Absolument tous les indicateurs marketing sont touchés :
<ul>
  <li>le taux de rebond (<a href="http://radar.oreilly.com/2014/01/web-performance-is-user-experience.html" hreflang="en">Etsy</a> : ajouter 160<abbr title="Kilo-octets">Ko</abbr> d’images = +12% de taux de rebond),</li>
  <li>le taux de clic (<a href="http://doubleclickadvertisers.blogspot.fr/2011/06/cranking-up-speed-of-dfa-leads-to.html" hreflang="en">DoubleClick</a> : supprimer 1 redirection = +12% de taux de clic),</li>
  <li>l’abandon pur et simple de la page (<a href="http://www.radware.com/PleaseRegister.aspx?returnUrl=6442454446&amp;ref=prodoverview" hreflang="en">Radware</a> : 60% d’abandon après 4 secondes de page blanche),</li>
  <li>l’abandon du processus d’achat (<a href="http://minus.com/msM8y8nyh#1e" hreflang="en">Wallmart</a> : -50% de conversion par seconde),</li>
  <li>l’abandon du visionnage (<a href="http://people.cs.umass.edu/~ramesh/Site/HOME_files/imc208-krishnan.pdf" hreflang="en">Akamai</a> : -6% de vidéo vue par seconde d’attente),</li>
  <li>la <a href="http://calendar.perfplanet.com/2013/slow-pages-damage-perception/" hreflang="en">perception négative</a> de la marque.</li>
</ul>
Le côté amusant de l’histoire : si vous surveillez vos taux de rebond sur mobile en vous disant « ça va encore, » dites-vous que les statistiques sont en dessous de la réalité puisque dans les cas des pages blanches, il est probable que la requête remontant sur le serveur qui comptabilise les hits ne soit jamais partie.
<h3>Nos sites !</h3>
Étant donné que les grands chefs regardent les sites sur des mobiles qui valent <a href="http://store.apple.com/fr/buy-iphone/iphone6/%C3%A9cran-5,5%C2%A0pouces-128go-or-d%C3%A9verrouill%C3%A9">le prix de ma première voiture</a>, via le wifi de la boite et qu’ils s'arrêtent à la homepage, forcément il y a un peu de laisser-aller sur les sites, y compris dédiés mobile. Le poids des sites dits dédiés mobile (mDot) <a href="http://www.webperformancetoday.com/2014/05/28/mobile-optimized-pages-bigger/" hreflang="en">augmente régulièrement</a>. La bonne idée d’avoir un site mobile dépouillé et ergonomiquement efficace s’efface pour laisser la place aux mauvaises habitudes des sites desktop : on met tout sur la HomePage, et on rajoute un maximum d’information sur les pages intérieures.

Le <abbr title="responsive web design, design web réactif">RWD</abbr> devient la norme pour les refontes graphiques, mais rarement le « <i lang="en">mobile first</i> » : on se retrouve donc avec des spécifications planifiant 5 <i lang="en">breakpoints</i>, devant afficher des images « retina » tout en gérant <abbr title="Internet Explorer">IE</abbr> 6 (oui cette spécification existe vraiment). Le poids des sites en général a doublé en 3 ans et grâce au <abbr>RWD</abbr> on demande maintenant d’afficher des sites de 2 <abbr>Mo</abbr> et 100 requêtes sur mobile. Ça finirait par arriver si l’utilisateur était encore là pour le voir.

Sur ces même sites <i lang="en">cross-device</i>, votre département marketing a bien sûr opté pour la totale :
<ul>
  <li>de l’<i lang="en">analytics</i> (augmentation du nombre de requêtes),</li>
  <li>des publicités (poids, temps d’exécution, requêtes),</li>
  <li>de l’<i lang="en">A/B Testing</i> (retardement <strong>volontaire</strong> de l’affichage, temps d’exécution),</li>
  <li>des boutons sociaux (le moindre bouton G+ rajoute 100 <abbr>Ko</abbr> et des dizaines de requêtes)</li>
</ul>
Et les tendances webdesign du moment ne sont plus aux coins arrondis qu’on aurait pu régler en CSS3 en pouffant, mais aux polices de caractère (blocage temporaire du rendu, texte invisible, poids), aux « <i lang="en">hero images</i> » (l’image principale en 1200x800, <i lang="en">retina-ready</i>, cordialement) ou pire aux <i lang="en">slideshows</i> (<a href="http://www.doisjeutiliser.fr/unCarrousel/">pourtant une mauvaise idée</a>) qui peuvent charger de grosses images invisibles.

<a href="http://media.24joursdeweb.fr/2014/12/chargement-pmvc.jpg"><img alt="Chargement du site Promovacances" src="http://media.24joursdeweb.fr/2014/12/chargement-pmvc-860x82.jpg" srcset="http://media.24joursdeweb.fr/2014/12/chargement-pmvc-860x82.jpg 860w, http://media.24joursdeweb.fr/2014/12/chargement-pmvc-430x41.jpg 430w, http://media.24joursdeweb.fr/2014/12/chargement-pmvc.jpg 1372w" width="860" height="82" /></a>

<a href="http://media.24joursdeweb.fr/2014/12/lequipe.jpg"><img alt="Chargement du site LEquipe.fr" src="http://media.24joursdeweb.fr/2014/12/lequipe-860x115.jpg" srcset="http://media.24joursdeweb.fr/2014/12/lequipe-860x115.jpg 860w, http://media.24joursdeweb.fr/2014/12/lequipe-430x57.jpg 430w, http://media.24joursdeweb.fr/2014/12/lequipe.jpg 1365w" width="860" height="115" /></a>

Veuillez patienter, un opérateur va prendre votre appel
<h2>L’arsenal technique</h2>
Le problème est donc bien réel et s’inscrit dans la durée : les utilisateurs seront de plus en plus exigeants, les sites de plus en plus gourmands, et la latence des réseaux mobiles ne fera pas de grands progrès. Ce qui tombe bien c’est qu’on peut y faire quelque chose.
<h3>Le chemin critique</h3>
D’accord le chemin critique est <a href="https://developer.yahoo.com/performance/rules.html" hreflang="en">connu depuis 2006</a>, désolé de redire les choses mais sur mobile il ne faut surtout pas se rater sur les bases. Prenons un cas un peu extrême (iOS &lt; 8, grosse latence, débit correct).

<a href="http://media.24joursdeweb.fr/2014/12/lemonde-deroule.jpg"><img alt="Chargement du site LeMonde.fr" src="http://media.24joursdeweb.fr/2014/12/lemonde-deroule-860x128.jpg" srcset="http://media.24joursdeweb.fr/2014/12/lemonde-deroule-860x128.jpg 860w, http://media.24joursdeweb.fr/2014/12/lemonde-deroule-430x64.jpg 430w, http://media.24joursdeweb.fr/2014/12/lemonde-deroule.jpg 1575w" width="860" height="128" /></a>

La fameuse angoisse de la page blanche (1 case = 1 seconde)

Pourquoi 9 secondes avant le premier pixel, et 10 avant de voir quelque chose d’utile ? Regardons l’action au ralenti, avec la trace réseau suivante.

<a href="http://media.24joursdeweb.fr/2014/12/lemonde-reseau.jpg"><img alt="Vue réseau sur LeMonde.fr" src="http://media.24joursdeweb.fr/2014/12/lemonde-reseau.jpg" width="759" height="671" /></a>

Le trait vert est le premier Graal.

Vous pouvez considérer que tout ce qui est à gauche de la ligne verticale verte (l’affichage du premier pixel) est sur le chemin critique. Un effort d’optimisation est déjà en place car les <abbr title="Cascading stylesheets, feuilles de styles">CSS</abbr> sont déjà groupés et <strong>les JavaScript sont chargés de manière asynchrone</strong>. Sauf que c’est à cause de cette dernière optimisation que le site est ralenti : ce qui marche sur navigateur de bureau n’est pas forcément vrai sur mobile.

<strong>Déplacer les JavaScript en bas de page aurait aussi été une mauvaise idée</strong>, grâce à notre ami Safari iOS qui refuse d’afficher quoi que ce soit tant que ces fichiers ne sont pas là. Ici comme sur la plupart des sites, on gagnerait plusieurs secondes en chargeant de manière classique les <strong>fichiers <abbr title="Javascript">JS</abbr> agglomérés, en haut de page</strong>.

Pour résumer :
<ul>
  <li>testez sur les vraies plate-formes avant d’appliquer des recettes qui marchent ailleurs.</li>
  <li>choisissez la bonne stratégie de chargement des <abbr>JS</abbr> : de manière classique en haut de page, ça marche souvent très bien</li>
  <li>les recettes classiques doivent être appliquées : groupement en 6 requêtes maximum des <abbr>HTML</abbr> / <abbr>CSS</abbr> / <abbr>JS</abbr></li>
  <li>minification et gzip des ressources de type texte</li>
</ul>
<h3>La police</h3>
Les <i lang="en">fonts</i> sont la grosse <a href="http://httparchive.org/trends.php?s=All&amp;minlabel=Nov+15+2011&amp;maxlabel=Nov+15+2014#perFonts" hreflang="en">tendance 2013</a> et sont maintenant présentes sur la moitié des sites. C’est joli tout plein mais ça retarde encore le moment où l’utilisateur peut accéder au contenu. En fait il y a 3 états possibles pour l’affichage de votre texte.

<a href="http://media.24joursdeweb.fr/2014/12/deroule-font-lequipe.jpg"><img alt="Chargement des polices sur LEquipe.fr" src="http://media.24joursdeweb.fr/2014/12/deroule-font-lequipe-860x364.jpg" srcset="http://media.24joursdeweb.fr/2014/12/deroule-font-lequipe-860x364.jpg 860w, http://media.24joursdeweb.fr/2014/12/deroule-font-lequipe-430x182.jpg 430w, http://media.24joursdeweb.fr/2014/12/deroule-font-lequipe.jpg 1180w" width="860" height="364" /></a>

Les phases possibles pour un chargement de police

<strong>Phase 1</strong> : pas de bras, pas de chocolat, le texte est là mais masqué. Un peu dommage pour un site essentiellement basé sur le contenu textuel.

<strong>Phase 2</strong> : le navigateur en a marre et vous sauve la mise en affichant le texte. Mal stylé mais fonctionnel.

<strong>Phase 3</strong> : tout va bien, le site est quasiment comme sur la maquette. On espère que l’utilisateur regarde encore.
<h4>Savoir coder</h4>
L’art ancestral de <strong>l’amélioration progressive </strong>doit aussi s’appliquer aux polices : la phase 2 n’existe que si vous avez bien pensé à préciser une police système de fallback. Dans le cas contraire <strong>l’utilisateur regardera plus longtemps le site sans rien pouvoir y lire</strong>.

Pour raccourcir voire éliminer la phase sans tete lisible, il s’agit de faire un travail approfondi de coopération intégrateur / designer : 1 ou 2 polices maximum, de moins de 40<abbr>Ko</abbr>, testées sur toutes les plate-formes (mention spéciale à Chrome sur XP)

Ensuite vient le travail d’optimisation :
<ul>
  <li>la <i lang="en">font</i> critique doit avoir sa<strong> place dans vos 6 premières requêtes</strong>.</li>
  <li>chargement asynchrone des polices secondaires (variantes, titres invisibles, police d'icônes…).</li>
</ul>
Il y a des techniques plus avancées permettant de toujours afficher du texte même si ça n’est pas la <i lang="en">font</i> finale, ce qui ne plaît pas toujours. Vous pouvez également utiliser les <code>data:uri</code> et un encodage base64 de votre police directement dans le <abbr>CSS</abbr>, mais vous l’alourdissez du poids de la police.

Testez.
<h3>Se méfier des autres</h3>
Je vois encore des développeurs inclure des scripts depuis des serveurs qui ne leur appartiennent pas. Il s’agit le plus souvent d’une version plus ou moins à jour de jQuery ou de <abbr>HTML</abbr>5 shim. Les bénéfices sont nuls, et le risque énorme. Vous pensiez sérieusement que les serveurs des grands de ce monde sont disponibles 100% du temps ? Ou qu’ils répondent toujours plus vite que vos propres serveurs ?

Non.

<a href="http://media.24joursdeweb.fr/2014/12/spof-facebook.jpg"><img alt="Single Point Of Failure Facebook" src="http://media.24joursdeweb.fr/2014/12/spof-facebook.jpg" width="662" height="293" /></a>

<abbr title="Single point of failure, point unique d'échec">SPOF</abbr> ? pouf.

Sur ces graphes, on voit les temps d’affichage de sites majeurs (ligne du haut) <strong>augmenter de 20 secondes</strong> pendant le temps où les serveurs Facebook sont moins disponibles. Pour faire le point des dangers page par page, je vous conseille d’utiliser <a href="https://chrome.google.com/webstore/detail/spof-o-matic/plikhggfbplemddobondkeogomgoodeg" hreflang="en">SPOF-O-Matic</a>.

Sur mobile la résolution du nouveau nom de domaine est particulièrement pénible à cause de la latence. Les stratégies sont relativement simples, en fonction de la nature de la ressource :
<ul>
  <li><i lang="en">widget</i> / bouton : utiliser la version asynchrone pour ne pas dépendre des autres,</li>
  <li><i lang="en">widget</i> / bouton : mieux, une version statique vous évite 100-200<abbr>Ko</abbr> et des temps d’exécution énormes,</li>
  <li>JavaScript ou <abbr>CSS</abbr> tiers : <strong>rapatriez le script bien au chaud chez vous, groupez-le avec les autres si il est critique</strong></li>
  <li>polices : rapatriement sur vos serveurs, quitte à payer une licence</li>
</ul>
<h4>Publicités, trackers et autres <i lang="en">A/B testing</i></h4>
Ils requièrent tous d’être en haut de page pour être exécutés en premier et sont massivement déployés sur les sites même dédiés mobiles. Le département marketing moderne se doit de posséder au moins un exemplaire de chaque catégorie.

<img alt="Poignée de main" src="http://media.24joursdeweb.fr/2014/12/poignee-de-main-383x430.jpg" srcset="http://media.24joursdeweb.fr/2014/12/poignee-de-main-383x430.jpg 383w, http://media.24joursdeweb.fr/2014/12/poignee-de-main.jpg 663w" width="383" height="430" />

« J’ai dépassé mon objectif de 42 régies pub sur la home, et toi ?
— Je viens de découvrir 4 fournisseurs d’<i lang="en">A/B testing</i>, ça part en prod demain »

Les solutions techniques non bloquantes sont à négocier d’abord en interne pour faire prendre conscience du problème, puis auprès des fournisseurs, de moins en moins réfractaires. On arrive fréquemment à mettre les publicités dans des <i lang="en">iframes</i> et les scripts de <i lang="en">tracking</i> en asynchrone. Pour l’<i lang="en">A/B testing</i> ou certaines zones de publicité vitales, une bonne option, techniquement complexe, consiste à rapatrier en local, de préférence de manière automatisée, le code <abbr>JS</abbr> du prestataire, puis à l’inclure comme le reste des <abbr>JS</abbr> critiques en haut de page.
<h3>Les 3 caches</h3>
C’est comme les 3 coquillages : on peut faire sans, mais les gens du futur se moqueront. Partant du principe qu’il n’y a pas plus rapide qu’une requête qu’on ne fait pas, il convient de maîtriser rapidement les 3 techniques suivantes sur mobile.
<h4>Le cache <abbr title="Hypertext transfer protocol, Protocole de transfert hypertexte">HTTP</abbr></h4>
Depuis toujours la recette est simple :
<ul>
  <li>mettre des temps de cache très longs sur toutes les ressources statiques (plus d’1 mois),</li>
  <li>en cas de mise en production d’une nouvelle version, la mise à jour des caches clients se fait par le changement des <abbr title="Universal resource locator, emplacement de ressource unique">URL</abbr>s pointant sur les statiques.</li>
</ul>
Point.

Sur navigateurs de bureau, on pourrait s'arrêter là mais sur mobile le cache <abbr>HTTP</abbr>peut se révéler très volatile, voire capricieux. Si l’<abbr title="Operating system, Système d'exploitation">OS</abbr> estime qu’il a besoin de mémoire, il peut vider le cache du navigateur. C’est la raison pour laquelle certains sites ont développé leur propre système de mise en cache, en se basant sur <i lang="en">localStorage</i>.
<h4><i lang="en">DOM Storage</i></h4>
<a href="https://developer.mozilla.org/en-US/docs/Web/Guide/API/DOM/Storage" hreflang="en"><i lang="en">localStorage</i></a> est un système simplissime de stockage de clé / valeur, de 5<abbr>Mo</abbr>minimum. Pour limiter les accès au serveur, stockez-y des informations aussi simples qu’un historique de navigation ou lourdes et complexes, comme une liste de toutes les marques automobile et des modèles. L’utilisation en est tellement facile que c’en est ennuyeux.

Du coup certains en abusent.

<a href="http://media.24joursdeweb.fr/2014/12/localstorage-google.jpg"><img alt="localStorage sur Google" src="http://media.24joursdeweb.fr/2014/12/localstorage-google.jpg" width="795" height="320" /></a>

Tiens, du <abbr>CSS</abbr> dans mon <i lang="en">localStorage</i>

Certains sites stockent carrément du <abbr>HTML</abbr>, du <abbr>CSS</abbr>, du JavaScript, des images ou des <i lang="en">fonts</i> puis via un système classique de gestion de session évitent de renvoyer les ressources au client.
<h4><i lang="en">Application Cache</i> (dit <i lang="en">offline</i>)</h4>
L’énorme avantage ergonomique d’une application native installée chez l’utilisateur, c’est qu’elle va s’ouvrir quand vous appuyez dessus, même si à ce moment-là vous transitez par le tunnel sous la manche. Imaginez que l’on puisse faire de même avec nos sites : il suffirait d’aller une seule fois sur une des pages pour rapatrier l’ensemble de l’interface, et toute la navigation se ferait en local. Les mises à jour du contenu ne se feraient que lorsque la connexion est rétablie et on ne dépendrait pas de la validation d’un <i lang="en">store</i> pour mettre à jour son application.

Figurez-vous que la <a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Using_the_application_cache" hreflang="en">technologie existe</a>, et date même d’une époque lointaine où Apple ne pensait pas encore à l’<i lang="en">app store</i> et à Objective C pour écrire des applications mais bien au Web et ses langages . Faites-moi plaisir et enfourchez votre mobile pour aller sur l’<abbr>URL</abbr> suivante :

<a href="http://appcache.offline.technology/demo/" hreflang="en">http://appcache.offline.technology/demo/</a>

Une fois la page chargée complètement (scrollez en bas pour voir les <i lang="en">logs</i>), <strong>passez <i lang="en">offline</i></strong> (mode avion), puis allez sur la 2de page nommée cache. Non seulement la page s’affiche comme si de rien n’était mais l’image du machin vert est également présente alors qu’elle ne peut pas être dans le cache HTTP classique.

<img alt="Belle bête" src="http://media.24joursdeweb.fr/2014/12/tbrun5-430x312.png" srcset="http://media.24joursdeweb.fr/2014/12/tbrun5-430x312.png 430w, http://media.24joursdeweb.fr/2014/12/tbrun5.png 767w" width="430" height="312" />

Belle bête

Si vous utilisez Chrome, vous pouvez regarder sur <kbd>chrome://appcache-internals/</kbd>votre liste des sites utilisant cette technologie.

À ce stade de la compétition vous avez peut être déjà entendu dire <a href="http://alistapart.com/article/application-cache-is-a-douchebag" hreflang="en">beaucoup de mal</a>sur appCache. C’est effectivement le genre d’<abbr title="Interface de programmation applicative">API</abbr> qui demande du temps et de l’amour pour être bien comprise, mais elle apporte un énorme confort utilisateur et est très bien <a href="http://caniuse.com/#feat=offline-apps" hreflang="en">supportée</a> sur mobile et même sur bureau. Elle est très adapté aux sites qui se comportent comme des applications (une seule page) mais il existe des techniques à base d’<i lang="en">iframe</i> pour partager ce super cache entre plusieurs pages d’un site plus classique.
<h3>Les images</h3>
Elles représentent fréquemment les deux tiers du poids des sites et ralentissent le chargement des ressources critiques.
<h4>Éviter de les charger</h4>
Captain Obvious a dit : c’est plus rapide sans image. C’est pas faux et il commence à y avoir pas mal de techniques permettant d’éviter des requêtes :
<ul>
  <li><abbr>CSS</abbr>3 pour les dégradés, arrondis, rotations et opacités. Dommage que ça ne soit plus à la mode.</li>
  <li>Les caractères unicodes des polices fournies avec les <abbr>OS</abbr> : ►★✓ ⇨ (attention si vous visez également <abbr>IE</abbr>8 sur Windows XP)</li>
</ul>
Lorsque certaines images <strong>sont critiques pour votre interface</strong>, vous pouvez les embarquer directement dans le <abbr>HTML</abbr> ou le <abbr>CSS</abbr>. C’est la technique du <a href="http://css-tricks.com/data-uris/" hreflang="en"><code lang="en">data:uri</code>couplé à l’encodage en base64</a>. Oui ça a l’air sale mais uniquement si c’est mal fait.

L’image la plus importante à charger est celle indiquant justement qu’il y a chargement ! Pourquoi ne pas mettre également le logo ou une image de contenu réellement importante.

<a href="http://media.24joursdeweb.fr/2014/12/loading-pmvc.jpg"><img alt="Chargement du site Promovacances" src="http://media.24joursdeweb.fr/2014/12/loading-pmvc-860x127.jpg" srcset="http://media.24joursdeweb.fr/2014/12/loading-pmvc-860x127.jpg 860w, http://media.24joursdeweb.fr/2014/12/loading-pmvc-430x63.jpg 430w, http://media.24joursdeweb.fr/2014/12/loading-pmvc.jpg 1500w" width="860" height="127" /></a>

L’image d’attente arrive bien avant toutes les autres

Il ne faut pas abuser de la technique car elle alourdit le <abbr>HTML</abbr> et le <abbr>CSS</abbr> qui sont des ressource critiques, mais sur quelques petites images stratégiques cela fait patienter l’utilisateur.
<h4>Chargement à la demande</h4>
C’est à se demander pourquoi les navigateurs ne le font pas déjà d’eux-mêmes : l’idée est bêtement de <strong>ne pas charger les images qui ne sont pas visibles </strong>! C’est radical pour les sites avec beaucoup d’images car cela libère la bande passante pour les ressources critiques et les images qui sont visibles. Voici un <a href="https://github.com/vvo/lazyload" hreflang="en">bon exemple de librairie</a>, utilisé notamment sur lequipe.fr, bureau ou mobile. Naviguez dessus (c’est pour le travail) et scrollez très vite : vous verrez les images apparaître au fur et à mesure de votre progression dans la page.
<h3>Cette technique seule sauve des dauphins tous les jours.</h3>
<h3>Les images <i lang="en">responsive</i></h3>
Comment servir le meilleur rapport poids / qualité à l’utilisateur mobile ?
<h4>Le standard</h4>
Votre plus grand problème concernant les images à délivrer sur mobile est de comprendre ce que vous voulez réellement en faire. Le problème est de définir le problème si vous voulez, en répondant à ces 4 questions :
<ul>
  <li>adaptation à la taille du <i lang="en">viewport</i> : veut on servir des petites images aux petits écrans ?</li>
  <li>écran haute densité de pixels (ok, retina©) : veut-on leur servir des images de très haute qualité ?</li>
  <li><i lang="en">art direction</i> : pourquoi ne pas afficher des images au cadrage différent en fonction de la taille de l’écran ?</li>
  <li>format de l’image : veut-on servir du WebP à Chrome, du <abbr>JPG</abbr> <abbr>XR</abbr> à windows et du <abbr>JPG</abbr> 2000 à iOS ?</li>
</ul>
Pour compliquer l’affaire, sachez que :
<ul>
  <li>un écran retina peut avoir un petit <i lang="en">viewport</i>,</li>
  <li>deviner la bande passante de l’utilisateur est hautement hasardeux,</li>
  <li>vous ne connaissez pas la seule valeur qui compte : la taille physique de l’écran et la distance écran-œil.</li>
</ul>
Si vous êtes capable d’écrire un cahier des charges répondant à ces questions, alors vous pouvez vous pencher sur la réponse officielle et standard au problème : &lt;<a href="https://dev.opera.com/articles/responsive-images/" hreflang="en"><code lang="en">picture</code></a>&gt;. Le temps que ça marche partout, la librairie <abbr>JS</abbr> officielle est <a href="https://github.com/scottjehl/picturefill" hreflang="en">picturefill 2.0</a>.

<img alt="La balise &lt;picture&gt;" src="http://media.24joursdeweb.fr/2014/12/picture-430x257.jpg" srcset="http://media.24joursdeweb.fr/2014/12/picture-430x257.jpg 430w, http://media.24joursdeweb.fr/2014/12/picture-860x514.jpg 860w, http://media.24joursdeweb.fr/2014/12/picture.jpg 911w" width="430" height="257" />

La question est complexe, la réponse aussi.

La peinture étant un peu fraîche, il n’y a pour l’instant pas de retour sur la mise en production de cette technique. L’amélioration de performances devra donc se tester chez vous, en particulier sur les plateformes sans <a href="http://caniuse.com/#search=picture">support</a>.
<h4>Fait à la main, roulé sous les aisselles</h4>
Difficile pour une solution générique de faire mieux que du code écrit spécifiquement. À force j’utilise généralement chez mes clients un petit ensemble de techniques :
<ul>
  <li><abbr>JPG</abbr> grande résolution, qualité 0 : pour une image unique à faire passer sur tous type d’écran, éditée à la main. Ça passe pour 90% des <abbr>JPG</abbr>. (<a href="http://bit.ly/jpg-0">à essayer</a> sur un écran haute densité),</li>
  <li>images basse définition pour remplir l’espace, suivies du chargement de la haute définition. Le <abbr>JPG</abbr> progressif marchant mal sur iOS, c’est une manière d’avoir le même effet,</li>
  <li><i lang="en">lazy-loading</i> pour la majorité des images, images embarquées pour les critiques,</li>
  <li>quand on peut, des formats vectoriels : <abbr title="Scalable vector graphics, images vectorielles scalables">SVG</abbr> et polices d’icônes.</li>
</ul>
Combinez, testez et saupoudrez de <abbr>JS</abbr> pour obtenir de beaux effets. La maintenance n’est pas facile mais les résultats sont bons.
<h3>Interfaces fluides</h3>
Vous avez remarqué que les smartphones sont capables de jouer de manière fluide des jeux 3D mais ont des difficultés dès qu’il s’agit de retailler des <code>div</code>s ou de jouer avec du texte ? Au contraire de la 3D, il n’existe pas de puce dédiée à recalculer un <abbr title="Document object model, Modèle d'ojet document">DOM</abbr> qui bouge ou l’écoulement des caractères. Il va donc falloir aider un peu nos pages.
<h4>Animations</h4>
Elles peuvent être fluides si on les travaille au corps. D’abord il y a un certain nombre de propriétés CSS qu’il vaut mieux ne pas animer comme <i lang="en">top</i>, <i lang="en">left</i>, <i lang="en">width</i> et <i lang="en">height</i>. On leur préférera les variations de <i lang="en">transforms</i> comme <i lang="en">translateX()</i> ou <i lang="en">scale()</i> qui ont la bonne idée d’être pris en charge directement au niveau du processeur graphique.

Vous pouvez essayer dans l’ordre :
<ul>
  <li>les <a href="https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Using_CSS_transitions" hreflang="en">transitions <abbr>CSS</abbr></a>, qui suffisent à la plupart des sites,</li>
  <li>les <a href="https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Using_CSS_animations" hreflang="en">animations <abbr>CSS</abbr></a>, quipermettent des effets plus avancés</li>
  <li>enfin si <abbr>JS</abbr> doit tout piloter (un jeu par exemple), utilisez <code lang="en">requestAnimationFrame</code>, <strong>arrêtez jQuery animate</strong> et préférez des librairies spécialisées comme D3.js, GSAP ou TweenJS qui génèrent du <abbr>CSS</abbr>3.</li>
</ul>
<h4><abbr>CSS</abbr> 3 avec modération</h4>
Remplacer les images de décoration par <abbr>CSS</abbr> c’était une bonne idée jusqu’à ce que vous réalisiez que le scroll lag sérieusement depuis que vous avez rajouté une ombre portée sur toute la hauteur de votre liste. De fait, tous les effets d’ombre, de transparence et de coins arrondis sont calculés en continu et cela peut coûter cher.

Une seule règle : tester, et sur de vrais mobiles bien sûr.
<h4>Calculs</h4>
On ne calcule pas tous les jours une suite de Fibonacci mais si ça vous arrive, utilisez les <i lang="en">Web Workers</i>. Sur les mobiles modernes ça vous donne accès à un second processeur pour exécuter du JavaScript. Sur les autres, la bonne veille technique du <code lang="en">setTimeout(0)</code> reste valable pour exécuter des boucles lourdes sans bloquer l’interface. Devinez qui vous a écrit <a href="https://gist.github.com/jpvincent/867010d224d61a6539d3">un script</a> pour ça ?
<h4>Testez !</h4>
Les derniers navigateurs mobiles acceptent enfin le déboguage à distance ! Cela signifie que vous allez pouvoir utiliser le <i lang="en">profiler</i> que vous connaissez sur de vrais téléphones et débusquer les endroits qui font ramer votre interface.

<a href="http://media.24joursdeweb.fr/2014/12/profiler.jpg"><img alt="Vue réseau" src="http://media.24joursdeweb.fr/2014/12/profiler.jpg" width="717" height="318" /></a>

Oh la belle verte : c’est mon <abbr>CSS</abbr> qui est gourmand

Pour les anciens <abbr>OS</abbr> comme Android 2.3, cela reste la devinette, rassurez vous.
<h2>Impliquer, mesurer, surveiller</h2>
On ne peut pas résoudre le problème qu’on ne voit pas. Avant de commencer tout projet de performance, il faut <strong>définir</strong> ce que doit être la performance, prendre un point de départ, choisir ses métriques, et surveiller automatiquement les performances dans le temps.

Sinon le projet performance s'arrêtera après la mise en prod des premières améliorations de performance.

Il fut un temps où il était possible d’avoir une équipe Web composée d’une seule personne : référencement, accessibilité, code, design, administration système, c’était fun. Mais aujourd’hui, à toutes vos compétences techniques il faut maintenant rajouter une grosse capacité à comprendre pourquoi l’on code et comment dialoguer avec le reste de la boite.

Donc par pitié, ne partez pas en croisade solitaire contre les lenteurs de votre site, au risque que personne dans la boîte ne s’en aperçoive. Il faut d’abord sensibiliser et évangéliser sur le coût du manque de performance pour la société et l’image du produit. Toute l’introduction de cet article est là pour cela.

Une fois que vous avez l’oreille des décideurs, faites écrire dans ce qui se rapproche le plus d’une spécification les objectifs de performance : en dessous de quels temps considère-t-on qu’il faut agir ? Vous pouvez par exemple prendre ces chiffres, relativement universels sans être trop ambitieux :
<ol>
  <li>Pour 80% des utilisateurs</li>
  <li>Premier rendu en moins de 2 secondes</li>
  <li>Fonctionnalité principale en moins de 5 secondes</li>
  <li>Navigation de page en page en moins de 2 secondes</li>
</ol>
<h3>Les utilisateurs et le premier rendu</h3>
Les deux premiers points demandent à déterminer l’équipement des utilisateurs :
<ul>
  <li>Quels navigateurs ?</li>
  <li>Quelle est la puissance des mobiles ?</li>
  <li>Quelle est la qualité de leur connexion ? (l’origine géographique peut suffire à la deviner)</li>
</ul>
Les seuls moments où on vous a dit que le site était lent, c’est lorsque vos collaborateurs ont essayé de l’utiliser sans le Wifi du bureau. Comme des vrais gens donc. Il faut reproduire ces conditions de manière systématique et certaine pour constater les dégâts et juger des progrès avec le temps.

Le temps de premier rendu est relativement facile à vérifier avec des outils comme WebPagetest.org. Ce qui est plus compliqué c’est de paramétrer WebPagetest avec une simulation de connexion qui soit réaliste par rapport à vos utilisateurs. Comme ce n’est pas le cas de Webpagetest.org par défaut, je vous donne mes paramètres beaucoup plus représentatifs de la France.

<a href="http://media.24joursdeweb.fr/2014/12/connexion-webpagetest.png"><img alt="Connexions WebPageTest" src="http://media.24joursdeweb.fr/2014/12/connexion-webpagetest.png" width="462" height="91" /></a>

Des paramètres de connexion plus réalistes.

WebPagetest s’installe également en interne, et peut <a href="https://code.google.com/p/mobitest-agent/" hreflang="en">piloter des mobiles</a>.
<h3>La fonctionnalité principale</h3>
Là ça se complique : comment <strong>déterminer le temps d’accès à la fonctionnalité principale</strong> ? Aucun outil ne peut faire cela automatiquement pour vous car seul vous connaissez bien votre interface (prends ça <a href="http://en.wikipedia.org/wiki/Skynet_%28Terminator%29" hreflang="en">Skynet</a>). Par contre vous pouvez collecter automatiquement des temps en JavaScript et vous les envoyer dans l’outil de tracking qui vous sied (G. Analytics peut suffire au début).

Si vous êtes un site de news ou à contenu visuel, trackez la fin de téléchargement de la première image visible utile par exemple (tiens, <a href="https://github.com/jpvincent/requestTracker">voici du code</a> qui peut vous y aider). Si votre fonctionnalité principale dépend de JavaScript (vidéo, moteurs de recherche complexes, application…), il est encore plus facile de minuter le moment où le contenu devient interactif.
<h3>La navigation interne</h3>
Le dernier point consiste à <strong>ne pas décevoir l’utilisateur après la première page</strong>, que ce soit la vitesse de chargement des pages suivantes ou la fluidité de l’interface.

Là vous pouvez surveiller les taux de mise en cache client (utilisation avancée de <a href="http://fr.slideshare.net/jpvincent/le-monitoring-de-la-performance-front">WebPagetest Monitor</a>) et simuler des navigations dans vos tests de performance.

<a href="http://media.24joursdeweb.fr/2014/12/cache-F.png"><img alt="Note de F en Cache Static Content" src="http://media.24joursdeweb.fr/2014/12/cache-F.png" width="59" height="93" /></a>

Attention chérie ça va ramer

Il n’y a pas de moyen facile et automatique de surveiller que l’interface elle-même est fluide sur mobile : contentez-vous d’une vérification systématique et digitale.
<h2>Stratégies de chargement</h2>
Il faut jouer avec la perception utilisateur et afficher quelque chose d’utile le plus rapidement possible. Cela demande à s'asseoir autour d’un café avec les autres équipes et à écrire la liste des priorités d’une page. Charge à vous de traduire cela en code pour régler l’ordre dans lequel les ressources vont être chargées par le navigateur.

Prenons 2 exemples de sites qui ont fait ce travail de priorité des requêtes.
<h3>Home Google</h3>
La fonctionnalité principale est évidemment l’affichage du champ texte. Mais les fonctionnalités secondaires sont légion : <i lang="en">autocomplete</i> sur ce champ, l’obligatoire intégration Google+, le menu, la suggestion de géolocalisation et même une bannière d’auto-promotion.

<a href="http://media.24joursdeweb.fr/2014/12/hp-google.jpg"><img alt="Home de Google" src="http://media.24joursdeweb.fr/2014/12/hp-google.jpg" width="241" height="428" /></a>

Ils ont de l’avenir

Google n’attend pas : il embarque un <abbr>CSS</abbr> minimaliste et suffisant dans le <abbr>HTML</abbr>, le <abbr>HTML</abbr> ne contient que le champ et des éléments pour définir la structure de la page. <strong>En 1 requête et 22<abbr>Ko</abbr>, soit l’équivalent de 2 allers-retours réseau, la fonctionnalité principale est déjà utilisable</strong>. Ensuite arrivent plus de 200<abbr>Ko</abbr> de ressources diverses pour afficher et rendre fonctionnel le reste de la page.
<h3>Home avec carrousel massif</h3>
Cette homepage était principalement vue depuis la Chine, il a donc fallu la calibrer pour des petits débits / grosses latences : elle était déjà optimisée pour le mobile. Par contre il fallait également afficher dans un carrousel plusieurs images de haute qualité de 1200 pixels de large.

<a href="http://media.24joursdeweb.fr/2014/12/hp-cma.jpg"><img alt="Home de CMA" src="http://media.24joursdeweb.fr/2014/12/hp-cma.jpg" width="236" height="344" /></a>

Le carrousel classique

Du point de vue du réseau, charger plusieurs ressources en parallèle est généralement vu comme une bonne idée. Sauf lorsque le débit est ténu et les ressource trop grosses comme c’est le cas ici. Le chargement de plusieurs images en simultané ralentissait le chargement des ressources du chemin critique.

Il a donc fallu prioriser : logo, texte, et image principale d’abord.

Les fonctionnalités secondaires sont les icônes, un moteur de recherche (non visible ici) les <strong>autres images du carrousel</strong> et des images bien plus bas.

Adieu donc la police, utilisation de <code>data:uri</code> pour embarquer le logo en base64, inclusion en <code>&lt;img src&gt;</code> traditionnel de la première image du carrousel (et seulement elle) et on développe un petit script pour <strong>aller chercher de manière asynchrone les photos suivantes</strong> avant de vraiment démarrer le carrousel.
<h2>Conclusion : <i lang="en">Mobile first</i></h2>
Ça n’est pas du lâcher de <i lang="en">buzzword</i> gratuit mais bien une adaptation à la manière dont nos utilisateurs utilisent nos sites, qu’ils soient en situation de mobilité, dans un pays lointain ou qu’ils fassent partie des 20% de français avec moins de 2<abbr>Mb</abbr>/<abbr>s</abbr>.

Le <i lang="en">mobile first</i> est l’héritier de la pensée « amélioration progressive » : délivrons très rapidement la promesse initiale, chargeons les fonctionnalité supplémentaires après et en fonction des capacités du client. Cela oblige à se poser la vraie question : quelles sont les priorités de chaque page ?

Cela s'intègre parfaitement avec un processus de conception moderne où designer et intégrateur web passent pas mal de temps l’un à côté de l’autre pour fignoler les maquettes sur les mobiles.

Nous avons évoqué une palanquée de techniques : certaines sont connues depuis bientôt 10 ans, beaucoup émergent et certaines sont devenues dangereuses. Toutes répondent à une situation particulière donc c’est à vous de <strong>tester ce qui marche ou pas</strong>, page à page.

Le processus de travail est presque aussi important que les techniques individuelles : la communication et l’effort de groupe sont vitaux pour maintenir la qualité à travers le temps. Sans monitoring ou automatisation des déploiements cela va rester amateur et pénible. Sans soutien hiérarchique du client (interne, externe, hiérarchique, bref celui qui vous paye) vous n’irez pas bien loin seul. Ou pire vous serez frustré par ce métier pourtant formidable qui est le nôtre.
