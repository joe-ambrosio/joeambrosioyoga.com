---
layout: default
title: "Home"
---

<div id="payment-successful" class="infobox">
	Merci pour votre réservation ! Vous allez recevoir un email de confirmation.
</div>

<div id="welcome" class="infobox">
	<p>
		Hey, bienvenue !
		<br/>
		<br/>
		Je m’appelle Joe, et je vous propose ici de découvrir, d’explorer ou d’approfondir votre connaissance du yoga. 
		Le yoga est un chemin de découverte et de transformation. Il nous invite à explorer chaque partie de notre être : notre corps, notre respiration, nos pensées... Afin de les dépoussiérer, et de les laisser rayonner.
		<br/>
		<br/>
		Les asanas (postures) nous apprennent force, souplesse et légèreté du corps. En cherchant à les réaliser en conscience, nous développons aussi notre concentration et apaisons notre mental.
		<br/>
		<br/>
		Associé à pranayama (contrôle de la respiration) et dhyana (méditation), le yoga mène vers une profonde découverte de soi, et s’avère être un allié précieux pour notre vie quotidienne ! Dans chacun de mes cours, hatha ou vinyasa yoga, il me tient à cœur de vous proposer ces instants de méditation. 
		<br/>
		<br/>
		Donc si le cœur vous en dit, on se retrouve juste ici ↴
	</p>
</div>

<div id="book">
	<div>
		<h2>Réserver un cours</h2>
		<div id='calendar'></div>
	</div>
	<p>Les cours proposés sont en live, disponibles via un lien zoom que vous recevrez par mail.<br/><br/><br/><br/>Durée : 1h15<br/>Tarif : 10€</p>
</div>


<div id="yoga-types-info">
	<div class="infobox">
		<h2>Vinyasa</h2>
		<p>Le Vinyasa yoga est une pratique dynamique et créative, qui allie respiration et mouvement. En se synchronisant à la respiration, les asanas s’enchaînent de manière fluide, ce qui en fait un yoga endurant, au rythme plutôt soutenu. En pratiquant le Vinyasa yoga, on développe sa force et sa souplesse mais aussi son endurance et son agilité. </p>
	</div>
	<div class="infobox">
		<h2>Hatha</h2>
		<p>Le Hatha yoga représente l’approche traditionnelle du yoga. C‘est de celui-ci que sont issus tous les asanas. La pratique est plus introspective, plus méditative : nous prenons le temps de respirer longuement dans les postures, de les laisser évoluer, et d’en ressentir les effets dans tout le corps, au moment présent. Le rythme de la pratique est modéré. Le Hatha yoga fait travailler le corps en profondeur, la force tout autant que la souplesse. </p>
	</div>
</div>

<div id="modal">
  <div>
    <span id="close-modal">&times;</span>
    <div id="lesson-info">
    	<h2 id="lesson-title"></h2>
    	<p id="lesson-time"></p>
    	<p>
    		Tarif : <span id="lesson-price"></span>€<br/>
    		Durée : <span id="lesson-duration"></span><br/>
    		Places disponibles : <span id="lesson-bookings-remaining"></span>
    		<p id="lesson-description"></p>
    		<p id="lesson-warning">Cours non adapté aux femmes enceintes.</p>
    		<p id="lesson-id"></p>
    	</p>
    </div>
    <div class="booking" id="booking-info">
    	<p>Merci de remplir une addresse email valide, vous recevrez sur cette addresse toutes les informations pour vous connecter au cours.</p>
    	<input type="email" name="email" placeholder="jekiffeleyoga@gmail.com" id="email" />
    	<button id="lesson-book" disabled="disabled" type="submit">Continuer vers le paiement<span id="wait"></span></button>
    </div>
    <div class="booking" id="booking-full">
    	<p>Oups, ce cours est complet !</p>
    </div>
  </div>
</div>


<div>
	<script src="https://js.stripe.com/v3"></script>
	<script src="https://cdn.jsdelivr.net/npm/fullcalendar@5.3.2/main.min.js" integrity="sha256-mMw9aRRFx9TK/L0dn25GKxH/WH7rtFTp+P9Uma+2+zc=" crossorigin="anonymous"></script>
	<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/fullcalendar@5.3.2/main.min.css" integrity="sha256-uq9PNlMzB+1h01Ij9cx7zeE2OR2pLAfRw3uUUOOPKdA=" crossorigin="anonymous">
	<script>
		// Utils
	  function replaceForLesson(name, text) {
	    document.getElementById("lesson-" + name).innerText = text
	  }
	  // If end of payment flow
	  if(window.location.hash == "#payment-successful") {
	    document.getElementById("payment-successful").style.display = "block"
	  }
	  //
	  document.addEventListener('DOMContentLoaded', function() {
	    // Vars
	    const modal = document.getElementById("modal")
			const emailInput = document.getElementById('email')
	    const calendarEl = document.getElementById('calendar');
	    const lessonBook = document.getElementById("lesson-book")
	    const reEmail = /^\w+([-+.']\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/
	    const stripe = Stripe('pk_test_q23TkZKgp8unr6VHj80CFF4F00XhYwquMh');
			// Restore previous email inputed
			const previousEmail = localStorage.getItem('email')
			if (previousEmail) {
				emailInput.value = previousEmail
			}
		  // Modal handling
	    closeModal = () => modal.style.display = "none"
	    document.getElementById("close-modal").addEventListener("click", closeModal)
	    window.addEventListener("click", (event) => {
	      event.target == modal && closeModal()
	    })
			// Fetch events
	  	fetch('https://ga09zolgt2.execute-api.eu-west-3.amazonaws.com/events.json')
		  .then(response => {
		  	if (response.ok) {
		  		return response.json()
		  	} else {
		  		throw new Error("No OK response")
		  	}
		  })
		  .then(events => {
		  	calendar.addEventSource({
		  		events: events,
		  		color: "#74503b",
		  		textColor: "white"
		  	})
		  })
		  .catch(err => {
		  	console.error(err)
		  	calendarEl.prepend("Impossible de récupérer les cours actuellement, revenez plus tard.")
		  })
	    // Validate email in real time
	  	emailInput.addEventListener("input", (event) => {
	      	lessonBook.disabled = ! reEmail.test(String(event.target.value).toLowerCase())
	    })
	    emailInput.dispatchEvent(new Event("input"))
	    // Init FullCalendar
	    const calendar = new FullCalendar.Calendar(calendarEl, {
	      initialView: 'dayGridWeek',
	      titleFormat: { day: 'numeric', month: 'short' },
	      locale: 'fr',
	      firstDay: 1,
	      buttonText: {
	        today: "Aujourd'hui"
	      },
	      eventDisplay: "block",
	      eventTimeFormat: {
	        hour: '2-digit',
	        minute: '2-digit',
	        meridiem: false
	      },
	      height: "auto",
	      eventClick: (info) => {
	      	// Populate the modal
	        const durationMs = info.event.end - info.event.start
	        const duration = `${parseInt(durationMs/(3600*1000))}h${String(parseInt(durationMs/(60*1000))%60).padStart(2, '0')}`
	        const monthNames = ["janvier", "février", "mars", "avril", "mai", "juin", "juillet", "août", "septembre", "octobre", "novembre", "décembre"]
	        const dayNames = ["Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi", "Dimanche"]
	        const start = `${dayNames[info.event.start.getDay() - 1]} ${info.event.start.getDate()} ${monthNames[info.event.start.getMonth()]} à ${info.event.start.getHours()}h${String(info.event.start.getMinutes()).padStart(2, '0')}`
	        const bookingsRemaining = info.event.extendedProps.bookings_remaining
	        document.getElementById("booking-info").style.display = (bookingsRemaining > 0) ? "block" : "none"
	        document.getElementById("booking-full").style.display = (bookingsRemaining > 0) ? "none" : "block"
	        ;[
	          ["id", info.event.id],
	          ["title", info.event.extendedProps.long_title],
	          ["description", info.event.extendedProps.description],
	          ["time", start],
	          ["duration", duration],
	          ["price", info.event.extendedProps.price],
	          ["bookings-remaining", bookingsRemaining],
	        ].map(r => replaceForLesson(r[0], r[1]))
	        // Diplay it
	        modal.style.display = "flex"
	      }
	    })
	    calendar.render()
	    // Handle modal submit button
	  	document.getElementById('lesson-book').addEventListener("click", () => {
	  		// Loading animation
  		  const wait = document.getElementById("wait")
	  		const oldLessonBookText = lessonBook.innerText
	  		lessonBook.disabled = true
	  		const dotsSetInterval = setInterval(() => {
  		    if (wait.innerHTML.length > 3 ) 
  		        wait.innerHTML = ""
  		    else 
  		        wait.innerHTML += "."
	  		}, 250)
	  		// Save email
	  		const email = emailInput.value
	  		localStorage.setItem('email', email)
	  		// Create a Stripe Session
	     	fetch(
      		"https://ga09zolgt2.execute-api.eu-west-3.amazonaws.com/setupNewBooking",
      		{
      			method: "POST",
      			headers: { "Content-Type": "application/json" },
      			body: JSON.stringify({
    					id: document.getElementById("lesson-id").innerText,
    					email: email
      			})
      	})
        .then(response => {
        	if (response.ok) {
        		return response.json()
        	} else {
        		throw new Error("No OK response")
        	}
        })
        .then(j => {
        	stripe.redirectToCheckout({"sessionId": j.stripe_session_id})
        })
        .catch(err => {
        	// Clear animation
        	clearInterval(dotsSetInterval)
        	document.getElementById("wait").innerHTML = ""
        	lessonBook.innerText = oldLessonBookText
        	//
        	console.error(err)
        	document.getElementById("booking-info").append("Impossible de mettre en place le paiement, veuillez rééssayer plus tard.")
        })
	    })
	  });
	</script>
</div>