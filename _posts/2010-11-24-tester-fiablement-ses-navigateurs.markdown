---
layout: post
title:  "Tester avec fiabilité ses navigateurs"
date:   2010-11-24 16:04:21 +0100
---
Pour une fois, ce blog ne parlera pas d'HTML5, de javascript avancé ou de performances mais de <strong>bonnes pratiques</strong>. On reste toutefois dans la modernité du développement Web, car nous allons voir pourquoi j'estime que la plupart des développeurs Web et pire, des Web agencies sont <strong>sous-équipées lorsqu'il s'agit de tester ses navigateurs</strong>.

Je vous livre même la conclusion immédiatement : <strong>vous vous devez d'avoir des machines virtuelles</strong> pour avoir un environnement de test simplement normal et voici pourquoi.
<!--more-->
<h2>Je ne m'énerve pas, j'explique</h2>
Je travaille dans des conditions outrageusement luxueuses.
Comprenez par là que je bosse sur un MacBook Pro depuis 4 ans, et que j'ai une machine virtuelle pour chaque version d'Internet Explorer. J'ignorais ma condition de nanti jusqu'au jour où j'ai travaillé avec une web agency puis discuté avec d'autres développeurs.
Ce qui comme la plupart des développeurs ne me gênait pas au début (après tout c'est cher à mettre en place) est devenu rapidement un problème : mes prestataires ne pouvaient pas reproduire certains bugs car ils n'avaient pas les outils nécessaires. Après une petite année à ce régime, j'ai cumulé suffisamment de preuves pour me permettre de demander légitimement aux prochains prestas de s'équiper avant de travailler avec nous.
<h2>Arrettez les "compatibility mode"</h2>
[caption id="attachment_620" align="aligncenter" width="345" caption="Choix du mode de compatibilité dans IE8 Debug Tool"]<a href="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/11/compatibility-view.png"><img class="size-full wp-image-620" title="compatibility-view" src="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/11/compatibility-view.png" alt="" width="345" height="89" /></a>[/caption]

C'est probablement le pire moyen de tester : le mode de compatibilité d'IE8 ou IE9 supposés exécuter le moteur de rendu d'<abbr title="Internet Explorer 7">IE7</abbr> ou <abbr title="Internet Explorer 8">IE8</abbr>. <abbr title="Microsoft">MS</abbr> a voulu bien faire pour faciliter la vie de ses utilisateurs et leur permettre une transition en douceur de IE7 vers IE8, mais les développeurs se sont vite rendu compte que <strong>le mode de compatibilité générait des rendus différent de IE7</strong>. A tel point que Yahoo! et sa matrice de navigateurs à tester recommandent constamment de tester spécifiquement le mode de compatibilité (voir <a href="http://yuiblog.com/blog/2010/11/03/gbs-update-2010q4/">les updates</a>, dans les notes).

<strong>Certaines techniques sont faussées</strong> : j'ai par exemple constaté que le support des images en data:uri et base64 marchait dans IE8 en mode IE7 standard alors que IE7 ne le supporte pas. Comment faire pour valider certaines techniques émergentes (ici pour les performances) si vous n'avez pas les bons navigateurs pour tester ?

La différence de comportement la plus grave que j'ai constaté (et <a href="http://blog.dynatrace.com/2010/11/11/uncover-javascript-json-parsing-errors-with-ie-compatibility-mode/">je ne suis pas le seul</a>) vient <strong>du moteur javascript</strong> : sous <abbr title="Internet Explorer 6">IE6</abbr> et <abbr title="Internet Explorer 6">IE7</abbr>, le fait d'oublier d'enlever la dernière virgule (,) après une chaîne <abbr title="JavaScript Object Notation ">JSON</abbr> a toujours planté le script :
<pre><code>var oData : {
  value1:"blah",
  value2:"blah", // === boom
};</code></pre>
Sous <abbr title="Internet Explorer 8">IE8</abbr> comme sur tous les autres navigateurs, cette syntaxe est valide. Or si vous vérifiez avec le mode de compatibilité IE7 sous IE8, <strong>vous ne verrez pas cette erreur</strong> qui casse votre javascript et qui enlève donc des fonctionnalités majeures de votre site, simplement parce que vous n'êtes pas dans les bonnes conditions pour tester.
<h2>Les installation multiples d'Internet Explorer</h2>
[caption id="" align="aligncenter" width="476" caption="Screenshot IE tester"]<a href="http://www.my-debugbar.com/wiki/uploads/IETester/ietester-0.3.png"><img class=" " title="Screenshot IE tester" src="http://www.my-debugbar.com/wiki/uploads/IETester/ietester-0.3.png" alt="" width="476" height="275" /></a>[/caption]
<p style="text-align: left;">Internet Explorer est à ce point implanté dans son OS qu'il est en théorie impossible d'installer plusieurs versions d'Internet Explorer (ce qui leur avait même valu des <a href="http://en.wikipedia.org/wiki/United_States_v._Microsoft">problèmes avec la justice</a>). Mais plusieurs softs ont pallié à cette situation et permettent <strong>d'exécuter plusieurs versions d'IE</strong> comme <a href="http://www.my-debugbar.com/wiki/IETester/HomePage">IETester</a>, <a href="http://utilu.com/IECollection/">IECollection</a> ou <a href="http://tredosoft.com/Multiple_IE">Multiple IE</a>. Le problème, c'est que ces différentes instances d'IE continuent de partager certaines DLLs et autres appels système. Ca n'a l'air de rien, mais concrètement pour le développeur Web ça l'empêche de voir certaines choses :</p>

<h3>Plugins</h3>
Les interaction avec des plugins populaires comme Flash ou Java sont problématiques :
<ul>
  <li>J'ai déjà vu Flash planter sans que IE dans un IETester ne s'en aperçoive, le fallback que nous implémentions en Flash (la sélection multiple de fichiers pour un uploader) était essentiel, mais le développement était pénible : on s'est rendu compte au bout d'un moment que <strong>Flash ne répondait plus ou mal</strong>, et qu'il fallait tuer le processus Flash pour que cela remarche. Avec un <strong>IE natif, pas de problèmes</strong> : lorsque Flash plante, il se relance correctement, ce qui accélérait le développement</li>
  <li>l'interaction avec une applet java (toujours pour un uploader, en drop de fichiers cette fois) était tout simplement <strong>impossible à tester avec plusieurs IE</strong> dans le même IETester. Une machine virtuelle, c'est déjà lourd à lancer, alors imaginez 3 applets en parallèle qui essayent de s'afficher sur la même zone de l'écran. Les fichiers n'étaient pas réceptionnés dans la bonne instance d'IE ...</li>
</ul>
<h3>Fonts et couleurs</h3>
Pour le rendu des fonts il vous faudra typiquement vérifier votre site sous un Safari sur Mac et sur un IE6 sous XP : le <strong>Safari Mac rend les polices exactement comme votre graphiste l'a prévu</strong> sur sa belle maquette Photoshop. C'est la vision qu'il a du site, et c'est surement celle que les décideurs ont du site ...
Le couple terrible IE6-XP qui est encore là pour longtemps est lui à <strong>l'extrémité opposée du spectre des rendus</strong>, et correspond au pire de ce que verront vos visiteurs. Les versions supérieures de IE et des navigateurs ne font qu'améliorer le rendu sans jamais totalement atteindre la perfection du couple Safari / OSX.

[caption id="attachment_634" align="aligncenter" width="542" caption="Au moins 4 rendus par police"]<a href="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/11/rendu_police.png"><img class="size-full wp-image-634" title="rendu_police-mini" src="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/11/rendu_police-mini.png" alt="" width="542" height="67" /></a>[/caption]

J'ajoute que selon la taille de police, l'activation ou pas de l'option lissage, le type de police et les caractères utilisés, certains mots peuvent devenir illisibles ou du moins pénibles à lire sur les Windows. Lorsque vous décidez de l'utilisation d'une police, il vous faudra d'abord <strong>vérifier son rendu final sur au moins XP, Vista ou Seven, et enfin MacOS</strong>. Soit vous maintenez 3 machines physiques avec 1 OS sur chacun, soit vous utilisez les machines virtuelles (ou plus pénible : des machines en dual boot).

Le rendu des couleurs : j'avoue ne pas très bien savoir comment faire ici. Mac et les différents Windows rendent les couleurs différemment, discutez en 2 minutes avec un graphiste, ça lui laissera le loisir de vous montrer qu'il en sait plus que vous. Hors avec les machines virtuelles, on affiche un Windows sur un Mac, je n'ai aucune idée de ce que cela donne réellement. Pour en rajouter un peu, je n'obtiens pas les mêmes couleurs selon que je suis sur l'écran du macbook ou sur le 2nd écran ... Devant mon incapacité à vous expliquer comment tout cela marche, je ne peux que citer une des bonnes pratiques généralement accolée à l'accessibilité : <strong>inutile de jouer avec des nuances de couleurs, préférez de forts contrastes</strong>.

<h3>Etre à jour</h3>
Microsoft patche régulièrement ses OS ainsi que ses IE. Rarement mais sûrement certaines choses peuvent <strong>s'arrêter de fonctionner suite à ces patchs</strong>. Exemple concret : dans les IEs on pouvait récupérer le chemin local d'un fichier sélectionné par un <code>input type=file</code>. Ceci permettait d'afficher une miniature d'une image avant upload, fonctionnalité d'ailleurs à nouveau permise par HTML5. En 2009 un patch est passé qui a fait disparaître cette possibilité, après 10 ans de bons et loyaux services (voir <a href="http://braincracking.org/2009/11/27/cest-comme-ca-quon-fixe-les-bugs-chez-ms/">les screenshots</a> que j'avais fait).

Les packs d'IE ne permettent pas de rester à jour, ou alors vous dépendez de leur bon vouloir. Avoir une machine virtuelle permet d'utiliser le mécanisme d'auto-update de MS.

<h3>Autres fonctionnalités cassées</h3>
Il y a d'autres bugs connus sur Multiple IE ou IETester :
<ul>
 <li>les champs texte sur "Multiple IE" ne fonctionnent plus
 <li>les popups et leur interaction avec la page d'appel est cassée
 <li>les filtres CSS ne marche pas
 <li>la fonctionnalité d'impression ne marche pas
</ul>

<h2>Dégradation gracieuse et Fallbacks</h2>
<h3>Les anciens et les futurs navigateurs</h3>
Outre Internet Explorer, certains versions de Firefox valent encore la peine d'être testés avec plusieurs versions. Ca a été le cas pendant longtemps pour <abbr title="Firefox 3.0">FF3</abbr>, c'est encore peut être le cas sur vos sites pour <abbr title="Firefox 3">FF3.5</abbr> et à l'heure d'écrire ce post, c'est certainement vrai pour FF3.6, alors que l'on est passé à la 4.0.

Les bugfixs passés dans ces releases impactent rarement le travail des webdevs (mais ça arrive) mais il y a surtout eu des <strong>ajouts de fonctionnalité majeures</strong> (rangées sous l'ombrelle HTML5) ou l'ajout de supports CSS3 : il faut que vous testiez au moins une fois pendant le développement <strong>comment se comporte votre fallback ou votre graceful degradation sur ces anciennes versions</strong>.

Il n'est peut être pas utile de conserver des versions de Chrome et Opéra, l'un car il force l'auto-update, l'autre parce qu'il est rare, mais c'est à vous de juger.

<img class="alignleft size-full wp-image-641" title="firefox4" src="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/11/firefox4.png" alt="" width="274" height="134" />Par contre il est vital d'avoir <strong>les versions de développement de IE (la 10 preview) et Firefox (Minefield)</strong> pour tester régulièrement votre site avec : faites corriger les bugs <a title="exemple de bug que j'ai ouvert" href="https://connect.microsoft.com/IE/feedback/details/549601/ie9-preview-fails-to-manage-script-type-text-html">par Microsoft</a> ou <a title="exemple de bug que j'ai ouvert" href="https://bugzilla.mozilla.org/show_bug.cgi?id=610378">par Mozilla</a> avant que vos utilisateurs ne tombent dessus et vous fasse changer votre code !

Il n'est peut être pas nécessaire d'avoir une machine virtuelle dédiée à une version passées et futures des navigateurs hors IE, car il me semble que différentes versions de FF savent cohabiter ensembles et que les beta/preview d'IE savent également s'installer sans conséquences. Mais il vaut peut être mieux <strong>jouer la prudence</strong> là dessus : ce sont des versions beta et avec les nouvelles fonctionnalités type accélération matérielle, les navigateurs s'intègrent de plus en plus au système, il me semble donc évident que <strong>toute nouvelle installation met en péril l'intégrité des autres versions du même navigateur</strong>.
Et une fois que le mal est fait, difficile de revenir en arrière, tandis que sur une machine virtuelle vous pouvez isoler le problème, et vous pouvez même <strong>revenir en arrière grâce aux fonctionalités de snapshot</strong> à exécuter avant toute installation hasardeuse.
<h3>Plugins et addons</h3>
Votre site dépend parfois de Flash ou Java ? Il affiche des publicités ?<img class="aligncenter size-full wp-image-644" title="no-flash" src="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/11/no-flash.jpg" alt="" width="450" height="371" /> Il vous faut au moins une machine où Flash et Java ne sont pas installés, afin de <strong>tester de manière réaliste votre contenu alternatif ou le fallback de votre fonctionnalité</strong>. On ne développe pas ce qu'on ne voit pas.

Concernant Flash, et c'est valable avec Java : il existe des différences entre Flash 9 (encore répandu) et Flash 10.x. Les flasheurs développent et testent toujours avec la dernière version : ayez donc une machine avec une ancienne installation de Flash pour <strong>vérifier rapidement que le Flash qu'on vous livre marche pour tout le monde</strong>.

Votre Firefox est peut être comme le mien : c'est devenu un navigateur de développement sur lequel est installé une dizaine de plugins dont le gourmand firebug. Après quelques heures de développement mon Firefox est à la limite du praticable sur certaines pages chargées. Un tour régulier sur un Firefox sans aucun plugin dans une machine virtuelle me permet <strong>de vérifier (et non pas de supposer) si c'est un plugin ou mon code qui fait ramer le navigateur</strong>.
<h2><strong>Quelles contraintes ?</strong></h2>
Vous êtes probablement d'accord avec le principe d'être dans de bonnes conditions pour tester, mais vous avez peur que cela demande <strong>du budget et de la ressource machine</strong> ? C'est le cas, mais rien d'insurmontable. Concernant votre machine de développement d'abord :
<ul>
  <li><strong>Mac OS</strong> : il faudra le tester de toute façon, ne serait ce que pour tester ce que voient<strong> les graphistes et les journalistes</strong>... Pendant longtemps il n'y a pas eu de machine virtuelle capable de le faire tourner mais <a title="Guide d'installation VMWare" href="http://www.redmondpie.com/how-to-install-os-x-snow-leopard-in-vmware-windows-7-9140301/">il semblerait</a> que cela soit aujourd'hui possible, donc si vous voulez rester sous Windows ou Linux, c'est peut être jouable, à vos risques et périls. Pour ma part et depuis 4 ans, j'ai convaincu mes employeurs de payer 50% plus cher mon matériel pour être dans des conditions de test réalistes. Pour les managers qui doivent justifier ce budget : outre la qualité, un développeur sous mac est un développeur qui restera plus longtemps chez vous :)</li>
  <li><strong>4GO de RAM</strong> : pendant le développement, il faut tourner avec au moins un IE6 (ou 7) sous XP en permanence ouvert en plus l'environnement de développement normal (Firefox, éditeurs, divers logiciels ...). Avec <strong>2Go, c'est parfois pénible</strong>, du coup certains développeurs ferment à jamais la machine. 4Go, c'est mieux, et ça permet de supporter ces jours pénibles, généralement en début et en fin de projet, où vous allez lancer successivement toutes vos machines virtuelles pour valider des choix techniques ou le développement lui même.</li>
  <li><strong>un disque rapide</strong> : 2 OS qui cohabitent, ça fait énormément d'appels au disque dur, surtout que vous allouerez peu de RAM à Windows (512Mo max en général), et ce dernier utilisera la mémoire virtuelle (le disque donc). Si vous n'avez que 2Go de RAM et un disque de portable à 5400 tr/mn, la situation sera intenable. Essayez d'upgrader à 7200 (la différence est flagrante), voir de le remplacer par un SSD, technologie qui commence à être abordable.
</ul>
Concernant le software :
<ul>
  <li>XP et Seven : il vous faut au moins ces 2 OS pour installer de IE6 à IE9. Concernant les licenses, chez Yahoo! nous utilisions des licences dites corporate (valables pour des milliers de postes), donc pas de problème. En PME, la pratique généralement admise est d'acheter la licence, d'installer ... puis de cloner l'image de la machine autant de fois qu'il y a d'environnement et de développeur. Honnêtement <strong>je ne sais pas si c'est prévu par la licence</strong>. Si vous émulez à partir de Seven avec Virtual PC, Microsoft vous offre <a href="http://www.microsoft.com/downloads/en/details.aspx?FamilyID=21eabb90-958f-4b64-b5f1-73d0a413c8ef&amp;displaylang=en">des images d'installation</a> (limitées dans le temps) parfaites pour les webdevs.</li>
  <li>Une machine virtuelle Mac :
<ul>
  <li>payant : j'utilise <a href="http://www.parallels.com/fr/products/desktop/">Parallels Desktop</a> qui me satisfait pleinement mais qui a la réputation d'être gourmand en ressources. J'ai entendu du bien de <a href="http://www.vmware.com/products/fusion/">VMWare</a>. Dans tous les cas comptez <strong>moins de 100 euros</strong>.</li>
  <li>gratuit : <a href="http://www.virtualbox.org/">VirtualBox</a> est le leader de l'open source, mais n'était pas sorti lorsque j'ai commencé à utiliser Parallels il y a 4 ans, je ne peux donc pas en parler. <strong>Si vous êtes sur Windows Seven, vous pouvez utiliser <a href="http://www.microsoft.com/windows/virtual-pc/default.aspx">Virtual PC</a></strong>.</li>
</ul>
</li>
</ul>
Les machines virtuelles vous fournissent généralement des images pré-installées. Mais elles ne sont jamais en français, et bien sur ne dispensent pas de fournir une clé Windows valide. Veillez aussi à ce que les images d'XP fournies contiennent bien IE6 :  on ne peut pas désinstaller IE7 pour installer IE6.
<h2>La matrice de tests</h2>
Je vous conseille de suivre <a href="http://developer.yahoo.com/yui/articles/gbs/">la matrice de Yahoo</a>!, mise à jour 2 à 3 fois par an. Voici en tout cas le contenu de mes machines après plusieurs années de pratique :

[caption id="attachment_639" align="aligncenter" width="564" caption="Liste des configurations dans Parallels"]<a href="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/11/machines.gif"><img class="size-full wp-image-639" title="machines" src="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/11/machines.gif" alt="" width="564" height="407" /></a>[/caption]

Vista et FF2-3 ne sont peut être plus utiles à posséder, mais sinon toutes ces combinaisons tiendraient en 5 machines. Cela donne accès à :
<ul>
  <li>pas de flash/java, flash 9 et un Flash à jour partout ailleurs</li>
  <li>IE 6, 7 et 8 sur XP, IE9 sur Seven</li>
  <li>FF3.6 et FF4 (qui n'apparaît pas)</li>
  <li>IE10 preview à jour, un FF Minefield pour vérifier des bugfix à l'occasion</li>
  <li>Ces machines comportent presque toutes un Chrome et un Opéra à jour, en plus du Chrome / Opéra / Safari de mon Mac. Oui il y a parfois des différences pas uniquement visuelles entre une version Mac et Windows d'un même navigateur.</li>
</ul>
Cela fait beaucoup de combinaisons et il en manque encore : par exemple si je dois un jour tester le site sur IE6 sans Flash, je n'ai pas de machine toute prête. Qu'à cela ne tienne, il me suffit de cloner ou de faire un snapshot de ma machine IE6 puis de desinstaller Flash pour vérifier mon bug.
<h2 title="Mis à jour le 1er janvier 2011">Les alternatives</h2>
<h3>Service en ligne</h3>
Des services comme <a href="http://browsershots.org/">browsershots.org</a> ou <a href="https://browserlab.adobe.com/fr-fr/features.html#PreviewAnything">browserlab</a> permettent d'obtenir rapidement un screenshot d'une page vue par beaucoup de couples OS/navigateurs différents. Le désavantage évident, et dans mon cas bloqueur, est que votre page doit être sur une <abbr>URL</abbr> publique et que vous ne pouvez pas interagir avec la page. Peu d'équipe développent directement en ligne, préférant des <abbr>URL</abbr>s internes, et il est difficile de se passer de tester des interaction (<code>:hover</code>, formulaires, application Web ...)
<h3>Machines physiques</h3>
Les machines physiques sont encore plus réalistes que les machines virtuelles, en théorie vous y gagnerez donc en qualité de test. Le point négatif que j'ai expérimenté ne vient pas de l'outil mais de la nature humaine : accessibles physiquement ou depuis un <a href="http://www.realvnc.com/vnc/index.html">VNC</a>, c'est une solution peu pratique que les développeurs finissent par ne plus utiliser, préférant coder à l'aveugle plutôt que de se déplacer physiquement ou subir des lags réseau... Oui les développeurs sont comme ça, d'où l'intérêt d'investir dans du matériel :)

J'ajoute que les machines virtuelles permettent de faire des <span lang="en">snapshots</span>, ce qui permet de gagner en souplesse en installant sans remords des programmes pour tester certaines conditions extrêmes (IE toolbars, mises à jour de Flash, <span lang="en">Silverlight</span> et Java, reproduire un bogue avec un antivirus agressif ...). Maintenir un parc de machines physiques qui permet d'avoir autant de souplesse prendrait beaucoup de temps
<h3>Linux, windows comme OS principal</h3>
Par conviction ou par économie, vous pouvez préférer développer sur autre chose que Mac OS. Sur ces 2 familles d'OS vous pouvez tout à fait créer des machines virtuelles, mais il vous reste cependant le problème de l'émulation du Mac (je maintiens qu'il faut le tester quoi qu'il en coûte) : à priori la seule solution viable est d'avoir une machine physique Mac constamment à jour que l'équipe pourra utiliser. Mais d'expérience cette machine finira par être sous utilisée, et donc les tests de compatibilité seront moins assurés.
<h2>Conclusion</h2>
<ul>
  <li>Si vous vous considérez comme un professionnel du Web, vous vous devez d'investir dans un environnement de test fiable</li>
  <li>Il y a de vraies différences de comportement et de rendu entre un IE natif et des packs d'IE, et entre OS</li>
  <li>Il y a un gros nombre de combinaisons possibles (4 OS, 5 navigateurs, plusieurs sous-versions, Flash ...) et les différences iront en s'accentuant (nouveaux navigateurs, adaptation à chaque OS ...)</li>
  <li>La seule solution fiable et souple est d'avoir plusieurs machines virtuelles</li>
  <li>Il vous faut une machine Mac OS physique pour tester cet OS (une machine partagée ou une par développeur)</li>
</ul>
