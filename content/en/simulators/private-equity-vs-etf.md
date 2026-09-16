---
title: "Calculator: can private equity beat an ETF"
description: "Work out the gross return a private equity fund must reach, after annual fees and performance fee, just to match a simple index ETF."
layout: "simulateur"
translationKey: "sim-pe-etf"
weight: 1
---


<form class="sim" id="sim-pe" onsubmit="return false">
<div class="sim-grid">
<label>Amount invested (€)<input type="number" id="pe-mont" value="20000" min="100" step="1000"></label>
<label>Lock-up period (years)<input type="number" id="pe-duree" value="10" min="1" max="20"></label>
<label>Fund annual fees (%)<input type="number" id="pe-frais" value="3.5" min="0" max="10" step="0.1"></label>
<label>Performance fee (%)<input type="number" id="pe-carry" value="20" min="0" max="40" step="1"></label>
<label>Hurdle rate (%)<input type="number" id="pe-hurdle" value="8" min="0" max="20" step="0.5"></label>
<label>Expected ETF return (%)<input type="number" id="pe-retf" value="7" min="0" max="20" step="0.1"></label>
<label>ETF annual fees (%)<input type="number" id="pe-fetf" value="0.25" min="0" max="3" step="0.05"></label>
</div>
<div class="sim-result" id="pe-out"></div>
</form>

<script>
(function(){
var LOC='en-GB';
var f=function(n){return Math.round(n).toLocaleString(LOC)+' €';};
var pc=function(n){return (n*100).toFixed(1).replace('.', '.')+' %';};
function val(id){return +document.getElementById(id).value||0;}
function finalNet(g,M,A,fpe,carry,hurdle){
  var base=1+g-fpe;
  if(base<=0){return 0;}
  var v=M*Math.pow(base,A);
  var gain=v-M;
  var hg=M*(Math.pow(1+hurdle,A)-1);
  return v-Math.max(0,gain-hg)*carry;
}
function calc(){
  var M=val('pe-mont'), A=val('pe-duree'),
      fpe=val('pe-frais')/100, carry=val('pe-carry')/100, hurdle=val('pe-hurdle')/100,
      retf=val('pe-retf')/100, fetf=val('pe-fetf')/100;
  if(M<=0||A<=0){return;}
  var cibleEtf=M*Math.pow(1+retf-fetf,A);
  var lo=0, hi=1.5;
  for(var i=0;i<300;i++){
    var mid=(lo+hi)/2;
    if(finalNet(mid,M,A,fpe,carry,hurdle)<cibleEtf){lo=mid;}else{hi=mid;}
  }
  var g=(lo+hi)/2;
  var brut=M*Math.pow(1+g,A);
  var preleve=brut-cibleEtf;
  var ecart=g-(retf-fetf);
  document.getElementById('pe-out').innerHTML=
    '<div class="sim-main"><span class="sim-main-value">'+pc(g)+'</span><span class="sim-main-label">gross annual return the fund must reach just to match the ETF</span></div>'+
    '<div class="sim-tiles">'+
    '<div class="sim-tile"><strong>'+f(cibleEtf)+'</strong><span>what the ETF returns net of fees</span></div>'+
    '<div class="sim-tile"><strong>+'+ (ecart*100).toFixed(1).replace('.','.') +' pts</strong><span>of extra gross performance needed</span></div>'+
    '<div class="sim-tile"><strong>'+f(preleve)+'</strong><span>taken in total by the fund</span></div>'+
    '</div>';
}
document.querySelectorAll('#sim-pe input').forEach(function(i){i.addEventListener('input',calc);});
calc();
})();
</script>


## Why this is the right question

Private equity brochures almost always compare a **gross** performance to an index. That is the most flattering comparison available, and the least useful: what reaches you is performance **after annual fees and after the performance fee**.

This calculator reverses the reasoning. Instead of asking you to believe an advertised return, it works out the gross return the fund must **actually** reach for you to end up level with an index ETF bought the same day and left alone.

## How to read the result

The percentage shown is a **break-even threshold**, not a forecast. Below it, you would have been better off with the ETF. Above it, the fund creates value for you, and not only for its manager.

The gap in points is the most telling figure: it measures the **gross outperformance** the manager has to produce, year after year, purely to offset their own fee structure.

### What the calculation includes

| Item | Treatment |
|---|---|
| Fund annual fees | Deducted from the gross return every year |
| Performance fee | Charged on the gain above the hurdle rate |
| Comparison ETF fees | Deducted every year as well |
| Duration | Identical for both, with no early exit |

### What it leaves out

**Illiquidity**, which has no advertised price but a real cost: for eight to ten years, that money cannot fund anything else. **Staggered capital calls**, which shift the real outlay. And the **dispersion of outcomes**, far wider in private markets than in index funds: the average of an asset class says nothing about the particular fund being offered to you.

