---
layout: post
title:  "Usage avancé des fonctions JavaScript"
date:   2011-11-17 16:04:21 +0100
---
Cet article est un complément à l'article sur <a href="http://braincracking.org/?p=883">les 3 fondamentaux de JavaScript</a>, il vaut mieux être déjà à l'aise avec JavaScript avant de crier au scandale en voyant ce qu'on peut en faire. Pour reprendre un bon mot de quelqu'un qui avait assisté à ma <a href="http://www.slideshare.net/jpvincent/javascript-fondamentaux-et-oop">conférence sur JavaScript</a> :

[caption id="attachment_897" align="aligncenter" width="542" caption="javascript == la pornstar des langages de dev: souple, puissant, tu lui fait faire ce que tu veux, et ça peut finir bien crade."]<a href="https://twitter.com/#!/pierreca/status/134629949985398785"><img class="size-full wp-image-897  " title="pornstar" src="http://braincracking.org/wp-content/uploads/2011/11/pornstar.png" alt="" width="542" height="214" /></a>[/caption]

Admettons donc que vous ayez digéré sans problème les portées et les fonctions, passons à deux choses vraiment particulières à JavaScript :
<ol>
  <li>le renvoi de fonction qui permet de belles optimisations et qui ouvre la voie à des patterns que les amoureux de la théorie du langage apprécieront,</li>
  <li>une implémentation de <strong>classe statique</strong>, pour reprendre le terme utilisé en PHP ou en Java.</li>
</ol>
Et enfin nous verrons une proposition d'implémentation de deux <em>design pattern</em> célèbres et particulièrement utiles en JavaScript : <em>Singleton</em> et <em>Factory</em>.
<!--more-->
<h1>Classe statique</h1>
Pour rappel, en PHP et dans d'autres langages, une propriété ou une méthode statique peut être appelée sans que la classe n'ait été instanciée pour créer un objet. C'est généralement là que l'on range les constantes ou les fonctions utilitaires par exemple. En JavaScript, tout étant objet y compris les fonctions, cela se fait assez naturellement :
<pre><code>// constructeur
var <strong>myClass</strong> = function () { 
};
<strong>myClass.staticMethod</strong> = function() {
  console.log('OK');
};
// que voit on au niveau global ?
myClass.staticMethod(); // OK
</code></pre>
Regardez la manière dont est définie <code>staticMethod</code> : on la range directement dans la fonction <code>myClass</code> ! Elle est donc directement disponible sans passer par la création d'un objet. Comparons d'ailleurs avec une définition de méthode d'objet comme on l'a vu dans les paragraphes précédents pour bien comprendre où sont disponibles ces nouvelles méthodes de classe.
<pre><code>// constructeur
var myClass = function () { 
  return { 
    publicMethod:function() {
      console.log('OK');
    }
  }
};
myClass.staticMethod = function() {
  console.log('OK');
};

// que voit on au niveau global ?
myClass.publicMethod(); // Error
myClass.staticMethod(); // OK

// que voit l'instance ?
myObject = myClass(); 
myObject.publicMethod(); // OK
myObject.staticMethod(); // Error 
</code></pre>
Si vous exécutez ce code dans votre console, vous allez voir où se produisent les erreurs :
<ul>
  <li>vous ne pouvez pas accéder à <code>publicMethod</code> sans avoir d'abord instancié <code>myClass</code>,</li>
  <li>l'instance de <code>myClass</code> ne contient pas <code>staticMethod</code> car celle ci est directement disponible à partir du nom de la classe.</li>
</ul>
<h1>Renvoi de fonction</h1>
Une fonction peut se redéfinir elle même quand elle s'exécute. Ca a l'air sale dit comme ça, mais nous allons voir un cas concret où cela est bien utile. Imaginez que vous construisez une mini-librairie dont une des fonctions permet de se rattacher à un événement du DOM. Pour supporter tous les navigateurs, il y a deux méthodes aux noms distincts et aux arguments légèrement différents que vous pouvez encapsuler dans une fonction en faisant un simple <code>if</code>.
<pre><code>var onDOMEvent =
  function( el, ev, callback) {
    // le monde de Microsoft
    <strong>if(document.body.attachEvent){</strong>
      el.attachEvent('on'+ev, callback);
    // le monde du W3C
    } else {
      el.addEventListener( ev, callback, false);
    }
  };
</code></pre>
Cette fonction marche très bien, mais à chaque fois qu'elle est exécutée, le test sur la condition aussi est exécutée. Si vous faîtes une librairie vous vous devez de ne pas ralentir les développeurs qui l'utiliseraient. Hors cette fonction pourrait très bien être appelée des centaines de fois, exécutant ainsi inutilement du code. Pour optimiser cela, nous allons redéfinir la fonction à la volée lors de sa première exécution.
<pre><code>var onDOMEvent =
function( ) {
  if(document.body.attachEvent) {
    <strong>return function</strong>(element, event, callback) {
      element.attachEvent('on'+ event, callback);
    };
  } else {
    <strong>return function</strong>(element, event, callback) {
      element.addEventListener( event, callback);
    };
  }
}<strong>()</strong>;
</code></pre>
Comme vous le voyez :
<ul>
  <li>cette fonction est auto-exécutée grâce aux deux parenthèses finales, et n'a plus besoin des arguments puisqu'elle ne sera plus jamais exécutée après.</li>
  <li>le <code>if</code> reste, mais il ne sera exécuté qu'une seule fois.</li>
  <li>la fonction <strong>renvoie des fonctions anonymes</strong> contenant les codes spécifiques au navigateur. Notez que ces fonctions attendent toujours les mêmes paramètres.</li>
  <li>lorsque <code>onDOMEvent()</code> sera appelé, seul l'un ou l'autre corps de fonction sera exécuté, nous avons atteint notre objectif</li>
</ul>
C'est une technique d'optimisation pas forcément évidente à intégrer mais qui donne de très bons résultats. Cela irait un peu trop loin pour cet article mais si vous avez l'âme mathématique, cherchez donc sur le Web comment calculer la suite de Fibonacci en JavaScript, avec et sans "memoization" (<a href="http://fr.wikipedia.org/wiki/M%C3%A9moization">Wikipedia</a>). Vous pouvez également créer des fonctions spécialisées qui capturent certains paramètres pour vous éviter d'avoir à les préciser à chaque fois, technique connue sous le nom de <em>currying</em> (voir ce <a href="http://ejohn.org/blog/partial-functions-in-javascript/">post de John Resig</a> à ce sujet).
Autre cas concret d'école d'utilisation de cette technique. Partons du code suivant qui boucle sur un petit tableau d'objet et qui rattache l'événement <code>onload</code> à une fonction anonyme.
<pre><code>var queries = [ new XHR('url1'), new XHR('url2'), new XHR('url3')];
for(var i = 0; i &lt; queries.length; i++) { 
  queries[i].onload = function() { 
    console.log( i ); // référence 
  }
} 
</code></pre>
Observez bien la valeur de <code>i</code> : notre fonction anonyme crée une portée, le parseur javascript ne voit pas <code>i</code> dans cette fonction, il remonte donc d'un niveau pour la trouver. Jusqu'ici tout va bien notre variable est bien référencée. Pourtant lorsque l'événement <code>onload</code> est appelé par le navigateur, nous avons une petite surprise:
<pre><code>queries[ 0 ].onload(); // 3! 
queries[ 1 ].onload(); // 3! 
queries[ 2 ].onload(); // 3!</code></pre>
<code> </code>
L'interpréteur JavaScript a correctement fait son boulot : la fonction <code>onload</code> voit bien <code>i</code> (sinon nous aurions eu <code>undefined</code>), mais c'est une <strong>référence</strong> vers la variable, pas une copie de sa valeur ! Hors <code>onload</code> n'est appelé qu'après que la boucle se soit terminée, et <code>i</code> a été incrémentée entretemps. Pour fixer cela, nous allons utiliser deux choses :
<ol>
  <li>l'auto-exécution, qui va nous permettre de copier la valeur de <code>i</code></li>
  <li>le renvoi de fonction pour que <code>onload</code> soit toujours une fonction</li>
</ol>
Attention, ça peut piquer les yeux :
<pre><code>for(var i = 0; i &lt; queries.length; i++) { 
  queries[i].onload =  function(i) { 
    <strong>return function() { </strong>
      console.log( i ); // valeur 
    }; 
  }<strong>(i);</strong> // exécution immédiate 
} 
// plus tard ... 
queries[ 0 ].onload(); // 0 
queries[ 1 ].onload(); // 1 
queries[ 2 ].onload(); // 2
</code></pre>
Essayez de suivre le chemin de l'interpréteur :
<ul>
  <li><code>i</code> est donné à la fonction anonyme auto-exécutante</li>
  <li>le paramètre de cette première fonction anonyme s'appelle aussi <code>i</code> : dans cette portée locale, <code>i</code> a pour valeur 0 (pour la première passe)</li>
  <li>la fonction renvoyée embarque toute la portée avec elle et n'a donc que la <strong>valeur</strong> de ce nouveau <code>i</code>, qui ne bougera plus</li>
</ul>
Pour info, ce cas d'école est souvent posée lors des entretiens d'embauche si on veut vérifier que vous avez bien compris les portées.
<h1>Implémenter des design pattern</h1>
En combinant namespace (voir l'article <a href="http://braincracking.org/?p=89">JavaScript pour les développeurs PHP</a>), portée maîtrisée, espace privé et émulation d'objets, nous allons implémenter le <em>design pattern Factory</em>. Factory ou Singleton sont très intéressants en JavaScript, notamment pour les <em>widgets</em> (type jQuery UI) : vous pouvez vous retrouver sur des pages où vous ne savez pas si le JavaScript d'un Widget s'est déjà exécuté sur tel élément du DOM. Pour optimiser, vous ne voulez pas recréer systématiquement le Widget mais plutôt <strong>créer une nouvelle instance ou récupérer l'instance en cours</strong>. Vous avez donc besoin :
<ol>
  <li>d'interdire la création directe d'un objet</li>
  <li>de passer par une fonction qui va faire la vérification pour vous et vous renvoyer une instance</li>
</ol>
Commençons par créer notre espace de travail, ainsi que notre namespace:
<pre><code>(function(){
// création ou récupération du namespace et du sous-namespace
MY = window.MY || {};
MY.utils = MY.utils || {};
// constructeur 
MY.utils.XHR=function( url ){
  console.log( url );
};
})();
</code></pre>
A ce stade nous avonc une classe accessible de l'extérieur avec <code>new MY.utils.XHR( url );</code>. C'est bien mais pas top, nous voudrions passer par une fonction qui va vérifier s'il n'existe pas déjà une instance avec ce même paramètre URL. Nous allons déclencher une erreur en cas d'appel direct (<a href="http://www.php.net/manual/fr/language.oop5.patterns.php#language.oop5.patterns.singleton">comme en PHP</a>) et prévoir une fonction <code>getInstance( url )</code> qui va se rattacher au namespace de la fonction constructeur.
<pre><code>(function(){
// création ou récupération du namespace et du sous-namespace
MY = window.MY || {};
MY.utils = MY.utils || {};
// constructeur 
MY.utils.XHR=function( url ){
  <strong>throw new Error('please use MY.utils.XHR.getInstance()');</strong>
};
// factory
<strong>MY.utils.XHR.getInstance</strong> = function( url ) {
};
})();
</code></pre>
Enfin nous allons introduire une variable privée qui va contenir la liste de nos instances (un objet <code>currentInstances</code> avec en index l'url et en valeur l'instance). Nous allons également rendre privé notre vrai constructeur.
<pre><code>(function(){
// constructeur 
MY.utils.XHR=function( url ){
  throw new Error('please use MY.utils.XHR.getInstance()');
};

//constructeur privé
<strong>var XHR = function( url ){</strong>
  console.log( url );
};
// liste privée d'instances
<strong>var currentInstances = {};</strong>

// factory
MY.utils.XHR.getInstance = function( url ) {
  // déjà créé ? on renvoie l'instance
  if(currentInstances[url]) {
    return <strong>currentInstances[url]</strong>;
  // on crée, on enregistre, on renvoie
  } else {
    return currentInstances[url] = <strong>new XHR(url);</strong>
  }
};
})();
</code></pre>
Telle quelle, cette implémentation permet déjà de créer une factory pour des objets qui ont besoin d'être unique mais qui n'ont qu'un seul paramètre (<code>id</code> d'un objet DOM, URL ...). La vraie raison d'être d'une Factory, c'est de gérer des objets complexes à instancier, à vous donc d'étendre ses fonctionnalités. Les puristes auront remarqué que l'objet renvoyé n'était pas du type <code>MY.utils.XHR</code>, et qu'on ne pouvait donc pas faire de vérification avec <code>instanceof</code> du type de l'objet. Honnêtement je ne connais pas de bon moyen de le faire, à vous de voir si c'est un manque dans votre code.
Vous voilà paré à écrire du code un peu plus maintenable et mieux rangé, et vous pourrez crâner dans les dîners en ville en racontant vos exploits de codeur JS orienté objet.
<h1>Conclusion</h1>
<ul>
  <li>JavaScript a des concepts différents des langages majeurs et devient extrêmement important sur votre CV. Prenez le temps de l'apprendre</li>
  <li>Les librairies telles que jQuery ne sont pas faites pour couvrir les cas que nous venons de voir. jQuery permet d'utiliser le DOM sereinement, pas d'organiser votre code proprement.</li>
  <li>J'ai essayé d'être pratique, mais lire un post de blog ne suffira jamais pour comprendre des concepts de programmation : codez !</li>
</ul>
