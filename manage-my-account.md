---
layout: default
---

<div class="infobox">
	<p>
	Mon email: <span id="account-email"></span><br/>
	Crédit de votre compte : <span id="account-balance"></span><br/>
	Nombre de cours réservés à venir : <span id="nb-future-bookings"></span>
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
		<div>
			<h3>Reporter ce cours</h3>
			<p>Reporter gratuitement cette réservation sur le cours de votre choix. Recommandé si vous ne pouvez pas assister à ce cours mais voulez assister à un autre cours déjà planifié.</p>
			<button data-href="/#postpone?customerId=%customerId%&lessonToPostponeId=%lessonToPostponeId%">Reporter</button>
		</div>
		<div>
			<h3>Créditer mon compte</h3>
			<p>Créditer la valeur de ce cours sur mon compte. La prochaine fois que je réserverais avec mon email je n'aurais pas à payer ce cours. Recommandé si vous ne pouvez pas assister à ce cours mais que vous n'êtes pas encore sûr de quand vous pourrez le rattraper.</p>
			<button data-href="https://ga09zolgt2.execute-api.eu-west-3.amazonaws.com/account/credit?customerId=%customerId%&id=%lessonToPostponeId%">Créditer</button>
		</div>
		<div>
			<h3>Demander un remboursement</h3>
			<p>Me faire remboursser de la valeur de ce cours. Recommandé si vous ne pouvez pas assister à ce cours et que vous ne pensez pas reprendre un cours ici. Le rembourssement sera fera sur la carte bleu qui a servi au paiement sous 5 à 10 jours.</p>
			<button data-href="https://ga09zolgt2.execute-api.eu-west-3.amazonaws.com/account/refund?customerId=%customerId%&id=%lessonToPostponeId%">Me faire rembourser</button>
		</div>
	</div>
</div>


<div>
	<script>
		document.addEventListener('DOMContentLoaded', function() {
			if (window.location.hash) {
				const customerId = window.location.hash.slice(1)
			  	fetch('https://ga09zolgt2.execute-api.eu-west-3.amazonaws.com/account?customerId=' + customerId)
				  .then(response => {
				  	if (response.ok) {
				  		return response.json()
				  	} else {
				  		throw new Error("No OK response")
				  	}
				  })
				  .then(account => {
				  	document.getElementById("account-email").innerText = account.email
				  	document.getElementById("account-balance").innerText = account.balance
				  	document.getElementById("nb-future-bookings").innerText = account.bookings.length
				  		console.log(account)
				  	for (booking of account.bookings) {
				  		const {durationHuman, startHuman} = datetimeToFrenchDatetimeAndDuration(new Date(booking.start_datetime), new Date(booking.end_datetime))
				  		let clone = document.querySelector('#booked-class-template').cloneNode(true)
				  		clone.setAttribute( 'id', "")
				  		clone.querySelectorAll(".booked-class-title")[0].innerText = booking.long_title
				  		clone.querySelectorAll(".booked-class-description")[0].innerText = booking.description
				  		clone.querySelectorAll(".booked-class-date")[0].innerText = startHuman
				  		clone.querySelectorAll(".booked-class-duration")[0].innerText = durationHuman
				  		clone.querySelectorAll("button").forEach((el) => {
				  			el.dataset.href = el.dataset.href.replace("%lessonToPostponeId%", booking.id)
				  			el.dataset.href = el.dataset.href.replace("%customerId%", customerId)
				  		})
				  		document.querySelector('#content').appendChild(clone)
				  	}
				  })
				  .catch(err => {
				  	console.error(err)
				  	document.querySelectorAll('.infobox p')[0].innerText = "Impossible de récupérer les informations de votre compte, revenez plus tard."
				  })
			}
		})
	</script>
</div>