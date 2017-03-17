---
layout: post
title:  "État des lieux de l‘accessibilité de HTML5"
date:   2011-02-19 16:04:21 +0100
---
Après avoir affirmé à Paris Web 2010 que <a href="http://braincracking.org/?p=597">HTML5 était utilisable immédiatement en production</a>, un expert accessibilité m‘a repris en disant qu‘il était dangereux de dire que HTML5 était accessible (j‘en parlais au futur cela dit). Dans le cadre d‘un gros projet autour de HTML5, j‘ai depuis fait pas mal de recherches ce qui m‘a permis de mieux comprendre son intervention.
Je sais qu‘il est dangereux de parler en dehors de son domaine d‘expertise, mais il faut bien qu‘un développeur Web mette les pieds dans le plat et à tout le moins provoque le débat. Autant vous dire que si vous vous y connaissez en accessibilité, je prend tout ajout ou correction.

Autant vous le dire tout de suite, mes conclusions sont mitigées, et il se peut même que je revienne sur ce que j‘affirmais à l‘époque.
<!--more-->

<h2>Quel HTML5 ?</h2>
Le terme commençant à être flou, il vaut mieux le préciser à chaque fois qu'on l'évoque : pour une fois sur ce blog le HTML5 dont nous parlerons dans cet article représente uniquement <a href="http://www.w3.org/TR/html5/content-models.html#annotations-for-assistive-technology-products-aria">la spécification adoubée par le W3C</a>, celle qui représente l‘évolution de HTML4 avec les nouvelles balises, le nouvel algorithme de parsing, canvas et le multimédia.

<h2>Les nouvelles balises</h2>
<h3>Leur apport</h3>
Les balises comme <code>article</code>, <code>time</code>, <code>header</code>, <code>nav</code> ou <code>figure</code> apportent de la sémantique à votre page. Cela signifie concrètement plusieurs choses :
<ul>
  <li>une meilleure <strong>lisibilité du code</strong>, ce qui est important pour ceux qui l'écrivent et facilite la maintenance</li>
  <li>une (future) exploitation facilitée pour les tiers :  <a href="https://www.readability.com/">Readability</a> (Application de lecture, service de rétribution des auteurs de blogs) se base sur ces nouvelles balises (ou sur microformat) pour tirer les informations des articles des pages. Il est difficile de trouver d‘autres exemples pour le moment.</li>
  <li>un (futur) meilleur référencement : la position officielle de Google sur le sujet est qu‘ils adaptent leurs algos à toutes les pratiques émergentes. Si beaucoup de sites utilisent <code>article</code>, google finira par l‘interpréter.</li>
  <li>une (future) meilleure accessibilité : les navigateurs vont <a href="http://www.w3.org/TR/html5/content-models.html#annotations-for-assistive-technology-products-aria">rajouter automatiquement</a> des rôles ARIA sur certaines balises. <a href="http://www.html5accessibility.com/">Ce tableau</a> vous montre le support actuel, il est aujourd‘hui très limité : Firefox 4 pour certaines balises, Opéra pour les formulaires sont leaders</li>
</ul>
Bref, les apports immédiats pour tous les sites ne sont pas évidents. Il y aura tout de même un bonus pour ceux qui jouent l‘innovation et y passent avant les concurrents lorsque ces lecteurs de code source (services, moteurs de recherche, navigateurs) exploiteront cette mine d'informations.

<h3>Les problèmes</h3>
Le reproche principal fait aux nouvelles balises, c‘est que <abbr title="Internet Explorer versions 6 à 8">IE6-8</abbr> font disparaître du DOM les éléments qu‘il ne connaît pas, et donc ne les présentent pas aux technologies d‘assistance. Le moyen de contourner cela <a title="lien vers le projet HTML5 shim" href="http://code.google.com/p/html5shim/">est connu</a>, facile à mettre en place et <em>bullet-proof</em> mais il induit une <strong>dépendance envers JavaScript</strong>. Les derniers <a href="http://developer.yahoo.com/blogs/ydn/posts/2010/10/how-many-users-have-javascript-disabled/">chiffres français</a> pris sur la homepage de Yahoo! font état de 1.5% d‘utilisateurs dans cet état. La part de marché de IE est d‘environ 50%, cela vous laisse avec 0.7% d‘utilisateurs que cela peut gêner parmi vos visiteurs handicapés.

A ma connaissance, il y a 2 autres choses qui ont donné une mauvaise réputation à ces nouveaux éléments :
<ul>
  <li>une combinaison de certaines versions de <abbr title="Leader des lecteur d‘écran">JAWS</abbr> et de Firefox avait un bug qui faisait <strong>disparaître le contenu</strong> situé dans une balise <code>&lt;header&gt;</code>. En plus de la perte brute de contenu, selon où elle est placée, <code>&lt;header&gt;</code> peut contenir des titres ou des liens, 2 éléments fondamentaux pour la navigation avec les technologies d‘assistance.</li>
  <li>Une autre combinaison, celle de versions de <abbr title="Second sur le marché des lecteurs d‘écran">Windows-Eyes</abbr> et d‘Internet Explorer, n‘arrivait pas à lister les liens à l‘intérieur d‘éléments HTML5 avec une balise <code>role</code>. C‘est très spécifique mais là aussi il y a perte d‘éléments de navigation fondamentaux.</li>
</ul>
Doit on changer sa manière de coder parce 2 logiciels buguent ? Historiquement les développeurs Web ont toujours répondu oui à cette question, en changeant leur code pour s‘adapter aux bugs de IE par exemple. Sauf que IE est officiellement supporté par tous les sites Web alors que ce n‘est pas le cas pour ces 2 logiciels. Même si ces bugs ont été corrigés, <strong>ils restent majeurs</strong> (perte de contenu). Je n‘ai pas de chiffre sur le taux de pénétration de ces versions buguées, mais les <a href="http://webaim.org/projects/screenreadersurvey/">seuls chiffres publics</a> que j'ai pu trouver montrent que 25% de ces utilisateurs n‘ont pas mis à jour leur version après un an, ce qui signifie qu‘il faut prendre en compte ces bugs plusieurs années.

<strong>En conclusion</strong> : l‘introduction des nouvelles balises est surtout compromise par des bugs majeurs de certains logiciels importants et votre <strong>sentiment</strong> sur la dépendance à JavaScript (que vous devriez d‘ailleurs mesurer sur votre site). Elle est à mettre en balance avec les avantages de ces balises, qui eux ne sont pas immédiats.

<h2>Les zones de navigation</h2>
L‘introduction des balises <code>nav</code>, <code>header</code>, <code>footer</code> et <code>aside</code> est très intéressante car elle permet au navigateur de fournir aux technologies d‘assistance une carte de la page. Jusqu‘ici les utilisateurs de ces technologies naviguaient principalement avec la liste des titres et la liste des liens d‘une page, ils pourraient également avoir accès automatiquement aux zones classiques présentes sur les sites Web.

Première ombre au tableau, HTML5 <a href="http://www.w3.org/TR/html5/content-models.html#annotations-for-assistive-technology-products-aria">définit</a> un mapping strict entre certaines balises et des rôles de zone ARIA, mais sur ces zones là il n‘y a pas de rôle par défaut. Il y a donc contradiction entre la spécification et les rôles que les navigateurs devraient automatiquement implémenter ( <code>&lt;nav role="navigation"&gt;</code>, <code>&lt;header role="banner"&gt;</code>, <code>&lt;footer role="contentinfo"&gt;</code>, <code>&lt;aside role="complementary"&gt;</code>). Qui dit contradiction dit mésentente possible entre navigateurs.

Ensuite vient le support : la traduction automatique des balises en rôle de zone ARIA par les navigateurs arrive à peine, Firefox 4 étant pour le moment seul. Par contre ARIA est bien supporté par les versions récentes des lecteurs d‘écran. Sauf que comme pour les navigateurs, les clients de ces logiciels mettent un certain temps avant de passer aux nouvelles versions.

Enfin, ces rôles et ces balises sont censées permettre la disparition des liens d‘échappement, bonne pratique d‘accessibilité reconnue. Mais ça serait oublier ceux qui n‘utilisent pas ces logiciels, mais qui naviguent tout de même au clavier (choix, accident de souris, mauvais matériel, certains mobiles ...). A ma connaissance les navigateurs n‘ont pas prévu de fonctionnalité pour ces utilisateurs en leur mettant à disposition des raccourcis vers ces zones.

<strong>En conclusion</strong> : la technique du lien d‘évitement restera très longtemps un moyen de navigation plus universel. Si vous ne la pratiquiez pas, utiliser les nouvelles balises maintenant apportera naturellement de l‘accessibilité au fur et à mesure du déploiement des nouvelles versions de navigateurs et de technologie d‘assistance

<h2>Le multimédia</h2>
Les éléments <code>audio</code> et <code>video</code> natifs ont fait rêver un peu tout le monde, et sur le papier règlent d‘épineux problèmes. En pratique c‘est beaucoup plus compliqué (sur mon projet, nous avons jugé que les implémentations navigateur ne valaient pas encore Flash) et l‘accessibilité n‘est pas en reste. Les implémentations des navigateurs concernant la navigation clavier sont variées, parfois buguées et parfois inexistantes.

Les sous-titres ne sont implémentés nativement nul part et les spécifications ont vraiment beaucoup bougé à ce sujet. Difficile de savoir si le consensus actuel autour de l‘élément <code>track</code> et le format WebSrt va rester.

Comme vous devez de toute manière fournir une lecture de la vidéo en Flash, qui lui est naturellement encore moins accessible (il pourrait, mais les flasheurs ne le font pas), votre seule option est de coder vous même un lecteur vidéo accessible, en sortant les contrôles du lecteur (natif ou Flash) pour en faire des éléments HTML marqués avec <a href="http://dev.opera.com/articles/view/more-accessible-html5-video-player/#wai-aria">ARIA</a>, et d‘implémenter votre propre solution de sous-titrage en Javascript. Mais vous sacrifiez l‘option fullscreen, ce qui est rarement toléré.

<strong>Conclusion</strong> : les balises <code>video</code> et <code>audio</code> sont vraiment trop jeunes concernant l‘accessibilité, et il n‘y a pas de gros bénéfice à en tirer sur les navigateurs de bureau. Flash étant pire, vous devez améliorer vous même les choses en codant un player accessible ou en espérant en trouver un dans <a href="http://praegnanz.de/html5video/">cette longue liste</a>.

<h2>Les titres</h2>
HTML5 <a href="http://www.w3.org/TR/html5/sections.html#outlines">définit un algorithme</a> pour déterminer le vrai niveau des <code>Hx</code> d‘une page. Concrètement si vous mettez un <code>h1</code> dans une balise <code>section</code> ou <code>article</code> qui n‘est elle même pas enfant d‘une autre balise sectionnante, alors votre <code>h1</code> est en fait un <code>h2</code>. C‘est très pratique pour ceux qui développent des sites Web de façon modulaire : on ne s‘occupe que de la hiérarchie interne d‘un bout de page, sans avoir à se demander quel est le nombre de niveaux de titre déjà introduit. Pour voir cet algorithme en action, vous pouvez utiliser l‘outil <a href="http://code.google.com/p/h5o/">HTML5 outliner</a>.

En quoi est ce important ? les titres sont la première manière de naviguer sur une page lorsque l‘on utilise un lecteur d‘écran et selon le moteur HTML utilisé, la hiérarchie interprétée ne sera pas la même, et vous ne pouvez rien y faire. Honnêtement si vous n'aviez pas prêté attention à la hiérarchie globale des titres jusqu‘à présent, HTML5 vous aidera au contraire à mieux vous organiser module par module et obtenir au final quelque chose de cohérent au moins sur les nouveaux navigateurs. Si vous aviez une belle hiérarchie, alors pour la conserver sur tous les navigateurs, il vous faudra éviter les balises sectionnantes comme <code>article</code>, <code>section</code> ou <code>aside</code>.

Cela dit, une hiérarchie bien maîtrisée de titres n‘est réellement importante que si la page comporte beaucoup de titres, par exemple sur des sites à fort contenu textuel (longs articles de journaux, encyclopédie ...). La plupart des sites se contentent d‘une vingtaine de titres de niveau 1 et 2.

Conclusion : Si vous ne gériez pas votre hiérarchie parfaitement au niveau de la page (site très modulaire, maintenu par plusieurs personnes ou par méconnaissance), autant passer directement à la logique HTML5, vous devrez vivre avec une hiérarchie de titres différente selon le navigateur. Si vous aviez une belle hiérarchie et beaucoup de titres, alors pour la conserver sur tous les navigateurs, il vous faudra éviter les balises sectionnantes comme <code>article</code>, <code>section</code> ou <code>aside</code>.

<h2>Canvas</h2>
Canvas est effectivement un trou noir d‘où rien ne sort, et ironiquement même Flash peut être plus accessible que cette balise (encore une fois, si le développeur Flash a fourni un effort supplémentaire). Seul IE9 permet d‘accéder au <em>shadow DOM</em>, mais en supposant que tous les navigateurs fassent de même, la question de l‘intérêt se pose : pourquoi accéder à des paquets de pixels ? Si vous l‘utilisez pour obtenir des effets graphiques décoratifs, un contenu alternatif textuel peut suffire. Si vous l‘utilisez pour la navigation, vous vous êtes probablement trompé de technologie. Si votre application repose de manière justifiée sur cette technologie, comme pour un jeu, alors il me semble de toute façon impossible de rendre accessible ce type d‘application très graphique.

Conclusion : ressortez donc les bonnes pratiques, fournissez un contenu alternatif si c‘est possible et évaluez bien vos besoins : SVG / VML ou une navigation scriptée peuvent suffire.

<h2>Passer à ARIA</h2>
Ma conclusion personnelle est qu‘il faut rajouter ARIA et ses multiples rôles à votre page HTML5. Voici ce que j‘utilise occasionnellement pour tester l‘ajout d‘ARIA ou pour vérifier l‘accessibilité d‘une page. Pour tester ARIA, il vous faut un Windows, un Firefox et un Internet Explorer 8 qui sont les 2 navigateurs les plus utilisés par les clients des lecteurs d‘écran. Ce sont également les 2 navigateurs qui supportent le mieux ARIA. En lecteur d‘écran <a href="http://www.freedomscientific.com/fs_products/software_jaws.asp">JAWS</a> par Freedom Scientific est clairement le leader du marché, suivi par <a href="http://www.gwmicro.com/Window-Eyes/">Window-Eyes</a> de GW Micro. Ils offrent tous deux des version d‘essai gratuit dont la limite est que vous devez redémarrer Windows toutes les 40 minutes. Astuce : utilisez des machines virtuelles, et la fonctionnalité <em>snapshot</em> pour vous éviter ces redémarrages fastidieux. Vous pouvez également utiliser un lecteur d‘écran open-source : <a href="http://www.nvda-project.org/">NVDA</a> ainsi que la toolbar <a href="https://addons.mozilla.org/fr/firefox/addon/juicy-studio-accessibility-too/">Juicy Studio Accessibility Toolbar</a> pour Firefox qui permet de vérifier ses zones pendant le développement.

<h2>Conclusion</h2>
Vaste sujet que l‘accessibilité de HTML5, et j‘imagine (j‘espère!) que des experts accessibilité passeront sur ce blog pour me corriger ou rajouter d‘autres difficultés. En conclusion :
<ul>
 <li>il me faut l‘admettre, l‘introduction des nouvelles balises peut causer des bugs majeurs ou gêner des logiciels de lecture d‘écran. Ces bugs et cette gêne de la hiérarchie sont là pour plusieurs années tandis que les avantages des nouvelles balises ne sont pas encore massivement arrivés. A vous de voir
 <li>l‘utilisation des balises <code>video</code> et <code>audio</code> est prématurée à tous les points de vue
 <li>il va falloir encore du temps avant que HTML5 vous apporte plus d‘accessibilité que ARIA. Soit vous passez à ARIA immédiatement, soit vous pariez sur le futur
</ul>

Si le ton négatif de ce billet vous fait conclure qu‘il ne faut pas utiliser HTML5, alors je vous invite à me relire : j'ai principalement <strong>listés les problèmes</strong> afin que vous sachiez à quoi vous vous engagez en restant sur les techniques actuelles ou en partant sur certaines nouveautés de HTML5. HTML5 au sens des applications Web avec toutes ses nouvelles APIs JS ne sont ici pas concernées.
