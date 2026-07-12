A simple php class to generate and check captcha values.
<br><br>Notes:
<br>- Single file class, no database needed.
<br>- The check is not case sensitive.
<br>- A token can be used within 10 minutes (customizable static property).

# Usage
1. include the class in your project and assign a new own value to the static class property $key.
2. use ``` $formToken = BasicCaptcha::generateFormToken(); ```  to obtain a new token to be sent alongside the captcha value inserted by the form user.
3. use ``` $captchaValue = BasicCaptcha::generate($formToken); ```, passing the previously obtained form token, to obtain a new captcha value.
4. use ``` $captchaBase64 = BasicCaptcha::getB64($captchaValue); ```, passing the previously obtained captcha value, to obtain a base64 encoded image of the captcha value.
5. in a form, add an ``` <img src="data:image/png;base64,<?php echo $captchaBase64;?>"> ``` node, needed to show the user the captcha image.
6. in the same form, add a ``` <input type="text" maxlength="8" name="captcha" placeholder="Input the captcha code" required /> ``` input, needed to let the user input the captcha code.
7. in the form processing page, use ``` $isCaptchaValid = BasicCaptcha::verify($captcha,$formToken); ``` making sure both the arguments are obtained from the post submission, to check if the captcha input is valid.

# Simplest use example

Exposed form

```
<?php
$formToken = Captcha::generateFormToken();
$captchaValue = Captcha::generate($formToken);
$captchaBase64 = Captcha::getB64($captchaValue);
?>

<form>
<img src="data:image/png;base64,<?php echo $captchaBase64;?>" />	
<input name="form_token" type="hidden" value="<?php echo $formToken; ?>" readonly>
<input name="captcha" type="text" minlength="8" maxlength="8" placeholder="Input captcha" required>
<button type="submit">Submit</button>
</form>
```

Backend code verification

```
<?php
$captcha = $_GET['captcha'] ?? null;
$formToken = $_GET['form_token'] ?? null;
if ( !Captcha::verify($captcha, $formToken) ){ throw new \Exception('invalid captcha'); }
```

# Audio player
You can obtain an array of base64 audios by using ```BasicCaptcha::getB64($captchaValue,'audio','en');```
<br>on your page, add all the obtained audios using <b>hidden</b> audio elements: ``` <audio style="display:none;" class="captcha_audio" src="data:audio/mpeg;base64,<?php echo $a; ?>"></audio> ```
<br>and a button to play them all: ``` <button type="button" id="captcha_audio_button"> Play captcha audio </button> ```
<br>then include the BasicCaptcha.js class, and use it as follows : 
<br>
``` 
let captcha = new BasicCaptcha({'wrapperQuery':'#captcha-wrapper' ,'logEnabled':true ,'audioPauseDurationMs':800});
captcha.setAudioNodes('.captcha_audio');
captcha.setAudioPlayer('#captcha_audio_button');
```

# Screenshots
<img width="454" height="69" alt="Screenshot 2026-07-12 alle 19 11 15" src="https://github.com/user-attachments/assets/7195748e-45e4-4966-86a2-a8c27523b92e" />


