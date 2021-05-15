---
layout: default
title: ""
description: "Hey, bienvenue ! Je m’appelle Joe, et je vous propose ici de découvrir ou d’approfondir votre connaissance du yoga."
---

<div id="payment-credit-successful" class="infobox end-of-flow-success">
	Merci pour votre réservation ! Vous avez réservé ce cours grâce au solde de votre compte. Le nouveau solde de votre compte est de <span class="new-balance"></span>€. Vous allez recevoir un email de confirmation.
</div>

<div id="payment-successful" class="infobox end-of-flow-success">
	Merci pour votre réservation ! Vous allez recevoir un email de confirmation.
</div>

<div id="credit-successful" class="infobox end-of-flow-success">
	Cours crédité avec succès ! Le nouveau solde de votre compte est de <span class="new-balance"></span>€. Vous allez recevoir un email de confirmation.
</div>

<div id="cancel-successful" class="infobox end-of-flow-success">
	Cours annulé avec succès !
</div>

<div id="refund-successful" class="infobox end-of-flow-success">
	Cours remboursé avec succès ! Le remboursement se fera sur la carte bleue qui a servi au paiement sous 5 à 10 jours. Vous allez recevoir un email de confirmation.
</div>

<div id="postpone-successful" class="infobox end-of-flow-success">
	Cours reporté avec succès ! Vous allez reçevoir un email de confirmation. 
</div>

<div id="postpone-mode" class="infobox">
	Pour finaliser votre report de cours, veuillez sélectionner le nouveau cours qui vous convient. 
</div>

{% if site.news %}
<div id="news" class="infobox">
    <p>
      {{ site.news }}
    </p>
</div>
{% endif %}

<div id="welcome" class="infobox">
	<p>
		Hey, bienvenue ! 
		<br/>
		<br/>
		Je m’appelle Joe, et je vous propose ici de découvrir ou d’approfondir votre connaissance du yoga. 
		<br/>
		<br/>
		Le yoga nous invite à explorer chaque partie de notre être : notre corps, notre respiration, notre mental... Dans le but de nous découvrir réellement. Il nous aide à dépoussiérer ces différents aspects de nous, afin de les laisser rayonner. 
		<br/>
		<br/>
		Les asanas (postures) nous apprennent force, souplesse et légèreté du corps. En cherchant à les réaliser en conscience, nous développons aussi notre concentration et apaisons notre mental.
		<br/>
		<br/>
                Lors de mes cours, j’aime aussi porter votre attention sur votre respiration, et accorder des temps de méditation, essentiels selon moi pour profiter pleinement des bienfaits de la pratique. 
		<br/>
		<br/>
		Le yoga mène vers une profonde découverte de soi, et s’avère être un allié précieux pour notre vie quotidienne ! 
		<br/>
		<br/>
		Donc si le cœur vous en dit, on se retrouve juste ici ! ↴
	</p>
	<img id="v-posture-yoga" src="assets/v-posture-yoga.jpg"/>
</div>


<div id="book">
	<div>
		<h2>Mes prochains cours</h2>
                <p>Cliquez sur une séance pour en savoir plus !</p>
		<div id='calendar'></div>
	</div>
	<p>Les cours proposés sont en live, disponibles via un lien Zoom que vous recevrez par mail.<br/><br/>Votre réservation est :<br/><br/>- Flexible : vous pouvez reporter votre cours ou vous faire rembourser facilement.<br/>- Rapide : pas de création de compte, la réservation est liée à votre adresse email.</p>
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
    		<p id="lesson-warning">Tous niveaux.<br/>Cours non adapté aux femmes enceintes.</p>
    	</p>
    </div>
    <div class="booking" id="booking-info">
    	<p>Merci de remplir une adresse email valide, vous recevrez sur cette adresse toutes les informations pour vous connecter au cours.</p>
    	<input type="email" name="email" placeholder="jekiffeleyoga@gmail.com" id="email" />
    	<button id="lesson-book" disabled="disabled" type="submit"><span id="wait"></span></button>
    </div>
    <div class="booking" id="booking-full">
    	<p>Oups, ce cours est complet !</p>
    </div>
  </div>
</div>


<div>
	<script defer src="https://js.stripe.com/v3"></script>
	<script defer src="https://cdn.jsdelivr.net/npm/fullcalendar@5.3.2/main.min.js" integrity="sha256-mMw9aRRFx9TK/L0dn25GKxH/WH7rtFTp+P9Uma+2+zc=" crossorigin="anonymous"></script>
	<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/fullcalendar@5.3.2/main.min.css" integrity="sha256-uq9PNlMzB+1h01Ij9cx7zeE2OR2pLAfRw3uUUOOPKdA=" crossorigin="anonymous">
	<script>
	  document.addEventListener('DOMContentLoaded', function() {
		// Utils
		window.vars = {postponeMode: false}
	  function replaceForLesson(name, text) {
	    document.getElementById("lesson-" + name).innerText = text
	  }
	  // If end of flow
	  if (window.location.hash == "#payment-successful") {
	    document.getElementById("payment-successful").style.display = "block"
      amplitude.getInstance().logEvent('paymentSuccessful')
	  }
	  else if (window.location.hash.startsWith("#payment-credit-successful")) {
	    document.getElementById("payment-credit-successful").style.display = "block"
	    document.querySelectorAll(".new-balance").forEach((el) => {
	    	el.innerText = window.location.hash.split(":")[1]
	    })
      amplitude.getInstance().logEvent('paymentCreditSuccessful')
	  }
	  else if (window.location.hash == "#postpone-successful") {
	    document.getElementById("postpone-successful").style.display = "block"
      amplitude.getInstance().logEvent('postponeSuccessful')
	  }
	  else if (window.location.hash == "#cancel-successful") {
	    document.getElementById("cancel-successful").style.display = "block"
      amplitude.getInstance().logEvent('cancelSuccessful')
	  }
	  else if (window.location.hash == "#refund-successful") {
	    document.getElementById("refund-successful").style.display = "block"
      amplitude.getInstance().logEvent('refundSuccessful')
	  }
	  else if (window.location.hash.startsWith("#credit-successful")) {
	    document.getElementById("credit-successful").style.display = "block"
	    document.querySelectorAll(".new-balance").forEach((el) => {
	    	el.innerText = window.location.hash.split(":")[1]
	    })
      amplitude.getInstance().logEvent('creditSuccessful')
	  }
	  // If postpone mode
          const emailInput = document.getElementById('email')
	    const lessonBook = document.getElementById("lesson-book")
	  if (window.location.hash.startsWith("#postpone?")) {
	  	const params = new URLSearchParams(window.location.hash.slice(10))
	  	window.vars.postponeMode = true
                emailInput.value = params.get("customerEmail")
                amplitude.getInstance().setUserId(params.get("customerEmail"))
                emailInput.disabled = true
	  	window.vars.customerId = params.get("customerId")
	  	window.vars.lessonToPostponeId = params.get("lessonToPostponeId")
	  	window.vars.lessonToPostponePrice = parseInt(params.get("lessonToPostponePrice"))
	  	window.vars.alreadyBookedLessons = params.get("alreadyBookedLessons").split(",")
	    document.getElementById("postpone-mode").style.display = "block"
	  }
	    // Vars
	    const modal = document.getElementById("modal")
	    const calendarEl = document.getElementById('calendar');
	    const reEmail = /^\w+([-+.']\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/
	    const stripe = Stripe('pk_live_Pn9Hu57ZG2cuRQ0eplC3KcEl00mgnX2Bfg');
            if (!window.vars.postponeMode) {
			// Restore previous email inputed
			const previousEmail = localStorage.getItem('email')
			if (previousEmail) {
				emailInput.value = previousEmail
                                amplitude.getInstance().setUserId(previousEmail)
			}
            }
		  // Modal handling
	    closeModal = () => { modal.style.display = "none"; amplitude.getInstance().logEvent('closeModal') }
	    document.getElementById("close-modal").addEventListener("click", closeModal)
	    window.addEventListener("click", (event) => {
	      event.target == modal && closeModal()
	    })
			{% if site.bookingEnabled %}
			// Fetch events
	  	fetch('{{site.apiBaseUrl}}/events.json')
		  .then(response => {
		  	if (response.ok) {
		  		return response.json()
		  	} else {
		  		throw new Error("No OK response")
		  	}
		  })
		  .then(events => {
		  	let filter = []
		  	if (window.vars.postponeMode) {
                           filter = window.vars.alreadyBookedLessons.concat(events.filter(e => e.price !== window.vars.lessonToPostponePrice).map(e => e.id))
                        }
		  	const filteredEvents = events.filter(e => ! filter.includes(e.id))
		  	calendar.addEventSource({
		  		events: filteredEvents,
		  		color: "#74503b",
		  		textColor: "white"
		  	})
		  })
		  .catch(err => {
		  	console.error(err)
		  	calendarEl.prepend("Impossible de récupérer les cours actuellement, revenez plus tard.")
        amplitude.getInstance().logEvent('errGetEvents', {err: String(err)})
		  })
		  {% endif %}
	    // Validate email in real time
	  	emailInput.addEventListener("input", (event) => {
	      	lessonBook.disabled = ! reEmail.test(String(event.target.value).toLowerCase())
	    })
	    emailInput.dispatchEvent(new Event("input"))
	    // Init FullCalendar
      let firstRender = true
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
              datesSet: (dateInfo) => {
                if (firstRender) {
                  firstRender = false
                } else {
                  amplitude.getInstance().logEvent('changeCalendarView', {range: dateInfo.view.title})
                }
              },
	      eventClick: (info) => {
	      	// Populate the modal
	      	const {durationHuman, startHuman} = datetimeToFrenchDatetimeAndDuration(info.event.start, info.event.end)
	        const bookingsRemaining = info.event.extendedProps.bookings_remaining
	        document.getElementById("booking-info").style.display = (bookingsRemaining > 0) ? "block" : "none"
	        document.getElementById("booking-full").style.display = (bookingsRemaining > 0) ? "none" : "block"
	        window.vars.lessonId = info.event.id
                amplitude.getInstance().logEvent('clickEvent', {id: info.event.id, name: `${info.event.extendedProps.long_title} ${startHuman}`})
	        ;[
	          ["title", info.event.extendedProps.long_title],
	          ["description", info.event.extendedProps.description],
	          ["time", startHuman],
	          ["duration", durationHuman],
	          ["price", info.event.extendedProps.price],
	          ["bookings-remaining", bookingsRemaining],
	        ].map(r => replaceForLesson(r[0], r[1]))
          if (window.vars.postponeMode) {
            lessonBook.innerHTML = "Reporter pour ce cours" + lessonBook.innerHTML.replace(/^[^<]+/, "")
          } else if (info.event.extendedProps.price === 0) {
            lessonBook.innerHTML = "Réserver" + lessonBook.innerHTML.replace(/^[^<]+/, "")
          } else {
            lessonBook.innerHTML = "Continuer vers le paiement" + lessonBook.innerHTML.replace(/^[^<]+/, "")
          }
	        // Diplay it
	        modal.style.display = "flex"
          amplitude.getInstance().logEvent('openModal')
	      }
	    })
	    calendar.render()
	    // Handle modal submit button
	  	lessonBook.addEventListener("click", () => {
	  		// Loading animation
  		  clearAnimation = animateWaitElement(document.getElementById("wait"), lessonBook)
	  		// Save email
	  		const email = emailInput.value
	  		localStorage.setItem('email', email)
        amplitude.getInstance().setUserId(email)
	  		if (window.vars.postponeMode) {
		  		// Postpone
          amplitude.getInstance().logEvent('clickPostpone', {id: window.vars.lessonToPostponeId, newId: window.vars.lessonId})
		     	fetch(
	      		`{{site.apiBaseUrl}}/account/postpone?customerId=${window.vars.customerId}&id=${window.vars.lessonToPostponeId}&newId=${window.vars.lessonId}`,
	      		{ method: "POST" }
	      	)
	        .then(response => {
	        	if (response.ok) {
	        		window.scrollTo({ top: 0, behavior: 'auto' })
	        		window.location.hash = "#postpone-successful"
	        		window.location.reload()
	        	} else {
	        		throw new Error("No OK response")
	        	}
	        })
	        .catch(err => {
	        	clearAnimation()
	        	console.error(err)
	        	document.getElementById("booking-info").append("Impossible de reporter le cours, veuillez rééssayer plus tard.")
            amplitude.getInstance().logEvent('errPostpone', {err: String(err)})
	        })
	  		} else {
		  		// Create a Stripe Session
          amplitude.getInstance().logEvent('clickBook', {id: window.vars.lessonId})
		     	fetch(
	      		"{{site.apiBaseUrl}}/setupNewBooking",
	      		{
	      			method: "POST",
	      			headers: { "Content-Type": "application/json" },
	      			body: JSON.stringify({
	    					id: window.vars.lessonId,
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
	        	if (j.redirect_to_hash) {
	        		window.scrollTo({ top: 0, behavior: 'auto' })
	        		window.location.hash = j.redirect_to_hash
	        		window.location.reload()
	        	} else {
	        		stripe.redirectToCheckout({"sessionId": j.stripe_session_id})
	        	}
	        })
	        .catch(err => {
	        	clearAnimation()
	        	console.error(err)
	        	document.getElementById("booking-info").append("Impossible de mettre en place le paiement, veuillez rééssayer plus tard.")
            amplitude.getInstance().logEvent('errBook', {err: String(err)})
	        })
	  		}
	    })
	  })
	</script>
</div>
