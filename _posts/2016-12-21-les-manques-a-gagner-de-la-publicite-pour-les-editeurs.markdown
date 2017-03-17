---
layout: post
title:  "Les manques à gagner de la publicité pour les éditeurs"
date:   2016-12-21 16:04:21 +0100
---

<blockquote>Cet article et <a href="http://braincracking.org/?p=1197">le précédent, plus technique, sur les tiers classiques</a> sont une transcription d’une conférence donnée à Blend Web Mix 2016. <a href="https://www.youtube.com/watch?v=7tbTm1Jo0gE">Cliquez ici si vous êtes plus audio que texte</a>.</blockquote>
<h1>La publicité et les performances</h1>
Mettre "pub" et "perf" dans la même phrase fait rire jaune tout travailleur du Web. Le peu de fois où nous désactivons nos adblockers, nous nous demandons comment font "les gens" pour arriver à naviguer. Le peu de fois où nous testons le site sur autre chose que nos mobiles personnels, généralement performants, nous prenons notre mal en patience car c’est pour le travail. Les vrais utilisateurs eux, sont déjà partis.
<h2>Performance perçue et Performance business</h2>
Il y a des palanquées de chiffres démontrant la relation business/temps de chargement, regardez ne serait-ce que cette <a href="https://wpostats.com/tags/2016/">collection 2016 chez wpostats</a> (communautaire) ou ces <a href="https://www.dareboost.com/fr/webperf-impacts">traductions en français chez Dareboost</a> (solution commerciale). Laissez moi vous sortir quelques chiffres de chez mes clients.

[caption id="attachment_1229" align="alignnone" width="1091"]<a href="http://braincracking.org/wp-content/uploads/2016/12/onfocus.png"><img class="size-full wp-image-1229" alt="courbe de nombre de pages vues par session pour chaque temps de chargement" src="http://braincracking.org/wp-content/uploads/2016/12/onfocus.png" width="1091" height="509" /></a> une très classique relation engagement / temps de chargement[/caption]

Cette courbe (réalisée avec le produit commercial <a href="http://webperf.io/">webperf.io</a>) est assez classique et range les utilisateurs par temps de chargement (axe horizontal) et indique pour chaque segment le taux d’engagement (plus c’est haut, plus il consomme). Ici c’est un site de presse, donc c’est le nombre de pages consommées par session. Comme souvent, on voit que les utilisateurs sont moins consommateurs lorsque leurs pages se chargent en plus de 5 secondes. Puis au-delà de 20 secondes, la courbe s'aplanit, car ne restent que les fidèles et les motivés. Je précise que j'ai généré cette courbe en filtrant pour n’avoir que les utilisateurs Windows, afin de ne pas être influencé par les mobiles.

Autre constat en fouillant les chiffres :
<blockquote>Les utilisateurs avec adblock ont un engagement 50% supérieur aux autres</blockquote>
Quand on sait que la majorité des temps de chargement sont consacrés à la seule publicité, il devient évident que cette source de revenus a un coût qui dépasse de loin ce qui est écrit sur le contrat avec la régie.
<h2>Comment louper 85% des internautes français</h2>
Avec quoi vos utilisateurs mobiles naviguent-ils ? Vous trouverez sans doute dans vos analytics les modèles de smartphones et tablettes. Prenez le top 25 et allez récupérer pour chaque modèle son prix de vente actuel, occasions comprises (surtout pour les iPhones). Chez 3 clients (2 journaux, 1 site de e-commerce) j’obtiens à peu près la même valeur moyenne : 380€.
En voyant ces stats, je me suis longtemps dit que les français étaient plutôt privilégiés par rapport à leurs voisins et pouvaient mettre de beaux budgets dans leurs smartphones (merci Free ?). Jusqu'à ce que je trouve les prix d’achat du marché français des smartphones :

[caption id="attachment_1232" align="alignnone" width="663"]<a href="http://braincracking.org/wp-content/uploads/2016/12/marche-mobile.png"><img class="size-full wp-image-1232" alt="graphique de répartition du prix d'achat des mobiles dans la population française" src="http://braincracking.org/wp-content/uploads/2016/12/marche-mobile.png" width="663" height="633" /></a> prix d’achat des mobiles en 2015[/caption]

L’étude de marché <a href="http://www.challenges.fr/high-tech/les-grandes-tendances-du-marche-des-smartphones-en-france_60272">est faite par GFK</a> pour le 1er semestre 2015 et montre clairement que <strong>la population qui possède des portables de plus de 300€ ne représente que 15% des français</strong> … La médiane est plutôt comprise entre 100 et 150€.
Utilisent-ils le Web ? Oui puisque d'après l‘<a href="http://www.arcep.fr/index.php?id=13365#c94750"><abbr title="Autorité de Régulation des Communications Électroniques et des Postes">ARCEP</abbr></a> 70% des cartes SIM utilisent la 3G. Même en supposant que seuls les possesseurs des smartphones les plus chers achètent de la 3G, cela signifie qu’il n’est pas rare d’avoir des mobiles de 75-100€ accéder au Web.
Pourquoi ne les voyez-vous pas sur vos sites ? Tentez de charger une page de journal avec un mobile ou une tablette à moins de 200€ ! Je pense que les gens accèdent bien au Web mais que l’expérience d’un site chargé en pub est tellement pénible qu’ils restent sur les applications de Facebook ou Google (où la pub est parfaitement sous contrôle d’ailleurs).

Pour résumer, laisser ses pubs faire n’importe quoi, c’est se couper de la majorité du parc mobile français.
<h2>Pub lourdes ? Essayez avec moins!</h2>
Il y a des stratégies intéressantes pour laisser plus d’utilisateurs devant des publicités plus visibles. Elles jouent sur les modes de facturation de la pub (impression ou visibilité garantie), sur la notion de valeur moyenne de l’inventaire et sur la réalité technique qui est que moins on affiche de pubs, plus ça va vite, et plus l’utilisateur consommera de pages, donc d’affichage de pubs.
Prenons par exemple le journal anglais Guardian.
[video width="2624" height="1690" webm="http://braincracking.org/wp-content/uploads/2016/12/Guardian-pub-differee-peu-nombreuses-et-sticky.webm"][/video]
<a href="http://braincracking.org/wp-content/uploads/2016/12/Guardian-pub-differee-peu-nombreuses-et-sticky.mp4">Lien direct vers la vidéo en MP4</a>.
Sur un réseau que j’ai artificiellement ralenti avant de filmer l’écran, on voit les priorités données :
<ul>
  <li>on affiche d’abord texte et photo principale, c’est ce qui fait que l’utilisateur va rester</li>
  <li>notez qu’un espace blanc en haut de page est déjà prévu pour y afficher une bannière et éviter les désagréables effets de page qui saute</li>
  <li>dans la plus pure tradition de l’amélioration progressive, la plupart des liens sont déjà cliquables et on peut interagir avec le menu même si les fonctionnalités JavaScript n’ont pas encore été récupérées</li>
  <li>affichage progressif des fonctionnalités secondaires, comme les boutons de partage sur la gauche</li>
  <li>enfin, affichage de deux emplacements pub classiques en haut et à droite</li>
  <li>chose importante : aucune publicité en dehors du viewport ne commence à se charger, pour ne pas gêner le chargement des ressources plus visibles</li>
  <li>au scroll, la pub de droite reste bien visible</li>
</ul>
Du côté de chez Yahoo!, où la performance frontend a été inventée il y a 10 ans (j’y étais !), on peut aussi espérer qu’un soin particulier ait été apporté aux pubs.
[video width="2820" height="1518" mp4="http://braincracking.org/wp-content/uploads/2016/12/scroll-yahoo-et-pub-unique-sur-le-co%CC%82te%CC%81.mp4"][/video]
Sur la Homepage très chargée en informations, Yahoo! joue sur la visibilité d’une position en particulier (à droite) tandis que les autres positions sont remplies par des pubs très légères à gauche (Unicef) et au centre. Elles se confondent d’ailleurs avec le contenu, évitant au passage la <a href="https://en.wikipedia.org/wiki/Banner_blindness">"banner blindness"</a>, c’est à dire le masquage inconscient par l’utilisateur de tout ce qui ressemble à une pub.

En France, nous avons l’exemple de Libération :
[video width="2820" height="1598" mp4="http://braincracking.org/wp-content/uploads/2016/12/liberation-pub-col-droite-en-sticky1.mp4"][/video]
Encore une fois, moins de positions au global (mais ils pourraient faire mieux), une position à droite qui reste très visible, et j’ai l’impression qu’ils affichent surtout des publicités vidéo, qui ont l‘avantage de se vendre plus cher. Ce ne sont pas mes clients (du coup ils n’ont peut être justement pas besoin de moi :) ), donc je ne sais pas si c’est rentable pour eux au final, mais en tout cas l’expérience utilisateur est plus agréable et en allant sur leur Une, l’identité visuelle du site se démarque quelque peu des autres sites de presse.

<strong>En résumé</strong> : on réduit l’inventaire, on y met des formats différenciants ou légers, et dans tous les cas on maximise la visibilité d’une position en particulier. Cette dernière étant vendue plus chère, elle compensera les pertes sur les positions disparues qui étaient vendues à l‘impression. L’amélioration de l’expérience utilisateur et l’ouverture à une gamme plus large de mobiles devraient remonter le nombre de pages vues.
Le fait d’avoir moins d’inventaire permet aussi aux équipes de se concentrer sur la qualité de ce qui est diffusé, ce qui est à mon avis un gros plus pour l’image de marque.
<h1>La pub et la qualité</h1>
<h2>L’image de marque</h2>
Une mauvaise publicité ne dégrade-t-elle pas l’image de marque des éditeurs ? Ça semble évident et pourtant je n’ai pas réussi à trouver d'étude sur ce sujet. Les éditeurs historiques devraient protéger leur image de marque, dont la crédibilité est encore un critère qui fait cliquer sur un article par exemple, mais on sent bien qu’ils ont partiellement abandonné la partie !
Rassurez-moi, il n’y a pas que moi qui pense que les services de "content discovery" (en France Outbrain, Ligatus ou Taboola) mettent à mal l’image de sérieux des titres de presse ?

[caption id="attachment_1233" align="alignnone" width="1702"]<a href="http://braincracking.org/wp-content/uploads/2016/12/mauvaises-pubs.png"><img class="size-full wp-image-1233" alt="captures d'écran de pubs sans liens avec le contenu" src="http://braincracking.org/wp-content/uploads/2016/12/mauvaises-pubs.png" width="1702" height="1118" /></a> À gauche : "Le Monde" et Outbrain,<br />À droite "Le Parisien" et Taboola[/caption]

Je n’ai pas eu à me forcer pour trouver sur "Le Monde" sous un article à propos du traité CETA la mise en avant d’articles aux sujets … variés : femme pulpeuse, mal au dos, fabriquer un vélo, Madonna VS Trump … Et pour "Le Parisien", les thèmes sont petites culottes, photos en bikini et poids des stars, juste sous deux articles à propos du sexisme… Oh Ironie.

Avoir une marque c’est aussi avoir une identité visuelle forte.

[caption id="attachment_1235" align="alignnone" width="1299"]<a href="http://braincracking.org/wp-content/uploads/2016/12/charte.png"><img class="size-full wp-image-1235" alt="Extrait des slides de la conférence de Renaud Forestié sur le redesign des sites de presse" src="http://braincracking.org/wp-content/uploads/2016/12/charte.png" width="1299" height="779" /></a> Extrait des slides de la <a href="https://vimeo.com/136370421">conférence de Renaud Forestié</a> sur le redesign d’un site de presse[/caption]

Pas facile de se démarquer des autres quand il faut respecter la charte des régies : avoir une largeur de 1000 pixels (à 20 près), un contenu forcément centré et un pavé de droite visible dans les 800 premiers pixels. Compliqué également pour ces journaux de tenter des formats nouveaux, ce qui est un manque à gagner quand on sait que l’introduction de nouveaux formats augmente toujours le taux d’interaction pub (<a href="https://www.google.fr/url?sa=t&amp;rct=j&amp;q=&amp;esrc=s&amp;source=web&amp;cd=1&amp;cad=rja&amp;uact=8&amp;ved=0ahUKEwjK_93JuKLOAhXBWBoKHS5CC9IQFggeMAA&amp;url=http%3A%2F%2Fwww.iabfrance.com%2Fsystem%2Ffiles%2Fefficacite_digitale_etude_millward_brown_iab_06.2010v.publique.pdf%3Fdownload%3D1&amp;usg=AFQjCNG93wHhZeW92MXzlwlQQI4WiTGtUw&amp;sig2=k8VvsPw-kKesshK_27gevw">source</a>).
<h2>Qui paye la non-qualité des pubs ?</h2>
Quand on arrive de l’extérieur comme moi, c’est compliqué de comprendre comment les régies et les fabricants de pub peuvent se permettre de diffuser les choses suivantes.
<h3>Des virus !</h3>
[caption id="attachment_1246" align="alignnone" width="712"]<a href="http://braincracking.org/wp-content/uploads/2016/12/msn.png"><img class="size-full wp-image-1246" alt="Titre d’article : MSN diffuse des virus via la publicité" src="http://braincracking.org/wp-content/uploads/2016/12/msn.png" width="712" height="386" /></a> Quand la pub diffuse des virus, c’est l’éditeur qu’on accuse.[/caption]

Régulièrement des pubs vérolées sont diffusées, y compris par <a href="https://blog.malwarebytes.com/threat-analysis/2016/01/msn-home-page-drops-more-malware-via-malvertising/">des grands éditeurs comme Yahoo! ou MSN</a>. Les virus exploitent les failles des navigateurs non patchés pour s’installer automatiquement sur les machines. Ne croyez pas que ça ne concerne que les vieux <abbr title="Internet Explorer">IE</abbr> sur <abbr title="Windows XP">XP</abbr>, iOS 10.0 a récemment été vulnérable à ce même genre d’attaques : <a href="https://support.apple.com/en-us/HT207271">le simple affichage d’un JPEG permettait de l’infecter</a>. Il est donc rentable pour des groupes criminels de pirater des serveurs de pub ou simplement d’acheter des emplacements avec très peu de budget (RTB + ciblage navigateur). Notez que dans ces cas là, c’est l’image de marque du diffuseur qui prend, pas la régie ni ses prestataires.
<h3>Des pubs qui ne marchent pas</h3>
Au hasard de mes audits de WebPerf qui se font sans adblocker (moment toujours pénible), il est fréquent de tomber sur des pubs qui non seulement font ramer le site mais qui en plus ne marchent pas. Comme cette pub qui tourne en boucle sur une erreur, fait plafonner le processeur mais ne réussira jamais à jouer la vidéo.

[caption id="attachment_1247" align="alignnone" width="1494"]<a href="http://braincracking.org/wp-content/uploads/2016/12/erreurs-pub.png"><img class="size-full wp-image-1247" alt="screenshot d’une pub vidéo en erreur" src="http://braincracking.org/wp-content/uploads/2016/12/erreurs-pub.png" width="1494" height="716" /></a> Non, cette pub ne s’affichera jamais, mais on va la facturer quand même.[/caption]
<h3>Des pubs loooooongues</h3>
D’autres fois il faut tellement de temps à une publicité pour s’afficher qu’on se demande comment quelqu’un espère gagner de l’argent grâce à elle.

[caption id="attachment_1248" align="alignnone" width="1350"]<a href="http://braincracking.org/wp-content/uploads/2016/12/vache-qui-rame.png"><img class="size-full wp-image-1248" alt="pub très longue à afficher (17 secondes)" src="http://braincracking.org/wp-content/uploads/2016/12/vache-qui-rame.png" width="1350" height="290" /></a> La Vache Qui Rame™[/caption]

Ici il a fallu 17 secondes à la marque pour s’afficher, sur une connexion tout à fait standard. Sur un article passe encore car il y a de bonnes chances que l’utilisateur soit encore là, mais sur une page type Home, résultat de recherche, il n’y a probablement plus personne pour voir la marque.
<h3>Qui paye pour ces pubs peu ou pas du tout visibles ?</h3>
Il y a deux cas de figure.

L’annonceur paye la régie pour un <strong>volume d’impressions</strong>. Techniquement tout se joue sur le moment où la comptabilisation de l’impression remonte. Dans le cas "Vache qui rit", il y avait plus de 12 secondes entre la comptabilisation par la régie et l‘apparition de la marque… Dans ces cas, c’est donc <strong>les annonceurs qui sont floués</strong>, mais ils doivent y être habitués puisque cela fait 20 ans que des pubs sont vendues à l’affichage même si aucun humain ne les voit (56% des pubs ne sont pas vues <a href="https://www.thinkwithgoogle.com/infographics/5-factors-of-viewability.html">d’après Google</a>).

L’annonceur achète avec une <strong>garantie de visibilité</strong>. La visibilité est, d’après le groupement des professionnels de la pub (<abbr title="Interactive Advertising Bureau ">IAB</abbr>), lorsqu’au moins 50% des pixels d’une pub sont dans le viewport navigateur pendant plus d’une seconde. Si la comptabilisation se fait honnêtement, c’est à dire après le chargement de la pub mais que celle-ci est trop lente ou en erreur, <strong>c’est l’éditeur qui a un manque à gagner</strong> en ayant fait une page vue non facturable.

<strong>La régie gagne à tous les coups…</strong>
<video width="540" height="300" src="http://braincracking.org/wp-content/uploads/2016/12/ramasse-tout.mp4" autoplay="autoplay" loop="loop" muted="" playsinline=""><object width="540" height="300" classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,40,0"><param name="src" value="http://braincracking.org/wp-includes/js/tinymce/plugins/media/moxieplayer.swf" /><param name="flashvars" value="url=/wp-content/uploads/2016/12/ramasse-tout.mp4&amp;poster=/wp-admin/" /><param name="allowfullscreen" value="true" /><param name="allowscriptaccess" value="true" /><embed width="540" height="300" type="application/x-shockwave-flash" src="http://braincracking.org/wp-includes/js/tinymce/plugins/media/moxieplayer.swf" flashvars="url=/wp-content/uploads/2016/12/ramasse-tout.mp4&amp;poster=/wp-admin/" allowfullscreen="true" allowscriptaccess="true" /></object></video>
Ce ne sont pas de grandes méchantes pour autant : elles sont réactives dès lors qu’on leur signale des publicités en erreur ou qui font planter le site par exemple, et ensuite des rapports de force polis s’installent comme dans toute relation commerciale. Mais disons qu’elles n’ont pas le besoin économique impératif de vérifier toutes les créations de tous leurs prestataires.
<h2>Quelles solutions pour les éditeurs ?</h2>
Le rapport de force étant en faveur des intermédiaires de la pub, qui ont bien d’autres canaux de diffusion, c’est clairement aux éditeurs de s’organiser pour ne pas périr étouffés.
D’abord en définissant des contraintes de qualité technique (côté éditorial, ça ne peut se faire qu’au cas par cas). Ça tombe bien, <abbr title="Groupement des professionnels de la pub américains">l’IAB US</abbr> a défini un <a href="http://www.iab.com/wp-content/uploads/2015/11/IAB_Display_Mobile_Creative_Guidelines_HTML5_2015.pdf">cahier des charges précis</a> où, pour chaque format, ils définissent des limites. Par exemple un Skyscraper ne doit pas dépasser 200Ko au démarrage, puis a le droit de récupérer 300Ko supplémentaires 1 seconde après le chargement de la page. Une vidéo doit se jouer en 24 images/seconde pendant 15 secondes max et un interstitiel animé ne doit pas dépasser 15 secondes.

[caption id="attachment_1253" align="alignnone" width="663"]<a href="http://braincracking.org/wp-content/uploads/2016/12/iab.png"><img class="size-full wp-image-1253" alt="De la contrainte nait la créativité … et une meilleure rentabilité" src="http://braincracking.org/wp-content/uploads/2016/12/iab.png" width="663" height="673" /></a> De la contrainte nait la créativité … et une meilleure rentabilité[/caption]

C’est dommage que les pros français de la pub n’en aient jamais entendu parler, mais encore une fois ce n’est pas l’univers bouillonnant des intermédiaires de la pub qui va s’auto-imposer des contraintes tant que leurs clients ne leur demande rien.
On peut aussi définir un seuil de temps processeur à ne pas dépasser et exiger le respect des actions techniques de base (compression, concaténation, mise en cache …). Que l’on utilise ces guidelines ou que chaque site les adapte à sa cible (typiquement, nos réseaux mobiles nous permettent d’être moins stricts qu’aux US), le tout est d’en avoir et de s’accorder dessus avec sa régie.

Une fois les règles définies, il faut passer à la phase de vérification, si possible automatisée donc permanente et systématique.
Je n’ai pas eu connaissance de solution, même commerciale, qui permettrait de vérifier automatiquement les pubs délivrées à partir d’un cahier des charges. Même pour surveiller les tiers de manière générale il n’y a pas vraiment d’offre complète, à tel point que je suis en train de construire ce genre de dashboard sur commande.

[caption id="attachment_1255" align="alignnone" width="711"]<a href="http://braincracking.org/wp-content/uploads/2016/12/stop-pub.png"><img class="size-full wp-image-1255" alt="Visualisation du poids servi par nom de domaine" src="http://braincracking.org/wp-content/uploads/2016/12/stop-pub.png" width="711" height="241" /></a> Visualisation du poids servi par nom de domaine. Tiens une pub de 3 Mo a arrêté d'être distribuée.[/caption]

On peut surveiller la consommation processeur, réseau, les temps de réponse des tiers… Pour certaines campagnes ciblées on pourrait récupérer les métriques de visibilité : savoir quand le logo de la marque est visible par exemple, vu depuis les postes utilisateur, information que certaines régies font payer. Lorsque le business model dépend des tiers, ce monitoring peut expliquer la variation de certaines métriques métier comme la baisse du nombre de vues le même jour qu’une défaillance partielle des serveurs de la régie.
À la fin, cela redonne matière à négocier un peu plus de qualité ou peut redonner confiance envers votre prestataire.
<h1>Conclusion</h1>
Nous avons vu <a href="http://braincracking.org/2016/12/21/combien-coute-un-service-tiers-gratuit/">dans un précédent article</a> que les tiers en général (pubs, widgets, trackers, services …) pouvaient vous coûter de l’argent pour des raisons de contre-performance perçue, de distraction, d’atteinte à l’image de marque ou présentaient un risque de fuite de données. Nous avions vu que les solutions techniques existaient mais qu’il faut se laisser le temps de les appliquer lors de l’intégration du tiers (principe "si c’est rapide à intégrer, c’est que ça va ramer").

Nous venons de voir que la publicité posait ses propres difficultés qui demandent bien plus que les solutions techniques classiques. Les éditeurs ont intérêt à rapidement définir des cahiers des charges et à s’équiper pour s’assurer qu’ils seront respectés pour stopper les pertes de revenus dues aux mauvaises qualités de certaines pubs d’une part, et à l’excès de position d’autre part.
Des éditeurs ont choisi des stratégies d’affichage des publicités qui améliorent grandement l’expérience utilisateur (moins d’emplacements, vendus plus cher) et potentiellement les revenus : qu’attendez-vous pour étudier le sujet ?
