<script lang="ts" >
import { FormGroup, Input, Label, Col, Row, Button, Container } from 'sveltestrap';
import firebase from 'firebase/app';
import 'firebase/firestore';
import {firebaseConfig} from "../lib/firebaseConfig.svelte";
import {userDisplayName, userPhotoURL, username} from './Auth.svelte'


if (!firebase.apps.length) {
   firebase.initializeApp(firebaseConfig);
}
else {
   firebase.app();
}
let name = '';
let description = '';
let content = '';

let docSuccess = false


export const db = firebase.firestore();

    function post(){
    let userDisplayName2 = {$userDisplayName}
    let userPhotoURL2 = {$userPhotoURL}
    let userId2 = {$username}
    db.collection("posts").add({
    name: {name},
    description: {description},
    content: {content},
    userCreator: userDisplayName2,
    userCreatorImage: userPhotoURL2,
    userId: userId2,
    })
    .then((docRef) => {
        console.log("Document written with ID: ", docRef.id);
        docSuccess = true
    })
    .catch((error) => {
        console.error("Error adding document: ", error);
    });
    }



</script>

<main>
{#if !docSuccess}
<Row>
    <Col sm="12" md={{ size: 6, offset: 3 }}>

        <FormGroup>
            <Label>Namn</Label>
            <Input type="text" bind:value={name} />
        </FormGroup>
        <FormGroup>
            <Label>Beskrivning</Label>
            <Input type="text" bind:value={description} />
        </FormGroup>
        <FormGroup>
            <Label>Innehål</Label>
            <Input bind:value={content} style="height: 150px;" rows={1} type="textarea"/>
        </FormGroup>
        <Button on:click={post}>Lägg</Button>
</Col>
</Row>
{/if}
{#if docSuccess}
<Container>
    <div class="bg-success">
        skapat!
    </div>
</Container>


{/if}
    

</main>
<style>
main {
  overflow-x: hidden; /* Hide horizontal scrollbar */
}
.bg-success {
border-radius: .25rem;
margin-top: 10px;
padding: 15px;
background-color: rgb(78, 134, 91);
color: whitesmoke;
text-align: center;
}
</style>