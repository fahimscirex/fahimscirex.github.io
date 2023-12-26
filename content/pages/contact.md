---
title: Anonymous Mail Box
subtitle: Send Me an Anonymous Message
layout: page
url: '/message/'
description: Send an Anonymous Message to Fahim Montasir
comments: false
ShowShareButtons: false
---
Write your message down below without revealing yourself. If you want to have a conversation anonymously, there are options for that too. For example, you could email me from a disposable email address, or you could leave a text on my Telegram bot [@yetanotherassbot](https://t.me/yetanotherasbot) from a private account without actually revealing your identity.

But why all of these? Mostly because I want to keep the option open for people who just don’t want to disclose their identity but still would like to say something. And partly because it’s somewhat exciting. Or maybe the other way around :v

{{< rawhtml >}}
<form class="wj-contact" action="https://formspree.io/f/xbjqooad" method="POST">
  <!--  <input type="text" name="email" placeholder="Email Address"> -->
    <textarea type="text" name="content" rows="10" placeholder="Write your message here "></textarea>
    <input type="hidden" name="_next" value="<REDIRECTION LINK> ">
    <input type="hidden" name="_subject" value="New Contact Form Submission">
    <input type="text" name="_gotcha" style="display:none">
    <input type="submit" value="Submit">
</form>

<style>
form.wj-contact input[type="text"], form.wj-contact textarea[type="text"] {
    width: 100%;
    vertical-align: middle;
    margin-top: 0.25em;
    margin-bottom: 0.5em;
    padding: 0.75em;
    font-family: monospace, sans-serif;
    font-weight: lighter;
    border-style: solid;
    border-color: #444;
    outline-color: #2e83e6;
    border-width: 1px;
    border-radius: 3px;
    transition: box-shadow .2s ease;
}

form.wj-contact input[type="submit"] {
    outline: none;
    color: white;
    background-color: #2e83e6;
    border-radius: 3px;
    padding: 0.5em;
    margin: 0.25em 0 0 0;
    border: 1px solid transparent;
    height: auto;
}
</style>
</form>

{{< /rawhtml >}}
