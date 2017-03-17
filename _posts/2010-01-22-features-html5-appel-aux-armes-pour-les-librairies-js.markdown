---
layout: post
title:  "Features HTML5, appel aux armes pour les librairies JS"
date:   2010-01-22 16:04:21 +0100
---
<h1>Le constat</h1>
Cela fait une bonne année que les browser majeurs implémentent <strong>systématiquement</strong> de belles features HTML5, alors même que la spec n'est pas finalisée. D'un côté c'est tant mieux car
<ul>
  <li>ces implémentations font <strong>progresser les spécifications elle même</strong>. Le HTML working group ou le W3C peuvent voir en action sous Opéra ou FF ce qu'ils essayent de spécifier, et ils ont de vrais retours des implémenteurs, voire des développeurs web</li>
  <li>nous les développeurs web pouvont <strong>tester et se préparer</strong> à ce que sera le futur des applications web. Ceux qui prévoient la rétro-compatibilité peuvent même mettre des choses en production</li>
</ul>
Cependant, pour nous développeurs web :
<ul>
  <li>l'état non finalisé de la spec fait que les browsers implémentent la même fonctionalité <strong>avec des interfaces différentes</strong>, voire rajoutent des features certes intéressantes mais dont on ne sait pas si elles seront implémentées partout</li>
  <li>l'état du marché des browsers fait qu'on gère et <strong>gèrera encore pour longtemps</strong> des browsers qui n'<strong>implémentent pas du tout</strong> ces features :
<ul>
  <li>IE6 et IE7 sont encore là et resteront quelques annéees, le temps que Seven soit massivement adopté</li>
  <li>IE8 a fait un remarquable effort (DOM Storage, offline detector, JSON natif, CSS selector natif, onHashChange ...) mais on commence à se douter qu'<strong>aucune mise à jour n'implémentera de nouvelle feature majeure</strong> (geolocalisation, Web Worker, &lt;<em>video</em>&gt;, Canvas ...)</li>
  <li>les <strong>roadmaps</strong> de FF, Chrome, Safari et Opéra ne sont évidemment pas synchrones, donc la diffusion des features n'est jamais garantie même sur ces browsers modernes</li>
  <li>les habitudes de navigation changent et il faut prendre en compte la navigation sur les browsers riches mobiles. Un même user peut utiliser IE6 au bureau, continuer sur son Webkit/Opera mobile dans les transports et terminer sur son Firefox à la maison.</li>
</ul>
</li>
</ul>
Avant même de commencer à utiliser ces features, il faut d'abord passer du temps à comprendre et implémenter plusieurs API différentes, en plus d'une version de l'applicatif qui "doit faire sans".
<h1>Exemple concret : Storage</h1>
Mettons que je veuille développer une <strong>application de type Maps</strong>, qui affiche par exemple des photos sur une carte. Sans compter les images qui sont gérées par le cache du browser, au bout de quelques minutes de navigation, on va se retrouver avec des <strong>Mo's de données</strong> importées progressivement via XHR ne serait ce que pour lister le nom, l'endroit et l'url des images à placer sur la carte. Si l'utilisateur rafraichit la page, il perd tout.

La solution ici serait d'<strong>implémenter HTML5 <a href="http://dev.w3.org/html5/webstorage/">WEB Storage</a><span style="font-weight: normal;"> pour que l'utilsateur garde les données des photos sur son disque et lui garantir un affichage éclair des photos dans les zones déjà visitées</span></strong>

<strong><span style="font-weight: normal;">La spec est <strong>encore à l'état de DRAFT mais on trouve 4 browsers (50% des visiteurs) qui en ont implémenté une variation :</strong></span></strong>
<ul>
  <li>IE8 a <a href="http://msdn.microsoft.com/en-us/library/cc197062(VS.85).aspx">DOM Storage</a> (d'ailleurs je ne sais même plus quel est le nom officiel de la feature ...)</li>
  <li>FF2 puis FF3 ont <a href="https://developer.mozilla.org/en/DOM/Storage">DOM Storage</a>, ainsi que <a href="http://hacks.mozilla.org/2009/06/localstorage/">LocalStorage</a> depuis FF3.5</li>
  <li>Safari 4 appelle ça <a href="http://developer.apple.com/safari/library/documentation/iPhone/Conceptual/SafariJSDatabaseGuide/Name-ValueStorage/Name-ValueStorage.html#//apple_ref/doc/uid/TP40007256-CH6-SW1">Key-Value Storage</a>, et je ne sais pas si ça marche sous l'iPhone qui limite de toute façon fortement l'espace cache donné aux pages (EDIT: pobo me signale qu'avec certaines restrictions, l'iPhone aussi le supporte)</li>
  <li>Chrome inclue GEARS qui permet l'accès à une <a href="http://code.google.com/intl/fr-FR/apis/gears/api_database.html">Database</a>. Mais ils ont annoncé récemment qu'ils <a href="http://tr.im/GhbK">abandonneraient GEARS</a> au profit de HTML5</li>
</ul>
<strong>La fonctionalité globale est la même mais le moyen d'y accéder, le retour des fonctions et les limitations diffèrent.</strong> On est dans un cas bien plus complexe que le support de telle ou telle propriété CSS2, et autant dire qu'en tant que développeur on n'a même pas envie de commencer une implémentation, d'autant qu'il faut inclure dans le coup du développement le cas majoritaire : l'absence totale de Storage.

C'est assez représentatif de toutes les features HTML5 de manière générale, la seule solution viable est donc l'utilisation d'une libraire Javascript qui permet d'<strong>avoir une API uniforme</strong>.
<h1>L'unique solution : les librairies JS</h1>
Dans l'écosystème du développement web, c'est le rôle des librairies JS que d'<strong>aplanir les différences entre browsers</strong>, et dans l'exemple de WEB Storage, seul <a href="http://developer.yahoo.com/yui/storage/">YUI</a> a produit de quoi pouvoir utiliser cette feature sereinement :
<ul>
  <li>une manière <strong>uniforme</strong> d'accéder à 5 implémentations différentes (en incluant flash)</li>
  <li>YUI fait partie des grandes librairies dont le <strong>support à long terme</strong> est quasi assuré</li>
  <li>un <a href="http://developer.yahoo.com/yui/storage/#limitations">résumé des limitations</a> auxquelles s'attendre, qui permet de savoir avant même d'implémenter si la solution sera vraiment utile</li>
  <li>une <strong>documentation et des exemples</strong> qui sont autrement plus lisibles qu'une spec W3c</li>
  <li>bonus : via Flash on a même une <strong>solution de fallback quasi universelle</strong>, ce qui ne serait pas possible avec toutes les features HTML5 (geoloc par exemple)</li>
</ul>
Il semble y avoir des plugins pour <a style="color: blue !important; text-decoration: underline !important; cursor: text !important;" href="http://code.google.com/p/jquery-jstore/">Jquery</a> ou Dojo (impossible de retrouver une doc pour Dojo) mais ils m'ont semblé non supporté ou à moitié abandonné.

Si seul YUI a pu sortir quelque chose (signalons que Yahoo peut se payer une équipe à plein temps), c'est parce qu'on est arrivé je pense dans <strong>une autre ère du développement JS</strong> : on ne se contente plus en quelques lignes d'implémenter un <strong>.getElementByClassName() ou un accès aux events DOM uniforme</strong>, on a maintenant affaire a des features beaucoup plus compliquées, <strong>nouvelles</strong>, qui touchent aux <strong>ressources machine</strong>, avec lesquelles on <strong>communique surtout par event/callback</strong> et pour lesquelles <strong>les specs ne sont pas bien établies.
<span><span style="font-weight: normal;">Alors que le terrain de bataille actuel des librairies se fait surtout sur </span>les widgets<span style="font-weight: normal;">, les librairies n'ont plus le temps de revenir à leur raison d'être originale qui était d'uniformiser l'accès cross-browser.</span></span></strong>
<h1>Wish list</h1>
Il y a de la place pour tout le monde, voici des features HTML5 dont le développeur d'application Web d'aujourd'hui a besoin (dans le désordre) :
<ul>
  <li>Web Storage : fait par <a href="http://developer.yahoo.com/yui/storage/">YUI</a>, avec fallback flash, au poil</li>
  <li>OnHashChange event : pour la navigation en mode AJAX. Le fallback pourrait être une boucle qui monitore l'url ...</li>
  <li>&lt;<em>video</em>&gt; : pour la vitesse d'affichage d'une vidéo. Ca serait pas mal si on pouvait insérer un tag &lt;<em>video</em>&gt; à remplacer par un player flash s'il n'est pas supporté. Mais à ce jeu là je vois (malheureusement pour le monde du libre) h.264 sortir gagnant face à OGG</li>
  <li>le multi upload, avec barre de progression et preview : comme dans le récent FF3.6 le support du <em>multiple="true"</em> dans les <em>&lt;input type="file"&gt;</em>, de l'event onProgress pour indiquer où en est l'upload et l'affichage de la miniature de la photo qui est sur le disque. Et en fallback une des 2 millions d'implémentation flash déjà existante.</li>
  <li>le drag &amp; drop depuis le bureau : implémenté aussi dans FF3.6, la seule alternative est activeX ou java ... Il vaut peut être mieux se passer d'alternative pour cette fois.</li>
  <li>Canvas : IE a VML, les features sont comparables au point qu'<a href="http://sourceforge.net/projects/excanvas/">il existe déjà une librairie</a> qui fait le pont entre les 2</li>
  <li>mode offline : le fallback ne doit pas être très pratique (pinger le serveur de manière régulière jusqu'à tomber en timeout ...) mais c'est peut être jouable. Gérer le cache et retenir les infos à envoyer au serveur jusqu'à ce que l'on revienne online ne peut se faire qu'avec Storage (donc flash en fallback)</li>
  <li>Web Workers : pour faire de grosses opérations JS sans freezer le browser. Peut être simulable avec une iframe ?</li>
  <li>Geolocation : demander au browser où il se trouve physiquement. Pas de fallback possible si ce n'est la détection de l'IP. De toute façon les applications qui utilisent ces infos prévoient toujours en premier lieu un moyen d'indiquer manuellement sa position</li>
  <li>Input type calendar, mail etc... : pour par exemple afficher un calendrier lorsqu'on clique sur un input de type date (seul opéra le fait aujourd'hui). Toutes les libraires proposent des widgets calendar que l'on pourrait mettre en fallback</li>
</ul>
<h1>Les librairies ont le pouvoir</h1>
Tout cela nécessite une grosse force de développement et il est même possible que devant la rareté ce soient d'abord des <strong>librairies payantes</strong> qui fassent leur trou ... Mais les développeurs de framework peuvent trouver des <strong>alliés </strong>
<ul>
  <li><strong>dans les équipes des navigateurs</strong> qui pourraient partager leur expérience au nom d'un meilleur web</li>
  <li><strong>pourquoi pas chez Adobe</strong> vu le nombre de fois où flash est utilisé comme fallback ...</li>
  <li>et même avoir des <strong>représentants au W3C et au HTML working group</strong>, étant donné que le 1er contact des développeurs avec les nouvelles features passera d'abord par les librairies ...</li>
</ul>
Qui se lance ? :)
