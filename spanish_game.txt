// define dictionary
var fruit_dictionary = {
	"apple":"manzana",
	"pear":"pera",
	"banana":"plátano",
	"kiwi":"kiwi",
	"cherry":"cereza",
	"strawberry":"fresa",
	"date":"datil",
	"peach":"melocotón",
	"watermelon":"sandía",
	"avocado":"aguacate",
	"orange":"naranja",
	"pineapple":"piña",
}
	
var jobs_dictionary = {
	"artist":"pintor",
	"builder":"constructor",
	"chef":"cocinero",
	"detective":"detective",
	"doctor":"médico",
	"fireman":"bombero",
	"fisherman":"pescador",
	"gardener":"jardinero",
	"mailman":"cartero",
	"pilot":"piloto",
	"plumber":"fontanero",
	"police_man":"policía",
	"mechanic":"mecánico",
	"sailor":"marinero",
}
	
var numbers_dictionary = {
	"zero":"cero",
	"one":"uno",
	"two":"dos",
	"three":"tres",
	"four":"cuatro",
	"five":"cinco",
	"six":"seis",
	"seven":"siete",
	"eight":"ocho",
	"nine":"nueve",
}
	
function main() {
	
	var dic, totalRight, catOption, categ;
	
	// function to add slash as background image
	function addBackgroundSlash(source, size) {
		source.style.backgroundImage = 'url("slash.png")';
		source.style.backgroundPosition = "center center";
		source.style.backgroundSize = size;
		source.style.backgroundRepeat = "no-repeat";
	}
	
	// music turns on and off when button is pressed
	var music = document.getElementById("music");
	music.addEventListener("click", function () {
		if (document.getElementById("backmusic").paused) {
			document.getElementById("backmusic").play();
			music.style.backgroundImage = 'none';
		} else {
			document.getElementById("backmusic").pause();
			addBackgroundSlash(music, "60%");
			music.style.backgroundPosition = "center center";
		}
	});
	
	// sound turns on and off when button is pressed
	var sound = document.getElementById("sound");
	document.getElementById("sound").addEventListener("click", function () {
		if (document.getElementById("checksound").muted) { 
			document.getElementById("checksound").muted = false
			document.getElementById("xsound").muted = false
			document.getElementById("win").muted = false
			sound.style.backgroundImage = 'none';
		} else {
			document.getElementById("checksound").muted = true
			document.getElementById("xsound").muted = true
			document.getElementById("win").muted = true;
			addBackgroundSlash(sound, "60%");
		};
	});
	
	/* create function to initialize variables,
	chose dictionary, clear opening screen
	and open main screen */
	function start() {
	
		// function to call the moveup animation for object obj
		function moveUp(obj) {
			document.getElementById(obj).style.animation
				 = "moveup 1s 1";
			document.getElementById(obj).style.animationFillMode 
				= "forwards";
			document.getElementById(obj).style.animationTimingFunction
				= "linear";
		}
	
		// start music on first round
		if (event.currentTarget.id == "firstRound") {
			document.getElementById("soundbuttonscontainer").
				style.display = "block";
			document.getElementById("backmusic").volume = 0.4;
			document.getElementById("backmusic").play();
		}
		
		// remove original "play" button 
		// (which is only used in the first round)
		document.getElementById("firstRound").style.display = "none";
		
		// remove cover
		moveUp("cover1");
		
		// enable user to chose dictionary
		catOption = document.getElementsByClassName("catoptions");
		for (var i = 0; i < catOption.length; i++) {
			catOption[i].addEventListener("click", choseCateg);
		}
		function choseCateg() {
			categ = event.currentTarget.id;
			setUpDic();
		}
		
		// fill dic, dicind, and totalright with values, 
		// call callEachTurn
		function setUpDic() {
			for (var i = 0; i < catOption.length; i++) {
				catOption[i].removeEventListener("click",
					choseCateg);
			}
			dic = Object.keys(eval(categ+"_dictionary"));
			if (event) {
				dicInd = [];
				for (var i = 0; i < dic.length; i++) {
					dicInd.push(i);
				}
				totalRight = 0;
			}
			callEachTurn(categ);
		}
		
		// start game, using the chosen dictionary
		function callEachTurn(categ) {
			moveUp("categories");
			eachTurn(categ);
		}
	}
	
	// call start function after button was pressed
	if (event) {
		start();
	} else {// in the first round, new game button triggers start
		document.getElementById("firstRound").addEventListener
			("click", start);
	}
	
	// set functions that take place each turn
	function eachTurn(cat) {
	  if (dicInd.length > 0) {
		  
		//remove the "next" button
		document.getElementById("next").style.display = "none";
		
		// choose a random item from the dictionary 
		// and its associated name in "text"
		window.randArrayRaw = Math.floor
			(Math.random()*dicInd.length);
		window.randArray = dicInd[window.randArrayRaw];
		document.getElementById("text").innerHTML = Object.
			values(eval(cat+"_dictionary"))[window.randArray];
		
		// remove the item just used from dicind
		dicInd.splice(window.randArrayRaw, 1);
		
		// create array rand of random numbers for option boxes
		window.rand = [];
		var optionBox = document.getElementsByClassName("box");
		for (var i = 0; i < optionBox.length; i++) {
			rand.push(Math.floor(Math.random()*dic.length));
		}
		
		// create and call a function that replaces double random numbers with unused ones
		function fixDoubles() {/*
			while (window.rand[0] == window.randArray ) {
				window.rand[0] = Math.floor(Math.random()*dic.length);
			}
			while (window.rand[1] == window.rand[0] || window.rand[1] == window.randArray ) {
				window.rand[1] = Math.floor(Math.random()*dic.length);
			}
			while (window.rand[2] == window.rand[0] || window.rand[2] == window.rand[1] || window.rand[2] == window.randArray ) {
				window.rand[2] = Math.floor(Math.random()*dic.length);
			}
			while (window.rand[3] == window.rand[0] || window.rand[3] == window.rand[1] || window.rand[3] == window.rand[2] || window.rand[3] == window.randArray ) {
				window.rand[3] = Math.floor(Math.random()*dic.length);
			}*/
			for (var i = 0; i < rand.length; i++) {
				while (window.rand.indexOf(rand[i]) != window.rand.
					lastIndexOf(rand[i]) || window.rand[i] == randArray) {
					rand[i] = Math.floor(Math.random()*dic.length);
				}
			}
		}
		fixDoubles();
		
		// print pictures in all boxes
		for (var i = 0; i < optionBox.length; i++){
			document.getElementById("outerbox"+i).innerHTML = 
				'<div id="box'+i+
				'" class="innerflexitem" ><img src='+cat+
				'/' + dic[eval("window.rand["+i+"]")] +
				'.png alt=' + dic[eval("window.rand["+i+
				"]")] + '></div>';
			document.getElementById("outerbox"+i).style.
				boxShadow = "0px 0px 20px gray";
		}
		
		// put the item associated with "randarray" 
		// into a random box "randbox"
		randBox = Math.floor(Math.random()*optionBox.length);
		document.getElementById("box"+randBox).innerHTML = 
			'<img src='+cat+'/' + dic[window.randArray] + 
			'.png alt=' + dic[window.randArray] + ' />';
		
		// create a function that displays a check or an X 
		// on the corner of the clicked object
		function giveFeedback(event) {
			var number = event.currentTarget.id.substr(8,1);
			if (number == randBox) {
				document.getElementById("checksound").play();
				document.getElementById("outerbox"+randBox).innerHTML += '<img src="check.png" height="25%" style="position: absolute; top: -5%; left: -4%;" />';
				totalRight++;
			} else {
				document.getElementById("xsound").play();
				document.getElementById("outerbox"+number).innerHTML += '<img src="x.png" height="25%" style="position: absolute; top: -5%; left: -4%;" />';
				document.getElementById("outerbox"+randBox).
					style.boxShadow = "0px 0px 20px red";
			}
			uncallFeedback();
			document.getElementById("next").style.display
				= "block";
		}
		
		// calls the giveFeedback function when a box is clicked
		function callFeedback() {
			for (var i = 0; i < optionBox.length; i++) {
				optionBox[i].addEventListener("click",
					giveFeedback);
			}
		}
		// uncalls giveFeedback
		function uncallFeedback() {
			for (var i = 0; i < optionBox.length; i++) {
				optionBox[i].removeEventListener("click",
					giveFeedback);
			}
		}
		callFeedback();
	  } else { // if dicind is empty
		// function that tells the player his "grade"
		function grading() {
	
			// calls the moveDown animation for object obj
			function moveDown(obj) {
				document.getElementById(obj).style.animation
					= "movedown 1s 1";
				document.getElementById(obj).style.
					animationFillMode = "forwards";
				document.getElementById(obj).style.
					animationTimingFunction = "linear";
			}
			
			// "cover" comes with "grade" and new game button
			document.getElementById("rank").style.display
				= "block";
			document.getElementById("laterRound").
				style.display = "block";
			document.getElementById("laterRound").
				addEventListener("click", start);
			moveDown("cover1");
			moveDown("categories");
			
			// "grade" is printed on cover
			if (totalRight == dic.length) {
				document.getElementById("cover1").
					style.backgroundImage =
					'url("confetti.gif"), url("blue.jpg")';
				document.getElementById("cover1").style.
					backgroundSize = "contain, 100vw 100vh";
				document.getElementById("winsound").play();
				document.getElementById("win").style.display
					= "none";
				document.getElementById("rank").innerHTML
					= '¡Ganaste!';
			} else {
				document.getElementById("win").style.display
					= "block";
				document.getElementById("cover1").style.
					backgroundImage = 'url("blue.jpg")';
				document.getElementById("cover1").style.
					backgroundSize = '100vw 100vh';
				if (totalRight >= 2/3*dic.length) {
					document.getElementById("rank").innerHTML
						= '¡Muy bueno!';
				} else if (totalRight >= 1/3*dic.length) {
					document.getElementById("rank").innerHTML
						= 'Es bueno';
				} else {
					document.getElementById("rank").innerHTML =
						'No bueno';
				}
				document.getElementById("win").innerHTML = 
					"You got " + totalRight + "/" + 
					dic.length +  " correct!";
			}
		}
		grading();
	  } // end else (meaning: dicind is used up)
	} // end eachturn
	
	// next button triggers new round
	document.getElementById("next").addEventListener("click", 
		function(){eachTurn(categ);});
	document.getElementById("next").removeEventListener("click", 
		function(){eachTurn(categ);});
}
main();