# Laravel Notifications - create your custom notification channel

> This article is published also on [DEV Community](https://dev.to/c0d3b0t/laravel-notifications-create-your-custom-notification-channel-1hg3)

Hello community!  

In this post I want to share how I created a custom Notification channel to handle SMS notifications via Twilio Programmable SMS product.

This article is not a tutorial about integrating Twilio with Laravel, so I assume you've already installed [Twilio PHP Helper Library](https://www.twilio.com/docs/libraries/php) and setup all necessary configurations (SID, Token, From Number etc.).  

### The SMS definition

First of all, I want to describe the definition of SMS in my code.  

```php
namespace App\Notifications\Messages;

class SmsMessage
{
    /**
     * @var string
     */
    private string $_recipient;
    
    /**
     * @var string
     */
    private string $_content;

    /**
     * @return string
     */
    public function getRecipient(): string
    {
        return $this->_recipient;
    }

    /**
     * @param string $recipient
     * @return $this
     */
    public function setRecipient(string $recipient): SmsMessage
    {
        $this->_recipient = $recipient;
        return $this;
    }

    /**
     * @return string
     */
    public function getContent(): string
    {
        return $this->_content;
    }

    /**
     * @param string $content
     * @return $this
     */
    public function setContent(string $content): SmsMessage
    {
        $this->_content = $content;
        return $this;
    }
}
```
This is a simple value object, that has a general purpose - type safety.

### The Channel

Now I'm creating a notification channel, that has a single responsibility - send the SMS message.

```php
namespace App\Channels;

use App\Services;
use App\Notifications\Messages\SmsMessage;
use Illuminate\Notifications\Notification;

class TwilioSms
{
    /**
     * @param $notifiable
     * @param Notification $notification
     * @throws \Twilio\Exceptions\TwilioException
     */
    public function send($notifiable, Notification $notification)
    {
        $message = $notification->toTwilioSms($notifiable);

        (new Services\TwilioSms())->sendSms(
            $message->getRecipient(),
            $message->getContent()
        );
    }
}
```
You've probably noticed the `Services\TwilioSms` class. It's just a wrapper that I've created to interact with Twilio SDK. 
You can find it here - https://github.com/c0d3b0t/twilio-sms-client

### The Notification

Finally I've defined the `via` and `toTwilioSms` methods in my Notification class.  

```php
/**
 * Get the notification's delivery channels.
 *
 * @param  mixed  $notifiable
 * @return array
 */
public function via($notifiable)
{
    return [\App\Channels\TwilioSms::class];
}

/**
 * @param $notifiable
 * @return SmsMessage
 */
public function toTwilioSms($notifiable): SmsMessage
{
    return (new SmsMessage())
        ->setContent("Whatever message you need.")
        ->setRecipient($notifiable->phone);
}
```

That's all! 

### Summary

This code is running on PHP 7.4 version (due to typed properties).  

Thanks for reading! Happy coding :) 



