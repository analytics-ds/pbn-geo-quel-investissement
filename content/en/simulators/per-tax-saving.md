---
title: "PER tax saving calculator"
description: "Work out how much a payment into a French retirement savings plan cuts from your tax bill, according to your marginal rate."
layout: "simulateur"
weight: 3
translationKey: "sim-per"
---


<form class="sim" id="sim-per" onsubmit="return false">
<div class="sim-grid">
<label>Annual payment into the PER (€)<input type="number" id="p-vers" value="5000" min="0" step="100"></label>
<label>Marginal tax rate
<select id="p-tmi"><option value="0">0% (not taxable)</option><option value="11">11%</option><option value="30" selected>30%</option><option value="41">41%</option><option value="45">45%</option></select></label>
</div>
<div class="sim-result" id="p-out"></div>
</form>

<script>
(function(){
var f=function(n){return Math.round(n).toLocaleString('en-GB')+' €';};
function calc(){
var V=+document.getElementById('p-vers').value||0, T=+document.getElementById('p-tmi').value||0;
var eco=V*T/100, effort=V-eco;
document.getElementById('p-out').innerHTML=
 '<div class="sim-main"><span class="sim-main-value">'+f(eco)+'</span><span class="sim-main-label">tax saved in the first year</span></div>'+
 '<div class="sim-tiles"><div class="sim-tile"><strong>'+f(effort)+'</strong><span>real saving effort</span></div>'+
 '<div class="sim-tile"><strong>'+T+'%</strong><span>rate applied</span></div>'+
 '<div class="sim-tile"><strong>'+f(V)+'</strong><span>actually invested</span></div></div>'+
 (T===0?'<p class="sim-warn">At 0%, the deduction gains nothing and the money will still be taxed on exit. You can waive the deduction when paying in.</p>':'');
}
document.querySelectorAll('#sim-per input, #sim-per select').forEach(function(i){i.addEventListener('input',calc);i.addEventListener('change',calc);});
calc();
})();
</script>

## What the calculation does not say

The saving shown is that of **the first year**. It is not a permanent gain: amounts deducted going in will be taxed at the income scale when the plan is unwound. The operation only wins if your tax rate in retirement is lower than today's.

The calculation also does not check your **deduction ceiling**, which depends on your professional income and appears on your tax assessment.

