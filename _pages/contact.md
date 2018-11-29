---
layout: page
title: Contacto
permalink: /contact/
show-in-menu: yes
active: active
---


Si deseas contactar conmigo personalmente, por favor, usa este formulario facilitando los datos que se piden a continuación.

<form id="contact-form" class="form" action="https://getsimpleform.com/messages?form_api_token={{site.simpleform-token}}" method="POST" enctype="multipart/form-data">
        <ul class="contact-ul">
            <li class="contact-li">
                <label class="contact-label" for="name">Nombre:</label>
                <input required type="text" placeholder="Tu nombre" id="name" class="contact-input" name="name" tabindex="1"/>
            </li>
            <li class="contact-li">
                <label class="contact-label" for="email">Email:</label>
                <input required type="email" placeholder="Tu correo electrónico" id="email" class="contact-input" name="email" tabindex="2"/>
            </li>
            <li class="contact-li">
                <label class="contact-label" for="message">Mensaje:</label>
                <textarea required class="contact-textarea" placeholder="Escribe aquí tu mensaje" class="contact-input" rows="4" id="message" name="message" tabindex="3"></textarea>
            </li>     
        </ul>
        <input type="submit" value="Enviar" id="submit"/>
        <input type="hidden" name='redirect_to' value="{{site.redirect-to}}" />
        
</form>

También puedes contactarme mediante Linkedin o Twitter.

<style>
form {
    width: 100%;
}
.contact-li {
    list-style: none;
}

.contact-input {
    border:none;
    border-bottom: 1px solid #eee;
    transition-duration: 0.3s;
    width: 100%;
    background-color: transparent;
}

.contact-input:focus {
    outline:none;
    border-bottom: 1px solid #514A9D;

}

.contact-label {
    display: block;
}

ul.contact-ul {
    margin: 0;
    padding: 10px;
    width: 100%;
}

#submit {
    border:none;
    background-color: #514A9D;
    padding: 5px 15px;
    color: #eee;
    opacity: 0.8;
}

#submit:hover {
    opacity: 1;
    cursor: pointer;
}


#contact-form {
    border: 1px solid #aaa;
    display: inline-flex;
    margin-bottom: 1em;
}

</style>