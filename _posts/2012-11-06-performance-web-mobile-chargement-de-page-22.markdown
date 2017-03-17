---
layout: post
title:  "Performance web mobile, chargement de page 2/2"
date:   2012-11-06 16:04:21 +0100
---
<h1>Reprioriser les optimisations</h1>
On l’a vu <a href="http://braincracking.org/?p=1156">dans le premier article</a>, certaines techniques pourtant reconnues peuvent devenir contre-productives si l’on ne fait pas les tests nécessaires. Ne jetons cependant pas tout aux orties, voici les techniques que vous devrez maîtriser plus que jamais et pousser dans leurs extrêmes si vous voulez obtenir un résultat correct sur mobile ; par ordre de priorité :

<h2>1. Travailler le chemin critique</h2>
<strong>La notion de performance est plus un ressenti qu’une collection de chiffres</strong>. Lorsque vous devez retravailler une page pour un réseau mobile, demandez-vous ce que l’utilisateur est vraiment venu voir et dégagez le passage pour afficher au plus vite la zone ou la rendre fonctionnelle :

<ul>
  <li>pas (trop) de redirections, et surtout pas côté client ;</li>
  <li>concaténation ou mise en ligne des CSS ;</li>
  <li>un code HTML / CSS prêt à "marcher" sans JavaScript : pour un article, c’est facile, on affiche le texte (sans police de caractère, jamais) ; pour une vidéo, on peut utiliser HTML5 comme interface de lecture en attendant JavaScript. Si vous dépendez de JavaScript pour afficher la fonctionalité (un webmail par exemple), affichez au moins un indicateur de chargement ;</li>
  <li>scripts asynchrones comme vu dans le premier article ;</li>
  <li>JavaScript chargé et exécuté par ordre de priorité : on peut très bien imaginer de charger d’abord les fichiers JavaScript concaténés qui concernent la zone principale, puis un second pack de fichiers qui concernerait tout le reste de la page.</li>
</ul>

<h2>2. Minimiser le nombre de requêtes HTTP</h2>
On l’a vu lors de l’exemple du <em>domain-sharding</em>, la première limite vient du réseau : le même mobile sur la même session de surf pourra passer d’un WiFi de bonne qualité à une absence complète de réseau. Vous ne pouvez jamais prévoir quand la latence va s’allonger jusqu’au <em>timeout</em> ou quand le débit passera en dessous de ce dont vous auriez besoin pour afficher une simple image. En supposant que vous serviez votre site classique sur mobile (1 Mo, 100 requêtes, 15 domaines… <a href="http://httparchive.org/trends.php">source http archive</a>), il va falloir faire baisser de manière drastique le nombre de requêtes pour minimiser les effets de latence ou de timeout et maximiser l’utilisation de la bande passante. Cet objectif est là pour durer et la plupart des techniques classiques utilisées jusqu’ici sont non seulement à investiguer, mais à pousser dans leurs extrêmes.

<h3>La concaténation</h3>
Il est plus que jamais temps de faire de vos CSS et JS des fichiers uniques ou presque. De base dans <a href="http://rubyonrails.org/">Ruby on Rails</a>, avec des plugins divers pour <a href="http://www.spip.net/">Spip</a> ou <a href="http://drupal.org/">Drupal</a>, avec <a href="https://github.com/widmogrod/zf2-assetic-module">Assetic</a> pour Zend 2, des dizaines de librairies pour d’autres projets PHP, avec <a href="http://sass-lang.com/">SASS</a> côté CSS et des outils comme <a href="http://yuilibrary.com/projects/yuicompressor/">YUI Compressor</a>.
Il n’y a plus vraiment d’excuses pour ne pas gérer correctement vos JS et CSS, sauf peut être le coût de refactoring initial, mais il reste généralement négligeable par rapport aux bénéfices attendus.

À noter que les stratégies de concaténation vont varier en fonction de la taille de votre site : si il est relativement petit, mettez tous vos CSS dans une seule feuille ; si il est plus grand, mettez le CSS commun dans une première feuille et les CSS de chaque type de page dans une autre. Ne dépassez jamais 2 feuilles par page. En JavaScript, les stratégies sont plus variées du moment que vous utilisez l’asynchrone : un fichier unique pour tout le site si il est petit, un fichier par zone fonctionnelle sinon (avec chargement asynchrone).

<h3>Les images</h3>
Les images sont généralement petites et nombreuses, c’est donc votre seconde cible après le CSS. Vous avez forcément déjà entendu parler du <a href="http://www.alsacreations.com/tuto/lire/1068-sprites-css-background-position.html">spriting</a> ; sur mobile, vous pouvez aller encore plus loin avec les caractères unicode : lorsque vous avez beaucoup d’icônes, essayez de voir si vous pouvez en réutiliser (♻) qui viendraient directement des polices du système !
Attention à la compatibilité navigateur ET système d’exploitation. Pour les mobiles, c’est généralement bon ; pour IE sur XP, vous n’aurez qu’un choix limité.

Encodage des images : visez le zéro requête en encodant vos images en texte directement dans le CSS, voire dans le HTML. C’est un peu fastidieux à faire à la main mais un simple script PHP suffit :

<pre><code>print '&lt;img src="data:image/png;base64,'.base64_encode(file_get_contents('/path/to/image.png')).'" /&gt;';</code></pre>
Qui vous donnera en sortie quelque chose de ce genre :
<pre><code>
&lt;img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAMAAAAoLQ9TAAABO1BMVEX92RlCQj+7vLzvzSzmhgnXgybyz3uLj5T13mj99cDwrmWMfzXmyTz78bT1zUf++uX437bxzqzmpj/dlDdcVUbuxnjszDLq6uvqmS/43Hv99t+foaOMgl5tYDjsu1vfwT388NijlXr52Vn22Y3999Z2bmD34svhrFlQTD378ajuwWXThzH668PpsFbvw5zrtU2Vh03x0Y3MzMz73kr664qZmZnmnUblqm/7w1HokRbefBb//e324HL2z0TsuENYRC399s/343z46paYijr88s/z1KXvyIvspkzroTj81yL56cnajjLwxm9KSkLmqj1gVj+Xg2H3vkv+/tz35XL+3CD89cj45Lf88MSipab13FnvtGP224T45Yb78rr++uv3zij25Grz1Ub1yi7wxFztu2vYiTH95kfqs2v45csXvxkaAAAAAXRSTlMAQObYZgAAAMlJREFUaN5jYAACY0llQzafZFsGCAiJDvNSkJJSEEjR9wQLROvw86s4OISqxLkJgfhKYfz8/NZA4MBrnW4HFGBzkZKKAwlIxWpmCAMFRKVCQ1VUXFzCY52CpVKBAlpeKo4xUVFR7jGRJmreQAE+N2sbCRsbDhuWxCBzP5Cpcq5BHjYJCTaWnIKsYHs9dV25bIDAXY81CeIyT3WzwLQ0CyvteKhTGcQCFJ25Ve2ZGeBAJiLClN0fwWcwYBI3kpZFEuAR8WXUkGfACgC55CKssDjPhQAAAABJRU5ErkJggg==" /&gt;
</code></pre>
Des frameworks complets comme SASS permettent de le faire dans le CSS. Vous pouvez utiliser cette technique à partir de IE8, et envisager <a href="http://www.phpied.com/the-proper-mhtml-syntax/">l’équivalent MHTML</a> pour IE7.

<h3>Le nombre de domaines</h3>
Le site moyen a entre 10 et 15 domaines à faire résoudre et il n’y a pas eu d’études sur l’utilisation du cache DNS sur mobile. Pratiquement invisible sur ordinateur de bureau, le coût de recherche DNS est peut être dix fois plus important sur mobile à cause de la latence. Chaque recherche de domaine va bloquer une file de téléchargement en attendant la réponse. Donc, soit vous sacrifiez trackers, publicité, widgets et autres sources externes, soit vous les dépriorisez fortement, tout simplement en les appelant après vos propres fichiers.

<h3>Connexions permanentes</h3>
Si vous avez une version dédiée mobile de votre site (pas de problématique de référencement, code spécifique), autant y aller à fond et utiliser une technologie très prometteuse pour contrer les effets de la latence : WebSocket. Pointez deux navigateurs <a href="http://html5demos.com/web-socket">cette page de démo</a>, et observez le peu de délai dans la transmission d'une info d'un navigateur à l'autre, réseau mobile ou pas. Websocket ou d'autres techniques de connexions permanentes peuvent être détournées : vous chargez en une seule requête une page initiale ne contenant que le strict nécessaire de CSS, de contenu et de JS, puis vous chargez tout le reste (images, CSS, JS, contenu) via cette connexion, que vous pouvez même réutiliser pour renvoyer des informations (AJAX, tracking…), sans payer le prix d'ouverture de nouvelles connexions HTTP.
Je n'ai pas encore vu d'équipe web déclarer avoir implémenter cela, mais ça ne saurait tarder. Au pire, patientez un peu, je vais coder une démo sur de vrais sites.

<h2>3. Le cache et l’offline</h2>
Comme avant, il va falloir que vous commenciez par maîtriser absolument toutes les URLs vers les statiques de votre site pour pouvoir les servir avec des caches agressifs, sur des domaines sans cookies. Ça c’est surtout pour les navigateurs de bureau car <a href="http://www.guypo.com/mobile/understanding-mobile-cache-sizes/">le cache des navigateurs mobile est une vraie calamité</a> en terme d’efficacité ! Il y a beaucoup trop de limites (nombre d’objets, taille maximale par objet, taille maximale par site et taille maximale pour tout le navigateur) pour espérer de vrais gains avec les comportements par défaut. Attention il ne faut pas les ignorer pour autant, si votre visiteur va d'une page à l'autre, il sentira la différence.

Les sites très efficaces en sont donc à utiliser HTML5 localStorage pour stocker des JS, des CSS et même des images (encodées en base64). C’est la technique choisie par LinkedIn mobile, Bing ou GMail, et elle demande un certain niveau de maîtrise de son application. Par exemple, imaginez-vous sur une page d’accueil : vous allez télécharger via XHR tous les objets dont vous avez ou aurez besoin, puis les stocker dans localStorage.

<pre><code>var loader = new XMLHttpRequest();
loader.open("GET", "http://example.com/js/main.js");
loader.onload = function(e) {
    localStorage.setItem('main.js', e.response);
}
loader.send();</code></pre>
Ensuite, au moment ou vous en avez besoin, vous vérifiez la présence des fichiers voulus dans votre cache maison.
<pre><code>if( localStorage.getItem('main.js') !== null ) {
    eval( localStorage.getItem('main.js') );
}</code></pre>
Oui, vous faites à la main le boulot du navigateur (télécharger, gérer le cache, insérer…) mais c’est avec ce genre de code un peu extrême que l’expérience utilisateur sur mobile fera la différence. En prime, les navigateurs de bureau en profiteront aussi.

Une note pour ceux qui ont entendu dire que LocalStorage était contre-performant car synchrone (lorsque vous écrivez, vous bloquez le programme le temps que le disque ait fini de travailler) :
<ul>
  <li>c’est évitable facilement en passant par un wrapper qui va simplement effectuer l’écriture en asynchrone ;</li>
  <li>les mobiles écrivant tout en RAM / Flash disk, les temps d’accès sont de toute façon négligeables.</li>
</ul>
Vous pouvez encore passer au niveau supérieur en utilisant une technique qui a autant fait ses preuves que montré ses limites : l’utilisation de la <a href="http://appcachefacts.info/">spécification Application Cache</a>. <strong>Native, bien supportée par tous les navigateurs modernes mobiles ou de bureau</strong>, elle permet de déclarer en un fichier (le manifeste) quels sont les fichiers à télécharger, quelles ressources doivent continuer à être servies en ligne, et quoi afficher lorsque la connexion n’est plus là. Au contraire de la gestion du cache présentée précédemment qui est personnalisée et donc très adaptée à votre application, Application Cache vous force à coder d’une certaine manière (maîtriser absolument toutes ses URLs), présente certains manques (invalider le cache de certains fichiers seulement) et il faut un petit temps d’adaptation pendant la phase de développement pour comprendre son fonctionnement intrinsèque. En revanche, en terme de performance et de mode de distribution, c’est parfait !

<h2>4. Réduire la taille</h2>
<h3>Minification et compression des JS / CSS</h3>
La minification est généralement exécutée au déploiement du site juste après la concaténation et vous pouvez généralement gagner 50 % du poids des fichiers avec des outils solides comme <a href="http://yuilibrary.com/projects/yuicompressor/">YUI Compressor</a>. Activez la compression gzip par-dessus et vous aurez gagné au final 90 % du poids initial de vos fichiers. Sachant qu’il n’est plus rare de trouver des sites avec 200-500 Ko de CSS et de JS, le gain est assez important.
<h3>Les photos</h3>
Pour les images de l’interface, comme pour les photos, vous avez beaucoup à gagner en compressant au maximum les images servies. Encore plus efficace : ne les afficher que si elles sont nécessaires. Dans le cas d’une application Web Mobile ou l’interface permet de beaucoup scroller, considérez comme une amélioration majeure de faire du lazy-loading sur les images, c’est à dire de ne télécharger l’image que lorsqu’elle est sur le point d’être affichée.
En terme de compresseur d'image sans perte de qualité, voyez avec <a href="http://advsys.net/ken/utils.htm">PNGout</a> (ligne de commande) et <a href="http://pnggauntlet.com/">PNG Gauntlet</a>  (son interface visuelle) pour compresser efficacement les PNG, ou <a href="http://sauron.jyu.fi/cgi-bin/viewcvs.cgi/jpegoptim/">JPGoptim</a> (ligne de commande) et <a href="http://imageoptim.com/">Image Optim</a> (meta interface pour Mac) pour les JPG.

<h3>Les cookies</h3>
Là aussi, investissez du temps pour maîtriser toutes les URLs de toutes les ressources statiques de votre site , afin de les mettre sur des domaines séparés du domaine principal. Imaginez un cookie d’un peu moins d’un Ko qui serait envoyé à la centaine d’objets généralement appelée par une page Web classique : vous demandez à votre utilisateur d’uploader 100 Ko de données inutiles. Une autre piste pour remplacer les cookies qui sont parfois servis sur des pages statiques qui n’utilisent même pas de données personnalisées est de ne pas les utiliser du tout… pour les remplacer par HTML5 localStorage si vous avez tout de même besoin de persistance côté client (préférence d’affichage, historique…). Pour la compatibilité navigateur, passez par des petites librairies spécialisées.



<H1>En résumé</h1>
Vous voilà donc paré pour arrêter de perdre vos clients dès qu’ils essayent d’accéder à votre site à partir d’un réseau mobile. Nous avons vu que :
<ul>
  <li>la performance est empirique : testez même les grands classiques avant de choisir vos solutions ;</li>
  <li>la plupart des techniques déjà connues marchent mais sont à traiter dans un autre ordre ;</li>
  <li>ces techniques sont à pousser dans leurs extrêmes ;</li>
  <li>de nouvelles possibilités sont apparues.</li>
</ul>
Et ne comptez pas sur l’évolution future de l’équipement réseau : sauf inversion de la tendance actuelle, non seulement vos sites vont continuer à grossir mais en plus les futurs utilisateurs des réseaux 4G voire Wimax auront toujours des doigts, des murs, des gens et de l’atmosphère à traverser avant d’arriver sur vos serveurs.

L’optimisation, c’est maintenant.
