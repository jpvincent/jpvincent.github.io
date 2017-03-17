---
layout: post
title:  "Quel est le débit moyen en France ?"
date:   2012-11-13 16:04:21 +0100
---
À quelle vitesse s’affiche un site ? La question est importante tant le confort d’utilisation et les revenus d’un site peuvent dépendre de cette réponse. Vous intégrez probablement déjà des pratiques visant à améliorer le rendu des pages et leur réactivité, et si vous êtes en phase d’industrialisation, vous faites peut-être du monitoring de la performance. Au moment de paramétrer votre solution, une question a dû vous poser problème : quelle est la connexion type en France ? Et pour les mobiles ?

Nous allons non seulement répondre à ces deux questions, mais en plus nous apercevoir qu’il y a une question plus pertinente à se poser.
<h2>Pourquoi et comment calibrer ses test ?</h2>
Les solutions de <em>monitoring</em> du marché testent les pages généralement sans vous dire avec quel navigateur (probablement des <em>Webkit headless</em>, pour accélérer les tests), et le font depuis des <em>datacenters</em> qui sont beaucoup trop proches des nœuds principaux du Web. Potentiellement, les sondes sont exécutées à partir de machines hébergées physiquement dans le même <em>datacenter</em> que votre hébergeur ou avec un <em>peering</em> privilégié avec le CDN.

Autant dire que les tests standard ne sont pas représentatifs de ce qu'endurent vos utilisateurs et c'est la raison pour laquelle on limite volontairement débit et latence, par exemple sur l'indispensable <a href="http://www.webpagetest.org/">WebPageTest</a> ou chez certaines solutions payantes. WebPageTest a le mérite d'être transparent car vous avez une liste de connexions pré-paramétrées cependant nous allons voir qu'elles ne représentent pas du tout la France.

[caption id="attachment_1142" align="alignnone" width="455"]<a href="http://braincracking.org/wp-content/uploads/2012/10/paramètre-WPT-org.png"><img class="size-full wp-image-1142 " title="paramètre WPT org" src="http://braincracking.org/wp-content/uploads/2012/10/paramètre-WPT-org.png" alt="" width="455" height="140" /></a> Paramètres par défaut de WebPageTest.org[/caption]

Côté transit réseau, il y a essentiellement 4 données qui influencent l'affichage d'un site :
<ul>
  <li>la latence ou ping, qui est le temps d'un aller / retour entre un navigateur et le serveur qui lui répond. Pour le <a href="http://httparchive.org/trends.php">site moyen</a>, en HTTP 1.1, basé sur TCP, c'est <a href="http://performance.survol.fr/2008/03/impact-de-la-latence-reseau/">la mesure la plus importante</a>. C'est aussi la plus difficile à obtenir.</li>
  <li>le download ou bande passante descendante, qui reste importante pour récupérer de gros objets. On nous vend des ADSL à plus de 20Mb/s (<a href="http://adsl.sfr.fr/toutes-nos-offres/">1</a>, <a href="http://abonnez-vous.orange.fr/residentiel/accueil/accueil.aspx">2</a>, <a href="http://www.free.fr/adsl/internet.html">3</a> et des mobiles à 5Mb/s, qu'en est il vraiment ?</li>
  <li>l'upload qui est rarement pertinent dans la webperf.</li>
  <li>la perte de paquets : pratiquement inexistante sur des connexion filaires, mais omniprésent sur mobile. Là par contre impossible de trouver des chiffres, en supposant qu'ils signifient quelque chose</li>
</ul>
Idéalement, il faudrait récupérer ces métriques de vos propres utilisateurs, grâce à du <a href="http://yahoo.github.com/boomerang/doc/use-cases.html">Real User Monitoring</a> (RUM), mais la mise en œuvre est moins aisée que de récupérer les informations de sources externes, telle que nous allons le faire ici.

Si vous êtes pressé ou si vous me faîtes aveuglément confiance (et je vous comprends), vous pouvez sauter directement à <a href="#resume">"En résumé"</a>
<h2>à la pêche aux chiffres</h2>
En cherchant <a href="http://delicious.com/braincracking">dans mes archives</a> et sur Google, je me suis rendu compte qu'il existait très peu de rapports rendus publics. J'en liste ici 6 en extrayant des données brutes. N'hésitez pas à m'en indiquer d'autres ou à décortiquer plus avant les sources.
<h3><a href="http://royal.pingdom.com/2010/11/12/real-connection-speeds-for-internet-users-across-the-world/">Etude Akamai / Pingdom de 2010</a></h3>
Akamai est le leader mondial des CDNs, autant dire qu'ils voient passer du traffic et ont une bonne idée de ce qui se fait à l'échelle mondiale. L'étude de cette année là comprend la répartition des débits en France.
<ul>
  <li>Débit moyen : 3,3 Mb/s</li>
  <li>Répartition :
<ul>
  <li>25 % moins de 2 Mb/s</li>
  <li>60 % entre 2 et 5 Mb/s</li>
  <li>15 % plus de 5 Mb/s</li>
</ul>
</li>
</ul>
<h3><a href="http://www.akamai.com/stateoftheinternet/">Akamai 2012 </a></h3>
Chiffres plus récents, mais dont l'échelle de répartition a changé.
<ul>
  <li>Débit moyen : 4,6Mb/s</li>
  <li>Débit moyen des réseaux mobiles : 2,8Mb/s</li>
  <li>Répartition :
<ul>
  <li>0,1% moins de 256Kb/s</li>
  <li>45% plus de 4Mb/s</li>
  <li>4% plus de 10Mb/s</li>
</ul>
</li>
</ul>
<h3>DegroupTest, <a href="http://www.degrouptest.com/publications/6/barometre-debit-internet-s1-2012/1">2012</a> et <a href="http://www.degrouptest.com/publications/5/barometre-debit-internet-s2-2011/6">2011</a></h3>
La population des utilisateurs de Degrouptest est probablement biaisée, car on a affaire à des gens qui au minimum s'inquiètent de leur débit. Ce qui exclut ma mère et probablement la votre. Mais ils ont l'avantage de mesurer le ping, et d'isoler les extrêmes comme la fibre, le mobile et l'outre-Mer.
<ul>
  <li>Débit moyen : 8,27 Mb/s (5,4Mb/s hors fibre )</li>
  <li>Débit moyen des réseaux mobiles : 2.5Mbps (chiffres 2011)</li>
  <li>Ping moyen : 80ms (86ms hors fibre)</li>
  <li>Ping moyen réseaux mobiles : 200ms</li>
  <li>Débit montant moyen : 1,3 Mb/s ( 641Kbp/s hors fibre)</li>
  <li>Répartition :
<ul>
  <li>13% à 27Mb/s de débit moyen</li>
  <li>87% à 5,4Mb/s</li>
</ul>
</li>
  <li>à noter :
<ul>
  <li>Meilleur latence possible : Fibre Orange avec 19ms. Les opérateurs ADSL se situent entre 70ms (Free) et 100ms (SFR)</li>
  <li>1s-1.5s de latence sur satellite</li>
  <li>270ms de latence depuis l'outre-mer, 3Mbps de débit moyen</li>
</ul>
</li>
</ul>
A noter que le débit et la latence moyenne ont baissé entre 2011 et 2012. J'imagine que leur public a du s'élargir à des zones moins bien couvertes.
<h3><a href="http://www.60millions-mag.com/actualites/archives/la_verite_sur_le_debit_des_connexions_internet">60 millions de consommateurs, 2012</a></h3>
Leur testeur de débit a tourné pendant 1 an en 2011 et leur public est probablement plus proche du français moyen que DegroupTest. En plus on y quantifie les variations périodiques de débit.
<ul>
  <li>Débit moyen : 5.6Mb/s</li>
  <li>Répartition :
<ul>
  <li>11.8% moins de 1Mb/s</li>
  <li>12.8% de 1 à 2Mb/s</li>
  <li>31,8% de 2 à 5Mb/s</li>
  <li>30% de 5 à 10Mb/s</li>
  <li>12% plus de 10Mb/s</li>
</ul>
</li>
  <li>A noter :
<ul>
  <li>-20% de débit à 21h</li>
  <li>-10% le dimanche</li>
</ul>
</li>
</ul>
<h3><a href="http://www.arcep.fr/fileadmin/reprise/observatoire/4-2011/obs-marches-t4-2011.pdf">Etude ARCEP</a></h3>
De cette étude sur les chiffres d’affaire des FAI et opérateurs mobile, j’ai extrait les volumes de population pour confirmer ou requalifier les chiffres des autres :
<ul>
  <li>22 millions d'abonnements ADSL et fibre</li>
  <li>0,6 million d'abonnement fibre / cable</li>
  <li>0,3 million abonnés RTC</li>
  <li>32 millions d'abonnés mobile "data"</li>
  <li>+80% de volume de data mobile par an</li>
</ul>
<h3><a href="http://www.cedexis.com/fr/country-reports/">Statistiques Cedexis</a></h3>
Les statistiques de “l’aiguilleur du net” mesurent le temps d’aller-retour moyen entre un utilisateur de FAI et un CDN afin de choisir le meilleur. Les valeurs de latence y sont plus hautes qu’ailleurs car les CDN ne font pas que servir des fichiers statiques, il y a beaucoup d’intelligence lorsqu’une requête arrive chez eux. Je les prends car elles donnent une bonne idée de la vitesse avec laquelle les fichiers arrivent chez l’internaute qui visite des sites utilisant les CDN.
<ul>
  <li>118 ms de latence pour des fichiers hébergés sur Akamai (leader)</li>
  <li>134 ms de latence pour des fichiers sur Amazon EC2 Europe</li>
</ul>
Après les chiffres bruts, passons à l'analyse.
<h2>Quelle analyse ?</h2>
Il faut bien se dire que l'on compare des études avec des buts différents : mesurer l'efficacité des CDNs, des FAI ou avoir un vision plus globale oriente les méthodologies. C'est pour cela que j'explicite ma réfléxion, pour que vous utilisiez les commentaires pour critiquer ma tentative de compréhension.
<h3>Le débit moyen</h3>
Les statistiques d'abonnement de l'ARCEP montrent 2,7% d'abonnés fibres par rapport au nombre total d'abonnement, alors que Degrouptest reçoit 13% de ses mesures depuis la fibre. Nous pouvons donc requalifier le débit moyen de Degrouptest : <strong>5,01Mb/s</strong> (97,3 % * 5,4 Mb/s + 2,7 % * 27,3 Mb/s = 5,01 Mb/s).
Cette nouvelle moyenne se situe entre celle d’Akamai pour la France (4,6 Mb/s), et celle de 60 millions de consommateurs (5,6 Mb/s). Akamai ne détaille pas sa méthode mais on suppose qu’elle est plus représentative, car non volontaire. Elle ne mesure cependant que la connexion entre Akamai et un internaute et Cedexis nous indique qu’Akamai n’est pas le plus performant. Il faudrait donc légèrement remonter ce chiffre. D’un autre côté, 60 millions de consommateurs a fait son étude en 2011, et entre-temps DegroupTest semble indiquer une baisse de débit ADSL de 600 Ko/s. Il faudrait donc revoir leur moyenne à la baisse. Les chiffres convergent vers la valeur DegroupTest, que j’estime légèrement surestimée (population trop “aware”).

Pour arrondir, on peut donc <strong>situer un débit moyen à 4,8Mb/s</strong> sur ligne fixe. Concernant le débit mobile moyen, Akamai et Degrouptest ont à 30Kb/s près la même moyenne, on peut l'arrondir à <strong>fixer à 2,5Mb/s</strong>.
<h3>La latence</h3>
On applique la correction des stats ARCEP sur les stats DegroupTest et on obtient cette valeur arrondie : <strong>85ms</strong>. Cependant en regardant les données Cedexis, on voit des temps de réponse bien supérieurs, de 110ms par exemple pour récupérer un statique sur un CDN populaire comme Akamai. Cela est assez cohérent avec le fait qu'un CDN rajoute forcément un peu de latence (temps de routage supplémentaire, mise en cache, etc.) et que la population Degrouptest est surement un peu moins représentative que celle mesurée par Cedexis.

Concernant le ping sur mobile, on manque cruellement de données, la seule qu'on ait trouvé pour la France est celle de Degrouptest de 200ms. Dans des études bien moins larges dans d'autres pays, les chiffres varient de <a href="http://www.webperformancetoday.com/2012/04/02/mobile-versus-desktop-latency/">200ms</a> à <a href="http://www.webperformancetoday.com/2011/10/26/interesting-findings-3g-mobile-performance-is-up-to-10x-slower-than-throttled-broadband-service/">300ms</a>.

Pour la latence, nous nous fixons donc une moyenne arrondie de <strong>95ms sur ligne fixe</strong> et <strong>200ms sur mobile</strong>.
<h3>Le cas du mobile</h3>
Donc le débit moyen sur réseau mobile (2,5Mb/s) est plus élevé que 25% des ADSL de France. La latence est tout de même deux à trois fois plus élevée. Mais même en calibrant vos tests avec ces données et en supposant que vous les exécutiez depuis <a href="http://mobitest.akamai.com/m/index.cgi">un vrai mobile</a>, vous réalisez vite que quelque chose cloche : le ressenti utilisateur n'est pas du tout le même. En fait il manque une dernière composante, assez rare en ADSL : <strong>la perte de paquets TCP</strong>. Constamment les connexions ouvertes entre votre téléphone et l'antenne relai sont interrompues par des obstacles physiques, électro-magnétiques ou un changement d'antenne. Je n'ai pas trouvé d'étude avec un taux moyen de perte de paquet, aussi c'est un peu au pifomètre que j'utilise la valeur 25%. à débattre.
Il manque également le côté aléatoire de la connexion qui fait tout le charme des réseaux mobiles : parfois vous téléchargez une image en une demie seconde, parfois vous n'arrivez pas à avoir un octet pendant plusieurs minutes.
Exécutez régulièrement les applications <a href="http://speedtest.net/mobile.php">speedtest</a> ou Degroupest (<a href="http://links.fhsarl.com/pplipple">iOS</a>, <a href="http://links.fhsarl.com/applendroid">Android</a>) pour halluciner sur la variabilité des caractéristiques réseau.

[caption id="attachment_1137" align="alignnone" width="300"]<a href="http://braincracking.org/wp-content/uploads/2012/10/test-débit-mobile.png"><img class="size-medium wp-image-1137 " title="test débit mobile" src="http://braincracking.org/wp-content/uploads/2012/10/test-débit-mobile-300x258.png" alt="" width="300" height="258" /></a> Test débit mobile[/caption]

Sur quelques tests ci-dessus, sur 2 tests effectués à moins d'une minute d'intervalle et sans bouger, vous pouvez voir une différence de latence de 1 à 4. Vous pouvez également voir des tests qui vont de 14Kb/s à 9Mb/s, sans compter ceux qui n'aboutissent pas.

Mais non seulement il n'existe pas d'étude sur les trous réseau (en supposant qu'une telle étude puisse être représentative), mais en plus <strong>nous ne voulons pas introduire d'aléa dans le cadre du monitoring</strong>, ou alors comment savoir si une alerte en est vraiment une ? Nous devons malheureusement choisir des mesures dégradées mais fixes.
<h3><a name="resume"></a>En résumé</h3>
<table><caption>Les connexions moyennes françaises en 2012</caption>
<tbody>
<tr>
<th></th>
<th scope="col">Lignes Fixes</th>
<th scope="col">Réseau mobile</th>
</tr>
<tr>
<th scope="row">Débit descendant</th>
<td>4,8Mb/s</td>
<td>2,5Mb/s</td>
</tr>
<tr>
<th scope="row">Latence</th>
<td>95ms</td>
<td>200ms
(+25% de perte de paquets)</td>
</tr>
</tbody>
</table>
<h3>Il n'y a pas de connexion type</h3>
Mon prof de stats me tirerait les oreilles si on s'arrêtait là. En effet une moyenne seule est à peu près inutile sans <a href="http://fr.wikipedia.org/wiki/%C3%89cart_type">l'écart-type</a> qui permet de juger de la représentativité de cette moyenne. En fait une moyenne est certes pratique à manipuler, et il est utile de surveiller son évolution, mais <strong>elle est particulièrement trompeuse</strong> si la répartition des mesures n'est pas concentrée autour de la moyenne.
Et justement les études d'Akamai, de l'ARCEP et de 60 millions de consommateurs montrent qu'il y a <strong>de fortes disparités dans l'équipement des utilisateurs</strong>.
<ul>
  <li>L'ARCEP nous dit qu'il reste 1,4% de gens en RTC (56Kb/s), soit à l'échelle de la France 300 000 personnes. Dur de savoir si c'est leur unique moyen d'accéder au Web, mais c'est un débit 500 fois inférieur aux mobiles ! Akamai signale également un faible taux de débit inférieurs à 256Kb/s</li>
  <li>DegroupTest nous avertit sur le fait que depuis la France d'outre-mer, on a du 250ms de ping en moyenne, l'équivalent du mobile, et un débit 40% inférieur à la métropole</li>
  <li>60 millions de consommateurs et Akamai montrent que <strong>50% des utilisateurs ont moins de 5Mb/s</strong>, avec une répartition qui a l'air à peu près égale par tranche de 1Mb/s : 10% ont moins de 1Mb/s, <strong>20% moins de 2Mb/s</strong>, etc.</li>
  <li>les tuyaux du Web français sont embouteillés le soir, avec une <strong>perte de 20% de débit entre 21h et 22h</strong>.</li>
</ul>
[caption id="attachment_1144" align="alignnone" width="509"]<a href="http://braincracking.org/wp-content/uploads/2012/10/répartition-débits.png"><img class="size-full wp-image-1144" title="répartition débits" src="http://braincracking.org/wp-content/uploads/2012/10/répartition-débits.png" alt="" width="509" height="369" /></a> Il n'y a pas d'ADSL type en France[/caption]

<strong>Nous nous sommes donc posés la mauvaise question</strong> : calibrer vos tests sur LA moyenne française est à peine moins trompeur que de mesurer depuis votre poste avec votre navigateur récent, sur votre réseau d’entreprise branché à la fibre (oui tout le monde le fait, et c’est MAL).

<h2>Enfin : quelle stratégie de test ?</h2>
Tous ces chiffres vont nous permettre de définir une vraie stratégie de test de la performance perçue. Cela commence par définir des objectifs de réactivité raisonnables en terme de confort d’utilisation, par exemple :

<ul>
  <li>afficher quelque chose avant 1 seconde (start render)</li>
  <li>promesse / fonction principale de la page affichée avant 2 secondes</li>
  <li>fonction principale interactive avant 5 secondes</li>
</ul>
Note : les valeurs que j’indique ici sont un peu génériques, et sont à définir entre les équipes “produit” (marketing, ergonomie) et les équipes techniques (développeurs, sysadmins).
Vous devez maintenant être cruel et déterminer quelle proportion d’utilisateurs n’aura pas droit à cette rapidité. C’est un peu rude dit comme ça, mais une page rapide pour “tout le monde” coûte très cher. Généralement on vise à satisfaire par exemple 80 % de la population.

Voici un petit tableau qui devrait vous aider à prendre vos décisions :
<table><caption>Calibrage des tests pour ligne fixe</caption>
<tbody>
<tr>
<th scope="col">Un débit de <strong>X</strong> Mb/s</th>
<th scope="col">et une latence de <strong>Y</strong> ms</th>
<th scope="col">laissent de côté <strong>Z</strong> % des visiteurs</th>
</tr>
<tr>
<td>5.0Mb/s</td>
<td>95ms</td>
<td>50%</td>
</tr>
<tr>
<td>4.0Mb/s</td>
<td>100ms</td>
<td>40%</td>
</tr>
<tr>
<td>3.0Mb/s</td>
<td>110ms</td>
<td>30%</td>
</tr>
<tr>
<td>2.0Mb/s</td>
<td>120ms</td>
<td>20%</td>
</tr>
</tbody>
</table>
Remarques :
<ul>
  <li>Les débits et pourcentages semblent joliment arrondis. C'est de la chance.</li>
  <li>Pour le ping, on n'a aucune stat sur la répartition réelle, ce sont donc des suppositions personnelles que je vous invite à critiquer</li>
</ul>
Pour le réseau mobile, on est bien embêté car on n’a aucune idée de la répartition ! On sait juste qu’elle est très volatile, ne serait-ce que pour le même utilisateur. Donc, soit on joue ça au doigt mouillé avec la certitude de se tromper, soit on utilise la moyenne qui est pratiquement un mensonge. Mmmm. Choix cornélien mais, à choisir, autant partir sur une valeur un tout petit peu objective, c’est-à-dire la moyenne constatée, et y rajouter de la perte de paquets.

Je mettrais donc ces 3 valeurs par ordre de priorité :
<ol>
  <li>3,0Mb/s avec une latence de 110ms pour représenter 70% de vos utilisateurs</li>
  <li>2,5Mb/s avec une latence de 200ms et <strong>une perte de paquets de 25%</strong> pour l'ensemble des utilisateurs de réseau mobile</li>
  <li>4,80Mb/s avec une latence de 95ms pour "la moyenne" des connexion filaires</li>
</ol>
La moyenne reste là surtout parce que c'est un chiffre que tout le monde (croit) comprendre et pour comparer mobile et bureau.

Si vous avez une population particulière (outre-mer, visiteurs à 21 h, habitant une grande ville, professionnelle, très mobile, etc.), il va vous falloir fouiller dans les chiffres ci-dessus pour trouver ce qui se rapproche le plus de vos utilisateurs. Ou, bien sûr, monter votre propre monitoring des capacités de vos utilisateurs.

<h2>Résultats</h2>
Pour conclure, comparons les valeurs DSL par défaut de WebPageTest.org avec celles que nous avons déterminées pour représenter 70 % des français. J’ai lancé un test sur un site moyennement complexe, qui n’a pas (encore) été optimisé et qui fait partie des sites à forte audience de France : Pluzz.fr.

[caption id="attachment_1181" align="alignnone" width="1544"]<a href="http://braincracking.org/wp-content/uploads/2012/11/pluzz-comparaison-valeurs-defaut-et-custom.png"><img src="http://braincracking.org/wp-content/uploads/2012/11/pluzz-comparaison-valeurs-defaut-et-custom.png" alt="" title="pluzz-comparaison-valeurs-defaut-et-custom" width="1544" height="290" class="size-full wp-image-1181" /></a> Comparaison valeurs par défaut et personnalisées[/caption]

Pour plus de détails, voir <a href="http://www.webpagetest.org/video/compare.php?tests=121014_2994e714a56d3b251702fd9e2dcf43c0%2C121014_aa3557eec96550f644cc462b1d3647bb&thumbSize=200&ival=100&end=visual">le détail du déroulé</a>. Avec les valeurs par défaut, ce site s’affiche en moins d’une seconde, donnant l’impression que l’optimisation n’est pas nécessaire. Avec nos valeurs plus réalistes prenant en compte 70 % de la population, le start render se situe à 1,4 seconde, confirmant le besoin d’entamer des travaux si l’on veut atteindre les objectifs de performance perçue.
