---
layout: post
title: 'Tickets sportcentrum'
date: 2015-04-16 15:40:51 +0200
category: blog
tags: sportcentrum, scrapers
---

Onlangs is in Nijmegen het Sportfondsenbad Oost vervangen door het [Erica Terpstra zwembad](http://www.sportfondsennijmegen.nl/).
Bijzonder gunstig voor mij is dat het bad op 5 minuten loopafstand van mijn studentenkamer ligt.
Ook mag ik als lid van het [Universitair Sportcentrum](http://www.ru.nl/sportcentrum/) er gratis zwemmen.

Ik doe dit dan ook regelmatig.
Inmiddels lig ik zo vaak in het water dat het claimen van een zwemticket een behoorlijke klus is geworden.
Dat zit zo:
het sportcentrum stelt elk half uur 10 sporttickets beschikbaar aan studenten en medewerkers die lid zijn.
Als lid kun je 2 tickets tegelijkertijd geclaimed hebben.
Zijn de tickets op? Dan heb je als ijverige zwemmer dikke pech.
Wil je toch zwemmen, dan moet je steeds de website refreshen, in de hoop dat iemand afzegt.
Alsof je niets beters te doen hebt.

Een beetje kundige programmeur kan natuurlijk dit hele proces [automatiseren](http://github.com/Wassasin/vrijbrief).
Vervolgens kun je een uitgebreide discussie houden over de eerlijkheid en de ethiek van dit soort oplossingen.
In de praktijk besluit ik 30 minuten van te voren dat ik wil zwemmen, en zet ik dan pas dat programmatje aan.
Ik krijg dus alleen de last-minute-uitschrijf-tickets te pakken.
Het afval dus; de tickets die toch niemand anders wil.
Enkel mensen die op 5 minuten loopafstand wonen kunnen dit doen.
Dus prima toch?

Alleen soms lukt dit niet.
Heel soms moet ik het programmatje uitzetten, omdat zelfs ik dan niet op tijd bij het zwembad aan kan lopen.
En wat zie ik dan?
Mensen die twee minuten van te voren uitschrijven.

Eerder zat er geen penalty op het niet uitprinten van je ticket:
Het kwam schijnbaar ook regelmatig voor dat leden alvast een ticket pikten, maar vervolgens besloten niet te gaan zwemmen.
Tegenwoordig wordt er na een paar van deze zogenaamde 'no-show' gebeurtenissen je sportkaart geblokkeerd.
Deze is dan weer te re-activeren na een donatie aan het Ronald Macdonald-huis.
Een sympathieke actie, als je het mij vraagt.

Maar nu schrijven mensen zich dus luttele momenten voor de aanloop tijd zich uit.
Echte opperklootzakken dus.
Want deze tickets zijn dan gewoon verspilt.
Ik vraag me dan af, hoe vaak komt dit eigenlijk voor?
De afgelopen week heb ik het aantal beschikbare tickets voor het zwemmen vastgelegd.

<div id="chart-normal"></div>

Deze grafieken doen vermoeden dat er altijd wel een plek vlak van te voren vrij is.
Wat als ik het uiteindelijk aantal vrije plekken plot, met daarnaast de fractie van die tickets die door asociale zwemmers net op het laatste moment zijn vrijgegeven?

<div id="chart-left"></div>

En inderdaad: 's ochtends schrijft men zich vaak op het laatste moment uit: precies wanneer ik regelmatig zwem.
De rest van de dag zijn aso-zwemmers zeldzamer.
Vermoedelijk besluit men dat wat langer doorsnoozen toch de voorkeur heeft boven het water in springen.
Helaas heb ik geen eerlijke oplossing voor dit probleem.

Misschien dat ik nu wat vaker na de lunch ga zwemmen.

<script>
parse_data = function(data) {
	var rows = [];
	$.each(data, function(_i, xs) {
		rows.push([Date.parse(xs[0]), xs[1]]);
	});
	return rows;
};

$(function() {
	$('#chart-normal').highcharts({
		chart: {
			type: 'spline'
		},
		title: {
			text: 'Aantal uiteindelijk vrije plekken voor een zwemmoment'
		},
		xAxis: {
			type: 'datetime',
			title: 'Tijdstip'
		},
		yAxis: {
			title: 'Aantal resterende inschrijvingen',
			min: 0,
			max: 10,
			tickInterval: 2
		}
	});

	$('#chart-left').highcharts({
		chart: {
			type: 'column'
		},
		title: {
			text: 'Aantal overgebleven plekken per zwemmoment'
		},
		xAxis: {
			type: 'datetime',
			title: 'Zwemmoment'
		},
		yAxis: {
			title: 'Aantal resterende inschrijvingen',
			min: 0,
			max: 10,
			tickInterval: 2
		}
	});

	$('#chart-asshole').highcharts({
		chart: {
			type: 'column'
		},
		title: {
			text: 'Aantal verspillende aso\'s per zwemmoment'
		},
		xAxis: {
			type: 'datetime',
			title: 'Zwemmoment'
		},
		yAxis: {
			title: 'Aantal aso\'s',
			min: 0,
			max: 4,
			tickInterval: 2
		}
	});

	$.ajax({type: "GET", url: "/assets/zwemtickets/2015-04-15%2008:30.json", dataType: 'json', success: function(data) {
		$('#chart-normal').highcharts().addSeries({
			name: '2015-04-15 08:30',
			data: parse_data(data),
			id: 0	
		});
	}});
	$.ajax({type: "GET", url: "/assets/zwemtickets/2015-04-15%2012:30.json", dataType: 'json', success: function(data) {
		$('#chart-normal').highcharts().addSeries({
			name: '2015-04-15 12:30',
			data: parse_data(data),
			id: 1	
		});
	}});
	$.ajax({type: "GET", url: "/assets/zwemtickets/2015-04-15%2016:30.json", dataType: 'json', success: function(data) {
		$('#chart-normal').highcharts().addSeries({
			name: '2015-04-15 16:30',
			data: parse_data(data),
			id: 2	
		});
	}});
	$.ajax({type: "GET", url: "/assets/zwemtickets/left 2015-04-15.json", dataType: 'json', success: function(data) {
		$('#chart-left').highcharts().addSeries({
			name: 'Uiteindelijke vrije plekken (2015-04-15)',
			data: parse_data(data),
			id: 0	
		});
	}});
	$.ajax({type: "GET", url: "/assets/zwemtickets/asshole 2015-04-15.json", dataType: 'json', success: function(data) {
		$('#chart-left').highcharts().addSeries({
			name: 'Waarvan vrijgegeven door aso\'s (2015-04-15)',
			data: parse_data(data),
			id: 1
		});
	}});
});
</script>
