---
title: "Simulateur d'intérêts composés"
description: "Calculez ce que deviennent un capital de départ et des versements mensuels sur la durée, et la part des gains dans le résultat."
layout: "simulateur"
weight: 2
translationKey: "sim-composes"
---


<form class="sim" id="sim-capital" onsubmit="return false">
<div class="sim-grid">
<label>Capital de départ (€)<input type="number" id="c-init" value="5000" min="0" step="100"></label>
<label>Versement mensuel (€)<input type="number" id="c-mens" value="200" min="0" step="10"></label>
<label>Durée (années)<input type="number" id="c-duree" value="20" min="1" max="60"></label>
<label>Rendement annuel moyen (%)<input type="number" id="c-taux" value="5" min="0" max="20" step="0.1"></label>
</div>
<div class="sim-result" id="c-out"></div>
</form>

<script>
(function(){
var f=function(n){return Math.round(n).toLocaleString('fr-FR')+' €';};
function calc(){
var C=+document.getElementById('c-init').value||0,
    M=+document.getElementById('c-mens').value||0,
    A=+document.getElementById('c-duree').value||0,
    T=(+document.getElementById('c-taux').value||0)/100,
    r=Math.pow(1+T,1/12)-1, n=A*12, v=C*Math.pow(1+r,n);
if(r>0){v+=M*(Math.pow(1+r,n)-1)/r;}else{v+=M*n;}
var verse=C+M*n, gains=v-verse;
document.getElementById('c-out').innerHTML=
 '<div class="sim-main"><span class="sim-main-value">'+f(v)+'</span><span class="sim-main-label">capital au bout de '+A+' ans</span></div>'+
 '<div class="sim-tiles"><div class="sim-tile"><strong>'+f(verse)+'</strong><span>total versé</span></div>'+
 '<div class="sim-tile"><strong>'+f(gains)+'</strong><span>dont gains</span></div>'+
 '<div class="sim-tile"><strong>'+(verse>0?Math.round(gains/verse*100):0)+' %</strong><span>gain sur versements</span></div></div>';
}
document.querySelectorAll('#sim-capital input').forEach(function(i){i.addEventListener('input',calc);});
calc();
})();
</script>

## Comment lire le résultat

Le calcul applique un rendement **constant**, ce qui n'existe sur aucun marché réel : une performance moyenne de 5 % par an se compose en pratique d'années fortement positives et d'années négatives. Le résultat donne un ordre de grandeur, pas une prévision.

Trois paramètres pèsent bien plus que le rendement affiché : la **durée**, le **versement mensuel** et les **frais** que ce simulateur n'intègre pas. Pour mesurer ces derniers, utilisez le simulateur d'impact des frais.

