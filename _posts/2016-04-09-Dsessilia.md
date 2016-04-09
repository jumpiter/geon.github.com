---
layout: post
category : Programming
tags : [dislessia, typoglycemia, Javascript]
title: Dsessilia
---
{% include JB/setup %}

Una amica affetta da dislessia mi ha descritto cosa prova nella lettura. Lei *è in grado* certamente di leggere, ma le occorre molta concentrazione dato che le lettere sembrano saltare qua.

Allora mi sono ricordato di aver letto qualcosa sulla [tipoglicemia](solo in inglese su https://en.wikipedia.org/wiki/Typoglycemia).
Perché non realizzare tutto questo interattivamente su un sito web usando Javascript? Potevate scommetterci!

Feel like making a bookmarklet of this or something? [Fork it](https://github.com/geon/geon.github.com/blob/master/_posts/2016-03-03-dsxyliea.md) on github.

> La dislessia è un disturbo classificato tra i Disturbi specifici di apprendimento (DSA) con il codice F81.0, e la sua principale manifestazione consiste nella difficoltà che hanno i soggetti colpiti a leggere velocemente e correttamente, nonché ad elaborare e comprendere quello che leggono.  Dato che leggere è un complesso processo mentale, la dislessia ha svariate espressioni. In generale le dislessie possono essere suddivise in due grandi gruppi.

> Quella di origine fonologica (o logopedica), caratterizzata dalla difficoltà a effettuare una lettura accurata e/o fluente e da scarse abilità nella scrittura (disortografia). Queste difficoltà derivano tipicamente da un deficit nella componente fonologica del linguaggio, che è spesso inatteso in rapporto alle altre abilità cognitive e alla garanzia di un'adeguata istruzione scolastica. Conseguenze secondarie possono includere i problemi di comprensione nella lettura e una ridotta pratica nella lettura che può impedire una crescita del vocabolario e della conoscenza generale.

> Quella di origine visiva, che non è causata da un difetto refrattivo, ma da una difficoltà di elaborazione e riconoscimento dell'immagine che arriva al cervello, anche se di buona qualità. In questo caso il dislessico non ha nessun tipo di difficoltà ad esprimersi o a comprendere il linguaggio verbale, ma ha molte difficoltà a distinguere i grafemi perché non riesce a percepirne le differenze, specialmente per quanto riguarda i gruppi "p, b, d, q", "s, z" e "a, e" nello stampato minuscolo, ma anche le cifre come "2, 5" o "6, 9". In alcuni casi è quindi associata anche a discalculia. In questi casi, la difficoltà nel riconoscere i grafemi sposta l'attenzione del bambino dal significato alla lettera, diminuendo drasticamente la possibilità di ricordare ciò che si legge. Inoltre per riconoscere il simbolo, il bambino è spesso costretto a rileggerlo più e più volte, abituandosi a fare dei movimenti oculari "a saltelli" che avranno come conseguenza la lettura speculare di parole monosillabiche come "il, lo, la, al.." o l'inventarsi la fine delle parole lunghe.

*Source: [Wikipedia](http://it.wikipedia.org/wiki/Dislessia)*


<script type="text/javascript" src="//cdnjs.cloudflare.com/ajax/libs/jquery/2.0.3/jquery.min.js"></script>
<script type="text/javascript">

"use strict";

$(function(){

	var getTextNodesIn = function(el) {
	    return $(el).find(":not(iframe,script)").addBack().contents().filter(function() {
	        return this.nodeType == 3;
	    });
	};

	// var textNodes = getTextNodesIn($("p, h1, h2, h3"));
	var textNodes = getTextNodesIn($("*"));



	function isLetter(char) {
		return /^[\d]$/.test(char);
	}


	var wordsInTextNodes = [];
	for (var i = 0; i < textNodes.length; i++) {
		var node = textNodes[i];

		var words = []

		var re = /\w+/g;
		var match;
		while ((match = re.exec(node.nodeValue)) != null) {

			var word = match[0];
			var position = match.index;

			words.push({
				length: word.length,
				position: position
			});
		}

		wordsInTextNodes[i] = words;
	};


	function messUpWords () {

		for (var i = 0; i < textNodes.length; i++) {

			var node = textNodes[i];

			for (var j = 0; j < wordsInTextNodes[i].length; j++) {

				// Only change a tenth of the words each round.
				if (Math.random() > 1/10) {

					continue;
				}

				var wordMeta = wordsInTextNodes[i][j];

				var word = node.nodeValue.slice(wordMeta.position, wordMeta.position + wordMeta.length);
				var before = node.nodeValue.slice(0, wordMeta.position);
				var after  = node.nodeValue.slice(wordMeta.position + wordMeta.length);

				node.nodeValue = before + messUpWord(word) + after;
			};
		};
	}

	function messUpWord (word) {

		if (word.length < 3) {

			return word;
		}

		return word[0] + messUpMessyPart(word.slice(1, -1)) + word[word.length - 1];
	}

	function messUpMessyPart (messyPart) {

		if (messyPart.length < 2) {

			return messyPart;
		}

		var a, b;
		while (!(a < b)) {

			a = getRandomInt(0, messyPart.length - 1);
			b = getRandomInt(0, messyPart.length - 1);
		}

		return messyPart.slice(0, a) + messyPart[b] + messyPart.slice(a+1, b) + messyPart[a] + messyPart.slice(b+1);
	}

	// From https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random
	function getRandomInt(min, max) {
		
		return Math.floor(Math.random() * (max - min + 1) + min);
	}


	setInterval(messUpWords, 50);
});


</script>
