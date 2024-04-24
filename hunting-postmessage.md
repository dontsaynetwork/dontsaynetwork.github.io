Hunting postMessage

Hi, 
This might not be as insightful as you would expect as there are many guide on how to hunt postMessage vulnerabilities and this might be full of grammatical errors but bear with me.

The rule (for me) is to know that there are __sinks__ and __sources__ in the postMessage. The basics are __sources__ are where the you can input some arbitrary characters in the source code. An example of sources in JavaScript is __URLSearchParams__, which looks for parameters in the URL. Since we are dealing with postMessage, we should look for any postMessage which looks like _something-here.postMessage(__someVariable__,"*")_. In this case, the most important is the ___someVariable___ because it tells us that the postMessage is dynamically generated, whether we are able to modify it is unknown at this moment but still we know that it is a possibility. If it was _something-here.postMessage(__{"admin":true__,"*")_, in most cases this would prove unsuable because it is not a __source__, you can't modify it (it might be possible but I don't know of any way to act on the postMessage if it is like that.

Note: From there, I will refer to the __postMessage__ as __pM__, it is too long to write :-) 

From there, you know that you have some __sources__, the next that I would do is to fire Burp (any version), enable DOM Invader, enable _pM_ only, disable all the sub-options and visit the target, letting all the _pM_ fire as they would do normally. If you see the _pMs_, then I would enable _only_ the __postMessage origin spoofing__ ) and wait and see if the _pM_ still fires. The reason for this is I want to know if I can use my origin to send a _pM_ to the target. If you see the message in DOM Invader, there are great chances that they are not checking the origin, however, they might be checking the origin down the line but there are still 50% chances that they are not.
![image](https://github.com/dontsaynetwork/dontsaynetwork.github.io/assets/137604590/fae1af8c-95d5-4c4f-9a22-0193ee13b8db)



From there, it is your investigating power that would be useful, you will need to see where the message that you manipulated is going. For that, you got DOM Invader again that can help you. In the _pM_ list, you will see all the _pM_that was succesfully triggered, you can just _Replay message_ and you will see all the __sinks__ which were triggered by this specific message. You have now narrowed where the message is coming from to where the message is going.  ![image](https://github.com/dontsaynetwork/dontsaynetwork.github.io/assets/137604590/a11083d2-7a2a-4cb2-81ed-679899e8e837)

Now, what is left is your knowledge, stubborness and imagination to see what you can do in these sinks. You can try to see if the reflection of these variables are important. One important advice is to know when you have to quit because if there is nothing in the postMessage, there is nothing. I once spent 2 weeks on postMessage on a target and I knew since day 2 that it was not vulnerable but I really wanted to see something even when there was nothing to see. 

Good luck and happy hunting.

dontsaynetwork/liardom
