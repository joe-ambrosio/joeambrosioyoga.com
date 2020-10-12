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

<div class="booked-class infobox">
	<div>
		<h2 class="booked-class-title"></h2><br/>
		Vous pouvez modifier cette réservation de plusieurs façons :
	</div>
	<div class="booked-class-options">
		<div>
			<h3>Reporter ce cours</h3>
			<p>Reporter gratuitement cette réservation sur le cours de votre choix. Recommandé si vous ne pouvez pas assister à ce cours mais voulez assister à un autre cours déjà planifié.</p>
			<button>Reporter</button>
		</div>
		<div>
			<h3>Créditer mon compte</h3>
			<p>Créditer la valeur de ce cours sur mon compte. La prochaine fois que je réserverais avec mon email je n'aurais pas à payer ce cours. Recommandé si vous ne pouvez pas assister à ce cours mais que vous n'êtes pas encore sûr de quand vous pourrez le rattraper.</p>
			<button>Créditer</button>
		</div>
		<div>
			<h3>Demander un remboursement</h3>
			<p>Me faire remboursser de la valeur de ce cours. Recommandé si vous ne pouvez pas assister à ce cours et que vous ne pensez pas reprendre un cours ici. Le rembourssement sera fera sur la carte bleu qui a servi au paiement sous 5 à 10 jours.</p>
			<button>Me faire rembourser</button>
		</div>
	</div>
</div>


<div>
	<script>
		document.addEventListener('DOMContentLoaded', function() {
		  	fetch('https://ga09zolgt2.execute-api.eu-west-3.amazonaws.com/account?customerId=' + window.location.hash.slice(1))
			  .then(response => {
			  	if (response.ok) {
			  		return response.json()
			  	} else {
			  		throw new Error("No OK response")
			  	}
			  })
			  .then(account => {
			  	console.log(account)
			  	document.getElementById("account-email").innerText = account.email
			  	document.getElementById("account-balance").innerText = account.balance
			  	document.getElementById("nb-future-bookings").innerText = account.bookings.length
			  	document.querySelectorAll(".booked-class-title")[0].innerText = account.bookings[0].long_title
			  })
			  .catch(err => {
			  	console.error(err)
			  	calendarEl.prepend("Impossible de récupérer les cours actuellement, revenez plus tard.")
			  })

		})
	</script>
</div>