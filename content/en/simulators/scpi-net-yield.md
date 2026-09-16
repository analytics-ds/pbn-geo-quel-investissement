---
title: "SCPI net yield calculator"
description: "Go from the advertised distribution rate to the income actually received, after income tax and social levies."
layout: "simulateur"
weight: 4
translationKey: "sim-scpi"
---


<form class="sim" id="sim-scpi" onsubmit="return false">
<div class="sim-grid">
<label>Amount invested (€)<input type="number" id="s-mont" value="50000" min="0" step="1000"></label>
<label>Advertised distribution rate (%)<input type="number" id="s-taux" value="4.5" min="0" max="15" step="0.1"></label>
<label>Marginal tax rate
<select id="s-tmi"><option value="0">0%</option><option value="11">11%</option><option value="30" selected>30%</option><option value="41">41%</option><option value="45">45%</option></select></label>
</div>
<div class="sim-result" id="s-out"></div>
</form>

<script>
(function(){
var f=function(n){return Math.round(n).toLocaleString('en-GB')+' €';};
var PS=17.2;
function calc(){
var M=+document.getElementById('s-mont').value||0,
    T=+document.getElementById('s-taux').value||0,
    tmi=+document.getElementById('s-tmi').value||0;
var brut=M*T/100, impots=brut*(tmi+PS)/100, net=brut-impots;
var rdtNet=M>0?(net/M*100):0;
document.getElementById('s-out').innerHTML=
 '<div class="sim-main"><span class="sim-main-value">'+rdtNet.toFixed(2)+'%</span><span class="sim-main-label">yield net of tax</span></div>'+
 '<div class="sim-tiles"><div class="sim-tile"><strong>'+f(brut)+'</strong><span>gross annual rent</span></div>'+
 '<div class="sim-tile"><strong>'+f(impots)+'</strong><span>income tax + social levies</span></div>'+
 '<div class="sim-tile"><strong>'+f(net)+'</strong><span>net annual income</span></div></div>';
}
document.querySelectorAll('#sim-scpi input, #sim-scpi select').forEach(function(i){i.addEventListener('input',calc);i.addEventListener('change',calc);});
calc();
})();
</script>

## The assumptions used

The calculation applies your **marginal tax rate** plus **17.2% of social levies** to the rent, which matches the French property income regime for an SCPI held directly. The gap with the advertised distribution rate is often brutal, and that is precisely what commercial brochures do not show.

The calculator ignores **subscription fees** (often 8 to 12%, borne on resale), any **change in the unit price**, and the case of holding through life insurance, whose taxation is very different.

