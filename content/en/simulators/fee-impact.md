---
title: "Fee impact calculator"
description: "Measure what annual fees really take out of your capital over time, compounding included."
layout: "simulateur"
weight: 5
translationKey: "sim-frais"
---


<form class="sim" id="sim-frais" onsubmit="return false">
<div class="sim-grid">
<label>Capital invested (€)<input type="number" id="fr-cap" value="50000" min="0" step="1000"></label>
<label>Duration (years)<input type="number" id="fr-duree" value="20" min="1" max="60"></label>
<label>Gross annual return (%)<input type="number" id="fr-rdt" value="6" min="0" max="20" step="0.1"></label>
<label>Total annual fees (%)<input type="number" id="fr-frais" value="1.8" min="0" max="5" step="0.05"></label>
</div>
<div class="sim-result" id="fr-out"></div>
</form>

<script>
(function(){
var f=function(n){return Math.round(n).toLocaleString('en-GB')+' €';};
function calc(){
var C=+document.getElementById('fr-cap').value||0,
    A=+document.getElementById('fr-duree').value||0,
    R=(+document.getElementById('fr-rdt').value||0)/100,
    F=(+document.getElementById('fr-frais').value||0)/100;
var sans=C*Math.pow(1+R,A), avec=C*Math.pow(1+R-F,A), cout=sans-avec;
document.getElementById('fr-out').innerHTML=
 '<div class="sim-main"><span class="sim-main-value">'+f(cout)+'</span><span class="sim-main-label">total cost of fees over '+A+' years</span></div>'+
 '<div class="sim-tiles"><div class="sim-tile"><strong>'+f(sans)+'</strong><span>capital without fees</span></div>'+
 '<div class="sim-tile"><strong>'+f(avec)+'</strong><span>capital after fees</span></div>'+
 '<div class="sim-tile"><strong>'+(sans>0?Math.round(cout/sans*100):0)+'%</strong><span>share of capital absorbed</span></div></div>';
}
document.querySelectorAll('#sim-frais input').forEach(function(i){i.addEventListener('input',calc);});
calc();
})();
</script>

## Why the gap looks disproportionate

Fees do not simply remove a percentage each year: they also remove **everything that amount would have earned afterwards**. That is why a fee gap that looks trivial over one year becomes considerable over twenty.

To compare two solutions, add up every layer of fees: the fund's own charges, the wrapper's fees, and any switching costs. That total is what you should enter here.

