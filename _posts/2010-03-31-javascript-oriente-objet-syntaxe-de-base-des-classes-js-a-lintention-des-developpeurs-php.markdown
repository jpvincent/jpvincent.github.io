---
layout: post
title:  "Javascript Orienté Objet : syntaxe de base des classes JS à l’intention des développeurs PHP"
date:   2010-03-31 16:04:21 +0100
---
<h1>Pourquoi, pour qui ?</h1>
Ce tuto a pour cible les développeurs qui ont une expérience du PHP (5) et qui veulent se lancer dans un projet Javascript qui dépasse le simple scripting. Cela va donc commencer par <strong>savoir écrire des classes en JS</strong>. Pour avoir galéré en tant que développeur puis en tant que lead technique avoir formé de bons développeurs PHP à faire des applications Web où le JS représente plus d'un tiers du code et la moitié du temps de dev, j'ai pu constater les énervements classiques lorsqu'on commence à vouloir faire des choses sérieuses en JS.

Le but ici n'est pas de rentrer dans la théorie du langage JS ou même d'être exhaustif (voir à la fin de cet article des ressources qui le font très bien) mais de vous fournir un template pour commencer à écrire vos classes.
<h1>JS c'est compliqué</h1>
Quand on arrive du PHP, du C ou même de Java, JavaScript peut être franchement surprenant. Certains <a href="http://wtfjs.com/">s'en amusent</a>, d'autres prennent sa défense en rappelant son <a href="http://www.yuiblog.com/blog/2010/02/03/video-crockonjs-1">histoire mouvementée</a> (la fusion de 3 langages, une implémentation en quelques semaines, pris dans la Browser War depuis 15 ans) et surtout une chose qui est bien particulière aux développeurs web : <strong>personne ne prend la peine de l'apprendre</strong> !

Ajouté à cela, il y a le <strong>DOM</strong> dont l'implémentation dans chaque browser varie, la <strong>programmation événementielle</strong> que les développeurs PHP n'ont en général jamais expérimenté, le manque de <strong>documentation centralisée</strong> (pas d'équivalent à PHP.net) et enfin la version implémentée d'EcmaScript qui varie selon le browser (pour info, il faut en rester à la <strong>version 1.5</strong> qui est celle de IE6-8)

Concrètement il y a 2 choses à comprendre pour éviter les erreurs classiques et partir sur une bonne base de code pour programmer avec des objets :
<ul>
  <li><strong>le scope :</strong> repérer et utiliser <code>var</code> et les <em>closures ou <code>{}</code></em> qui l'entourent, vos variables ne sont visibles qu'à l'intérieur</li>
  <li><strong>tout est </strong><code><strong>function()</strong></code> : fonctions, méthodes, classes, constructeur sont créées de cette seule manière</li>
</ul>
<h1>Architecture comparée JS/PHP</h1>
<h2>Eviter les globales, utiliser <em>var</em> et les namespaces</h2>
Valable dans les deux langages, vous allez essayer de minimiser le nombre de variables disponibles dans <code>$_GLOBALS</code> et <code>window.*</code> et donc modifiables par les librairies que vous incluez, l'arrivée d'un nouveau code ou toute modification de l'existant.

Exemple, ceci génère une boucle infinie :
<pre><code>function genericFunctionName() {
for(i = 0; i &lt; myArray.length; i++) {
  ....
}

for(i = 0; i &lt; 10; i++) {
  genericFunctionName();
}</code></pre>
on a créé ici une <strong>boucle infinie</strong> car le i à l'intérieur et à l'extérieur de la fonction font référence à <code>window.i</code>: c'est la même variable globale. Ici la première librairie venue (ou publicité) risque en outre <strong>d'écraser le nom de votre fonction</strong> si il est trop générique : j'ai déjà vu par exemple jQuery et l'API mappy se géner l'un l'autre sur la même page! Et, même si JS est monothread, il y a des cas où la valeur de <code>i</code> peut être modifiée par une autre boucle qui utilise ce nom si répandu.

la solution ici est de rajouter <code>var</code> pour que le <code>i</code> à l'intérieur de la fonction ne soit pas visible de l'extérieur. <strong>Son scope est la closure la plus proche</strong>, c'est à dire la déclaration de fonction la plus proche.
<pre><code>for(var i= 0; .... </code></pre>
Pour partir sur de bonnes bases, on va <strong>créer un namespace</strong> pour tout le code de notre application web. Pendant tous les développements, il faudra prendre l'habitude de <strong>déclarer ses variables avec <code>var</code></strong>.
<pre><code>var MY_APP_NAMESPACE = {}; // l'équivalent namespace de PHP5.3 n'existe pas, on déclare un objet JS

MY_APP_NAMESPACE.genericFunctionName = function() {
var aMyArray = [ .... ], // multiples déclaration de variables locales
iTotal = aMyArray.length;
  for(var i = 0; i &lt; iTotal; i++) ....
}; // notez le ; final, on a tendance à l'oublier

for(i = 0; i &lt; 10; i++) {
  MY_APP_NAMESPACE.genericFunctionName();
}</code></pre>
La boucle dans la fonction est protégée, ni <code>i</code> ni le tableau ne sont accessibles de l'extérieur. Il est peu probable que des codes extérieurs utilisent votre namespace, et si ils le font, vous le sentirez de toute manière passer, ce qui (sans ironie) est plus facile à détecter qu'un petit bug !
<h2>Classes statiques</h2>
Toujours dans l'idée de <strong>libérer notre espace global</strong>, c'est une bonne pratique en PHP comme en JS d'arrêter d'accumuler des listes sans fin de fonctions : il vaut mieux les <strong>regrouper par thème</strong> dans des "<strong>classes statiques" en PHP5</strong>, et dans des <strong>sous-namespaces en PHP5.3-JS</strong>.

Exemple d'une classe <strong>PHP</strong> servant à valider le format d'input utilisateur :
<pre><code>namespace MY_APP_NAMESPACE;
class validation {
  public static $regMail='^[w-]+(?:.[w-]+)*@(?:[w-]+.)+[a-zA-Z]{2,7}$';
  public static $regPassword='^[a-zA-Z0-9./\+=%ù£*^¨_&amp;!@#-]{3,50}$';

  static function isMailValid($sMail) {
    return (preg_match("/".self::$regMail."/", $sMail) === 1) ;
  }
  static function isValidPassword($sPassword) {
    return (preg_match("/".self::$regPassword."/", $sPassword ) === 1);
  }
}</code></pre>
De n'importe où en PHP, on peut donc appeller ces fonctions de cette manière :
<code> </code>
<pre><code>print MY_APP_NAMESPACEvalidation::isValidMail( 'mon@mail.com' );</code></pre>
Voici l'équivalent JS (<a href="http://jsfiddle.net/braincracking/P7a5g/" target="_blank">Exécuter dans votre browser</a>) :
<pre><code>
MY_APP_NAMESPACE = {};

(function(){ // début de scope local
MY_APP_NAMESPACE.utils = MY_APP_NAMESPACE.utils || {};
// déclaration de la classe de validation proprement dite
MY_APP_NAMESPACE.utils.validation = {
    // déclaration de nos variables statiques
    regMail: /^[w-]+(?:.[w-]+)*@(?:[w-]+.)+[a-zA-Z]{2,7}/,
    regPassword: /^[a-zA-Z0-9./\+=%ù£*^¨_&amp;!@#-]{3,50}/,
    // déclaration de nos méthodes
    isMailValid:function( sMail ) {
        return (self.regMail.exec( sMail ) != null);
    },
    isValidPassword:function( sPassword ) {
        return (self.regPassword.exec( sPassword ) != null);
    }
}; // fin de classe

// trick JS pour émuler le self:: en PHP : on utilise une variable locale
var self = MY_APP_NAMESPACE.utils.validation;
})(); // fin de scope local
</code></pre>
De n'importe où en JS, on peut donc appeller ces fonctions de cette manière :
<pre><code>alert(MY_APP_NAMESPACE.utils.validation.isMailValid( 'monm@il.com' )); //true
alert(MY_APP_NAMESPACE.utils.validation.isMailValid( 'moççna@il.com' ));​ // false​​​​
</code></pre>
La syntaxe est radicalement différente de PHP car <strong>il n'y a rien d'explicite</strong> et on exploite plusieurs spécifités JS, mais on est arrivé au même résultat. Je vous conseille de <strong>prendre ce bout de code comme un template pour créer des classes statiques</strong> JS. Si vous aimez comprendre :
<ul>
  <li>La première et la dernière ligne est une fonction anonyme autoexécutée que l'on met autour de chaque classe. Elle sert à avoir un scope local propre à la classe et donc à <strong>émuler des variables privées pour cette classe</strong>.</li>
  <li>la 2nde ligne suppose que <code>MY_APP_NAMESPACE</code> a déjà été déclarée mais n'est pas certaine du sous namespace <code>utils</code>. La notation spéciale <strong><code>||</code></strong> (double pipe) permet donc de créer ce sous-namespace uniquement si il n'est pas déjà déclaré (et donc de ne pas l'écraser)</li>
  <li>la classe statique <code>validation</code> est <strong>concrètement un objet JS</strong> composé de 2 expressions régulières (directement compilées avec / /) et de 2 fonctions. Le tout est déclaré avec la notation JSON : { clé : valeur , … }. Attention à ne jamais laisser trainer une virgule derrière la dernière clé, car cela fait planter IE, et il ne le dit pas clairement</li>
  <li>dans l'avant dernière ligne on déclare une variable privée qu'on appelle <code>self</code> et qui remplit la même fonction qu'en PHP : faire référence à notre classe statique. Comme en PHP elle est là pour des raisons de confort (éviter de retaper entièrement et en dur le nom de la classe)</li>
</ul>
<h2>Objets instanciables</h2>
Créons une classe PHP classique avec constructeur, méthode publique, variable privée et variable statique publique. Prenons par exemple un objet permettant de manipuler des dates :
<pre><code>namespace MY_APP_NAMESPACE;
class customDate {
  private $iTimestamp = 0; // variable privée propre à chaque instance
  static $aMonthNames = array('January', ....); // variable statique partagée pour tout le code
  // constructeur
  public function __construct($iTimestamp) {
    $this-&gt;iTimestamp = $iTimestamp;
  }
  // méthode publique propre à chaque instance
  public function getMonthName() {
    $month_number = date('n', $this-&gt;iTimestamp) -1;
    return self::$aMonthNames[$month_number];
  }
}</code></pre>
Utilisation :
<pre><code>$date = new MY_APP_NAMESPACEcustomDate( 1268733478547 );
print $date-&gt;getMonthName(); // 'March'
</code></pre>
Maintenant en JS (<a href="http://jsfiddle.net/braincracking/q6gwx/" target="_blank">exécuter le code</a>) :
<pre><code>(function(){ // début de scope local
MY_APP_NAMESPACE.utils = MY_APP_NAMESPACE.utils || {}; // création d'un sous namespace pour y stocker nos classes utilitaires si celui-ci n'est pas déjà créé

// constructeur
MY_APP_NAMESPACE.utils.customDate = function( iTimeStamp ) {
  this.iTimeStamp = iTimeStamp;
  this.date = new Date();
  this.date.setTime(this.iTimeStamp);
};

// variables et méthodes publiques propres à chaque instance
MY_APP_NAMESPACE.utils.customDate.prototype = {
  date:null,
  iTimeStamp:0,
  getMonthName:function() {
    var iMonthNumber = this.date.getMonth();
  return self.aMonthNames[iMonthNumber];
  }
};

// variable statique partagée pour tout le code
MY_APP_NAMESPACE.utils.customDate.aMonthNames = ['January', 'February','March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];

// trick JS pour émuler le self:: en PHP : on utilise une variable locale
var self = MY_APP_NAMESPACE.utils.customDate,
privateVariable = 0; // variable privée visible par toutes les instances

})(); // fin de scope local
</code></pre>
Remarquez que cette syntaxe (dite <em><strong>prototype</strong></em>) <strong>n'autorise pas à avoir des variables privées pour chaque instance</strong> comme en PHP: ici <code>this.iTimeStamp</code> se réfère bien à l'instance de <code>customDate</code>, mais est accessible depuis l'extérieur. La variable vraiment privée est <code>privateVariable</code> mais elle est visible et modifiable par toutes les instances, concept qui serait équivalent en PHP à une <strong>variable statique privée</strong>, qui peut par exemple servir à un manager d'instance (pattern factory + accessor) pour stocker une liste des instances en cours.
<h3>Syntaxe alternative</h3>
Il existe une autre syntaxe, dite <em><strong>closure</strong></em>, qui permet d'avoir des variables et méthodes privées pour chaque instance mais <strong>elle est moins performante si vous instanciez des centaines d'objets, ou que vos objets commencent à contenir plusieurs méthodes</strong> (voir<a href="http://blogs.msdn.com/kristoffer/archive/2007/02/13/javascript-prototype-versus-closure-execution-speed.aspx">ce petit bench</a>, il en existe des dizaines d'autres qui confirment la même chose). A vous de faire la balance entre avoir des variables privées et de meilleures performances (<a href="http://jsfiddle.net/braincracking/6MyJV/" target="_blank">exécuter le code</a>) :
<pre><code>
MY_APP_NAMESPACE = {};

(function(){ // début de scope local
MY_APP_NAMESPACE.utils = MY_APP_NAMESPACE.utils || {}; // création d'un sous namespace pour y stocker nos classes utilitaires si celui-ci n'est pas déjà créé

// constructeur
MY_APP_NAMESPACE.utils.customDate = function( iTimeStamp ) {
    // ces variables resteront privées, spécifiques à l'instance
    var iTimeStamp = iTimeStamp,
        date = new Date();
    date.setTime(iTimeStamp);

    // on renvoie ce qui est public sous la forme d'un objet
    return {
        getMonthName:function() {
            var iMonthNumber = date.getMonth();

            return self.aMonthNames[iMonthNumber];
        }
    };
};

// variable statique partagée pour tout le code
MY_APP_NAMESPACE.utils.customDate.aMonthNames = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];

// trick JS pour émuler le self:: en PHP : on utilise une variable locale
var     self = MY_APP_NAMESPACE.utils.customDate,
    privateVariable = 0; // variable privée visible par toutes les instances
})(); // fin de scope local

// privateVariable est privé
console.log('variable privée :'+ typeof privateVariable ); // undefined
// création d'un objet date
var date1 = new MY_APP_NAMESPACE.utils.customDate(1268733478547); // 16 mars 2010, 10:58
console.log('méthode publique d instance :'+date1.getMonthName()); // March
console.log('variable privée d instance :'+ typeof date1.date ); // undefined

// la variable statique est disponible en lecture
console.log('variable statique : '+MY_APP_NAMESPACE.utils.customDate.aMonthNames[0]); // January

// et aussi en écriture, ici on change de langue à la volée
MY_APP_NAMESPACE.utils.customDate.aMonthNames = ['Janvier', 'Février', 'Mars', 'Avril', 'Mai', 'Juin', 'Juillet', 'Août', 'Septembre', 'Octobre', 'Novembre', 'Decembre'];
console.log( date1.getMonthName() ); // Mars​
</code></pre>
Ici on a <strong>remplacé l'utilisation de </strong><code><strong>prototype</strong></code> pour ajouter des propriétés par un <code>return</code> dans le constructeur d'un <strong>objet contenant des propriétés publiques</strong>. Comme on se trouve dans le scope du constructeur, les méthodes ont accès à <code>iTimestamp</code> et <code>date</code> qui sont <strong>privées et propres à chaque instance</strong>. Le <strong>coût en performances</strong> vient du fait que les fonctions sont redéfinies à chaque fois qu'on crée un objet, alors qu'avec prototype la définition n'était faite qu'une seule fois.
<h2>Conclusion</h2>
Vous voici donc avec la notion salutaire de <strong><code>namespace</code></strong>, une implémentation de <code><strong>self::</strong></code>, une idée sur le fonctionnement du scope et <strong>2 templates de classes de base</strong> (en mode <code>prototype</code> et en mode <code>closure</code>, le premier étant à préférer). Ces templates m'ont servi ces 4 dernières années sur des projets JS d'envergure moyenne (+ de 100 classes, des milliers de lignes de code), ce modèle est donc éprouvé.

Notez que je n'ai <strong>pas traité l'héritage des classes</strong>, c'est parce que je suppose que si vous en êtes à vouloir développer en JS Orienté Objet, vous êtes probablement sur un projet suffisamment large pour utiliser des librairies JS comme <a href="http://developer.yahoo.com/yui/examples/yahoo/yahoo_extend.html">YUI</a> ou <a href="http://jquery.developpeur-web2.com/documentation/fonctions-diverses/$.extend.php">jQuery</a> qui ont chacun des méthodes pour simuler l'héritage (qui n'existe pas formellement en JS). Maintenant si ça vous intéresse, je peux faire un petit post là dessus, <strong>dites moi ça dans les commentaires</strong>.

Javascript étant extrêmement versatile, sachez cependant qu'il existe une bonne dizaine de variations autour des closures et de prototype. Le tout étant d'en choisir une qui marche dans tous les cas (et de savoir pourquoi), voici une short-list de références :
<ul>
  <li><a href="http://valums.com/b/">contre performance des variables privées</a>, ou pourquoi préférer <em>prototype</em></li>
  <li>A propos de la versatilité des function en JS (<a href="http://developer.yahoo.com/yui/theater/video.php?v=crockonjs-3">vidéo + slides + transcription</a> 1h en anglais, par Douglas Crockford)</li>
  <li>une des premières <a href="http://yuiblog.com/blog/2007/06/12/module-pattern/">explications concrètes du module pattern</a></li>
  <li><a href="http://javascriptweblog.wordpress.com/2010/03/16/five-ways-to-create-objects/">5 manières de créer des objets</a>, court et concis</li>
</ul>
