---
title: "Simulateur d'économie d'impôt PER"
description: "Calculez ce que votre versement sur un plan épargne retraite fait baisser d'impôt selon votre tranche marginale."
layout: "simulateur"
weight: 3
translationKey: "sim-per"
---


<form class="sim" id="sim-per" onsubmit="return false">
<div class="sim-grid">
<label>Versement annuel sur le PER (€)<input type="number" id="p-vers" value="5000" min="0" step="100"></label>
<label>Tranche marginale d'imposition
<select id="p-tmi"><option value="0">0 % (non imposable)</option><option value="11">11 %</option><option value="30" selected>30 %</option><option value="41">41 %</option><option value="45">45 %</option></select></label>
</div>
<div class="sim-result" id="p-out"></div>
</form>

<script>
(function(){
var f=function(n){return Math.round(n).toLocaleString('fr-FR')+' €';};
function calc(){
var V=+document.getElementById('p-vers').value||0, T=+document.getElementById('p-tmi').value||0;
var eco=V*T/100, effort=V-eco;
document.getElementById('p-out').innerHTML=
 '<div class="sim-main"><span class="sim-main-value">'+f(eco)+'</span><span class="sim-main-label">économie d\'impôt la première année</span></div>'+
 '<div class="sim-tiles"><div class="sim-tile"><strong>'+f(effort)+'</strong><span>effort d\'épargne réel</span></div>'+
 '<div class="sim-tile"><strong>'+T+' %</strong><span>tranche appliquée</span></div>'+
 '<div class="sim-tile"><strong>'+f(V)+'</strong><span>capital réellement placé</span></div></div>'+
 (T===0?'<p class="sim-warn">À 0 %, la déduction ne rapporte rien et les sommes seront malgré tout imposées à la sortie. Vous pouvez renoncer à la déduction lors du versement.</p>':'');
}
document.querySelectorAll('#sim-per input, #sim-per select').forEach(function(i){i.addEventListener('input',calc);i.addEventListener('change',calc);});
calc();
})();
</script>

## Ce que le calcul ne dit pas

L'économie affichée est celle de **la première année**. Elle n'est pas un gain définitif : les sommes déduites à l'entrée seront imposées au barème au moment de la sortie du plan. L'opération n'est réellement gagnante que si votre taux d'imposition à la retraite est inférieur à celui d'aujourd'hui.

Le calcul ne vérifie pas non plus votre **plafond de déduction**, qui dépend de vos revenus professionnels et figure sur votre avis d'imposition.

