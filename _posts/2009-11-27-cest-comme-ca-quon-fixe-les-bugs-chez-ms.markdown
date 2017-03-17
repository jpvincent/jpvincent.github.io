---
layout: post
title:  "C’est comme ça qu’on fixe les bugs chez MS"
date:   2009-11-27 16:04:21 +0100
---
C'est passé assez inaperçu, mais pendant longtemps il y avait dans IE6, IE7 et même IE8 un bug fort pratique permettant d'accéder au chemin complet d'un fichier via un <code>&lt;input type="file"&gt;.</code> Je dis pratique car il permettait aux plus malins d'entre nous d'afficher une fausse preview d'une photo à côté du champs.

Code :
<pre><code>&lt;form&gt;
&lt;input type="file" id="file-upload" onchange="document.getElementById('thumb').src=this.value;" /&gt;
&lt;img id="thumb" src="avatar_JP.jpg" /&gt;
&lt;/form&gt;</code></pre>
Si vous avez des versions non patchées de IE6, 7 ou 8, vous pouvez le tester :

<form> <input id="file-upload" onchange="document.getElementById('thumb').src=this.value;" type="file" /> <img id="thumb" src="http://94.23.51.58/jpvincent/blog/wp-content/uploads/2010/05/avatar_JP_square.jpg" alt="" width="30" height="30" />
</form>Les autres browsers se donnent simplement le nom du fichier, pour des raisons pas si évidentes que ça de sécurité : Il y a 5 ans de ça j'ai même expérimenté un virus qui exploitait cet oubli de MS.

Voici un screenshot de IE7 avec un <em>alert( this.value ); :</em>

[caption id="" align="aligncenter" width="773" caption="dans l&#39;alert, le chemin complet du fichier sur le disque local"]<a href="http://jpv.typepad.com/.a/6a012875b0b7ba970c0120a6e214db970b-800wi"><img src="http://jpv.typepad.com/.a/6a012875b0b7ba970c0120a6e214db970b-800wi" alt="Screenshot IE" width="773" height="421" /></a>[/caption]

Et voici la même page vue sur un IE8 patché :

[caption id="" align="aligncenter" width="744" caption="Le chemin complet est préfixé avec C:fakepath&quot;"]<img src="http://jpv.typepad.com/.a/6a012875b0b7ba970c012875e4243f970c-800wi" alt="Bug_MS" width="744" height="444" />[/caption]

Le chemin est maintenant "<strong>C:fakepathnom_image</strong>" ... Ca sent le dévelopeur stressé qui quick fix discrètement un soft un peu trop gros pour lui :)

EDIT: Opera 10.10 fait quasi pareil ! : <em>C:fake_pathporsche_06032005_exif2.jpg</em>
