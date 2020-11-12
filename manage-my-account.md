---
layout: default
title: "Gérer mon compte - "
description: "Gérer mon compte"
noindex: 1
---

<div class="infobox" id="my-account-summary">
	<p>
		Mon email : <span id="account-email"></span><br/>
		Solde de votre compte : <span id="account-balance"></span>€ (doit intégralement couvrir un cours pour être utilisé)<br/>
		Nombre de cours réservés à venir : <span id="nb-future-bookings"></span><br/><br/>
		<a href="/">Réserver votre prochain cours</a>
	</p>
</div>

<div class="booked-class infobox" id="booked-class-template">
	<div>
		<h2 class="booked-class-title"></h2><br/>
		Date : <span class="booked-class-date"></span><br/>
		Durée : <span class="booked-class-duration"></span><br/>
		<p class="booked-class-description"></p><br/>
		Vous pouvez modifier cette réservation de plusieurs façons :
	</div>
	<div class="booked-class-options">
		<div data-postpone-or-credit-or-be-refunded-option="1">
			<h3>Reporter ce cours</h3>
			<p>Reporter gratuitement cette réservation sur le cours de votre choix. Le cours choisi doit être au même prix que celui déjà réservé. Recommandé si vous ne pouvez pas assister à ce cours mais voulez assister à un autre cours déjà planifié.</p>
			<button data-href="/#postpone?customerId=%customerId%&customerEmail=%customerEmail%&lessonToPostponeId=%currentLessonId%&alreadyBookedLessons=%alreadyBookedLessons%&lessonToPostponePrice=%lessonToPostponePrice%" data-onclick="redirect">Reporter<span class="wait"></span></button>
		</div>
		<div data-postpone-or-credit-or-be-refunded-option="1">
			<h3>Créditer mon compte</h3>
			<p>Créditer la valeur de ce cours sur mon compte. La prochaine fois que je réserverai avec mon email je n'aurai pas à payer. Recommandé si vous ne pouvez pas assister à ce cours mais que vous n'êtes pas encore sûr de quand vous pourrez le rattraper.</p>
			<button data-href="{{site.apiBaseUrl}}/account/credit?customerId=%customerId%&id=%currentLessonId%">Créditer<span class="wait"></span></button>
		</div>
		<div data-postpone-or-credit-or-be-refunded-option="1">
			<h3>Demander un remboursement</h3>
			<p>Me faire rembourser de la valeur de ce cours. Recommandé si vous ne pouvez pas assister à ce cours et que vous ne pensez pas reprendre un cours avec moi. Le remboursement se fera sur la carte bleue qui a servi au paiement sous 5 à 10 jours.</p>
			<button data-href="{{site.apiBaseUrl}}/account/refund?customerId=%customerId%&id=%currentLessonId%">Me faire rembourser<span class="wait"></span></button>
		</div>
		<div data-postpone-or-credit-or-be-refunded-option="0">
			<h3>Annuler ce cours</h3>
			<p>Vous ne pouvez plus assister à ce cours ? Annulez votre réservation pour libérer votre place, merci !</p>
			<button data-href="{{site.apiBaseUrl}}/account/cancel?customerId=%customerId%&id=%currentLessonId%">Annuler ce cours<span class="wait"></span></button>
		</div>
	</div>
</div>

<div>
	<script>
		function clickOption(event) {
	  		if (event.target.dataset.onclick === "redirect") {
          amplitude.getInstance().logEvent('clickPostponeOption', {href: event.target.dataset.href})
	  			window.location.href = event.target.dataset.href
	    	} else {
          if (event.target.dataset.href.includes("/credit")) {
            amplitude.getInstance().logEvent('clickCreditOption', {href: event.target.dataset.href})
          } else if (event.target.dataset.href.includes("/refund")) {
            amplitude.getInstance().logEvent('clickRefundOption', {href: event.target.dataset.href})
          } else {
            amplitude.getInstance().logEvent('clickCancelOption', {href: event.target.dataset.href})
          }
	    		clearAnimation = animateWaitElement(event.target.querySelector("span"), event.target)
		     	fetch(
	      		event.target.dataset.href,
	      		{ method: "POST" }
		      	)
		        .then(response => {
		        	if (response.ok) {
		        		return response.json()
		        	} else {
		        		throw new Error("No OK response")
		        	}
		        })
		        .then(j => {
		        	window.location.href = j.redirect_to
		        })
		        .catch(err => {
		        	clearAnimation()
		        	console.error(err)
		        	event.target.parentNode.parentNode.parentNode.append("Impossible d'effectuer cette opération, veuillez rééssayer plus tard.")
            amplitude.getInstance().logEvent('errClickOption', {err: String(err)})
		        })
	    	}
		}
		document.addEventListener('DOMContentLoaded', function() {
			if (window.location.hash) {
				const customerId = window.location.hash.slice(1)
			  	fetch('{{site.apiBaseUrl}}/account?customerId=' + customerId)
				  .then(response => {
				  	if (response.ok) {
				  		return response.json()
				  	} else {
				  		throw new Error("No OK response")
				  	}
				  })
				  .then(account => {
				  	document.getElementById("account-email").innerText = account.email
            amplitude.getInstance().setUserId(account.email)
				  	document.getElementById("account-balance").innerText = account.balance
				  	document.getElementById("nb-future-bookings").innerText = account.bookings.length
				  	const alreadyBookedLessons = []
				  	for (booking of account.bookings) {
				  		alreadyBookedLessons.push(booking.id)
				  	}
				  	for (booking of account.bookings) {
				  		const {durationHuman, startHuman} = datetimeToFrenchDatetimeAndDuration(new Date(booking.start_datetime), new Date(booking.end_datetime))
				  		let clone = document.querySelector('#booked-class-template').cloneNode(true)
				  		clone.setAttribute( 'id', "")
				  		clone.querySelectorAll(".booked-class-title")[0].innerText = booking.long_title
				  		clone.querySelectorAll(".booked-class-description")[0].innerText = booking.description
				  		clone.querySelectorAll(".booked-class-date")[0].innerText = startHuman
				  		clone.querySelectorAll(".booked-class-duration")[0].innerText = durationHuman
				  		clone.querySelectorAll(".booked-class-options > div").forEach((el) => {
				  			const PCRO = el.dataset.postponeOrCreditOrBeRefundedOption === "1"
                                                        el.style.display = (PCRO == booking.customer_can_postpone_or_credit_or_be_refunded) ? "flex" : "none"
				  		})
				  		clone.querySelectorAll("button").forEach((el) => {
				  			el.dataset.href = el.dataset.href.replace("%currentLessonId%", booking.id)
				  			el.dataset.href = el.dataset.href.replace("%lessonToPostponePrice%", booking.price)
				  			el.dataset.href = el.dataset.href.replace("%customerId%", customerId)
				  			el.dataset.href = el.dataset.href.replace("%customerEmail%", account.email)
				  			el.dataset.href = el.dataset.href.replace("%alreadyBookedLessons%", alreadyBookedLessons.join(","))
				  			el.addEventListener("click", clickOption)
				  		})
				  		document.querySelector('#content').appendChild(clone)
				  	}
				  })
				  .catch(err => {
				  	console.error(err)
				  	document.querySelectorAll('.infobox p')[0].innerText = "Impossible de récupérer les informations de votre compte, revenez plus tard."
            amplitude.getInstance().logEvent('errGetCustomerAccount', {err: String(err)})
				  })
			}
		})
	</script>
</div>
