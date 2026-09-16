---
title: "Compound interest calculator"
description: "Work out what a starting capital and monthly payments become over time, and how much of the result comes from gains."
layout: "simulateur"
weight: 2
translationKey: "sim-composes"
---


<form class="sim" id="sim-capital" onsubmit="return false">
<div class="sim-grid">
<label>Starting capital (€)<input type="number" id="c-init" value="5000" min="0" step="100"></label>
<label>Monthly payment (€)<input type="number" id="c-mens" value="200" min="0" step="10"></label>
<label>Duration (years)<input type="number" id="c-duree" value="20" min="1" max="60"></label>
<label>Average annual return (%)<input type="number" id="c-taux" value="5" min="0" max="20" step="0.1"></label>
</div>
<div class="sim-result" id="c-out"></div>
</form>

<script>
(function(){
var f=function(n){return Math.round(n).toLocaleString('en-GB')+' €';};
function calc(){
var C=+document.getElementById('c-init').value||0,
    M=+document.getElementById('c-mens').value||0,
    A=+document.getElementById('c-duree').value||0,
    T=(+document.getElementById('c-taux').value||0)/100,
    r=Math.pow(1+T,1/12)-1, n=A*12, v=C*Math.pow(1+r,n);
if(r>0){v+=M*(Math.pow(1+r,n)-1)/r;}else{v+=M*n;}
var verse=C+M*n, gains=v-verse;
document.getElementById('c-out').innerHTML=
 '<div class="sim-main"><span class="sim-main-value">'+f(v)+'</span><span class="sim-main-label">capital after '+A+' years</span></div>'+
 '<div class="sim-tiles"><div class="sim-tile"><strong>'+f(verse)+'</strong><span>total paid in</span></div>'+
 '<div class="sim-tile"><strong>'+f(gains)+'</strong><span>of which gains</span></div>'+
 '<div class="sim-tile"><strong>'+(verse>0?Math.round(gains/verse*100):0)+' %</strong><span>gain on payments</span></div></div>';
}
document.querySelectorAll('#sim-capital input').forEach(function(i){i.addEventListener('input',calc);});
calc();
})();
</script>

## How to read the result

The calculation applies a **constant** return, which exists in no real market: an average of 5% a year is made in practice of strongly positive and negative years. The result gives an order of magnitude, not a forecast.

Three parameters weigh far more than the headline return: **duration**, the **monthly payment** and the **fees** this calculator does not include. To measure those, use the fee impact calculator.

