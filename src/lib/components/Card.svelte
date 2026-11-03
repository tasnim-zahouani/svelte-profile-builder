<script lang="ts">
	// Déclaration des propriétés (props) reçues du parent
	let { nom, role, couleur, chercheStage, description} = $props();
</script>

<div class="card" style="border-top: 5px solid {couleur};">
	<div class="avatar" style="background-color: {couleur};">
		<!-- Affiche la 1ère lettre du nom, ou '?' si vide -->
		{nom ? nom.charAt(0).toUpperCase() : '?'}
	</div>
	
	<h2>{nom || 'Votre Nom'}</h2>
	<p class="role">{role || 'Votre Rôle'}</p>

	<p class="description">{description || 'Votre description'}</p>
	
	{#if chercheStage}
		<span class="badge">🚀 En recherche d'opportunité</span>
	{/if}
</div>

<style>
	.card {
		padding: 30px; border-radius: 12px; box-shadow: 0 10px 25px rgba(0,0,0,0.1);
		text-align: center; width: 320px; background: white; font-family: sans-serif;
	}
	.avatar {
		width: 80px; height: 80px; border-radius: 50%; margin: 0 auto 15px;
		display: flex; align-items: center; justify-content: center;
		color: white; font-size: 2.5rem; font-weight: bold;
	}
	h2 { margin: 0 0 5px 0; font-size: 1.5rem; }
	.role { color: #666; margin: 0 0 20px 0; font-size: 1.1rem; }
	.badge { background: #e0f2fe; color: #0284c7; padding: 8px 16px; border-radius: 20px; font-size: 0.85rem; font-weight: bold; }
	.description { padding-left: 5%; text-align: left; text-wrap-style: balance; font-size: 1.1rem;}
</style>