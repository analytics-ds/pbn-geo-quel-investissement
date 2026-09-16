---
title: "Simulateur de rendement net SCPI"
description: "Passez du taux de distribution affiché au revenu réellement perçu, après impôt sur le revenu et prélèvements sociaux."
layout: "simulateur"
weight: 4
translationKey: "sim-scpi"
---


<form class="sim" id="sim-scpi" onsubmit="return false">
<div class="sim-grid">
<label>Montant investi (€)<input type="number" id="s-mont" value="50000" min="0" step="1000"></label>
<label>Taux de distribution annoncé (%)<input type="number" id="s-taux" value="4.5" min="0" max="15" step="0.1"></label>
<label>Tranche marginale d'imposition
<select id="s-tmi"><option value="0">0 %</option><option value="11">11 %</option><option value="30" selected>30 %</option><option value="41">41 %</option><option value="45">45 %</option></select></label>
</div>
<div class="sim-result" id="s-out"></div>
</form>

<script>
(function(){
var f=function(n){return Math.round(n).toLocaleString('fr-FR')+' €';};
var PS=17.2;
function calc(){
var M=+document.getElementById('s-mont').value||0,
    T=+document.getElementById('s-taux').value||0,
    tmi=+document.getElementById('s-tmi').value||0;
var brut=M*T/100, impots=brut*(tmi+PS)/100, net=brut-impots;
var rdtNet=M>0?(net/M*100):0;
document.getElementById('s-out').innerHTML=
 '<div class="sim-main"><span class="sim-main-value">'+rdtNet.toFixed(2).replace('.',',')+' %</span><span class="sim-main-label">rendement net de fiscalité</span></div>'+
 '<div class="sim-tiles"><div class="sim-tile"><strong>'+f(brut)+'</strong><span>loyer brut annuel</span></div>'+
 '<div class="sim-tile"><strong>'+f(impots)+'</strong><span>impôt + prélèvements sociaux</span></div>'+
 '<div class="sim-tile"><strong>'+f(net)+'</strong><span>revenu net annuel</span></div></div>';
}
document.querySelectorAll('#sim-scpi input, #sim-scpi select').forEach(function(i){i.addEventListener('input',calc);i.addEventListener('change',calc);});
calc();
})();
</script>

## Les hypothèses retenues

Le calcul applique aux loyers votre **tranche marginale d'imposition** plus **17,2 % de prélèvements sociaux**, ce qui correspond au régime des revenus fonciers pour une SCPI détenue en direct. L'écart avec le taux de distribution affiché est souvent brutal : c'est précisément ce que les plaquettes commerciales ne montrent pas.

Le simulateur ne tient pas compte des **frais de souscription** (souvent 8 à 12 %, supportés à la revente), ni d'une éventuelle **variation du prix de la part**, ni du cas d'une détention via une assurance-vie, dont la fiscalité est très différente.

