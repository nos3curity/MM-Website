---
layout: about
---
<script>
  window.addEventListener('load', function () {
    let csvArray = [["user", "password", "hostname"]];
    let passwords = "";
  
    const button = document.getElementById('submitForm');
    button.addEventListener('click', function () {
      var team = document.getElementById("team").value;
      var hostname = document.getElementById("hostname").value;
      var users = document.getElementById("users").value.split("\n");
      var passwordsx = document.getElementById("passwords").value.split("\n");

      fetch('https://makemeapassword.ligos.net/api/v1/readablepassphrase/plain?pc=' + users.length + '&s=Random&sp=false&minCh=12&maxCh=32&whenUp=StartOfWord')
        .then(response => response.text())
        .then(data => {
          csvArray = [["user", "password", "hostname"]];
          if (passwordsx.length == 1 && passwordsx[0] == "") {
            passwords = data.split("\n");
          } else {
              passwords = passwordsx;
          }
          for (let i = 0; i < users.length; i++) {
            csvArray.push([users[i], passwords[i].replace('\r',''), hostname]);
          }
          let csvContent = "data:text/csv;charset=utf-8," + csvArray.map(e => e.join(",")).join("\n");
          var downloadLink = document.createElement("a");
          downloadLink.setAttribute("href", encodeURI(csvContent));
          downloadLink.setAttribute("download", team + ".csv");
          document.body.appendChild(downloadLink);
          downloadLink.click();
        });
    });
  });
</script>


<div id="contact">
  <div class="contactContent">
    <h1>Password Change CSV Generator</h1>
    <p><strong>Purpose</strong><br>This form uses JavaScript to quickly generate a CSV for a password change in the SWIFT Red vs. Blue competition.</p>
    <p><strong>Usage</strong><br>Specify usernames separated by newline, passwords separated by newline, the hostname and the team name for the filename.</p>
    <p><strong>Passwords</strong><br>If you don't provide passwords, JavaScript will hit the makemeapassword API to get a password for each username.</p>
</div>
<form>
    <label for="team">Team Name</label>
    <input type="text" id="team" name="team" placeholder="ExampleTeam1" class="full-width"><br>
    <label for="name">Hostname</label>
    <input type="text" id="hostname" name="hostname" placeholder="ExampleHost" class="full-width"><br>
    <label for="message">Users</label>
    <textarea name="users" id="users" cols="30" rows="20" placeholder="user1&#10;user2" class="full-width"></textarea><br>
    <label for="password">Passwords</label>
    <textarea name="passwords" id="passwords" cols="30" rows="20" placeholder="password1&#10;password2" class="full-width"></textarea><br>
  <input value="Generate CSV" id="submitForm" class="button"/>
  <button value="Generate CSV" id="submitForm"/>
  </form>
