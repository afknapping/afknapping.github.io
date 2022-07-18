<script src="https://unpkg.com/hyperscript.org@0.9.6"></script>

<textarea id="thecontent" _="
							 on click log 'clicked'
							 end
							 on keydown[8 or 46 or 37 or 38 or 39 or 40]
							 if event.keyCode is 8
							 or event.keyCode is 46
							 or event.keyCode is 37
							 or event.keyCode is 38
							 or event.keyCode is 39
							 or event.keyCode is 40
								 event.preventDefault()
								 log 'prohibited'
							 end
							 if #thecontent.selectionStart is not #thecontent.selectionEnd
							 event.preventDefault()
							 log 'prohibited'
							 end
							 end
							 

							 

											
" >
</textarea>


<p>
<button id="clear" _="on click clearcontent() then hide #clear then hide #copy then setFocus()">
clear
</button>

<button id="restore" _="on click loadcontent() then hide #restore then show #clear then show #copy then setFocus()">
restore
</button>

<button id="copy" _="on click copycontent() then setFocus()">
Copy
</button>

<button id="fullscreen" _="on click
						   document.documentElement.requestFullscreen()
							   then hide #fullscreen
							   then show #exitfullscreen
						   		 then setFocus()
						  ">
		fullscreen
</button>
<button id="exitfullscreen" _="on click
						   document.exitFullscreen()
							   then show #fullscreen
							   then hide #exitfullscreen
							    then setFocus()
						  ">
		exit fullscreen
</button>
</p>

<script type="text/hyperscript">

def checkforforbiddenkeys(keyCode) -- unused, would need to pass keycode
	 if keyCode is 8
		or keyCode is 46
		or keyCode is 37
		or keyCode is 38
		or keyCode is 39
		or keyCode is 40
		   event.preventDefault()
		   log 'prohibited'
	end
end

def restorecontent()
  wait 1s
end

def copycontent()
navigator.clipboard.writeText(#thecontent.value)
log "clipboard: "+navigator.clipboard.readText()
end

def clearcontent()
  put "" into #thecontent.value
  show #restore
end

def savecontent()
  if #thecontent.value != 0
	  hide #restore
	  show #copy
	  show #clear
	  set field to "content"
	  localStorage.setItem(field, #thecontent.value)
	  localStorage.setItem("backup", #thecontent.value)						
	  log "saved "+#thecontent.value+" to "+field
  else
    if localStorage.getItem("content") is not empty
      show #restore
    end
    hide #copy
    log "empty"
  end
end

def loadcontent()
  put localStorage.getItem("content") into #thecontent.value
end

on load
if localStorage.getItem("content") is empty
  hide #restore
end
hide #clear
hide #copy
hide #exitfullscreen
-- loadcontent()
repeat forever
  wait 2s
  -- log "calling save"
  savecontent()
end

if localStorage.getItem("content") is not empty
  log 'now shoing restore'
  show #restore
end
		  
</script>
					   
<style>
body { padding: 2em}
</style>
	
<script type="text/javascript">


// thx https://www.endyourif.com/set-cursor-position-of-textarea-with-javascript/

function setSelectionRange(input, selectionStart, selectionEnd, event) {
// console.log('some loggy')
if (input.setSelectionRange) {
  event.preventDefault();
  input.focus();
  input.setSelectionRange(selectionStart, selectionEnd);
  }

else if (input.createTextRange) {
  var range = input.createTextRange();
  range.collapse(true);
  range.moveEnd('character', selectionEnd);
  range.moveStart('character', selectionStart);
  range.select();
  }
}

var setCaretToPos = function(input, pos, event) {
  setSelectionRange(input, pos, pos, event)
  console.log('set caret to end')
}



	
function setFocus() {
	input.focus();
}
function catchClick() {
  setCaretToPos(input, 3000, event);
}

	
const input = document.querySelector('#thecontent');
input.addEventListener('mousedown', catchClick);



// Find the right method, call on correct element
function launchFullScreen(element) {
  if(element.requestFullScreen) {
    element.requestFullScreen();
  } else if(element.mozRequestFullScreen) {
    element.mozRequestFullScreen();
  } else if(element.webkitRequestFullScreen) {
    element.webkitRequestFullScreen();
  }
}

// Launch fullscreen for browsers that support it!
// launchFullScreen(document.documentElement); // the whole page
// launchFullScreen(document.getElementById("videoElement")); // any individual element

	
window.addEventListener('load', (event) => {
    console.log('The page has fully loaded')
	input.focus();

})
// document.addEventListener('document.exitFullscreen', (event) => {
// 	console.log('fullscreen exited')
// })
	
document.addEventListener('fullscreenchange', function(e) {
	
    console.log("fullscreen is: "+document.fullscreen)
	if document.fullscreen == true {
		// document.querySelector("#fullscreen")
	}
	// if ((document.fullScreenElement && document.fullScreenElement !== null) || 
	// (!document.mozFullScreenElement && !document.webkitFullScreenElement)) {
	// console.log('fullscreen');
	// } else {
	// console.log('not fullscreen');
	// }
}, false);

</script>

<style>

#thecontent {
  width: 100%;
  height: 50vh;
  box-sizing: border-box;
/*   border: none; */
 border: none;
  font-size: 24px;
  background-color: #ddd;
  font-family: monospace;
}


#thecontent:focus {
  outline: none;
}


body {
  padding: 3em;
  background-color: #ddd;
}

</style>






<style>
  
.markdown-body h1:first-of-type {
  display: none;
}
  
<style>
