# PHP

[<TIL](Programming.md)
- [Prevent adding a new property to a instance of a class](#Prevent adding a new property to a instance of a class)
- [Checking if extension is enabled from the command line](#Checking if extension is enabled from the command line)

## Prevent adding a new property to a instance of a class

```PHP
  public function __set($name, $value) {
        throw new Exception("Cannot add new property \$$name to instance of " . __CLASS__);
    }
```
When you set a value to an instance variable in PHP, it will first look for that variable.
If it exists then the value is set directly and `__set()` is never called. If the instance variable
does not exist (or if it cannot be accessed due to a private declaration) then it will call `__set()`.
If `__set()` doesn't exist, then it either creates the variable or throws an exception (in the case of a private variable).

## Checking if extension is enabled from the command line
 You can see the names of various extensions by using phpinfo() or if you're using the CGI or CLI version of PHP you can use the -m switch to list all available extensions:
`$ php -m`

## Force using more unlimited memory on a process
`$ php -d memory_limit=-1 /usr/local/bin/composer update`

[source](https://stackoverflow.com/questions/36107400/composer-update-memory-limit)

## Send Mail As (Google SMTP)
So now that you have all your email accounts forwarding mail to your personal Gmail account you need a way to send from those accounts.

1. The very first thing you will need to do is ensure that you have 2-step verification enabled on your primary Gmail account.
Important: If you don’t do this you will get an invalid password error further below when trying to authenticate your email address.
So first go and [enable 2-step verification](https://www.google.com/landing/2step/).

2. Next, you will need to generate an [App password](https://security.google.com/settings/security/apppasswords).
You then use the app password in place of your personal Gmail password further below. This is the only way this process will work.
If you haven’t enabled 2-step verification you will get an error (see below) saying “The setting you are looking for is not available for your account.”

When creating the app password simply choose “Other” and give it a name (example: my 2nd business email).
This will generate a password you will need to save for later.

3. Now back in Gmail, go to settings, and can click on “Accounts and Import.” Then click on “Add another email address you own.”

4. Enter your additional business name and business email that is on the custom domain.

Then for the SMTP server, it will by default show your custom domain. However, you will want to change this to use Google’s free SMTP server. So change it to smtp.gmail.com. Then enter in your personal Gmail address for the username (yourname@gmail.com) and your password. This is the app password you generated earlier, not your Gmail password.

5. It will then send an email confirmation code to the email you just added. You will need to click the link in the email to confirm it or manually enter the code (this proves that you are in fact the owner of the additional email account). And that’s it!

You then can repeat the above steps for your other additional email accounts. Create separate app passwords for each additional email you add.
And now all your email will come to one place and when you go to send mail you can choose from the drop down which one to send from.
In the Gmail settings, you can also set the “Reply from the same address the message was sent to” to make it easier when replying to emails.

[source](https://kinsta.com/knowledgebase/free-smtp-server/)
