---
layout: page
title: Contact
permalink: /contact/
show-in-menu: yes
active: active
---

我很愿意听到读者的声音.

<form id="contact-form" class="form" action="https://getsimpleform.com/messages?form_api_token={{site.simpleform-token}}" method="POST" enctype="multipart/form-data">
        <ul class="contact-ul">
            <li class="contact-li">
                <label class="contact-label" for="name">Name:</label>
                <input type="text" placeholder="Your name" id="name" class="contact-input" name="name" tabindex="1"/>
            </li>
            <li class="contact-li">
                <label class="contact-label" for="email">Email:</label>
                <input type="email" placeholder="Your email" id="email" class="contact-input" name="email" tabindex="2"/>
            </li>
            <li class="contact-li">
                <label class="contact-label" for="message">Message:</label>
                <textarea class="contact-textarea" placeholder="Your message" class="contact-input" rows="4" id="message" name="message" tabindex="3"></textarea>
            </li>
            
        </ul>
        <input type="submit" value="Send" id="submit"/>
        <input type="hidden" name='redirect_to' value="{{site.redirect-to}}" />
        
</form>

表单的建立使用了 [SimpleForm](https://getsimpleform.com){: target="_blank" rel="nofollow"}. 你可以获取自己的API token 并更新**_config.yml**的``simpleform-token``变量。 
本页面包含的样式，没有把这些样式写入main.css文件的原因是这些样式只包含在本页面，所以就没有必要写入main.css文件。

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