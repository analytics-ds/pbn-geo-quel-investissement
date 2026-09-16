---
title: "Simulateur : le private equity peut-il battre un ETF"
description: "Calculez le rendement brut qu'un fonds de private equity doit atteindre, frais et commission de surperformance déduits, pour seulement faire jeu égal avec un ETF indiciel."
layout: "simulateur"
translationKey: "sim-pe-etf"
weight: 1
---


<form class="sim" id="sim-pe" onsubmit="return false">
<div class="sim-grid">
<label>Montant investi (€)<input type="number" id="pe-mont" value="20000" min="100" step="1000"></label>
<label>Durée de blocage (années)<input type="number" id="pe-duree" value="10" min="1" max="20"></label>
<label>Frais annuels du fonds (%)<input type="number" id="pe-frais" value="3.5" min="0" max="10" step="0.1"></label>
<label>Commission de surperformance (%)<input type="number" id="pe-carry" value="20" min="0" max="40" step="1"></label>
<label>Seuil de déclenchement (%)<input type="number" id="pe-hurdle" value="8" min="0" max="20" step="0.5"></label>
<label>Rendement attendu d'un ETF (%)<input type="number" id="pe-retf" value="7" min="0" max="20" step="0.1"></label>
<label>Frais annuels de l'ETF (%)<input type="number" id="pe-fetf" value="0.25" min="0" max="3" step="0.05"></label>
</div>
<div class="sim-result" id="pe-out"></div>
</form>

<script>
(function(){
var LOC='fr-FR';
var f=function(n){return Math.round(n).toLocaleString(LOC)+' €';};
var pc=function(n){return (n*100).toFixed(1).replace('.', ',')+' %';};
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
    '<div class="sim-main"><span class="sim-main-value">'+pc(g)+'</span><span class="sim-main-label">de rendement brut annuel exigé du fonds pour faire jeu égal avec l\'ETF</span></div>'+
    '<div class="sim-tiles">'+
    '<div class="sim-tile"><strong>'+f(cibleEtf)+'</strong><span>ce que rapporte l\'ETF net de frais</span></div>'+
    '<div class="sim-tile"><strong>+'+ (ecart*100).toFixed(1).replace('.',',') +' pts</strong><span>de performance brute à trouver en plus</span></div>'+
    '<div class="sim-tile"><strong>'+f(preleve)+'</strong><span>prélevé au total par le fonds</span></div>'+
    '</div>';
}
document.querySelectorAll('#sim-pe input').forEach(function(i){i.addEventListener('input',calc);});
calc();
})();
</script>


## Pourquoi cette question est la bonne

Les plaquettes de private equity comparent presque toujours une performance **brute** à la performance d'un indice. C'est la comparaison la plus flatteuse qui soit, et la moins utile : ce qui vous revient, c'est la performance **après frais annuels et après commission de surperformance**.

Ce simulateur inverse le raisonnement. Plutôt que de vous demander de croire à un rendement annoncé, il calcule le rendement brut que le fonds doit **réellement** atteindre pour que vous finissiez au même niveau qu'avec un ETF indiciel acheté le même jour et laissé tranquille.

## Comment lire le résultat

Le pourcentage affiché est un **seuil d'équivalence**, pas une prévision. En dessous, vous auriez mieux fait de prendre l'ETF. Au-dessus, le fonds crée de la valeur pour vous, et pas seulement pour son gérant.

L'écart en points est le chiffre le plus parlant : il mesure la **surperformance brute** que le gérant doit produire, année après année, uniquement pour compenser sa propre structure de frais.

### Ce que le calcul intègre

| Élément | Traitement |
|---|---|
| Frais annuels du fonds | Déduits chaque année du rendement brut |
| Commission de surperformance | Prélevée sur la plus-value au-delà du seuil de déclenchement |
| Frais de l'ETF de comparaison | Déduits chaque année également |
| Durée | Identique pour les deux, aucune sortie anticipée |

### Ce qu'il n'intègre pas

L'**illiquidité**, qui n'a pas de prix affiché mais qui a un coût réel : pendant huit à dix ans, cet argent ne peut financer aucun autre projet. Les **appels de fonds échelonnés**, qui décalent la mise réelle. Et la **dispersion des résultats**, bien plus large en non coté qu'en indiciel : la moyenne d'une classe d'actifs ne dit rien du fonds particulier que l'on vous propose.

