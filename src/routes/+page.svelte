<script lang="ts">
	import Card from '$lib/components/Card.svelte';

	// Svelte 5 : Création de l'état réactif de l'application
	let userNom = $state('Ada Lovelace');
	let userRole = $state('Développeuse');
	let userCouleur = $state('#ff3e00'); // Orange Svelte
	let isLooking = $state(false);
</script>

<main class="container">
	<div class="editor">
		<h1>Générateur de Profil</h1>
		
		<label>Nom complet
			<input type="text" value={userNom} placeholder="Ex: Ada Lovelace" />
		</label>

		<label>Rôle
			<input type="text" value={userRole} placeholder="Ex: Développeur Web" />
		</label>

		<label>Couleur du thème
			<input type="color" value={userCouleur} />
		</label>
	</div>

	<div class="preview">
		<!-- On transmet nos variables d'état au composant enfant -->
		<Card 
			nom={userNom} 
			role={userRole} 
			couleur={userCouleur} 
			chercheStage={isLooking} 
		/>
	</div>
</main>

<style>
	.container { display: flex; gap: 40px; padding: 40px; max-width: 900px; margin: auto; font-family: sans-serif; }
	.editor { flex: 1; display: flex; flex-direction: column; gap: 15px; background: #f8f9fa; padding: 25px; border-radius: 12px; }
	.preview { flex: 1; display: flex; justify-content: center; align-items: center; }
	label { display: flex; flex-direction: column; gap: 5px; font-weight: bold; font-size: 0.9rem; }
	input[type="text"] { padding: 10px; border: 1px solid #ccc; border-radius: 6px; font-size: 1rem; }
	input[type="color"] { width: 100%; height: 40px; border: none; cursor: pointer; }
</style>