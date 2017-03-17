---
layout: post
title:  "Mashup : YQL + google trends"
date:   2009-11-23 16:04:21 +0100
---
<p>Mettons que votre site ait besoin de générer du contenu au kilomètre. Et que tant qu&#39;à faire vous ayez envie de coller aux mots clés du moment. Et qu&#39;en plus il faut que ça soit international.</p>
<p>Pour faire ça il suffit d&#39;accèder à <a href="http://www.google.com/insights/search/">Google Trends</a>, et de faire une recherche sur un pays et une période de recherche (7 derniers jours dans mon cas). Très bien lorsqu&#39;on veut le faire à la main, mais voici comment faire pour le mettre de manière automatique sur un site.</p>
<p>En ouvrant Firebug, on se rend compte qu&#39;une requête XHR est faite pour récupérer le résultat. Exemple pour la France :</p>
<code>http://www.google.com/insights/search/overviewReport?geo=FR&amp;date=today%207-d&amp;gprop=news&amp;cmpt=q&amp;content=1</code>
<p>On récupère du HTML. Plutôt que de jouer avec les regexps pour récupérer les données qui nous intéressent, on va plutôt utiliser YQL qui permet de récupérer le contenu d&#39;une URL et de lancer <strong>XPath</strong> dessus !. En inspectant le HTML, le Xpath le plus spécifique et le plus court semble être &quot;<strong>//tr[@class=&quot;trends-table-row&quot;]//a</strong>&quot;</p>
<p>En utilisant la <a href="http://developer.yahoo.com/yql/console/?q=select%20*%20from%20search.siteexplorer.pages%20where%20query%3D%22http%3A%2F%2Fsearch.yahoo.com%22&amp;env=store://datatables.org/alltableswithkeys#h=select%20content%20from%20html%20where%20url%3D%22http%3A//www.google.com/insights/search/overviewReport%3Fgeo%3DFR%26date%3Dtoday%25207-d%26gprop%3Dnews%26cmpt%3Dq%26content%3D1%22%20and%20xpath%20%3D%20%27//tr%5B@class%3D%22trends-table-row%22%5D//a%27">console YQL</a>, on peut tester et valider la requete suivante :</p>
<code>
select content // le contenu de la balise A visée
<br /><span>&#0160;</span>from html // la table spéciale YQL
<br /><span>&#0160;</span>where url=&quot;http://www.google.com/insights/search/overviewReport?geo=FR&amp;date=today%207-d&amp;gprop=news&amp;cmpt=q&amp;content=1&quot; // notre source
<br /><span>&#0160;</span>and xpath = &#39;//tr[@class=&quot;trends-table-row&quot;]//a&#39; // le chemin Xpath vers la liste des keywords
</code>
<p>On a coché les options JSON (le XML killer), vidé le champs de callback (cbfunc par défaut) et décoché les options de diagnostic. Ensuite on copie/colle l&#39;URL donnée dans le bloc de droite (REST query) pour l&#39;utiliser en PHP</p>
<code>
// j&#39;ai mis en évidence l&#39;insertion du pays du user
<br /><span>&#0160;</span>$url = &#39;http://query.yahooapis.com/v1/public/yql?q=select%20content%20from%20html%20where%20url%3D%22http%3A%2F%2Fwww.google.com%2Finsights%2Fsearch%2FoverviewReport%3Fgeo%3D&#39;;<br /><span>&#0160;</span>// insert the country of the user
$url .= $Intl-&gt;getCountry(); // GB, FR, US, DE ...
<br /><span>&#0160;</span>$url .= &quot;%26date%3Dtoday%25207-d%26gprop%3Dnews%26cmpt%3Dq%26content%3D1%22%20and%20xpath%20%3D%20&#39;%2F%2Ftr%5B%40class%3D%22trends-table-row%22%5D%2F%2Fa&#39;&amp;format=json&amp;diagnostics=false&amp;env=store%3A%2F%2Fdatatables.org%2Falltableswithkeys&quot;;
<br /><span>&#0160;</span>// récupération des résultats YQL
<br /><span>&#0160;</span>$json = file_get_contents($url, &#39;r&#39;);
<br /><span>&#0160;</span>// interprétation
<br /><span>&#0160;</span>$data = json_decode($json);
<br /><span>&#0160;</span>// les résultats qui nous intéressent sont dans un sous tableau :
<br /><span>&#0160;</span>$search-terms = $data-&gt;query-&gt;results-&gt;a;
<br /><span>&#0160;</span>
// un petit nettoyage avant utilisation ultérieure pour affichage
<br /><span>&#0160;</span>foreach($this-&gt;data[&#39;search-terms&#39;] as $i =&gt; $term) {
<br />&#0160;&#0160;&#0160;&#0160;$term = preg_replace(&#39;/ss+/&#39;, &#39; &#39;, $term);
<br /><span>&#0160;</span>}
</code>
<p>Et voila, YQL et Xpath permettent de récupérer du contenu sur n&#39;importe quel site sans forcément passer par une API ou avoir faire des expressions régulières tordues (ce qu&#39;on a tous fait au moins une fois :)-</p>
<p>Bien sur ça ne change rien quand à la fragilité de ce code le jour où google change le format de son HTML, mais ce n&#39;est en général pas le genre de code qu&#39;on pousse en production sans précautions :&#0160;</p>
<ul>
<li><span>&#0160;</span>un système système de cache est obligatoire dans ce cas, ne serait ce que pour ne pas abuser de YQL et google (voire se faire blacklister),&#0160;</li>
<li>un système d&#39;alertes (mail au développeur) lorsqu&#39;on rencontre des erreurs sera utile pour être alerté d&#39;un changement de format</li>
</ul>
<p>Le même système est jouable en JS (ici avec la librairie YUI), en rajoutant cette vois ci un callback :</p>
<code>
var sUrl = &#39;http://query.yahooapis.com/v1/public/yql?q=select%20content%20from%20html%20where%20url%3D%22http%3A%2F%2Fwww.google.com%2Finsights%2Fsearch%2FoverviewReport%3Fgeo%3D&#39;;
<br /><span>&#0160;</span>// insert the country of the user&#0160;<br /><span>&#0160;</span>sUrl += Intl.getCountry(); // GB, FR, US, DE ...
<br /><span>&#0160;</span>sUrl += &quot;%26date%3Dtoday%25207-d%26gprop%3Dnews%26cmpt%3Dq%26content%3D1%22%20and%20xpath%20%3D%20&#39;%2F%2Ftr%5B%40class%3D%22trends-table-row%22%5D%2F%2Fa&#39;&amp;format=json&amp;diagnostics=false&amp;env=store%3A%2F%2Fdatatables.org%2Falltableswithkeys&quot;;
<br /><span>&#0160;</span>sUrl += &#39;&amp;callback=myCallback&#39;;
<br /><span>&#0160;</span>YAHOO.util.Get.script(sUrl);
<br /><span>&#0160;</span>var myCallback = function(oResponse) {
<br /><span>&#0160;</span>
aResults = oResponse.query.results.a;
<br /><span>&#0160;</span>
alert(aResults);
<br />}</code><p><p>Bien sur on perd l&#39;intérêt du référencement dans ce cas là :)</p></p>
