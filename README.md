# TP Svelte — Profil Builder

- **Sommaire**

[https://canva.link/oohtsvwjpg4573n](https://canva.link/oohtsvwjpg4573n)

---

## **Objectifs**

L'objectif de ce TP est de vous initier au framework Svelte (version 5). À travers la création d'un "Profil Builder" interactif, vous découvrirez comment Svelte simplifie drastiquement la création d'Interfaces Homme-Machine (IHM) réactives. À la fin de cette session, vous saurez :

- Initialiser une application SvelteKit.
- Créer et importer des composants (.svelte).
- Gérer l'état local avec la nouvelle syntaxe des Runes ($state).
- Transmettre des données entre composants ($props).
- Utiliser la liaison bidirectionnelle (*Two-Way Binding*) pour des mises à jour en temps réel.

### **Prérequis**

- Node.js installé (version 18 minimum). Vérifiez avec node -v dans votre terminal.
- Un éditeur de code (VS Code recommandé).

#### Structure du projet SvelteKit

Voici les dossiers et fichiers sur lesquels nous allons travailler :

```powershell
src/
├── lib/
│   └── components/
│       └── Card.svelte        # Composant enfant : affichage visuel de la carte
├── routes/
│   └── +page.svelte           # Page principale : formulaire et état réactif
├── app.d.ts                   # Déclarations TypeScript globales
└── app.html                   # Point d'entrée HTML principal
```

---

## **Partie 1 : Initialisation du projet SvelteKit**

SvelteKit est le framework officiel pour construire des applications Svelte (l'équivalent de Next.js pour React).

1. Ouvrez votre terminal, placez-vous dans le dossier de votre choix et exécutez :

`npx sv create tp-svelte-profil`

2. Répondez aux questions de l'assistant d'installation exactement comme ceci :

    - *Which template would you like?* ➔ **SvelteKit minimal**
    - *Add type checking with TypeScript?* ➔ **Yes, using TypeScript syntax**
    - *What would you like to add to your project?* ➔ **Aucun** (appuyez sur Entrée)
    - *Which package manager do you want to install dependencies with?* ➔ **npm**
3. Une fois l'installation terminée, placez-vous dans le dossier, installez les dépendances et lancez le serveur :

`cd tp-svelte-profil`

`npm install`

`npm run dev -- --open`

<aside>
<img src="https://app.notion.com/icons/exclamation-mark_orange.svg" alt="https://app.notion.com/icons/exclamation-mark_orange.svg" width="40px" />

Votre navigateur s'ouvre sur `http://localhost:5173`. Gardez ce serveur actif !

</aside>

## **Partie 2 : Le Composant Visuel (L'Enfant)**

Dans Svelte, un composant réunit la logique (<script>), la structure (HTML) et le style (<style>) dans un seul fichier `.svelte`. Nous allons créer la "Carte" d'affichage.

1. Dans votre éditeur, créez un dossier components à l'intérieur de `src/lib/.`
2. Créez-y le fichier `src/lib/components/Card.svelte.`
3. Svelte 5 introduit les **Runes** (des fonctions commençant par $). Nous utilisons $props() pour déclarer les propriétés que ce composant s'attend à recevoir.

- Copiez-collez le code suivant dans `Card.svelte` :
    
    ```flow
    <script lang="ts">
    	// Déclaration des propriétés (props) reçues du parent
    	let { nom, role, couleur, chercheStage } = $props();
    </script>
    
    <div class="card" style="border-top: 5px solid {couleur};">
    	<div class="avatar" style="background-color: {couleur};">
    		<!-- Affiche la 1ère lettre du nom, ou '?' si vide -->
    		{nom ? nom.charAt(0).toUpperCase() : '?'}
    	</div>
    	
    	<h2>{nom || 'Votre Nom'}</h2>
    	<p class="role">{role || 'Votre Rôle'}</p>
    	
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
    </style>
    ```
    

## **Partie 3 : L'Application Principale (Le Parent)**

Le point d'entrée de notre site est le fichier `src/routes/+page.svelte`. C'est ici que nous allons stocker nos variables interactives grâce à la Rune $state().

- Ouvrez src/routes/+page.svelte et **remplacez tout son contenu** par :
    
    ```flow
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
    ```
    

## **Partie 4 : Observation et "Magie" Svelte**

Regardez votre navigateur. L'interface s'affiche correctement et le composant enfant a bien reçu les données (Ada Lovelace, couleur orange, etc.).

**L'expérience :** Essayez de modifier le nom dans le champ texte. 

**Que se passe-t-il ?** 
Rien ! En HTML/JS classique, le fait de modifier l'input ne met pas à jour la variable `userNom`. Il faudrait écrire un écouteur d'événement (`onChange`) pour capter la saisie et forcer la mise à jour.

**La solution Svelte : le Two-Way Binding** Svelte élimine cette complexité. Dans +page.svelte, modifiez la ligne de l'input Nom en ajoutant simplement **bind:** devant value :

<!-- AVANT -->

`<input type="text" value={userNom} placeholder="Ex: Ada Lovelace" />`

<!-- APRÈS -->

`<input type="text" bind:value={userNom} placeholder="Ex: Ada Lovelace" />`

**Testez à nouveau :** Tapez sur votre clavier. L'état se met à jour en temps réel et la carte réagit instantanément à chaque frappe !

<aside>
<img src="https://app.notion.com/icons/hammer_orange.svg" alt="https://app.notion.com/icons/hammer_orange.svg" width="40px" />

 **À vous de jouer :** Appliquez le `bind:value` sur le champ "Rôle" et le champ "Couleur" pour rendre toute l'application réactive.

</aside>

## **Partie 5 (Bonus) : Checkbox et Événements**

Pour terminer, manipulons les événements et les types d'inputs différents.

1. **La case à cocher** : Sous l'input de couleur dans `+page.svelte`, ajoutez une case pour activer la recherche de stage. Attention, pour une **checkbox**, on utilise `bind:checked` :

```flow
<label style="flex-direction: row; align-items: center; cursor: pointer;">
	<input type="checkbox" bind:checked={isLooking} />
	Je cherche un stage / emploi
</label>
```

*Observez la carte : le badge de recherche (qui utilise le bloc {#if}) apparaît et disparaît instantanément.*

1. **Les événements (Le bouton Reset)** : Ajoutez un bouton pour réinitialiser le formulaire. Svelte utilise onclick (sans majuscule en Svelte 5). Ajoutez ceci à la fin de votre <div class="editor"> :

```flow
<button onclick={() => {
	userNom = '';
	userRole = '';
	userCouleur = '#000000';
	isLooking = false;
}} style="margin-top: 15px; padding: 10px; cursor: pointer;">
	Tout effacer
</button>
```

<aside>
<img src="https://app.notion.com/icons/compose_orange.svg" alt="https://app.notion.com/icons/compose_orange.svg" width="40px" />

En Svelte 5, réassigner une variable `$state` déclenche automatiquement la mise à jour de l'interface graphique. Pas de `setState` complexe !

</aside>

*Bravo! Le TP est terminé :)*
