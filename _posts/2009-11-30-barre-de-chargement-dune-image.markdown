---
layout: post
title:  "Barre de chargement d’une image"
date:   2009-11-30 16:04:21 +0100
---
Autant le dire tout de suite, ce genre de technique ne marchera pas pour IE6-7-8 mais pour le tiers restant du marché. En faisant un petit effort, IE8 peut être supporté, ce qui amènerait la couverture du marché de cette technique à 50%.

Certains sites peuvent avoir besoin de charger de très grosses images :
<ul>
  <li>une galerie qui affiche les originaux ou de très grandes photos</li>
  <li>un site de manipulation de photo qui joue avec l'original</li>
  <li>ou plus bêtement une petite connection</li>
</ul>
En HTML il n'y a pas 36 solutions, on demande au browser via un tag <em>&lt;img&gt;</em> de charger les données ET de les afficher. N'importe quelle technique en JS passe par ce même tag pour aller chercher les données.
<pre>&lt;img src="/path/to/img"&gt;</pre>
ou
<pre>var oImage = document.createElement('IMG');
oImage.src = '/path/to/img';</pre>
Si vous vouliez <strong>afficher un loading</strong>, vous mettiez éventuellement un spinner en <em>background-image</em> de l'élément, mais vous ne pouviez pas indiquer le <strong>temps de téléchargement de l'image</strong>. Une combinaison de plusieurs techniques peut cependant le permettre :
<ul>
  <li>XHR (AJAX) pour aller chercher les données. Rien de neuf ici, seul IE6 ne le supporte pas.</li>
  <li>l'event <strong>onprogress</strong> de XHR : c'est la nouveauté supportée par <strong>Chrome, Safari et FF3+</strong>. Il <a href="http://msdn.microsoft.com/en-us/library/cc197058(VS.85).aspx">existe aussi dans un autre objet pour IE8</a>, mais l'utilisation de l'objet <a href="http://msdn.microsoft.com/en-us/library/cc288060(VS.85).aspx">XDomainRequest</a> est un peu contraignante</li>
  <li>la technique des <a href="http://actuel.fr.selfhtml.org/articles/graphisme/inline-images/index.htm">images inline</a>, qui utilise les uri et base64. Non supporté par IE6 et IE7.</li>
</ul>
On commence par la <strong>détection du support </strong>de la feature, qui va nous permettre d'afficher un spinner traditionnel en fallback :
<pre>function checkSupport() {
var oEl = new XMLHttpRequest();
if(!oEl) // if standard XHR does not exist, we're probably on IE6 that do not support event progress anyway
return false;
return ('onprogress' in oEl);
}</pre>
On crée notre connection XHR en écoutant l'event <em>onprogress</em>, et en calculant le pourcentage de téléchargement :
<pre>var xhr = new XMLHttpRequest();
xhr.onprogress = function( e ) {
if (e.lengthComputable) {
var iProgress = (e.loaded / e.total);
// Ici mise à jour de l'interface
}
};</pre>
Attention car l'event onprogress est appellé très souvent selon les browsers, donc si nécessaire il faut temporiser les updates de l'interface

Lorsque toute l'image est téléchargée côté client, on met à jour le <em>src</em> de notre image :
<pre>xhr.onload = function( e ) {
// ici le code pour arretter la barre de progression
oImage.src = 'data:image/jpg;base64,'+e.target.responseText;
};
// les listeners sont posés, on peut lancer la requete
xhr.open('GET', '/path/to/image?base64', true);
xhr.send();</pre>
Server side, il faut bien penser à encoder en base64 le contenu du fichier lorsque c'est demandé, et à renvoyer en header l'information de taille :
<pre>header('Cache-Control: max-age=86400');
header('Content-Type: image/jpg');
if(isset($_GET['base64'])) {
$out = base64_encode(file_get_contents($path));
header('Content-Length: '.strlen($out));
print $out;
} else {
header('Content-Length: '.filesize($path));
print file_get_contents($path);
}</pre>
Comme toutes les solutions qui essayent d'améliorer l'existant pour les browsers qui le supportent, celle ci coûte bien sur plus cher en temps de développement puis en support, donc c'est à vous d'évaluer le rapport coût/bénéfice. D'autant plus que si vous étendez le support jusqu'à IE8 vous avez la moitié du marché d'aujourd'hui, ce qui commence à vraiment valoir le coup.

<em>EDIT : Il semble qu'il y ait une </em><strong><em>limite de taille</em></strong><em> et un comportement qui change selon les browsers en ce qui concerne la quantité de données que l'on peut mettre dans les urls. IE8 par exemple supporte 32Ko max, ce qui rend cette <strong>technique inutile </strong>pour des images autre que de la décoration. Je n'ai pas trouvé les limites pour Chrome et FF3, mais en essayant avec des <strong>images supérieures à 1Mo</strong>, elles s'affichent partiellement ou pas du tout selon le browser. Donc jusqu'à nouvel ordre, <strong>cette technique doit rester une expérimentation.</strong></em>
