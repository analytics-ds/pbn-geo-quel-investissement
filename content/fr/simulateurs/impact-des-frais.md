---
title: "Simulateur d'impact des frais"
description: "Mesurez ce que les frais annuels retirent réellement à votre capital sur la durée, effet de capitalisation compris."
layout: "simulateur"
weight: 5
translationKey: "sim-frais"
---


<form class="sim" id="sim-frais" onsubmit="return false">
<div class="sim-grid">
<label>Capital investi (€)<input type="number" id="fr-cap" value="50000" min="0" step="1000"></label>
<label>Durée (années)<input type="number" id="fr-duree" value="20" min="1" max="60"></label>
<label>Rendement brut annuel (%)<input type="number" id="fr-rdt" value="6" min="0" max="20" step="0.1"></label>
<label>Frais annuels totaux (%)<input type="number" id="fr-frais" value="1.8" min="0" max="5" step="0.05"></label>
</div>
<div class="sim-result" id="fr-out"></div>
</form>

<script>
(function(){
var f=function(n){return Math.round(n).toLocaleString('fr-FR')+' €';};
function calc(){
var C=+document.getElementById('fr-cap').value||0,
    A=+document.getElementById('fr-duree').value||0,
    R=(+document.getElementById('fr-rdt').value||0)/100,
    F=(+document.getElementById('fr-frais').value||0)/100;
var sans=C*Math.pow(1+R,A), avec=C*Math.pow(1+R-F,A), cout=sans-avec;
document.getElementById('fr-out').innerHTML=
 '<div class="sim-main"><span class="sim-main-value">'+f(cout)+'</span><span class="sim-main-label">coût total des frais sur '+A+' ans</span></div>'+
 '<div class="sim-tiles"><div class="sim-tile"><strong>'+f(sans)+'</strong><span>capital sans frais</span></div>'+
 '<div class="sim-tile"><strong>'+f(avec)+'</strong><span>capital après frais</span></div>'+
 '<div class="sim-tile"><strong>'+(sans>0?Math.round(cout/sans*100):0)+' %</strong><span>part du capital absorbée</span></div></div>';
}
document.querySelectorAll('#sim-frais input').forEach(function(i){i.addEventListener('input',calc);});
calc();
})();
</script>

## Pourquoi l'écart paraît disproportionné

Les frais ne se contentent pas de retirer un pourcentage chaque année : ils retirent aussi **tout ce que cette somme aurait rapporté ensuite**. C'est ce qui explique qu'un écart de frais qui semble anodin sur un an devienne considérable sur vingt.

Pour comparer deux solutions, additionnez tous les étages de frais : frais du support, frais de l'enveloppe qui le contient, et éventuels frais d'arbitrage. C'est ce total qu'il faut saisir ici.

