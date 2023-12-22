<script lang="ts">
    import { localStorageStore } from "@skeletonlabs/skeleton"  //Backs up our data
    import type { writable } from "svelte/store"  // handles our data
    
    //writwble
    // Read / Write from throughout our application
    // Writable needs a type
    /*
    Writable
    [
        {name: "Paresh", phoneNumber:"(91+) 8114788624"}
    ]
    */ 
    
    let inputName = "";
    let inputPhoneNumber = "";
    
    interface Contact{
        name: string
        phoneNumber: string
    } 
    
    // "Contacts" = Contact[]
    const contactStore: writable<Contact[]> = localStorageStore("contactStore",[]);
    //ContactStorage is a writable which holds an array of contacts
    // $contactstorage
    // contactStore is the writable object (aubscriner, delete, change value)
    // $contactStorage is how you get the VALUE of your store
    
    function addContact() {
        // $contactStore = [paresh , sourav]  ...[paresh,sourav] = paresh,sourav
        //[Ajit, ...[paresh , sourav]] = [ajit,paresh,sourav]
        $contactStore = [
            {
                name: inputName,
                phoneNumber: inputPhoneNumber 
            },
            ...$contactStore
        ]
    }
    
    function deleteContact(index:number) {
        $contactStore = $contactStore.filter((contact, contactIndex)=> contactIndex !== index);
    
    }
    </script>
    
    <div class="container h-full mx-auto flex justify-center items-center">
        <div class="space-y-5">
            <h1 class="h1">ContactList</h1>
            <p>All your contacts in one persistance state!</p>
            <label  class="label">
                <span>Name</span>
            <input type="text" class="input" placeholder="Name" bind:value={inputName} />
        </label>
        <label  class="label">
            <span>Phone Number</span>
        <input type="text" class="input" placeholder="Phone Number" bind:value={inputPhoneNumber}>
    </label>
    <button type="button" class="btn variant-filled" on:click={addContact}>Add Contact</button>
    <hr />
    <h2 class="h2"> Your Contacts</h2>
    {#each $contactStore as contact, index}
    <div class="card p-2">
    <h1>{contact.name}</h1>
    <h1>{contact.phoneNumber}</h1>
    <button type="button" class="btn btn-sm variant-filled-error" on:click={()=>deleteContact(index)}>Delet</button>
    </div>
    {/each}
        </div>
    </div>
