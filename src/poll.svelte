<script>
  import { get } from "svelte/store";

  let question;
  let pollId;
  let creator;
  let options;

  function createPoll() {
    if (creator != null) {
      let newPoll = { a: 1 };
      let optionList = [];

      fetch("http://localhost:8080/users", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(newPoll),
      })
        .then((response) => response.json())
        .then((json) => {
          console.log("success");
        })
        .catch((error) => {
          console.error("Error fetching data:", error);
        });
    } else {
      alert("Cannot create an empty user");
    }
  }

  // Function to handle voting
  function vote(index) {
    let newVote = { username: "tester1", option: index };
    let vc;
    fetch("http://localhost:8080/polls/1", {
      method: "PUT",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(newVote),
    })
      .then((response) => response.json())
      .then((json) => {
        vc = json.voteCount; // Assign the fetched data to the 'data' variable
        console.log(vc);
      })
      .catch((error) => {
        console.error("Error fetching data:", error);
      });
  }

  let pollById;
  let o = new Array();
  function getPoll() {
    fetch("http://localhost:8080/polls/1")
      .then((response) => response.json())
      .then((json) => {
        pollById = json.question; // Assign the fetched data to the 'data' variable
        o = json.options;
        console.log(o);
      })
      .catch((error) => {
        console.error("Error fetching data:", error);
      });
  }
  getPoll();
</script>

<div>
  <p>Create Poll</p>
  <input type="text" bind:value={pollId} placeholder="Poll ID" /><br /><br />
  <input type="text" bind:value={question} placeholder="Question" /><br /><br />
  <input type="text" bind:value={creator} placeholder="Creator" /><br /><br />
  <input type="" bind:value={options} placeholder="Options" /><br /><br />
  <button on:click={createPoll()}>Submit</button>
</div>

<h1>{pollById}</h1>

{#each o as option, index}
  <div class="poll-option">
    <span>{option.caption} - {option.voteCount} votes</span>
    <button on:click={() => vote(index)}>Vote</button>
  </div>
{/each}

<!-- {#each options as option, index}
  <div class="poll-option">
    <span>{option.text} - {option.votes} votes</span>
    <button on:click={() => vote(index)}>Vote</button>
  </div>
{/each} -->

<style>
  .poll-option {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
    border: 2px solid black; /* Add this line to set a black border */
    border-radius: 1rem;
  }
</style>
