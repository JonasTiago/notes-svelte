<script lang="ts">
    import { notes, type Note } from "../stores/notes";
    import { v4 as generateId } from "uuid";
    import { fly } from "svelte/transition";

    let contentNote: string;
    let storesNotes: Note[] = [];

    function createNote(){
        notes.update((currentNotes)=>[...currentNotes, {
            id: generateId(),
            content: contentNote,
        }]);
    }

    notes.subscribe((currentNotes)=> {
        storesNotes = currentNotes;
    })
</script>

<div class="container" 
in:fly={{y: -50, duration: 250, delay: 300 }}
out:fly={{y: -50, duration: 250}}
>
    <form on:submit|preventDefault={createNote}>
        <textarea bind:value={contentNote}></textarea>
        <button type="submit">Criar</button>
    </form>
    
    { #each storesNotes as note(note.id)}
        <a href={`/${note.id}`}>
            <div class="note" id={note.id} 
                in:fly={{ x: -50, delay: 250 }} 
                out:fly={{ x: -50, delay: 250 }}>
                {note.content}
            </div>
        </a>
    {/each}
</div>

<style>
    a{
        text-decoration: none;
    }
    .note{
        background-color: antiquewhite;
        width: auto;
        padding: 1rem 1.5rem;
        border-radius: 10px;
        color: black;
        margin: 1rem;
    }
</style>