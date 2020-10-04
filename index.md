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
    		Durée : <span id="lesson-duration"></span>
    		<p id="lesson-description"></p>
    		<p id="lesson-warning">Cours non adapté aux femmes enceintes.</p>
    		<p id="lesson-id"></p>
    	</p>
    </div>
    <div id="book-info">
    	<p>Merci de remplir une addresse email valide, vous recevrez sur cette addresse toutes les informations pour vous connecter au cours.</p>
    	<input type="email" name="email" autocomplete="true" placeholder="jekiffeleyoga@gmail.com" id="email" />
    	<button id="lesson-book" disabled="disabled">Continuer vers le paiement</button>
    </div>
  </div>
</div>


<div>
	<script src="https://js.stripe.com/v3"></script>
	<script src="https://cdn.jsdelivr.net/npm/fullcalendar@5.3.2/main.min.js" integrity="sha256-mMw9aRRFx9TK/L0dn25GKxH/WH7rtFTp+P9Uma+2+zc=" crossorigin="anonymous"></script>
	<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/fullcalendar@5.3.2/main.min.css" integrity="sha256-uq9PNlMzB+1h01Ij9cx7zeE2OR2pLAfRw3uUUOOPKdA=" crossorigin="anonymous">
	<script>
	  function replaceForLesson(name, text) {
	    document.getElementById("lesson-" + name).innerText = text
	  }
	  document.addEventListener('DOMContentLoaded', function() {
	    if(window.location.hash == "#payment-successful") {
	      document.getElementById("payment-successful").style.display = "block"
	    }
	    const modal = document.getElementById("modal");
	    closeModal = () => modal.style.display = "none"
	    document.getElementById("close-modal").addEventListener("click", closeModal)
	    window.addEventListener("click", (event) => {
	      event.target == modal && closeModal()
	    })
	    const lessonBook = document.getElementById("lesson-book")
	    const reEmail = /^\w+([-+.']\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/
	  	const emailField = document.getElementById("email")
	  	emailField.addEventListener("input", (event) => {
	      	lessonBook.disabled = ! reEmail.test(String(event.target.value).toLowerCase())
	    })
	    emailField.dispatchEvent(new Event("input"))
	    var stripe = Stripe('pk_test_q23TkZKgp8unr6VHj80CFF4F00XhYwquMh');
	    const now = new Date
	    var then = new Date
	    then.setDate(now.getDate() + 90)
	    const calendarEl = document.getElementById('calendar');
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
	      eventSources: [{
	        url: '/events.json',
	        startParam: now.toISOString(),
	        endParam: then.toISOString(),
	        color: "#74503b",
	        textColor: "white",
	        failure: () => {
	          calendarEl.prepend("Impossible de récupérer les cours actuellement, revenez plus tard.")
	        }
	      }],
	      eventClick: (info) => {
	        modal.style.display = "flex"
	        const durationMs = info.event.end - info.event.start
	        const duration = `${parseInt(durationMs/(3600*1000))}h${parseInt(durationMs/(60*1000))%60}`
	        const monthNames = ["janvier", "février", "mars", "avril", "mai", "juin", "juillet", "août", "septembre", "octobre", "novembre", "décembre"]
	        const dayNames = ["Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi", "Dimanche"]
	        const start = `${dayNames[info.event.start.getDay() - 1]} ${info.event.start.getDate()} ${monthNames[info.event.start.getMonth()]} à ${info.event.start.getHours()}h${String(info.event.start.getMinutes()).padStart(2, '0')}`
	        ;[
	          ["id", info.event.id],
	          ["title", info.event.extendedProps.long_title],
	          ["description", info.event.extendedProps.description],
	          ["time", start],
	          ["duration", duration],
	          ["price", info.event.extendedProps.price],
	        ].map(r => replaceForLesson(r[0], r[1]))
	        // stripe.redirectToCheckout({
	        //   lineItems: [{price: info.event.id, quantity: 1}],
	        //   mode: 'payment',
	        //   successUrl: window.location.href + '#payment-successful',
	        //   cancelUrl: window.location.href,
	        //   submitType: "book"
	        // })
	      },
	      height: "auto"
	    });
	    calendar.render();
	  });
	</script>
</div>