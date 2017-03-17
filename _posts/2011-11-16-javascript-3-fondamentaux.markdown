---
layout: post
title:  "JavaScript : 3 fondamentaux"
date:   2011-11-16 16:04:21 +0100
---
Après quelques années à écrire dans un langage, on finit facilement par oublier les premières difficultés que l'on avait rencontrées. Et à force de faire de la veille, de l'autoformation et de parler entre experts dans des conférences, j'ai un peu quitté la réalité de la majorité des équipes Web.

Maintenant que je suis consultant indépendant je retourne dans des équipes qui avaient autre chose à faire que de se demander si on a le droit de parler de classe en JavaScript, quelle est la bonne définition d'une <em>closure</em>, ou quelles sont les fonctionnalités de EcmaScript 5 qui auraient du rester dans Ecmascript.Next.

J'avais déjà parlé sur ce blog de <a href="http://braincracking.org/?p=89">JavaScript et la programmation orienté objet pour les développeurs PHP</a>, nous allons explorer ici les <strong>3 notions fondamentales de JavaScript</strong> qui sont probablement les plus grosses sources de bugs, d'incompréhension et de frustration pour le développeur Web moyen. Et qui accessoirement sont la base d'une programmation plus évoluée par la suite.
<h1>JavaScript est différent : apprenez le</h1>
Le monde du développement Web semble dominé par les langages dérivés de la syntaxe du C, PHP en tête, avec des paradigmes qui se ressemblent. Forcément en abordant JavaScript dont la syntaxe n'est pas vraiment révolutionnaire, on est tenté d'appliquer la même logique. Mais c'est oublier un peu vite que ce langage a été créé il y a déjà 15 ans, quand Java était seulement à mode et pas encore ultra dominant comme aujourd'hui, et qu'il est principalement l'héritier de langages comme Erlang et Lisp, aujourd'hui très peu connus. En fait le mot Java dans JavaScript a été rajouté pour des raisons commerciales, et seuls quelques concepts comme la syntaxe et je crois la gestion des dates ont contribué à former JavaScript. JavaScript n'est donc qu'un cousin éloigné des langages <em>mainstream</em>.

Le maître mot de ses concepteurs semble avoir été la versatilité. Nous allons voir qu'en explorant seulement 3 bases <em>à priori</em> simples, il est possible d'obtenir à peu près n'importe quoi, ce que les grands maître de JavaScript s'amusent tous les jours à faire. En attendant de passer dans la catégorie des maîtres, il faut déjà maîtriser ces bases pour faciliter son travail au quotidien.

Nous allons donc nous baser sur EcmaScript 3 (le javascript de IE6-7-8) dont les trois fondamentaux sont :
<ul>
  <li>La portée des variables ( <code>var + function</code>)</li>
  <li>Les fonctions</li>
  <li>Le contexte (<code>this</code>)</li>
</ul>
Commençons par la portée.
<!--more-->
<h1>La portée des variables</h1>
<h2>La théorie</h2>
La portée, c'est ce que voit l'interpréteur à un moment donné de l'exécution du code. Ouvrez un fichier JS, tapez le mot clé <code>debugger;</code> où bon vous semble, démarrez firebug ou votre outil préféré et cherchez l'onglet "Espions" ou "Watch".

[caption id="attachment_884" align="aligncenter" width="490"]<a href="http://braincracking.org/wp-content/uploads/2011/11/firebug-javascript-2.gif"><img class="size-full wp-image-884" title="Onglet Watch de Firebug" alt="" src="http://braincracking.org/wp-content/uploads/2011/11/firebug-javascript-2.gif" width="490" height="232" /></a> L'onglet Watch ou Espions de Firebug[/caption]

La règle pour comprendre comment est créée une portée est très simple :
<p style="text-align: center;">1 function = 1 portée</p>
Concrètement vous avez la portée globale où tout se trouve (dans un navigateur, c'est <code>window</code>), puis à chaque fonction que vous créez, vous créez une nouvelle portée, et celles ci peuvent s'imbriquer à l'infini. Chaque fois que vous précédez une assignation par le mot clé <code>var</code>, la variable n'est visible qu'à partir de cette portée.
<pre><code>test1 = function() {
  var x = 1;
  test2 = function() {
    var x = 2;
    test3 = function() {
      var x = 3;
      console.log(x); // <strong>3</strong>
    }();
  }();
  console.log(x); // <strong>1</strong>
}();
console.log(x); // <strong>undefined</strong>
</code></pre>
Si vous exécutez le code précédent dans votre console JavaScript, vous constaterez 3 choses :
<ul>
  <li>le <code>console.log</code> le plus profond affiche bien sur la dernière valeur de <code>x</code>, à savoir 3. Dans ce troisième niveau, on a écrasé les valeurs précédentes de <code>x</code>.</li>
  <li>malgré cet écrasement à un niveau inférieur, le second <code>console.log</code> (qui affiche 1) montre bien que les nouvelles valeurs de <code>x</code> ne sont pas visibles depuis la première fonction.</li>
  <li>enfin le dernier <code>console.log</code> est fait au niveau global, en dehors des définitions de fonction. Ici <code>x</code> n'existe même pas.</li>
</ul>
<h2>La pratique</h2>
Nous avons vu comment le couple <code>var</code> + <code>function</code> permet de définir une portée et empêche le code en dehors de la portée de voir certaines variables. Pourquoi ne pas tout mettre au niveau global ? Pour des raison d'organisation du code et de maintenabilité ! Imaginez le code suivant :
<pre><code>function genericFunctionName() {
  for(<strong>i</strong> = 0; i &lt; myArray.length; i++) {
    <strong>console.log(i);</strong>
  }
}
for(<strong>i</strong> = 0; i &lt; 10; i++) {
  genericFunctionName();
}
</code></pre>
Nous avons simplement une fonction qui parcourt un tableau fictif, puis nous appelons cette fonction 10 fois. N'exécutez surtout pas cela dans votre navigateur, car <strong>c'est une boucle infinie</strong> !

En fait les deux boucles <strong>for</strong> utilisent le même nom de variable (<strong>i</strong>) pour compter les tours. C'est un nom extrêmement commun et ça ne poserait pas de problème si les deux boucles ne pointaient pas sur <strong>la même variable</strong> ! Fixons cela.
<pre><code>function genericFunctionName() {
  for( <strong>var i</strong> = 0; i &lt; myArray.length; i++) {
    console.log(i);
  }
}
for(i = 0; i &lt; 10; i++) {
  genericFunctionName();
}
</code></pre>
Dans la fonction, nous avons rajouté le mot clé <code>var</code> pour spécifier que la variable n'appartenait qu'à cette fonction. Les deux bouclent ne pointent plus sur la même variable même si leurs noms sont les mêmes. C'est très important pour le développeur de savoir qu'il est le seul maître sur les variables qu'il crée pour créer un code particulier. Ceci nous donne la première application concrète des portées.
<h2>Créer son espace de travail sécurisé</h2>
En tant que développeur, vous allez créer ou modifier du code sur des pages dont vous ne maîtrisez pas l'environnement. Entre l'historique du code, vos collègues et les publicités ou les widgets, beaucoup de variables sont créées ou écrasées au niveau global sans que vous ne puissiez le prévoir. Au milieu de cette tourmente, je vous propose de vous créer un petit havre de paix :
<pre><code><strong>(function() { </strong>
<strong>}()) </strong>
</code></pre>
Je vous conseille de démarrer tout fichier javascript ou tout code entre balise <code>&lt;script&gt;</code> par la première ligne et de toujours terminer par la dernière. Ce bout de code vous permet de poser les bases de votre portée et de créer des variables qui n'appartiennent qu'à vous, sans pour autant s'isoler du reste du monde.
<pre><code>(function() {
  var privateVariable = true;
  window.init = function() {
    console.log( privateVariable );
  }
}())
init(); // <strong>true</strong>
console.log(privateVariable); // <strong>undefined</strong> </code></pre>
Exécutez ce code, et vous verrez que <code>privateVariable</code> n'est pas accessible de l'extérieur, mais que le code défini à l'intérieur permet d'y accéder. Très pratique en environnement hostile. Vous remarquerez aussi que l'on rattache la fonction <code>init</code> directement à la portée globale, afin qu'elle soit elle aussi visible de l'extérieur.
<h1>Les fonctions</h1>
<h2>C'était si simple ...</h2>
Il est facile de créer une fonction et de l'exécuter. Les syntaxes que vous utilisez au quotidien sont celles ci :
<pre><code>// création v1
function action() {}
// création v2
action = function() {};
// exécution
action();
</code></pre>
Simple non ?
<h2>La fausse bonne idée</h2>
Au fait quelle est la différence entre les deux manières de déclarer une fonction ? Pour la première ( <code>function nom()</code> ) le compilateur JavaScript extrait toutes les définitions et les met à disposition au niveau global AVANT de commencer l'exécution. Concrètement vous pouvez même exécuter la fonction avant qu'elle ne soit créée dans la source.
<pre><code>// exécution avant la définition !
action();
// création
function action() {}
</code></pre>
Cool ? Pas vraiment : c'est systématiquement la dernière déclaration dans la source qui est prise en compte par les interpréteurs JS. Vous ne pouvez pas conditionner la définition des fonctions. Par exemple le code suivant ne va pas s'exécuter comme vous pourriez le penser :
<pre><code>
if( 1 == 1 ) {
  // l'exécution du code passe ici ...
  function action() {
    console.log('a == 1');
  }
} else {
  // ... pourtant le compilateur se souvient de cette fonction
  function action() {
    console.log('a != 1');
  }
}
action(); // <strong>a != 1 </strong>
</code></pre>
Lors de l'exécution, l'interpréteur passe bien dans le premier <code>if</code> et pourtant c'est la seconde définition qui est retenue, uniquement parce qu'elle arrive en dernier dans la source.
Et malgré cela, contrairement à d'autres langages, la déclaration d'une fonction n'est valable que pour la portée en cours et ses descendantes. Dans le code suivant, la fonction <code>action</code> n'est pas visible au niveau global :
<code></code>
<pre>(function() {
  function action() {
    console.log('action');
  }
}())
action(); // undefined</pre>
La manière traditionnelle de déclarer les fonctions est donc assez trompeuse et de ce fait tombe doucement en désuétude. Pour des débutants on utilisera plutôt la seconde méthode qui a le mérite de pouvoir <strong>explicitement</strong> choisir la portée de ses fonctions, au même titre qu'une variable
<pre><code>var action = function() {};</code></pre>
En ce qui concerne le compilateur cette syntaxe est une variable avec pointeur sur une fonction anonyme. Le désavantage est que lorsque vous utilisez un débogueur pas à pas, la pile d'appels (<em>call stack</em>) ne vous affiche pas le nom des fonctions (l'anonymat sur Internet existe donc). Vous pouvez retrouver le nom de la variable assignée simplement en cliquant sur la fonction et en regardant dans la source.
<h2>Particularités Javascript</h2>
JavaScript a rajouté plusieurs particularités aux fonctions qui les rendent terriblement puissantes et flexibles. En fait toutes les constructions de code un peu élaborées telles que l'orientation objet ou l'application de <em>design pattern</em> se basent sur ces spécificités. Nous entrons donc dans le coeur de JavaScript.
<h3>Auto-exécution</h3>
En rajoutant simplement une paire de parenthèses après une déclaration, la fonction va immédiatement être exécutée.
<pre><code>var autoInit = function() {
  console.log('hello world');
}<strong>()</strong>;
// <strong>hello world</strong>
</code></pre>
Comme nous l'avons vu tout à l'heure, cela vous permettra principalement de créer des portées, pour protéger l'ensemble de votre script, et nous allons voir dans une minute que vous pouvez aussi l'utiliser dans certains cas particuliers.
<h3>Classe ou fonction ?</h3>
En JavaScript, tout est objet. Un objet n'est jamais qu'une collection de clés et de valeurs, et les valeurs peuvent être tout et n'importe quoi y compris d'autres objets. Lorsqu'une fonction est créée, automatiquement JavaScript y rajoute la propriété <code>prototype</code>. Tapez le code suivant dans votre console JavaScript :
<pre><code>var myFunction = function() {};
console.log( myFunction.prototype ); // <strong>Object</strong>
</code></pre>
Nous avons donc un objet <code>myFunction.prototype</code> et nous pouvons directement y accéder pour y rajouter des valeurs, comme d'autres fonctions par exemple. Ensuite pour accéder à ces fonctions, nous allons utiliser le mot clé <code>new</code>.
<pre><code>// cette innocente fonction devient un constructeur
var myClass = function () {
  <strong>this.</strong>publicVariable = 0;
};
// accès au prototype
<strong>myClass.prototype</strong> = {
  decrement:function() {
    console.log( --this.publicVariable );
  },
  increment:function() {
    console.log( ++this.publicVariable );
  }
};

myObject = <strong>new myClass</strong>();
myObject.decrement(); // -1
myObject.decrement(); // -2

myObject2 = <strong>new myClass</strong>();
myObject2.increment(); // 1
myObject2.increment(); // 2
</code></pre>
Entre nous, nous venons de créer quelque chose qui ressemble furieusement à une classe, avec une instanciation (<code>new myClass</code>), des variables propres à chaque instance ( <code>this.publicVariable</code> ) et un constructeur (le corps de la fonction).
<h3>Orienté objet ?</h3>
Si vous trouvez bizarre qu'une fonction se transforme soudainement en classe puis en objet, attendez de voir la suite. Toujours grâce à la notion du tout objet de JavaScript, nous pouvons également faire de l'héritage ! Là encore, en utilisant <code>.prototype</code>, nous allons créer une nouvelle fonction (ou classe) qui va hériter des méthodes de la première. Outre la déclaration de la nouvelle fonction, cela ne va prendre que deux lignes.
<pre><code>mySubClass = function() {
  // maintenant on part de 10 au lieu de 0
  this.publicVariable = 10;
};
<strong>mySubClass.prototype = myClass.prototype;
mySubClass.prototype.constructor = mySubClass;</strong>

myObject2 = new mySubClass();
myObject2.increment(); // 11
myObject2.increment(); // 12
</code></pre>
La première ligne après la déclaration de fonction fait pointer l'objet <code>.prototype</code> de notre nouvelle fonction vers l'objet <code>.prototype</code> de la classe mère, empruntant ainsi toutes ses propriétés. La seconde ligne est là pour rectifier un défaut de l'écrasement de <code>.prototype</code> : on refait pointer <code>.prototype.constructor</code> (encore une propriété automatiquement créée pour toutes les fonctions) vers le bon constructeur.
<h3>Slip ou caleçon ?</h3>
Pardon je voulais dire : pour les classes, vous êtes plutôt prototype ou closure ? On vient de voir que grâce à <code>.prototype</code> on pouvait émuler une classe en JavaScript. Les fonctions peuvent également renvoyer des objets simples contenant des propriétés et des méthodes, et la manière de les utiliser se rapprochera toujours terriblement d'une classe.
<pre><code>myClass = function () { 
  // variable privée !
  var privateVariable = 0; 
  // méthodes publiques
  <strong>return { </strong>
    decrement:function() { 
      console.log( --privateVariable ); 
    }, 
    increment:function() { 
      console.log( ++privateVariable ); 
    } 
  <strong>}</strong> 
};
myObject = myClass(); 
myObject.decrement(); // -1 
myObject.decrement(); // -2 
myObject2 = myClass(); 
myObject2.increment(); // 1 
myObject2.increment(); // 2
</code></pre>
Plusieurs différences par rapport à la version prototype :
<ul>
  <li>Notez le <code>return {...}</code> dans la définition de la fonction : vous pouvez y mettre exactement ce que vous auriez mis dans le prototype.</li>
  <li>Vous pouvez utiliser la portée de la fonction constructeur pour avoir des variables privées (ici <code>var privateVariable</code>), alors qu'il n'est <strong>pas possible d'avoir des variables privées avec prototype</strong></li>
  <li>Lorsque vous instanciez votre classe, vous n'avez plus besoin d'utiliser <code>new myClass()</code>, un simple appel sans <code>new</code> suffit.</li>
  <li>Le mot clé <code>this</code> désigne l'objet renvoyé, alors qu'avec prototype il désignait le corps de la fonction constructeur. Généralement avec closure on n'utilise plus du tout <code>this</code></li>
  <li>la création des fonction est évalué à chaque fois que vous invoquez la fonction constructeur, tandis que ce n'est fait qu'une seule fois avec prototype. <strong>N'utilisez jamais closure avec des objets qui peuvent être instanciés des centaines de fois</strong>.</li>
</ul>
Voilà pour l'essentiel sur les fonctions : avec cela vous maîtrisez déjà leur portée et vous pouvez commencer à développer orienté objet, ce qui va vous permettre d'éviter beaucoup de bug et d'avoir un code plus maintenable.
<h1>Le contexte ou this</h1>
<code>this</code> est le petit mot clé qui perd beaucoup de développeurs, notamment lorsque l'on est habitué aux dérivés du C comme PHP. En JavaScript comme ailleurs, il fait référence au contexte d'exécution, donc le code suivant s'exécutera probablement comme vous vous y attendez :
<pre><code>myClass = function() { 
  this.id = <strong>'myClass'</strong>; 
} 
myClass.prototype = { 
  action:function() { 
    console.log( <strong>this.id</strong> ); 
  } 
}; 
myObject = new myClass(); 
myObject.action(); // <strong>'myclass'</strong>
</code></pre>
Ici on a défini dans le constructeur <code>this.id</code> avec une certaine valeur, puis on instancie la classe (<code>new myClass()</code>) et la méthode <code>action()</code> accède tout naturellement à <code>this</code> pour retrouver la valeur de <code>id</code>.
Pourtant lorsque vous voulez exécuter <code>myObject.action</code> au clic sur un élément du DOM, plus rien ne marche comme prévu.
<pre><code>document.body.onclick = myObject.action; 
// document.body.id
 </code></pre>
<code> </code>
Vous pouvez exécuter les deux bouts de code précédent dans votre console JavaScript et cliquer sur la page, vous verrez que <code>this.id</code> n'est plus 'myClass' mais fait référence à l'<code>id</code> du <code>body</code>, qui n'existe peut être même pas. Ceci n'est pas particulier au DOM d'ailleurs, imaginez que vous créiez un autre objet qui aurait un événement <code>onfire</code> que vous auriez besoin d'écouter :
<pre><code>myEvent = { 
  id:'myEvent' 
};
myEvent.onfire = myObject.action; 
myEvent.onfire(); // 'myEvent' au lieu de 'myClass'
</code></pre>
<code> </code>
Lorsque <code>onfire()</code> s'exécute, il exécute bien le corps de la fonction <code>action</code>, mais comme pour les objets du DOM, <strong><code>this</code> fait référence à l'objet d'où est exécutée la fonction</strong>, pas forcément à l'objet d'origine de la fonction.
Ceci est du à la versatilité de JavaScript qui permet de très simplement copier le corps d'une fonction d'un objet à l'autre, alors que dans les langages traditionnels orienté objet il vous faut explicitement faire de l'héritage. Comment fixer cela ? Il y a au moins deux méthodes.
<h2>Changement de contexte (natif)</h2>
La première méthode est native à JavaScript et utilise la fonction <code>.call()</code>. Ici seule la dernière ligne a changé :
<pre><code>myClass = function() { 
  this.id = 'myClass';
} 
myClass.prototype = { 
  action:function() { 
    console.log(this.id); 
  } 
}; 
myObject = new myClass(); 
myEvent = { 
  id:'myEvent' 
};
myEvent.onfire = myObject.action; 
<strong>myEvent.onfire.call( myObject ); // myClass</strong>
</code></pre>
<code> </code>
Le premier argument de la fonction <code>.call()</code> est l'objet qui sera utilisé pour <code>this</code>. Ici on redonne à <code>onfire</code> l'objet d'origine de la fonction <code>action</code>.
Le problème avec cette solution, c'est qu'il faudrait que vous soyez certain que TOUS les endroits où votre fonction peut être appelée vont rectifier le contexte d'exécution. Hors ne serait ce que pour les événement natifs du DOM cela n'est pas possible, sauf si vous utilisez une bibliothèque de manière systématique. Donc faire confiance à <code>this</code> n'est raisonnable que si vous maîtrisez parfaitement tout le code qui sera écrit.
<h2>Conservation du contexte (à la main)</h2>
Pour éviter ces problèmes de maintenance, la solution en vogue est de tout simplement ne jamais utiliser <code>this</code> directement et de préférer utiliser la syntaxe closure et une variable privée pour définir ses classes.
<pre><code>myClass = function() { 
  this.id = 'myClass';
  // capture du contexte
  <strong>var me = this;</strong>
  return {
    action:function() {
      console.log( <strong>me.id</strong> );
    }
  }
};
myObject = new myClass(); 
document.body.onclick = myObject.action; 
// 'myClass'
</code></pre>
<code> </code>
On voit donc ici :
<ul>
  <li>dans le constructeur, on capture le contexte avec une variable privée : <code>var me = this</code></li>
  <li>dans la fonction <code>action</code> on n'utilise plus <code>this</code> mais <code>me</code></li>
</ul>
Peu importe l'endroit d'où est lancé la fonction (ici au clic sur la page), et peu importe la valeur réelle de <code>this</code>, on est maintenant certain que l'on aura toujours un accès à l'objet originel grâce à une variable privée. Bien sur <code>this</code> reste utilisable normalement, notamment si vous avez besoin de retrouver l'objet DOM cliqué par exemple.

Pour encore plus de souplesse avec les fonctions de JavaScript, allez voir <a href="http://braincracking.org/?p=896">la deuxième partie de cet article</a>.
<h1>L'essentiel</h1>
Si vous ne deviez retenir que quelques points, les voici :
<ul>
  <li>JavaScript a des concepts différents des langages majeurs et devient extrêmement important sur votre CV. Prenez le temps de l'apprendre</li>
  <li>Les bibliothèques telles que jQuery ne sont pas faites pour couvrir les cas que nous venons de voir. jQuery permet d'utiliser le DOM sereinement, pas d'organiser votre code proprement.</li>
  <li>Lorsque vous créez une classe, choisissez la syntaxe prototype pour la performance ou closure pour les variables privées et la maîtrise du contexte d'exécution</li>
  <li>N'utilisez plus la syntaxe <code>function maFonction() {}</code></li>
  <li>Utilisez systématiquement la fonction anonyme auto-exécutée ( <code>(function() { var .. }())</code>) en début et fin de fichier ou de <code>&lt;script&gt;</code></li>
  <li>Utilisez systématiquement <code>var</code> pour vos variables, y compris celles qui font référence à des fonctions</li>
</ul>
Dernière chose : j'ai essayé d'être pratique, mais lire un post de blog ne suffit jamais pour comprendre des concepts de programmation : codez !
