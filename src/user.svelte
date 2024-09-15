<script>
  let username;
  let email;

  // function to create a new user
  /**
   * @param {String} username
   * @param {String} email
   */
  function createUser(username, email) {
    if (username != null && email != null) {
      let newUser = { username: username, email: email };
      fetch("http://localhost:8080/users", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(newUser),
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
</script>

<div>
  <p>Create User</p>
  <input type="text" bind:value={username} placeholder="Username" /><br /><br />
  <input type="text" bind:value={email} placeholder="email" /><br /><br />
  <button on:click={createUser(username, email)}>Submit</button>
</div>
