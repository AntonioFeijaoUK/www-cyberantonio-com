---
title: "Contact"
# search:
#   exclude: true

hide:
  - navigation
  - toc
  #- footer
---

<div class="hero-text">
    <h1 style="font-size:2.0rem;">hello AT cyberantonio.com</h1>
</div>

<div class="hero-image"></div>



<!--
<style>
  body {
  /* Center the form on the page */
  text-align: center;
}

form {
  display: inline-block;
  /* Form outline */
  padding: 1em;
  border: 1px solid #ccc;
  border-radius: 1em;
}

p + p {
  margin-top: 1em;
}

label {
  /* Uniform size & alignment */
  display: inline-block;
  min-width: 90px;
  text-align: right;
}

input,
textarea {
  /* To make sure that all text fields have the same font settings
     By default, text areas have a monospace font */
  font: 1em sans-serif;
  /* Uniform text field size */
  width: 300px;
  box-sizing: border-box;
  /* Match form field borders */
  border: 1px solid #999;
}

input:focus,
textarea:focus {
  /* Set the outline width and style */
  outline-style: solid;
  /* To give a little highlight on active elements */
  outline-color: #000;
}

textarea {
  /* Align multiline text fields with their labels */
  vertical-align: top;
  /* Provide space to type some text */
  height: 5em;
}

.button {
  /* Align buttons with the text fields */
  padding-left: 90px; /* same size as the label elements */
}

button {
  /* This extra margin represent roughly the same space as the space
     between the labels and their text fields */
  margin-left: 0.5em;
}
</style>

-->

## WIP - not in use, just testing for now :)

<form action="http://127.0.0.1:3331" method="post">
  <p>
    <label for="name">Name:</label>
    <input type="text" id="name" name="user_name" />
  </p>
  <p>
    <label for="mail">Email:</label>
    <input type="email" id="mail" name="user_email" />
  </p>
  <p>
    <label for="msg">Message:</label>
    <textarea id="msg" name="user_message"></textarea>
  </p>
  <p>
    <button type="submit">Send</button>
  </p>
  <!-- The following line controls and configures the Turnstile widget. -->
<div style="display: block; flex-flow: row;">
  <div
    class="cf-turnstile"
    data-sitekey="0x4AAAAAABh-b-ks_shO7wZt"
    data-size="compact"
  ></div>
</div>

</form>




<script
  src="https://challenges.cloudflare.com/turnstile/v0/api.js?onload=onloadTurnstileCallback"
  defer
></script>
<!-- end. -->






