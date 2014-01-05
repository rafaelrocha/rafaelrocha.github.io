---
layout: post
title: "Fugindo da Macarronada Javascript com Module Pattern"
date: 2013-12-17 15:24:08 -0800
comments: true
categories: javascript pattern
author: "Rafael Rocha <a href='https://github.com/rafaelrocha' target='_blank'>@rafaelrocha</a>"
---

Demorei mas finalmente bloguei! Primeiro post e escrito em portugês ao contrário do jovem <a href='https://github.com/jlucasps' target='_blank'>@jlucasps</a>. Bão é assim, bagunça! Vamos ao que interessa, Javascript!

Porque a maioria das pessoas tem pavor de JavaScript? Tudo bem que ele faz bizarrises como:

* retornar "fooNaN" para <code>("foo" + + "bar")</code>
* retornar 10000000000000000 para  <code>9999999999999999</code>
* e retornar false para <code>0.1 + 0.2 == 0.3</code>

*Mais bizarrises [aqui](http://brian.io/slides/dotjs-2012/).*

Ele tem seus defeitos tadinho e não são poucos, mas o problema maior é o desprezo pela linguagem e a falta de curiosidade da maioria dos desenvolvedores sobre como fazer um código limpo e reutilizável. 
Module é um padrão que nos ajuda a criar abstrações sobre um domínio, encapsulando a lógica do seu problema e expondo somente aquilo que interessa.

<!-- more -->

O objetivo deste post não é explicar em detalhes como funciona este padrão mas sim mostrar as vantagens que ele traz. Para isso vamos implementar um filtro por período usando JQuery UI que nos deixe escolher o período (lógico) e que nos mostre a quantidade de mêses desse intervalo.

{% jsfiddle dPp8S [tabs] [skin] [height] [width] %}<br/>

Supimpa! Funciona belezinha mas se precisarmos de um outro filtro com o mesmo comportamento? Temos duas opções:

1. copy & paste trocando somente os id's dos elementos
2. extrair esse comportamento para um module

Prefiro a segunda opção mas para podermos comparar vamos implementar as duas!

* copy & paste trocando somente os id's dos elementos *(não gaste muito tempo com esse código, só faça uma análise crítica do mesmo e perceba a macarradonada que esta sendo cozinhada)*
		
{% jsfiddle xSaFE [tabs] [skin] [height] [width] %}<br/>

* comportamento extraído para um module chamado rangeDatePicker

{% jsfiddle b6x4b [tabs] [skin] [height] [width] %}<br/>

Basta olhar na quantidade de linhas de código adicionais para implementar o "copy & paste" contra o module para perceber que a segunda opção resolve o problema de forma mais elegante, limpa e manutenivel, 28 linhas contra 4!

{% codeblock Usando module lang:js %}
var rangeOne = rangeDatePicker("#initRangeDateA", "#endRangeDateA");
$('#computeA').click(function() {
    $('#differenceInMonthA').text(rangeOne.differenceInMonth())
});
{% endcodeblock %}

{% codeblock Cozinhando macarrão lang:js %}
$("#initRangeDateA").datepicker({
    defaultDate: "+1w",
    changeMonth: true,
    numberOfMonths: 1,
    onClose: function (selectedDate) {
        $("#endRangeDateA").datepicker("option", "minDate", selectedDate);
    }
});

$("#endRangeDateA").datepicker({
    defaultDate: "+1w",
    changeMonth: true,
    numberOfMonths: 1,
    onClose: function (selectedDate) {
        $("#initRangeDateA").datepicker("option", "maxDate", selectedDate);
    }
});

var differenceInMonthA = function() {
    var fromIdElementDate = $("#initRangeDateA").datepicker("getDate");
    var toIdElementDate = $("#endRangeDateA").datepicker("getDate");
    return (toIdElementDate.getFullYear() - fromIdElementDate.getFullYear()) * 12
    + toIdElementDate.getMonth() - fromIdElementDate.getMonth();
}

$('#computeA').click(function() {
    $('#differenceInMonthA').text(differenceInMonthA())
});
{% endcodeblock %}


A idéia desse post não é entrar em detalhes sobre como funciona o module mas sim ascender uma faísca de curiosidade na cabeça de quem ainda não o conhece. Quem estiver interessado em aprofundar, primeiro procure entender [closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures) e depois estude module. Abaixo seguem ótimas referências para estudo.

* [Modularização em JavaScript](http://tableless.com.br/modularizacao-em-javascript/#.UrNqIPRDsn4)
* [JavaScript Closures and the Module Pattern](http://www.joezimjs.com/javascript/javascript-closures-and-the-module-pattern)

Bjo!