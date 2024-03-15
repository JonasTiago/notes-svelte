<script lang="ts">
    import type { PageData } from "./$types";
    import { notes } from "../../stores/notes";
    import { goto } from "$app/navigation";
    import {scale } from "svelte/transition";
    export let data: PageData;
    let editMode = false;
    let currentNoteContent = data.note.content; 

    function deleteNote(event: Event){
        const button = event.target as HTMLButtonElement;
        notes.update((currentNotes)=>currentNotes.filter(note => note.id !== button.id));
        goto('/');
        
    }

    function editNote(){
        notes.update((currentNotes) => currentNotes.map(note => {
            if(note.id === data.note.id){
                note.content = currentNoteContent;
            }
            return note;
        }) )
    }

    function toggleEditMode(){
        if(editMode) editNote()
        editMode = !editMode;
    }

    
</script>

<div class="container" in:scale={{ delay: 300 }} out:scale={{ duration: 250}}> 
    <a href="/">Voltar</a>
    <button on:click={toggleEditMode} >
        {#if !editMode}
            Editar
        {:else}
            Salvar
        {/if}
    </button>
    <button on:click={deleteNote} id={data.note.id}>Apagar</button>
    {#if editMode}
        <textarea bind:value={currentNoteContent}></textarea>
    {:else}
        <p>{data.note.content}</p>
    {/if}
</div>


<style>
    .container{
        background-color: antiquewhite;
        min-height: 100vw;
    }
</style>