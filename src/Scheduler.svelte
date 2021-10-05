<script>
	import { fade, fly } from 'svelte/transition';
	import { createEventDispatcher } from 'svelte';
	
	import Appointment from './Appointment.svelte';
	
	const dispatch = createEventDispatcher();
	
	export let dateID;
	export let dateHeading = "May 8, 2021";
	export let appointments = [];
	
	let apptDetails = {
		eventName: "",
		eventName2: "",
		appName2:"",
		appName3:"",
		eventName3:"",
		hour: "",
		minutes: "",
		amOrPM: "",
		completed: true
	}

$: time = `${apptDetails.hour}:${apptDetails.minutes < 10 ? '0'+apptDetails.minutes : apptDetails.minutes}${apptDetails.amOrPM}`;
	
	const submitAppt = () => {
		dispatch('addAppt', apptDetails);
		// Return to empty values
		apptDetails = {
			eventName: "",
			eventName2:"",
			eventName3:"",
			hour: "",
			minutes: "",
			amOrPM: "",
			completed: false
		}
	}
	
	// $: console.log(`${apptDetails.eventName} at ${time} - ${dateHeading}`)
</script>


<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.4.0/css/font-awesome.min.css">


<section transition:fade={{ duration: 125 }}>
	<form method="post"
				id={dateID}
				on:submit|preventDefault={submitAppt}>
		
		<div id="closer-cont">
			<span on:click={() => dispatch('modalClose')} 
						class="close" 
						title="Close Modal">
				&times;
			</span>
		</div>

		<header>
			<h2>My Workout Schedule for</h2>
			<h2>{dateHeading}</h2>
		
						 
			<div id="time-cont">
				
				<div id="hrs-mins-cont">
					<input type="number" 
								 id="time-input" 
								 name="time" 
								 min="1" 
								 max="12"
								 step="1"
								 placeholder="Hr."
								 bind:value={apptDetails.hour}>
					<span id="time-colon">:</span>
					<input type="number" 
							 id="time-input" 
							 name="time" 
							 min="0" 
							 max="59"
							 step="1"
							 placeholder="Mins."
							 bind:value={apptDetails.minutes}>
				</div>	

				<div id="am-pm-cont">
					<div>
						<input type="radio" 
									 id="amPMChoice1"
								 	 name="contact" 
									 bind:group={apptDetails.amOrPM}
									 value="am">
						<label for="contactChoice1">AM</label>
					</div>
					
					<div>
						<input type="radio" 
									 id="amPMChoice2"
								 	 name="contact" 
									 bind:group={apptDetails.amOrPM}
									 value="pm">
						<label for="contactChoice2">PM</label>
					</div>		
				</div>
				
			</div> <!-- end of .time-cont -->
			
				<input list="work-out" name="work-out"  placeholder="Pick a workout type from list" required  bind:value={apptDetails.eventName}>
  <datalist id="work-out">
    <option value="Cardio">
    <option value="Stretching">
    <option value="Abs">
    <option value="Strength Training">
		<option value="Calisthenics">
  </datalist> 
			
			
			<input list="work-out4" name="work-out4"   placeholder="Pick you actual workout" required  bind:value={apptDetails.eventName2}>
  <datalist id="work-out4">
    <option value="Lat Pulls">
    <option value="Seated Rows">
    <option value="Situps">
    <option value="Pushups">
		<option value="Pullups">
		<option value="Stretches">
		<option value="Planks">
		<option value="Side situps">
		<option value="Lowerback lifts">
		<option value="Lunges">
		<option value="Squats">
		<option value="Tricep pulldowns">
		<option value="Hip thrusts">
		<option value="Dips">
		<option value="Row machine">
		<option value="Jumpropes">
		<option value="Reverse rows">
		<option value="Shrugs">
		<option value="Squat machine">
		<option value="Touch ankles stretch">
		<option value="Quad stretch">
		<option value="Knee down hipflexor stretch">
		<option value="Stair master">
  </datalist>
			
			<input list="work-out5" name="work-out5"  placeholder="Pick the workout intensity" required  bind:value={apptDetails.eventName3}>
  <datalist id="work-out5">
    <option value="Light">
    <option value="Medium">
    <option value="Hard">
  </datalist> 
		
			
			<div>
				<button class="addBtn">Add</button>
			</div>	
		</header>
	</form>

		<table id="appts-cont">
			{#if appointments.length === 0}
				<h1>No workouts scheduled</h1>
			{:else}
				{#each appointments as appt}
			  <tr>
    
		<th> </th>
    <th>Workout Type</th>
					<th> Workout</th>
    <th>Intensity</th>
		<th>Time</th>
  </tr>
					<Appointment {dateID}
											 apptID={appt.id}
											 
										
   
											
											 apptName2={appt.eventname2}
											apptName={appt.eventname}
											 apptName3={appt.eventname3}
											 time={appt.time}
											 completed={appt.completed}
											 />
				{/each}
			{/if}
		</table>
</section>


<style>
	section { 
		box-sizing: border-box;
		width: 100%;
		height: 100vh;
		position: absolute;
		left: 0;
		top: 0;
		background-color: orange;
		color:orange;
	}
	
	h1 {
		text-align: center;
		color: orange;
	}
	
	h2 {
		margin: 5px 0;
		color:orange;
	}
	
	/* Bordered form */
	form {
		top: 0; left: 0; bottom: 0; right: 0;
		z-index: 10;
		overflow: auto;
		margin: auto;
		background-color: white;
		box-shadow: 0 0 10px black;
		font:white;
		
	}
		
	table {
		border-collapse: collapse;
		border-spacing: 0;
		width: 100%;
		border: 1px solid #ddd;
		background-color: white;
		font: orange;
	}

	
	
	/* Style the close button */
	.close {
		position: absolute;
		font-size: 2rem;
		color: black;
		right: 0;
		top: 0;
		padding: 8px 16px 12px 16px;
		cursor: pointer;
	}

	.close:hover {
		background-color: orange;

	}

	/* Style the header */
	header {
		display: flex;
		flex-direction: column;
		align-items: center;
		
		padding: 30px 40px;
		color: black;
		 background-image: url("https://cdn.pixabay.com/photo/2014/11/17/13/17/crossfit-534615_1280.jpg");
  background-size: 100% 100%;
  background-repeat: no-repeat;
	
	
	/*   text-align: center; */
	}

	#time-cont {
		width: 100%;
		display: flex;
		justify-content: center;
		margin-bottom: 10px;
	}
	
	#hrs-mins-cont {
		width: 200px;
		display: flex;
		justify-content: space-between;
	}
	
	#time-colon {
		font-size: 3rem;
	}
	
	input[type=number] {
		margin-right: 5px;
		color: white;

	}
	
	input[type=number]::-webkit-inner-spin-button {
    opacity: 1
	}
	
	#am-pm-cont > div {
		width: 60px;
		display: flex;
		align-items: center;
		justify-content: center;

	}
	
	input[type="radio"] {
		width: 20px;
		color:white;
	}



	
	/* Style the input */
	input {
		margin: 10px 0;
		border: none;
		border-radius: 0;
		width: 300px;
		padding: 10px;
		float: left;
		font-size: 1.1rem;
		background-color:orange;
		color: white;
	}
	
	input:hover {
		margin: 10px 0;
		border: none;
		border-radius: 0;
		width: 300px;
		padding: 10px;
		float: left;
		font-size: 1.1rem;
		background-color:black;
		color: orange;
	}

	/* Style the "Add" button */
	
	.addBtn {
		padding: 10px;
		width: 160%;
		background:	black;
		color: orange;
		float: left;
		text-align: center;
		font-size: 16px;
		cursor: pointer;
		transition: 0.1s;
		border:	1px solid hsl(168, 76%, 40%);
		border-radius: 0;
	}

	.addBtn:hover {
		border: 1px solid orange;
		background-color: orange;
		color: black;
	}	

	.addBtn:active {
		background-color: orange;
		color: hsl(168, 76%, 25%);
	}	

	

	/* Change styles for span and cancel button on extra small screens */
	@media screen and (max-width: 300px) {

	}
	
	table, th  {
  border:1px solid black;
		
}
	
	
	tr {
margin-left: 300px;
	}
</style>